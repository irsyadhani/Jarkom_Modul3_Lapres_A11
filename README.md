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
----------------------------------------------------------------
# DHCP
#### Soal 1:
_**Penyelesaian:**_

● Membuat topologi dengan sintaks berikut
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
  ● Tambahkan interfaces="eth0" pada isc-dhcp-server
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

  ● Aktifkan baris net.ipv4.conf.all.accept_source_rote = 0 dan ubah nilainya menjadi 1 pada /etc/sysctl.conf
  ![alt text](/img/2_surabaya_sysctl.PNG)

  ● Periksa dengan ```sysctl -p``` kemudian restart server
  ● install paket isc-dhcp-relay
  ```
  apt-get isntall isc-dhcp-relay -y
  ```
  ● masukkan IP server DHCP, yaitu IP TUBAN
  ● masukkan ethernet card yang terhubung dengan layanan DHCP
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
  

 
