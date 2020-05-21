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
Return Path: alamat email yang akan dikirimi pesan apabila email tidak bisa dikirim
Delivery-date: tanggal pesan diterima
Date: tanggal pesan dikirimkan
Message-ID: nomor identifikasi sebuah pesn. terdiri dari dua bagian [karakterrandom]@[namadomain]. tidak terdapat format baku untuk menghasilkan Message-ID, sehingga tiap server dapat menghasilkan format yang berbeda.
X-Mailer: The mail client (mail program) used to send the message
From: The message sender in the format: "Friendly Name" <email@address.tld>
To: The message recipient in the format: "Friendly Name" <email@address.tld>
Subject: The message subject


#### DKIM
owner domain -> pub/pri key -> pub publish di DNS record
body pesan -> hash -> hash di encrypt dgn pri key
penerima pesan -> decrypt dgn pub key -> calculate body hash -> compare

https://www.youtube.com/watch?v=nK5QpGSBR8c
https://www.metaspike.com/leveraging-dkim-email-forensics/
https://www.arclab.com/en/kb/email/how-to-read-and-analyze-the-email-header-fields-spf-dkim.html


#####the aftermath
