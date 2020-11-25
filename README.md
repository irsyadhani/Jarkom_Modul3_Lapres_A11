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
- Memasukkan perinah kode ```htpasswd -c /etc/squid/passwd userta_a11``` dan passwordnya ```inipassw0rdta_a11```
- Memasukkan perintah kode di file ```squid.conf```
```
http_port 8080
visible_hostname mojokerto
acl all src 0.0.0.0/0.0.0.0

auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd
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
include /etc/squid/acl.conf
visible_hostname mojokerto
acl all src 0.0.0.0/0.0.0.0

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
acl AVAILABLE_WORKING_2 time TWH 21:00-23:59
acl AVAILABLE_WORKING_3 time WHF 00:00-09:00
```
- Memasukkan perintah kode di file ```squid.conf```
```
include /etc/squid/acl.conf
visible_hostname mojokerto
acl all src 0.0.0.0/0.0.0.0

http_port 8080
http_access allow AVAILABLE WORKING_2
http_access allow AVAILABLE WORKING_3
http_access deny all
```
- Hasil:

![alt text](/img/9_3.jpg)
#
#### Soal 10: 
##### Setiap dia mengakses google.com, maka akan di redirect menuju monta.if.its.ac.id
_**Penyelesaian:**_
- Memasukkan perintah kode di file ```squid.conf```
```
visible_hostname mojokerto
acl all src 0.0.0.0/0.0.0.0
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
- Memasukkan perintah ke UML Mojokerto
```
cd /usr/share/squid/errors
mv /usr/share/squid/errors/en /usr/share/squid/errors/en1
mkdir /usr/share/squid/errors/en
wget 10.151.36.202/ERR_ACCESS_DENIED
cp -r ERR_ACCESS_DENIED /usr/share/squid/errors/en
```
- Memasukkan perintah kode di file ```/etc/bind/named.conf.local```
```
zone "janganlupa-ta.a11.pw"{
	type master;
	file"/ets/bind/jarkom/janganlupa-ta.a13.pw";
};
```
- Hasil:

![alt text](/img/.jpg)
#### Soal 12:
menggunakan proxy cukup dengan mengetikkan domain janganlupa-ta.yyy.pw dan memasukkan port 8080.
_**Penyelesaian:**_
- Memasukkan perintah kode di file ```/etc/bind/jarkom/janganlupa-ta.a11.pw```
```
;
; BIND data file for local loopback interface
;
$TTL	604800
@	IN	SDA	janganlupa-ta.a11.pw. janganlupa-ta.a11.pw.localhost. (
			2020112401	; Serial
			604800		; Referesh
			86400		; Retry
			2419200		; Expire
			60480 )		; Negative Cache TTL
;
@	IN	NS	janganlupa-ta.a11.pw.
@	IN 	A	10.151.73.99
```
- Hasil:

![alt text](/img/.jpg)
