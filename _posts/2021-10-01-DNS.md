---
layout: post
title: DNS - Domain Name System
author: ms
date: 2021-10-01
# categories: [DNS]
comments: false
tags: [dns]
pin: False
author:
---


## 1. DNS - 101

DNS is the yellow pages of the internet. Humans locate a webpage with domain names, such as `google.com` or `facebook.com`. The web browser locates the webpage(domain) via a lookup. This translation of domain to IP is done by the DNS. This is a defined protocol as part of [RFC1035](https://www.ietf.org/rfc/rfc1035.txt).

![Desktop View](https://kinsta.com/wp-content/uploads/2018/05/what-is-dns.png){: width="972" height="589" }
_request ISP to locate domain.com_

## 2. Types of DNS Servers

1. **TLD DNS Server** - These servers handle queries for specific top-level domains - `.com` `.net` `.org`.

2. **Root DNS Server** - They provide information about the authoritative DNS servers for top-level domains (TLDs). These are the starting point for any domain resolution.

3. **Recursive DNS Server** - They recursively search and fetch the information from the authoritative DNS servers. Commisioned for ISP.

4. **Authoritative DNS Server** - These servers are responsible for storing the actual DNS records which maps to domains. These are maintained by domain owners or domain providers.

## 3. Components of DNS Servers

1. **Domain Registrar**  -This entity is responsible for managing the reservation of Internet Domain Names.

2. **Name Servers** - These are the yellow-pages servers which store the DNS records which map the IP to Domain Names.

3. **DNS Records** - A record for capturing DNS Data. There are numerous number of [DNS Record types](https://en.wikipedia.org/wiki/).
  + **A record**: A record data for `IPv4 address`.
  + **AAAA record**: A record data for `IPv6 address`.
  + **CNAME record**: A record data to create `Alias` for a domain. It not not contain IP address.
  + **NS record**: A record data to store the `Name Server` for a DNS.
  + **SVCB record**: A special record which can provide: `IP address` `ECH` `PublicKeys` `Service` `AltEndpoints` - directly. 

4. **Web-based services** A platform to host the web service.

## 4. DNS LookIP

1. Send a query for domain `www.example.com`.
2. Query is addressed by the LDNS configured and is forwarded to the DNS recursive resolver.
3. The resolver requests the DNS root nameserver.
4. The root server responds with the address of the TLD of `.com` DNS server.
5. The resolver makes a query to the `.com` TLD.
6. The TLD server responds with the IP address of the domain's nameserver `example.com`.
7. The recursive resolver sends query to domain's nameserver.
8. The IP address for `example.com` is returned to the resolver from the nameserver.
9. The DNS resolver then responds to the web browser with the IP address of the domain requested.
10. The web browser makes HTTP request for the web contet, towards the IP address retrieved.
11. The hosting server return the content to the browser.


## 5. DNS request.

```shell
❯ dig @1.1.1.1 www.cloudflare.com +dnssec

; <<>> DiG 9.18.20 <<>> @1.1.1.1 www.cloudflare.com +dnssec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 46329
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 4096
;; QUESTION SECTION:
;www.cloudflare.com.    IN  A

;; ANSWER SECTION:
www.cloudflare.com. 206 IN  A 104.16.123.96
www.cloudflare.com. 206 IN  A 104.16.124.96
www.cloudflare.com. 206 IN  RRSIG A 13 3 300 20231208072716 20231206052716 34505 www.cloudflare.com. If3bsv5KD/fqHFQ228bSeGibtM8T8hz5KRKavhZ/XItxIEP8ScrhhVAg 5OQMlRiY3Nm1nJGNhqrxBaiWM8S4gg==

;; Query time: 64 msec
;; SERVER: 1.1.1.1#53(1.1.1.1) (UDP)
;; WHEN: Thu Dec 07 11:59:03 IST 2023
;; MSG SIZE  rcvd: 193
```
{: .nolineno }


## 6. types of DNS queries:
1. **Recursive query** - The Client requests the DNS server to respond with Answer or with an Error message if record is not available.
2. **Iterative query** - The Client requests the DNS server to respond with the best answer, if there is no match for the query the DNS server should return a referral to a DNS server for a lower level domain.
3. **Non-recursive query** - The Client requests authoritative DNS server for the record or if record is available in the cache.




