# Jarkom_modul3_praktikum_C12

## Kelompok C12
* 05111840000146 - [I Kadek Ricky Suirta](https://github.com/riclown)
* 05111840000162 - [Fransiskus Xaverius Kevin Koesnadi](https://github.com/fxkevink)

## Daftar Isi
* [Soal DHCP](#dhcp-soal-1---6)
* [Soal 1](#soal-1)
* [Soal 2](#soal-2)
* [Soal 3](#soal-3)
* [Soal 4](#soal-4)
* [Soal 5](#soal-5)
* [Soal 6](#soal-6)
* [Soal Proxy](#proxy-soal-7---12)
* [Soal 7](#soal-7)
* [Soal 8](#soal-8)
* [Soal 9](#soal-9)
* [Soal 10](#soal-10)
* [Soal 11](#soal-11)
* [Soal 12](#soal-12)


## DHCP (Soal 1 - 6)

* IP TUBAN C12 = 10.151.77.108

## Soal 1

Sintaks untuk **topo.sh**

```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.53 eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server switch 2
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Klien switch 1
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &

# Klien switch 3
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
```
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.0.jpg)

Lalu  jalankan `bash topo.sh` pada *putty* dan masukkan *username* dan *password* default. Pada router **SURABAYA** lakukan setting `sysctl` dengan mengetikkan perintah `nano /etc/sysctl.conf`, dan tambahkan `net.ipv4.conf.all.accept_source_route = 1`.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.9.jpg)

Lalu setting IP pada setiap *interfaces* uml dengan mengetikkan `nano /etc/network/interfaces` sebagai berikut:

**SURABAYA (Sebagai Router)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.54
netmask 255.255.255.252
gateway 10.151.76.53

auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 10.151.77.105
netmask 255.255.255.248
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.1.jpg)

**MALANG (Sebagai DNS Server)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.106
netmask 255.255.255.248
gateway 10.151.77.105
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.3.jpg)

**MOJOKERTO (Sebagai Web Server)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.107
netmask 255.255.255.248
gateway 10.151.77.105
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.2.jpg)

**TUBAN (Sebagai Web Server)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.108
netmask 255.255.255.248
gateway 10.151.77.105
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.4.jpg)

**SIDOARJO (Sebagai Klien)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.0.2
netmask 255.255.255.0
gateway 192.168.0.1
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.7.jpg)

**GRESIK (Sebagai Klien)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.0.3
netmask 255.255.255.0
gateway 192.168.0.1
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.6.jpg)

**BANYUWANGI (Sebagai Klien)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.0.4
netmask 255.255.255.0
gateway 192.168.1.1
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.5.jpg)

**MADIUN (Sebagai Klien)**

```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.0.5
netmask 255.255.255.0
gateway 192.168.1.1
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/1.8.jpg)

Restart network dengan mengetikkan `service networking restart` di setiap UML. Ketikkan `iptables –t nat –A POSTROUTING –o eth0 –j MASQUERADE –s 192.168.0.0/16` pada router **SURABAYA**. IP Tables dapat dimasukkan ke dalam *script*, dalam hal ini, nama script berupa `table.sh`.

## Soal 2

* Meng-install `dhcp server` di **TUBAN** dengan cara `apt-get update` lalu `apt-get install isc-dhcp-server`. Setelah itu setting pada **TUBAN** dengan mengetikkan

```
nano /etc/default/isc-dhcp-server
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/2.0.jpg)

* Meng-install dhcp relay di **SURABAYA** dengan cara `apt-get update` lalu `apt-get install isc-dhcp-relay`. Setelah itu setting pada **SURABAYA** dengan mengetikkan

```
nano /etc/default/isc-dhcp-relay
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/2.1.jpg)

Lalu pastikan seluruh *client* menggunakan konfigurasi IP DHCP.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/2.7.jpg)

## Soal 3

Pada UML **TUBAN** buka file *dhcpd.conf* dengan menetikkan `nano /etc/dhcp/dhcpd.conf`, dan tambahkan range pada script sesuai soal

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/3.1.jpg)

Lalu pada client subnet 1 dapat mencoba `ifconfig` untuk cek IP

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/3.2.jpg)

## Soal 4

Pada UML **TUBAN** buka file *dhcpd.conf* dengan menetikkan `nano /etc/dhcp/dhcpd.conf`, dan tambahkan range pada script sesuai soal

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/4.0.jpg)

Lalu pada client subnet 3 dapat mencoba `ifconfig` untuk cek IP

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/4.1.jpg)

## Soal 5

Pada UML **TUBAN** buka file *dhcpd.conf* dengan menetikkan `nano /etc/dhcp/dhcpd.conf`, dan tambahkan pada script di bagian `option  domain-name-servers` sesuai soal yaitu **DNS MALANG** dan **DNS 202.46.129.2**

```
option domain-name-servers 10.151.77.106, 202.46.129.2;
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/5.0.jpg)

Setlah itu lakukan restart dhcp-relay pada **SURABAYA** dengan mengetikkan 

```
service isc-dhcp-relay restart
```

dan dhcp-server pada **TUBAN** dengan mengetikkan 

```
service isc-dhcp-relay restart
```

Ketikkan `service networking restart` pada setiap UML Client dan masukkan `ifconfig`. Periksa /etc/resolv.conf dengan menggunakan perintah

```
cat /etc/resolv.conf
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/5.1.jpg)
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/5.2.jpg)
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/5.3.jpg)
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/5.4.jpg)

## Soal 6

Pada UML **TUBAN** buka file *dhcpd.conf* dengan menetikkan `nano /etc/dhcp/dhcpd.conf`, dan tambahkan pada script di bagian `default-lease-time` sesuai soal yaitu client di subnet 1 selama 5 menit dan client di subnet 3 selama 10 menit.

* 5 menit = `60 s * 5` = 300
* 10 menit = `60 s * 10` = 600

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/6.0.jpg)

## Proxy (Soal 7 - 12)

* Konfigurasi `janganlupa-ta.yyy.pw` menjadi `janganlupa-ta.c12.pw` sesuai dengan nama kelompok.
* Konfigurasi `userta_yyy` menjadi `userta_c12` sesuai dengan nama kelompok.
* Konfigurasi ` inipassw0rdta_yyy` menjadi ` inipassw0rdta_c12` sesuai dengan nama kelompok.
* Susunan file config keseluruhan

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/7.0.jpg)

## Soal 7

Meng-install `squid` di **MOJOKERTO** dengan cara `apt-get update` lalu 

```
apt-get install isc-dhcp-server
```

dan meng-install `apache2-utils`

```
apt-get install apache2-utils
```

Backup terlebih dahulu file konfigurasi default yang disediakan Squid.

```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```

Buat konfigurasi baru dengan mengetikkan

```
nano /etc/squid/squid.conf
```

Kemudian, pada file config yang baru, ketikkan script:

```
http_port 8080
visible_hostname mojokerto
```

Setelah itu buat user dengan nama **userta_c12** dan password  pada **MOJOKERTO** dengan mengetikkan:

```
htpasswd -c /etc/squid/passwd userta_c12
```

Edit konfigurasi squid menjadi:

```
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

Restart squid `service squid restart`, dan berikutnya ubah **proxy** pada *web browser* ataupun *OS*

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/7.2.jpg)

Selanjutnya dapat mencoba untuk mengakses situs tertentu seperti `google.com`

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/7.1.jpg)
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/7.3.jpg)

## Soal 8

Pada UML **MOJOKERTO** buat file konfigurasi dengan megetikkan

```
nano /etc/squid/acl.conf
```
dan tambahkan

```
acl KERTA time TW 13:00-18:00
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/8.1.jpg)

Buka kembali file `squid.conf` dengan mengetikkan

```
include /etc/squid/acl.conf

http_port 8080
http_access allow USERS KERTA
http_access deny all
visible_hostname mojokerto
```

Simpan file tersebut. Kemudian `service squid restart`. Lalu conba akses situs apapun, contoh `detik.com`, jika sesuai dengan jam yang ditentukan, maka situs `detik.com` akan terbuka, jika tidak sesuai dengan jam yang telah ditentukan, maka situs tersebut tidak dapat diakses.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/8.2.jpg)

## Soal 9

Sama halnya dengan nomor 8. Pada UML **MOJOKERTO** buat file konfigurasi dengan megetikkan

```
nano /etc/squid/acl.conf
```
dan tambahkan

```
acl BIMTA time TWH 21:00-23:59
acl BIMTA time WHF 00:00-09:00
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/9.1.jpg)

Buka kembali file `squid.conf` dengan mengetikkan

```
include /etc/squid/acl.conf

http_port 8080
http_access allow USERS BIMTA
http_access deny all
visible_hostname mojokerto
```

Simpan file tersebut. Kemudian `service squid restart`. Lalu conba akses situs apapun, contoh `detik.com`, jika sesuai dengan jam yang ditentukan, maka situs `detik.com` akan terbuka, jika tidak sesuai dengan jam yang telah ditentukan, maka situs tersebut tidak dapat diakses.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/8.2.jpg)


## Soal 10

Buka file `ban.acl` dengan mengetikkan

```
nano /etc/squid/ban.acl
```

Lalu tambahkan pada file tersebut `google.com`. hal ini dimaskuudkan dengan soal yaitu jika *user* mengetikkan `google.com` makan me-*redirect* ke `monta.if.its.ac.id`.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/10.0.jpg)

Buka file `squid.conf` dengan mengetikkan `nano /etc/squid/squid.conf`, dan tambahkan script berikut:

```
acl NOO url_regex "/etc/squid/ban.acl"
deny_info http://monta.if.its.ac.id/ NOO
http_access deny NOO
```

Restart squid `service squid restart`, lalu masukkan `google.com` pada search bar, maka akan teralihkan ke `monta.if.its.ac.id`. (Berlaku jika sesuai dengan jam yang telah ditetapkan).

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/10.2.jpg)

## Soal 11

Buka folder `cd /usr/share/squid/errors/en` dan download error page dengan cara `wget 10.151.36.202/ERR_ACCESS_DENIED`. Lalu tampilannya akan sebagai berikut dengan `ls`

![MAAF FILE BELUM TERSEDIA](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/10.100.jpg)

Ganti file `ERR_ACCESSS_DENIED` dan rename file yang baru saja didownload yaitu `ERR_ACCESSS_DENIED.1` menjadi `ERR_ACCESSS_DENIED`, dengan cara:

```
rm ERR_ACCESSS_DENIED
mv ERR_ACCESSS_DENIED.1 ERR_ACCESSS_DENIED
```

Buka file `restrict-sites.acl` dengan cara 

```
nano /etc/squid/restrict-sites.acl
```

Lalu tambahkanalamat url yang hendak diblok, contohnya dalam hal ini `elearning.if.its.ac.id` dan `tempo.co`. 

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/11.2.jpg)

Buka kembali konfigurasi `squid.conf` dengan mengetikkan `nano /etc/squid/squid.conf`. Ubah file konfigurasi squid menjadi seperti berikut ini.

```
http_port 8080
visible_hostname mojokerto

acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
http_access deny BLACKLISTS
http_access allow all
```

Restart squid `service squid restart` dan jalankan alamat URL yang telah ditambahkan sebelumnya di file `restrict-sites.acl`.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/11.0.jpg)
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/11.1.jpg)

## Soal 12

Buka **MALANG** dan update package lists dengan menjalankan command:

```
apt-get update
```

Setelah melakukan update silahkan install aplikasi bind9 pada MALANG dengan perintah:

```
apt-get install bind9 -y
```

Lakukan perintah pada MALANG. Isikan seperti berikut:

```
nano /etc/bind/named.conf.local
```

Isikan configurasi domain janganlupa-ta.c12.pw sesuai dengan syntax berikut:
```
zone "janganlupa-ta.c12.pw" {
 	type master;
	file "/etc/bind/jarkom/janganlupa-ta.c12.pw";
};
```
![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/12.0.jpg)

Buat folder jarkom di dalam /etc/bind
```
mkdir /etc/bind/jarkom
```

Copykan file db.local pada path /etc/bind ke dalam folder jarkom yang baru saja dibuat dan ubah namanya menjadi janganlupa-ta.c12.pw
```
cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.c12.pw
```

Kemudian buka file janganlupa-ta.c12.pw dan edit seperti gambar berikut dengan IP MOJOKERTO masing-masing kelompok:
```
nano /etc/bind/jarkom/janganlupa-ta.c12.pw
```

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/12.1.jpg)

Restart bind9 dengan perintah
```
service bind9 restart
```

Ganti proxy pada *web browser* atau *OS* yang sebelumnya `10.151.77.107` menjadi `janganlupa-ta.c12.pw`, dengan port yaitu 8080.

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/12.4.jpg)

Lalu coba periksa proxy yang telah diubah tersebut

![Img](https://github.com/riclown/Jarkom_modul3_praktikum_C12/blob/main/img/11.1.jpg)

### Kendala
* Susunan logic *config* yang sempat membingungkan, sehingga beberapa nomor sempat tidak bisa berjalan.
