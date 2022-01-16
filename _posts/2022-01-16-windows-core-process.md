---
layout: post
published: true
title: windows core processes
author: admin
comments: true
date: '2022-01-16 23:00'
categories:
  - DFIR
  - incident Response
  - windows internals
  - sysinternals
  - malware
---
Dalam sebuah kegiatan incident response, adakalanya kita perlu mengetahui proses yang sedang berjalan.
Sehingga dapat memutuskan apakah proses tersebut malicious atau tidak. Berikut beberapa proses inti windows (Windows core processes),
untuk melakukan filtering proses yang malicious atau tidak.

<!--more-->
#### pengantar

sebelum melihat lebih jauh proses yang berjalan, baiknya kita mengingat kembali beberapa topik pengantar berikut.
1. user mode vs kernel mode
sebuah proses bisa dijalankan dalam dua buah mmode yang berbeda, yaitu kernel mode dan user mode. aplikasi biasa berjalan di user mode, sedangkan core operating system component berjalan di kernel mode.

2. session 0 vs session 1
sejak windows vista, microsoft memperkenalkan "session 0 isolation". session 0 diperuntukan untuk servis dan aplikasi non-interaktif. user yang login akan berada di session 1 atau sealnjutnya.
proses yang berjalan di session 0 tidak memiliki GUI.

3. tools
ada beberapa tools yang bisa digunakan untuk memahami lebih jauh proses ini, kita akan menggunakan [processes explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) dan [processes hacker](https://processhacker.sourceforge.io/)

#### windows core processes
1. system


#### penutup
bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D

#### referensi
1. [user mode and kernel mode](https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)
2. [session 0 isolation](http://securityinternals.blogspot.com/2014/02/windows-session-0-isolation.html)
