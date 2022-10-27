# Jarkom-Modul-2-B13-2022

### Kelompok B13
| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Amsal Herbert  | 5025201182 | 
| 2 | Elbert Dicky Aristyo | 5025201231 |
| 3 | Nathanael Roviery | 5025201258 |

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
Setelah itu ia juga ingin membuat subdomain **eden.wise.yyy.com** dengan alias **www.eden.wise.yyy.com** yang diatur DNS-nya di WISE dan mengarah ke Eden

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
  