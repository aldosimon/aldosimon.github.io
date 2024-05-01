---
title: USBdeview timestamp quirk
date: 2024-05-01 02:57:00 Z
---

---
layout: post
title: USBdeview timestamp quirk
date: 2016-09-29 08:29
author: admin
comments: true
categories: [forensic, registry, windows]
---
<p style="text-align: justify;">USBdeview adalah sebuah program yang mampu "menerjemahkan" catatan pada registry windows yang berkaitan dengan koneksi USB. Mudahnya begini, aplikasi ini bisa membuat anda mengetahui USB device apa saja yang pernah dikoneksikan, kapan device tersebut dikoneksikan dan banyak informasi lain.</p>
Saya sendiri seringkali menggunakan USBdeview dalam proses akuisisi live system. Karena selain mudah digunakan, dengan USBdeview, kita bisa dengan cepat mengetahui informasi dari device target.Karena memang tidak dirancang sebagai sebuah perangkat forensik digital, terdapat beberapa hal yang perlu diketahui dalam menggunakan perangkat semacam ini. Selalu lakukan pengujian terlebih dahulu untuk meyakinkan bahwa perangkat dimaksud akan menghasilkan informasi yang "<em>forensically sound</em>".
<p style="text-align: justify;">Salah satu kendala yang dimiliki oleh USBdeview adalah informasi yang ditampilkan terkait <em>created date. </em>Permasalahannya sendiri tidak terdapat pada usbdeview sendiri, namun cara windows mencatat informasi tersebut pada registry.</p>
<p style="text-align: justify;"><em>Creation date</em> dicatat pada HKLM\System\CurrentControlSet\Enum\USB\VID&amp;PID, menariknya, pada sistem operasi Windows 7, key tersebut hanya meng-<em>update </em>pada <em>insertion </em>USB  pertama setelah <em>shutdown. </em>Bila <em>user </em>melakukan hibernasi, maka USB <em>device</em>  baru dapat di pasang tanpa mengupdate key tersebut, sampai nanti ketika <em>user </em>melakukan <em>reboot</em> kembali.</p>
&nbsp;

<hr />

sumber:
[1]<a href="http://dfire.ucd.ie/?p=337">Some Pitfalls of Interpreting Forensic Artifacts in Windows Registry</a>
[2]<a href="http://www.nirsoft.net/utils/usb_devices_view.html">Nirsoft's USBdeview</a>
