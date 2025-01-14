---
title: "Konfigurasi Database Server"
tags: ["Linux", "Sekolah", "Network"]
date: "2023-11-01"
summary: "Installasi, Konfigurasi, Pengetesan Samba Server"
descrition: "Installasi, Konfigurasi, Pembuatan Table MySql"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: false
---

# Database Server (MySql) Debian 10 

Haloo...

Diblog ini kali ini saya akan menjelaskan bagaimana caranya untuk menginstall sekalian proses konfigurasi database server menggunakan MariaDB/MySQL. Untuk prosesnya kita siapkan terlebih dahulu Debian 10-nya seperti biasa.

## Proses Instalasi Package MariaDB

Untuk pertama kali kita siapkan terlebih dahulu cd pada Debian 10 kita untuk proses instalasi package 

![pasangcd](/images/databasepost/pasangcd.PNG)

Setelah itu kita tambahkan list cd baru kepada package manager kita

`# apt-cdrom add`

Jika sudah terpasang cdnya kita update package manager kita untuk mengscan cd yang baru kita pasangkan. 

`# apt update`

Install package MariaDBnya jika sudah terupdate package manager kita

`# apt install mariadb-server`

## Setting Secure Instalation MariaDB

Setelah package MariaDB sudah terinstall, kita harus menyeting keamanan database server kita agar aman, seperti menambahkan password pada saat login. Untuk command penyetingan databasenya bisa lakukan seperti ini.

`# mysql_secure_instalation`

Disini kita akan diberikan beberapa prompt yang perlu kita isi. Yang pertama yang akan kita lihat adalah memasukan password. Berhubung kita masih pertama kali instal kita tidak perlu memasukan apa-apa.

![awalsecure](/images/databasepost/awalsecure.png)

Setelah itu kita masukkan perintah iya saja, dikarenakan kita ingin mengkonfigurasi untuk awal-awal. Untuk prompt pembuatan login password buat saja passwordnya

![awalsecure](/images/databasepost/awalsecure1.png)

![awalsecure](/images/databasepost/awalsecure2.png)

## Pembuatan Database, Table Baru

Untuk kita memasuki shell MariaDB kita, cukup masukan perintah.

`# mysql -u root -p`

Jika sudah, kita tinggal membuat database baru agar bisa membuat table pada MariaDB

`# create database namadatabase`

Setelah itu kita bisa langsung menggunakan database kita, nanti tampilan akan seperti ini.

`# use namadatabase`

![database](/images/databasepost/database.png)

Kita bisa membuat table baru untuk sekarang, untuk pembuatan table bisa bebas terserah kita. untuk syntaxnya seperti ini.

`# create table namatable (namakolom tipedata, namakolom tipedata);`

Untuk saya sendiri saya akan membuat table seperti ini. Untuk pengecekan database juga bisa dilakukan seperti ini.

`# describe namatable;`

![database1](/images/databasepost/database1.png)

Jiksa sudah seperti itu kita sudah selesai!

Kamu bisa mengeksplor dengan sendiri jika ingin mengetahui lebih dalam tentang database server. Saya menyarankan untuk melihat seperti channel youtube Programmer Zaman Now, dll untuk penjelasan database lebih lengka lagi.

Untuk itu, sekian dari saya. 
Terima Kasih sudah mampir ke blog saya. Have a nice day  ^.^
