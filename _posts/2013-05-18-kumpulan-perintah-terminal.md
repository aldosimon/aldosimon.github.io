---
layout: post
title: kumpulan perintah terminal
date: 2013-05-18 15:45
author: admin
comments: true
categories: [shell command, ubuntu]
---
Beberapa perintah terminal yang sering saya gunakan

<strong>melakukan tail pada dmesg</strong>
<pre>dmesg refresh: while true;dmesg -c;sleep 5;done dmesg-c;tail -f /var/log/messages
</pre>
<pre>watch -n [interval] : run terminal command then refresh output every [interval]
</pre>
<strong>turn off screen:</strong>
<pre>xset dpms force standby
</pre>
<strong>see user:</strong>
<pre>lastlog 
</pre>
<pre>cat /etc/passwd
</pre>
<strong>samba start/stop/restart:</strong>
<pre>sudo service smbd start/stop/restart
</pre>

<strong>Melihat detail linux anda</strong>

<pre>
sudo lsb_release -a
</pre>
<pre>
sudo uname -a
</pre>
