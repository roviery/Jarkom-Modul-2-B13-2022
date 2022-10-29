# Jarkom-Modul-2-B13-2022

### Kelompok B13
| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Amsal Herbert  | 5025201182 | 
| 2 | Elbert Dicky Aristyo | 5025201231 |
| 3 | Nathanael Roviery | 5025201258 |

- [Network Configuration](#network-configuration)
- [Package Installation (Starter)](#package-installation-starter)
- [Soal 1](#soal-1)
- [Soal 2](#soal-2)
- [Soal 3](#soal-3)
- [Soal 4](#soal-4)
- [Soal 5](#soal-5)
- [Soal 6](#soal-6)
- [Soal 7](#soal-7)
- [Soal 8](#soal-8)
- [Soal 9](#soal-9)
- [Soal 10](#soal-10)
- [Soal 11](#soal-11)
- [Soal 12](#soal-12)
- [Soal 13](#soal-13)
- [Soal 14](#soal-14)
- [Soal 15](#soal-15)
- [Soal 16](#soal-16)
- [Soal 17](#soal-17)

## Network Configuration
### **Ostania**
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address  192.179.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.179.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.179.3.1
	netmask 255.255.255.0
```
### **WISE**
```
auto eth0
iface eth0 inet static
	address 192.179.3.2
	netmask 255.255.255.0
	gateway 192.179.3.1
```
### **Berlint**
```
auto eth0
iface eth0 inet static
	address 192.179.2.2
	netmask 255.255.255.0
	gateway 192.179.2.1
```
### **Eden**
```
auto eth0
iface eth0 inet static
	address 192.179.2.3
	netmask 255.255.255.0
	gateway 192.179.2.1
```
### **SSS**
```
auto eth0
iface eth0 inet static
	address 192.179.1.2
	netmask 255.255.255.0
	gateway 192.179.1.1
```
### **Garden**
```
auto eth0
iface eth0 inet static
	address 192.179.1.3
	netmask 255.255.255.0
	gateway 192.179.1.1
```
## Package Installation (Starter) 
### **Ostania**
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.179.0.0/16
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
```
### **WISE** & **Berlint**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```
### **Eden**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get install wget
apt-get install unzip
apt-get install apache2
apt-get install apache2 apache2-utils
apt-get install php
apt-get install libapache2-mod-php7.0
service apache2 start
```
### **SSS** & **GARDEN**
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils -y
apt-get install lynx -y
```

## Opening
Twilight (〈黄昏 (たそがれ) 〉, <*Tasogare*>) adalah seorang mata-mata yang berasal dari negara Westalis. Demi menjaga perdamaian antara Westalis dengan Ostania, Twilight dengan nama samaran Loid Forger (ロイド・フォージャー, Roido Fōjā) di bawah organisasi WISE menjalankan operasinya di negara Ostania dengan cara melakukan spionase, sabotase, penyadapan dan kemungkinan pembunuhan. Berikut adalah peta dari negara Ostania:

## Soal 1
WISE akan dijadikan sebagai DNS Master, Berlint akan dijadikan DNS Slave, dan Eden akan digunakan sebagai Web Server. Terdapat 2 Client yaitu SSS, dan Garden. Semua node terhubung pada router Ostania, sehingga dapat mengakses internet.

Penyelesaian:
- Mengatur semua network configuration
- Masukkan command pada router Ostania
  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16
  ```
- Menambahkan nameserver pada file /etc/resolv.conf di setiap node (Ostania, WISE, Berlint, Eden, SSS, Garden) 
  ```
  nameserver 192.168.122.1
  ``` 
- Hasil 

  ![soal-1-1](https://cdn.discordapp.com/attachments/818146232689098802/1035216888382365777/unknown.png) 

## Soal 2
Untuk mempermudah mendapatkan informasi mengenai misi dari Handler, bantulah Loid membuat website utama dengan akses wise.yyy.com dengan alias www.wise.yyy.com pada folder wise

Penyelesaian:
- Pada file */etc/bind/named.conf.local* tambahkan:
  ```
  zone "wise.b13.com" {
          type master;
          file "/etc/bind/b13/wise.b13.com";
  };
  ```
- Buat direktori */etc/bind/b13* dengan command:  
  ```
  mkdir /etc/bind/b13
  ```
- Buat file *wise.b13.com* di folder */etc/bind/b13* dan isi file berikut dengan isian sebagai berikut
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     wise.b13.com. root.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      wise.b13.com.
  @       IN      A       192.179.3.2     ; IP WISE
  www     IN      CNAME   wise.b13.com.
  @       IN      AAAA    ::1
  ```
- Untuk proses testing, ubah nameserver client menjadi IP Wise dan lakukan command sebagai berikut
  ```
  ping wise.b13.com -c 5
  host -t CNAME www.wise.b13.com
  ```
- Restart service bind9 pada WISE
  ```
  service bind9 restart
  ```
- Hasil 

  ![soal-2-1](https://cdn.discordapp.com/attachments/818146232689098802/1035221183802658957/unknown.png)


## Soal 3
Setelah itu ia juga ingin membuat subdomain **eden.wise.yyy.com** dengan alias **www.eden.wise.yyy.com** yang diatur DNS-nya di WISE dan mengarah ke Eden.

Penyelesaian:
- Menambahkan Eden sebagai subdomain di file */etc/bind/b13/wise.b13.com*
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     wise.b13.com. root.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @               IN      NS      wise.b13.com.
  @               IN      A       192.179.3.2             ; IP WISE
  www             IN      CNAME   wise.b13.com.
  eden            IN      A       192.179.2.3             ; IP Eden
  @               IN      AAAA    ::1
  ```
- Membuat zone baru untuk penamaan alias **eden.wise.b13.com** di */etc/bind/named.conf.local* sehingga */etc/bind/named.conf.local* menjadi seperti berikut
  ```
  zone "wise.b13.com" {
        type master;
        file "/etc/bind/b13/wise.b13.com";
  };

  zone "eden.wise.b13.com" {
          type master;
          file "/etc/bind/b13/eden.wise.b13.com";
  };
  ```
- Buat file baru *eden.wise.b13.com* di folder */etc/bind/b13* dan isi dengan isian sebagai berikut
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     eden.wise.b13.com. root.eden.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @               IN      NS      eden.wise.b13.com.
  @               IN      A       192.179.2.3             ; IP Eden
  www             IN      CNAME   eden.wise.b13.com.
  @               IN      AAAA    ::1
  ```
- Restart service bind9 pada WISE
  ```
  service bind9 restart
  ```
- Hasil 

  ![soal3-1](https://cdn.discordapp.com/attachments/818146232689098802/1035223725701857360/unknown.png)

## Soal 4
Buat juga reverse domain untuk domain utama

Penyelesaian:
- Menambahkan zone baru sehingga file */etc/bind/named.conf.local* menjadi seperti berikut
  ```
  zone "wise.b13.com" {
        type master;
        file "/etc/bind/b13/wise.b13.com";
  };

  zone "eden.wise.b13.com" {
          type master;
          file "/etc/bind/b13/eden.wise.b13.com";
  };

  zone "3.179.192.in-addr.arpa" {
          type master;
          file "/etc/bind/b13/3.179.192.in-addr.arpa";
  };
  ```
  3.179.192 merupakan 3 bytes pertama dari WISE yang di-*reverse* (192.179.3.2) 

- Membuat file */etc/bind/b13/3.179.192.in-addr.arpa* dengan isian sebagai berikut
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     wise.b13.com. root.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  3.179.192.in-addr.arpa.         IN      NS      wise.b13.com.
  2                               IN      PTR     wise.b13.com.   ; Byte ke-4 WISE
  ```
- Restart service bind9 pada WISE
  ```
  service bind9 restart
  ```
- Hasil 

  ![Soal-4-1](https://cdn.discordapp.com/attachments/818146232689098802/1035226116509683712/unknown.png)

## Soal 5
Agar dapat tetap dihubungi jika server WISE bermasalah, buatlah juga Berlint sebagai DNS Slave untuk domain utama

Penyelesaian:
  - Ubah */etc/bind/named.conf.local* pada WISE bagian menjadi seperti berikut
    ```
    zone "wise.b13.com" {
          type master;
          notify yes;
          also-notify { 192.179.2.2; }; // IP Berlint
          allow-transfer { 192.179.2.2; }; // IP Berlint
          file "/etc/bind/b13/wise.b13.com";
    };

    zone "eden.wise.b13.com" {
            type master;
            notify yes;
            also-notify { 192.179.2.2; }; // IP Berlint
            allow-transfer { 192.179.2.2; }; // IP Berlint
            file "/etc/bind/b13/eden.wise.b13.com";
    };

    zone "3.179.192.in-addr.arpa" {
            type master;
            file "/etc/bind/b13/3.179.192.in-addr.arpa";
    };
    ```
  - Pada node Berlint, isi file */etc/bind/named.conf.local* dengan isian sebagai berikut
    ```
    zone "wise.b13.com" {
        type slave;
        masters { 192.179.3.2; }; // IP WISE
        file "/var/lib/bind/wise.b13.com";
    };
    ```
  - Restart bind9 di node WISE dan Berlint
    ```
    service bind9 restart
    ```
  - Matikan bind9 di node WISE
    ```
    service bind9 stop
    ```
  - Tambahkan nameserver Berlint pada client
    ```
    nameserver 192.179.3.2
    nameserver 192.179.2.2
    ```
  - Hasil 

    ![soal-5-1](https://cdn.discordapp.com/attachments/818146232689098802/1035229726626820228/unknown.png) 

    ![soal-5-2](https://cdn.discordapp.com/attachments/818146232689098802/1035229951890292807/unknown.png)

## Soal 6
Karena banyak informasi dari Handler, buatlah subdomain yang khusus untuk operation yaitu **operation.wise.yyy.com** dengan alias **www.operation.wise.yyy.com** yang didelegasikan dari WISE ke Berlint dengan IP menuju ke Eden dalam folder operation

Penyelesaian:
- Menambahkan delegasi operation di */etc/bind/b13/wise.b13.com* pada node WISE
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     wise.b13.com. root.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @               IN      NS      wise.b13.com.
  @               IN      A       192.179.3.2             ; IP WISE
  www             IN      CNAME   wise.b13.com.
  eden            IN      A       192.179.2.3             ; IP Eden
  ns1             IN      A       192.179.2.2             ; IP Berlint
  operation       IN      NS      ns1
  @               IN      AAAA    ::1
  ```
- Menambahkan zone di */etc/bind/named.conf.local* pada node Berlint
  ```
  zone "wise.b13.com" {
          type slave;
          masters { 192.179.3.2; }; // IP WISE
          file "/var/lib/bind/wise.b13.com";
  };

  zone "operation.wise.b13.com" {
          type master;
          file "/etc/bind/operation/operation.wise.b13.com";
  };
  ```
- Membuat folder */etc/bind/operation* di Berlint
  ```
  mkdir /etc/bind/operation
  ```
- Membuat file *operation.wise.b13.com* di folder */etc/bind/operation* dan isi dengan isian seperti berikut
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     operation.wise.b13.com. root.operation.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      operation.wise.b13.com.
  @       IN      A       192.179.2.3             ; IP Eden
  www     IN      CNAME   operation.wise.b13.com.
  @       IN      AAAA    ::1
  ```
- Hasil 

  ![soal-6-1](https://cdn.discordapp.com/attachments/818146232689098802/1035505163202465873/unknown.png)

## Soal 7
Untuk informasi yang lebih spesifik mengenai Operation Strix, buatlah subdomain melalui Berlint dengan akses **strix.operation.wise.yyy.com** dengan alias **www.strix.operation.wise.yyy.com** yang mengarah ke Eden

Penyelesaian:
- Menambahkan subdomain di */etc/bind/operation/operation.wise.b13.com* pada node Berlint sehingga seperti berikut
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     operation.wise.b13.com. root.operation.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      operation.wise.b13.com.
  @       IN      A       192.179.2.3             ; IP Eden
  strix   IN      A       192.179.2.3             ; IP Eden
  @       IN      AAAA    ::1
  ```
- Membuat zone baru di */etc/bind/named.conf.local*
  ```
  zone "wise.b13.com" {
          type slave;
          masters { 192.179.3.2; }; // IP WISE
          file "/var/lib/bind/wise.b13.com";
  };

  zone "operation.wise.b13.com" {
          type master;
          file "/etc/bind/operation/operation.wise.b13.com";
  };

  zone "strix.operation.wise.b13.com" {
          type master;
          file "/etc/bind/operation/strix.operation.wise.b13.com";
  };
  ```
- Menambuat file *strix.operation.wise.b13.com* di folder */etc/bind/operation* dan menambahkan alias
  ```
  ;
  ; BIND data file for local loopback interface
  ;
  $TTL    604800
  @       IN      SOA     strix.operation.wise.b13.com. root.strix.operation.wise.b13.com. (
                      2022102401         ; Serial
                          604800         ; Refresh
                            86400         ; Retry
                          2419200         ; Expire
                          604800 )       ; Negative Cache TTL
  ;
  @       IN      NS      strix.operation.wise.b13.com.
  @       IN      A       192.179.2.3             ; IP Eden
  www     IN      CNAME   strix.operation.wise.b13.com.
  @       IN      AAAA    ::1
  ```
- Hasil 

  ![soal-7-1](https://cdn.discordapp.com/attachments/818146232689098802/1035860064193495071/unknown.png)

## Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver **www.wise.yyy.com**. Pertama, Loid membutuhkan webserver dengan DocumentRoot pada /var/www/wise.yyy.com 

Penyelesaian:
- Ubah Record A dan PTR pada wise.b13.com mengarah ke Eden
- Membuat folder baru */var/www/wise.b13.com* di node Eden
  ```
  mkdir /var/www/wise.b13.com
  ```
- Memasukan Resource **wise.b13.com**
- Membuat file konfigurasi *wise.b13.com.conf* di folder */etc/apache2/sites-available* dan isi dengan isian seperti berikut
  ```
  <VirtualHost *:80>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/wise.b13.com
          ServerName wise.b13.com
          ServerAlias www.wise.b13.com

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
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Enable sites
  ```
  a2ensite wise.b13.com
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Buka melalui client
  ```
  lynx wise.b13.com
  ```
- Hasil 

  ![soal-8-1](https://cdn.discordapp.com/attachments/818146232689098802/1035863178787172435/unknown.png)

## Soal 9
Setelah itu, Loid juga membutuhkan agar url www.wise.yyy.com/index.php/home dapat menjadi menjadi www.wise.yyy.com/home

Penyelesaian:
- Menambahkan alias pada /etc/apache2/sites-available/wise.b13.com
  ```
  Alias "/home" "/var/www/wise.b13.com/index.php/home"
  ```
- Restart service apache2
  ```
  service apache2 restart
  ```
- Buka melalui client
  ```
  lynx wise.b13.com/home
  ```
- Hasil 

  ![soal-9-1](https://cdn.discordapp.com/attachments/818146232689098802/1035863178787172435/unknown.png)

## Soal 10
Setelah itu, pada subdomain **www.eden.wise.yyy.com**, Loid membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/eden.wise.yyy.com

Penyelesaian:
- Membuat folder */var/www/eden.wise.b13.com*
  ```
  mkdir /var/www/eden.wise.b13.com
  ```
- Memasukkan resource eden.wise ke folder tersebut
- Melakukan konfigurasi web server dengan membuat file */etc/apache2/sites-available/eden.wise.b13.com.conf* dan isi dengan isian berikut
  ```
  <VirtualHost *:80>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/eden.wise.b13.com
          ServerName eden.wise.b13.com
          ServerAlias www.eden.wise.b13.com

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
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Enable sites
  ```
  a2ensite eden.wise.b13.com
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Hasil 

  ![soal-10-1](https://cdn.discordapp.com/attachments/818146232689098802/1035865904011034675/unknown.png)

## Soal 11
Akan tetapi, pada folder /public, Loid ingin hanya dapat melakukan directory listing saja

Penyelesaian:
- Masukan directory listing pada folder /public
  ```
  <Directory /var/www/eden.wise.b13.com/public>
          Options +Indexes
  </Directory>
  ```
- Restart service apache
  ```
  service apache2 restart
  ```

## Soal 12
Tidak hanya itu, Loid juga ingin menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache

Penyelesaian:
- Menambahkan ErrorDocument pada *eden.wise.b13.com.conf*
  ```
  ErrorDocument 404 /error/404.html
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Hasil 

  ![soal-12-1](https://cdn.discordapp.com/attachments/818146232689098802/1035866742313992242/unknown.png)

## Soal 13
Loid juga meminta Franky untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset **www.eden.wise.yyy.com/public/js** menjadi **www.eden.wise.yyy.com/js**

Penyelesaian:
- Menambahkan alias pada file */etc/apache2/sites-available/eden.wise.b13.com.conf* sehingga menjadi seperti berikut
  ```
  <VirtualHost *:80>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/eden.wise.b13.com
          ServerName eden.wise.b13.com
          ServerAlias www.eden.wise.b13.com

          <Directory /var/www/eden.wise.b13.com/public>
                  Options +Indexes
          </Directory>

          Alias "/js" "/var/www/eden.wise.b13.com/public/js"

          # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
          # error, crit, alert, emerg.
          # It is also possible to configure the loglevel for particular
          # modules, e.g.
          #LogLevel info ssl:warn

          ErrorLog ${APACHE_LOG_DIR}/error.log
          CustomLog ${APACHE_LOG_DIR}/access.log combined

          ErrorDocument 404 /error/404.html

          # For most configuration files from conf-available/, which are
          # enabled or disabled at a global level, it is possible to
          # include a line for only one particular virtual host. For example the
          # following line enables the CGI configuration for this host only
          # after it has been globally disabled with "a2disconf".
          #Include conf-available/serve-cgi-bin.conf
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Buka melalui client dengan command
  ```
  lynx www.eden.wise.yyy.com/js
  ```
- Hasil 

  ![soal-13-1](https://cdn.discordapp.com/attachments/818146232689098802/1035517364550242314/unknown.png)

## Soal 14
Loid meminta agar www.strix.operation.wise.yyy.com hanya bisa diakses dengan port 15000 dan port 15500

Penyelesaian:
- Membuat file *strix.operation.wise.b13.com.conf* di folder */etc/apache2/sites-available* dengan isian sebagai berikut
  ```
  <VirtualHost *:15000 *:15500>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/strix.operation.wise.b13.com
          ServerName strix.operation.wise.b13.com
          ServerAlias www.strix.operation.eden.wise.b13.com


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
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Menambahkan Listen 15000 dan Listen 15500 di */etc/apache2/ports.conf* sehingga menjadi seperti berikut 

  ```
  # If you just change the port or add more ports here, you will likely also
  # have to change the VirtualHost statement in
  # /etc/apache2/sites-enabled/000-default.conf

  Listen 80
  Listen 15000
  Listen 15500

  <IfModule ssl_module>
          Listen 443
  </IfModule>

  <IfModule mod_gnutls.c>
          Listen 443
  </IfModule>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Membuat folder baru di /var/www/strix.operation.wise.b13.com 
  ```
  mkdir /var/www/strix.operation.wise.b13.com
  ```
- Memasukan resource *strix.operation.wise* yang telah disediakan ke folder */var/www/strix.operation.b13.com*
  ```
  cp -R strix.operation.wise/* /var/www/strix.operation.wise.b13.com
  ```
- Enable sites **stri.operation.wise.b13.com**
  ```
  a2ensite strix.operation.wise.b13.com
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Hasil 

  ![soal-14-1](https://cdn.discordapp.com/attachments/818146232689098802/1035510320774529094/unknown.png)

## Soal 15
dengan autentikasi username Twilight dan password opStrix dan file di /var/www/strix.operation.wise.yyy

Penyelesaian:
- Buat file *.htpasswd* di */etc/apache2/.htpasswd* dan isi dengan password opStrix
  ```
  htpasswd -c /etc/apache2/.htpasswd Twilight
  ```
- Sehingga hasilnya seperti ini
  ![soal-15-1](https://cdn.discordapp.com/attachments/818146232689098802/1035512116335104030/unknown.png)
- Lalu tambahkan Auth untuk directory */var/www/strix.operation.wise.b13.com* di file */etc/apache2/sites-available/strix.operation.wise.b13.com.conf* sehingga menjadi seperti berikut 

  ```
  <VirtualHost *:15000 *:15500>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/strix.operation.wise.b13.com
          ServerName strix.operation.wise.b13.com
          ServerAlias www.strix.operation.eden.wise.b13.com

          <Directory "/var/www/strix.operation.wise.b13.com">
                  AuthType Basic
                  AuthName "Restricted Content"
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
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Hasil
  - Proses Input Username
  ![soal-15-1](https://cdn.discordapp.com/attachments/818146232689098802/1035514418307539014/unknown.png)
  - Proses Input Password
  ![soal-15-2](https://cdn.discordapp.com/attachments/818146232689098802/1035514483688345621/unknown.png)
  - Hasil setelah berhasil melewati authentikasi
  ![soal-15-3](https://cdn.discordapp.com/attachments/818146232689098802/1035514546946834473/unknown.png)

## Soal 16
dan setiap kali mengakses IP Eden akan dialihkan secara otomatis ke www.wise.yyy.com

Penyelesaian:
- Mengatur DocumentRoot 000-default.conf agar mengarah ke */var/www/wise.b13.com* sehingga menjadi seperti berikut
  ```
  <VirtualHost *:80>
          # The ServerName directive sets the request scheme, hostname and port that
          # the server uses to identify itself. This is used when creating
          # redirection URLs. In the context of virtual hosts, the ServerName
          # specifies what hostname must appear in the request's Host: header to
          # match this virtual host. For the default virtual host (this file) this
          # value is not decisive as it is used as a last resort host regardless.
          # However, you must set it for any further virtual host explicitly.
          #ServerName www.example.com

          ServerAdmin webmaster@localhost
          DocumentRoot /var/www/wise.b13.com

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
  </VirtualHost>

  # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Akses IP Eden melalui client dengan command berikut
  ```
  lynx 192.179.2.3
  ```
- Hasil 

  ![soal-16-1](https://cdn.discordapp.com/attachments/818146232689098802/1035520483979956234/unknown.png)

## Soal 17
Karena website www.eden.wise.yyy.com semakin banyak pengunjung dan banyak modifikasi sehingga banyak gambar-gambar yang random, maka Loid ingin mengubah request gambar yang memiliki substring “eden” akan diarahkan menuju eden.png. Bantulah Agent Twilight dan Organisasi WISE menjaga perdamaian! 

Penyelesaian:
- Membuat file *.htaccess* pada folder */var/www/eden.wise.b13.com* dan isi dengan RewriteRule seperti berikut
  ```
  RewriteEngine On
  RewriteCond %{REQUEST_URI} !^/public/images/eden.png$
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule ^(.*)eden(.*)$ /public/images/eden.png [R=301,L]
  ```
- Menambahkan Directory /var/www/eden.wise.b13.com pada file /etc/apache2/sites-available/wise.b13.com.conf sehingga seperti berikut
  ```
  <Directory /var/www/eden.wise.b13.com>
          Options +FollowSymLinks -Multiviews
          AllowOverride All
  </Directory>
  ```
- Restart service apache
  ```
  service apache2 restart
  ```
- Tes buka melalui client dengan command berikut
  ```
  lynx www.eden.wise.b13.com/awodijadedenaiwodj
  ```
- Hasil 

  ![soal-17-1](https://cdn.discordapp.com/attachments/818146232689098802/1035853131520679936/unknown.png)
  
  Tekan "D" untuk *download* 

  ![soal-17-2](https://cdn.discordapp.com/attachments/818146232689098802/1035853308247691314/unknown.png)

  Ketik **eden.png** sebagai nama output file yang akan di download

  ![soal-17-3](https://cdn.discordapp.com/attachments/818146232689098802/1035853732895784960/unknown.png)

  Sehingga diperoleh hasil seperti berikut

  ![soal-17-4](https://cdn.discordapp.com/attachments/818146232689098802/1035854135834193920/unknown.png)
