---
layout: post
title: membangun home server: installasi dan konfigurasi ssh
date: 2013-06-13 07:46
author: admin
comments: true
categories: [home server, ubuntu]
---
Sebuah box linux belum lengkap tanpa ada akses ssh. Ssh adalah kependekan dari secure shell, dan sesuai dengan namanya kita bisa mengetahui bahwa ini adalah sebuah shell yang secure. Secara sederhana, fungsinya adalah anda bisa memasukan perintah dari jarak jauh, walaupun kemudian penggunaannya tidak hanya untuk memasukan perintah dari jauh (SFTP, SCP, port forwarding dsb).

cukup dengan teorinya, berikut installasi, konfigurasi ssh pada ubuntu 12.04.

1. installasi openssh server
<pre>sudo apt-get install openssh-server</pre>

2. ganti port? konfigurasi lain?
<pre>sudo vi /etc/ssh/sshd_config</pre>
baris yang perlu mendapat perhatian dari config daemon ssh adalah

<pre>Port 22</pre>
ganti dengan port lain untuk meningkatkan keamanan

<pre>ListenAddress 192.168.1.1</pre>
secara default sshd akan menunggu koneksi pada 0.0.0.0, artinya semua ip addressm bila dirasa perlu untuk meningkatkan kemanan, ganti baris ini.

3. setelah mengganti jangan lupa restart servicenya.
<pre>sudo /etc/init.d/ssh restart</pre>
<em>atau</em>
<pre>sudo service ssh restart</pre>

4. tes ssh anda dari mesin lain.
<pre>ssh username@alamatserver</pre>
<em>atau</em>

5. gunakan putty
<em>pada ubuntu:</em> <pre>sudo apt-get install putty</pre>
<em>pada windows: </em><a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" title="download putty">http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html</a>
<a href="http://aldosimon.com/blog/wp-content/uploads/2013/06/putty.png"><img src="http://aldosimon.com/blog/wp-content/uploads/2013/06/putty-300x290.png" alt="putty" width="300" height="290" class="alignnone size-medium wp-image-119" /></a>


<em>Tulisan ini adalah bagian dari tulisan berseri dengan judul <a title="membangun home server sendiri" href="http://aldosimon.com/blog/pembukaan-membuat-home-server-sendiri-1/" target="_blank">membangun home server sendiri</a>.</em>
