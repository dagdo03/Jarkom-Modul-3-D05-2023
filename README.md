# Jarkom-Modul-2-D05-2023
**Praktikum Jaringan Komputer Modul 3 Tahun 2023**

## Author
| Nama | NRP |Github |
|---------------------------|------------|--------|
|Ihsan Widagdo | 5025211231 | https://github.com/dagdo03 |
|Sandyatama Fransisna Nugraha | 5025211196 | https://github.com/TamaFn | 

# Laporan Resmi 

## Topologi Soal 3
![image](Img/Topologi.png)


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

  auto eth1
  iface eth1 inet static
          address 10.24.1.1
          netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
          address 10.24.2.1
          netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
          address 10.24.3.10
          netmask 255.255.255.0

  auto eth4
  iface eth3 inet static
          address 10.24.4.10
          netmask 255.255.255.0
  ```
- **Freiren (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.4
          netmask 255.255.255.0
          gateway 10.24.4.10
  ```
- **Flamme (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.3
          netmask 255.255.255.0
          gateway 10.24.4.10
  ```
- **Fern (Laravel Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.2
          netmask 255.255.255.0
          gateway 10.24.4.10
  ```
- **Lugner (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.2
          netmask 255.255.255.0
          gateway 10.24.3.1
  ```

- **Linie (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.3
          netmask 255.255.255.0
          gateway 10.24.3.1
  ```
- **Lawine (PHP Worker)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.4
          netmask 255.255.255.0
          gateway 10.24.3.1
  ```
- **Stark (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.5
          netmask 255.255.255.0
          gateway 10.24.4.1
  ```

- **Sein (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.4.6
          netmask 255.255.255.0
          gateway 10.24.4.10
  ```

- **Revolte (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.6
          netmask 255.255.255.0
          gateway 10.24.3.10
  ```

- **Richter (Client Dynamic)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.3.5
          netmask 255.255.255.0
          gateway 10.24.3.10
  ```

- **Himmel (DHCP Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.1.2
          netmask 255.255.255.0
          gateway 10.24.1.1
  ```

- **Heiter (DNS Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.1.3
          netmask 255.255.255.0
          gateway 10.24.1.1
  ```

- **Denken (Database Server)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.2.2
          netmask 255.255.255.0
          gateway 10.24.2.1
  ```

- **Heiter (Eisen Balancer)**
  ```
  auto eth0
  iface eth0 inet static
          address 10.24.2.3
          netmask 255.255.255.0
          gateway 10.24.2.1
  ```


- **Alamat IP Pada Masing-masing Node**
  ```
  Aura (Router DHCP Relay)    : 10.24.1.1 
  Fern (Laravel Worker)	      : 10.24.4.2
  Flamme (Laravel Worker)     : 10.24.4.3 
  Freiren (Laravel Worker)	  : 10.24.4.4
  Stark (Client Dynamic)	  : 10.24.4.5
  Sein (Client Dynamic)	      : 10.24.4.6
  Lugner (PHP Worker)	      : 10.24.3.2
  Linie (PHP Worker)          : 10.24.3.3
  Lawine (PHP Worker)	      : 10.24.3.4
  Richter (Client Dynamic)	  : 10.24.3.5
  Revolte (Client Dynamic)	  : 10.24.3.6
  Denken (Database Server)    : 10.24.2.2
  Eisen (Load Balancer)       : 10.24.2.3
  Himmel (DHCP Server)        : 10.24.1.2
  Heiter (DNS Serve)          : 10.24.1.3
  ```

Lakukan instalasi setiap node menggunakan  `nano .bashrc` agar tiap node siap melakukan konfigurasi

- **Aura**
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.24.0.0/16
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install isc-dhcp-relay -y
  echo 'SERVERS="10.24.1.2"
  INTERFACES="eth1 eth2 eth3 eth4"
  OPTIONS=""
  ' > /etc/default/isc-dhcp-relay
  echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf
  service isc-dhcp-relay start
  ```
- **Himmel**
  ```
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
  apt-get install isc-dhcp-server -y

  echo 'subnet 10.24.1.0  netmask 255.255.255.0 {
    
  };      
  subnet 10.24.2.0 netmask 255.255.255.0 {

  };
  subnet 10.24.3.0  netmask 255.255.255.0 {
    range 10.24.3.16 10.24.3.32;
    range 10.24.3.64 10.24.3.80;
    option routers 10.24.3.1;
    option broadcast-address 10.24.3.255;
    option domain-name-servers 10.24.1.3;
    default-lease-time 180;
    max-lease-time 5760;
  };
  subnet 10.24.4.0  netmask 255.255.255.0 {
    range 10.24.4.12 10.24.4.20;
    range 10.24.4.160 10.24.4.168;
    option routers 10.24.4.1;
    option broadcast-address 10.24.4.255;
    option domain-name-servers 10.24.1.3;
    default-lease-time 720;
    max-lease-time 5760; 
  };' > /etc/dhcp/dhcpd.conf

  service isc-dhcp-server start
  ```
- **Heiter**
  ```
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
  apt-get update
  apt-get install bind9 -y
  echo 'zone "riegel.canyon.yyy.com"{  
        type master;
        file "/etc/bind/jarkom/riegel.canyon.yyy.com";
  };' > /etc/bind/named.conf.local
  echo 'zone "granz.channel.yyy.com"{  
        type master;
        file "/etc/bind/jarkom/granz.channel.yyy.com";
  };' >> /etc/bind/named.conf.local
  mkdir /etc/bind/jarkom
  cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.yyy.com
  cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.yyy.com
  echo ';
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     riegel.canyon.www.com. root.riegel.canyon.yyy.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      riegel.canyon.yyy.com.
  @       IN      A       10.24.3.1       ; IP  
  www     IN      CNAME   riegel.canyon.yyy.com.
  @       IN      AAAA    ::1' > /etc/bind/jarkom/riegel.canyon.yyy.com

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
  @       IN      NS      granz.channel.yyy.com.
  @       IN      A       10.24.4.1       ; IP  
  www     IN      CNAME   granz.channel.yyy.com.
  @       IN      AAAA    ::1' > /etc/bind/jarkom/granz.channel.yyy.com

  service bind9 start
  ```











- **Nginx Config**
  ```
  apt install nginx php php-fpm -y
  ```
- **Apache2 Config**
  ```
  apt-get update
  apt-get install dnsutils -y
  apt-get install lynx -y
  apt-get install nginx -y
  service nginx start
  apt-get install apache2 -y
  apt-get install libapache2-mod-php7.0 -y
  service apache2 start
  apt-get install wget -y
  apt-get install unzip -y
  apt-get install php -y
  echo -e "\n\nPHP Version:"
  php -v
  ```
- **Zip Download and Unzip**
  ```
  wget -O '/var/www/abimanyu.a09.com' 'https://     drive.usercontent.google.com/download?id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc'
  unzip -o /var/www/abimanyu.a09.com -d /var/www/
  mv /var/www/abimanyu.yyy.com /var/www/abimanyu.a09
  rm /var/www/abimanyu.a09.com
  rm -rf /var/www/abimanyu.yyy.com

  wget -O '/var/www/parikesit.abimanyu.a09.com' 'https://drive.usercontent.google.com/download?id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS'
  unzip -o /var/www/parikesit.abimanyu.a09.com -d /var/www/
  mv /var/www/parikesit.abimanyu.yyy.com /var/www/parikesit.abimanyu.a09
  rm /var/www/parikesit.abimanyu.a09.com
  rm -rf /var/www/parikesit.abimanyu.yyy.com
  mkdir /var/www/parikesit.abimanyu.a09/secret

  wget -O '/var/www/rjp.baratayuda.abimanyu.a09.com' 'https://drive.usercontent.google.com/download?id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6'
  unzip -o /var/www/rjp.baratayuda.abimanyu.a09.com -d /var/www/
  mv /var/www/rjp.baratayuda.abimanyu.yyy.com /var/www/rjp.baratayuda.abimanyu.a09
  rm /var/www/rjp.baratayuda.abimanyu.a09.com
  rm -rf /var/www/rjp.baratayuda.abimanyu.yyy.com
  ```

## Soal 1 
> Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut


Untuk melakukan testing pengujian apakah sudah sesuai ip kelompok, maka lakukan sebagai berikut pada nodes Pandudewanata,  

```R
ip a
```

Apabila sudah benar, maka akan muncul hasil yang seperti ini

![image](Img/IpA.png)

Bisa kita lihat bahwa IP sudah berjalan pada IP yang sudah kita setting. Langkah selanjutnya adalah mengakses jaringan luar dengan menuliskan kode program dibawah ini. Lalu cek apakah sudah bekerja atau tidak.
 

```R
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.173.0.0/16
  echo 'nameserver 192.168.122.1' > /etc/resolv.conf
``` 

Apabila sudah benar, maka akan muncul hasil yang seperti ini
<br>

![image](Img/con1.png)

Lalu tuliskan masing-masing node dengan kode program dibawah ini untuk melakukan tes ping koneksi dengan ping google.com. 

```R
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

Apabila sudah benar, maka akan seperti ini :

![image](Img/ping1.png)

## Soal 2
> Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

Karena DNS akan hilang saat dihentikan, maka penggunaan script.sh sangat diperlukan agar proses dapat berjalan dengan optimal dan tidak berulang-ulang. Untuk melakukannya, ketikkan nano .bashrc pada web console. Lalu tuliskan kembali langkah sebelumnya dibawah. 

(Contoh pada Client Nakula)

![image](Img/no2.png)

### Script
**Yudhistira**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
echo 'zone "arjuna.D05.com"{  
	type master;
	file "/etc/bind/jarkom/arjuna.D05.com";
};' > /etc/bind/named.conf.local
mkdir /etc/bind/jarkom
cp /etc/bind/db.local /etc/bind/jarkom/arjuna.D05.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.D05.com. root.arjuna.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.D05.com.
@       IN      A       10.24.3.3       ; IP Arjuna 
www     IN      CNAME   arjuna.D05.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom/arjuna.D05.com
service bind9 restart

```

**Nakula dan Sadewa**

Jangan lupa untuk setup nameserver terlebih dahulu yang diarahkan ke `IP Node yudhistira`

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils
echo nameserver 10.24.2.2 > /etc/resolv.conf
ping arjuna.D05.com
```

### Result

![image](/Img/hasilno2.png)



## Soal 3 
> Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

Langkah-langkah yang dilakukan sama dengan yang nomor 2,  akan tetapi ada perbedaan kode program. Tuliskan kode program ini pada node `Yudhistira`

### Script
**Yudhistira**
```
echo 'zone "abimanyu.D05.com"{  
        type master;
        file "/etc/bind/jarkom/abimanyu.D05.com";
};' >> /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.D05.com
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.D05.com. root.abimanyu.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.D05.com.
@       IN      A       10.24.3.4       ; IP Abimanyu 
www     IN      CNAME   abimanyu.D05.com.
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.D05.com
service bind9 restart
```


**Nakula dan Sadewa**
```
echo nameserver 10.24.2.2 > /etc/resolv.conf
host -t CNAME www.abimanyu.D05.com  
ping www.abimanyu.D05.com -c 5
```

### Result

![image](Img/hasilno3.png)



## Soal 4
> Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

Langkah-langkah yang dilakukan sama dengan nomor 3,  akan tetapi ada penambahan subdomain dengan nama parikesit pada kode program. Tuliskan kode program ini pada web console `Yudhistira`

### Script
**Yudhistira**
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.D05.com. root.abimanyu.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.D05.com.
@       IN      A       10.24.3.4       ; IP Abimanyu 
www     IN      CNAME   abimanyu.D05.com.
parikesit   IN  A   10.24.3.4   ; IP Abimanyu
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.D05.com
service bind9 restart
```

**Nakula dan Sadewa**
```
ping parikesit.abimanyu.D05.com
```

### Result

![image](Img/hasilno4.png)



## Soal 5 
> Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

Langkah-langkah selanjutnya adalah menuliskan kembali kode program yang berupa penambahan nama domain reverse dalam `Yudhistira`. Kode program adalah seperti ini

### Script
**Yushistira**
```
echo 'zone "3.24.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/3.24.10.in-addr.arpa";
};' >> /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/3.24.10.in-addr.arpa
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.D05.com. root.abimanyu.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.24.10.in-addr.arpa.   IN  NS   abimanyu.D05.com.   ; IP Abimanyu
4       IN  PTR  abimanyu.D05.com.   ; 
' > /etc/bind/jarkom/3.24.10.in-addr.arpa

service bind9 restart
```

**Nakula dan Sadewa**
```
echo nameserver 10.24.2.2 > /etc/resolv.conf
host -t PTR 10.24.3.4
```

### Result

![image](https://github.com/Caknoooo/simple-django-restful-api/assets/92671053/1d45043b-1756-49a2-9d4a-533547ec6a1c)



## Soal 6 
> Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

Untuk mengerjakan DNS Slave, kita memerlukan beberapa konfigurasi pada `DNS Master` dan `DNS Slave (Werkudara)`

### Script
**Yudhistira**
```
echo 'zone "abimanyu.D05.com" {
    type master;
    notify yes;
    also-notify { 10.24.3.2; };
    allow-transfer { 10.24.3.2; };
    file "/etc/bind/jarkom/abimanyu.D05.com";
};
zone "arjuna.D05.com" {
        type master;
        notify yes;
        also-notify { 10.24.3.2; };
        allow-transfer { 10.24.3.2; };
        file "/etc/bind/jarkom/arjuna.D05.com";
};
zone "3.24.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/3.24.10.in-addr.arpa";
};' > /etc/bind/named.conf.local
service bind9 restart
service bind9 stop
```

**Werkudara**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
echo 'zone "abimanyu.D05.com" {
    type slave;
    masters { 10.24.2.2; }; // Masukan IP DNS Master
    file "/var/lib/bind/abimanyu.D05.com";
};' > /etc/bind/named.conf.local
service bind9 restart
```

**Nakula dan Sadewa**
```
echo nameserver 10.24.2.2 > /etc/resolv.conf
host -t PTR 10.24.3.4
```

### Result

![image](Img/hasilno6.png)


## Soal 7 
> Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

Untuk melakukan Delegasi subdomain,  kita memerlukan beberapa configurasi pada DNS Master dan DNS Slave. Kita juga meemerlukan beberapa modifikasi pada `/etc/bind/named.conf.options`

### Script
**Yudhistira**
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.D05.com. root.abimanyu.D05.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.D05.com.
@       IN      A       10.24.3.4       ; IP DNS Master
www     IN      CNAME   abimanyu.D05.com.
parikesit       IN      A       10.24.3.4       ; IP abimanyu
ns1     IN      A       10.24.3.2 ; IP werkudara
baratayuda      IN      NS      ns1
@       IN      AAAA    ::1' > /etc/bind/jarkom/abimanyu.D05.com

echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options

service bind9 restart
```

**Werkudara**
```
echo 'options {
        directory "/var/cache/bind";
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
mkdir /etc/bind/baratayuda
cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.D05.com
echo 'zone "abimanyu.D05.com" {
    type slave;
    masters { 10.24.2.2; }; // Masukan IP DNS Master
    file "/var/lib/bind/abimanyu.D05.com";
};
zone "baratayuda.abimanyu.D05.com" {
    type master;
    file "/etc/bind/baratayuda/baratayuda.abimanyu.D05.com";
};' > /etc/bind/named.conf.local
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.D05.com. root.baratayuda.abimanyu.D05.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.D05.com.
@       IN      A       10.24.3.2       ; IP werkudara
www     IN      CNAME   baratayuda.abimanyu.D05.com.
@       IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.D05.com
service bind9 restart

```

**Nakula dan Sadewa**
```
ping baratayudha.abimanyu.D05.com -c 5
```

### Result

![image](Img/hasilno7.png)


## Soal 8 
> Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

Karena kita sudah melakukan Delegasi subdomain, langkah selanjutnya adalah melakukan subdomain melalui `Werkudara` dengan perlu memerlukan beberapa konfigurasi pada DNS Master dan `DNS Slave`. 

### Script
**Werkudara**
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.D05.com. root.baratayuda.abimanyu.D05.com. (
                                2       ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.D05.com.
@       IN      A       10.24.3.2       ; IP werkudara
www     IN      CNAME   baratayuda.abimanyu.D05.com.
rjp     IN      A       10.24.3.4       ; IP abimanyu
www.rjp  IN     CNAME   rjp.baratayuda.abimanyu.D05.com.
@       IN      AAAA    ::1' > /etc/bind/baratayuda/baratayuda.abimanyu.D05.com
service bind9 restart
```

**Nakula dan Sadewa**
```
ping www.rjp.baratayudha.abimanyu.D05.com -c 5
```

### Result

![image](Img/hasilno8.png)


## Soal 9 
> Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

Langkah selanjutnya adalah dengan melakukan pengecekan konfigurasi dengan benar pada Nginx dan aturan load balancing yang sesuai. Disini, kita bisa menggunakan algoritma yang sesuai untuk konfigurasinya. Selanjutnya, adalah melakukan load balancing pada masing-masing worker dengan mengunggah aplikasi atau layanan web. Bila konfigurasi dan deploymenet telah selesai dilakukan, maka `Arjuna` akan bertindak sebagai load balancer yang akan mendistribusikan lalu lintas web ke worker yang tersedia

### Script
**Arjuna sebagai Load Balancer**
```
echo 'upstream backend {
  server 10.24.3.5; # IP PrabuKusuma
  server 10.24.3.4; # IP Abimanyu
  server 10.24.3.6; # IP Wisanggeni
}

server {
  listen 80;
  server_name arjuna.D05.com www.arjuna.D05.com;

  location / {
    proxy_pass http://backend;
  }
}
' > /etc/nginx/sites-available/jarkom
```

Lalu sambungkan load balancing dengan menggunakan perintah `symlink`

```
ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom
```

Lalu, lakukan penghapusan file default agar port tidak bertabrakan saat melakukan instalasi nginx

```
rm /etc/nginx/sites-enabled/default
```

Lalu lakukan restart 

```
service nginx restart
```

**Prabukusuma, Abimanyu, Wisanggeni**

Jalankan program dengan perintah `shell`

```
service php7.0-fpm start

echo 'server {
        listen 80;

        root /var/www/jarkom;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
```

**Nakula dan Sadewa**

Lakukan testing pada client dengan menggunakan kode berikut

```
lynx http://10.24.3.5
lynx http://10.24.3.4
lynx http://10.24.3.6
lynx http://arjuna.D05.com
```

### Result
![image](Img/hasilno10.png)



## Soal 10 
> Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

Sebelum mengerjakan perlu untuk melakukan [setting](#sebelum-memulai) terlebih dahulu. Disini kita perlu melakukan testing terhadap semua node yang ada. Disini kami melakukan testing pada client nakula

### Script
**Arjuna sebagai Load Balancer**

```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 nginx -y
cd /etc/nginx/sites-available
echo ' upstream arjuna.D05  {
        server 10.24.3.5:8001; #IP Prabakusuma
        server 10.24.3.4:8002; #IP Abimanyu
        server 10.24.3.6:8003; #IP Wisanggeni
 }

 server {
        listen 80;
        server_name arjuna.D05.com;

        location / {
        proxy_pass http://arjuna.D05;
        }
 }' > lb-arjuna.D05
ln -s /etc/nginx/sites-available/lb-arjuna.D05 /etc/nginx/sites-enabled
service nginx restart```

### Script
**Abimanyu, Prabakusuma, dan Wisanggeni sebagai WebServer**
```
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update && apt install nginx php php-fpm -y
apt install wget -y
apt-get install unzip -y
mkdir /var/www
wget 'https://drive.google.com/uc?export=download&id=17tAM_XDKYWDvF-JJix1x7txvTBEax7vX' -O arjuna.yyy.com.zip
unzip arjuna.yyy.com.zip
mv arjuna.yyy.com arjuna.D05
mv arjuna.D05 /var/www
echo 'server {

        listen 8001;

        root /var/www/arjuna.D05;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        }

 location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/arjuna.D05.log;
        access_log /var/log/nginx/arjuna.D05_access.log;
 }' > arjuna.D05
ln -s /etc/nginx/sites-available/arjuna.D05 /etc/nginx/sites-enabled
service php7.0-fpm start
service nginx restart
```
### Script
**Nakula**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils
apt-get install lynx
echo 'nameserver 10.24.2.2 # IP DNS Master
nameserver 10.24.3.2 # IP DNS Slave' > /etc/resolv.conf
```

### Result
![image](Img/hasilno10.png)

## Soal 11
> Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

Sebelum mengerjakan perlu untuk melakukan [setting](#sebelum-memulai) terlebih dahulu. Disini kita perlu melakukan testing terhadap semua node yang ada. Disini kami melakukan testing pada client 'Nakula'

### Script
**Abimanyu sebagai WebServer**
```
cd /etc/apache2/sites-available
cp 000-default.conf abimanyu.D05.com.conf
echo "<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.D05
        ServerName abimanyu.D05.com
        ServerAlias www.abimanyu.D05.com
        ServerAlias 10.24.3.4
        <Directory /var/www/abimanyu.D05/index.php/home>
         Options +Indexes
        </Directory>

        Alias '/home' '/var/www/abimanyu.D05/index.php/home'

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>" > abimanyu.D05.com.conf
a2ensite abimanyu.D05.com
wget 'https://drive.google.com/uc?export=download&id=1a4V23hwK9S7hQEDEcv9FL14UkkrHc-Zc' -O abimanyu.yyy.com.zip
unzip abimanyu.yyy.com.zip
mv abimanyu.yyy.com abimanyu.D05
mv abimanyu.D05 /var/www
rm -r abimanyu.yyy.com.zip
service apache2 restart
```

### Result
![image](Img/hasilno11.png)


## Soal 12 
> Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

Untuk menyelesaikan soal tersebut kita perlu untuk melakukan metode alias pada abimanyu WebServer. 


### Script
**Abimanyu WebServer**
```
Alias '/home' '/var/www/abimanyu.D05/index.php/home'
```

### Result
![image](Img/hasilno11.png)


## Soal 13, 14, 15, 16 
> Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
> Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).
> Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.
> Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js

Untuk menyelesaikan soal tersebut kita perlu menambah beberapa script pada Abimanyu WebServer. Agar directory /secret tidak dapat diakses kita perlu menambahkan kode berikut `<Directory /var/www/parikesit.abimanyu.D05/secret>
        Options -Indexes
        Deny from all
        </Directory>`, lalu untuk merubah kode eror menjadi custom, kita perlu menambahkan line berikut `ErrorDocument 404 /error/404.html`
        `ErrorDocument 403 /error/403.html` pada file .conf. Lalu agar dapat diakses hanya dengan /js kita dapat mengubahnya menggunakan alias `Alias "/js" "/var/www/parikesit.abimanyu.D05/public/js"`

### Script
**Abimanyu WebServer**
```
cp 000-default.conf parikesit.abimanyu.D05.com
echo "<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/parikesit.abimanyu.D05
        ServerName parikesit.abimanyu.D05.com
        ServerAlias www.parikesit.abimanyu.D05.com
        ErrorDocument 404 /error/404.html
        ErrorDocument 403 /error/403.html
        <Directory /var/www/parikesit.abimanyu.D05/public>
        Options +Indexes
        </Directory>
        <Directory /var/www/parikesit.abimanyu.D05/secret>
        Options -Indexes
        Deny from all
        </Directory>
        <Directory /var/www/parikesit.abimanyu.D05/public>
                Options +Indexes
        </Directory>

        Alias "/js" "/var/www/parikesit.abimanyu.D05/public/js"
        <Directory /var/www/parikesit.abimanyu.D05>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog /error.log
        CustomLog /access.log combined

        <Directory /var/www/parikesit.abimanyu.D05>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with a2disconf.
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>" > parikesit.abimanyu.D05.com.conf
a2ensite parikesit.abimanyu.D05.com
a2enmod rewrite
wget 'https://drive.google.com/uc?export=download&id=1LdbYntiYVF_NVNgJis1GLCLPEGyIOreS' -O parikesit.abimanyu.yyy.com.zip
unzip parikesit.abimanyu.yyy.com.zip
mv parikesit.abimanyu.yyy.com parikesit.abimanyu.D05
mv parikesit.abimanyu.D05 /var/www
rm -r parikesit.abimanyu.yyy.com.zip
```

### Result
![image](Img/hasilno13.png)  
![image](Img/hasilno13b.png)


## Soal 17 dan 18 
> Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.
> Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

Untuk menyelesaikan soal tersebut kita perlu membuat script pada file `rjp.baratayuda.abimanyu.D05.com.conf` dengan memasukkan code berikut `<VirtualHost *:14000 *:14400>` dan mengubah file port.conf menjadi seperti di bawah ini. Lalu untuk mengamankan ketika mengakses sehingga harus menggunakan username dan password kita perlu menambahakan line berikut `<Directory '/var/www/rjp.baratayuda.abimanyu.D05'>
                AuthType Basic
                AuthName 'Restricted Content'
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>` pada file `rjp.baratayuda.D05.com.conf` 


### Script
**Abimanyu**
```
cd /etc/apache2/sites-available
cp 000-default.conf rjp.baratayuda.abimanyu.D05.com.conf
echo "<VirtualHost *:14000 *:14400>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.D05
        ServerName rjp.baratayuda.abimanyu.D05.com
        ServerAlias www.rjp.baratayuda.abimanyu.D05.com

        <Directory '/var/www/rjp.baratayuda.abimanyu.D05'>
                AuthType Basic
                AuthName 'Restricted Content'
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>"> rjp.baratayuda.abimanyu.D05.com.conf
a2ensite rjp.baratayuda.abimanyu.D05.com
echo 'Listen 80
Listen 14000
Listen 14400
<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>' > /etc/apache2/ports.conf
wget 'https://drive.google.com/uc?export=download&id=1pPSP7yIR05JhSFG67RVzgkb-VcW9vQO6' -O rjp.baratayuda.abimanyu.yyy.com.zip
unzip rjp.baratayuda.abimanyu.yyy.com.zip
mv rjp.baratayuda.abimanyu.yyy.com rjp.baratayuda.abimanyu.D05
mv rjp.baratayuda.abimanyu.D05 /var/www
rm -r rjp.baratayuda.abimanyu.yyy.com.zip
htpasswd -c -b /etc/apache2/.htpasswd Wayang baratayudad05
service apache2 restart
```

### Result
![image](Img/hasilno17.png)  
![image](Img/hasilno18.png)  


## Soal 19 
> Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

Untuk menyelasaikan soal tersebut kita perlu menambahkan konfigurasi server alias seperti berikut `ServerAlias 10.24.3.4` pada file abimanyu.D05.com.conf 

### Script
**Abimanyu**
```
 ServerAdmin webmaster@localhost
        DocumentRoot /var/www/abimanyu.D05
        ServerName abimanyu.D05.com
        ServerAlias www.abimanyu.D05.com
        ServerAlias 10.24.3.4
```

### Result
![image](Img/19.png)  
![image](Img/hasilno11.png)  

## Soal 20 
> Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

Untuk menyelesaikan soal tersebut kita perlu membuat file .htaccess pada folder `/var/www/parikesit.abimanyuu.D05` dengan isi script di bawah ini 

### Script
**Abimanyu**
```
echo 'RewriteEngine On
RewriteCond %{REQUEST_URI} !^/public/images/abimanyu.png
RewriteCond %{REQUEST_URI} abimanyu
RewriteRule \.(jpg|jpeg|png)$ /public/images/abimanyu.png [L]'
```

### Result
![image](Img/hasilno20.png)
