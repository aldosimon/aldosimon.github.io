---
title: fix ubuntu 11.04 wifi problem
date: 2012-08-29 12:30:00 Z
categories:
- how to
- networking
- rfkill
- ubuntu
layout: post
author: admin
comments: true
---

No wifi on 11.04?
<!--more-->

#### show list
```bash
sudo rfkill list
```


#### result
```bash
1: phy0: Wireless LAN
    Soft blocked: yes
    Hard blocked: no
2: acer-wireless: Wireless LAN
    Soft blocked: yes
    Hard blocked: no
```
yes means softblocked. use this:

```bash
sudo rfkill unblock all sudo rfkill list all
```
or

```bash
sudo rmmod -f acer-wmi sudo rfkill unblock all sudo rfkill list all
```


cheers :)
