# Jarkom-Modul-5-E01-2021

- Clarence 05111940000104
- Nur Putra Khanafi 05111940000020
- Husnan 05111940007002

## Pembagian Subnet

![topologi awal](https://user-images.githubusercontent.com/65794806/145667043-9e7bc343-492e-41a6-a3d4-e77843c96ce4.png)

- Doriki adalah DNS Server
- Jipangu adalah DHCP Server
- Maingate dan Jorge adalah Web Server
- Jumlah Host pada Blueno adalah 100 host
- Jumlah Host pada Cipher adalah 700 host
- Jumlah Host pada Elena adalah 300 host
- Jumlah Host pada Fukurou adalah 200 host

## VLSM(Variable Length Subnet Masking)

Menentukan jumlah alamat IP yang dibutuhkan oleh tiap subnet dan  melabel netmask berdasarkan jumlah IP yang dibutuhkan.\

| Subnet | Note | Jumlah IP | Length |
| --- | --- | --- | --- |
| A1 | Water7 Doriki Jipangu | 3 | 29 |
| A2 | Water7 Blueno | 101 | 25 |
| A3 | Water7 Cipher | 701 | 22 |
| A4 | Water7 Foosha | 2 | 30 |
| A5 | Foosha Guanhao | 2 | 30 |
| A6 | Guanhao Elena | 301 | 23 |
| A7 | Guanhao Fukurou | 201 | 24 |
| A8 | Guanhao Maingate Jorge | 3 | 29 |
| Total |  | 1314 | 21 |

### Pohon IP

Subnet besar yang dibentuk memiliki NID 192.200.0.0 dengan netmask /21 sehingga pembagian IP berdasarkan NID dan netmask dihitung sesuai dengan pohon berikut.

![pohon ip](https://user-images.githubusercontent.com/65794806/145666971-0b67d798-f095-4e2a-86b8-1a0295df2849.png)

Pada VLSM ini diturunkan sesuai dengan length atasnya sehingga ketika /21 akan diturunkan menjadi /22, dan pembagian IPnya mengikuti tabel. Kemudian jika ada subnet yang bisa diassign, maka kita langsung mengassign. Hal ini dilakukan berulang" sampai semua subnet selesai di assign.

### Subnet + IP

Selanjutnya menyesuaikan pembagian IP sesuai dengan subnet yang telah dibagi berdasarkan pohon diatas pada topologi. Setelah itu akan didapatkan pembagian IP untuk tiap nodenya.

![topologi subnet](https://user-images.githubusercontent.com/65794806/145666890-f127faf4-841f-4ce3-a860-2d9b46035ca3.png)

### Hasil Pembagian IP per node

| Subnet | Note | Jumlah IP | Length | IP | Subnet Mask |
| --- | --- | --- | --- | --- | --- |
| A1 | Water7 Doriki Jipangu | 3 | 29 | 192.200.7.128 | 255.255.255.248 |
| A2 | Water7 Blueno | 101 | 25 | 192.200.7.0 | 255.255.255.128 |
| A3 | Water7 Cipher | 701 | 22 | 192.200.0.0 | 255.255.252.0 |
| A4 | Water7 Foosha | 2 | 30 | 192.200.7.144 | 255.255.255.252 |
| A5 | Foosha Guanhao | 2 | 30 | 192.200.7.148 | 255.255.255.252 |
| A6 | Guanhao Elena | 301 | 23 | 192.200.4.0 | 255.255.254.0 |
| A7 | Guanhao Fukurou | 201 | 24 | 192.200.6.0 | 255.255.255.0 |
| A8 | Guanhao Maingate Jorge | 3 | 29 | 192.200.7.136 | 255.255.255.248 |

## Topologi Pada GNS

![topologi gns](https://user-images.githubusercontent.com/65794806/145667142-4c366eaf-c4f4-4736-82d5-310cd0c6722d.png)

## Configuration

### Config ETH

Selanjutnya melakukan mengisi konfigurasi network,  melakukan assignment dengan IP yang telah ditetapkan sesuai dengan subnet.

#### Doriki

```bash
auto eth0
iface eth0 inet static
address 192.200.7.130
netmask 255.255.255.248
gateway 192.200.7.129
```

#### Jipangu

```bash
auto eth0
iface eth0 inet static
address 192.200.7.131
netmask 255.255.255.248
gateway 192.200.7.129
```

#### Water7

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.200.7.145
netmask 255.255.255.252
gateway 192.200.7.146

auto eth1
iface eth1 inet static
address 192.200.7.129
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.200.7.1
netmask 255.255.255.128


auto eth3
iface eth3 inet static
address 192.200.0.1
netmask 255.255.252.0
```

#### Foosha 

- ip eth0 192.168.122.96

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
hwaddress ether b2:ab:03:2e:50:76

auto eth1
iface eth1 inet static
address 192.200.7.146
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.200.7.149
netmask 255.255.255.252
```

#### Jorge

```bash
auto eth0
iface eth0 inet static
address 192.200.7.138
netmask 255.255.255.248
gateway 192.200.7.137
```

#### Maingate

```bash
auto eth0
iface eth0 inet static
address 192.200.7.139
netmask 255.255.255.248
gateway 192.200.7.137
```

#### Guanhao

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.200.7.150
netmask 255.255.255.252
gateway 192.200.7.149

auto eth1
iface eth1 inet static
address 192.200.7.137
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.200.4.1
netmask 255.255.254.0

auto eth3
iface eth3 inet static
address 192.200.6.1
netmask 255.255.255.0
```

### Routing Foosha

```bash
# kiri
route add -net 192.200.7.128 netmask 255.255.255.248 gw 192.200.7.145
route add -net 192.200.7.0 netmask 255.255.255.128 gw 192.200.7.145
route add -net 192.200.0.0 netmask 255.255.252.0 gw 192.200.7.145

# kanan
route add -net 192.200.4.0 netmask 255.255.254.0 gw 192.200.7.150
route add -net 192.200.6.0 netmask 255.255.255.0 gw 192.200.7.150
route add -net 192.200.7.136  netmask 255.255.255.248 gw 192.200.7.150
```

### Config DHCP

1. Host yang menggunakan DHCP berjumlah 4, maka harus dibuat dulu subnetnya, sehingga network configuration diisi sebagai berikut.
```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

2. Kemudian install deps
```bash
# jipangu
apt-get update
apt-get install isc-dhcp-server -y
```

3. Pada Jipangu, ubah config interfaces di isc-dhcp-server, tambahkan eth0
```bash
# /etc/default/isc-dhcp-server
INTERFACES="eth0"
```

4. Melakukan konfigurasi pada /etc/dhcp/dhcpd.conf
```bash
# /etc/dhcp/dhcpd.conf
# Blueno A2
subnet 192.200.7.0 netmask 255.255.255.128 {
	range 192.200.7.2 192.200.7.126;
	option routers 192.200.7.1;
	option broadcast-address 192.200.7.127;
	option domain-name-servers 192.200.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Cipher A3
subnet 192.200.0.0 netmask 255.255.252.0 {
	range 192.200.0.2 192.200.3.254;
	option routers 192.200.0.1;
	option broadcast-address 192.200.3.255;
	option domain-name-servers 192.200.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}


# Elena A6
subnet 192.200.4.0 netmask 255.255.254.0 {
	range 192.200.4.2 192.200.5.254;
	option routers 192.200.4.1;
	option broadcast-address 192.200.5.255;
	option domain-name-servers 192.200.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}


# Fukurou A7
subnet 192.200.6.0 netmask 255.255.255.0 {
	range 192.200.6.2 192.200.6.254;
	option routers 192.200.6.1;
	option broadcast-address 192.200.6.255;
	option domain-name-servers 192.200.7.130;
	default-lease-time 600;
	max-lease-time 7200;
}

# Routing dari Jipangu ke router
subnet 192.200.7.128 netmask 255.255.255.248 {
        option routers 192.200.7.129;
}
```

### Config Relay

1. Menginstall relay
```bash
# water7 guanhao
apt-get update
apt-get install isc-dhcp-relay -y
```

2. Melakukan konfigurasi pada isc-dhcp-relay
```bash
# /etc/default/isc-dhcp-relay
# What servers should the DHCP relay forward requests to?
SERVERS="192.200.7.131"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

#### SS DHCP

Setelah melakukan konfigurasi, maka dhcp didapatkan IP sesuai subnet.

![ss dhcp1](https://user-images.githubusercontent.com/65794806/145668013-c13459db-cf87-43c3-8715-21b6967c90a5.png)

![ss dhcp2](https://user-images.githubusercontent.com/65794806/145668014-59eacf01-f6df-4dbd-bfcc-dbdade5ba435.png)

![ss dhcp3](https://user-images.githubusercontent.com/65794806/145668015-b43e3d97-bcc1-47db-b335-916cf0e5d223.png)

![ss dhcp4](https://user-images.githubusercontent.com/65794806/145668012-5e14a7a4-cc6d-4d77-b130-e7c26855d5d8.png)

### Config DNS (Doriki)

```bash
# /etc/bind/named.conf.options
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                192.168.122.1;
        };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
				listen-on-v6 { any; };
};
```

## Problem Solution

### 1

> Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

Syntax iptables yang dapat digunakan untuk masalah tersebut adalah sebagai berikut.

```bash
iptables -t nat -A POSTROUTING -s 192.200.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.96
```
##### Testing

![problem 1](https://user-images.githubusercontent.com/65794806/145668234-89086423-06fb-4e94-8cb4-74107336141a.gif)

### 2

> Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP server dan DNS Server demi menjaga keamanan.

Syntax iptables yang dapat digunakan untuk masalah tersebut adalah sebagai berikut.

```bash
# di doriki & jipangu
iptables -A FORWARD -d 192.200.7.128/29 -i eth0 -p tcp --dport 80 -j DROP
iptables -A FORWARD -d 192.200.7.128/29 -i eth0 -p tcp --dport 443 -j ACCEPT
```
##### Testing
ping google.com dan ping monta.if.its.ac.id

![problem 2](https://user-images.githubusercontent.com/65794806/145668333-91c04d99-a344-4311-af9e-1edeab04a617.gif)

### 3

> Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Syntax iptables ditempatkan pada ip DHCP server dan DNS Server.

```bash
# run di doriki & jipangu
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

##### Testing

![problem 3](https://user-images.githubusercontent.com/65794806/145668392-cf5f8269-a61b-4c6d-96ac-ee3b0038bdb0.png)

### 4

> Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Syntax iptables yang dapat digunakan untuk masalah tersebut adalah sebagai berikut.

```bash
# Script di run di doriki
# Blueno
iptables -A INPUT -s 192.200.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.200.7.0/25 -j REJECT

# Cipher
iptables -A INPUT -s 192.200.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.200.0.0/22 -j REJECT
```
##### Testing
```bash
Senin
date -s "6 DEC 2021 08:00:00" -> diterima
date -s "6 DEC 2021 16:00:00" -> ditolak
date -s "6 DEC 2021 18:00:00" -> ditolak


Sabtu
date -s "13 NOV 2021 09:00:00" -> ditolak
date -s "13 NOV 2021 01:00:00" -> ditolak
```

![problem 4a](https://user-images.githubusercontent.com/65794806/145668478-51917a3d-4b82-4668-8d13-75f06de95399.png)

![problem 4b](https://user-images.githubusercontent.com/65794806/145668481-af7494a5-3c62-4639-8cb1-8f322a6d59cc.png)

### 5

> Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

Syntax iptables yang dapat digunakan untuk masalah tersebut adalah sebagai berikut.

```bash
#  ELENA
iptables -A INPUT -s 192.200.4.0/23 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.200.4.0/23 -j REJECT

# FUKUROU
iptables -A INPUT -s 192.200.6.0/24 -m time --timestart 15:01 --timestop 06:59 -j ACCEPT
iptables -A INPUT -s 192.200.6.0/24 -j REJECT
```

##### Testing
```bash
Senin
date -s "6 DEC 2021 15:00:00" -> ditolak
date -s "7 DEC 2021 02:00:00" -> diterima
```
![problem 5a](https://user-images.githubusercontent.com/65794806/145668523-5f22ed6e-70c3-4cea-8a1e-d2fb1143b19e.png)

![problem 5b](https://user-images.githubusercontent.com/65794806/145668524-41566249-b262-4fff-8e2c-f49f754e60a3.png)

### 6

>

### Kendala Pengerjaan

1. Pada awalnya masih bingung dengan aturan dan arah penggabungan subnet pada CIDR.
