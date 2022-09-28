---
title: "DNS Server"
tags: ["Sekolah", "Linux"]
description: "Penginstalan, Konfigurasi, Pengetesan"
---
# DNS Server
Haloo.. 
  Nama saya **Angga Aditia Kurnia** dari kelas **XI TKJ 1**, disini saya akan menjelaskan bagaimana proses penkonfigurasian DNS Server di linux Debian 10.
  Pertama kita harus melakukan proses installasi packagenya terlebih dahulu..

## Proses Installasi Package Pada Debian 10

  Sebelumnya lakukan pengupdatean apt agar ter-refresh dengan mirror terbaru. Commandnya seperti ini..
`root@anggaww # apt update`
  Selanjutnya lakukan penginstallan package **bind9** untuk aplikasi DNS Servernya. Juga install package **dnsutils**. Untuk commandnya seperti ini..
`root@anggaww # apt install bind9 dnsutils`

## Proses Konfigurasi BIND9
  Untuk memulai konfigurasi BIND9 kita dapat masuk ke direktori `/etc/bind` terlebih dahulu. Caranya seperti dibawah..
`root@anggaww # cd /etc/bind`
  dan lihat list file yang ada didalam direktori tersebut
`root@anggaww # ls`
  Jika sudah terlihat, copy file db.local menjadi db.domain dan copy file db.127 menjadi db.ip. File file yang kita copy tersebut adalah file yang nantinya akan kita konfigurasi. Dengan cara seperti..
`root@anggaww # cp db.local db.domain`
`root@anggaww # cp db.127 db.ip`
  Edit file db.domain dan konfigurasi sesuai dengan domain yang kita inginkan dan jangan lupa untuk memasukan alamat ip address yang nantinya akan di translasikan ke domain yang kamu ingin. Sebagai contoh saya akan membuat domain angga.net dengan ip address saya yaitu 10.100.69.18 dan subdomain www dan blog. Sebagai catatan jika server subdomain sama dengan salah satu alamat server yang dituju maka anda bisa gunakana CNAME (canonical name). Saya menggunakan text editor nano sebagai catatan setelah mengedit, untuk keluar dan menyimpan tekan ctrl+x.
