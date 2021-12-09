---
layout: post
published: false
title: mengamati RDP failed login dengan Azure sentinel
author: admin
comments: true
date: '2021-12-09 21:52'
categories:
  - cloud
  - honeypot
---
#### intro

terlalu banyak waktu luang menyebabkan saya menginstall sebuah VM windows di Azure, menghubungan dengan log analytics workspace, serta mengatur beberapa kondisi agar VM tersebut mudah ditemukan, dan memiliki sebuah RDP service yang menerima login serta mematikan firewall.

Tulisan ini akan merangkum apa saja yang saya lakukan, serta bagaimana internet menyambut VM saya yang memiliki RDP login tanpa firewall.

#### menyiapkan VM windows

mematikan firewall
mengizinkan rdp login


#### menyiapkan log analytics workspace (law)

menghubungkan law dengan vm windows

#### menyiapkan azure sentinel

mencoba koneksi


https://github.com/RoganDawes/LOGITacker/blob/USB_host_enum/fingerprint_os.md
https://kieczkowska.com/2020/05/02/usb-101/
https://kieczkowska.com/2020/05/11/usb-forensics/
#####the aftermath
