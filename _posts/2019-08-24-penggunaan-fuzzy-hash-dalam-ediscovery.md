---
layout: post
title: penggunaan fuzzy hash dalam ediscovery
date: '2019-08-24 06:35'
author: admin
comments: true
categories:
  - ediscovery
  - forensic
  - ssdeep
published: true
---
<p style="text-align: justify;">Beberapa saat yang lalu, ketika sedang menunggu rekan-rekan praktisi forensik digital melakukan akuisisi pada beberapa perangkat komunikasi, mata penulis terpaku pada sebuah poster SANS forensic yang berisi teknik identifikasi <em>malware</em>. Dari beberapa teknik identifikasi tersebut, terdapat salah satu teknik untuk menghitung kesamaan dari dua buah <em>malware</em> yang berbeda. Penulis bertanya di dalam hati, mengapa teknik tersebut tidak digunakan untuk mencari dokumen elektronik yang memiliki kesamaan. Pertanyaan tersebut tidak menuai jawaban hari itu….karena penulis bertanya di dalam hati.</p>

<p style="text-align: justify;"><strong>pengantar</strong></p>
<p style="text-align: justify;">Sebagai praktisi forensik digital, terutama yang berfokus di e-discovery, kegiatan menemukan file sudah menjadi makanan sehari-hari. Namun adakalanya diperlukan juga bagi praktisi forensik digital untuk menemukan file yang serupa dengan file yang sudah ditemukan (file yang sudah di-<em>update, </em>di-<em>edit, </em>di-<em>save as­</em> dsb).  Fuzzy hash dapat digunakan sebagai salah satu alternative untuk menyelesaikan permasalahan tersebut.</p>
<p style="text-align: justify;"><strong>hash dan fuzzy hash</strong></p>
<p style="text-align: justify;"><em>Hash function </em>adalah fungsi matematis yang digunakan untuk “merangkum” data elektronik yang berjumlah berapa pun menjadi jumlah tetap yang disebut dengan <em>hash values</em>. Data elektronik yang berbeda kontennya, akan menghasilkan <em>hash value </em>yang berbeda. Konsep ini yang menjadi kunci pentingnya penggunaan <em>hash function </em>untuk menjamin keaslian dari sebuah data elektronik.</p>
<p style="text-align: justify;">Sedikit berbeda dengan <em>hash function </em>tradisional,<em> fuzzy hash </em>“memotong-motong” bagian dari sebuah dokumen elektronik, dan kemudian menggunakan teknik <em>hash </em>tradisional untuk menghitung <em>hash values </em>dari masing-masing potongan untuk kemudian dibandingkan.</p>
<p style="text-align: justify;">Bagi praktisi forensik digital yang berfokus di bidang <em>malware analysis, </em>konsep <em>fuzzy hash </em>tersebut sering digunakan untuk menemukan kekerabatan dari berbagai <em>malware</em>. <em>Malware</em> yang beredar di “alam liar”, dan memiliki <em>source code</em> yang serupa, sehingga dengan menggunakan <em>fuzzy hash </em>dapat dipetakan <em>malware </em> yang mirip dan berkerabat. Sedangkan, di bidang e-discovery, ternyata teknik fuzzy hash seringkali dipakai di kasus <em>copyright infringement.</em></p>
<p style="text-align: justify;"><strong>ssdeep</strong></p>
<p style="text-align: justify;">ssdeep merupakan implementasi dari konsep <em>fuzzy hash</em>. Untuk penggunaan dalam e-discovery, khususnya menemukan file-file yang memiliki kemiripan, dapat digunakan beberapa teknik ssdeep berikut:</p>
<p style="text-align: justify;">1. Menghitung ssdeep hash.</p>
untuk menghitung ssdeep hash, dapat digunakan perintah:

<code><em>ssdeep -b nama_file</em></code>

atau apabila hasil hash tersebut akan disimpan ke dalam sebuah file:

<code><em>ssdeep -b nama_file &gt; hash.txt</em></code>

2. Membandingkan hash yang disimpan dalam file dengan:

<code><em>ssdeep -b -m hash.txt nama_file</em></code>

3. Apabila anda pemalas kelas kakap, dapat dilakukan perbandingan langsung seluruh folder secara <em>recursive </em>dengan

<code><em>ssdeep -l -r -p nama_directory</em></code>

flags:
<span style="font-size: 8pt;">-b:  tidak perlu tampilkan <em>path
</em></span><span style="font-size: 8pt;">-m: tandingkan dengan <em>hash
</em></span><span style="font-size: 8pt;">-l:  gunakan <em>path relative
</em></span><span style="font-size: 8pt;">-r:  <em>recursive
</em></span><span style="font-size: 8pt;">-p: tampilkan semua yang sama</span>

<strong>kekurangan</strong>

Selain itu presentasi kemiripan kedua belah file seringkali tidak mencerminkan kecocokan yang tepat, hal ini dikarenakan sehingga terdapat kemungkinan terdapat potongan yang memiliki <em>hash </em>yang sama, walaupun sesungguhnya kedua file asal <em>hash </em>tersebut tidak berkerabat dekat. Contohnya adalah <em>header </em>dari sebuah <em>filetype.</em>

<strong>to do list</strong>

Dikarenakan cara penyimpanan data yang berbeda pada file-file media (gambar, film), terdapat beberapa kemungkinan pengembangan yang dapat dibahas di masa depan, antara lain menggunakan ssdeep untuk mencari file gambar yang mirip, ataupun media lain seperti film atau lagu.
