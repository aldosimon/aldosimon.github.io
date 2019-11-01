---
layout: post
title: melihat status hard disk dengan hdsentinel
date: 2013-05-25 08:39
author: admin
comments: true
categories: [hardware, ubuntu]
---
HDD external yang tidak terdeteksi sensors atau hdtemp? Coba gunakan hdsentinel, mampu mendeteksi HDD external yang tidak terdeteksi hdtemp, bahkan yang terhubung via docking.

<strong>1 . download hdsentinel:</strong>
<strong>32bit:</strong> <pre>wget -c http://www.hdsentinel.com/hdslin/hdsentinel_008.zip </pre>

<strong>64bit:</strong> <pre>wget -c http://www.hdsentinel.com/hdslin/hdsentinel_008_x64.zip </pre>

&nbsp;

<strong>2. buka zipnya:</strong>
<pre>unzip hdsentinel_008_x64.zip</pre>
&nbsp;

<strong>3. pindahkan  ke /usr/local/bin untuk kemudahan access dan ganti namanya jadi smallcase semua untuk mudah mengingat</strong>
<pre>mv HDSentinel /usr/local/bin/hdsentinel</pre>
&nbsp;

cheers.
