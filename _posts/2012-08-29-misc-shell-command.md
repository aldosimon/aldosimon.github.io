---
layout: post
title: misc. shell command
date: 2012-08-29 12:15
author: admin
comments: true
categories: [linux]
---

dmesg refresh
```bash
while true;dmesg -c;sleep 5;done dmesg-c;tail -f /var/log/messages
```
<!--more-->
turn off screen:
```bash
xset dpms force standby
```

see user:
```bash
lastlog
cat /etc/passwd
```

samba start/stop/restart:
```bash
sudo service smbd start/stop/restart
```
