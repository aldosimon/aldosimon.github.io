---
layout: post
title: office 2007 on lucid
date: 2012-08-29 12:31
author: admin
comments: true
categories: [how to, ubuntu, ubuntu]
---
Baru saja menginstallkan ubuntu ke salah satu temen dan karena  katanya ga bisa make open office, jadi diinstallin Office 2007 deh...here goes...Mohon maaf bukan tutorial, sekedar berbagi aja. 

1. Download Wine (http://wine.budgetdedicated.com/archive/index.html), saya pakai versi  1.1.24, dan ga tau kenapa versi terbarunya malah ga bisa.

2. Jalankan installasi Office 2007, Run setup.exe via wine (right click-run with wine)
ps. no. 2, pertama saya coba jalankan setup via USB&gt; tidak bisa, via directory windows lama yang masih ntfs &gt; tidak bisa, kopikan filesnya ke desktop ubuntu &gt; baru bisa. Lagi2 saya tidak tahu kenapa, silahkan trial and error aja.

3. Install dlls, fonts dan lain-lain yang dibutuhkan officenya. 
3.a winetricks : <pre>
sudo wget www.kegel.com/wine/winetricks
</pre>

3.a.1 <pre>
chmod +x ./winetricks
</pre>

3.b <pre>
cabextracts sudo apt-get install cabextract
</pre>

3.c fonts, dlls : <pre>
./winetricks gdiplus riched20 riched30 msxml3 msxml4 msxml6 corefonts tahoma vb6run vcrun6 msi2
</pre>


4. wincfg
terminal &gt; wincfg &gt; pilih tab libraries &gt; pilih rpcrt4 &gt; edit to native then built in

5. saya sih abis itu  direstrat, ga tau klo ga direstart piye....  

6. Voila Word 2k7 on lucid doing lorem ipsum 

Berhubung saya juga cumin coba2, klo ada kendala boleh post, tapi ga janji bisa ya. good luck.  

ps. sorry bingung mau kasih screnie apa, abis isinya terminal smua.

pps. originally posted on oprekpc.com/forum

cheers :)
