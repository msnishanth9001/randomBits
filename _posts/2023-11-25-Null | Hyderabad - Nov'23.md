---
layout: post
title: Null | Hyderabad - Nov'23
author: ms
date: 2023-11-25
comments: false
tags: [talk, owasp-ml10]
pin: True
author:
---

Had a wonderful experience presenting [OWASP ML TOP 10](https://owasp.org/www-project-machine-learning-security-top-10/) at Null Hyd this Sunday. It was wonderful participation. Loved the intruiguing questions from people working in various security and ML domains. The [slides](https://github.com/msnishanth9001/msnishanth9001.github.io/blob/main/PPTX/Null%20%7C%20Hyd%20Nov.pptx) from the presentation.

A Brief on the talk:

## 1. Intro

### 1.1 OWASP 
- Open Web Application Security Project.
- A nonprofit organization. With the main goal of improving software security by providing tools, guidelines, and best practices to help developers and organizations to improve the security in applications. The organization emphasizes on Collaboration and Knowledge-sharing within the cybersecurity community and also promotes awareness against threats and proactive measures to be safe.

### 1.2 OWASP Machine Learning Top 10
- AI/ ML models and LLMs are growing as an integral part of the common man's life, assisting with various tasks and reducing the effort required. This gives the attackers more attack surface to exploit a vulnerablity in application and affect users in large scale.
- OWASP ML Top 10, is a security report put together by security experts from all over the world to be used as a guideline to address and mitigate the 10 most critical risks. This is an ‘awareness' documentation and recommendation that Organizations and Developer community can incorporate into their development processes and successfully minimize and/or mitigate security risks.

### 1.3 Rise of ML and LLMs
The story is two ways. One, It started with man trying to elevate the quality of life by transferring the efforts required by man. Heavy machinery solved the physical strength required to do a task. The automation made way for repetitive tasks by completing them tirelessly to increase productivity. Combinig machinery to automation by adding intelligence to these many mundane tasks could be complete tasks efficiently and faster without much human intervention. Two, the rise of statistics and the art of understanding and learing the history to predict the future outcome. At one point these two streams combined and MachineLearning became a separate field of study. 

As all inventions are dreamt first, the bots and AI has manifested in dreams, movies and books long ago. Which gave the idea of a better world with AI and bots amidst humans.

### 1.4 Machine Learning.
Machine Learning is a vast field. It has theoretical aspect which is the foundations to Machine Learning, starting with Statistics. And the applied ML which is use of various algorithms with each other to find the better suite and forumate the ML model. In terms of field of study DL is a subset of ML which is a subset of AI. The path for How to ML has developed a lot, with 100's of algorithms for various individual tasks such as attention/ gradient/ flow. The Basic ML is of 3 types. Lets consider a case. Zabuka goes to Ikea to buy furniture. In case of,
1. Supervised Learning - There is a blueprint on how to assemble, and the model tries to use this to build the furniture. Here the Outcome is labelled for the model to learn from. This is Supervised Learning.
2. Unsupervised Learning - There is no blueprint and the model has to figure out by self on how to assemble by understanding the blocks and patterns, this is Unsupervised Learning.
3. Reinforcement Learning - There is no blueprint avaialable. The model has to try, experiment and test to figure out which is the best way to assemble.


### 1.5 ML OPs
Machine Learning operations are streamlined with the development cycle to give a better overview for deploying, managing, and continuously improving machine learning models in production. This has manual and automated pipeline, with the tasks being the same but segregated on the basis on how it is achieved. Here in this pipeline we can see various segments that are prone to attack individually. 
1.  Data - The Data itself is a entity to be protected by a vault. Data can be highly sensitive w.r.t individual (Patient Data, Financial Data). This needs to be redacted and obsfuscated from the Data engineers also and should be only available on demand basis via an API call without presenting the data itself.
2. Model training - This API should have RBAC enabled with high observation to ensure data integrity and repudiation.
3. Model Evaluation - The model available for validation, should be robust to ensure the validation is proper and does not draw inconclusive/ wrong analysis.
4. Model Deployment - The model avaialability becomes a question with model params and values available for access should be well secured.

With so many surface areas available the Goal of OWASP ML Top 10 is to indentify and secure the ML model and the Development pipeline.

## 2. ML01:2023 Input Manipulation Attack

Case of Autonomous Vehicle: The malicious actors can modify the road signs can be modified by adding small perturbations which changes the intent of the sign board. This results in ML model to take wrong and harmful actions for the vehicle. Example as in figure the STOP sign can be perturbed to be read as 120kmph.

## 3. ML02:2023 Data Poisoning Attack

Case of WAF: A WAF deployed for SQLI attack-defense can be countered by various methods which results in easy breakthrough and WAF is rendered useless. This is similar to giving prompts to by-pass the filters.

## 4. ML03:2023 Model Inversion Attack

Case of Bot Detection: The Taylor Swift Tickets were sold out in a jiffy with the bots. The battle against bot detection is an uphill battle as the defense mechanism should not hinder user-experience and also detect bot trying to fool the system.

## 5. ML04:2023 Membership Inference Attack

Case of Banking: A malicious attacker wants to gain access to sensitive financial information of individuals. They do this by training a machine learning model on a dataset of financial records and using it to query whether or not a particular individual’s record was included in the training data. The attacker can then use this information to infer the financial history and sensitive information of individuals.

## 6. ML05:2023 Model Stealing

Case of Cloning ML Model: New sophisticated ways are being researched to clone a model parameter based on interaction and prompt results.

## 7. ML06:2023 AI Supply Chain Attacks

Case of Trojans: The packages and Frameworks are not tested and certified. Common dev will not aware of the consequences of this. This is more of a hindsight missed from the package developer and is not intended always.

## 8. ML07:2023 Transfer Learning Attack

Case of Backdoor: The packages and Frameworks are not tested and certified. Common dev will not aware of the consequences of this. This is a growing research topic to exploit a ML model.

## 9. ML08:2023 Model Skewing

Case of Feedback: An attacker provides fake feedback data to the Financial Bank system, indicating that high-risk applicants have been approved for loans in the past, and this feedback is used to update the model’s training data.

## 10. ML09:2023 Output Integrity Attack

Case of Classifiers: Calssifiers are simple at first glance, ML models are being integrated to medicine which helps doctors catch/ observe items that could be missed. Incorrect classification of Tumorous vs Bening tumor for patients.

## 11. ML10:2023 Model Poisoning

Case of Spams: Mailbox, emails are the messangers of the century. The popular email suites have spam and ad detection integrated onto them. With a classifier trained with poisoned data/ keyword "hurry" or "exclusive deal" the SPAM filter could be bypassed.





