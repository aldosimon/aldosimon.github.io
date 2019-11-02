---
layout: post
title: Penggunaan fuzzy hash dalam ediscovery
date: '2019-08-24 06:35'
author: admin
comments: true
categories:
  - ediscovery
  - forensic
  - ssdeep
published: true
---
Beberapa saat yang lalu, ketika sedang menunggu rekan-rekan praktisi forensik digital melakukan akuisisi pada beberapa perangkat komunikasi, mata penulis terpaku pada sebuah poster SANS forensic yang berisi teknik identifikasi <em>malware</em>. Dari beberapa teknik identifikasi tersebut, terdapat salah satu teknik untuk menghitung kesamaan dari dua buah <em>malware</em> yang berbeda. Penulis bertanya di dalam hati, mengapa teknik tersebut tidak digunakan untuk mencari dokumen elektronik yang memiliki kesamaan. Pertanyaan tersebut tidak menuai jawaban hari itu….karena penulis bertanya di dalam hati.

<strong>pengantar</strong>
Sebagai praktisi forensik digital, terutama yang berfokus di e-discovery, kegiatan menemukan file sudah menjadi makanan sehari-hari. Namun adakalanya diperlukan juga bagi praktisi forensik digital untuk menemukan file yang serupa dengan file yang sudah ditemukan (file yang sudah di-<em>update, </em>di-<em>edit, </em>di-<em>save as­</em> dsb).  Fuzzy hash dapat digunakan sebagai salah satu alternative untuk menyelesaikan permasalahan tersebut.

<strong>hash dan fuzzy hash</strong>
<em>Hash function </em>adalah fungsi matematis yang digunakan untuk “merangkum” data elektronik yang berjumlah berapa pun menjadi jumlah tetap yang disebut dengan <em>hash values</em>. Data elektronik yang berbeda kontennya, akan menghasilkan <em>hash value </em>yang berbeda. Konsep ini yang menjadi kunci pentingnya penggunaan <em>hash function </em>untuk menjamin keaslian dari sebuah data elektronik.
Sedikit berbeda dengan <em>hash function </em>tradisional,<em> fuzzy hash </em>“memotong-motong” bagian dari sebuah dokumen elektronik, dan kemudian menggunakan teknik <em>hash </em>tradisional untuk menghitung <em>hash values </em>dari masing-masing potongan untuk kemudian dibandingkan.
Bagi praktisi forensik digital yang berfokus di bidang <em>malware analysis, </em>konsep <em>fuzzy hash </em>tersebut sering digunakan untuk menemukan kekerabatan dari berbagai <em>malware</em>. <em>Malware</em> yang beredar di “alam liar”, dan memiliki <em>source code</em> yang serupa, sehingga dengan menggunakan <em>fuzzy hash </em>dapat dipetakan <em>malware </em> yang mirip dan berkerabat. Sedangkan, di bidang e-discovery, ternyata teknik fuzzy hash seringkali dipakai di kasus <em>copyright infringement.</em>

<strong>ssdeep</strong>
ssdeep merupakan implementasi dari konsep <em>fuzzy hash</em>. Untuk penggunaan dalam e-discovery, khususnya menemukan file-file yang memiliki kemiripan, dapat digunakan beberapa teknik ssdeep berikut:
1. Menghitung ssdeep hash.
untuk menghitung ssdeep hash, dapat digunakan perintah:
  `ssdeep -b nama_file`
  atau apabila hasil hash tersebut akan disimpan ke dalam sebuah file:
  `ssdeep -b nama_file > hash.txt`

2. Membandingkan hash yang disimpan dalam file dengan:
  `ssdeep -b -m hash.txt nama_file`

3. Apabila anda pemalas kelas kakap, dapat dilakukan perbandingan langsung seluruh folder secara <em>recursive </em>dengan
`ssdeep -l -r -p nama_directory`

flags:
  -b:  tidak perlu tampilkan path
  -m: tandingkan dengan hash
  -l:  gunakan path relative
  -r:  recursive
  -p: tampilkan semua yang sama

<strong>kekurangan</strong>

Selain itu presentasi kemiripan kedua belah file seringkali tidak mencerminkan kecocokan yang tepat, hal ini dikarenakan sehingga terdapat kemungkinan terdapat potongan yang memiliki <em>hash </em>yang sama, walaupun sesungguhnya kedua file asal <em>hash </em>tersebut tidak berkerabat dekat. Contohnya adalah <em>header </em>dari sebuah <em>filetype.</em>

<strong>to do list</strong>

Dikarenakan cara penyimpanan data yang berbeda pada file-file media (gambar, film), terdapat beberapa kemungkinan pengembangan yang dapat dibahas di masa depan, antara lain menggunakan ssdeep untuk mencari file gambar yang mirip, ataupun media lain seperti film atau lagu.
