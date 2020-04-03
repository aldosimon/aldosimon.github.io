---
layout: post
title: membangun home server: installasi CUPS 
date: 2013-06-08 15:08
author: admin
comments: true
categories: [home server, ubuntu]
---
Installasi printer via terminal di ubuntu server sangat berbelit-belit, tapi CUPS membuat segalanya jadi sederhana. CUPS sendiri adalah aplikasi printer server, dan enaknya adalah CUPS punya antarmuka berbasis web. Begini cara installnya.

1. installasi cups
<pre>sudo apt-get install cups</pre>

2. tambahkan user ke grup lpadmin, atau gunakan sudo atau login root saat mengadministrasikan printer via antarmuka webnya CUPS.
<pre>sudo usermod -a -G lpadmin [nama_user]</pre>


3. edit config cups sebagai berikut:
<pre>sudo vi /etc/cups/cupsd.conf</pre>



a. bila digunakan di headless server, ganti <em>'Listen localhost:631'</em> menjadi <em>'Listen *:631.'</em>
b. Pada tag2 <location/>, <Location /printers> dan <Location /admin>, tambahkan baris 'Allow all' atau ganti dengan subnet anda 'Allow  192.168.1.*'
c. hasil akhir config

<pre>
Listen *:631
<Location />
Order allow,deny
Allow 192.168.1.*
</Location>
<Location /printers>
Order allow,deny
Allow 192.168.1.*
</Location>
# Restrict access to the admin pages...
<Location /admin>
Order allow,deny
Allow 192.168.1.*
</Location>
</pre>


4. restart cups
<pre>sudo /etc/init.d/cups restart</pre>
atau
<pre>sudo service cups restart</pre>


5.tambahkan printer baru serta jangan lupa men-share melalui antar muka webnya
<pre>https://ip_server_printer:631/admin</pre>


cheers.

<em>Tulisan ini adalah bagian dari tulisan berseri dengan judul <a title="membangun home server sendiri" href="http://aldosimon.com/blog/pembukaan-membuat-home-server-sendiri-1/" target="_blank">membangun home server sendiri</a>.</em>
