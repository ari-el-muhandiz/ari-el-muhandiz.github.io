---
layout: post
title: "Node JS, Javascript Server"
review: Di dunia pengembangan web, javascript banyak dipakai untuk membuat website agar lebih menarik dan interaktif. Ketika sebuah link  diklik maka akan muncul popup, ketika button di klik akan berubah warna, semua contoh diatas bisa terwujud karena javascript. Kemudian dikenalkan AJAX (Asyncronous Javascript And XML), yaitu sebuah teknologi dimana client bisa berhubungan dengan server tanpa melakukan reload halaman. Dengan hadirnya teknologi ini, era teknologi website berubah menjadi semakin dinamis...
date: 2016-04-29
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/ztspvPYybIY" frameborder="0" allowfullscreen></iframe>
<sub>*Ryan Dahl mempresentasikan Node JS pada acara JSConf 2009*</sub>

<p class='with-indent' markdown='1'>
	 Di dunia pengembangan web, javascript banyak dipakai untuk membuat website agar lebih menarik dan interaktif. Ketika sebuah link  diklik maka akan muncul popup, ketika button di klik akan berubah warna, semua contoh diatas bisa terwujud karena javascript. Kemudian dikenalkan AJAX (Asyncronous Javascript And XML), yaitu sebuah teknologi dimana client bisa berhubungan dengan server tanpa melakukan reload halaman. Dengan hadirnya teknologi ini, era teknologi website berubah menjadi semakin dinamis. Kemudian Google meluncurkan gmail dengan mendukung teknologi ajax, dan ini merupakan salah satu website pertama yang keseluruhan request dari client ke server, menggunakan teknologi ini (full ajax request). Akan tetapi revolusi teknologi ini hanya berada disisi client, hingga akhirnya di tahun 2009, Ryan Dahl memperkenalkan Node JS.
</p>

<p class='with-indent' markdown='1'>
	Node JS adalah javascript server yang berjalan diatas google V8, yaitu sebuah mesin javascript yang sangat cepat dan efisien yang digunakan di browser Google Chrome. Ide awal munculnya Node JS, Ryan Dahl menginginkan proses IO(Input/Output) pada komputer harus dilakukan dengan cara yang berbeda. IO adalah proses terlama diantara operasi-operasi pada sebuah komputer. Mengakses memori dilakukan dalam waktu 10e-9 detik(nanoseconds) dan mengakses data pada disk atau pada jaringan dilakukan dalam waktu 10e-3 detik(miliseconds). Pada umumnya ketika proses IO sedang berlangsung akan terjadi waktu tunggu antara request ke IO hingga proses tersebut selesai, hal ini dikenal dengan istilah blocking IO. Dan kebanyakan bahasa pemrograman akan menunggu dan tidak melakukan apapun hingga proses tersebut selesai, baru kemudian akan melakukan eksekusi proses selanjutnya. Misal kita melakukan query ke database: `var db = db.query("Select * From T");`, proses ini tidak akan menjadi masalah jika memakan waktu sekian detik, akan tetapi jika proses tersebut memakan waktu bermenit-menit hingga berjam-jam karena besarnya data atau kompleksnya query ke database tersebut, maka ini akan menjadi masalah. Bayangkan sebuah server yang setiap hari diakses berjuta-juta orang, pasti pengguna aplikasi tersebut dibuat jengkel karena sering terjadi timeout akses ke server, aplikasi dalam kondisi hang dan spinner terus berputar disertai tulisan "mohon tunggu, kami sedang mengakses data ke server". Oleh karena itu solusinya, proses IO tidak boleh kita hentikan atau yang dikenal dengan istilah nonblocking IO, dan Node JS merupakan sistem yang didesain untuk mengatasi masalah ini.
</p>

<p class='with-indent' markdown='1'>
	Jika kita bandingkan antara blocking dan nonblocking IO di dunia nyata, mungkin kita bisa analogikan seperti proses memesan makanan di meja resepsionis. Untuk blocking IO, ketika di meja resepsionis, proses pemesanan, pembayaran, dan penyajian dilakukan disitu, pembeli akan diminta menuggu hingga pesanannya selesai. Bayangkan jika sedang rame2nya, pasti antriannya akan panjang bukan. Hal ini berbeda dengan nonblocking IO, pada nonblocking IO receptionist akan menerima pesanan, mungkin juga terjadi pembayaran, kemudian kita akan diberikan sebuah tanda identitas dan kita bisa menunggu di meja, kemudian pelayan akan mengantar pesanan kita. Hal ini bisa mengurangi antrian, karena pelangggan tidak harus menunggu waktu lama proses memasak di meja resepsionis, dia tinggal menunggu di meja yang disediakan dan bisa melakukan aktifitas yang lain.
</p>

<p class='with-indent' markdown='1'>
	Inilah alasan, kenapa IO harus dilakukan secara nonblocking. Ditulisan saya selanjutnya, akan membahas lebih mendalam, mengenai arsitektur node JS dan bagaimana node JS menghandle request secara nonblocking.
</p>

