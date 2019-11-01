---
layout: post
title: membangun home server: installasi dan konfigurasi webmin
date: 2013-06-29 01:16
author: admin
comments: true
categories: [uncategorized]
---
Selain menggunakan ssh, webmin bisa digunakan untuk melakukan administrasi pada box linux. Webmin sendiri menggunakan antar muka berbasis web, sehingga hanya memerlukan perambah maya (browser) biasa sebagai kliennya. Selain antarmuka yang ramah, webmin menyediakan berbagai macam pilihan kostumasi box linux, mulai dari apache, samba bahkan sampai user. Oleh karena berbagai kemudahan dan fitur itu webmin cocok digunakan untuk kostumasi sehari-hari, namun ada baiknya tetap mengerti ssh.

Berikut cara menginstall webmin pada ubuntu 12.04.

1. download program2 yang dibutuhkan webmin
<pre>
sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python
</pre>

2.download webmin
<pre>
wget http://prdownloads.sourceforge.net/webadmin/webmin_1.580_all.deb
</pre>

3. install deb tadi dengan dpkg
<pre>
sudo dpkg -i webmin_1.580_all.deb
</pre>

4. login ke webmin di alamat http://ipserver:10000
<img src="http://aldosimon.com/blog/wp-content/uploads/2013/06/webmin.png" alt="login ke webmin" />

<em>Tulisan ini adalah bagian dari tulisan berseri dengan judul <a title="membangun home server sendiri" href="http://aldosimon.com/blog/pembukaan-membuat-home-server-sendiri-1/" target="_blank">membangun home server sendiri</a>.</em>
