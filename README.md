# Jarkom_Modul3_Lapres_T8
### Anggota Kelompok:
#### 1. Kadek Nesya Kurniadewi (05311840000009)
#### 2. Agung Mulyono (053118400000035)

### Setting Topologi
![1](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/topologi.png)
### Setting DHCP Server
* Langkah pertama yang dilakukan adalah menginstal isc-dhcp-server pada **TUBAN** dengan menggunakan perintah<br>
`apt-get install isc-dhcp-server`
* Kemudian buka file konfigurasi interface dengan perintah `nano /etc/default/isc-dhcp-server`<br>
* Setelah itu pada bagian **INTERFACES** masukkan **eth0**<br>
`INTERFACES="eth0"`<br>
![2](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/dhcpserver.png)
### Setting DHCP Relay
* Install isc-dhcp-relay pada **SURABAYA** dengan menggunakan perintah<br>
`apt-get install isc-dhcp-relay`<br>
* Lalu masukkan **IP TUBAN** sebagai destinasi dhcp server<br>
* Buka file konfigurasi interface dengan menggunakan perintah `nano /etc/default/isc-dhcp-relay`<br>
* Kemudian pada bagian **INTERFACES** masukkan **eth1 eth2 eth3**<br>
`INTERFACES="eth1 eth2 eth3"`<br>
![3](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/relay.png)
## Soal DHCP
1. Seluruh client TIDAK DIPERBOLEHKAN menggunakan konfigurasi IP Statis.
2. Client pada subnet 1 mendapatkan range IP dari 192.168.0.10 sampai 192.168.0.100 dan 192.168.0.110 sampai 192.168.0.200.
3. Client pada subnet 3 mendapatkan range IP dari 192.168.1.50 sampai 192.168.1.70.
4. Client mendapatkan DNS Malang dan DNS 202.46.129.2 dari DHCP
5. Client di subnet 1 mendapatkan peminjaman alamat IP selama 5 menit, sedangkan client pada subnet 3 mendapatkan peminjaman IP selama 10 menit.

### Penyelesaian DHCP
![4](https://github.com/agung56/Jarkom_Modul3_Lapres_T8/blob/main/img/dhcpconf.png)

## Soal Proxy
1. akses ke proxy hanya bisa dilakukan oleh Anri sendiri sebagai user TA. User autentikasi milik Anri memiliki format:
    * User : userta_yyy
    * Password : inipassw0rdta_yyy
    
* Keterangan : yyy adalah nama kelompok masing-masing. Contoh: userta_c01

2. setiap hari Selasa-Rabu pukul 13.00-18.00
3. setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).
4. setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id agar Anri selalu ingat untuk mengerjakan TA
5. Bu Meguri meminta Anri untuk mengubah error page default squid
6. arena Bu Meguri dan Anri adalah tipe orang pelupa, maka untuk memudahkan mereka, Anri memiliki ide ketika menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080. 
(Keterangan : yyy adalah nama kelompok masing-masing. Contoh: janganlupa-ta.c01.pw)
