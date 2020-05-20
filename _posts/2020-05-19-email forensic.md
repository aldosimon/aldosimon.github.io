---
layout: post
published: false
title: forensik email: analisis header email
author: admin
comments: true
date: '2020-05-19 21:52'
categories:
  -random
---
#### intro
Email merupakan sebuah sarana komunikasi yang sangat digunakan banyak pihak. Penting bagi seorang investigator forensik digital untuk mengerti secara menyeluruh, bagaimana protokol email bekerja. Tulisan ini akan membahas sebagian dari forensik email, dengan berfokus pada header email.

<!--more-->


#### field pada email header

    Return Path: The email address which should be used for bounces.
    The mail server will send a message to the specified email address if the message cannot be delivered
    Delivery-date: The data the message was delivered
    Date: The date the message was sent
    Message-ID: The ID of the message
    X-Mailer: The mail client (mail program) used to send the message
    From: The message sender in the format: "Friendly Name" <email@address.tld>
    To: The message recipient in the format: "Friendly Name" <email@address.tld>
    Subject: The message subject


#### DKIM


https://www.youtube.com/watch?v=nK5QpGSBR8c
https://www.metaspike.com/leveraging-dkim-email-forensics/
https://www.arclab.com/en/kb/email/how-to-read-and-analyze-the-email-header-fields-spf-dkim.html


#####the aftermath
