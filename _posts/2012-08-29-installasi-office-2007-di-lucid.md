---
layout: post
published: true
title: installasi office 2007 di ubuntu lucid
date: 2012-08-29 12:31
author: admin
comments: true
categories: [how to, ubuntu, ubuntu]
---
baru saja menginstallkan ubuntu ke salah satu temen dan karena  katanya ga bisa make open office, jadi diinstallin Office 2007 deh, berikut langkah nya:
<!--more-->
1. Download Wine (http://wine.budgetdedicated.com/archive/index.html), saya pakai versi  1.1.24, dan ga tau kenapa versi terbarunya malah ga bisa.

2. Jalankan installasi Office 2007, Run setup.exe via wine (right click-run with wine)
ps. no. 2, pertama saya coba jalankan setup via USB tidak bisa, via directory windows lama yang masih ntfs tidak bisa, kopikan filesnya ke desktop ubuntu baru bisa. Lagi2 saya tidak tahu kenapa, silahkan trial and error aja.

3. Install dlls, fonts dan lain-lain yang dibutuhkan officenya.
`sudo wget www.kegel.com/wine/winetricks`
`chmod +x ./winetricks`
`cabextracts sudo apt-get install cabextract`
`./winetricks gdiplus riched20 riched30 msxml3 msxml4 msxml6 corefonts tahoma vb6run vcrun6 msi2`

4. wincfg
terminal > wincfg > pilih tab libraries > pilih rpcrt4 > edit to native then built in

5. restart!

ps. originally posted on oprekpc.com/forum

cheers :)
