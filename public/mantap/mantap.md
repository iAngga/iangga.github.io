---
title: "blablabasb"
tags: ["Linux", "Network"]
date: "2024-11-12"
summary: "Installasi, Konfigurasi, Pengetesan Samba Server"
description: "Installasi, Konfigurasi, Pengetesan Samba Server"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: false
---

# Samba Server Debian 10
Haloo..

Kembali lagi sekarang dipostingan blog saya yang baru. Kali ini saya akan menjelasakan bagaimana cara mengkonfigurasi Samba Server di Debian 10. 

Sebelum mulai apa-apa siapkan Debian 10-nya yang sudah terinstall, sesudah itu ini untuk langkah-langkahnya.

## Proses Instalasi Package Samba 
Untuk yang pertama kita siapkan dulu cd Debian 10, dan memasukannya kedalam Debian 10 virtual machine kita.

![pasangcd](images/mantap.png#light)
![pasangcd](images/mantap.png#dark)

Sudah terpasang cdnya, kita masukan command.

`# apt-cdrom add`

Setelah itu kita bisa menuliskan command ini.

`# apt update`

Jika sudah, kita langsung bisa menginstall package Sambanya.

`# apt install samba`

Pada saat loading penginstallan, nanti kita akan diberikan prompt untuk **"samba server and utilities"**, kita pilih **no**.
![pilihno](/static/post/sambas/gambar2.PNG)

## Setting IP Untuk Debian

Jika proses penginstallan sudah beres, kita bisa lanjut untuk memberikan IP Address kepada mesin Debian kita. Untuk caranya kita edit file `/etc/network/interfaces` dengan teks editor sesuai pilihan, disini saya akan menggunakan teks editor **Vim**.

![settingip](post/sambas/gambar3.PNG)

Jika sudah tersetting IPnya kita restart saja servicenya.

`# /etc/init.d/networking restart`

## Konfigurasi Samba
Sebelum kita mengkonfigurasi Sambanya, kita buat dulu directory untuk digunakan pada sharing file dengan client Windowsnya. Disini, saya akan membuat directory didalam `/home` directory.

`# mkdir /home/sharing`

Setelah itu kita berikan perizinan pada directory yang baru kita buat agar bisa dishare untuk **write, read, dan lainnya**. Disini saya akan berikan izin semuanya.

`# chmod 777 /home/sharing`

Jika sudah, kita bisa lanjut untuk konfigurasinya. Pertama kita pakai teks editor kita lagi untuk mengedit file `/etc/samba/smb.conf`

`# vim /etc/samba/smb.conf`

Jika sudah masuk filenya kita bisa lanjut untuk menambahkan konfigurasinya. Dipaling bawah kita tambahkan ini

![konfigurasi1](/static/post/sambas/gambar4.PNG)

Juga berikan komentar dengan menggunakan `#` di bagian ini. dimulai dari `[homes]` sampai `valid users`

![konfigurasi2](/static/post/sambas/gambar5.PNG)

Sudah selesai dengan konfigurasinya, tinggal save filenya dan restart service Sambanya.

`# /etc/init.d/samba-ad-dc restart`

Buat juga user baru untuk kita login dalam Sambanya, dikarenakan kita sudah menggunakan `guest ok = no`. Pertama kita buat usernya dan memberikannya password.

`# useradd namakita`

`# passwd namakita`

Setelah itu, kita buat user ini agar bisa login untuk prompt login jika ingin membuka folder di Windows.

`# smbpasswd -a namakita`

![username](/static/post/sambas/gambar6.PNG)

## Pengetesan Di Client Windows
Selesai sudah pengkonfigurasian Samba di Debiannya, kita mulai pengetesannya.

Untuk pertama kita setting IP adapter `Host-only` kita dan samakan networknya dengan yang Debian.

![ipwindows](/static/post/sambas/gambar7.PNG)

Jika sudah kita tinggal tes dengan menggunakan aplikasi `Run` dengan menekan `Windows+R` di keyboard dan isikan IP Debian kita.

![run](/static/post/sambas/gambar8.PNG)

Jika ada folder yang kita namakan pada konfigurasi Debiannya, tandanya itu sudah bisa sharing file antar Windows dengan Debian hanya dengan menggunakan Windows Explorer. Jangan lupa jika kamu masuk kedalam foldernya nanti ada prompt untuk memasukan password, disana tinggal masukan saja. 

![sharekeun](/static/post/sambas/gambar9.PNG)

![passwd](/static/post/sambas/gambar10.PNG)

Selamat jika kamu sudah bisa login dan bisa masuk kefoldernya kamu tinggal share-share deh hehe.

Sekian dari saya, Terima kasih sudah mampir ^.^
