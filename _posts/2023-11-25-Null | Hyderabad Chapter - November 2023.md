---
layout: post
title: Null | Hyderabad Chapter - November 2023
author: ms
date: 2023-11-25
comments: false
tags: [talk, owasp ml10]
pin: True
author:
---

Had a wonderful experience presenting OWASP ML TOP 10 at Null Hyd this Sunday. It was wonderful participation. Loved the intruiguing questions from people working in various security and ML domains. The slides from the presentation is available [here](https://github.com/msnishanth9001/msnishanth9001.github.io/blob/main/PPTX/Null%20%7C%20Hyd%20Nov.pptx).

A Brief on the talk:

## 1. Intro

### 1.1 OWASP 
- Open Web Application Security Project.
- A nonprofit organization. The main goal is focused on improving software security by providing tools, guidelines, and best practices to help developers and organizations to improve the security in applications. The organization importance to collaboration and knowledge-sharing within the cybersecurity community, Including to promote awareness against threats and proactive measures to be safe.

### 1.2 OWASP Machine Learning Top 10
- AI/ ML models and LLMs are growing as an integral part of the common man's life, assisting with various tasks and reducing the effort required. This gives attackers more attack surface to exploit a vulnerable application and affect the multiple users.
- OWASP ML 10, is a security report put together by security experts from all over the world for the developers to use as a guideline to address and mitigate the 10 most critical risks. This is an ‘awareness' documentation and recommendation that companies and devs  can incorporate their development processes in order to minimize and/or mitigate security risks.

### 1.3 Rise of ML and LLMs
The story is two ways. One, It started with man trying to elevate the quality of life by transfering the efforts required from man to robot. Heavy machinery solved the physical strength required to do a task. The automation made way for repetitive tasks by completing them tirelessly to increase productivity. Combinig machinery to automation by adding intelligence to these many mundane tasks could be completed faster without much human intervention. Two, the rise of statistics and the art of understanding and learing the history to predict the future outcome. At one point these two streams combined and MachineLearning became a separate field of study. 

As all inventions are dreamt first, the bots and AI has manifested in movies and books long ago. Which gives the idea of a better AI and bots amidst humans.

### 1.4 Machine Learning.
Machine Learning is a vast field. It has theoretical aspect which is the foundations to Machine Learning, starts with Statistics. And the applied ML which is use of various algorithms with each other to find the better suite and forumate the ML model. DL is a subset of ML which is a subset of AI interms of field of study. The path on how to ML is so vast now with 100's of algorithms for various individual tasks such as attention/ gradient/ flow. Basically ML is of 3 types. Lets consider a case. Zabuka goes to Ikea to buy furniture. In case of,
1. Supervised Learning - There is a blueprint on how to assemble, this is supervised learning. Here the Outcome is labelled for the model to learn from.
2. Unsupervised Learning - There is no blueprint and have to figure out by self how to assemble by understanding the blocks and patterns, this is unsupervised learning.
3. Reinforcement Learning - There is no blueprint avaialable. The model has to try, experiment and test to figure out which is the best way to assemble.

Broadly these are the 3 machine learning methods.

### 1.5 ML OPs
Machine Learning operations are streamlined with the development cycle to give a better overview for deploying, managing, and continuously improving machine learning models in production. This has manual and automated pipeline, with the tasks being the same but segregated on the basis on how it is achieved. Here in this pipeline we can see various segments that are prone to attack individually. 
1.  Data - The Data itself is a entity to be protected in a vault. Data can be highly sensitive w.r.t individual (Patient Data, Financial Data). This needs to be redacted and obsficated from the Data engineers also and should be only available on demand basis via an API call without presenting the data itself.
2. Model training - This API should have RBAC enabled with high observation to ensure data integrity and repudiation.
3. Model Evaluation - The model available for test, should be robust to ensure the validation is proper and does not draw inconclusive/ wrong analysis.
4. Model Deployment - The model avaialability becomes a question with model params and values available for access should be well secured.

With so many surface areas available the Goal of OWASP ML10 is to indentify and secure the ML model and the pipeline.

## 2. ML01:2023 Input Manipulation Attack

Case of Autonomous Vehicle: The signs can be modified by threat actors by adding perturbations which changes the intent of the sign board. This results in ML model to take wrong and harmful actions for the vehicle.

## 3. ML02:2023 Data Poisoning Attack

Case of WAF: The SQLI attack for WAF can be countered by various methods which results in easy breakthrough.

## 4. ML03:2023 Model Inversion Attack

Case of Bot Detection: The Taylor Swift Tickets were sold out in a jiffy with the bots.

## 5. ML04:2023 Membership Inference Attack

Case of Banking: A malicious attacker wants to gain access to sensitive financial information of individuals. They do this by training a machine learning model on a dataset of financial records and using it to query whether or not a particular individual’s record was included in the training data. The attacker can then use this information to infer the financial history and sensitive information of individuals.

## 6. ML05:2023 Model Stealing

Case of Cloning ML Model: New sophisticated ways are being researched to clone a model parameter based on interaction.

## 7. ML06:2023 AI Supply Chain Attacks

Case of Trojans: The packages and Frameworks are not tested and certified. Common dev will not aware of the consequences of this.

## 8. ML07:2023 Transfer Learning Attack

Case of Backdoor: The packages and Frameworks are not tested and certified. Common dev will not aware of the consequences of this.

## 9. ML08:2023 Model Skewing

Case of Feedback: An attacker provides fake feedback data to the Financial Bank system, indicating that high-risk applicants have been approved for loans in the past, and this feedback is used to update the model’s training data.

## 10. ML09:2023 Output Integrity Attack

Case of Classifiers: Incorrect classification of Tumorous vs Bening tumor for patients.

## 11. ML10:2023 Model Poisoning

Case of Spams: Mailbox, easy targets.





