---
layout: post
title: how to start networking ubuntu
date: 2012-08-29 12:27
author: admin
comments: true
categories:
  - how to
  - networking
  - ubuntu
---
Mess your connection? here's how to start networking ubuntu from console
<!--more-->
```bash
/etc/init.d/NetworkManager start
ifconfig
ifconfig wlan0 up
iwlist scan iwconfig wlan0 essid essid_name dhcp wlan0
```

cheers :)
