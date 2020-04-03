---
layout: post
title: melihat sekilas komunikasi reverse https meterpreter
date: 2018-03-31 00:02
author: admin
comments: true
categories: [DFIR, forensic, forensic, meterpreter, Image, security, security]
---
<p style="text-align: justify;">Reverse https adalah salah satu kanal komunikasi yang digunakan meterpreter untuk menghubungi command and control server (C2 server). Komunikasi terenkripsi ini merupakan kendala dalam mendeteksi meterpreter. 
Menghadapi kanal terenkripsi seperti https banyak solusi yang umum digunakan, antara lain man in the middle attack, atau corporate proxy macam Symantecs ProxySG, namun tulisan ini hanya akan melihat beberapa indikasi dalam komunikasi reverse https meterpreter tanpa melakukan dekripsi, dan diharapkan yang dapat menjadi starting point untuk mendeteksi meterpreter.   
Penulis menggunakan Metasploit versi 4.14 dan windows 7 sebagai target, dengan seluruh parameter standard.Terdapat kemungkinan kedepannya terdapat perubahan dalam indikasi yang dipaparkan berikut. </p>

<strong>Server Cipher Suite Selection</strong>
<p style="text-align: justify;">Komunikasi via kanal terenkripsi ini cukup rumit dan teknis, penulis melampirkan tautan menuju pembahasan lengkap (dan menarik)[1] di ujung tulisan ini. Bermodalkan pengertian (yang terbatas) mengenai kanal terenkripsi ini, kita bisa melihat beberapa indikasi, salah satu yang pertama yaitu tanggal yang digunakan untuk mengenerasi keyblock sebelum memulai kanal terenkripsi tidak memperlihatkan tanggal yang tepat. Tanggal tersebut seharusnya menyajikan tanggal lokal server, malah meleset beberapa dekade, sebagaimana kita lihat di gambar berikut. Tentu saja ini bisa juga berarti sebuah server 'normal', namun memiliki miskonfigurasi pada tanggal.</p>

<strong>Server Cipher Suite Selection</strong>
<p style="text-align: justify;">Cipher suite yang digunakan oleh server juga merupakan cipher suite yang sangat jarang digunakan oleh server 'normal'. Penelitian Lee, H.K., Malkin, T. and Nahum, E. (2007) [2] menunjukan cipher suite pilihan server ini hanya digunakan oleh 0,03% dari 19.000 sample yang diteliti. Mengingat penelitian ini dilakukan pada 2007, jumlah 0,03% ini akan lebih kecil seiring dengan semakin aman (dan canggih) cipher suite yang digunakan oleh server secara umum.</p>
<img src="http://aldosimon.com/blog/wp-content/uploads//2018/03/Picture1-min.png" alt="" width="536" height="183" class="aligncenter size-full wp-image-302" />

<strong>Client Cipher Suite Selection</strong>
<p style="text-align: justify;">Cipher suite yang digunakan client (meterpreter) juga tidak sesuai dengan user agent strings yang digunakan. Client mengenalkan diri sebagai IE11 namun menggunakan cipher suite yang berbeda dengan yang seharusnya digunakan. Perlu diketahui bahwa dalam kanal terenkripsi kita tidak bisa melihat user agent string client, sehingga sedikit 'guess work' diperlukan.</p>
<img src="http://aldosimon.com/blog/wp-content/uploads//2018/03/Picture2-min.png" alt="" width="543" height="250" class="aligncenter size-full wp-image-303" />
<strong>SSL Certificate</strong>
<p style="text-align: justify;">SSL certificate yang digunakan oleh C2 server juga merupakan indikasi sendiri. Kita bisa lihat bahwa hanya common name (CN) yang memiliki value, itupun terdiri dari 6 random character dan tidak menyerupai nama domain. Certificate-nya sendiri tidak 'di-tandatangan' oleh trusted certificate authority.</p>
<img src="http://aldosimon.com/blog/wp-content/uploads//2018/03/Picture3-min.png" alt="" width="547" height="550" class="aligncenter size-full wp-image-313" />

<p style="text-align: justify;">Untuk melakukan deteksi beberapa indikasi diatas dapat dibuat menjadi filter/rule IDS. Namun perlu diingat bahwa sangat mungkin indikasi tersebut mengalami perubahan, karena perubahan source code metasploit atau perubahan parameter oleh user. Namun indikasi-indikasi tersebut merupakan tempat yang cukup untuk memulai deteksi.</p>

<p style="text-align: justify;">
[1] The First Few Milliseconds of an HTTPS Connection - http://www.moserware.com/2009/06/first-few-milliseconds-of-https.html</p>
<p style="text-align: justify;">[2] Lee, H.K., Malkin, T. and Nahum, E., (2007) ‘Cryptographic strength of ssl/tls servers: current
and recent practices.’ In Proceedings of the 7th ACM SIGCOMM conference on Internet
measurement (pp. 83-92). ACM</p>

