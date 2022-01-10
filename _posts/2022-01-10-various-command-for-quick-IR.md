---
layout: post
published: true
title: various command for quick IR
author: admin
comments: true
date: '2022-01-10 23:00'
categories:
  - DFIR
  - incident Response
  - shell
---
#### intro
Saya sedang mengerjakan sebuah investigation challenge di tryhackme.com,
dan ada beberapa command line yang saya pakai disana yang menurut saya cukup menarik untuk di dokumentasikan,
serta dapat dipakai sebagai script IR kilat di bagian awal asesmen.

<!--more-->
#### command lines
.list usernames
```bash
net user
```

.last logon, group member, password settings, user full name, etc
```bash
net user [username]
```

.show local group and/or members of groups
```bash
net localgroup
```
```bash
net localgroup "Administrators"
```

.list running programs
```bash
tasklist
```

.list all schedule task.
```bash
schtasks /query /fo list /v > schtasks.txt
```

.export security event list to text.
```bash
wevtutil qe Security /f:text > seclogs.txt
```


#### penutup
bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D
