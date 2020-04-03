---
layout: post
title: membangun home server sendiri (pembukaan)
date: 2013-05-26 01:53
author: admin
comments: true
categories: [home server]
---
Seluruh post dengan sub judul "membuat home server sendiri" adalah guide untuk membangun home server anda. 
Secara umum guide ini akan terdiri dari langkah-langkah dari mempersiapkan hardware, software sampai langkah-langkah lain sehingga anda bisa membangun home server sendiri dengan ubuntu server sebagai operating system yang akan berfungsi sebagai file server, media server, torrent box bahkan web server.

<strong>Mengapa ?</strong>
Karena saya ga ada hobi lain. Selain itu:
	<li>file server/media server: file/media server memusatkan penyimpanan anda. Sehingga tidak setiap PC, laptop, galaxy tab, HTPC, PS3 dan xbox masing-masing menyimpan episode game of thrones terbaru, namun semuanya berbagi pakai. </li>
	<li>Torrent box: seed bro! dedicated torrent box memberi kesempatan anda untuk memberi kembali kepada komunitas torrent dengan melakukan seeding. Karena sekaligus berfungsi sebagai file server jadi anda bisa men-download, share, seed tanpa repot memindah-mindahkan file</li>
	<li>web server: bila anda berminat, anda bisa menggunakan service dinamic dns dan menjalankan web server, tidak ada lagi biaya hosting.</li>


<strong>Lumayan kan?</strong>
berikut daftar lengkap rangkaian tulisan ini (work in progress):
1. persiapan hardware

2. installasi operating system

3. installasi dan konfigurasi remote access (ssh, webmin)
3.a <a href="http://aldosimon.com/blog/installasi-dan-konfigurasi-ssh-pada-ubuntu-12-04-lts/" title="installasi dan konfigurasi ssh" target="_blank">installasi dan konfigurasi ssh</a>
3.b <a href="http://aldosimon.com/blog/membangun-home-server-installasi-dan-konfigurasi-webmin/" title="installasi dan konfigurasi webmin" target="_blank">installasi dan konfigurasi webmin</a>


4. installasi dan konfigurasi aplikasi berbagi-pakai server (samba, subsonic, CUPS)
4.a installasi dan konfigurasi samba
4.b installasi dan konfigurasi subsonic
4.c <a href="http://aldosimon.com/blog/installasi-cups/" title="installasi dan konfigurasi CUPS" target="_blank">installasi dan konfigurasi CUPS</a>

5. installasi lain
5.a installasi dan konfigurasi deluge
5.b installasi dan konfigurasi filebot
5.c installasi dan konfigurasi apache dan mysql

6. sambungkan dengan internet dan akses dari mana saja 
6.a installasi noip2 sebagai ddns dan port forwarding
