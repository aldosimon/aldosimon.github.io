---
layout: post
published: true
title: forensik email melalui analisis header email
author: admin
comments: true
date: '2020-06-12 11:08'
categories:
  -email forensics
  -email forensik
  -email
---
email merupakan sebuah sarana komunikasi yang sangat digunakan banyak pihak. Penting bagi seorang investigator forensik digital untuk mengerti secara menyeluruh, bagaimana protokol email bekerja. Tulisan ini akan membahas sebagian dari forensik pada email, dengan berfokus pada header email.
<!--more-->
sebagai bagian pembahasan, kita akan membedah sebuah email dari paypal di bawah ini:
```bash
Delivered-To: [disembunyikan]@gmail.com
Received: by 10.229.84.10 with SMTP id h10cs128890qcl;
        Fri, 4 Dec 2009 05:31:43 -0800 (PST)
Received: by 10.150.87.2 with SMTP id k2mr5425632ybb.267.1259933502490;
        Fri, 04 Dec 2009 05:31:42 -0800 (PST)
Return-Path: <[disembunyikan]@info.paypal.com>
Received: from om-paypal-apac.rsys4.com (om-paypal-apac.rsys4.com [12.130.139.51])
        by mx.google.com with ESMTP id 22si6368786gxk.17.2009.12.04.05.31.39;
        Fri, 04 Dec 2009 05:31:41 -0800 (PST)
Received-SPF: pass (google.com: domain of [disembuyikan]@info.paypal.com designates 12.130.139.51 as permitted sender) client-ip=12.130.139.51;
Authentication-Results: mx.google.com; spf=pass (google.com: domain of [disembunyikan]@info.paypal.com designates 12.130.139.51 as permitted sender) smtp.mail=[disembunyikan]@info.paypal.com; dkim=pass header.i=[disembunyikan]@info.paypal.com
DKIM-Signature: v=1; a=rsa-sha1; c=relaxed/relaxed; s=responsys; d=info.paypal.com; h=MIME-Version:Content-Type:Date:From:Reply-To:Subject:List-Unsubscribe:To:Message-Id; i=[disembunyikan]@info.paypal.com; bh=gsI3Bb5slkuo+p/q6yjixbNU3mw=; b=D3rOkUdQ2clZdSo8DRNHL/dhCp2CWRmHpdF141GVzoULBmU04wArvKBaRKNsT0BN1fiMRCNXRJYm
   ypaEUzkvIonQoin9dHv25b1wbBZqURL203V4QVOIOGtaoe4AuZPh83X7lYwhh2nNco1j365UQZnq
   lOjmprcf9lOni+cxBq0=
DomainKey-Signature: a=rsa-sha1; c=nofws; q=dns; s=responsys; d=info.paypal.com; b=JyWv8SKVgNBckLxZClNYi2e4jB1O0vXN7+xXenYtgqxrhl7aTJB9Ccby1dTC5AMLnI6labChYrFK
   QLbtHPQNh7jBrXrCzp5lj5vTjXKv9eujU/3dkRNInnSzS127eebDWs0J/nCTwEXaVx6E8UrMh0Wp
   AK9CJR9afWN8hPNXS0c=;
Received: by om-paypal-apac.rsys4.com (PowerMTA(TM) v3.5r10) id h347jq0morc1 for <[disembunyikan]@gmail.com>; Fri, 4 Dec 2009 05:18:36 -0800 (envelope-from <[disembunyikan]@info.paypal.com>)
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="----msg_border"
Date: Fri, 4 Dec 2009 05:18:36 -0800
From: "PayPal" <[disembunyikan]@info.paypal.com>
Reply-To: "PayPal" <[disembunyikan]info.paypal.com>
Subject: <ADV> [disembunyikan], win over US$60K when you start shopping with PayPal
List-Unsubscribe: https://email0.paypal.com/servlet/optout?iisHiDVXYDUTELPHsKhlpthgFntHpsDJhtEf
X-cid: pplap.245.10
X-sgxh1: LOHtKhkpuhgxnuHptQJhu
To: [disembunyikan]@gmail.com
X-valueof-OFFERID: 20790
X-valueof-CAMPAIGNID: 5392
X-valueof-TREATMENTCODE: 000250461
X-valueof-EMAILCATEGORY: PPN
X-valueof-HASHID: 4G83453674863450B
Message-Id: <4B190C2C.00000A7C@om-paypal-apac.rsys4.com>
```
namun sebelum masuk ke pembahasan field pada email header, kita perlu mengetahui bahwa sebuah email memulai perjalanannya di Mail User Agent (MUA), yang merupakan client yang dapat berbicara dengan protokol tertentu (SMTP, POP3) kepada sebuah Mail Transfer Agent (MTA), yang berfungsi meneruskan email kepada MTA lain, atau langsung kepada MUA (dengan protokol IMAP). contoh MUA antara lain Thunderbird, Ms. Outlook. Sedangkan contoh sebuah MTA adalah Ms. Exchange, Sendmail, Postfix.

setelah memahami mengenai konsep dasar tersebut, kita akan memotong-motong header email di atas, serta membahasnya satu persatu.

#### field standar pada email header

```bash
Delivered-To: [disembunyikan]@gmail.com
Received: by 10.229.84.10 with SMTP id h10cs128890qcl;
        Fri, 4 Dec 2009 05:31:43 -0800 (PST)
Received: by 10.150.87.2 with SMTP id k2mr5425632ybb.267.1259933502490;
        Fri, 04 Dec 2009 05:31:42 -0800 (PST)
Return-Path: <[disembunyikan]@info.paypal.com>
Received: from om-paypal-apac.rsys4.com (om-paypal-apac.rsys4.com [12.130.139.51])
        by mx.google.com with ESMTP id 22si6368786gxk.17.2009.12.04.05.31.39;
        Fri, 04 Dec 2009 05:31:41 -0800 (PST)
--dipotong--
Received: by om-paypal-apac.rsys4.com (PowerMTA(TM) v3.5r10) id h347jq0morc1 for <[disembunyikan]@gmail.com>; Fri, 4 Dec 2009 05:18:36 -0800 (envelope-from <[disembunyikan]@info.paypal.com>)
```
terdapat beberapa field yang perlu diperhatikan pada sebuah header email. antara lain:
  1. Return Path: alamat email yang akan dikirimi pesan apabila email tidak bisa dikirim
  2. Delivery-date: tanggal pesan diterima
  3. Date: tanggal pesan dikirimkan
  4. Message-ID: nomor identifikasi sebuah email/pesan. Terdiri dari dua bagian [karakterrandom]@[namadomain]. Tidak terdapat format baku untuk menghasilkan Message-ID, sehingga tiap server dapat menghasilkan format yang berbeda.
  5. X-Mailer: jenis klien email/ pesan yang digunakan
  6. From: akun pengirim email
  7. To: akun tujuan/ penerima email
  8. Subject: subjek email

yang perlu diingat adalah field di atas dibaca dengan urutan dari bawah ke atas. sehingga informasi yang disampaikan dari field di atas adalah email dikirimkan dari domain om-paypal-apac.rsys4.com [12.130.139.51] dan diterima oleh mx.google.com dengan protokol ESMTP pada  04 Dec 2009 05:31:41 -0800 (PST). email ini selanjutnya diteruskan ke 10.150.87.2, dan terakhir ke 10.229.84.10.
perlu diperhatikan juga di bawah baris --dipotong-- kita bisa melihat pertama kali email tersebut dikirim oleh MUA kepada MTA, namun tidak tercantum nama klien yang digunakan. contoh di bawah memperlihatkan sebuah MUA yang mengirimkan kepada MTA, dengan nama client yang digunakan Exim 4.69. beberapa klien lain yang mungkin sering ditemui adalah Thunderbird, dan Ms. Outlook.

```bash
Received: from inibukuc by ns1.indolinux.net with local (Exim 4.69) (envelope-from <inibukuc@ns1.indolinux.net>) id 1MuUH0-0000qO-Kg; Sun, 04 Oct 2009 23:49:26 +0700
```

#### Sender Policy Framework (SPF)

```bash
Received-SPF: pass (google.com: domain of [disembuyikan]@info.paypal.com designates 12.130.139.51 as permitted sender) client-ip=12.130.139.51;
Authentication-Results: mx.google.com; spf=pass (google.com: domain of [disembunyikan]@info.paypal.com designates 12.130.139.51 as permitted sender) smtp.mail=[disembunyikan]@info.paypal.com;
```

Sender Policy Framework (SPF) adalah sebuah DNS record (TXT record) yang berisi informasi alamat IP  yang diperbolehkan mengirim  email atas nama domain tertentu. SPF berfungsi memberikan kepercayaan bahwa sebuah email yang dikirimkan dari IP tertentu memang berhak mengirimkan email tersebut. pada contoh di atas, terlihat bahwa MTA mx.google.com memverifikasi apakah IP address 12.130.139.51 (pengirim email) memang merupakan IP yang diperbolehkan mengirimkan email atas nama domain paypal.com. hasil verifikasi merupakan jawaban dari domain owner paypal.com yang menyatakan bahwa IP tersebut memang diperbolehkan mengirim email atas nama domain paypal.com, sehingga menghasilkan jawaban pass.

beberapa hasil jawaban dari query SPF antara lain:
  1. None berarti domain owner tidak menuliskan record SPF, sehingga tidak dapat dilakukan cek SPF.
  2. Pass berarti domain owner memperbolehkan IP tersebut mengirim pesan atas nama domain.
  3. Fail berarti record SPF pada domain tidak mencantumkan IP pengirim tersebut, sehingga IP tersebut tidak diperbolehkan mengirim email atas nama domain.
  4. SoftFail berarti domain owner tidak mencantumkan IP pengirim tersebut, namun email masih diperbolehkan untuk diterima namun ditandai sebagai spam.


#### DomainKeys Identified Mail (DKIM)
```bash
DKIM-Signature: v=1; a=rsa-sha1; c=relaxed/relaxed; s=responsys; d=info.paypal.com; h=MIME-Version:Content-Type:Date:From:Reply-To:Subject:List-Unsubscribe:To:Message-Id; i=[disembunyikan]@info.paypal.com; bh=gsI3Bb5slkuo+p/q6yjixbNU3mw=; b=D3rOkUdQ2clZdSo8DRNHL/dhCp2CWRmHpdF141GVzoULBmU04wArvKBaRKNsT0BN1fiMRCNXRJYm
   ypaEUzkvIonQoin9dHv25b1wbBZqURL203V4QVOIOGtaoe4AuZPh83X7lYwhh2nNco1j365UQZnq
   lOjmprcf9lOni+cxBq0=
```
DomainKeys Identified Mail (DKIM). DKIM adalah sebuah standar yang digunakan untuk membantu mengkonfirmasi integritas sebuah email/ pesan.

beberapa field terkait DKIM antara lain:
  1. v= versi DKIM yang digunakan. pada contoh di atas adalah versi 1.
  2. a= algoritma hash dan enkripsi yang digunakan. pada contoh di atas adalah rsa-sha1.
  3. c= jenis canonicalization yang digunakan. artinya bagaimana sebuah konten (pesan atau header pesan) harus diperlakukan sebelum di hash. secara jelas ada di RFC 6376 pada link. pada contoh di atas, header dan body nya di canonicalized dengan metode relaxed (karena field c-nya relaxed/relaxed)
  4. d= adalah nama domain yang mengklaim tanggung jawab atas email dimaksud. domain ini pula yang diquery untuk mendapatkan public key nya. pada contoh di atas adalah info.paypal.com
  5. s= adalah selector dari domain, mudahnya terdapat beberapa record dns milik domain (d), selector adalah penunjuk kita meminta record dns yang mana, agar mendapatkan public key. pada contoh di atas adalah responsys. anda bisa memverifikasi informasi pada vield ini dengan menggunakan berbagai aplikasi online (mxtoolbox contohnya) atau dengan menggunakan perintah dig pada linux, menggunakan field d (info.paypal.com) dan s (responsys).  DKIM key disimpan di bawah subdomain _domainkey, serta jenis dns record nya adalah TXT. sehingga perintah dig anda akan terlihat seperti:
  ```bash
  dig -t txt dig responsys._domainkey.info.paypal.com TXT
  ```
  6. h= adalah bagian dari header email yang dipilih dan nantinya ikut di encrypt dan hash untuk menjadi signature. pada contoh diatas MIME-Version:Content-Type:Date:From:Reply-To:Subject:List-Unsubscribe:To:Message-Id
  7. bh= body email yang sudah canonicalized (sesuai field c RFC 6376) di hash (sesuai dengan field a) dan kemudian disajikan dalam base64.
  8. b= signature dalam bentuk Base64 form.

secara sederhana, proses penggunaan DKIM dalam sebuah email  adalah:
  1. pemilik sebuah domain, pertama-tama membuat sebuah kombinasi public dan private key.
  2. setelah itu  public key tersebut di-publish pada DNS record domainnya.
  3. beberapa bagian email yang dikirimkan (hanya yang tercantum di field h) kemudian melalui proses canonicalization (sesuai jenis pada field c) dan dihash (sesuai jenis pada field a) dan hash tersebut di encrypt (sesuai jenis pada field a) dengan private key yang  dibuat pada langkah pertama. inilah yang menjadi signature dari email tersebut (field b)
  4. penerima pesan dapat memastikan integritas pesan dengan men-decrypt hash tersebut dengan public key yang tersedia, kemudian membandingkannya dengan menghitung hash dari email tersebut. apabila ada perbedaan, maka terdapat kemungkinan field-field yang disebutkan pada field h berubah.

kita dapat secara manual melakukan verifikasi DKIM dengan mengikuti langkah di atas (detail pada RFC), menggunakan aplikasi seperti dkimpy atau opendkim, dan juga menggunakan banyak aplikasi yang tersedia online. salah satu yang paling populer adalah [mxtoolbox](https://mxtoolbox.com/Public/Tools/EmailHeaders.aspx?).

#### x-headers

apabila diperhatikan, pada email di atas terdapat beberapa header yang berawalan "x-". x-headers adalah headers custom yang memiliki standar yang beragam. digunakan untuk berbagai hal mulai dari melacak user id, atau advertising id dari penerima email, karena minimnya standar dari x-headers, terdapat begitu banyak jenisnya.

#### verifikasi dengan dkimpy
melakukan verifikasi manual akan cukup memakan waktu, kita bisa juga menulis script sendiri dengan memperhatikan beberapa RFC terkait DKIM. apabila memutuskan untuk menggunakan aplikasi, maka dkimpy bisa di eksekusi sebagai berikut:
  1. installasi (apabila belum memiliki pip, silahkan install terlebih dahulu)
  ```bash
  pip install dkimpy
  ```
  2. baca manual
  ```bash
  man dkimverify
  ```
  3. verifikasi
  ```bash
  cat fileemail.eml | dkimverify
  ```

berikut hasil verifikasi (email yang digunakan di bawah berbeda dengan email paypal di atas):
  1. DKIM signature email
  ![email](/images/dkim.png)
  2. body email asli
  ![email](/images/email_asli.png)
  3. body email kedua (email asli yang ditambahkan "!" pada body)
  ![edited email](/images/email_edit.png)
  4. hasil verifikasi kedua email
  ![edited email](/images/verifikasi_dkim.png)

terlihat bahwa ketika isi email berubah sedikit saja (tambahan simbol "!"), maka verifikasi akan gagal. silahkan mencoba verifikasi email anda sendiri, dengan menggunakan mxtoolbox.

#### penutup: pertimbangan saat akuisisi
berdasarkan penjelasan tersebut, maka ketika melakukan akuisisi email, harus menyadari bahwa header email menyimpan banyak field yang dapat menjamin otentitas dari beberapa hal, khususnya field yang turut dihitung dalam DKIM signature, sehingga header email menjadi sangat penting untuk turut di-akuisisi.

memperhatikan lagi field yang tersedia, maka yang bisa dijamin oleh DKIM signature adalah beberapa field yang dicantumkan pada field h, termasuk field bh, yang merupakan hash dari body email. perlu diingat bahwa field-field yang tidak turut masuk dalam DKIM signature tetap menjadi petunjuk dalam melakukan analisis.


* [13cubed's youtube](https://www.youtube.com/watch?v=nK5QpGSBR8c)
* [metaspike](https://www.metaspike.com/leveraging-dkim-email-forensics/)
* [arclab](https://www.arclab.com/en/kb/email/how-to-read-and-analyze-the-email-header-fields-spf-dkim.html)
* [redsift](http://knowledge.ondmarc.redsift.com/en/articles/1148885-spf-hard-fail-vs-spf-soft-fail)
* [stackoverflow](https://stackoverflow.com/questions/48762829/proper-way-to-validate-dkim-signature-b-part)
* [stackoverflow](https://stackoverflow.com/questions/36047831/generating-dkim-signatures-via-python-for-custom-mta)
* [rfc6376](https://tools.ietf.org/html/rfc6376)
