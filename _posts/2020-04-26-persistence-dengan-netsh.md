---
layout: post
published: true
title: persistence dengan netsh
author: admin
comments: true
date: '2020-04-26 10:54'
categories:
  -forensic
  -persistence
  -artifacts
---

Sebagai sebuah blog yang *mengaku* blog forensik, malu rasanya tidak pernah menulis tentang forensik. Tadi pagi ketika membaca thisweekin4n6, ada artikel yang cukup menarik, yaitu teknik menggunakan netsh dalam melakukan persistence. Netsh sendiri secara default terinstall di Windows, sehingga teknik ini merupakan teknik yang cukup menarik untuk dibahas walaupun sudah lama beredar.

<!--more-->

#### intro
Netsh adalah command line utility yang memiliki fitur untuk melihat dan memodifikasi konfigurasi network (local dan remote). Selain fungsi utama tersebut, ternyata netsh memiliki fitur untuk menambahkan dll untuk di-load ketika netsh dijalankan. Fitur  yang satu ini dapat digunakan untuk melakukan persistence, dan disebut dengan helper.

#### eksekusi

1. buat dan upload payload

    tulisan ini hanya bicara mengenai persistence, oleh karena itu kita tidak akan membahas detail mengenai dua hal tersebut. Anda yang tertarik dapat meriset lebih jauh. Yang termudah dan paling umum adalah dengan membuat payload via msfvenom. Untuk kepentingan tulisan ini, maka payload kita akan disebut dengan payload.dll


2. tulis registry untuk menjalankan  netsh

    anda dapat menjalankan netsh melalui registry key berikut (atau dengan berbagai registry key lain)
    ```bash
    "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run" /v namakey /t REG_SZ /d "C:\Windows\System32\netsh"
    ```

    lalu tambahkan helper ke netsh, melalui perintah di command line windows:
    ```bash
    netsh add helper payload.dll
    ```

setelah ini, makan setiap restart, perintah pertama (di registry) akan menyalakan netsh, namun netsh yang secara otomatis me-load helper yang merupakan payload kita.

#### deteksi
untuk mendeteksi persistence ini, ada dua registry key yang bisa dipantau. pertama registry terkait persistence standar (kita menggunakan Run, seperti contoh di atas):

```js
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

serta registry terkait helper netsh, yang bisa dilihat di
```bash
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NetSh
```

bagi anda pengguna sysmon, maka dapat menambahkan line berikut (khusus untuk dua key di atas)
```bash
<RuleGroup name="" groupRelation="or">
  <RegistryEvent onmatch="include">
    <TargetObject name="T1060" condition="contains">CurrentVersion\Windows\Run</TargetObject> <!--Windows: [ https://msdn.microsoft.com/en-us/library/jj874148.aspx ] -->
    <TargetObject condition="begin with">HKLM\Software\Microsoft\Netsh</TargetObject> <!--Windows: Netsh helper DLL [ https://attack.mitre.org/wiki/Technique/T1128 ] -->
  </RegistryEvent>
</RuleGroup>
```
atau gunakan sysmon config swiftonsecurity (link di akhir tulisan).

#### penutup
Keuntungan utama dari persistence ini adalah netsh merupakan perintah yang cukup sering ditemui pada Windows, terutama server, sehingga melihat sebuah key menjalankan netsh dapat dengan mudah terlewat. Lebih jauh key konfigurasi helper netsh termasuk yang jarang dipantau.


* [mitre](https://attack.mitre.org/techniques/T1128/)
* [swiftonsecurity](https://github.com/SwiftOnSecurity/sysmon-config/blob/master/sysmonconfig-export.xml)
* [pentestlab on persistence](https://pentestlab.blog/2019/10/29/persistence-netsh-helper-dll/)
