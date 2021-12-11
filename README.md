# Jarkom-Modul-5-B07-2021

**Anggota kelompok**:

- Salman Damai Alfariq (05111840000159)
- Ridho Ajiraga Jagiswara (05111940000170)
- David Ralphwaldo Martuaraja (05111940000190)

**Topologi**
![topologi](https://user-images.githubusercontent.com/73969921/145676566-98a273dc-71b4-47b1-8974-6fe338a29dda.jpg)

Keterangan :

- DORIKI adalah DNS Server
- JIPANGU adalah DHCP Server
- MAINGATE dan JORGE adalah Web Server
- Jumlah Host pada BLUENO adalah 100 host
- Jumlah Host pada CIPHER adalah 700 host
- Jumlah Host pada ELENA adalah 300 host
- Jumlah Host pada FUKUROU adalah 200 host

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.

Masukkan perintah iptables berikut pada FOOSHA
```iptables -t nat -A POSTROUTING -s 192.180.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.21```

ping google.com dari setiap node untuk testing

***Blueno***

![1  ping blueno](https://user-images.githubusercontent.com/75240358/145679400-5f207313-bbdf-43b7-8798-58c9ecb9514d.jpg)


***Cipher***

![1  ping cipher](https://user-images.githubusercontent.com/75240358/145679402-07ce12c5-fe00-4569-aa35-02c6e13518fa.jpg)


***Elena***

![1  ping elena](https://user-images.githubusercontent.com/75240358/145679404-41c5cc8e-097b-4d81-a140-788740e56038.jpg)


***Fukurou***

![1  ping fukurou](https://user-images.githubusercontent.com/75240358/145679406-a431ca66-45ab-4cf5-9ebb-25829407bd62.jpg)


## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

Masukkan perintah iptables berikut pada WATER7
```iptables -A FORWARD -d 192.180.0.8/29 -p tcp --dport 80 -j DROP```

Untuk testingnya, lakukan perintah ```nmap -p 80 192.180.0.10``` pada client. Disini kami menggunakan Blueno

![2](https://user-images.githubusercontent.com/75240358/145679426-dc824a10-1860-453f-9525-1eeaf18ededd.jpg)


Kalau nanti outputnya port 80 ```filtered``` berarti berhasil ```iptables```-nya.

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Lakukan PING pada semua host yang ada secara bersamaan. Host terakhir yang dilakukan ping tidak akan bisa.

**Ping Jipangu dari Blueno**!

![3  ping jipangu blueno](https://user-images.githubusercontent.com/75240358/145679432-9f0a689e-b13f-49d2-b9d9-8eef0b797360.jpg)


**Ping Jipangu dari Cipher**!

![3  ping jipangu cipher](https://user-images.githubusercontent.com/75240358/145679436-b4e9837c-d97e-48e9-9386-b46ffee95e25.jpg)


**Ping Jipangu dari Elena**!

![3  ping jipangu elena](https://user-images.githubusercontent.com/75240358/145679437-c449aed2-4e58-46b4-9df4-d207b8fe9dac.jpg)


**Ping Jipangu dari Fukurou**

![3  ping jipangu fukurou](https://user-images.githubusercontent.com/75240358/145679440-5c92cee9-88c3-4e9a-a4e3-020a7bb7602c.jpg)

## Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Testing dilakukan pada host Blueno dan Ciper dengan melakukan ping ke Doriki, hasilnya tidak akan bisa.

**Blueno**

![4  ping blueno](https://user-images.githubusercontent.com/74144561/145680175-ebb91a9e-55aa-403a-82dc-3a5e1293adcf.jpg)

**Cipher**

![4  ping cipher](https://user-images.githubusercontent.com/74144561/145680165-6473acec-033a-4a03-97a8-20442aea412c.jpg)

## Soal 5
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

Pertama - tama periksa dahulu waktu sistem saat ini, dan lakukan ```ping Doriki``` dari host yang diminta.

**Elena**

![5  ping elena](https://user-images.githubusercontent.com/74144561/145680168-2f8723fe-f4db-4df0-b728-1154985da656.jpg)

**Fukurou**

![5  ping fukurou](https://user-images.githubusercontent.com/74144561/145680169-07fc8231-e916-4668-944d-97a59b4fe1d3.jpg)

## Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

Masukkan perintah - perintah iptables berikut pada Guanhao
```
  iptables -A PREROUTING -t nat -d 192.180.0.10 -p tcp -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.180.0.18
  iptables -A PREROUTING -t nat -d 192.180.0.10 -p tcp -j DNAT --to-destination 192.180.0.19
```

- Pada Jipangu dan Maingate, install service apache dengan menjalankan perintah berikut.
```
echo 'nameserver 192.168.122.1
' > /etc/resolv.conf
apt-get update
apt-get install apache2 -y

echo ' /server/maingate
' > /var/www/html/index.html

service apache2 restart
```

- Setelah kedua web server berhasil disiapkan, coba akses ke node Doriki (DNS Server) lewat salah satu node yang berhubungan langsung dengan router Guanhao (pada kasus ini digunakan node Elena) dengan menggunakan perintah ```curl 192.180.0.11```

![gambar 6](https://user-images.githubusercontent.com/74144561/145680171-07cc3573-58f1-4d33-959f-4c3d768af3a3.jpg)

