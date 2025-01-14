---
title: "Konfigurasi DNS Server"
tags: ["Linux", "Sekolah", "Network"]
date: "2023-11-02"
description: "Installasi, Konfigurasi, Pengetesan DNS Server"
summary: "Installasi, Konfigurasi, Pengetesan DNS Server"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: false
---

# DNS Server Debian 10
Haloo.. 

Nama saya **Angga Aditia Kurnia** dari kelas **XI TKJ 1**, disini saya akan menjelaskan bagaimana proses penkonfigurasian DNS Server di linux Debian 10.

Pertama kita harus melakukan proses installasi packagenya terlebih dahulu..

### Proses Installasi Package Pada Debian 10

Sebelumnya lakukan pengupdatean apt agar ter-refresh dengan mirror terbaru. Commandnya seperti ini..

`root@anggaww # apt update`

Selanjutnya lakukan penginstallan package **bind9** untuk aplikasi DNS Servernya. Juga install package **dnsutils**. Untuk commandnya seperti ini..

`root@anggaww # apt install bind9 dnsutils`

### Proses Konfigurasi BIND9
Untuk memulai konfigurasi BIND9 kita dapat masuk ke direktori `/etc/bind` terlebih dahulu. Caranya seperti dibawah..

`root@anggaww # cd /etc/bind`

dan lihat list file yang ada didalam direktori tersebut

`root@anggaww # ls`

Jika sudah terlihat, copy file db.local menjadi db.domain dan copy file db.127 menjadi db.ip. File file yang kita copy tersebut adalah file yang nantinya akan kita konfigurasi. Dengan cara seperti..

`root@anggaww # cp db.local db.domain`

`root@anggaww # cp db.127 db.ip`

Edit file db.domain dan konfigurasi sesuai dengan domain yang kita inginkan dan jangan lupa untuk memasukan alamat ip address yang nantinya akan di translasikan ke domain yang kamu ingin. Sebagai contoh saya akan membuat domain angga.net dengan ip address saya yaitu 10.100.69.18 dan subdomain www dan blog. Sebagai catatan jika server subdomain sama dengan salah satu alamat server yang dituju maka anda bisa gunakana CNAME (canonical name). Saya menggunakan text editor nano sebagai catatan setelah mengedit, untuk keluar dan menyimpan tekan ctrl+x.

`root@anggaww # nano db.domain`

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     angga.net. root.angga.net. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      angga.net.
@       IN      A       10.100.69.18
www     IN      A       10.100.69.18
blog    IN      CNAME   www

```

Kemudian kita edit file db.ip. Pada file ini kita ganti angka 1.0.0 menjadi angka akhiran dari ip address kita misal saya mempunyai ip address 10.100.69.18 maka saya akan mengganti angka 1.0.0 menjadi 18. Caranya sepertiini..

`root@anggaww # nano db.ip`


```
;
; BIND reverse data file for local loopback interface
;
$TTL    604800
@       IN      SOA     angga.net. root.angga.net. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      angga.net.
18      IN      PTR     angga.net.

```

Selanjutnya kita edit file named.conf.options untuk konfigurase reverse zone. Pada 69.100.10.in-addr.arpa isikan ip address kamu secara terbalik (dari belakang). Misal ip address saya adalah 10.100.69.18 maka jika dibalik 18.69.100.10 namun jangan dituliskan semua cukup ambil 3 bagian belakang yaitu 69.100.10. Mengapa 18 tidak di inputkan ? Karena 18 sudah kita inputkan pada file db.ip.

`root@anggaww # nano named.conf.local`

```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
// include "/etc/bind/zones.rfc1918";

zone "angga.net"{ 
        type master;
        file "/etc/bind/db.domain";
};

zone "69.100.10.in-addr.arpa"{ 
        type master;
        file "/etc/bind/db.ip";
};

```

Setelah itu kita edit file named.conf.options untuk mengkonfigurasi forwaders dns, ini berguna untuk misal kita membuat suatu dns server pada jaringan lokal kita dan ketika kita meminta request ke domain lain yang tidak kita tangani maka dns server kita akan meneruskan nya ke forwaders yang sudah kita set. Jangan lupa pada dnssec-validation kita set menjadi no.

`root@anggaww # nano named.conf.options `

```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                8.8.8.8;
        };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation no;

        listen-on-v6 { any; };
};

```

Selanjutnya kita edit file resolv.conf dan ubah nameserver menjadi alamat ip dns server kita.

`root@anggaww # nano /etc/resolv.conf`

`nameserver 10.100.69.18`

Jangan lupa untuk merestart service BIND9 agar konfigurasi yang baru kita terapkan bisa digunakan.

`root@anggaww # systemctl restart bind9.service `

### Pengetesan 
Untuk melakukan pengujian, kita hanya menggunakan perintah 

`root@anggaww # nslookup "ip dns-servernya"`


```
nslookup 10.100.69.18
18.69.100.10.in-addr.arpa	name = angga.net.

# nslookup angga.net
Server:		10.100.69.18
Address:	10.100.69.18#53

Name:	
Address: 10.100.69.18

nslookup www.angga.net
Server:		10.100.69.18
Address:	10.100.69.18#53

Name:	www.angga.net
Address: 10.100.69.18

nslookup blog.angga.net
Server:		10.100.69.18
Address:	10.100.69.18#53

blog.angga.net	canonical name = www.angga.net.
Name:	www.angga.net
Address: 10.100.69.18
```
Demikian, kita berhasil melakukan proses pengaturan DNS Server mulai dari penginstallan, konfigurasi, dan juga pengetesan. Sekian dari saya Terima Kasih ^.^
