---
layout: post
title: fix ubuntu 11.04 wifi problem
date: 2012-08-29 12:30
author: admin
comments: true
categories: [how to, networking, rfkill, ubuntu, ubuntu]
---
no wifi on 11.04?

<strong>SHOW LIST</strong>
<pre>
sudo rfkill list
</pre>


<strong>RESULT</strong>
<pre>
1: phy0: Wireless LAN
    Soft blocked: yes
    Hard blocked: no
2: acer-wireless: Wireless LAN
    Soft blocked: yes
    Hard blocked: no
</pre>

<strong>yes means softblocked. use this:</strong>

<pre>
1.sudo rfkill unblock all sudo rfkill list all
</pre>

<strong>OR</strong>
<pre>
2.sudo rmmod -f acer-wmi sudo rfkill unblock all sudo rfkill list all
</pre>


cheers :)
