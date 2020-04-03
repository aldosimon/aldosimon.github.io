---
layout: post
title: web scrapping lycamobile.ie
date: 2017-04-06 09:33
author: admin
comments: true
categories: [internet, python, python, scrapping, script]
---
<p style="text-align: justify;">Lycamobile adalah salah satu provider selular di Eropa.  Harga yang ditawarkan lycamobile cukup yahud, tapi seperti semua barang murah lainnya, lycamobile punya segudang kekurangan. Jaringan yang hanya 3G, sampai coverage yang tidak se-canggih vodafone.</p>
<p style="text-align: justify;">Salah satu kekurangan lycamobile yang bikin geregetan adalah, si provider kadang suka 'lupa' memberitahu kalau paket sudah hampir habis, terutama paket gratisan yang di-dapat setelah isi ulang (Ya! isi ulang juga dapat gratisan, 2Gb selama sebulan, canggih kan!). Lycamobile juga tidak punya aplikasi untuk memantau berapa sisa paket data gratisan kita, satu-satunya cara adalah login ke website lycamobile untuk melihat sisa paket data kita. Ini yang seringkali terlupa, sehingga akhirnya si provider memakai saldo uang kita untuk akses data.</p>
<p style="text-align: justify;">Web scrapping, sederhananya adalah cara meng-ekstrak data 'tidak terstruktur' dari website. Contohnya, ingin tahu berapa banyak berita di sebuah website yang membahas tentang politik? dengan web scrapping kita bisa mendownload semua artikel dalam bentuk yang terstruktur untuk kemudian di analisis.</p>
<p style="text-align: justify;">Ceritanya dengan web scrapping kita akan login ke website lycamobile, untuk mengecek saldonya, dan memberi tahu kalau saldonya kurang dari jumlah tertentu (atau periode gratisan hampir habis).</p>
<p style="text-align: justify;"><strong>Library yang digunakan</strong></p>
<p style="text-align: justify;">Python punya banyak library untuk melakukan suatu fungsi. Untuk yang satu ini saya menggunakan beautifulsoup, karena setelah coba library lain, lebih banyak errornya dari kode yang ditulis :roll: Selain itu kita juga perlu library pendukung lainnya.</p>

<pre>import requests
from bs4 import BeautifulSoup
import re</pre>
<p style="text-align: justify;">Setelah memastikan bahwa semua library itu ada, kita bisa pindah ke langkah selanjutnya.</p>
<strong>Login</strong>
<p style="text-align: justify;">Untuk mendownload halaman yang memuat saldo paket data, kita terlebih dahulu harus melakukan login ke website lycamobile, halaman loginnya bisa dipantau di:</p>

<pre>https://account.lycamobile.ie/login.aspx?lang=en</pre>
<p style="text-align: justify;">Selain itu kita juga perlu tahu field apa saja yang digunakan untuk login. Right-click, inspect di field username/password, setelah itu kita bisa kira-kira field mana saja yang digunakan untuk melakukan login.</p>

<pre><input id="ctl00_cphPro_txtMobileNumber" class="frm-txtbox" tabindex="1" autocomplete="off" maxlength="10" name="ctl00$cphPro$txtMobileNumber" type="text" placeholder="(ex:089-9XXXXXX)" />

<input id="ctl00_cphPro_txtPassword" class="frm-txtbox" tabindex="2" autocomplete="off" maxlength="15" name="ctl00$cphPro$txtPassword" type="password" /></pre>
<p style="text-align: justify;">Kita bisa lihat ada dua field yang dipakai untuk login, namanya "ctl00$cphPro$txtMobileNumber" dan "ctl00$cphPro$txtPassword". Dua field ini akan kita digunakan oleh script kita untuk login:</p>

<pre>url = 'https://account.lycamobile.ie/Login.aspx?lang=EN'
url_g = 'https://account.lycamobile.ie/Account/AccountInfo.aspx?lang=en' 

body = {'ctl00$cphPro$txtMobileNumber' : 'mycellnumberofcourse',
          'ctl00$cphPro$txtPassword' : 'nowayjose',
          'ctl00$cphPro$btnLogin' : 'Log in',
          'ctl00$cphPro$txtMobileNumber_MDevice':'',
          'ctl00$cphPro$txtPassword_MDevice': ''}
</pre>
<p style="text-align: justify;">Url yang pertama kita peroleh sebelumnya, sedangkan url yang kedua adalah halaman yang akan kita download setelah login, yaitu halaman yang mencantumkan sisa paket data kita. Bila diperhatikan, selain dua field yang disebutkan diatas, ada beberapa field tambahan lain, yang seharusnya tidak berpengaruh besar. Field lain ini bisa didapat dengan cara yang sama.</p>
<p style="text-align: justify;">Selain dua field yang terlihat jelas tadi, biasanya sebuah website modern punya beberapa field tersembunyi. Biasanya untuk menyimpan <a href="https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet">CSRF token</a> atau value lain yang sekiranya diperlukan. Untuk mendeteksi value lain yang tersembunyi, dan kemudian melakukan login, kita gunakan fungsi berikut</p>

<pre>def login(body, url, url_g):
    """
    login to website using parameters
    """
    s = requests.Session()
    loginPage = s.get(url)
    soup = BeautifulSoup(loginPage.text, "lxml")
    hiddenInputs = soup.findAll(name = 'input', type = 'hidden')
    
    for hidden in hiddenInputs:
        try:
            name = hidden['name']
            value = hidden['value']    
        except KeyError:
            name = ''
            value = ''
        body[name] = value

    page = s.post(url, data = body)
    page = s.get(url_g)
    return page
</pre>
<p style="text-align: justify;">Pertama-tama kita mendownload halaman loginnya, lalu dengan menggunakan beautiful soup kita bisa memisahkan element yang punya nama 'input' (name='input') dengan tipe tersembunyi (type='hidden'). Beautiful soup mengembalikan sebuah list dengan elemen-elemen tersebut. Tinggal menambahkannya ke dalam list/dictionary body yang kita siapkan sebelumnya (body[name] = value).</p>
<strong>Let the scrapping begin</strong>
<p style="text-align: justify;">Setelah fungsi login, kini kita beralih ke fungsi utama, mengambil data yang diperlukan dengan fungsi scrap dibawah</p>

<pre>def scrap(tag, cls, val, type):
    """
    scrap data from page, using chosen tag, class, class value and type
    type availavle are number and date
    """
    page = login(body, url, url_g)
    soup = BeautifulSoup(page.content, 'html.parser')
    name_box = soup.find(tag, attrs={cls:val})
    name = name_box.text.strip() # strip() is used to remove starting and trailing
    if type == 'number':
        name = re.findall('\d+\.\d+', name)
        name = name
    elif type == 'date':
        name = re.findall('\d+\/\d+\/\d+', name)
        name = name[0]
    return name 
</pre>
<p style="text-align: justify;">Fungsi scrap menerima empat parameter. 'tag', 'cls', dan 'val' digunakan untuk memisahkan tag html tertentu dan kemudian 'type' digunakan untuk membersihkan data yang sudah diambil. Fungsi scrap kemudian kita panggil dengan parameter yang diperlukan. Sebagai contoh, berikut potongan laman dan kode dari website lycamobile yang memuat sisa paket data dan periode akftif.</p>
<img src="http://aldosimon.com/blog/wp-content/uploads//2017/04/Screenshot-from-2017-04-06-100820.png" alt="" width="568" height="57" border="1" />
<pre>
<tr class="tbl-data">
<td data-title="DATA">
1760.87MB
</td>
<td data-title="FREE DATA EXPIRY DATE">
01/05/2017
</td>
</tr>
</pre>
<p style="text-align: justify;">kita bisa lihat tag, class dan value dari class yang digunakan, dan kemudian tinggal memanggil fungsi scrap dengan nilai-nilai tersebut.</p>

<pre>period = scrap('td', 'data-title', 'FREE DATA EXPIRY DATE', 'date')
sisa = scrap('td','data-title', 'DATA', 'number')
</pre>
Tinggal menjalankan script itu secara otomatis, misalnya dengan menggunakan cronjob dua minggu sekali, dan tiap kali sisa paket data/periode mendekati status kritis, kirimkan pemberitahuan. Saya sendiri menggunakan telegram untuk melakukan pemberitahuan.
<img src="http://aldosimon.com/blog/wp-content/uploads//2017/04/jtbot_telegram-min.jpg" alt="" />

<span style="font-size: 10pt;">*dicomot dari berbagai script</span>
