---
title: increasing digital forensic effectiveness with data mining techniques [ID]
date: 2015-12-22 02:55:00 Z
tags:
- DFIR
- eDiscovery
layout: default
---

---
layout: post
title: meningkatkan Efektifitas Dalam Proses Analisis Digital Forensik (Melalui Penggunaan Tehnik Data Mining)
date: 2015-12-22 14:37
author: admin
comments: true
categories: [data science, forensic, Image, text clustering]
---
<strong>I. Latar Belakang</strong>
Digital Forensik, secara sederhana adalah sebuah proses mengumpulkan, menganalisis, serta menyajikan bukti digital yang dilakukan secara ilmiah dan terstandar (sehingga dapat dipertanggungjawabkan di muka pengadilan). Sesuai dengan definisi tersebut, Digital Forensic melibatkan tiga buah proses besar yang saling berkaitan, yaitu : kegiatan mengumpulkan (akuisisi) bukti digital, kegiatan menganalisis bukti digital, dan kegiatan menyajikan bukti digital.
Kegiatan menganalisis sendiri terdiri dari beberapa metode, dengan beberapa metode yang sering digunakan adalah analisis berbagai file log, file carving, timeline analysis, dan text string search. Text string search sendiri memegang peranan penting dalam proses analisis, hal ini disebabkan besarnya data digital yang berbentuk text, dan data digital berbentuk text seringkali memiliki nilai yang tinggi dalam proses investigasi. Data digital berbentuk text dapat berbentuk email, text messages, instant messages, Microsoft Office files, browsing history, bahkan berbagai jenis log.
Pertumbuhan teknologi di dunia penyimpanan data dan meningkatnya computing power secara drastis menyajikan tantangan baru dalam proses investigasi digital forensic. Kapasitas hard drive yang umum ditemui 5 tahun lalu biasanya berkisar antara 160-320 Gigabyte, dengan semakin murahnya harga media penyimpan, dalam sebuah investigasi digital forensik sekarang, tidak jarang menemui hard drive berkapasitas 1 Terabyte, bahkan dalam 1-2 tahun kedepan hard drive 2 Terabyte akan menjadi hal yang umum.
Semakin terjangkaunya harga media penyimpan, akan mengakibatkan semakin tingginya noise dalam proses file text searching, sehingga tidak jarang investigator digital forensik harus dengan teliti menyaring ribuan hasil search hit sebuah keyword tertentu yang sebagian besar tidak berhubungan dengan investigasi. Sebuah pencarian dengan keyword “faktur” dengan harapan untuk menemukan pembicaraan email Wajib Pajak mengenai “faktur pajak fiktif” dapat menghasilkan ratusan file mengenai penawaran pelatihan faktur pajak, peraturan tentang faktur pajak dan banyak false hit lain.
Secara umum terdapat dua jenis solusi untuk mengatasi tingginya noise/false hit dalam text string search: (1) mengurangi hasil pencarian yang ditampilkan (2) menyajikan hasil pencarian dengan cara yang memudahkan investigator digital forensik untuk menemukan hasil yang relevan. Menurunkan hasil pencarian yang ditampilkan dapat dilakukan dengan membatasi jenis, ukuran, created date hasil pencarian yang ditampilkan melalui filtering, metode ini dapat membantu mengurangi noise, namun dapat pula menyebabkan investigator melewatkan bukti digital yang penting. Sebaliknya, solusi kedua, tidak mengurangi hasil pencarian yang ditampilkan, melainkan menampilkannya dengan cara yang memudahkan investigator dalam memilih hasil pencarian yang relevan.

<strong>II. Text Mining</strong>
Text mining, secara sederhana, adalah sebuah proses menyarikan informasi yang berarti dari sebuah kumpulan text. Beberapa metode pengolahan data yang termasuk dalam metode text mining adalah content summarization, visualization, text classification dan text clustering.
Content summarization adalah proses menyarikan informasi dari sebuah kumpulan data/text, sedangkan visualization adalah sebuah proses merepresentasikan data/text ke dalam bentuk grafis. Kedua proses ini jatuh ke dalam kategori (1) bahasan pada bagian sebelumnya, karena terdapat beberapa informasi yang hilang/ tidak ditampilkan. Apabila kedua metode text mining ini digunakan dalam sebuah proses text mining biasa, tidak tampilnya sebagian (kecil) data mungkin tidak menjadi masalah. Namun apabila digunakan dalam sebuah proses investigasi digital forensik, hal tersebut dapat berarti kehilangan petunjuk yang berarti dalam proses investigasi.
Sebaliknya text classification dan text clustering tidak mengurangi hasil pencarian yang ditampilkan, melainkan menyajikannya dengan mengelompokkannya menjadi bagian-bagian. Perbedaan keduanya adalah text classification memerlukan kriteria-kriteria sebagai batasan bagi masing-masing kelompok (training set), sedangkan text clustering mengelompokkan dengan menggunakan algoritma/metode statistik tertentu sehingga satu kelompok memiliki kesamaan terdekat dengan unit-unit di dalam kelompoknya, dibandingkan dengan kelompok lain.
Text clustering tidak menghilangkan/menyembunyikan hasil pencarian, namun menampilkannya dengan kelompok-kelompok yang paling memiliki kesamaan (cluster). Pada gambar 1, terlihat pencarian dengan keyword “jokowi” menghasilkan 7.440.000 hasil pencarian. Namun text clustering memberikan kita sarana untuk memilih cluster/kelompok yang berisi keyword “jokowi” dalam cluster “Presiden Indonesia”, dan bukan cluster “Gubernur Jakarta” ataupun cluster “film berjudul jokowi yang disutradarai Teuku Rifnu Wikana”.
<p style="text-align: center;"><a href="http://aldosimon.com/blog/wp-content/uploads//2017/04/categorization-1024x575-min.png"><img class="alignnone wp-image-216 size-medium" src="http://aldosimon.com/blog/wp-content/uploads//2017/04/categorization-1024x575-min.png" alt="" width="300" height="150" /></a>
<em>Gambar 1. text categorization dengan kata “jokowi” sebagai keyword</em></p>
<strong>III. Permasalahan yang Dihadapi</strong>
Data hasil akuisisi dalam investigasi digital forensik sebagian besar tidak dalam bentuk yang terstruktur, melainkan terbagi-bagi dalam berbagai macam jenis file, mulai dari yang umum dan mudah di baca seperti plain text, Microsoft Office files, sampai format yang hanya dipakai beberapa pihak tertentu dan sulit untuk dibaca seperti database dalam sebuah aplikasi pembukuan yang di buat sendiri. Berbagai format tersebut mungkin tidak menjadi masalah apabila pengolahan dilakukan dengan menggunakan software digital forensik yang umum, hal ini dikarenakan software digital forensik memang dirancang untuk mengenali dan mampu meng-index berbagai tipe file. Sebaliknya aplikasi yang umum digunakan untuk text mining biasanya hanya menerima input berupa file database, comma separated value atau plain text.
No. Metode yang Digunakan Aplikasi yang Digunakan Jenis Aplikasi
<ol>
 	<li>Acquisition FTK Imager, dd, dc3dd Aplikasi digital forensik</li>
 	<li>Data Recovery FTK, Encase, scalpel, foremost Aplikasi digital forensik</li>
 	<li>Pre-processing Converter, Weka Aplikasi/Script konversi yang custom build, Aplikasi text mining</li>
 	<li>Analysis: Text Mining Methods Weka Aplikasi text mining</li>
 	<li>Analysis: Digital Forensic Methods Autopsy, FTK, Encase, various parser Aplikasi digital forensik</li>
</ol>
&nbsp;
<table border="1" width="613" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="38">
<p align="center"><b>No.</b></p>
</td>
<td valign="top" width="190">
<p align="center"><b>Metode yang Digunakan</b></p>
</td>
<td valign="top" width="215">
<p align="center"><b>Aplikasi yang Digunakan</b></p>
</td>
<td valign="top" width="170">
<p align="center"><b>Jenis Aplikasi</b></p>
</td>
</tr>
<tr>
<td valign="top" width="38">1.</td>
<td valign="top" width="190">Acquisition</td>
<td valign="top" width="215"><em id="__mceDel">FTK Imager, dd, dc3dd</em></td>
<td valign="top" width="170">Aplikasi digital forensik</td>
</tr>
<tr>
<td valign="top" width="38"> 2.</td>
<td valign="top" width="190">Data Recovery</td>
<td valign="top" width="215">FTK, Encase, scalpel, foremost</td>
<td valign="top" width="170">Aplikasi digital forensik</td>
</tr>
<tr>
<td valign="top" width="38"> 3.</td>
<td valign="top" width="190">Pre-processing</td>
<td valign="top" width="215">Converter, Weka</td>
<td valign="top" width="170">Aplikasi/<i>Script</i> konversi yang <i>custom build</i>, Aplikasi <i>text mining</i></td>
</tr>
<tr>
<td valign="top" width="38"> 4.</td>
<td valign="top" width="190">Analysis: Text Mining Methods</td>
<td valign="top" width="215">Weka</td>
<td valign="top" width="170">Aplikasi <i>text mining</i></td>
</tr>
<tr>
<td valign="top" width="38"> 5.</td>
<td valign="top" width="190">Analysis: Digital Forensic Methods</td>
<td valign="top" width="215">Autopsy, FTK, Encase, <i>various parser</i></td>
<td valign="top" width="170">Aplikasi digital forensik</td>
</tr>
</tbody>
</table>
&nbsp;
<p style="text-align: center;"><em id="__mceDel"><em id="__mceDel"><em id="__mceDel"><em id="__mceDel">Tabel 1. bermacam aplikasi yang digunakan
</em></em></em></em></p>
Tabel 1 diatas memperlihatkan perbedaan input yang mampu diterima oleh masing-masing jenis aplikasi menyebabkan proses investigasi digital forensik harus dilakukan secara bertingkat dan untuk menghubungkan kedua aplikasi yang berbeda tersebut harus menggunakan sebuah aplikasi/script konversi yang dibangun sesuai kebutuhan pada tiap investigasi.

<em id="__mceDel"><em id="__mceDel"><em id="__mceDel"><em id="__mceDel">
</em></em></em></em><strong>IV. Solusi yang Ditawarkan</strong>

Hambatan utama penerapan text mining adalah rumitnya implementasi text mining dalam sebuah investigasi digital forensik, seperti telah digambarkan dalam bagian sebelumnya. Perlunya aplikasi data mining tersendiri yang terpisah dari aplikasi digital forensik, selain itu harus dilakukan preprocessing data sebelum dilakukan analisis text mining. Beberapa solusi yang dapat digunakan untuk mengatasi kendala tersebut adalah sebagai berikut:

1. Berbagi Pakai

Untuk tiap jenis data yang tidak mampu ditangani software text mining perlu dilakukan konversi, sehingga seorang digital investigator perlu membuat berbagai macam aplikasi/script konversi atau melakukan preprocessing secara manual. Permasalahan ini bukan merupakan hal yang baru, berbagai macam kegiatan parsing baik berupa log, network capture dan kegiatan analisis atas file lain yang perlu terlebih dahulu di konversi dahulu juga menimbulkan permasalahan yang serupa. Sehingga atas problem tersebut dapat digunakan solusi yang serupa, yaitu berbagi pakai script/aplikasi. Encase menyediakan fasilitas script yang dapat dengan mudah dibagi-pakai, sedangkan add on dari autopsy jumlahnya semakin banyak dan semakin populer. Besar kemungkinan adopsi text mining dalam investigasi digital forensic akan lebih efektif melalui sarana berbagi-pakai tersebut.

<em id="__mceDel">2. </em>Penggunaan terbatas text mining

Text mining menjadi sangat efektif apabila diaplikasikan pada kumpulan data berbentuk teks. Dengan melakukan analisis text mining pada sebuah file backup outlook, Investigator digital forensic akan memperoleh petunjuk lebih banyak daripada melakukan analisis menggunakan metode text mining terhadap sebuah database pembukuan atau kumpulan gambar.
Dengan melakukan pemilahan antara data yang sangat memerlukan text mining dengan data yang dapat dianalisis secara manual, maka kebutuhan untuk melakukan preprocessing dapat ditekan.

<strong>V. Kesimpulan</strong>

Sebagian besar aplikasi pengolahan digital forensik belum memiliki fitur yang menggunakan metode Text mining. Penggunaan text mining dalam investigasi digital forensik juga terbilang sedikit. Kedua hal ini menyebabkan implementasi text mining dalam digital forensik menjadi cukup rumit. Untuk mengatasi hal tersebut dapat digunakan beberapa solusi yang ditawarkan penulis diatas.
Meskipun terdapat banyak tantangan, kedepannya Penggunaan text mining dalam digital forensik sudah tidak dapat dihindari lagi. Dengan semakin bertambahnya jumlah data, Pencarian dengan index search akan semakin menghasilkan banyak noise sehingga menurunkan efektifitas investigasi digital forensik.

[1] Smita M. Nirkhi, Dr. R.V. Dharaskar, Dr. V. M. Thakre, “Data Mining: A Prospective Approach for Digital Forensic.
[2] Nicole Lang Beebe, Jan Guynes Clark, “Digital forensic text string searching: Improving information retrieval effectiveness by thematically clustering search results”.
[3] Chrysoula Tsochataridou, Avi Arampatzis, Vasilios Katos, “Improving Digital Forensics Through Data Mining”.
