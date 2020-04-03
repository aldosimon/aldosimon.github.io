---
layout: post
title: cara mengetahui password speedy
date: 2013-04-22 02:21
author: admin
comments: true
categories: [internet]
---
Modem Speedy sudah dipasang bertahun-tahun lalu dan tidak pernah diutak-atik, bila sama seperti saya, kemungkinan besar anda juga lupa password bahkan username speedy anda. Beginilah cara melihatnya.

<pre>
1. Masuk Command Prompt di menu start-program-accesories-command prompt. Atau gunakan putty, namun ganti koneksinya jangan ssh melainkan telnet (port 23)
2. Sambungkan modem dengan komputer, bisa memakai LAN atau USB.
3. Di jendela Command Prompt tadi, ketikkan : Telnet 192.168.1.1 lalu tekan Enter di keyboard.
4. Jika sukses akan muncul tulisan " Login ", ketikkan password modem nya (secara default user admin password admin, kecuali sudah dirubah) lalu tekan Enter.
5. Jika password benar, maka anda akan langsung masuk ke dalam konfigurasi modem dengan tampilan DOS ditandai dengan muncul tulisan " TP-LINK &gt; ".
6. Untuk melihat isinya anda tinggal mengetikkan " show all " lalu Enter, cari tulisan PPP Username yang merupakan username modem anda dan PPP Password yang merupakan password modem anda.
7. cek kebenaran password tadi di http://telkomspeedy.com/info-pemakaian-speedy
</pre>


*diolah dari berbagai sumber.
