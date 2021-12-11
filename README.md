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
