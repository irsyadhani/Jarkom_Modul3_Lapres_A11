# Jarkom_Modul3_Lapres_A11
Kelompok A11:
* _Irsyadhani Dwi Shubhi (0511184000022)_
* _Anggun Wahyuni (05111840000154)_

----------------------------------------------------------------
## Soal
* [DHCP](#dhcp)
  * [Soal 1](#soal-1)
  * [Soal 2](#soal-2)
  * [Soal 3](#soal-3)
  * [Soal 4](#soal-4)
  * [Soal 5](#soal-5)
  * [Soal 6](#soal-6)
* [Proxy](#proxy)
  * [Soal 7](#soal-7)
  * [Soal 8](#soal-8)
  * [Soal 9](#soal-9)
  * [Soal 10](#soal-10)
  * [Soal 11](#soal-11)
  * [Soal 12](#soal-12)
----------------------------------------------------------------
# DHCP
#### Soal 1:
_**Penyelesaian:**_
- Membuat topologi dengan sintaks berikut
```
# Switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &
uml_switch -unix switch3 > /dev/null < /dev/null &

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,'10.151.72.49' eth1=daemon,,,switch1 eth2=daemon,,,switch3 eth3=daemon,,,switch2 mem=256M &

# Server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=160M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T TUBAN -e linux ubd0=TUBAN,jarkom umid=TUBAN eth0=daemon,,,switch2 mem=128M &

# Client subnet 1
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=64M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=64M &

# Client subnet 3
xterm -T BANYUWANGI -e linux ubd0=BANYUWANGI,jarkom umid=BANYUWANGI eth0=daemon,,,switch3 mem=64M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch3 mem=64M &
```
#
#### Soal 2:
_**Penyelesaian:**_
  ##### A. Konfigurasi DHCP Server uml TUBAN
  ● Tambahkan ```interfaces="eth0"``` pada ```isc-dhcp-server```
  ![alt text](/img/2_tuban_interfaces.PNG)

  ● Buka file konfigurasi DHCP dan tambahkan script seperti gambar
  ![alt text](/img/2_tuban_dhcp.PNG)
  ![alt text](/img/2_tuban_dhcp_1.PNG)
  ![alt text](/img/2_tuban_dhcp_2.PNG)

  ● Restart service isc-dhcp-server dengan perintah
  ```
  service isc-dhcp-server restart
  ```
  ##### B. Konfigurasi DHCP Relay uml SURABAYA
  ● atur ip address DNS pada relay server
  ![alt text](/img/2_surabaya_dns.PNG)

  ● Aktifkan baris ```net.ipv4.conf.all.accept_source_rote = 0``` dan ubah nilainya menjadi 1 pada ```/etc/sysctl.conf```
  ![alt text](/img/2_surabaya_sysctl.PNG)

  ● Periksa dengan ```sysctl -p``` kemudian restart server
  
  ● install paket ```isc-dhcp-relay```
  ```
  apt-get isntall isc-dhcp-relay -y
  ```
  
  ● masukkan IP server DHCP, yaitu IP TUBAN
  
  ● masukkan interfaces yang terhubung dengan layanan DHCP
  ![alt text](/img/2_dhcp_relay.PNG)
  
  ##### C. Konfigurasi DHCP Client
  ● Buka ```/etc/network/interfaces``` dan tambahkan
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
  
  ● restart network
#
#### Soal 3:
_**Penyelesaian:**_
  ● GRESIK
  
  ![alt text](/img/3_gresik.PNG)
  
  ● SIDOARJO
  
  ![alt text](/img/3_sidoarjo.PNG)
#
#### Soal 4:
_**Penyelesaian:**_
  ● BANYUWANGI
  
  ![alt text](/img/4_banyuwangi.PNG)
  
  ● MADIUN
  
  ![alt text](/img/4_madiun.PNG)
#
#### Soal 5:
_**Penyelesaian:**_
  ● GRESIK
  
  ![alt text](/img/5_gresik.PNG)
  
  ● SIDOARJO
  
  ![alt text](/img/5_sidoarjo.PNG)
  
  ● BANYUWANGI
  
  ![alt text](/img/5_banyuwangi.PNG)
  
  ● MADIUN
  
  ![alt text](/img/5_madiun.PNG)
#
#### Soal 6:
_**Penyelesaian:**_

  ● Subnet 1
  
  ![alt text](/img/2_tuban_dhcp.PNG)
  
  ● Subnet 2
  
  ![alt text](/img/2_tuban_dhcp_1.PNG)
---
# Proxy
#### Soal 7: 
User autentikasi milik Anri, User : userta_a11, Password : inipassw0rdta_a11

_**Penyelesaian:**_
- Memasukkan perinah kode ```htpasswd -c /etc/squid3/passwd userta_a11``` dan passwordnya ```inipassw0rdta_a11```
- Memasukkan perintah kode di file ```squid.conf```
```
http_port 8080
visible_hostname mojokerto

auth_param basic program /usr/lib/squid3/ncsa_auth /etc/squid3/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USER
```
#
#### Soal 8:
Anri sudah menjadwal pengerjaan TA-nya setiap hari Selasa-Rabu pukul 13.00-18.00.

_**Penyelesaian:**_
- Memasukkan perintah kode di file ```acl.conf``` 
```
acl AVAILABLE_WORKING time TW 13:00-18:00
```
- Memasukkan perintah kode di file ```squid.conf```
```
include /etc/squid3/acl.conf
visible_hostname mojokerto

http_port 8080
http_access allow AVAILABLE_WORKING
http_access deny all
```
- Hasil:

![alt text](/img/8_3.jpg)
#
#### Soal 9:
Jadwal bimbingan dengan Bu Meguri adalah setiap hari Selasa-Kamis pukul 21.00 - 09.00 keesokan harinya (sampai Jumat jam 09.00).

_**Penyelesaian:**_
- Memasukkan perintah kode di file ```acl.conf```
```
acl AVAILABLE_WORKING time TW 13:00-18:00
acl AVAILABLE_WORKING2 time TWH 21:00-23:59
acl AVAILABLE_WORKING3 time WHF 00:00-09:00
```
- Memasukkan perintah kode di file ```squid.conf```
```
include /etc/squid3/acl.conf
visible_hostname mojokerto

http_port 8080
http_access allow AVAILABLE_WORKING
http_access allow AVAILABLE_WORKING2
http_access allow AVAILABLE_WORKING3
http_access deny all
```
- Hasil:

![alt text](/img/9_3.jpg)
#
#### Soal 10: 
Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id

_**Penyelesaian:**_
- Memasukkan perintah kode di file ```squid.conf```
```
visible_hostname mojokerto
acl lan src all
acl google dstdomain .google.com

http_port 8080
deny_info http://monta.if.its.ac.id lan
http_reply_access deny google lan
http_access allow all
```
- ketika membuka ```google.com```

![alt text](/img/10_2.jpg)
- Hasil:

![alt text](/img/10_3.jpg)
#
#### Soal 11:
Bu Meguri meminta Anri untuk mengubah error page default squid

_**Penyelesaian:**_
* Masuk ke dalam folder error dengan mennjalankan perintah `cd /usr/share/squid/errors/English`
* Jalankan perintah `ls` untuk mengetahui isi keseluruhan folder tersebut, kemudian cari file bernama `ERR_ACCESS_DENIED`
* Lalu jalankan perintah `rm ERR_ACCESS_DENIED` untuk menghapus file.
* Download file berupa gambar error untuk menggantikan file yg sebelumnya dihapus dengan menjalankan perintah `wget 10.151.36.202/ERR_ACCESS_DENIED`
* Memasukkan perintah kode di file ```squid.conf```
```
error_directory /usr/share/squid3/errors/English/
include /etc/squid3/acl.conf

http_port 8080
http_access deny all
visible_hostname mojokerto
```
- Hasil:

![alt text](/img/11.jpg)
#### Soal 12:
Menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.

_**Penyelesaian:**_
* Pada UML MALANG jalankan perintah apt-get `install bind9 -y` jangan lupa untuk melakukan `apt-get update`
* Edit file `named.conf.local` dengan menjalankan perintah `nano /etc/bind/named.conf.local` yang didalamnya sebagai berikut :
```
zone "janganlupa-ta.a11.pw" {
	type master;
	file "/etc/bind/jarkom/janganlupa-ta.a11.pw";
};
```
* Buat folder jarkom dengan menjalankan perintah `mkdir /etc/bind/jarkom
* Copy file `db.local` dengan menjalankan perintah `cp /etc/bind/db.local /etc/bind/jarkom/janganlupa-ta.a11.pw`
* Menjalankan perintah `nano /etc/bind/jarkom/janganlupa-ta.a11.pw`
* Memasukkan perintah kode di file `/etc/bind/jarkom/janganlupa-ta.a11.pw`
```
;
; BIND data file for local loopback interface
;
$TTL	604800
@	IN	SDA	janganlupa-ta.a11. root.janganlupa-ta.a11.pw. (
			2020112901	; Serial
			604800		; Referesh
			86400		; Retry
			2419200		; Expire
			60480 )		; Negative Cache TTL
;
@	IN	NS	janganlupa-ta.a11.pw.
@	IN 	A	10.151.73.99
```

* Simpan dan restart dengan menjalankan perintah `service bind9 restart`
* Pada UML GRESIK edit file `resolv.conf` dengan menjalankan perintah `nano /etc/resolv.conf`
* Tuliskan `nameserver 10.151.73.99`
* Hasil:

![alt text](/img/12.jpg)
