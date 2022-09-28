### DNS Server Debian 10
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
@       IN      SOA     zonabiner.dev. root.zonabiner.dev. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      zonabiner.dev.
18      IN      PTR     zonabiner.dev.

```

