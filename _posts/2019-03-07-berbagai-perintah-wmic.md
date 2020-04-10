---
layout: post
title: berbagai perintah wmic
date: 2019-03-07 08:00
author: admin
comments: true
categories: [DFIR, forensic, forensic, triage]
---
beberapa perintah wmic yang berguna saat melakukan *triage*, yang *rencananya* akan terus di update
<!--more-->
#### nama, model, ukuran dan serial number hard drive
```bash
wmic diskdrive get name,model,size,serialnumber
```
#### versi windows dan uuid mesin
```bash
wmic csproduct get name,version,uuid</code>
```
#### user account dan security identifier:
```bash
wmic useraccount get name,sid</code>
```
*updated 07/03/2019*
