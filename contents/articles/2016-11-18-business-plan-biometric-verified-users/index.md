---
template: article.jade
title: Business Plan - Biometric Verified Users 
date: 2016-11-18 12:30
author: neeraj
aliases: []
categories: [Ruby, Rails, ActiveRecord, Business Plan, Biometric Verified Users, Aten Verify]
tags: [Ruby, Rails, ActiveRecord, Business Plan, Biometric Verified Users, Aten Verify]
excerpt: It's an awesome business plan but available for free so that anyone who possesses the entrepreneurial skills can easily pick. It's also available as an open source. Its a clone of https://poc.crossverify.global.
---

It's an awesome business plan but available for free so that anyone who possesses the entrepreneurial skills can easily pick. It's also available as an open source.

Company A and Company B both can register to [https://biometric-verify.herokuapp.com](https://biometric-verify.herokuapp.com). Company A adds list of users. The users added by Company A can verify their accounts through a confirmation email as well as they can also retrieve their passwords by scanning a QR Code which has attached with confirmation mail. User can upload their passport, driver license and residence proofs images. User can also scan their face, both eyes and both palms and can upload.

Company B can search based on different categories. He can found the Company A within and specific category. Company A can request for users of Company B.

Based on Company A's approval, Company B can get access of credentials of users of Company A. The system is also not storing the user's private data, while just after upload and biometric server, it will delete.

There is a separate server to verify the biometric credentials.

However its clone of [https://poc.crossverify.global](https://poc.crossverify.global). Entire source code is residing over github. The URL is [https://github.com/neerajkumar/biometric-verify](https://github.com/neerajkumar/biometric-verify). You can customize the application according to your need and use for your own purpose.

There is also a demo application which is hosted on heroku. The URL is [https://biometric-verify.herokuapp.com](https://biometric-verify.herokuapp.com).

All other details about the DB architecture, technical architecture, flow chart, sample images etc are also uploaded on github in directory.

<img src="/assets/images/homepage-biometric.png" alt="https://biometric-verify.herokuapp.com" width="740" height="570">

### Architecture and Flow
The DB architecture, Proof of Concept and Techical Architecture files are in [data](https://github.com/neerajkumar/biometric-verify/tree/master/data) directory. [data](https://github.com/neerajkumar/biometric-verify/tree/master/data) directory also contains the flow charts and some sample images files.

### Sample Credentials
For demo purpose, you can use a sample credential of a company i.e. radhadevi98@gmail.com/password. 
