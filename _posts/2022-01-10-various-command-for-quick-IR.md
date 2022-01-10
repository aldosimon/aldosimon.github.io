---
layout: post
published: true
title: various command for quick IR
author: admin
comments: true
date: '2021-12-23 22:00'
categories:
  - DFIR
  - incident Response
  - shell
---
#### intro


<!--more-->
#### informasi terkait log4j/ log4shell
```bash
net user
```
list usernames

```bash
net user [username]
```
last logon, group member, password settings, user full name, etc

```bash
net localgroup
```
```bash
net localgroup "Administrators"
```
show local group and/or members of groups

```bash
tasklist
```

list running programs

```bash
schtasks /query /fo list /v > schtasks.txt
```
list all schedule task.

```bash
wevtutil qe Security /f:text > seclogs.txt
```
export security event list to text.

#### penutup
walaupun cukup heboh, nampaknya tidak terlalu banyak juga penyerang yang memanfaatkan log4j. Mungkin juga ada attacker yang ketika mengenali versi nginx yang saya jalankan, dan memutuskan tidak perlu mengirimkan string "jndi" sehingga saya tidak mendeteksinya (*pendapat tidak berdasar).
sudah terdapat beberapa langkah mitigasi di dunia maya, sehingga bila anda terdampak, sangat disarankan melakukan mitigasi.
log dari inetsim sendiri akan saya update bila hasilnya menarik.
