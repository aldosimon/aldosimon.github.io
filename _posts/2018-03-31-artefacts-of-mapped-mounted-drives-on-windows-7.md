---
layout: post
title: forensik artefak dari mapped/mounted drives di Windows 7
date: 2018-03-31 00:37
author: admin
comments: true
categories: [DFIR, forensic, forensic, registry]
---
<p style="text-align: justify;">Post ini lahir dari frustasi saat penugasan gw beberapa hari lalu. PC target terlihat sangat bersih dari data, dan saat melihat beberapa aktifitas terakhir dari user pada PC tersebut, terlihat bahwa user sebelumnya pernah me-<em>mount</em> beberapa network drive. Tentu saja hasil yang terlihat tidak lengkap, dan malah membuat penasaran. Pertanyaan yang muncul pertama adalah apakah user pernah me-<em>mount</em> <em>network drive</em> lain.</p>

<p style="text-align: justify;">
Beberapa <em>registry key</em> yang patut dilihat untuk mengetahui apakah user pernah me-<em>mount</em> <em>drive </em>lain adalah:
<pre>
1. Map Network Drive MRU
Software\Microsoft\Windows\CurrentVersion\Explorer\Map Network Drive MRU

2. MountPoints2
Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2

3. RunMru
Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
</pre>
<a href="http://aldosimon.com/blog/wp-content/uploads//2018/03/map-net-drive.jpeg"><img src="http://aldosimon.com/blog/wp-content/uploads//2018/03/map-net-drive.jpeg" alt="" width="810" height="468" class="aligncenter size-full wp-image-336" /></a>
<p style="text-align: justify;">
Hal lain yang perlu juga menjadi perhatian adalah kapan <em>registry-registry key </em> tersebut terakhir ditulis (dalam kasus gw 'dibersihkan'). Tentu saja sebuah <em>script batch</em> atau program kecil untuk mengekstraksi dan me-<em>parse </em><em>registry key</em> di atas dapat sangat membantu di lapangan.
</p>

