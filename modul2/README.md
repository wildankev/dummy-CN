[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/1niUih_B)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| A. Wildan Kevin Assyauqi | 5025241265 | Jaringan Komputer (A) |



## Put your topology config image here!
<img width="1242" height="753" alt="image" src="https://github.com/user-attachments/assets/bd7a04c6-c5ee-40b7-9d68-409286359307" />


## Put your GNS3 Project file here!
[https://drive.google.com/file/d/1-6RjDp89EMTVeSyMiKCgyc5Pu-CBFO3B/view?usp=sharing](https://drive.google.com/file/d/1-6RjDp89EMTVeSyMiKCgyc5Pu-CBFO3B/view?usp=sharing)

<br>

## Soal 1

> Dokumentasikan hasil pengelompokan subnet yang telah dibuat.

### Answer:

Prefix IP: **10.78**  
Netmask: **/24 (255.255.255.0)** untuk semua subnet.

**A. Subnet LAN**
| Switch | Subnet /24   | Gateway (Router) | Host Anggota |
|--------|--------------|------------------|--------------|
| Switch1 | 10.78.3.0/24 | BlackPanther:eth1 = 10.78.3.1 | CaptainAmerica (10.78.3.10), Falcon (10.78.3.20–25 DHCP) |
| Switch2 | 10.78.4.0/24 | BlackPanther:eth2 = 10.78.4.1 | WinterSoldier (10.78.4.10), Hawkeye (10.78.4.30–35 DHCP) |
| Switch3 | 10.78.5.0/24 | BlackWidow:eth2 = 10.78.5.1  | Thor (10.78.5.100–105 DHCP), ScarletWitch (10.78.5.40–45 DHCP) |
| Switch4 | 10.78.6.0/24 | Vision:eth0 = 10.78.6.1       | Hulk (10.78.6.50–55 DHCP) |
| Switch5 | 10.78.7.0/24 | Vision:eth1 = 10.78.7.1       | SpiderMan (10.78.7.60–65 DHCP), DoctorStrange (10.78.7.110–115 DHCP) |

**B. Subnet Antar-Router**
| Link | Subnet /24   | IP Sisi A | IP Sisi B |
|------|--------------|-----------|-----------|
| BlackPanther ↔ IronMan | 10.78.100.0/24 | BlackPanther:10.78.100.2 | IronMan:10.78.100.1 |
| IronMan ↔ BlackWidow   | 10.78.101.0/24 | IronMan:10.78.101.1 | BlackWidow:10.78.101.2 |

**C. WAN**
| Node    | Interface | Konfigurasi |
|---------|-----------|-------------|
| IronMan | eth0      | DHCP dari NAT |

#### Explanation:

  Pengelompokan subnet dilakukan dengan **meninjau topologi** dan menentukan **pemisahan jaringan berdasarkan router yang menghubungkan setiap switch.** Semua subnet memakai `netmask /24 (255.255.255.0)`, sehingga tiap `LAN` punya blok `IP` **unik** tanpa konflik. Hasilnya terdapat **5 subnet utama** dari `LAN3` sampai `LAN7`, yang masing-masing memiliki `gateway`, rentang `DHCP`, dan `klien` yang spesifik.

**Rangkuman Subnet:**

- LAN3 – Falcon
Prefix `10.78.3.0/24`, Gateway `10.78.3.1`, Rentang `10.78.3.20–25`

- LAN4 – Hawkeye
Prefix `10.78.4.0/24`, Gateway `10.78.4.1`, Rentang `10.78.4.30–35`

- LAN5 – ScarletWitch & Thor
Prefix `10.78.5.0/24`, Gateway `10.78.5.1`, Rentang ScarletWitch `10.78.5.40–45`, Thor `10.78.5.100–105`

- LAN6 – Hulk
Prefix `10.78.6.0/24`, Gateway `10.78.6.1`, Rentang `10.78.6.50–55`

- LAN7 – SpiderMan & DoctorStrange
Prefix `10.78.7.0/24`, Gateway `10.78.7.1`, Rentang SpiderMan `10.78.7.60–65`, DoctorStrange `10.78.7.110–115`

<br>

## Soal 2

> Lakukan konfigurasi routing agar setiap node dapat saling berkomunikasi. Pastikan setiap router dapat mengirimkan paket ke jaringan lain melalui tabel routing yang sesuai. Sertakan bukti bahwa Falcon bisa melakukan ping ke SpiderMan, DoctorStrange, dan ScarletWitch.

### Answer

Tujuan: agar semua node dapat saling berkomunikasi.  
Metode: Konfigurasi routing pada router IronMan, BlackPanther, BlackWidow, dan Vision.  
Bukti: Falcon dapat ping ke SpiderMan, DoctorStrange, dan ScarletWitch.

**Konfigrasi setiap `router`**

**IronMan**
```bash
auto lo
iface lo inet loopback

# WAN ke NAT
auto eth0
iface eth0 inet dhcp

# Transit ke BlackPanther
auto eth1
iface eth1 inet static
    address 10.78.100.1
    netmask 255.255.255.0

# Transit ke BlackWidow
auto eth2
iface eth2 inet static
    address 10.78.101.1
    netmask 255.255.255.0

# Routing antar subnet
post-up sysctl -w net.ipv4.ip_forward=1
post-up ip route add 10.78.3.0/24 via 10.78.100.2
post-up ip route add 10.78.4.0/24 via 10.78.100.2
post-up ip route add 10.78.5.0/24 via 10.78.101.2
post-up ip route add 10.78.6.0/24 via 10.78.101.2
post-up ip route add 10.78.7.0/24 via 10.78.101.2
```
Explanation:
- Mengaktifkan `IP forwarding` agar paket bisa diteruskan.
- Menambahkan rute manual ke subnet 3-7 agar IronMan tahu jalurnya via router lain.

**BlackPanther**
```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.100.2
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 10.78.3.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 10.78.4.1
    netmask 255.255.255.0

post-up sysctl -w net.ipv4.ip_forward=1
post-up ip route add default via 10.78.100.1
```
Explanation:
- BlackPanther menghubungkan LAN3 & LAN4 dengan IronMan.
- Default route diarahkan ke IronMan (`10.78.100.1`).

**BlackWidow**
```bash
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
    address 10.78.101.2
    netmask 255.255.255.0

# LAN6
auto eth1
iface eth1 inet static
    address 10.78.6.2
    netmask 255.255.255.0

# Gateway LAN5
auto eth2
iface eth2 inet static
    address 10.78.5.1
    netmask 255.255.255.0

post-up sysctl -w net.ipv4.ip_forward=1
post-up ip route add default via 10.78.101.1
# Rute ke LAN7 melalui Vision (gateway LAN7)
post-up ip route add 10.78.7.0/24 via 10.78.6.1
```
Explanation:
- BlackWidow menghubungkan LAN5 & LAN6.
- Default route ke IronMan (`10.78.101.1`).
- Rute ke LAN7 diarahkan via Vision (`10.78.6.1`).

**Vision**
```bash
auto lo
iface lo inet loopback

# Gateway LAN6
auto eth0
iface eth0 inet static
    address 10.78.6.1
    netmask 255.255.255.0

# Gateway LAN7
auto eth1
iface eth1 inet static
    address 10.78.7.1
    netmask 255.255.255.0

post-up sysctl -w net.ipv4.ip_forward=1
# Naik ke 'atas' via BlackWidow
post-up ip route add default via 10.78.6.2
```
Explanation:
- Vision menghubungkan LAN6 & LAN7.
- Default route diarahkan ke BlackWidow (10.78.6.2).

**Konfigurasi Host (sementara statis untuk kebutuhan pengujian)**
**Falcon**
```bash
auto eth0
iface eth0 inet static
    address 10.78.3.20
    netmask 255.255.255.0
    gateway 10.78.3.1
```

**CaptainAmerica**
```bash
auto eth0
iface eth0 inet static
    address 10.78.3.10
    netmask 255.255.255.0
    gateway 10.78.3.1
```

**Hawkeye**
```bash
auto eth0
iface eth0 inet static
    address 10.78.4.30
    netmask 255.255.255.0
    gateway 10.78.4.1
```

**WinterSoldier**
```bash
auto eth0
iface eth0 inet static
    address 10.78.4.10
    netmask 255.255.255.0
    gateway 10.78.4.1
```

**ScarletWitch**
```bash
auto eth0
iface eth0 inet static
    address 10.78.5.40
    netmask 255.255.255.0
    gateway 10.78.5.1
```

**Thor**
```bash
auto eth0
iface eth0 inet static
    address 10.78.5.100
    netmask 255.255.255.0
    gateway 10.78.5.1
```

**Hulk**
```bash
auto eth0
iface eth0 inet static
    address 10.78.6.50
    netmask 255.255.255.0
    gateway 10.78.6.1
```

**SpiderMan**
```bash
auto eth0
iface eth0 inet static
    address 10.78.7.60
    netmask 255.255.255.0
    gateway 10.78.7.1
```

**DoctorStrange**
```bash
auto eth0
iface eth0 inet static
    address 10.78.7.110
    netmask 255.255.255.0
    gateway 10.78.7.1
```
Explanation:
- Host menggunakan IP statis **sementara** sesuai subnet.
- Gateway **diarahkan** ke router terdekat agar bisa menjangkau subnet lain.

Verifikasi Routing
- Dari `Falcon`, bisa melakukan `ping` pada `SpiderMan`, `DoctorStrange`, `ScarletWitch`
```
ping 10.78.7.60     # SpiderMan
ping 10.78.7.110    # DoctorStrange
ping 10.78.5.40     # ScarletWitch
```

#### Screenshot
<img width="1006" height="664" alt="image" src="https://github.com/user-attachments/assets/24da9217-07b0-4e22-874b-1bcddc98cebc" />

#### Explanation

  Agar semua node dapat saling berkomunikasi, setiap router **perlu dikonfigurasi routing-nya.** Hal ini dilakukan dengan mengaktifkan `IP forwarding` dan menambahkan `tabel routing` sehingga paket dari satu subnet **bisa mencapai** subnet lain melalui jalur yang benar. `IronMan` sebagai router utama bertugas mengarahkan trafik ke seluruh jaringan, sementara `BlackPanther`, `BlackWidow`, dan `Vision` berfungsi sebagai `router transit` yang meneruskan paket ke subnet masing-masing.

Rangkuman Routing:
- IronMan: Router utama, menghubungkan router dari semua subnet (3–7) melalui BlackPanther dan BlackWidow.
- BlackPanther: Menghubungkan LAN3 dan LAN4, default route diarahkan ke IronMan.
- BlackWidow: Menghubungkan LAN5 dan LAN6, serta membuat route ke LAN7 melalui Vision.
- Vision: Router untuk LAN6 dan LAN7, default route ke BlackWidow.
- Host: sementara dikonfigurasi statis, dengan gateway masing-masing router terdekat.

<br>

## Soal 3

> Lakukan konfigurasi agar semua node dapat terhubung ke internet. Sertakan hasil uji coba dengan melakukan ping ke google.com dari node Falcon, CaptainAmerica, SpiderMan, dan Thor.

### Answer
Tujuan: agar node dapat mengakses internet melalui NAT di IronMan.

**Konfigurasi NAT di IronMan**
tambahkan sintaks di bawah melalui edit konfigurasi
```bash
post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
pre-down iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```
Explanation:
- `post-up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`
Mengaktifkan NAT (Network Address Translation) pada interface `eth0` (yang terhubung ke internet). Semua paket dari jaringan internal akan diterjemahkan menggunakan IP publik milik IronMan.
- `pre-down iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE`
Menghapus aturan NAT ketika interface eth0 dimatikan, agar konfigurasi bersih dan tidak menumpuk.

Dengan ini, node di jaringan internal **bisa akses internet melalui IronMan.**

**DNS Resolver (sementara)**
Tambahkan di host client melalui edit konfigurasi:
```bash
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```
Explanation:
- `dns-nameservers 8.8.8.8 1.1.1.1`, Menentukan DNS server yang digunakan adalah Google (`8.8.8.8`) dan Cloudflare (`1.1.1.1`).
- `post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf`, Menuliskan DNS Google ke file resolver utama `/etc/resolv.conf`.
- `post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf`, Menambahkan DNS Cloudflare ke baris berikutnya.

Dengan konfigurasi ini, klien dapat melakukan resolusi nama domain ke alamat IP, sehingga akses internet (misalnya ping google.com) dapat berjalan lancar.

**Verifikasi Internet**
dari host lakukan pengujian:
```bash
ping 8.8.8.8
ping google.com
```

#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/37bd5935-7b3e-4d13-a16e-e1d2ce663df4" />

#### Explanation
Agar semua node dalam topologi dapat mengakses internet, digunakan teknik Network Address Translation (NAT) pada router utama (IronMan). Dengan NAT, alamat IP privat dari subnet-subnet internal dapat diterjemahkan ke alamat IP publik milik IronMan, sehingga paket dapat keluar ke internet. Selain itu, setiap klien juga perlu disediakan DNS resolver agar bisa menerjemahkan nama domain (misalnya google.com) menjadi alamat IP.

Rangkuman Konfigurasi:
- NAT di IronMan
Menggunakan aturan `iptables MASQUERADE` pada interface `eth0` sehingga semua node internal bisa berbagi koneksi keluar menggunakan IP publik IronMan.

- DNS Resolver di Klien
Menambahkan nameserver `8.8.8.8` (Google) dan `1.1.1.1` (Cloudflare) di file konfigurasi jaringan dan /etc/resolv.conf, agar resolusi domain berjalan.

- Verifikasi
Pengujian dengan `ping 8.8.8.8` memastikan koneksi internet berjalan, sedangkan `ping google.com` memastikan DNS resolver berfungsi.

<br>

## Soal 4

> Berikan Falcon alamat IP dalam rentang [Prefix IP].3.20 - [Prefix IP].3.25
> <br> </br>
> Berikan Hawkeye alamat IP dalam rentang [Prefix IP].4.30 - [Prefix IP].4.35
> <br> </br>
> Berikan Hulk alamat IP dalam rentang [Prefix IP].6.50 - [Prefix IP].6.55

### Answer

**Langkah A - Setup ISC DHCP Server di CaptainAmerica**

Pastikan komfigurasi yang berjalan di node `CaptainAmerica` adalah sebagai berikut:
```bash
auto eth0
iface eth0 inet static
    address 10.78.3.10
    netmask 255.255.255.0
    gateway 10.78.3.1
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```
lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-server
```
Set interface layanan DHCP
```bash
nano /etc/default/isc-dhcp-server
```
ubah baris menjadi:
```bash
INTERFACESv4="eth0"
```
Konfigurasi tiga subnet + pool sesuai soal
```bash
rm /etc/dhcp/dhcpd.conf
nano /etc/dhcp/dhcpd.conf
```
isi dengan kode berikut:
```conf
# Subnet LAN3 - Falcon
subnet 10.78.3.0 netmask 255.255.255.0 {
  option routers 10.78.3.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  pool {
    range 10.78.3.20 10.78.3.25;
  }
}

# Subnet LAN4 - Hawkeye
subnet 10.78.4.0 netmask 255.255.255.0 {
  option routers 10.78.4.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  pool {
    range 10.78.4.30 10.78.4.35;
  }
}

# Subnet LAN6 - Hulk
subnet 10.78.6.0 netmask 255.255.255.0 {
  option routers 10.78.6.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  pool {
    range 10.78.6.50 10.78.6.55;
  }
}
```
Explanation
- `subnet … netmask`, Mendefinisikan jaringan subnet yang digunakan.  
- `option routers`, Menentukan gateway default untuk subnet.  
- `option domain-name-servers`, Menentukan DNS server yang digunakan (Google & Cloudflare).  
- `range …`, Menentukan rentang IP yang akan diberikan DHCP Server ke klien.  

Dengan ini, Falcon hanya bisa dapat IP `10.78.3.20–25`, Hawkeye `10.78.4.30–35`, dan Hulk `10.78.6.50–55`.

Start/restart service, dan cek running:
```bash
service isc-dhcp-server restart
service isc-dhcp-server status # pastikan Active: running
```

**Langkah B - Setup DHCP Relay di BlackPanther & Vision**

Pastikan konfigurasi `BlackPanther` dan `Vision` ada kode berikut:
```bash
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

**BlackPanther (Relay untuk LAN4)**
lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-relay
```
Set relay
```bash
nano /etc/default/isc-dhcp-relay
```
ubah baris menjadi:
```conf
SERVERS="10.78.3.10"
INTERFACES="eth2 eth1"
OPTIONS=""
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-relay restart
service isc-dhcp-relay status # pastikan Active: running
```

**Vision (Relay untuk LAN6)**
lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-relay
```
Set relay
```bash
nano /etc/default/isc-dhcp-relay
```
ubah baris menjadi:
```conf
SERVERS="10.78.3.10"
INTERFACES="eth0"
OPTIONS=""
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-relay restart
service isc-dhcp-relay status
```

**Langkah C — Ubah 3 Klien ke DHCP mode (via Configure)**

Ganti yang ada pada konfigurasi node dengan yang ada di bawah ini:

**Falcon (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname falcon
```

**Hawkeye (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname hawkeye
```

**Hulk (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname hulk
```

**Verifikasi**
```bash
ip -br addr #cek alamat IP, harus berada di range yang ditentukan
ping google.com #cek sambung internt 
```

#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/654c1962-6b1e-4782-9bd7-a6303f8e2e7a" />


#### Explanation
Untuk membatasi alamat IP yang diterima klien, dilakukan konfigurasi `DHCP pool` pada `DHCP server` (`CaptainAmerica`). Dengan cara ini, `Falcon`, `Hawkeye`, dan `Hulk` masing-masing hanya bisa memperoleh `IP` dalam rentang tertentu sesuai subnet-nya. Setiap subnet tetap menggunakan netmask /24 (255.255.255.0) dan diarahkan ke gateway router terdekat.

Rangkuman Pool DHCP:
- Falcon (LAN3)
Mendapatkan IP dari rentang `10.78.3.20`–`10.78.3.25`, gateway `10.78.3.1`.
- Hawkeye (LAN4)
Mendapatkan IP dari rentang `10.78.4.30`–`10.78.4.35`, gateway `10.78.4.1`.
- Hulk (LAN6)
Mendapatkan IP dari rentang `10.78.6.50`–`10.78.6.55`, gateway `10.78.6.1`.

<br>

## Soal 5

> Berikan ScarletWitch dan Thor alamat IP dalam rentang [Prefix IP].5.40 - [Prefix IP].5.45 dan [Prefix IP].5.100 - [Prefix IP].5.105

### Answer

**Langkah A - Setup ISC DHCP Server di CaptainAmeric**

Masih, melanjutkan nomer yang sebelumnya, yaitu nomer 4. **Tambahkan** konfigurasi dua subnet + pool sesuai soal
```bash
nano /etc/dhcp/dhcpd.conf
```
**Tambahkan kode berikut:**
```conf
subnet 10.78.5.0 netmask 255.255.255.0 {
  option routers 10.78.5.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  class "scarletwitch" { match if option host-name = "scarletwitch"; }
  pool {
    allow members of "scarletwitch";
    range 10.78.5.40 10.78.5.45;
  }

  class "thor" { match if option host-name = "thor"; }
  pool {
    allow members of "thor";
    range 10.78.5.100 10.78.5.105;
  }
}
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-server restart
service isc-dhcp-server status # pastikan Active: running
```

**Langkah B - Setup DHCP Relay di BlackWidow**

Pastikan konfigurasi `BlackWidow` ada kode berikut:
```bash
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

**BlackWidow (Relay untuk LAN5)**
lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-relay
```
Set relay
```bash
nano /etc/default/isc-dhcp-relay
```
ubah baris menjadi:
```conf
SERVERS="10.78.3.10"
INTERFACES="eth2 eth0"
OPTIONS=""
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-relay restart
service isc-dhcp-relay status
```

**Langkah C - Ubah 3 Klien ke DHCP mode (via Configure)**

**ScarletWitch (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname scarletwitch
```

**Thor (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname thor
```

**Verifikasi**
```bash
ip -br addr #cek alamat IP, harus berada di range yang ditentukan
ping google.com #cek sambung internt 
```

#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/b3c32d0d-839e-457d-a3f7-c77b03d1956f" />


#### Explanation
Untuk membatasi alamat IP yang diterima klien, dilakukan konfigurasi `DHCP pool` pada `DHCP server` (`CaptainAmerica`). Dengan cara ini, `Thor`, dan `ScarletWitch` masing-masing hanya bisa memperoleh `IP` dalam rentang tertentu sesuai subnet-nya. Setiap subnet tetap menggunakan netmask /24 (255.255.255.0) dan diarahkan ke gateway router terdekat.

Rangkuman Pool DHCP:
- Thor
Mendapatkan IP dari rentang `10.78.5.40`–`10.78.5.45`
- ScarletWitch
Mendapatkan IP dari rentang `10.78.5.100`–`10.78.5.105`
<br>

## Soal 6

> Berikan SpiderMan dan DoctorStrange alamat IP dalam rentang [Prefix IP].7.60 - [Prefix IP].7.65  dan [Prefix IP].7.110 - [Prefix IP].7.115

### Answer

**Langkah A - Setup ISC DHCP Server di CaptainAmeric**

Masih, melanjutkan nomer yang sebelumnya, yaitu nomer 5. **Tambahkan** konfigurasi dua subnet + pool sesuai soal
```bash
nano /etc/dhcp/dhcpd.conf
```
**Tambahkan kode berikut:**
```conf
subnet 10.78.7.0 netmask 255.255.255.0 {
  option routers 10.78.7.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  class "spiderman" { match if option host-name = "spiderman"; }
  pool {
    allow members of "spiderman";
    range 10.78.7.60 10.78.7.65;
  }

  class "doctorstrange" { match if option host-name = "doctorstrange"; }
  pool {
    allow members of "doctorstrange";
    range 10.78.7.110 10.78.7.115;
  }
}
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-server restart
service isc-dhcp-server status # pastikan Active: running
```

**Langkah B - Setup DHCP Relay di Vision**

Pastikan konfigurasi `Vision` ada kode berikut:
```bash
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

**Vision (Relay untuk LAN6)**
lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-relay
```
Set relay
```bash
nano /etc/default/isc-dhcp-relay
```
ubah baris menjadi:
```conf
SERVERS="10.78.3.10"
INTERFACES="eth0 eth1"
OPTIONS=""
```
Start/restart service, dan cek running:
```bash
service isc-dhcp-relay restart
service isc-dhcp-relay status
```

**Langkah C - Ubah 3 Klien ke DHCP mode (via Configure)**

**SpiderMan (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname spiderman
```

**DoctorStrange (Client)**
```bash
auto eth0
iface eth0 inet dhcp
hostname doctorstrange
```

**Verifikasi**
```bash
ip -br addr #cek alamat IP, harus berada di range yang ditentukan
ping google.com #cek sambung internt 
```
#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/068edd62-69bb-4a25-981b-b641a8e2ffbd" />


#### Explanation
Untuk membatasi alamat IP yang diterima klien, dilakukan konfigurasi `DHCP pool` pada `DHCP server` (`CaptainAmerica`). Dengan cara ini, `SpiderMan`, dan `DoctorStrange` masing-masing hanya bisa memperoleh `IP` dalam rentang tertentu sesuai subnet-nya. Setiap subnet tetap menggunakan netmask /24 (255.255.255.0) dan diarahkan ke gateway router terdekat.

Rangkuman Pool DHCP:
- SpiderMan
Mendapatkan IP dari rentang `10.78.7.60`–`10.78.7.65`
- DoctorStrange
Mendapatkan IP dari rentang `10.78.7.110`–`10.78.7.115`
<br>

<br>

## Soal 7

> Tetapkan waktu peminjaman alamat IP pada DHCP server untuk client yang terhubung melalui Switch 2 selama 5 menit (Default), dan untuk client melalui Switch 5 selama 10 menit (Default). Tetapkan juga batas waktu peminjaman maksimal selama 2 jam.
> <br> </br>
> Tetapkan waktu peminjaman alamat IP pada DHCP server untuk client yang terhubung melalui Switch 1 dan Switch 3 selama 2 menit (Default). Tetapkan juga batas waktu peminjaman maksimal selama 100 menit.

### Answer
**Tujuan**
Mengatur lease time per subnet sesuai ketentuan soal:
- Switch2 (LAN4) → Default 5 menit, Maksimal 2 jam
- Switch5 (LAN7) → Default 10 menit, Maksimal 2 jam
- Switch1 (LAN3) & Switch3 (LAN5) → Default 2 menit, Maksimal 100 menit

**Strategi**
- Lease time diatur pada DHCP server (CaptainAmerica) melalui parameter:
  - `default-lease-time`
  - `max-lease-time`
- Konversi menit menjadi detik:
  - 2 menit = 120, 5 menit = 300, 10 menit = 600, 100 menit = 6000, 2 jam = 7200

**Langkah A - Setup ISC DHCP Server di CaptainAmeric beserta waktu yang ditentukan**

Sama prosesnya seperti sebelumnya. **Tambahkan**  `default-lease-time` dan `max-lease-time` ke setiap subnet.
```bash
rm /etc/dhcp/dhcpd.conf
nano /etc/dhcp/dhcpd.conf
```

**Salin dan tempel kode berikut:**
```conf
# LAN3 (Switch1)
subnet 10.78.3.0 netmask 255.255.255.0 {
  option routers 10.78.3.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  default-lease-time 120;
  max-lease-time 6000;

  pool { range 10.78.3.20 10.78.3.25; }
}

# LAN4 (Switch2)
subnet 10.78.4.0 netmask 255.255.255.0 {
  option routers 10.78.4.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  default-lease-time 300;
  max-lease-time 7200;

  pool { range 10.78.4.30 10.78.4.35; }
}

# LAN5 (Switch3)
subnet 10.78.5.0 netmask 255.255.255.0 {
  option routers 10.78.5.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  default-lease-time 120;
  max-lease-time 6000;

  class "scarletwitch" { match if option host-name = "scarletwitch"; }
  pool {
    allow members of "scarletwitch";
    range 10.78.5.40 10.78.5.45;
    default-lease-time 120;
    max-lease-time 6000;
  }

  class "thor" { match if option host-name = "thor"; }
  pool {
    allow members of "thor";
    range 10.78.5.100 10.78.5.105;
    default-lease-time 120;
    max-lease-time 6000;
  }
}

# LAN7 (Switch5)
subnet 10.78.7.0 netmask 255.255.255.0 {
  option routers 10.78.7.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;

  default-lease-time 600;
  max-lease-time 7200;

  class "spiderman" { match if option host-name = "spiderman"; }
  pool {
    allow members of "spiderman";
    range 10.78.7.60 10.78.7.65;
    default-lease-time 600;
    max-lease-time 7200;
  }

  class "doctorstrange" { match if option host-name = "doctorstrange"; }
  pool {
    allow members of "doctorstrange";
    range 10.78.7.110 10.78.7.115;
    default-lease-time 600;
    max-lease-time 7200;
  }
}
```

Validasi & restart service, dan cek running:
```bash
dhcpd -t -cf /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
service isc-dhcp-server status
```
**Hapus State yang lama, agar tidak terjadi saling penumpukan**
Masukkan kode di bawah di setiap node:
```bash
pkill udhcpc 2>/dev/null
ip addr flush dev eth0
ip link set eth0 down
ip link set eth0 up
```

**Renew IP beserta waktu peminjaman**
```
udhcpc -i eth0 -x hostname:<namaHost> -n
```
untuk <namaHost> dapat diganti dengan hostname yang dituju, misal `SpiderMan`, `Thor`, dll.

#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/e77c1f7f-f299-4c24-8dcb-b272a7bbe843" />


#### Explanation

Konfigurasi ini bertujuan untuk mengatur waktu peminjaman alamat IP (lease time) berbeda-beda sesuai dengan subnet yang ditentukan. Dengan begitu, setiap kelompok klien memiliki aturan manajemen alamat IP yang sesuai kebutuhan jaringan. Pengaturan dilakukan pada DHCP server (CaptainAmerica) menggunakan parameter default-lease-time dan max-lease-time, dengan nilai dalam detik.

Rangkuman Aturan Lease Time:
- LAN4 (Switch2 – Hawkeye), Default 5 menit (300 detik), maksimal 2 jam (7200 detik)
- LAN7 (Switch5 – SpiderMan & DoctorStrange), Default 10 menit (600 detik), maksimal 2 jam (7200 detik)
- LAN3 (Switch1 – Falcon) & LAN5 (Switch3 – ScarletWitch & Thor), Default 2 menit (120 detik), maksimal 100 menit (6000 detik)
<br>

## Soal 8

> Ubah konfigurasi DHCP Server agar Hawkeye, Thor, dan SpiderMan mendapatkan IP statis dengan [Prefix IP].x.5, namun masih menggunakan DHCP.

### Answer

Maksud soal: ubah konfigurasi DHCP Server agar Hawkeye, Thor, dan SpiderMan tetap memakai DHCP, tetapi selalu mendapat IP statis bernilai [Prefix IP].x.5:
- Hawkeye → 10.78.4.5
- Thor → 10.78.5.5
- SpiderMan → 10.78.7.5

Strategi:
- Ambil MAC address masing-masing klien pada interface eth0.
- Tambahkan host declaration di dhcpd.conf berisi hardware ethernet <MAC> dan fixed-address <IP>.
- Restart DHCP server dan renew lease di klien.
- Verifikasi IP yang diterima adalah .x.5.

**Langkah Detail**
**Dapatkan MAC**
Jalankan kode berikut di masing-masing node
```bash
ip link show eth0
```
Output:
<img width="968" height="97" alt="image" src="https://github.com/user-attachments/assets/e4477bc7-52ab-47be-bff0-12a4f296466a" />

amati bagian `02:42:c28:e2:3d:00` itu merupakan MAC yang dituju.

**Setup ISC DHCP Server di CaptainAmerica**

Sama prosesnya seperti sebelumnya. **Tambahkan** `MAC` yang telah didapat.
```bash
nano /etc/dhcp/dhcpd.conf
```
**Salin dan tempel kode berikut:**
```bash
# Hawkeye (LAN4) -> 10.78.4.5
host hawkeye {
  hardware ethernet aa:bb:cc:dd:ee:01;  # GANTI sesuai MAC Hawkeye
  fixed-address 10.78.4.5;
}

# Thor (LAN5) -> 10.78.5.5
host thor {
  hardware ethernet aa:bb:cc:dd:ee:02;  # GANTI sesuai MAC Thor
  fixed-address 10.78.5.5;
}

# SpiderMan (LAN7) -> 10.78.7.5
host spiderman {
  hardware ethernet aa:bb:cc:dd:ee:03;  # GANTI sesuai MAC SpiderMan
  fixed-address 10.78.7.5;
}
```
Validasi & restart service, dan cek running:
```bash
dhcpd -t -cf /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
service isc-dhcp-server status
```
**Renew IP**
```
udhcpc -i eth0 -x hostname:<namaHost> -n
```
untuk <namaHost> dapat diganti dengan hostname yang dituju, misal `SpiderMan`, `Thor`, dll.

#### Screenshot
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/e19b73fa-534f-4d5c-bd09-533a483a751f" />


#### Explanation
Agar beberapa node tertentu tetap mendapatkan alamat IP yang konsisten walaupun menggunakan DHCP, digunakan teknik DHCP Reservation pada server DHCP (CaptainAmerica dan WinterSoldier). Dengan teknik ini, server akan mengenali perangkat berdasarkan alamat MAC milik masing-masing klien, kemudian memberikan IP yang sudah dipesan sebelumnya. Hal ini memungkinkan node-node penting (Hawkeye, Thor, dan SpiderMan) selalu memakai alamat yang sama, mempermudah proses administrasi jaringan tanpa perlu melakukan konfigurasi manual di sisi klien.

**Rangkuman Konfigurasi**

- Host Reservation di DHCP Server
Ditambahkan deklarasi host untuk setiap klien (Hawkeye, Thor, dan SpiderMan) di file dhcpd.conf.
Masing-masing berisi pasangan hardware ethernet <MAC> dan fixed-address <IP>, sehingga:
  - Hawkeye → 10.78.4.5
  - Thor → 10.78.5.5
  - SpiderMan → 10.78.7.5

- Konsistensi Alamat
Dengan metode ini, meskipun klien melakukan DHCP Discover, server akan selalu mengembalikan IP tetap yang telah dipesan sesuai MAC address.

- Verifikasi
Pengujian dilakukan dengan melakukan DHCP request (udhcpc) dari masing-masing klien. Hasilnya, ketiga node memperoleh IP .5 di subnetnya masing-masing sesuai harapan.

<br>

## Soal 9

> Buatlah konfigurasi DHCP Failover dengan WinterSoldier sebagai DHCP server backup untuk CaptainAmerica.

### Answer
**Tujuan**
Mengimplementasikan **DHCP Failover** dengan:
- **CaptainAmerica** sebagai DHCP Primary.
- **WinterSoldier** sebagai DHCP Secondary (backup).

Sehingga layanan DHCP tetap berjalan meskipun salah satu server mati.

**Setup ISC DHCP Server di CaptainAmerica (primer DHCP)**

Sama prosesnya seperti sebelumnya. 
```bash
nano /etc/dhcp/dhcpd.conf
```
**Salin dan tempel kode berikut **DI BAGIAN ATAS**:**
```bash
ddns-update-style none;
authoritative;

failover peer "dhcp-failover" {
  primary;
  address 10.78.3.10;
  port 647;
  peer address 10.78.3.20;
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  mclt 3600;
  split 128;
  load balance max seconds 3;
}
```

full code `primary`
```conf
ddns-update-style none;
authoritative;

failover peer "dhcp-failover" {
  primary;
  address 10.78.3.10;
  port 647;
  peer address 10.78.3.20;
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  mclt 3600;
  split 128;
  load balance max seconds 3;
}

# LAN3 (Switch1) - Falcon: 10.78.3.0/24
subnet 10.78.3.0 netmask 255.255.255.0 {
  option routers 10.78.3.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 120;      # 2 menit
  max-lease-time 6000;         # 100 menit

  pool {
    failover peer "dhcp-failover";
    range 10.78.3.20 10.78.3.25;
  }
}

# LAN4 (Switch2) - Hawkeye: 10.78.4.0/24
subnet 10.78.4.0 netmask 255.255.255.0 {
  option routers 10.78.4.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 300;      # 5 menit
  max-lease-time 7200;         # 2 jam

  pool {
    failover peer "dhcp-failover";
    range 10.78.4.30 10.78.4.35;
  }
}

# LAN5 (Switch3) - ScarletWitch & Thor: 10.78.5.0/24
subnet 10.78.5.0 netmask 255.255.255.0 {
  option routers 10.78.5.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 120;      # 2 menit
  max-lease-time 6000;         # 100 menit

  # Pool ScarletWitch
  class "scarletwitch" { match if option host-name = "scarletwitch"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "scarletwitch";
    range 10.78.5.40 10.78.5.45;
  }

  # Pool Thor
  class "thor" { match if option host-name = "thor"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "thor";
    range 10.78.5.100 10.78.5.105;
  }
}

# LAN6 (Switch4 -> Hulk): 10.78.6.0/24
subnet 10.78.6.0 netmask 255.255.255.0 {
  option routers 10.78.6.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  # lease time bebas (tak ditentukan soal), bisa pakai default server atau tambahkan sendiri.
  pool {
    failover peer "dhcp-failover";
    range 10.78.6.50 10.78.6.55;
  }
}

# LAN7 (Switch5) - SpiderMan & DoctorStrange: 10.78.7.0/24
subnet 10.78.7.0 netmask 255.255.255.0 {
  option routers 10.78.7.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 600;      # 10 menit
  max-lease-time 7200;         # 2 jam

  # Pool SpiderMan
  class "spiderman" { match if option host-name = "spiderman"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "spiderman";
    range 10.78.7.60 10.78.7.65;
  }

  # Pool DoctorStrange
  class "doctorstrange" { match if option host-name = "doctorstrange"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "doctorstrange";
    range 10.78.7.110 10.78.7.115;
  }
}

# Hawkeye (LAN4) -> 10.78.4.5
host hawkeye {
  hardware ethernet 02:42:28:e2:3d:00;  # GANTI sesuai MAC Hawkeye
  fixed-address 10.78.4.5;
}

# Thor (LAN5) -> 10.78.5.5
host thor {
  hardware ethernet 02:42:1e:9e:c9:00;  # GANTI sesuai MAC Thor
  fixed-address 10.78.5.5;
}

# SpiderMan (LAN7) -> 10.78.7.5
host spiderman {
  hardware ethernet 02:42:26:5f:90:00;  # GANTI sesuai MAC SpiderMan
  fixed-address 10.78.7.5;
}
```

Lalu tambahkan `fallover` (di bawah) di setiap subnet-nya
```bash
  pool {
    failover peer "dhcp-failover";
    range 10.78.3.20 10.78.3.25;
  }
```

Validasi & restart service, dan cek running:
```bash
dhcpd -t -cf /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
service isc-dhcp-server status
```

**Setup ISC DHCP Server di WinterSoldier (Sekundari DHCP)**
Sebelum set-up, pastikan konfigurasinode `WinterSoldier` seperti ini
```bash
auto eth0
iface eth0 inet static
    address 10.78.4.10
    netmask 255.255.255.0
    gateway 10.78.4.1
    dns-nameservers 8.8.8.8 1.1.1.1
    post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
    post-up echo "nameserver 1.1.1.1" >> /etc/resolv.conf
```

lalu, lakukan instalasi dengan melaui `Web console`:
```bash
apt update
apt install -y isc-dhcp-server
```
Set interface layanan DHCP
```bash
nano /etc/default/isc-dhcp-server
```
ubah baris menjadi:
```bash
INTERFACESv4="eth0"
```

Sama prosesnya seperti sebelumnya, kita mengedit konfig `DHCP`
```bash
rm /etc/dhcp/dhcpd.conf
nano /etc/dhcp/dhcpd.conf
```

**Salin dan tempel kode berikut **DI BAGIAN ATAS**:**
```bash
failover peer "dhcp-failover" {
  secondary;
  address 10.78.3.20;         # IP WinterSoldier
  port 647;
  peer address 10.78.3.10;    # IP CaptainAmerica
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  load balance max seconds 3;
}
```

full code `secondary`
```conf
failover peer "dhcp-failover" {
  secondary;
  address 10.78.3.20;
  port 647;
  peer address 10.78.3.10;
  peer port 647;
  max-response-delay 60;
  max-unacked-updates 10;
  load balance max seconds 3;
}

subnet 10.78.3.0 netmask 255.255.255.0 {
  option routers 10.78.3.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 120;
  max-lease-time 6000;

  pool {
    failover peer "dhcp-failover";
    range 10.78.3.20 10.78.3.25;
  }
}

subnet 10.78.4.0 netmask 255.255.255.0 {
  option routers 10.78.4.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 300;
  max-lease-time 7200;

  pool {
    failover peer "dhcp-failover";
    range 10.78.4.30 10.78.4.35;
  }
}

subnet 10.78.5.0 netmask 255.255.255.0 {
  option routers 10.78.5.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 120;
  max-lease-time 6000;

  class "scarletwitch" { match if option host-name = "scarletwitch"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "scarletwitch";
    range 10.78.5.40 10.78.5.45;
  }

  class "thor" { match if option host-name = "thor"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "thor";
    range 10.78.5.100 10.78.5.105;
  }
}

subnet 10.78.6.0 netmask 255.255.255.0 {
  option routers 10.78.6.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  pool {
    failover peer "dhcp-failover";
    range 10.78.6.50 10.78.6.55;
  }
}

subnet 10.78.7.0 netmask 255.255.255.0 {
  option routers 10.78.7.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 1.1.1.1;
  default-lease-time 600;
  max-lease-time 7200;

  class "spiderman" { match if option host-name = "spiderman"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "spiderman";
    range 10.78.7.60 10.78.7.65;
  }

  class "doctorstrange" { match if option host-name = "doctorstrange"; }
  pool {
    failover peer "dhcp-failover";
    allow members of "doctorstrange";
    range 10.78.7.110 10.78.7.115;
  }
}

# Hawkeye (LAN4) -> 10.78.4.5
host hawkeye {
  hardware ethernet 02:42:28:e2:3d:00;  # GANTI sesuai MAC Hawkeye
  fixed-address 10.78.4.5;
}

# Thor (LAN5) -> 10.78.5.5
host thor {
  hardware ethernet 02:42:1e:9e:c9:00;  # GANTI sesuai MAC Thor
  fixed-address 10.78.5.5;
}

# SpiderMan (LAN7) -> 10.78.7.5
host spiderman {
  hardware ethernet 02:42:26:5f:90:00;  # GANTI sesuai MAC SpiderMan
  fixed-address 10.78.7.5;
}
```

Lalu **samakan yang lainnya** dengan node `CaptainAmerica`.

Validasi & restart service, dan cek running:
```bash
dhcpd -t -cf /etc/dhcp/dhcpd.conf
service isc-dhcp-server restart
service isc-dhcp-server status
```

**Stop & Start service, dan cek running pada kedua node:**
Sebelum start service, kita stop dulu service yang ada pada kedua node.
```bash
service isc-dhcp-server stop
rm -f /var/run/dhcpd.pid
```
Pastikan urutannya node `CaptainAmerica` -> `WinterSoldier`
```bash
service isc-dhcp-server restart
service isc-dhcp-server status
```

**Verifikasi**
Stop service pada node `CaptainAmerica`
```bash
service isc-dhcp-service stop
```
Pada node client sembarang, jalankan:
```bash
udhcpc -i eth0 -n -q
```
Maka, apabila normal, `WinterSoldier` akan melakukan takeover.

#### Screenshot
<img width="717" height="469" alt="image" src="https://github.com/user-attachments/assets/3d7d70f0-e196-4cc7-83fc-a8e12962307b" />
<img width="823" height="410" alt="image" src="https://github.com/user-attachments/assets/591e48bc-7205-4398-ae23-72c712a5ff6b" />
<img width="784" height="254" alt="image" src="https://github.com/user-attachments/assets/dd634574-2c88-4584-be2c-a9418e92848e" />



#### Explanation

  `Put your explanation in here`

<br>

## Soal 10

> Buatlah konfigurasi agar CaptainAmerica dan WinterSoldier berjalan dengan mode Load Balancing.

### Answer
**Tujuan**
Mengaktifkan **Load Balancing** DHCP, di mana CaptainAmerica dan WinterSoldier berbagi beban 50:50 saat melayani klien.

**Konsep**
- Parameter `split 128` = 50:50 (primary:secondary).
- `load balance max seconds 3` = jika partner telat, server lain ikut merespons.

**Konfigurasi**
Konfigurasi di **CaptainAmerica** dan **WinterSoldier** identik dengan nomor 9, dengan parameter failover seperti di atas (`split 128` dan `load balance max seconds 3`).

**Verifikasi**
1. **Normal State**  
   Kedua server harus menampilkan:
   ```
   Failover association dhcp-failover now in normal state
   ```

2. **Tes dari banyak klien**  
   Jalankan DHCP renew di beberapa node (Falcon, Hawkeye, Thor, SpiderMan, dll).  
   Sebagian permintaan dijawab CaptainAmerica, sebagian WinterSoldier.

3. **Pantau OFFER** dengan `tcpdump`:
   ```bash
   tcpdump -ni eth0 port 67 or port 68 -vv
   ```
   Harus terlihat OFFER berasal dari dua server berbeda.

4. **Lease database sinkron**  
   Cek `/var/lib/dhcp/dhcpd.leases` → berisi klien dari kedua server.

---

#### Screenshot

  `Put your screenshot in here`

#### Explanation

  `Put your explanation in here`

<br>
  
## Problems

## Revisions (if any)
