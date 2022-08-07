---
layout: post
published: false
title: various command for quick IR
author: admin
comments: true
date: '2022-01-10 23:00'
categories:
  - DFIR
  - incident Response
  - shell
---
winevent viewer shell
#### command line yang saya pakai


#### penutup

bagusnya sih dirangkum dalam sebuah script yang dapat dengan mudah langsung dijalankan.
post ini tentu saja akan saya update d masa mendatang, dan semoga suatu saat script yang menyatukannya akan saya kerjakan :D

#### referensi

1. [arts.ubc.ca](https://isit.arts.ubc.ca/how-to-locate-serial-number-of-computer/)
2. [sans](https://www.sans.org/blog/wmic-for-incident-response/)
3. [jordanpotti](https://jordanpotti.com/2017/01/20/basics-of-windows-incident-response/)
4. [cybernote](http://www.cybernote.net/index.php/2020/05/02/practical-incident-response-commands-wmic/)

https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/Get-WinEvent?view=powershell-7.1
https://docs.microsoft.com/en-us/previous-versions/dotnet/netframework-4.0/ms256115(v=vs.100)
https://devblogs.microsoft.com/powershell/powershell-the-blue-team/
https://adamtheautomator.com/get-winevent/
https://r1d1cul0us.xyz/posts/pentest-001/boxes/windows/windows-tools/


Get-WinEvent -Path <Path to Log> -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=<Port>'

wevtutil qe security /f:text “/q:*[System[(EventID=4722)]]”
