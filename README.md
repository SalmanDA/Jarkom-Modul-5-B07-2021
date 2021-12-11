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
[gambar ping google.com]

***Cipher***
[gambar ping google.com]

***Elena***
[gambar ping google.com]

***Fukurou***
[gambar ping google.com]

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.

Masukkan perintah iptables berikut pada WATER7
```iptables -A FORWARD -d 192.180.0.8/29 -p tcp --dport 80 -j DROP```

Untuk testingnya, lakukan perintah ```nmap -p 80 192.180.0.10``` pada client. Disini kami menggunakan Blueno
[gambar 2]

Kalau nanti outputnya port 80 ```filtered``` berarti berhasil ```iptables```-nya.

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Lakukan PING pada semua host yang ada secara bersamaan. Host terakhir yang dilakukan ping tidak akan bisa.

**Ping Jipangu dari Blueno**

**Ping Jipangu dari Cipher**

**Ping Jipangu dari Elena**

**Ping Jipangu dari Fukurou**

## Soal 4
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Testing dilakukan pada host Blueno dan Ciper dengan melakukan ping ke Doriki, hasilnya tidak akan bisa.

**Blueno**

**Cipher**

## Soal 5
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

Pertama - tama periksa dahulu waktu sistem saat ini, dan lakukan ```ping Doriki``` dari host yang diminta.

**Elena**

**Fukurou**

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

[gambar 6]


