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
Saya sedang mengerjakan sebuah investigation challenge di tryhackme.com,
dan ada beberapa command line yang saya pakai disana yang menurut saya cukup menarik untuk di dokumentasikan,
serta dapat dipakai sebagai script IR kilat di bagian awal asesmen.

<!--more-->
#### command line yang saya pakai

1. list usernames
```bash
net user
```

2. last logon, group member, password settings, user full name, etc
```bash
net user [username]
```

3. show local group and/or members of groups
```bash
net localgroup
```
```bash
net localgroup "Administrators"
```

4. list running programs (and certain programs only)
```bash
tasklist
tasklist /m /fi “pid eq <Insert Process ID here w/out the brackets>”
```

5. list all schedule task.
```bash
schtasks /query /fo list /v > schtasks.txt
```

6. export security event list to text.
```bash
wevtutil qe Security /f:text > seclogs.txt
```

7. system information
```bash
systeminfo
```

8. various wmic
```bash
wmic /node:<remote-ip> /user:<username> startup list full | more
wmic /node:<remote-ip> /user:<username> service list full | more
wmic /node:<remote-ip> /user:<username> ComputerSystem Get UserName
wmic /node:<remote-ip> /user:<username> useraccount list full
wmic /node:<remote-ip> /user:<username> process get description,processid,parentprocessid,commandline /format:csv
wmic /node:<remote-ip> /user:<username> bios get serialnumber
wmic /node:<remote-ip> /user:<username> diskdrive get model,serialNumber,size,mediaType
```
9.ipconfig
```bash
ipconfig /displaydns
```

10. netstat with PID
```bash
netstat -bona
```

11. see firewall state
```bash
netsh firewall show state
```

#### penutup

bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D

#### referensi

[arts.ubc.ca](https://isit.arts.ubc.ca/how-to-locate-serial-number-of-computer/)
[sans](https://www.sans.org/blog/wmic-for-incident-response/)
[jordanpotti](https://jordanpotti.com/2017/01/20/basics-of-windows-incident-response/)
[cybernote](http://www.cybernote.net/index.php/2020/05/02/practical-incident-response-commands-wmic/)
