---
layout: post
published: true
title: the fault in our shell
author: admin
comments: true
date: '2020-09-09 11:34'
categories:
  -forensic
  -writings
  -honeypot
---
Tulisan lama saya mengenai honeypot. Secara umum sebagian besar malware yang melakukan serangan merupakan varian dari Mirai.
<!--more-->
Serangan dilakukan dengan menggunakan brute force kombinasi username/password yang sangat umum ditemui dalam wordlist/dictionary maupun username/password default milik beberapa device IoT.

Setelah mendapatkan akses, malware melakukan fingerprinting, mengunduh  dan menjalankan payload. Dari 43 payload unik yang diunduh, terdapat 6 (enam) dari yang bukan merupakan executable. 5 (lima) diantaranya merupakan bash/shell script, sedangkan sisanya merupakan backdoor perl script yang berkomunikasi ke C2 server melalui protocol IRC.

Dikarenakan keterbatasan interaksinya, cowrie tidak mendokumentasikan aktivitas dari payload yang diunduh dan dijalankan, hal ini dapat dijadikan topik penelitian lebih lanjut namun diperlukan penggunaan high interaction honeypot.

rinciannya dapat dilihat disini:
[the fault in our shell](https://aldosimon.com/writings/The Fault in Our Shell.pdf)

originally posted on:
[cdef 4](https://aldosimon.com/writings/cdef4.pdf)
