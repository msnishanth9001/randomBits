---
layout: post
title: ODoH in the wild
author: ms
date: 2023-08-10
# categories: [DNS]
comments: false
tags: [dns, odoh]
pin: False
author:
---

# ODoH - Oblivious DNS Over HTTPS - RFC 9230.

// proxy flag, switch case in go code.

## 1. DNS Goals
1. Security - Can be achieved with a layer of ENCRYPTION.
2. Integrity - Can be achieved with CHECKSUM.
2. Privacy - Can be achieved with a PROXY between Client and the DNS Server.

| Feature                                      | DNS | DOH | ODOH |
|----------------------------------------------|:---:|:---:|:----:|
| DNS Request / Response is Encrypted          | NO  | NO  | YES  |
| DNS Server track the Client's DNS requests   | NO  | NO  | YES  |
| DNS Transport is Encrypted                   | NO  | YES | YES  |

Note: DOH/ DOT/ DOQ all have same characteristic, except the transport layer is different.

//todo add example of 3 items

## 2. Publication
- ARXIV [ODoH: A Practical Privacy Enhancement to DNS](https://arxiv.org/abs/2011.10121)
- Oblivious DNS over HTTPS: ODoH [RFC 9230](https://datatracker.ietf.org/doc/rfc9230/)

## 3. ODOH PARAMS
1. odohConfig hosted URL.
- URL `https://odoh.cloudflare-dns.com/.well-known/odohconfigs`
2. domain to look up. 
- Domain `example.com`
3. which type of Resource Record, for the domain.
- RR Type `type AAAA` - `DNS_TYPE #28`
4. ODoH Server endpoint.
- endpoint `https://odoh.cloudflare-dns.com/dns-query`

## 4. ODoH Exchange 

![Desktop View](/assets/img/odoh/odoh_light.png){: .light}
![Desktop View](/assets/img/odoh/odoh_dark.png){: .dark}

|           Activity            | Function                                                  | 
|----------------------------------|--------------------------------------------------------|
|1. `CLIENT` REQ odohConfigs | REQUEST odohConfigs from DNS Server.                     |
|2. `SERVER` SEND odohConfigs | PACK and SEND HPKE-SUITE and PUB-KEY as odohConfigs.                   |
|3. `CLIENT` SEND ODoH Query   | ENCRYPT and SEND DNS Query to ODOH-PROXY.     |
|4. `PROXY` FWD ODoH Query   | FORWARD Encrypted DNS Query to ODNS Server.    |
|5. `SERVER` SEND ODoH Response   | SEND Encrypted DNS Answer to ODOH-PROXY.    |
|6. `PROXY` FORWARD ODoH Query   |  FORWARD Encrypted DNS Answer to CLIENT.   |

//todo add example of request and response output. 

## 5. odohConfigs

1. odohConfigs is a bundled info about the HPKE Suite and HPKE PUB-KEY, the ODoH Server is capable of communicating with.
2. odohConfigs is a list of odohConfig. The Structure of odoh configs is defined as:

```
   struct {
      uint16 kem_id;
      uint16 kdf_id;
      uint16 aead_id;
      opaque public_key<1..2^16-1>;
   } ObliviousDoHConfigContents;

   struct {
      uint16 version;
      uint16 length;
      select (ObliviousDoHConfig.version) {
         case 0x0001: ObliviousDoHConfigContents contents;
      }
   } ObliviousDoHConfig;
```

where,
1. `kem_id` `kdf_id` `aead_id` specify the Algorithm Identifier for KEM, KDF, and AEAD from HPKE Suite supported by the TARGET.
2. `public_key` is a byte stream of HPKE PUBLIC-Key derived from the Server's HPKE Suite.
3. `version` defines the odoh version used for packaging the odohConfigs. Current version is `0x0001`.
4. `length` defines the lenght of the odohConfigs as whole.

## 6. HTTPS Scheme Message Format

1. All Messages use HTTP2 and above.
2. ODoH Clients MUST NOT include any private state in requests to Proxies, such as HTTP cookies.
3. ODoH PROXY MUST NOT send any Client-identifying information about Client to ODoH Server such as, "Forwarded" HTTP headers [RFC7239]/ HTTP cookies.

Example from [RFC 9230, SECTION 4](https://datatracker.ietf.org/doc/rfc9230/)

A Client requests that a Proxy, `dnsproxy.example`, forward an encrypted message to
`dnstarget.example`.  The URI Template for the Proxy is
`https://dnsproxy.example/dns-query{?targethost,targetpath}`.
The URI for the Target is 
`https://dnstarget.example/dns-query`.


- CLIENT to PROXY

```
   :method = POST
   :scheme = https
   :authority = dnsproxy.example
   :path = /dns-query?targethost=dnstarget.example&targetpath=/dns-query
   accept = application/oblivious-dns-message
   content-type = application/oblivious-dns-message
   content-length = 106
   <Bytes containing an encrypted Oblivious-DoH Message/query>
```

- PROXY to TARGET.

```
   :method = POST
   :scheme = https
   :authority = dnstarget.example
   :path = /dns-query
   accept = application/oblivious-dns-message
   content-type = application/oblivious-dns-message
   content-length = 106
   <Bytes containing an encrypted Oblivious-DoH Message/Query>
```

- SERVER to PROXY

```
   :status = 200
   content-type = application/oblivious-dns-message
   content-length = 154
   <Bytes containing an encrypted Oblivious-DoH Message/Response>
```

- PROXY to CLIENT

```
   :status = 200
   content-type = application/oblivious-dns-message
   Proxy-Status: received-status=200
   content-length = 154
   <Bytes containing an encrypted Oblivious-DoH Message/Response>
```

## 7. Oblivious DoH Message Format

Oblivious DoH Message contains:
   1.  A DNS message, which is either a Query or Response, depending on context.
   2.  Padding of arbitrary length, which MUST contain all zeros.

Both DNS Query and Response messages use the ObliviousDoHMessagePlaintext 
format.They are encoded using the following structure:

```
   struct {
      opaque dns_message<1..2^16-1>;
      opaque padding<0..2^16-1>;
   } ObliviousDoHMessagePlaintext;

   struct {
      uint8  message_type;
      opaque key_id<0..2^16-1>;
      opaque encrypted_message<1..2^16-1>;
   } ObliviousDoHMessage;
```

1. `message_type`:  A one-byte identifier for the type of message.
    1. Query messages use message_type `0x01`.
    2. Response messages use message_type `0x02`.

2. `key_id`:  The identifier of the corresponding ObliviousDoHConfigContents key.

3. `encrypted_message`:  An encrypted message for the Oblivious Target
      (for Query messages) or Client (for Response messages).

## 8. ODoH Client Routines


### 8.1. `encrypt_query_body (..) {..}`
Constructs Oblivious DoH query from a DNS Query.

```
def encrypt_query_body(pkR, key_id, Q_plain)
     enc, context = SetupBaseS(pkR, "odoh query")
     aad = 0x01 || len(key_id) || key_id
     ct = context.Seal(aad, Q_plain)
     Q_encrypted = enc || ct
     return Q_encrypted
```
Func takes public key of the resolver `pkR`, a key identifier `key_id`, and the plain query `Q_plain` as input parameters. 
1. Compute `encapsulation (enc)` and `context` by BaseS-mode of HPKE Suite.
2. Construct additional authenticated-data `AAD`.
3. Compute `CipherText (ct)` of the query with the context and aad.
4. Form encrypted query `Q_encrypted` as concat of enc and ct.



### 8.2. `decrypt_response_body(..) {..}`
Decrypts an Oblivious DoH response.

```
def decrypt_response_body(context, Q_plain, R_encrypted, resp_nonce):
    aead_key, aead_nonce = derive_secrets(context, Q_plain, resp_nonce)
    aad = 0x02 || len(resp_nonce) || resp_nonce
    R_plain, error = Open(key, nonce, aad, R_encrypted)
    return R_plain, error
```

Func takes `context` Encryption Context, `Q_plain` PlainText Query, `R_encrypted` Encrypted Response and `resp_nonce` response nonce as input params.
1. Derive secrets: `aead_key` and `aead_nonce` from the context and the response nonce.
2. Construct `additional authenticated data (AAD)`.
3. Decrypt encrypted response to obtain the `R_plain: plain response`.

## 9. ODoH Server Routines

### 9.1. `setup_query_context(..) {..}`

```
def setup_query_context(skR, key_id, Q_encrypted):
    enc_32 || ct_: = Q_encrypted
    context = SetupBaseR(enc, skR, "odoh query")
    return context
```

Func takes `skR` Resolver's secret key, `key_id` Key identifier and `Q_encrypted` Encrypted ODoH query body as input params..
1. Extract the encryption parameters: `enc_32` and `ct_` from Q_encrypted.
2. Set up the decryption context `context` using BaseR-mode of HPKE Suite.


### 9.2. `decrypt_query_body (..) {..}`

```
def decrypt_query_body(context, key_id, Q_encrypted):
    aad = 0x01 || len(key_id) || key_id()
    enc_32 || ct_: = Q_encrypted
    Q_plain, error = context.Open(aad, ct)
    return Q_plain, error
```

Func takes `context` Decryption context, `key_id` Key identifier and `Q_encrypted` Encrypted ODoH query body as input params..
1. Construct the `additional authenticated data (AAD)`.
2. Extract the encryption parameters: `enc_32` and `ct_` from Q_encrypted.
3. Compute PlainText `Q_plain` the encrypted query using aad and ct_.

### 9.3. `derive_secrets (..) {..}`

```
def derive_secrets(context, Q_plain, resp_nonce):
    secret = context.Export("odoh response", Nk)
    salt = Q_plain || len(resp_nonce) || resp_nonce
    prk = Extract(salt, secret)
    key = Expand(odoh_prk, "odoh key", Nk)
    nonce = Expand(odoh_prk, "odoh nonce", Nn)
    return key, nonce
```

Func takes `context` Decryption context, `Q_plain` Plain query body and `resp_nonce` Response nonce as input params.
1. Export the `secret` from the context.
2. Compute `salt`.
3. Derive `pseudo-random key (PRK)`, by HPKE method of Extract.
3. Derive `key` and `nonce`, by HPKE method of Expand.

### 9.4. `encrypt_response_body (..) {..}`

```
def encrypt_response_body(R_plain, aead_key, aead_nonce, resp_nonce):
    aad = 0x02 || len(resp_nonce) || resp_nonce
    R_encrypted = Seal(aead_key, aead_nonce, aad, R_plain)
    return R_encrypted
```

Func takes `R_plain` Plain response body, `aead_key` Encryption key, `aead_nonce` Encryption nonce and `resp_nonce` Response nonce` as input params.
1. Construct the `additional authenticated data (AAD)`.
2. Encrypt the response body `R_encrypted` .








































