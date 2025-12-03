[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/oYnIPZ_t)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| A. Wildan Kevin Assyauqi | 5025241265 | Jaringan Komputer (A) |

## Put your topology config image here!
<img width="1268" height="693" alt="image" src="https://github.com/user-attachments/assets/7d0f6c1c-9b4a-4b10-b66c-79c5192d53a8" />

## Put your GNS3 Project file here!
[https://drive.google.com/file/d/1X4tcHzcmcXr8FfHkZd9SWGZHW9_E3wGB/view?usp=sharing](https://drive.google.com/file/d/1X4tcHzcmcXr8FfHkZd9SWGZHW9_E3wGB/view?usp=sharing)

<br>

## Soal 1

> Lakukan subnetting pada topologi diatas menggunakan metode VLSM: [Referensi](https://github.com/arsitektur-jaringan-komputer/Modul-Jarkom/tree/master/Modul-4/Subnetting#2-vlsm-variable-length-subnet-masking)  
*Cantumkan juga tabel dan diagram pembagian subnet pada laporan praktikum*.

### Answer:

Daftar Kebutuhan Host

| LAN / Node | Kebutuhan Host |
|------------|----------------|
| HR-PC-1 | 250 |
| HR-PC-2 | 200 |
| IT-PC-1 | 50 |
| IT-PC-2 | 25 |
| IT-PC-3 | 40 |
| Web-Server-1 | 25 |
| Web-Server-2 | 20 |
| DB-Server-1 | 12 |
| DB-Server-2 | 18 |

> VLSM Calculation

B.1 Tabel Subnet LAN

| LAN | Subnet | Prefix | Network ID | Broadcast | Range Host | Router Gateway | Host Anggota |
|------|---------|--------|-------------|-------------|------------------|-----------------|----------------|
| HR-LAN | HR-PC-1, HR-PC-2 | /23 | 10.78.0.0 | 10.78.1.255 | 10.78.0.1 - 10.78.1.254 | router-5:eth1 = 10.78.0.1 | 10.78.0.2 (HR-PC-1), 10.78.0.3 (HR-PC-2) |
| IT-LAN | IT-PC-1, IT-PC-2, IT-PC-3 | /25 | 10.78.2.0 | 10.78.2.127 | 10.78.2.1 - 10.78.2.126 | router-1:eth0 = 10.78.2.1 | 10.78.2.2, 10.78.2.3, 10.78.2.4 |
| Web-Server-1 LAN | Web-1 | /27 | 10.78.2.128 | 10.78.2.159 | 10.78.2.129 - 10.78.2.158 | router-4:eth1 = 10.78.2.129 | Web-Server-1 = 10.78.2.130 |
| Web-Server-2 LAN | Web-2 | /27 | 10.78.2.160 | 10.78.2.191 | 10.78.2.161 - 10.78.2.190 | router-4:eth2 = 10.78.2.161 | Web-Server-2 = 10.78.2.162 |
| DB-Server-1 LAN | DB-1 | /28 | 10.78.2.224 | 10.78.2.239 | 10.78.2.225 - 10.78.2.238 | router-3:eth1 = 10.78.2.225 | DB-Server-1 = 10.78.2.226 |
| DB-Server-2 LAN | DB-2 | /27 | 10.78.2.192 | 10.78.2.223 | 10.78.2.193 - 10.78.2.222 | router-3:eth2 = 10.78.2.193 | DB-Server-2 = 10.78.2.194 |

---

B.2 Tabel Subnet Antar-Router (Link /30)

| Link | Subnet | Prefix | Network ID | IP Sisi A | IP Sisi B | Broadcast |
|------|---------|--------|--------------|------------|-------------|-------------|
| router-1 ↔ router-2 | R1-R2 | /30 | 10.78.2.240 | r1:eth1 = 10.78.2.241 | r2:eth0 = 10.78.2.242 | 10.78.2.243 |
| router-2 ↔ NAT1 | R2-NAT | /30 | 10.78.2.244 | r2:eth3 = 10.78.2.245 | NAT1 = 10.78.2.246 | 10.78.2.247 |
| router-2 ↔ router-3 | R2-R3 | /30 | 10.78.2.248 | r2:eth2 = 10.78.2.249 | r3:eth0 = 10.78.2.250 | 10.78.2.251 |
| router-2 ↔ router-4 | R2-R4 | /30 | 10.78.2.252 | r2:eth1 = 10.78.2.253 | r4:eth0 = 10.78.2.254 | 10.78.2.255 |
| router-4 ↔ router-5 | R4-R5 | /30 | 10.78.3.0 | r4:eth3 = 10.78.3.1 | r5:eth0 = 10.78.3.2 | 10.78.3.3 |

Fungsi Setiap Node

| Node | Fungsi | Interface | IP |
|------|---------|-----------|----------|
| IT-PC-1 | Client IT | eth0 | 10.78.2.2 |
| IT-PC-2 | Client IT | eth0 | 10.78.2.3 |
| IT-PC-3 | Client IT | eth0 | 10.78.2.4 |
| HR-PC-1 | Client HR | eth0 | 10.78.0.2 |
| HR-PC-2 | Client HR | eth0 | 10.78.0.3 |
| Web-Server-1 | Web Server | eth0 | 10.78.2.130 |
| Web-Server-2 | Web Server | eth0 | 10.78.2.162 |
| DB-Server-1 | Database Server | eth0 | 10.78.2.226 |
| DB-Server-2 | Database Server | eth0 | 10.78.2.194 |
| router-1 | Router IT | eth0/eth1 | 10.78.2.1 / 10.78.2.241 |
| router-2 | Router Utama | eth0/eth1/eth2/eth3 | 10.78.2.242 / 10.78.2.253 / 10.78.2.249 / 10.78.2.245 |
| router-3 | Router DB | eth0/eth1/eth2 | 10.78.2.250 / 10.78.2.225 / 10.78.2.193 |
| router-4 | Router Web | eth0/eth1/eth2/eth3 | 10.78.2.254 / 10.78.2.129 / 10.78.2.161 / 10.78.3.1 |
| router-5 | Router HR | eth0/eth1 | 10.78.3.2 / 10.78.0.1 |

> Subnetting

- E.1 Konfigurasi Host IT dan HR

> IT-PC-1

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.2
    netmask 255.255.255.128
    gateway 10.78.2.1
```

> IT-PC-2

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.3
    netmask 255.255.255.128
    gateway 10.78.2.1
```
> IT-PC-3

```bash
auto eth0
iface eth0 inet static
        address 10.78.2.4
        netmask 255.255.255.128
        gateway 10.78.2.1
```

> HR-PC-1

```bash
auto eth0
iface eth0 inet static
    address 10.78.0.2
    netmask 255.255.254.0
    gateway 10.78.0.1
```

> HR-PC-2

```bash
auto eth0
iface eth0 inet static
    address 10.78.0.3
    netmask 255.255.254.0
    gateway 10.78.0.1
```



- E.2 Konfigurasi Web Server

> Web-Server-1

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.130
    netmask 255.255.255.224
    gateway 10.78.2.129
```

> Web-Server-2

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.162
   netmask 255.255.255.224
    gateway 10.78.2.161
```



- E.3 Konfigurasi Database Server

> DB-Server-1

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.226
    netmask 255.255.255.240
    gateway 10.78.2.225
```

> DB-Server-2

```bash
auto eth0
iface eth0 inet static
    address 10.78.2.194
    netmask 255.255.255.224
    gateway 10.78.2.193
```



- E.4 Konfigurasi Router

> router-1

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.2.1
    netmask 255.255.255.128

auto eth1
iface eth1 inet static
    address 10.78.2.241
    netmask 255.255.255.252
```

> router-2

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.2.242
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.78.2.253
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.78.2.249
    netmask 255.255.255.252

auto eth3
iface eth3 inet static
    address 192.168.122.111
    netmask 255.255.255.0
    gateway 192.168.122.1

post-up iptables -t nat -A POSTROUTING -o eth3 -j MASQUERADE
```

> router-3

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.2.250
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.78.2.225
    netmask 255.255.255.240

auto eth2
iface eth2 inet static
    address 10.78.2.193
    netmask 255.255.255.224
```

> router-4

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.2.254
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.78.2.129
    netmask 255.255.255.224

auto eth2
iface eth2 inet static
    address 10.78.2.161
    netmask 255.255.255.224

auto eth3
iface eth3 inet static
    address 10.78.3.1
    netmask 255.255.255.252
```

> router-5

```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.3.2
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.78.0.1
    netmask 255.255.254.0
```

### Screenshot
<img width="1268" height="693" alt="image" src="https://github.com/user-attachments/assets/7b5bc42a-1282-4720-ac8a-c4775c76deb5" />

### Explanation

Proses subnetting menggunakan metode Variable Length Subnet Mask (VLSM). Pembagian dilakukan berdasarkan kebutuhan host terbesar terlebih dahulu, yaitu HR-LAN yang membutuhkan lebih dari 400 host sehingga diberikan subnet /23. Setelah itu alokasi berlanjut ke LAN IT, Web Server, Database Server, dan terakhir link antar-router yang cukup menggunakan subnet /30.

Hasil akhirnya memastikan seluruh perangkat dalam topologi memiliki alamat IP unik tanpa pemborosan ruang alamat. Pembagian ini juga memudahkan konfigurasi routing pada tahap berikutnya karena setiap jaringan sudah terstruktur jelas dan konsisten antara tabel subnet dan konfigurasi antarmuka.

<br>

## Soal 2

> Buatlah agar router-2 dapat melakukan koneksi ke internet. [Dapat menggunakan static routing].

### Answer:

- Tujuan  
Agar router-2 dapat mengakses internet melalui NAT1. Router-2 dianggap berhasil terhubung dengan internet apabila dapat melakukan:
  - ping 8.8.8.8
  - ping google.com

- Langkah
> 1. Identifikasi Jaringan NAT

Saat interface router-2 di-set DHCP sementara, diperoleh:

```
inet 192.168.122.111/24 scope global eth3
```

Artinya:
  - NAT network: 192.168.122.0/24
  - Router-2 IP: 192.168.122.111
  - Gateway NAT: 192.168.122.1
  - Netmask: 255.255.255.0


Nilai ini yang akan dijadikan konfigurasi statis.



> 2. Mengaktifkan IP Forwarding (Wajib)

Agar router‑2 dapat meneruskan paket ke NAT dan ke internet, kernel harus mengaktifkan forwarding.

Edit file:
```bash
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
```

Aktifkan:
```bash
sysctl -p
```

Tanpa langkah ini, router tidak dapat meneruskan paket meskipun route sudah benar.



> 3. Konfigurasi router-2
```conf
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.2.242
    netmask 255.255.255.252

auto eth1
iface eth1 inet static
    address 10.78.2.253
    netmask 255.255.255.252

auto eth2
iface eth2 inet static
    address 10.78.2.249
    netmask 255.255.255.252

auto eth3
iface eth3 inet static
    address 192.168.122.111
    netmask 255.255.255.0
    gateway 192.168.122.1

post-up iptables -t nat -A POSTROUTING -o eth3 -j MASQUERADE
```

> 4. Verifikasi Koneksi

Ping NAT1
```
ping 192.168.122.1
```

Ping DNS Public
```
ping 8.8.8.8
ping 1.1.1.1
```

Ping Domain
```
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf

ping google.com
``` 

### Screenshot
<img width="911" height="746" alt="image" src="https://github.com/user-attachments/assets/3268c93c-ded2-4c44-bc2c-00663228c8d9" />


### Explanation

Node NAT1 tidak mengikuti skema subnet internal (10.78.x.x). NAT GNS3 menggunakan jaringan sendiri, yaitu 192.168.122.0/24.  
Oleh karena itu interface router-2 yang terhubung ke NAT harus diatur dengan IP yang berada di dalam subnet NAT. Dengan demikian, router-2 dapat mengakses internet menggunakan metode static routing 

<br>

## Soal 3

> Setelah mengimplementasi subnetting, buatlah agar seluruh topologi dapat terhubung. Lakukan Dynamic Routing pada topologi tersebut.
*Pastikan seluruh node yang ada dapat mengakses internet*.

### Answer:

- Tujuan
  1. Mengaktifkan dynamic routing (RIP) pada seluruh router.  
  2. Memastikan setiap router saling bertukar informasi routing secara otomatis.  
  3. Seluruh node (PC/LAN) dapat saling berkomunikasi.  
  4. Semua node dapat mengakses internet melalui Router-2.  

> Langkah 1 – Aktifkan IP Forwarding
Pada setiap router:

```bash
echo "net.ipv4.ip_forward=1" > /etc/sysctl.conf
```

Aktifkan:

```bash
sysctl -p
```

> Langkah 2 – Tambahkan DNS agar router dapat resolve domain**
```bash
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

> Langkah 3 - Menyiapkan FRRouting

FRR berada di folder:

```bash
/usr/lib/frr
```

Nyalakan daemon

```bash
/usr/lib/frr/zebra -d
/usr/lib/frr/ripd -d
/usr/lib/frr/mgmtd -d
```

Masuk ke CLI FRR:

```bash
vtysh
```

> Langkah 4 - Konfigurasi Dynamic Routing (RIP) untuk Setiap Router

- Router-1
```bash
configure terminal
router rip
 version 2

 network 10.78.2.0/25
 network 10.78.2.240/30

 no auto-summary
end
write
exit
```

- Router-2 (Router Internet Gateway)
```bash
configure terminal
router rip
 version 2

 network 10.78.2.240/30
 network 10.78.2.244/30
 network 10.78.2.248/30
 network 10.78.2.252/30

 default-information originate
 no auto-summary
end
write
exit
```

Tambahkan NAT agar seluruh node bisa ke internet:

```bash
apt update
apt install iptables -y
iptables -t nat -A POSTROUTING -o eth3 -j MASQUERADE
```

- Router-3
```bash
configure terminal
router rip
 version 2

 network 10.78.2.248/30
 network 10.78.2.192/27
 network 10.78.2.224/28

 no auto-summary
end
write
exit
```

- Router-4
```bash
configure terminal
router rip
 version 2

 network 10.78.2.252/30
 network 10.78.2.128/27
 network 10.78.2.160/27
 network 10.78.3.0/30

 no auto-summary
end
write
exit
```

- Router-5
```bash
configure terminal
router rip
 version 2

 network 10.78.3.0/30
 network 10.78.0.0/23

 no auto-summary
end
write
exit
```

> Langkah 6 - Verifikasi

Contoh dari IT-PC‑1:
- Verifikasi internet
```bash
ping 8.8.8.8

echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf

ping google.com
```

- Verifikasi Semua Node Terkoneksi
Dari IT-PC-1
```bash
ping 10.78.0.2
ping 10.78.2.130
ping 10.78.2.194
```

Dari HR-PC-1
```bash
ping 10.78.2.2
ping 10.78.2.130
```

Kalau berhasil, Semua harus reply.

### Screenshot
<img width="780" height="516" alt="image" src="https://github.com/user-attachments/assets/fb9eccb4-1b49-4fbe-9d70-acdaff274c8b" />

### Explanation

Dengan menggunakan **RIP**, seluruh router akan bertukar informasi subnet secara otomatis.  
Router tidak perlu dibuatkan static route satu per satu.  
Router-2 bertindak sebagai **gateway internet**, sehingga semua router akan menerima default route via RIP.

DNS diperlukan agar apt dan ping domain dapat bekerja.  
iptables diperlukan untuk NAT agar IP private dapat keluar menuju internet.

<br>

## Soal 4

> Lakukan setup web server dengan file html di attachment berikut: [ Attachment ](https://drive.google.com/file/d/199qwfTNJCkxDV7mdO-MsaDdApkmKsnAG/view?usp=sharing)  menggunakan nginx pada “Web-Server-1” dan “Web-Server-2”.  
*Config dibebaskan kepada praktikkan dengan catatan menggunakan port 80*.

### Answer:

- Tujuan  
  1. Menjalankan web server nginx pada **Web-Server-1** dan **Web-Server-2** di port **80**.  
  2. Menggunakan folder **/var/www/html** sebagai document root.  
  3. Menampilkan file **index.html** dari attachment praktikum.  
  4. Web server dapat diakses dari node lain (misalnya IT-PC-1).

Dilakukan pada node Web-Server
> Langkah 1 - Install nginx
```
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```
Install nginx
```
apt update
apt install nginx -y
service nginx start
```

> Langkah 2 - Buat file HTML
Buat file `index.html`
```
nano /var/www/html/index.html
```
Copy pasete file HTML berikut:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Page</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            color: #333;
        }
        .container {
            text-align: center;
            padding: 20px 30px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1 {
            margin-bottom: 10px;
            font-size: 24px;
            font-weight: 600;
        }
        p {
            margin: 0;
            font-size: 14px;
            color: #555;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Hello!</h1>
        <p>Web Server | Jarkom Praktikum Modul 4 </p>
    </div>
</body>
</html>
```

> Langkah 3 - Verifikasi

  a. Pengujian di Web Server
Lakukan pengujian di node web-server:
```
curl localhost
```

  b. Pengujian dari node lain
Contoh dari IT-PC-1:
```
curl 10.78.2.130   # Web-Server-1
curl 10.78.2.162   # Web-Server-2
```

Jika HTML tampil, konfigurasi berhasil.

### Screenshot
  
<img width="974" height="583" alt="image" src="https://github.com/user-attachments/assets/d65b7351-1147-49f0-9aa4-d5a33d26ee1b" />

<img width="972" height="512" alt="image" src="https://github.com/user-attachments/assets/684e3fe6-44b8-425d-8a57-f8969dd2c03b" />

### Explanation

Konfigurasi dilakukan pada kedua node Web-Server. Instalasi `nginx` yang by default listen di port 80. Dan `curl` yaitu sintaks sederhana untuk menampilkan html.

<br>

## Soal 5

> Kalian diminta untuk melakukan drop semua paket TCP yang masuk  ke subnet HR dengan port 1337 dan 4444. Lakukan testing dengan netcat.

### Answer:

Kita akan menerapkan `iptables` pada `router-5` karena merupakan Gateway dari node HR.

> Langkah 1 - Instalasi iptables

Jalankan pada **router-5**:

```bash
apt update
apt install iptables -y
```

> Langkah 2 - Tambahkan rule firewall

Masih di node `router-5`, jalankan:
```bash
iptables -A FORWARD -p tcp -d 10.78.0.0/23 --dport 1337 -j DROP
iptables -A FORWARD -p tcp -d 10.78.0.0/23 --dport 4444 -j DROP
```
- Penjelasan:
  - `FORWARD` -> paket transit menuju HR.
  - `-d 10.78.0.0/23` -> mencakup HR-PC-1 & HR-PC-2.
  - `--dport 1337 & 4444` -> port yang diblokir.
  - `DROP` -> paket langsung dibuang tanpa notifikasi.
 
> Langkah 3 - Testing Dengan netcat

Lakukan pengujian dari node **di luar HR**, misal `IT-PC-1`.

Test port yang diblokir
```bash
nc 10.78.0.2 1337
nc 10.78.0.2 4444
```
Apabila benar, maka terminal akan diam/hang.

Test port yang tidak diblokir
```bash
nc 10.78.0.2 1111
nc 10.78.0.2 5555
```
Apabila benar, maka terminal akan langsung ke perintah selanjutnya (tidak ada output apapun)

### Screenshot
<img width="499" height="238" alt="image" src="https://github.com/user-attachments/assets/7adf5d8b-c242-46e1-84ba-34a3017335ae" />

### Explanation
iptables memainkan peran sebagai **firewall** yang memastikan subnet HR hanya menerima trafik yang diizinkan, sekaligus menolak trafik berbahaya atau tidak dibutuhkan.  
Dengan demikian, iptables membantu menjaga **isolasi**, **kontrol akses**, serta **integritas jaringan**.
<br>

## Soal 6

> Lakukan pembatasan sehingga koneksi SSH pada semua Web Server hanya dapat dilakukan oleh user yang berada pada node IT-PC-1, IT-PC-2, dan IT-PC-3. 

### Answer:

- Tujuan
    - Membatasi akses SSH pada Web-Server-1 dan Web-Server-2.
    - Hanya mengizinkan koneksi SSH dari:
      - IT-PC-1  
      - IT-PC-2  
      - IT-PC-3  
    - Memblokir SSH dari seluruh node lainnya.

> Langkah 1 - Instalasi SSH Server & Client

Install SSH Server pada node Web-Server. Jalankan pada *kedua* Web Server:
```bash
apt update
apt install openssh-server -y
service ssh start
```

Instal SSH Client pada node IT-PC. Jalankan:
```bash
apt update
apt install openssh-client -y
```

> Langkah 2 - Konfigurasi rule firewall

Pembatasan dilakukan di **router-4** agar hanya IT-PC yang bisa SSH ke Web Server. Jalankan di `router-4`:
```bash
apt update
apt install iptables -y
```
Izinkan SSH dari IT-PC dan blokir sumber lain

```bash
iptables -A FORWARD -p tcp -s 10.78.2.2 -d 10.78.2.128/27 --dport 22 -j ACCEPT
iptables -A FORWARD -p tcp -s 10.78.2.3 -d 10.78.2.128/27 --dport 22 -j ACCEPT
iptables -A FORWARD -p tcp -s 10.78.2.4 -d 10.78.2.128/27 --dport 22 -j ACCEPT

iptables -A FORWARD -p tcp -s 10.78.2.2 -d 10.78.2.160/27 --dport 22 -j ACCEPT
iptables -A FORWARD -p tcp -s 10.78.2.3 -d 10.78.2.160/27 --dport 22 -j ACCEPT
iptables -A FORWARD -p tcp -s 10.78.2.4 -d 10.78.2.160/27 --dport 22 -j ACCEPT

iptables -A FORWARD -p tcp -d 10.78.2.128/27 --dport 22 -j DROP
iptables -A FORWARD -p tcp -d 10.78.2.160/27 --dport 22 -j DROP
```

> Langkah 3 - Verifikasi

Contoh dari IT-PC-1 (Berhasil):

```
ssh root@10.78.2.130
ssh root@10.78.2.162
```
apabila berhasil maka akan keluar output seperti berikut:
```
The authenticity of host can't be established...
root@10.78.2.11's password:
```

Contoh dari node lain, yaitu HR-PC-1 (gagal):

```
ssh root@10.78.2.130
```
apabila benar, maka terminal akan diam atau hang.

### Screenshot
<img width="911" height="380" alt="image" src="https://github.com/user-attachments/assets/b7c48ee3-de64-4921-8216-17cfd7d7d439" />


### Explanation

Untuk melindungi Web Server, aturan firewall diterapkan menggunakan iptables.
- Konsep:
    - Rule ACCEPT ditempatkan paling atas untuk mengizinkan IT-PC sebagai sumber terpercaya.
    - Rule DROP di bawahnya menolak seluruh permintaan SSH lainnya.
    - SSH bekerja pada port 22, sehingga pembatasan difokuskan pada port tersebut.
    - Dengan kombinasi ini, Web Server aman dari akses tidak sah dan hanya bisa diakses dari subnet IT.

<br>

## Soal 7

> Semua subnet hanya dapat mengakses semua DB-Server pada port 80 dan 443 (DB-Server-1 dan DB-Server-2) pada hari Senin-Sabtu, pukul 07:00- 22:00.

### Answer:
- Tujuan:

Membatasi akses ke DB-Server-1 dan DB-Server-2 sehingga hanya dapat diakses melalui port 80 dan 443, dan hanya pada hari Senin–Sabtu, pukul 07:00–22:00 (berdasarkan waktu UTC yang digunakan sistem).

Karena `router-3` yang menjadi gateway node `DB-Server`, maka kita akan menjalankan rule firewall di node `router-3`.
> Langkah 1 - Instalasi iptables pada router-3

Jalankan: 
```bash
apt update
apt install iptables -y
```

> Langkah 2 - Melakukan konfigurasi rule firewall

Agar koneksi yang sudah berjalan (misalnya balasan paket dari server) tetap diizinkan, tambahkan rule berikut pada chain `FORWARD`:
```bash
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
```
- Penjelasan:
    - Modul `state` digunakan untuk mengenali koneksi yang sudah ada.
    - Koneksi dengan status `ESTABLISHED` dan `RELATED` akan selalu diizinkan melewati router.

Selanjutnya, buat rule untuk mengizinkan akses HTTP/HTTPS ke DB-Server di waktu tertentu. Pada bagian ini, dibuat empat buah rule `ACCEPT` yang memperbolehkan trafik TCP ke port 80 dan 443 pada DB-Server-1 dan DB-Server-2.  
Semua rule ini menggunakan modul `time` untuk membatasi hari dan jam akses:
```bash
iptables -A FORWARD -p tcp -d 10.78.2.226 --dport 80   -m time --timestart 07:00 --timestop 22:00   --weekdays Mon,Tue,Wed,Thu,Fri,Sat   -j ACCEPT
iptables -A FORWARD -p tcp -d 10.78.2.226 --dport 443   -m time --timestart 07:00 --timestop 22:00   --weekdays Mon,Tue,Wed,Thu,Fri,Sat   -j ACCEPT

iptables -A FORWARD -p tcp -d 10.78.2.194 --dport 80   -m time --timestart 07:00 --timestop 22:00   --weekdays Mon,Tue,Wed,Thu,Fri,Sat   -j ACCEPT
iptables -A FORWARD -p tcp -d 10.78.2.194 --dport 443   -m time --timestart 07:00 --timestop 22:00   --weekdays Mon,Tue,Wed,Thu,Fri,Sat   -j ACCEPT
```
- Penjelasan:
    - `-p tcp` membatasi rule ini hanya untuk protokol TCP.
    - `-d 10.78.2.226` dan `-d 10.78.2.194` membatasi tujuan hanya ke DB-Server-1 dan DB-Server-2.
    - `--dport 80` dan `--dport 443` membatasi hanya ke port HTTP dan HTTPS.
    - `-m time` menggunakan modul `time` untuk pembatasan waktu:
      - `--timestart 07:00 --timestop 22:00` artinya rule ini aktif dari pukul 07:00 sampai 22:00 (waktu sistem, biasanya UTC).
      - `--weekdays Mon,Tue,Wed,Thu,Fri,Sat` artinya rule hanya aktif pada hari Senin hingga Sabtu.

Dengan demikian, di luar jam dan hari tersebut, rule `ACCEPT` ini tidak aktif dan paket tidak akan diizinkan oleh rule ini.

Selanjutnya, blokir semua akses TCP lain ke DB-Server. Tambahkan rule `DROP` untuk semua trafik TCP ke DB-Server di chain `FORWARD`.  
Rule ini akan menjadi “payung” yang memblokir semua trafik yang tidak cocok dengan rule `ACCEPT` sebelumnya (baik karena port tidak sesuai, waktu di luar jam kerja, atau hari yang tidak diizinkan).
```bash
iptables -A FORWARD -p tcp -d 10.78.2.226 -j DROP
iptables -A FORWARD -p tcp -d 10.78.2.194 -j DROP
```

> Langkah 3 - Verifikasi akses

Pengujian akses yang diizinkan (berhasil)

1. Di node `DB-Server-1`, jalankan `netcat` sebagai server pada port 80:

   ```bash
   nc -l -p 80
   ```

2. Pada node `IT-PC-1` (atau client lain), lakukan koneksi ke DB-Server-1 port 80:

   ```bash
   nc 10.78.2.226 80
   ```

3. Jika rule benar dan waktu akses berada dalam rentang yang diizinkan, koneksi akan tersambung (terminal `nc` akan terlihat “diam” dan teks yang diketik dari IT-PC-1 akan muncul pada terminal di DB-Server-1).

Contoh:

- Di IT-PC-1:

  ```bash
  nc 10.78.2.226 80
  nanti muncul di sana
  ```

- Di DB-Server-1:

  ```bash
  nc -l -p 80
  nanti muncul di sana
  ```

Hal ini menunjukkan bahwa koneksi ke DB-Server-1 port 80 berhasil dilakukan di dalam rentang waktu yang diizinkan.

Pengujian akses yang diblokir (gagal)

Untuk membuktikan bahwa port selain 80 dan 443 diblokir, dilakukan pengujian berikut.

1. Jalankan `netcat` pada DB-Server-1 di port 22 (hanya sebagai contoh port yang tidak diizinkan):

   ```bash
   nc -l -p 22
   ```

2. Dari IT-PC-1, coba koneksi ke DB-Server-1 port 22:

   ```bash
   nc 10.78.2.226 22
   ```

3. Karena port 22 tidak termasuk dalam rule `ACCEPT` dan rule `DROP` untuk semua TCP ke DB-Server sudah aktif, koneksi akan gagal. Terminal client akan tampak menunggu tanpa respon hingga pengguna menekan `Ctrl+C`.

Contoh:

- Di IT-PC-1:

  ```bash
  root@IT-PC-1:/# nc 10.78.2.226 22
  ```

Di DB-Server-1 (listener) tidak menerima input apa pun.

Ini menjadi bukti bahwa port selain 80 dan 443 telah diblokir, meskipun pengujian dilakukan pada jam yang masih berada di rentang 07:00–22:00.

### Screenshot
<img width="909" height="135" alt="image" src="https://github.com/user-attachments/assets/eb5d52a4-9040-4b60-b6b9-5671e0917009" />
<img width="917" height="151" alt="image" src="https://github.com/user-attachments/assets/0a0cc98a-257d-4217-bb9d-b2e8b3021386" />
<img width="420" height="54" alt="image" src="https://github.com/user-attachments/assets/4ccb2f2b-8565-46bb-ba68-3f168d947ca4" />

<img width="420" height="54" alt="image" src="https://github.com/user-attachments/assets/e529ff5e-7523-4dd9-995d-c7a577111c57" />


### Explanation

Rule `ACCEPT` pertama menggunakan modul `state` untuk memastikan bahwa koneksi balasan yang sudah berjalan tetap diizinkan.  
Empat rule `ACCEPT` berikutnya secara spesifik hanya mengizinkan trafik TCP ke DB-Server-1 (`10.78.2.226`) dan DB-Server-2 (`10.78.2.194`) pada port 80 dan 443, dengan batasan hari Senin–Sabtu serta jam 07:00–22:00. Modul `time` memungkinkan pembatasan berdasarkan hari dan jam ini.

Selanjutnya, dua rule `DROP` bertindak sebagai lapisan proteksi terakhir yang memblokir semua trafik TCP lain ke kedua DB-Server. Hxal ini menjamin bahwa port selain 80 dan 443 (misalnya port 22) tidak dapat diakses, meskipun dari subnet manapun dan meskipun berada dalam rentang waktu yang diizinkan.

Melalui pengujian dengan `netcat`, dibuktikan bahwa:
1. Akses ke DB-Server-1 port 80 dari IT-PC-1 berhasil ketika pengujian dilakukan pada jam yang diizinkan.
2. Akses ke DB-Server-1 port 22 gagal, meskipun masih dalam jam kerja, karena tidak termasuk dalam port yang diizinkan

<br>

## Soal 8

> Kemudian, buat agar “Web-Server-1” dan “Web-Server-2” hanya memperbolehkan traffic bertipe HTTP.

### Answer:

- Tujuan
    - Membatasi seluruh traffic menuju Web-Server-1 dan Web-Server-2 sehingga **hanya port 80 (HTTP)** yang diizinkan.  
    - Port lainnya seperti **22 (SSH), 443 (HTTPS)** dan semua lain-lain **ditolak**.

> Langkah 1 - Instalasi iptables

Jalankan pada **router-4**:

```bash
apt update
apt install iptables -y
```

> Langkah 2 - Tambahkan rule firewall

Izinkan HTTP ke Web-Server
```bash
iptables -A FORWARD -p tcp -d 10.78.2.130 --dport 80 -j ACCEPT
iptables -A FORWARD -p tcp -d 10.78.2.162 --dport 80 -j ACCEPT
```
- Penjelasan:
    - `-A FORWARD` aturan dipasang di chain FORWARD (paket yang “lewat” router).
    - `-p tcp` hanya berlaku untuk protokol TCP.
    - `-d 10.78.2.130 / 10.78.2.162` hanya paket yang menuju web server.
    - `--dport 80` hanya untuk port tujuan 80 (HTTP).
    - `-j ACCEPT` paket ini diizinkan lewat.

Blok semua TCP lain ke Web-Server
```bash
iptables -A FORWARD -p tcp -d 10.78.2.130 -j DROP
iptables -A FORWARD -p tcp -d 10.78.2.162 -j DROP
```

> Langkah 3 - Verifikasi

A. Pengujian akses yang diizinkan (berhasil)

1. Di node `Web-Server-1`, jalankan `netcat` sebagai server pada port 80:
   ```bash
   service nginx stop
   ```
   Stop sementara untuk `nginx` yang sebelumnya listen di `port 80` lalu jalankan perintah `netcat` di bawah
   ```bash
   nc -l -p 80
   ```

2. Pada node `IT-PC-1` (atau client lain), lakukan koneksi ke Web-Server-1 port 80:

   ```bash
   nc 10.78.2.130 80
   ```

3. Jika rule benar, koneksi akan tersambung (terminal `nc` akan terlihat “diam” dan teks yang diketik dari IT-PC-1 akan muncul pada terminal di Web-Server-1).

Contoh:

- Di IT-PC-1:

  ```bash
  nc 10.78.2.130 80
  nanti muncul di sana
  ```

- Di Web-Server-1:

  ```bash
  nc -l -p 80
  nanti muncul di sana
  ```

Hal ini menunjukkan bahwa koneksi ke Web-Server-1 port 80(`HTTP`) berhasil dilakukan.

### Screenshot
<img width="710" height="129" alt="image" src="https://github.com/user-attachments/assets/0f85d8c0-e8e7-4c0e-8d99-89981852f6a5" />
<img width="716" height="189" alt="image" src="https://github.com/user-attachments/assets/8ce740b2-58b3-4560-9ca4-fc87c5c88673" />


### Explanation

Untuk melindungi Web Server, aturan firewall diterapkan menggunakan **iptables**, yaitu mekanisme packet filtering pada Linux.

- Konsep:
    - Rule **ACCEPT** ditempatkan paling atas untuk mengizinkan IT-PC sebagai sumber terpercaya.
    - Rule **DROP** di bawahnya menolak seluruh permintaan SSH lainnya.
    - HTTP bekerja pada port **80**, sehingga pembatasan difokuskan pada port tersebut.

<br>

## Soal 9

> Pilih salah satu Subnet dan lakukan blokir terhadap semua request protokol ICMP (ping) dari luar subnet terhadap subnet tersebut.

### Answer:

- Tujuan:
    - Melakukan konfigurasi firewall agar:
        - Semua request ICMP (ping) dari luar subnet ditolak,
        - Tetapi komunikasi ICMP di dalam subnet tetap diperbolehk

> Langkah 1 - Instalasi iptables

Jalankan pada **router-1**:

```bash
apt update
apt install iptables -y
```

> Langkah 2 - Tambahkan rule firewall

Tambahkan aturan untuk memblokir ICMP dari luar subnet
```bash
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -p icmp -d 10.78.2.0/25 -j DROP
```
- Penjelasan penting:
    - `iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT` Izinkan semua koneksi yang sudah ada sebelumnya (ping ke-luar, etc.)
    - `-A FORWARD` firewall bekerja pada traffic antar interface (routing).
    - `-p icmp` memfilter paket ping.
    - `-d 10.78.2.0/25` tujuan paket adalah subnet IT-LAN.
    - `-j DROP` paket dibuang.

Jadi **semua ping dari luar subnet ke subnet IT-LAN akan gagal.**

> Lanbgkah 3 - Verfikasi / testing

A. Testing Gagal
Dari `HR-PC-1` (IP: 10.78.0.2), jalankan:
```bash
ping 10.78.2.2
```
Jika benar, **maka akan gagal**

B. Testing Berhasil
Dari `IT-PC-1` (IP: 10.78.2.2) ke sesama IT-LAN:
```bash
ping 10.78.2.3
```
Jika benar, **maka akan berhasil**


### Screenshot
<img width="908" height="309" alt="image" src="https://github.com/user-attachments/assets/c70e7eae-34a6-434f-938e-0d449973cf24" />
<img width="741" height="233" alt="image" src="https://github.com/user-attachments/assets/8374508e-c3e7-413d-961e-4938c188a2c0" />


### Explanation

Pada soal ini, kita perlu **membatasi** akses `ICMP` antar subnet. ICMP (Internet Control Message Protocol) digunakan untuk operasi ping. Tanpa konfigurasi firewall, ICMP akan bebas melewati router.

Dengan menggunakan iptables:
```bash
iptables -A FORWARD -p icmp -d 10.78.2.0/25 -j DROP
```
- memberlakukan aturan:
    - ICMP dari luar subnet 10.78.2.0/25 di-block
    - ICMP dari dalam subnet diperbolehkan
- Firewall bekerja di router-1 sebagai gateway subnet IT-LAN. Dengan hasil:
    - Komputer di luar subnet tidak dapat mengirim ping ke IT-LAN
    - Komputer di dalam subnet tetap dapat berkomunikasi normal

<br>

## Soal 10

> Konfigurasikan fitur logging untuk melakukan log terhadap seluruh paket yang di-DROP pada lalu lintas setiap node.

### Answer:

- Tujuan:
    - Mengonfigurasi logging pada router menggunakan `ulogd2` untuk mencatat seluruh paket yang di-DROP.
 
> Langkah 1 - Konfigurasi ulogd2

Jalankan `instalasi ulogd2` pada semua `router`
```bash
apt update
apt install ulogd2 -y
service ulogd2 start
```

> Langkah 2 - Menentukan Lokasi Log ulogd2

Periksa konfigurasi ulogd2:
```bash
nano /etc/ulogd.conf
```
dan cari baris seperti berikut:
```
file="/var/log/ulog/syslogemu.log"
```
maka log akan disimpaan di file:
```
/var/log/ulog/syslogemu.log
```

> Langkah 3 - Edit rule iptables yang sudah ada

3.1 - Flush rule yang sudah ada
```bash
iptables -F
```
Kita flush terlebih dahulu, untuk mengubahnya dengan rule baru nantinya. 

3.2 - Membuat Chain Logging Khusus
Selanjutnya kita buat chain logging: `NFLOG_DROP`
```bash
iptables -N NFLOG_DROP 2>/dev/null || true
iptables -A NFLOG_DROP -j NFLOG --nflog-prefix "DROP-IT-ICMP: " --nflog-group 0
iptables -A NFLOG_DROP -j DROP
```

3.3 - Menambahkan Rule DROP Dengan Logging
Kita tambahkan rule yang sudah ada sebelumnya dengan mengubah baris yang awal-nya `DROP` menjadi `NFLOG_DROP`. Berlaku untuk semua rule firewall.

```bash
DROP >> NFLOG_DROP
```

> Langkah 4 - Testing pemicu logging

Lakukan testing yang memicu kegagalan berdasarkan nomer-nomer sebelumnya.

> Langkah 5 - Melihat Log ulogd2
Di router yang dijasikan testing untuk pemicu logging, jalankan:
```bash
tail -f /var/log/ulog/syslogemu.log
```

### Screenshot
<img width="904" height="643" alt="image" src="https://github.com/user-attachments/assets/6aa18d61-e459-4a8f-b22b-be0782b89874" />

### Explanation

Untuk melakukan logging paket yang di-DROP, digunakan tool `ulogd2`, yang **mengambil log dari kernel melalui NFLOG** dan menuliskannya ke userspace. Setelah ulogd2 di-install dan dijalankan, konfigurasi file `/etc/ulogd.conf` digunakan untuk menentukan direktori penyimpanan log.

- Pada router, rule logging dibuat dengan menambahkan target NFLOG sebelum rule DROP, sehingga setiap paket yang memenuhi kriteria DROP akan dicatat terlebih dahulu oleh ulogd2. Contoh penerapannya adalah pembuatan chain `NFLOG_DROP` yang berisi:
    - `NFLOG` (untuk mencatat paket)
    - `DROP` (untuk memblokir paket)

Rule yang sudah ada kemudian diarahkan ke chain ini, sehingga paket yang seharusnya di-DROP akan melalui proses logging terlebih dahulu. Dengan demikian, setiap percobaan akses yang diblokir dapat dilihat melalui file log di `/var/log/ulog/`.

<br>
  
## Problems

Ada materi yang tidak dijelaskan dalam modul, yaitu logging. Jadi harus menggunakan modul tambahan, dan ternyata hal ini sudah menjadi problem di tahun sebelumnya(?) bersumber dari praktikum mas rey sebelumnya yang dapat diakses [di sini](https://github.com/ReynandrielPT/Rey-s-Jarkom-PractModules/tree/main/jarkom-5-j24-f11#problems), hehe ;).
## Revisions (if any)

Harusnya dan semoga saja tidak ada.
