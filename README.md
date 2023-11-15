# Jarkom-Modul-3-D05-2023
**Praktikum Jaringan Komputer Modul 3 Tahun 2023**

## Author
| Nama | NRP |Github |
|---------------------------|------------|--------|
|Ihsan Widagdo | 5025211231 | https://github.com/dagdo03 |
|Sandyatama Fransisna Nugraha | 5025211196 | https://github.com/TamaFn | 

# Laporan Resmi 

## Topologi Soal 3
![image](Img/topologi.png)


### Panduan Memulai Modul
Langkah pertama adalah menambahkan docker dengan debian sebelum melakukan preference. 

![image](Img/preference.png)

Langkah selanjutnya adalah menyetting debian dengan adapter 5 karena menggunakan 5 lintas arah jaringan

![image](Img/debian.png)



### Sebelum memulai 

Sebelum memulai setting node, konfigurasikan tiap-tiap node dengan ip sebagai berikut 

### Konfigurasi IP Pada Masing" Node 
- **Aura (Router DHCP Relay)**
  ```
  auto eth0
  iface eth0 inet dhcp
  up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.24.0.0/16

  auto eth1
  iface eth1 inet static
          address 10.24.1.0
          netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
          address 10.24.2.0
          netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
          address 10.24.3.0
          netmask 255.255.255.0

  auto eth4
  iface eth3 inet static
          address 10.24.4.0
          netmask 255.255.255.0
  ```
- **Freiren (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.3
          netmask 255.255.255.0
          gateway 10.24.4.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- **Flamme (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.2
          netmask 255.255.255.0
          gateway 10.24.4.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- **Fern (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.1
          netmask 255.255.255.0
          gateway 10.24.4.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- **Lugner (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.1
          netmask 255.255.255.0
          gateway 10.24.3.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```

- **Linie (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.2
          netmask 255.255.255.0
          gateway 10.24.3.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- **Lawine (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.3
          netmask 255.255.255.0
          gateway 10.24.3.0
          up echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- **Stark (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet dhcp

          
  ```

- **Sein (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Revolte (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Richter (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet dhcp

  ```

- **Himmel (DHCP Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.1.1
          netmask 255.255.255.0
          gateway 10.24.1.0
  ```

- **Heiter (DNS Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.1.2
          netmask 255.255.255.0
          gateway 10.24.1.0
  ```

- **Denken (Database Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.2.1
          netmask 255.255.255.0
          gateway 10.24.2.0
  ```

- **Heiter (Eisen Balancer)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.2.2
          netmask 255.255.255.0
          gateway 10.24.2.0
  ```


- **Alamat IP Pada Masing-masing Node**
  ```
  Aura (Router DHCP Relay)    : 10.24.1.0 
  Fern (Laravel Worker)	      : 10.24.4.1
  Flamme (Laravel Worker)     : 10.24.4.2 
  Freiren (Laravel Worker)	  : 10.24.4.3
  Stark (Client Dynamic)	  : 10.24.4.4
  Sein (Client Dynamic)	      : 10.24.4.5
  Lugner (PHP Worker)	      : 10.24.3.1
  Linie (PHP Worker)          : 10.24.3.2
  Lawine (PHP Worker)	      : 10.24.3.3
  Richter (Client Dynamic)	  : 10.24.3.4
  Revolte (Client Dynamic)	  : 10.24.3.5
  Denken (Database Server)    : 10.24.2.1
  Eisen (Load Balancer)       : 10.24.2.2
  Himmel (DHCP Server)        : 10.24.1.1
  Heiter (DNS Serve)          : 10.24.1.2
  ```

Lakukan instalasi setiap node menggunakan  `nano .bashrc` agar tiap node siap melakukan konfigurasi

- **Aura Router(DHCP Relay)**
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.24.0.0/16
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install isc-dhcp-relay -y
  echo 'SERVERS="10.24.1.1"
  INTERFACES="eth1 eth2 eth3 eth4"
  OPTIONS=""
  ' > /etc/default/isc-dhcp-relay
  echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf
  service isc-dhcp-relay start
  ```
- **Himmel (DHCP Server)**
  ```
  rm /var/run/dhcpd.pid
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install isc-dhcp-server -y
  echo 'DHCPDv4_PID=/var/run/dhcpd.pid
  INTERFACESv4="eth0"' > /etc/default/    isc-dhcp-server

  echo 'subnet 10.24.1.0  netmask 255.255.255.0 {
    
  };      
  subnet 10.24.2.0 netmask 255.255.255.0 {

  };
  subnet 10.24.3.0  netmask 255.255.255.0 {
    range 10.24.3.16 10.24.3.32;
    range 10.24.3.64 10.24.3.80;
    option routers 10.24.3.0;
    option broadcast-address 10.24.3.255;
    option domain-name-servers 10.24.1.2;
    default-lease-time 180;
    max-lease-time 5760;
  };
  subnet 10.24.4.0  netmask 255.255.255.0 {
    range 10.24.4.12 10.24.4.20;
    range 10.24.4.160 10.24.4.168;
    option routers 10.24.4.0;
    option broadcast-address 10.24.4.255;
    option domain-name-servers 10.24.1.2;
    default-lease-time 720;
    max-lease-time 5760; 
  };' > /etc/dhcp/dhcpd.conf

  service isc-dhcp-server start
  ```
- **Heiter (DNS Server)**
  ```
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y

  echo 'options {
        directory "/var/cache/bind";
        forwarders{
                192.168.122.1;
        };
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
  };' > /etc/bind/named.conf.options

  echo 'zone "riegel.canyon.d05.com"{  
        type master;
        file "/etc/bind/jarkom/riegel.canyon.d05.com";
  };' > /etc/bind/named.conf.local
  echo 'zone "granz.channel.d05.com"{  
        type master;
        file "/etc/bind/jarkom/granz.channel.d05.com";
  };' >> /etc/bind/named.conf.local


  mkdir /etc/bind/jarkom
  cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.d05.com
  cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.d05.com
  echo ';
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     riegel.canyon.d05.com. root.riegel.canyon.d05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      riegel.canyon.d05.com.
  @       IN      A       10.24.2.2       ; IP  
  www     IN      CNAME   riegel.canyon.d05.com.
  @       IN      AAAA    ::1' > /etc/bind/jarkom/riegel.canyon.d05.com

  echo ';
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     granz.channel.www.com. root.granz.channel.yyy.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      granz.channel.d05.com.
  @       IN      A       10.24.2.2       ; IP  
  www     IN      CNAME   granz.channel.d05.com.
  @       IN      AAAA    ::1' > /etc/bind/jarkom/granz.channel.d05.com

  service bind9 start
  ```

- **Denken ( Database Server )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Eisen ( Load Balancer )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Fern ( Laravel Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Flamme ( Laravel Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Freiren ( Laravel Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Lugner ( PHP Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Linie ( PHP Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Lawine ( PHP Worker )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Stark ( Client Dynamic )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Sein ( Client Dynamic )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Revolte ( Client Dynamic )**
  ```
  apt install nginx php php-fpm -y
  ```

- **Richter ( Client Dynamic )**
  ```
  apt install nginx php php-fpm -y
  ```

## Soal Pembuka
> Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

![image](Img/IpA.png)

## Soal 1 
> Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.

![image](Img/IpA.png)

## Soal 2
> Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80

![image](Img/no2.png)

### Soal 3 
> Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168

![image](Img/no2.png)

## Soal 4 
> Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut

![image](Img/no2.png)

## Soal 5 
> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit

![image](Img/no2.png)

## Soal 6 
> Pada masing-masing worker PHP, lakukan konfigurasi virtual host untuk website berikut dengan menggunakan php 7.3.

![image](Img/no2.png)

## Soal 7 
> Kepala suku dari Bredt Region memberikan resource server sebagai berikut:
- Lawine, 4GB, 2vCPU, dan 80 GB SSD.
- Linie, 2GB, 2vCPU, dan 50 GB SSD.
- Lugner 1GB, 1vCPU, dan 25 GB SSD.

Aturlah agar Eisen dapat bekerja dengan maksimal, lalu lakukan testing dengan 1000 request dan 100 request/second.


![image](Img/no2.png)

## Soal 8 
> Karena diminta untuk menuliskan grimoire, buatlah analisis hasil testing dengan 200 request dan 10 request/second masing-masing algoritma Load Balancer dengan ketentuan sebagai berikut:
- Nama Algoritma Load Balancer
- Report hasil testing pada Apache Benchmark
- Grafik request per second untuk masing masing algoritma. 
- Analisis

![image](Img/no2.png)

## Soal 9 
> Dengan menggunakan algoritma Round Robin, lakukan testing dengan menggunakan 3 worker, 2 worker, dan 1 worker sebanyak 100 request dengan 10 request/second, kemudian tambahkan grafiknya pada grimoire.

![image](Img/no2.png)

## Soal 10 
> Selanjutnya coba tambahkan konfigurasi autentikasi di LB dengan dengan kombinasi username: “netics” dan password: “ajkyyy”, dengan yyy merupakan kode kelompok. Terakhir simpan file “htpasswd” nya di /etc/nginx/rahasisakita/

![image](Img/no2.png)

## Soal 11 
> Lalu buat untuk setiap request yang mengandung /its akan di proxy passing menuju halaman https://www.its.ac.id. hint: (proxy_pass)

![image](Img/no2.png)

## Soal 12 
> Selanjutnya LB ini hanya boleh diakses oleh client dengan IP [Prefix IP].3.69, [Prefix IP].3.70, [Prefix IP].4.167, dan [Prefix IP].4.168. hint: (fixed in dulu clinetnya)

![image](Img/no2.png)

## Soal 13 
> Karena para petualang kehabisan uang, mereka kembali bekerja untuk mengatur granz.channel.yyy.com. Semua data yang diperlukan, diatur pada Denken dan harus dapat diakses oleh Frieren, Flamme, dan Fern. 

![image](Img/no2.png)

## Soal 14 
> Frieren, Flamme, dan Fern memiliki Granz Channel sesuai dengan quest guide berikut. Jangan lupa melakukan instalasi PHP8.0 dan Composer

![image](Img/no2.png)

## Soal 15,16,17 
> Granz Channel memiliki beberapa endpoint yang harus ditesting sebanyak 100 request dengan 10 request/second. Tambahkan response dan hasil testing pada grimoire. 
- POST /auth/register (15)
- POST /auth/login (16)
- GET /me (17)

![image](Img/no2.png)

![image](Img/no2.png)

![image](Img/no2.png)

## Soal 18 
> Untuk memastikan ketiganya bekerja sama secara adil untuk mengatur Granz Channel maka implementasikan Proxy Bind pada Eisen untuk mengaitkan IP dari Frieren, Flamme, dan Fern.

![image](Img/no2.png)

## Soal 19 
> Untuk meningkatkan performa dari Worker, coba implementasikan PHP-FPM pada Frieren, Flamme, dan Fern. Untuk testing kinerja naikkan 
- pm.max_children
- pm.start_servers
- pm.min_spare_servers
- pm.max_spare_servers
sebanyak tiga percobaan dan lakukan testing sebanyak 100 request dengan 10 request/second kemudian berikan hasil analisisnya pada Grimoire.

![image](Img/no2.png)

## Soal 20 
> Nampaknya hanya menggunakan PHP-FPM tidak cukup untuk meningkatkan performa dari worker maka implementasikan Least-Conn pada Eisen. Untuk testing kinerja dari worker tersebut dilakukan sebanyak 100 request dengan 10 request/second.

![image](Img/no2.png)


PS:
Grimoire dikumpulkan dalam bentuk PDF dengan format yyy_Grimoire.pdf.
yyy merupakan kode kelompok


