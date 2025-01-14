---
title: "Konfigurasi NTP Server"
tags: ["Linux", "Sekolah", "Network"]
date: "2023-11-03"
description: "Installasi, Konfigurasi, Pengetesan NTP Server"
summary: "Installasi, Konfigurasi, Pengetesan NTP Server"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: false
---

# Konfigurasi NTP Server Debian 10
Halo semua!

Lagi-lagi kembali dengan **saya**, kali ini saya akan menjelaskan bagaimana caranya kita mengkonfigurasi NTP Server di linux Debian 10.

### Proses Instalasi Package

Seperti biasa yang kita lakukan, di sini saya selalu menyarankan untuk menambahkan 3 cd Debian sekaligus. Fungsi ini disini agar memudahkan kita, walaupun, di proses konfigurasi disini hanya membutuhkan satu cd saja, yaitu cd 1 Debian 10.

Jika sudah ditambahkan kita tuliskan command untuk menambahkan cd yang baru di masukan agar apt bisa menambahkannya di package repo kita, juga sekalian kita refresh package manager kita.

`# apt-cdrom add`

`# apt update`

Selanjutnya, kita install saja package **ntp**nya.

`# apt install ntp`

### Proses Konfigurasi NTP Server

Disini kita lanjut ketahap konfigurasinya. Yang pertama harus kita lakukan yaitu mengkonfigurasi terlebih dahulu **IP Address** kita. Untuk settingnya kita tambahkan IP Static kita pada `/etc/network/interfaces`.

`# vim /etc/network/interfaces`

Sudah terkonfigurasi IP kita, restart servicenya dan lanjut untuk konfigurasi **NTP**nya.

`vim /etc/ntp.conf`

Yang kita di isi disini ada beberapa. Yang pertama kita tambahkan comment pada daerah pool default yang diberikan, dan tambahkan daerah kita sendiri.

```

# pool.ntp.org maps to about 1000 low-stratum NTP servers.  Your server will
# pick a different set every time it starts up.  Please consider joining the
# pool: <http://www.pool.ntp.org/join.html>
#pool 0.debian.pool.ntp.org iburst
#pool 1.debian.pool.ntp.org iburst
#pool 2.debian.pool.ntp.org iburst
#pool 3.debian.pool.ntp.org iburst
server 127.127.1.0
fudge 127.127.1.0 startum 1

```

Selanjutnya, hapus comment daerah dekat crytography dan juga setting disana sesuai network, netmask, broadcast kita.

```
# Clients from this (example!) subnet have unlimited access, but only if
# cryptographically authenticated.
restrict 192.168.7.0 mask 255.255.255.0 notrust nomodify


# If you want to provide time to your local subnet, change the next line.
# (Again, the address is an example only.)
broadcast 192.168.7.63
```

Jika sudah semua, save filenya dan restart service ntp-nya.

### Pengetesan NTP Server pada Client

Sudah semua, kita lanjut pada proses pengecekkan dan pengetesan. 

Yang pertama bisa kita lakukan adalah mengecek bahwa waktu lokal kita ada di ntpq kita.

`# ntpq -p`

```
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*LOCAL(0)        .LOCL.           5 l   50   64  377    0.000    0.000   0.000
 192.168.7.63    .BCST.          16 B    -   64    0    0.000    0.000   0.000

```

Jika sudah kita perlu setting waktu pada Windows client kita pada bagian internet timenya
