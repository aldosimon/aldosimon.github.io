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
dengan karakteristik masing-masing, sehingga ketika melakukan incident response kita memiliki kemudahan untuk melakukan filtering proses yang malicious atau tidak.


<!--more-->
#### pengantar

sebelum melihat lebih jauh proses yang berjalan, baiknya kita mengingat kembali beberapa topik pengantar berikut.
1. user mode vs kernel mode
sebuah proses bisa dijalankan dalam dua buah mmode yang berbeda, yaitu kernel mode dan user mode. aplikasi biasa berjalan di user mode, sedangkan core operating system component berjalan di kernel mode.

2. session 0 vs session 1
sejak windows vista, microsoft memperkenalkan "session 0 isolation". session 0 diperuntukan untuk servis dan aplikasi non-interaktif. user yang login akan berada di session 1 atau sealnjutnya.
proses yang berjalan di session 0 tidak memiliki GUI. sedangkan session 1 (dan seterusnya) untuk proses yang terkait/ dijalankan user.

3. tools
ada beberapa tools yang bisa digunakan untuk memahami lebih jauh proses ini, kita akan menggunakan [processes explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) dan [processes hacker](https://processhacker.sourceforge.io/).
selain itu bisa juga menggunakan command line yaitu tasklist, Get-Process atau ps (PowerShell), dan wmic.

#### windows core processes
###### system

system merupakan process yang berjalan dalam kernel mode, serta menjadi rumah bagi process2 lain yang berjalan di kernel mode.
system dijalankan (parent) oleh PID 0 (system idle process), atau pada process explorer tidak memiliki parent. beberapa ciri-ciri lain dari proses ini adalah:

beberapa karakteristik system process:
* system selalu dijalankan dengan PID 4
* hanya memiliki 1 instance
* berjalan di session 0
* user account yang menjalankan SYSTEM
* tidak memiliki parent process (pada process explorer atau system idle process PID 0 pada process hacker )
* image filename berada di C:\Windows\system32\ntoskrnl.exe * pada process hacker*
* start time:  At boot time

###### system > smss.exe

smss.exe (Session Manager Subsystem), atau windows session manager. smss.exe menjalankan csrss.exe dan wininit.exe di session 0, serta menjalankan csrss.exe dan winlogin.exe di session 1.
seperti yang ditulis sebelumnya, session 0 berisi proses-proses terkait servis sedangkan session 1 untuk proses terkait user.
smss.exe menjalankan proses dengan cara menjalankan child smss process setelah itu melakukan terminasi diri sendiri, sehingga pada suatu waktu seharusnya hanya terdapat sebuah smss.explorer

beberapa karakteristik smss.exe:
* hanya terdapat satu instances
* parent process system
* berjalan di session 0 (karena yang session 1 dst menterminasi diri sendiri)
* user account yang menjalankan SYSTEM
* image path c:\Windows\System32\smss.exe
* start time:  dalam beberapa detik dari boot time (untuk master instance)

###### csrss.exe

proses ini bertanggung jawab menyediakan Windows API, mapping drive letters, and menangani proses shutdown Windows. csrss.exe dijalankan (parent process) oleh smss.exe yang akan mematikan dirinya sendiri setelahnya.
oleh karena itu csrss.exe tidak memiliki parent process (parent process terminated/ non-existent process pada field parent)

beberapa karakteristik csrss.exe:
* tidak mempunyai parent process/ parent process sudah tidak jalan (smss.exe).
* image path c:\Windows\System32\csrss.exe
* bisa terdapat lebih dari satu instances (ingat smss.exe dimana tiap login akan menjalankan csrss.exe dan winlogin.exe pada session baru)
* user account yang menjalankan SYSTEM
* start time:  dalam beberapa detik dari boot time (untuk 2 instances pertama, dan setelah itu setiap ada login baru)

###### wininit.exe

process ini dijalankan oleh smss.exe, dan sama seperti csrss.exe, smss.exe akan mematikan dirinya sendiri setelah menjalankan proses ini, sehingga winit.exe tidak memiliki parent process.
The Windows Initialization Process atau wininit.exe bertanggung jawab menjalankan services.exe (Service Control Manager), lsass.exe (Local Security Authority), dan lsaiso.exe (hanya bila credential guard dinyalakan) dalam Session 0.

beberapa karakteristik wininit.exe:
* tidak mempunyai parent process/ parent process sudah tidak jalan (smss.exe).
* image path c:\Windows\System32\
* hanya satu instances
* user account yang menjalankan SYSTEM
* hati-hati terhadap image dengan nama yang mirip
* start time: dalam beberapa detik dari boot time

###### wininit.exe > services.exe

![services.exe](/images/services.png)

services.exe/ Service Control Manager (SCM) berfungsi mengontrol services yang dijalankan serta mengeset "Last Known Good control set/Last Known Good Configuration (HKLM\System\Select\LastKnownGood)" setelah berhasil login.
informasi services yang dijalankan bisa dilihat di "HKLM\System\CurrentControlSet\Services" atau dengan "sc.exe query". services.exe dijalankan oleh (parent process) winit.exe.

beberapa karakteristik services.exe:
* parent process winit.exe
* image path c:\Windows\System32\
* hanya satu instances
* user account yang menjalankan SYSTEM
* hati-hati terhadap image dengan nama yang mirip
* start time:  dalam beberapa detik dari boot time

###### wininit.exe > services.exe> svchost.exe
![svchost.exe](/images/svchost.png)

svchost.exe (service host/ host process for windows services) bertugas mengontrol windows services. servis yang dijalankan proses ini berbentuk dll, dan dapat dilihat di registry (HKLM\SYSTEM\CurrentControlSet\Services\SERVICE NAME\Parameters).
sebagai contoh, svchost menjalankan service terkait bluetooth, maka kita bisa melihat dll yang dijalankan  dengan
1. processhacker > right click on svchost.exe > properties > services > double click on name.  maka akan menampilkan gambar di atas, dimana pada binary path terlihat dll yang dijalankan.
perlu juga diperhatikan flag/parameter "-k" pada command line di binary path, hal ini merupakan perintah grouping services sejenis (sejak Windows 10 Version 1703 services sejenis dilakukan grouping pada mesin dengan memory di atas 3.5 GB); atau
2. pada registry key "\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BTAGService\Parameters"

beberapa karakteristik svchost.exe:
* parent process services.exe
* Image file path C:\Windows\System32
* hati-hati terhadap image dengan nama yang mirip
* adanya "-k" flag/parameter
* user account yang menjalankan beragam (SYSTEM, Network Service, Local Service) tergantung jenis services (pada windows 10 ada yang dijalankan logged-in user)
* start time:  dalam beberapa detik dari boot time, namun mungkin ada yang berjarak dari boot time

###### lsass.exe

Local Security Authority Subsystem Service (LSASS) adalah process Microsoft Windows operating systems yang berfungsi melakukan enforcing security policy on the system.
beberapa hal yang dilakukan antara lain verifikasi user login, password changes,  membuat access tokens, dan menulis Windows Security Log.

###### winlogon.exe

###### explorer.exe

###### penutup
bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D

#### referensi
1. [user mode and kernel mode](https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)
2. [session 0 isolation](http://securityinternals.blogspot.com/2014/02/windows-session-0-isolation.html)
3. [lsass.exe](https://yungchou.wordpress.com/2016/03/14/an-introduction-of-windows-10-credential-guard/)
4. [services](https://en.wikipedia.org/wiki/Service_Control_Manager)
