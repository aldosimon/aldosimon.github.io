---
title: beberapa hal unik di situs publik
date: 2021-12-11 21:52:00 Z
categories:
- malware
layout: post
author: admin
comments: true
---

#### intro

Tulisan ini mendokumentasikan beberapa hal unik yang kebetulan saya lihat bersarang di situs publik. Biasanya saya temukan ketika sedang iseng menjelajah internet.
<!--more-->
#### mining script di kemsos.go.id
<!--tanggal lapor: 18/09/2018-->

sebuah mining script di root kemsos.go.id yang di host di drupalupdates.tk dan hasil analisis [virustotal](https://www.virustotal.com/gui/url/329bf5da32eab152c3d09fde888de95a522e47366357ad08acd15fd2a1614f74/detection).

#### php webshell di website bpjs kesehatan
<!--tanggal lapor: 08/02/2019-->
1. alamat backdoor (sudah dibersihkan) [di sini](https://www.bpjs-kesehatan.go.id/bpjs/dmdocuments/1ee3b84cd9e26741c72ea52a93ebd7c1.doc)
2. backdoornya di encode dengan: gzinflate dan base64_decode
[di sini](/scripts/bpjs/avwillkillthis.7z) password: avwillkillthis
3. scan virustotal
[encoded](https://www.virustotal.com/en/file/b903fa1822ce9e817c024af675cfd9ce861b9abf6a2935f60dbaf8f0ca4551f2/analysis/1549635991/)
dan [decoded](https://www.virustotal.com/en/file/df1f419d1fda2c606a2782e091a8439623f4920b6728be12a15090c6648e5bf2/analysis/1549636052/)
4. menggunakan bahasa indonesia, sehingga ada kemungkinan ditulis oleh orang Indonesia,serta ada tulisan  $version = 'R57'; jadi kemungkinan merupakan variasi webshell r57,

#### php webshell di jdih
<!--tanggal lapor: 08/02/2019-->
1. alamat backdoornya (sudah dibersihkan) [di sini](https://jdih.kpu.go.id/data/foto/admin.php.txt)
2. scan virustotal filenya:[di sini](https://www.virustotal.com/en/filecf88126368ae74075da8d3be02177b13ea768e3f3bab926c0d628abac331cb86/analysis/)
