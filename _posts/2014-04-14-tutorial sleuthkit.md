---
layout: post
title: tutorial sleuthkit
date: 2014-04-14 14:07
author: admin
comments: true
categories: [forensic]
---
<p style="text-align: justify;">The Sleuthkit for Beginner - originally posted on echo|zine issue #29 ( 1-4-14 )</p>
<p style="text-align: justify;">-----| Pendahuluan</p>
<p style="text-align: justify;">Tahapan kedua pada proses forensik digital adalah tahapan analisis, setelah sebelumnya didahului oleh tahap akuisisi data. Tulisan kali ini akan memberikan pengenalan singkat mengenai beberapa tool yang dapat digunakan untuk melakukan analisis forensik. Tool-tool yang digunakan adalah bagian dari the The Sleuth Kit (TSK), yang ditulis oleh Brian Carrier (yang juga merupakan penulis buku Filesystem Analysis yang sangat lengkap bila tertarik mempelajari tentang filesystem)</p>
<p style="text-align: justify;">TSK sendiri terdapat pada repository dari banyak distro yang sudah cukup umum, sehingga tidak sulit menginstallnya. Selain itu terdapat pula autopsy yang merupakan antar-muka bagi TSK.</p>
<p style="text-align: justify;">1.img_stat</p>
<p style="text-align: justify;">tool bawaan dari TSK ini digunakan untuk mengetahui jenis image yang akan di proses, bila yang melakukan proses akuisisi data/image berbeda dengan yang melakukan analisis data, sehingga anda tidak mengetahui jenis image yang akan di proses.</p>
<p style="text-align: justify;">######################################################
input:
jtingkir@laptop:~/FORENSIC/$ img_stat USB2_Kingstone.001
output:
IMAGE FILE INFORMATION
--------------------------------------------
Image Type: raw
Size in bytes: 1572864000
######################################################</p>
<p style="text-align: justify;">tool-tool TSK lain biasanya mampu mendeteksi sendiri jenis image yang akan diproses, namun alangkah baiknya bila jenis image juga diketahui oleh yang melakukan analisis. Pada contoh diatas jenis image adalah raw.</p>
<p style="text-align: justify;">2.mmls
mmls adalah tool yang melakukan analisis pada tingkatan volume, dan digunakan untuk memperlihatkan layout dari sebuah volume (partisi-partisi yang berada didalamnya)</p>
<p style="text-align: justify;">######################################################
input:
jtingkir@laptop:~/FORENSIC/$ mmls USB2_Kingstone.001
output:
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors
Slot Start End Length Description
00: Meta 0000000000 0000000000 0000000001 Primary Table (#0)
01: ----- 0000000000 0000000062 0000000063 Unallocated
02: 00:00 0000000063 0003902975 0003902913 Win95 FAT32 (0x0B)
###################################################################</p>
<p style="text-align: justify;">Pada contoh diatas, mmls digunakan tanpa menentukan jenis imagenya, dan mmls dapat mengenali secara otomatis jenis imagenya. output dari perintah mmls tadi memperlihatkan terdapat tiga partisi dengan awal-akhir sektor terlihat pada kolom kedua dan ketiga sedangkan, jenis partisi terlihat pada kolom terakhir. Partisi dengan jenis FAT32 terlihat dimulai pada offset 63 (sector 63) dari
seluruh image tersebut, hal ini penting untuk kita gunakan pada analisis selanjutnya.</p>
<p style="text-align: justify;">3.fsstat
fsstat adalah tool yang melakukan analisis pada tingkatan file system, mirip seperti mmls, namun pada level yang berbeda. fsstat akan memperlihatkan layout dan detail lainya dari sebuah filesystem.</p>
<p style="text-align: justify;">######################################################
input:
jtingkir@laptop:~/FORENSIC/$ fsstat â€“o 63 USB2_Kingstone.001
output:
FILE SYSTEM INFORMATION
--------------------------------------------
File System Type: FAT32
OEM Name: MSDOS5.0
Volume ID: 0xf0e75840
Volume Label (Boot Sector): NO NAME
Volume Label (Root Directory):
File System Type Label: FAT32
Next Free Sector (FS Info): 7688
Free Sector Count (FS Info): 3895224</p>
<p style="text-align: justify;">Sectors before file system: 63</p>
<p style="text-align: justify;">File System Layout (in sectors)
Total Range: 0 - 3902912
Total Range in Image: 0 - 3071936
* Reserved: 0 - 31
** Boot Sector: 0
** FS Info Sector: 1
** Backup Boot Sector: 6
* FAT 0: 32 - 3835
* FAT 1: 3836 - 7639
* Data Area: 7640 - 3902912
** Cluster Area: 7640 - 3902911
*** Root Directory: 7640 - 7647
** Non-clustered: 3902912 - 3902912</p>
<p style="text-align: justify;">METADATA INFORMATION
--------------------------------------------
Range: 2 - 49028758
Root Directory: 2</p>
<p style="text-align: justify;">CONTENT INFORMATION
--------------------------------------------
Sector Size: 512
Cluster Size: 4096
Total Cluster Range: 2 - 486910</p>
<p style="text-align: justify;">FAT CONTENTS (in sectors)
--------------------------------------------
7640-7647 (8) -&gt; EOF
7648-7687 (40) -&gt; EOF
######################################################</p>
<p style="text-align: justify;">beberapa informasi singkat yang dapat diperoleh adalah sektor dimana terletak FAT files, root directory, ukuran sector, ukuran cluster, dan cluster range. Pada contoh output diatas juga terlihat terdapat dua file yang masih dicatatat oleh FAT entries yaitu file yang dimulai pada sector 7640 (dengan ukuran 8 sector) dan file yang dimulai pada sector 7648 (dengan ukuran 8 sector).
Informasi-informasi ini nantinya digunakan saat melakukan recovery file.</p>
<p style="text-align: justify;">4.fls
fls adalah tool yang melakukan analisis pada tingkatan file name layer, dan digunakan untuk melihat seluruh file yang ada dan sudah dihapus.</p>
<p style="text-align: justify;">######################################################
jtingkir@laptop:~/FORENSIC/$ fls -o 63 -rp USB2_Kingstone.001 &gt; file_list.txt
######################################################</p>
<p style="text-align: justify;">perintah fls diatas akan membuat daftar dari seluruh file (dan folder) dari image USB2_Kingstone.001, hanya pada partisi FAT32 (offset 63 / -o 63) dan hasilnya dituliskan ke file_list.txt. Sedangkan opsi â€“rp artinya dilakukan secara recursive dan mendisplay fullpath dari tiap-tiap file (dan folder).</p>
<p style="text-align: justify;">5.strings
######################################################
strings -t d USB2_Kingstone.001 &gt; strings.txt
grep -i "foto" strings.txt | less
######################################################</p>
<p style="text-align: justify;">perintah strings akan membuat daftar dari seluruh string yang terdapat pada image USB2_Kingstone.001 dan dituliskan ke dalam strings.txt. Setelah itu dapat dilakukan pencarian string yang dibutuhkan dengan perintah grep (opsi -i artinya case insensitive). perintah ini biasanya berguna bila pada image target terdapat catatan mengenai password/email yang dibutuhkan, karena output
perintah strings ini dapat dijadikan wordlist untuk bruteforce attack, menggunakan hydra atau perangkat lain nantinya.</p>
<p style="text-align: justify;">6.timeline
######################################################
creating bodyfile: fls -r -m "/" -i raw -o 63 USB2_Kingstone.001 &gt; bodyfile.txt
mactime -b bodyfile.txt &gt; output_mactime.txt "add -z for timezone"
######################################################</p>
<p style="text-align: justify;">perintah diatas adalah perintah yang digunakan untuk mendaftar seluruh file yang terdapat pada file image, dengan disertai mac-time nya (modified, accessed, created) sehingga bisa terlihat aktifitas file-nya. Namun memang hasilnya harus digunakan dengan hati-hati, karena berbagai OS menggunakan pencatatan mac-time yang agak berbeda, selain itu terdapat kemungkinan
perbedaan timezone.</p>
