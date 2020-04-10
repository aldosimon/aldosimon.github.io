---
layout: post
title: berbagai display filter wireshark
date: 2019-04-30 10:09
author: admin
comments: true
categories: [DFIR, forensic, forensic]
---
wireshark digunakan untuk melakukan network packet analysis. Untuk membantu melakukan analysis, dapat dilakukan filtering, capture filter dan display filter. Berikut beberapa display filter yang seringkali saya lupakan, sehingga perlu ditulis disini:
<!--more-->
#### filter web browsing (http/https web traffic)

```bash
http.request or ssl.handshake.type == 1.
```

#### identifying host information (mac address and hostname) from dhcp

```bash
bootp
```
or
```bash
dhcp
```

setelah melakukan filtering (dhcp atau bootp), pada jendela packet details (jendela tengah) bisa terlihat mac address dari client, serta hostname dari client tersebut.
{% comment %}
old picture
<p style="text-align: justify;"><a href="http://aldosimon.com/blog/wp-content/uploads//2019/04/mac-and-host-name.png"><img class="aligncenter size-full wp-image-406" src="http://aldosimon.com/blog/wp-content/uploads//2019/04/mac-and-host-name.png" alt="" width="572" height="243" /></a></p>
{% endcomment %}
#### identifying host information (mac address and hostname) from nbns
terkadang dhcp lease  (proses meminta ip address ke dhcp server) sudah terjadi, sehingga tidak ditemukan traffic di atas. Pada kasus tersebut dapat digunakan traffic NBNS, dengan filter berikut:
```bash
nbns
````
setelah melakukan filtering (nbns), pada jendela packet details (jendela tengah) bisa terlihat mac address dari client, serta hostname dari client tersebut.
{% comment %}
<a href="http://aldosimon.com/blog/wp-content/uploads//2019/04/mac-and-host-name2.png"><img class="aligncenter size-full wp-image-408" src="http://aldosimon.com/blog/wp-content/uploads//2019/04/mac-and-host-name2.png" alt="" width="855" height="323" /></a><span style="font-size: 10pt;"><em>*update 30 April 2019</em></span>
{% endcomment %}
