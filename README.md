# Jarkom_Modul3_Lapres_T8
### Anggota Kelompok:
#### 1. Kadek Nesya Kurniadewi (05311840000009)
#### 2. Agung Mulyono (05311840000035)

### Setting Topologi
![1](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/topologi.png)
### Setting DHCP Server
* Install **isc-dhcp-server** pada **TUBAN** dengan menggunakan perintah<br> 
`apt-get install isc-dhcp-server`<br>
* Kemudian buka file konfigurasi interface dengan menggunakan perintah<br>
`nano /etc/default/isc-dhcp-server`
* Pada bagian **INTERFACES** isikan **eth0**<br>
`INTERFACES="eth0"`<br>
![2](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/dhcpserver.png)
### Setting DHCP Relay
* Install **isc-dhcp-relay** pada **SURABAYA** dengan menggunakan perintah <br>
`apt-get install isc-dhcp-relay`<br>
* Isikan **IP TUBAN** sebagai destinasi dhcp server
* Buka file konfigurasi interface dengan menggunakan perintah<br>
`nano /etc/default/isc-dhcp-relay`<br>
* Pada bagian **INTERFACES** isikan **eth1 eth2 eth3**<br>
`INTERFACES="eth1 eth2 eth3"`<br>
![3](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/relay.png)

## Soal DHCP
### 1. Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
* Buka file konfigurasi interface network dengan memasukkan perintah 
`nano /etc/network/interfaces`<br>
* Kemudian ubah **iface eth0 inet static** menjadi **iface eth0 inet dhcp** pada semua client.<br>
Hal ini bertujuan agar IP yang didapat oleh client diperoleh dari dhcp server yang ada di**TUBAN** dan dapat berubah pada selang waktu yang telah ditentukan
* Setelah itu lakukan `service networking restart`
### 2. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200
### 3. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70
### 4. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
### 5. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.
#### Penyelesaian nomor 2 sampai nomor 5

![4](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/dhcpconf.png)
* Client 1 memiliki subnet **192.168.0.0** dan netmask **255.255.255.0**
* Buka file konfigurasi dhcp dengan menggunakan perintah<br>
`nano /etc/dhcp/dhcpd.conf`<br>
* Isikan seperti dibawah ini
```
subnet 192.168.0.0 netmask 255.255.255.0 {
   range 192.168.0.10 192.168.0.100; //range yang pertama (soal no.2)
   range 192.168.0.110 192.168.0.200; //range yang kedua (soal no.2)
   option routers 192.168.0.1;
   option broadcast-address 192.168.0.255;
   option domain-name-servers 10.151.83.146, 202.46.129.2; //(soal no.4)
   default-lease-time 300; //Waktu peminjaman IP pada client 1 yaitu 5 menit atau 300 detik
   max-lease-time 7200;
}
```
* Client 3 memiliki subnet **192.168.1.0** dan netmask **255.255.255.0**
* Buka file konfigurasi dhcp dengan menggunakan perintah<br>
`nano /etc/dhcp/dhcpd.conf`<br>
* Isikan seperti dibawah ini
```
subnet 192.168.1.0 netmask 255.255.255.0 {
   range 192.168.1.50 192.168.1.70; //range IP yang disediakan DHCP (soal no.3)
   option routers 192.168.1.1;
   option broadcast-address 192.168.1.255;
   option domain-name-servers 10.151.83.146, 202.46.129.2; //(soal no.4)
   default-lease-time 600; //Waktu peminjaman IP pada client 3 yaitu 10 menit atau 600 detik
   max-lease-time 7200;
}
```
* Kemudian tambahkan satu subnet lagi yaitu
`subnet 10.151.83.144 netmask 255.255.255.248 {}`<br>
Dimana 10.151.83.144 merupakan NID DMZ yang digunakan oleh server **TUBAN** agar bisa terhubung ke router **SURABAYA** dan terhubung ke client
* Setelah itu save file konfigurasi dan lakukan restart dengan menggunakan perintah
`service isc-dhcp-server restart`<br>

## Soal Proxy
### Instalasi Squid
- install squid pada UML MOJOKERTO dengan command 
```
apt-get install squid
```
- cek status squid untuk memastikan squid telah berjalan dengan menggunakan command
```
service squid status
```
### Konfigurasi Dasar Squid
- melakukan backup file konfigurasi default yang sudah diseduakan oleh Squid dengan menggunakan command
```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```
- membuat konfigurasi baru
```
nano /etc/squid/squid.conf
```
- mengetik script seperti dibawah ini, pada config yang baru
```
http_port 8080
visible_hostname mojokerto
```
- `http_port 8080` merupakan port yang digunakan untuk mengakses proxy yaitu port 8080
- `visible_hostname mojokerto` merupakan syntax nama proxy yang akan terlihat oleh user

- Restart squid dengan cara mengetikkan perintah seperti dibawah ini :
```
service squid restart
```
- Kemudian mengubah peraturan proxy. Menggunakan IP MOJOKERTO sebagai host dan isikan port 8080 pada OS Windows atau Pengaturan proxy pada browser Mozilla Firefox.
- Kemudian coba untuk mengakses web http://elearning.if.its.ac.id/ dengan menggunakan mode private/incognito). Maka akan muncul halaman seperti dibawah ini :
- Karena error page (access denied), supaya bisa mengakses web http://elearning.if.its.ac.id/, maka tambahkan script dibawah ini pada konfigurasi squid `nano /etc/squid/squid.conf`
```
http_access allow all
```
Dimana `http_access allow all` digunakan agar memperbolehkan semuanya untuk mengakses proxy via http. Pengaturan ini perlu ditambahkan karena pengaturan default squid adalah deny. <br>
Untuk menolak koneksi, ***allow*** diganti dengan ***deny***.
- Kemudian simpan file konfigurasi tersebut, lalu lakukan ***restart*** squid dengan menggunakan `service squid restart`, kemudian refresh halaman web http://elearning.if.its.ac.id/.


### 1. akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. (7) User autentikasi milik Anri memiliki format:
    * User : userta_yyy
    * Password : inipassw0rdta_yyy
    
* Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01
#### Penyelesaian :
- Melakukan update pada UML MOJOKERTO dengan command `apt-get update`
- Menginstall `apache2-utils` pada UML ***MOJOKERTO*** dengan menggunakan :
```
apt-get install apache2-utils
```
- Membuat user dengan username ***userta_t08*** dan password ***inipassw0rdta_t08*** dengan menggunakan command :
```
htpasswd -c /etc/squid/passwd userta_t08
```
Kemudian ketiikan password ***inipassw0rdta_t08***
- Mengedit kofigurasi squid menjadi :
```
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```
- Lakukan restart squid dengan mengetikkan `service squid restart`
- Kemudian mengubah peraturan proxy. Menggunakan IP MOJOKERTO sebagai host dan isikan port 8080 pada OS Windows atau Pengaturan proxy pada browser Mozilla Firefox. 
- Coba untuk mengakses web http://elearning.if.its.ac.id/ (dengan menggunakan mode private/incognito), akan muncul pop-up login
- Masukkan username ***userta_t08*** dan password ***inipassw0rdta_t08***, kemudian web http://elearning.if.its.ac.id/ berhasil dibuka

### 2. setiap hari Selasa-Rabu pukul 13.00-18.00
#### Penyelesaian :
- Buat file baru bernama ***acl.conf*** pada folder squid, dengan mengetik :
```
nano /etc/squid/acl.conf
```
- Kemudian menambahkan batasan waktu akses seperti dibawah ini :
```
acl AVAILABLE_WORKING time TW 13:00-18:00

```
- Mengedit file ***squid.conf*** dengan mengetikkan :
```
nano /etc/squid/squid.conf
```
Mengubah menjadi :
```
include /etc/squid/acl.conf
lupaaa
```
- Lakukan restart squid dengan menggunakan `service squid restart`
- Coba mengakses web http://elearning.if.its.ac.id/ (dengan menggunakan mode private/incognito) yang nantinya akan menampilkan halaman error saat mengakses ***diluar waktu akses***, ketika mengakses pada rentang waktu yang sesuai dengan soal maka akan menampilkan halaman web tersebut

### 3.  setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).
#### Penyelesaian :
- Mengedit file ***acl.conf*** pada `/etc/squid/acl.conf` dengan menambahkan command seperti dibawah ini :
```
acl AVAILABLE_WORKING time TWH 21:00-23:59
acl AVAILABLE_WORKING time WHF 00:00-09:00
```
- Lakukan restart squid dengan menggunakan `service squid restart`

### 4. setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA
#### Penyelesaian :
- Mengedit file pada squid.conf dengan mengetikkan :
```
nano /etc/squid/squid.conf
```
Mengubah dengan menambahkan setting redirect :
```
acl badsites dstdomain google.com
deny_info http://monta.if.its.ac.id badsites
http_reply_access deny badsites
```
- Lakukan restart squid dengan menggunakan `service squid restart`

### 5. Bu Meguri meminta Anri untuk mengubah error page default squid
#### Penyelesaian :
### 6. arena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. 
(Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw)
#### Penyelesaian :
#### Instalasi Bind
- Buka UML MALANG, kemudian lakukan update package list dengan menggunakan command :
```
apt-get update
```
- Menginstall aplikasi ***bind9*** dengan command :
```
apt-get install bind9 -y
```
#### Membuat Domain
Pada soal ini, kita diminta untuk membuat domain ***janganlupa-ta.t08.pw***
- Sebelum membuat domain, pastikan pada file VPN kita sudah berisi konfigurasi ***DHCP IP MALANG*** dengan `dhcp-option DNS IP MALANG`
- Mengubah isi file ***named.conf.local*** dengan menggunakan command dibawah ini :
```
nano /etc/bins/named.conf.local
```
- Kemudian mengisi komfigurasi domain ***janganlupa-ta.t08.pw*** dengan menggunakan syntax seperti dibawah ini :
```
zone "janganlupa-ta.t08.pw" { 
   type master;
   file "/etc/bind/jarkom/janganlupa-ta.t08.pw";
};
```
- Membuat folder jarkom di dalam /etc/bind `mkdir /etc/bind/jarkom`
- Copy file ***db.local*** pada path ***/etc/bind*** ke dalam folder jarkom yang baru saja dibuat dan kemudian mengubah namanya menjadi janganlupa-ta.t08.pw, dengan command :
```
cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.t08.pw
```
- Buka file ***janganlupa-ta.t08.pw*** dan edit sesuai dengan syntax seperti dibawah ini :
```
nano /etc/bind/jarkom/janganlupa-ta.t08.pw
```
- Kemudian restart bind9 dengan perintah `service bind9 restart`
- Masukkan domain ***janganlupa-ta.t08.pw*** pada host proxy dan 8080 sebagai portnya
