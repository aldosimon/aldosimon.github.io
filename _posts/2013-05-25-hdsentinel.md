---
layout: post
title: melihat status hard disk dengan hdsentinel
date: 2013-05-25 08:39
author: admin
comments: true
categories:
  - hardware
  - ubuntu
---
HDD external yang tidak terdeteksi sensors atau hdtemp? Coba gunakan hdsentinel, mampu mendeteksi HDD external yang tidak terdeteksi hdtemp, bahkan yang terhubung via docking.
<!--more-->
  1 . download hdsentinel:
  ```bash
  wget -c http://www.hdsentinel.com/hdslin/hdsentinel_008.zip
  ```
  atau
  ```bash
  wget -c http://www.hdsentinel.com/hdslin/hdsentinel_008_x64.zip
  ```
  2. buka zipnya:
  ```bash
  unzip hdsentinel_008_x64.zip
  ```
  3. pindahkan  ke /usr/local/bin untuk kemudahan access dan ganti namanya jadi smallcase semua untuk mudah mengingat
  ```bash
  mv HDSentinel /usr/local/bin/hdsentinel
  ```

cheers.
