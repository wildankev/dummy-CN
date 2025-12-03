[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/e_s827HM)
| Name           | NRP        | Kelas     |
| ---            | ---        | ----------|
| A. Wildan Kevin Assyauqi | 5025241265 | Jaringan Komouter (A) |



## Put your topology config image here!
<img width="1076" height="665" alt="image" src="https://github.com/user-attachments/assets/50480b31-2e5a-4e33-8137-b5f45d2fdd73" />

## Put your GNS3 Project file here!
[File](https://drive.google.com/drive/u/0/folders/1fjqolsO_7Y3s-iUL_mX0W2mYJxDVeYdD)

<br>

## Soal 1

> Dokumentasikan hasil pengelompokan subnet yang telah dibuat.

**Answer:**
Prefix IP: **10.78**
Netmask: **/16 (255.255.0.0)** digunakan untuk seluruh jaringan karena tidak menggunakan NAT.

**A. Subnet LAN**
| Switch | Subnet | Gateway (Router) | Host Anggota |
|--------|---------|------------------|---------------|
| Switch1 | 10.78.2.0/16 | Aline1:eth0 = 10.78.2.1 | Lune (10.78.2.11), Sciel (10.78.2.12), Gustave (10.78.2.13) |
| Switch2 | 10.78.3.0/16 | Aline1:eth1 = 10.78.3.1 | Renoir (10.78.3.10), Verso (10.78.3.11) |
| Switch3 | 10.78.4.0/16 | Aline1:eth2 = 10.78.4.1 | Alicia (10.78.4.10) |
| Switch4 | 10.78.5.0/16 | Aline1:eth3 = 10.78.5.1 | Esquie (10.78.5.21), Monocco (10.78.5.22), Maelle (10.78.5.23) |

**B. Subnet Antar-Switch/Router**
| Link | Subnet | IP Sisi A | IP Sisi B |
|------|---------|------------|------------|
| Aline1 ↔ Switch1 | 10.78.2.0/16 | Aline1:eth0 = 10.78.2.1 | Switch1:e0 |
| Aline1 ↔ Switch2 | 10.78.3.0/16 | Aline1:eth1 = 10.78.3.1 | Switch2:e0 |
| Aline1 ↔ Switch3 | 10.78.4.0/16 | Aline1:eth2 = 10.78.4.1 | Switch3:e0 |
| Aline1 ↔ Switch4 | 10.78.5.0/16 | Aline1:eth3 = 10.78.5.1 | Switch4:e0 |

**C. Fungsi Node**
| Node | Fungsi | Interface | IP |
|------|----------|------------|----|
| Renoir | DNS Master | eth0 | 10.78.3.10 |
| Verso | DNS Slave | eth0 | 10.78.3.11 |
| Lune | Web Server 1 | eth0 | 10.78.2.11 |
| Sciel | Web Server 2 | eth0 | 10.78.2.12 |
| Gustave | Web Server 3 | eth0 | 10.78.2.13 |
| Alicia | Reverse Proxy | eth0 | 10.78.4.10 |
| Esquie | Client | eth0 | 10.78.5.21 |
| Monocco | Client | eth0 | 10.78.5.22 |
| Maelle | Client | eth0 | 10.78.5.23 |

---
- Screenshot
<img width="1076" height="665" alt="image" src="https://github.com/user-attachments/assets/0a898da8-ab49-459d-8c1e-c8a3007b355a" />

- Explanation
  
Pengelompokan subnet dilakukan berdasarkan topologi [soal](https://docs.google.com/document/d/1WjMjAOECPLE9Hbooz5Ye60y1J33D3ATjaVehF8kb-ks), dengan mempertimbangkan setiap *switch* dan *router (Aline1)* sebagai pemisah jaringan.  
Semua subnet menggunakan netmask **/16 (255.255.0.0)** untuk menyederhanakan komunikasi tanpa NAT, sesuai ketentuan tambahan praktikum.  
Distribusi IP memastikan tiap node memiliki alamat unik dan terhubung sesuai peran berikut:
  - **Renoir** dan **Verso** berfungsi sebagai DNS Master dan Slave.
  - **Lune**, **Sciel**, dan **Gustave** berfungsi sebagai web server dengan Nginx.
  - **Alicia** bertindak sebagai reverse proxy dan load balancer.
  - **Esquie**, **Monocco**, dan **Maelle** berperan sebagai klien pengujian.

<br>

## Soal 2

> Buatlah konfigurasi untuk domain 
> **lune33.com** → ke IP node Lune , 
> **sciel33.com** → ke IP node Sciel ,
> **gustave33.com** → ke IP node Gustave 
> pada DNS Master Renoir. Kemudian konfigurasikan node Verso sebagai DNS Slave yang bekerja untuk DNS Master Renoir.

**Answer:**

**`renoir`**

```
cd root
nano named.conf
```
```conf
options {
    directory "/root";
    listen-on { any; };
    allow-query { any; };
    allow-transfer { 10.78.3.11; };//verso

    recursion no;
    dnssec-validation no;
    notify yes;
};

zone "lune33.com" {
    type master;
    file "db.lune33.com";
    allow-transfer { 10.78.3.11; };
    also-notify { 10.78.3.11; };
};

zone "sciel33.com" {
    type master;
    file "db.sciel33.com";
    allow-transfer { 10.78.3.11; };
    also-notify { 10.78.3.11; };
};

zone "gustave33.com" {
    type master;
    file "db.gustave33.com";
    allow-transfer { 10.78.3.11; };
    also-notify { 10.78.3.11; };
};
```

```bash
nano db.lune33.com
```
```conf
$TTL 86400
@   IN  SOA ns1.lune33.com. admin.lune33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.lune33.com.
ns1 IN  A   10.78.3.10
www IN  A   10.78.2.11
```

```
nano db.sciel33.com
```
```
$TTL 86400
@   IN  SOA ns1.sciel33.com. admin.sciel33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.sciel33.com.
ns1 IN  A   10.78.3.10
www IN  A   10.78.2.12
```

```
nano db.gustave33.com
```
```
$TTL 86400
@   IN  SOA ns1.gustave33.com. admin.gustave33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.gustave33.com.
ns1 IN  A   10.78.3.10
www IN  A   10.78.2.13
```

Jalankan semuanya.
```
named -g -c named.conf
```

**`Verso`**
```
cd root
nano named.conf
```
```
options {
    directory "/root";
    listen-on { any; };
    allow-query { any; };
    recursion no;
    dnssec-validation no;
};

zone "lune33.com" {
    type slave;
    masters { 10.78.3.10; };
    file "slv.lune33.com";
};

zone "sciel33.com" {
    type slave;
    masters { 10.78.3.10; };
    file "slv.sciel33.com";
};

zone "gustave33.com" {
    type slave;
    masters { 10.78.3.10; };
    file "slv.gustave33.com";
};
```
Jalankan.
```
named -g -c named.conf
```

cek di `verso`
1. Stop named
```
ls
```

**`testing di client`**
```
busybox nslookup www.sciel33.com 10.78.3.10
busybox nslookup www.gustave33.com 10.78.3.10
```

- Screenshot
<img width="908" height="463" alt="image" src="https://github.com/user-attachments/assets/aab3469d-3ca6-4cd4-8961-639589281c25" />
<img width="908" height="509" alt="image" src="https://github.com/user-attachments/assets/432d4afa-111b-44ca-9ba6-60cf66f493c0" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 3

> Tambahkan subdomain alias berupa exp.lune33.com yang mengarah ke alamat lune33.com dan exp.sciel33.com yang mengarah ke alamat sciel33.com (HINT: CNAME). Selain itu, tambahkan konfigurasi untuk melakukan reverse DNS lookup untuk domain gustave33.com

**Answer:**

**`Renoir`**
```
nano named.conf
```
```
zone "2.78.10.in-addr.arpa" {
    type master;
    file "db.2.78.10.in-addr.arpa";
    allow-transfer { 10.78.3.11; };
    also-notify { 10.78.3.11; };
};
```

```
nano db.lune33.com
```
```
$TTL 86400
@   IN  SOA ns1.lune33.com. admin.lune33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.lune33.com.
ns1 IN  A   10.78.3.10
www IN  A   10.78.2.11
exp IN  CNAME www.lune33.com.
```

```
nano db.sciel33.com
```
```
$TTL 86400
@   IN  SOA ns1.sciel33.com. admin.sciel33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.sciel33.com.
ns1 IN  A   10.78.3.10
www IN  A   10.78.2.12
exp IN  CNAME www.sciel33.com.
```

```
nano db.2.78.10.in-addr.arpa
```
```
$TTL 86400
@   IN  SOA ns1.gustave33.com. admin.gustave33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.gustave33.com.

13  IN  PTR gustave33.com.
```

```
pkill named 2>/dev/null
named -g -c named.conf
```

**`Verso`**
```
nano named.conf
```
```
zone "2.78.10.in-addr.arpa" {
    type slave;
    masters { 10.78.3.10; };
    file "slv.2.78.10.in-addr.arpa";
};
```

```
pkill named 2>/dev/null
named -g -c named.conf
```

**`testing di client`**
```
busybox nslookup exp.lune33.com 10.78.3.10
busybox nslookup exp.sciel33.com 10.78.3.10
busybox nslookup www.gustave33.com 10.78.3.10
```
- Screenshot
<img width="909" height="606" alt="image" src="https://github.com/user-attachments/assets/beeb2d1e-a12a-4fee-9470-e5f256dd9ad1" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 4

> Buatlah subdomain berupa expedition.gustave33.com dan delegasikan subdomain tersebut dari Renoir ke Verso dengan alamat IP tujuan adalah node Gustave. Kemudian, matikan Renoir dan coba lakukan ping ke semua domain dan subdomain yang telah dikonfigurasikan pada nomor 2, 3, dan 4.

**Answer:**

**`renoir`**
```
nano db.gustave33.com
```
```
expedition      IN  NS  ns1.expedition.gustave33.com.
ns1.expedition  IN  A   10.78.3.11
```

```
pkill named 2>/dev/null
named -g -c named.conf
```

**`verso`**
```
nano named.conf
```
```
zone "expedition.gustave33.com" {
    type master;
    file "db.expedition.gustave33.com";
};
```

```
nano db.expedition.gustave33.com
```
```
$TTL 86400
@   IN  SOA ns1.expedition.gustave33.com. admin.expedition.gustave33.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.expedition.gustave33.com.
ns1 IN  A   10.78.3.11
www IN  A   10.78.2.13
```

```
pkill named 2>/dev/null
named -g -c named.conf
```

**testing**

tambahken ke node client
```
nano /etc/resolv.conf
```
```
nameserver 10.78.3.10
nameserver 10.78.3.11
```

```
ping www.lune33.com
ping www.sciel33.com
ping exp.lune33.com
ping exp.sciel33.com
ping www.gustave33.com
ping www.expedition.gustave33.com
```
- Screenshot
<img width="911" height="607" alt="image" src="https://github.com/user-attachments/assets/16b718ad-6159-48a7-bebf-cffeba31a22a" />
<img width="920" height="391" alt="image" src="https://github.com/user-attachments/assets/6ee81465-53a0-4235-8b3e-d66f5ab57edf" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 5

> Konfigurasi node Lune, Sciel, dan Gustave agar berfungsi sebagai web server Nginx yang akan menyajikan halaman profil, dimana halaman profil akan berbeda untuk setiap node. Dari folder berikut, gunakan profile_lune.html untuk menyajikan halaman profil di node Lune, profile_sciel.html untuk menyajikan halaman profil di node Sciel, dan profile_gustave.html untuk menyajikan halaman profil di node Gustave. Konfigurasikan Nginx di setiap node untuk menyimpan custom access log ke file /tmp/access.log dan error log ke file /tmp/error.log. 

**Answer:**
**Setiap node**
```
mkdir -p /myscripts/myconfig
mkdir -p /myscripts/myweb
mkdir -p /myscripts/mylogs
nano /myscripts/myweb/index.html
```
lalu masukkan sesuai profile masing-masing node. Dapat diakses [di sini](https://drive.google.com/drive/folders/1A6_IelWIX90PY33oPIM3jHuS9QmBnXft)

Konfigurasikan `NGINX` untuk setiap node
```
nano /myscripts/myconfig/nginx.conf
```
`Lune`
```
user www-data;
worker_processes auto;
worker_cpu_affinity auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    access_log /tmp/access.log;

    server {
        listen 80;
        server_name lune33.com www.lune33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Sciel`
```
user www-data;
worker_processes auto;
worker_cpu_affinity auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    access_log /tmp/access.log;

    server {
        listen 80;
        server_name sciel33.com www.sciel33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Gustave`
```
user www-data;
worker_processes auto;
worker_cpu_affinity auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    access_log /tmp/access.log;

    server {
        listen 80;
        server_name gustave33.com www.gustave33.com _;

        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

Konfigurasikan `start script` untuk semua node (semua sama).
```
nano /myscripts/myconfig/start_http.sh
```
```
#!/bin/bash
nginx -c /myscripts/myconfig/nginx.conf -g 'daemon off;'
```

Berikan izin untuk eksekusi pada semua node
```
chmod +x /myscripts/myconfig/start_http.sh
```
Eksekusi
```
cd /myscripts/myconfig
bash start_http.sh
```

`client`
```
curl http://10.78.2.11
curl http://10.78.2.12
curl http://10.78.2.13
```

```
cat /tmp/access.log
```

- Screenshot
<img width="908" height="608" alt="image" src="https://github.com/user-attachments/assets/edd0e26b-e79d-4716-a312-8d3baae1c541" />
<img width="911" height="605" alt="image" src="https://github.com/user-attachments/assets/3c0aebb7-78ea-4fd7-b332-180a91a01f83" />
<img width="910" height="609" alt="image" src="https://github.com/user-attachments/assets/073fb1bb-7238-4d0e-a234-576ca89c2c27" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 6

> Setelah website berhasil dideploy pada masing-masing node web server dan halaman dapat menampilkan profil yang sesuai,  buatlah custom access log ke file /tmp/access.log di masing-masing node web server menggunakan format log tertentu seperti di bawah:
> - Tanggal dan waktu akses dalam format standar log.
> - Nama node yang sedang diakses.
> - Alamat IP klien yang mengakses website.
> - Metode HTTP dan URI yang diakses oleh klien.
> - Status respons HTTP yang diberikan oleh server.
> - Jumlah byte yang dikirimkan dalam respons.
> - Waktu yang dihabiskan oleh server untuk menangani permintaan.
> - Contoh format log yang sesuai:
>   [01/Oct/2024:11:30:45 +0000] Jarkom Node Lune Access from 192.168.1.15 using method "GET /resep/bayam HTTP/1.1" returned status 200 with 2567 bytes sent in 0.038 seconds

**Answer:**
Konfigurasikan untuk masing-masing node web server dengan cara di bawah ini:
```
rm /myscripts/myconfig/nginx.conf
nano /myscripts/myconfig/nginx.conf
```

`Lune`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Lune Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 80;
        server_name lune33.com www.lune33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Sciel`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Sciel Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 80;
        server_name sciel33.com www.sciel33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Gustave`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Gustave Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 80;
        server_name gustave33.com www.gustave33.com _;

        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

Kill dan jalankan ulang di masing-masing node.
```
pkill nginx 2>/dev/null
bash /myscripts/myconfig/start_http.sh
```

Testing di `client` untuk memancing log
```
curl http://10.78.2.11
curl http://10.78.2.12
curl http://10.78.2.13
```

Cek log hasil
```
cat /tmp/access.log
```

- Screenshot
<img width="905" height="185" alt="image" src="https://github.com/user-attachments/assets/29d2171d-908e-4df5-9891-8ef6810e91f1" />
<img width="913" height="193" alt="image" src="https://github.com/user-attachments/assets/2859e398-4850-44b5-914b-d93d5a165217" />
<img width="921" height="191" alt="image" src="https://github.com/user-attachments/assets/4f3ab457-5e4a-4d18-b78f-fcf5d3c26411" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 7

> Gustave merupakan web server yang tidak disarankan untuk dilihat oleh publik. Maka dari itu, ubahlah konfigurasi nginx sehingga halaman profil Gustave menjadi hanya bisa di akses melalui port 8080 dan 8888.

**Answer:**

Edit konfigurasi di `Gustave`
```
rm /myscripts/myconfig/nginx.conf
nano /myscripts/myconfig/nginx.conf
```

```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Gustave Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 8080;
        server_name gustave33.com www.gustave33.com _;

        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 8888;
        server_name gustave33.com www.gustave33.com _;

        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 80;
        return 403;
    }
}
```

Kill dan jalankan ulang.
```
pkill nginx 2>/dev/null
bash /myscripts/myconfig/start_http.sh
```

Testing di node `client`
```
curl http://10.78.2.13:8080
curl http://10.78.2.13:8888
curl http://10.78.2.13
```

- Screenshot
<img width="909" height="611" alt="image" src="https://github.com/user-attachments/assets/b95ff804-b0b6-4749-889f-b04109f27585" />
<img width="901" height="609" alt="image" src="https://github.com/user-attachments/assets/8e5d6150-c1c4-4890-940b-16d1d3f2a333" />
<img width="913" height="289" alt="image" src="https://github.com/user-attachments/assets/65d9ad62-7504-48ce-a9ce-fc71ef3ae38c" />

- Explanation

  `Put your explanation in here`

<br>

## Soal 8

> Untuk mempermudah program ekspedisi, maka node Lune, Sciel, Gustave sepakat untuk membuat halaman informasi dengan konten yang sama. Maka dari itu, buatlah lagi 1 server block di dalam konfigurasi nginx yang akan menyajikan file HTML ini. Namun, mereka ingin menyajikan halaman informasi tersebut di port yang berbeda-beda, yaitu Lune menggunakan port 8000, Sciel menggunakan port 8100, dan Gustave menggunakan port 8200.

**Answer:**

Buat file `info.html` untuk masing-masing node client. Isi file dapat diakses [di sini](https://drive.google.com/file/d/1BLg3S22ldhL-wRYN-ivowEqh8cYwwfVS/view)
```
mkdir -p /myscripts/info
nano /myscripts/info/info.html
```

Edit komfigurasi di masing-masing node client
```
rm /myscripts/myconfig/nginx.conf
nano /myscripts/myconfig/nginx.conf
```

`Lune`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Lune Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 80;
        server_name lune33.com www.lune33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 8000;
        server_name lune33.com www.lune33.com _;
        root /myscripts/info;
        index info.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Sciel`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Sciel Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 80;
        server_name sciel33.com www.sciel33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 8100;
        server_name _;
        root /myscripts/info;
        index info.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

`Gustave`
```
user www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events {
    worker_connections 768;
}

http {
    log_format custom_log '[$time_local] Jarkom Node Gustave Access from $remote_addr '
                         'using method "$request" returned status $status '
                         'with $body_bytes_sent bytes sent in $request_time seconds';

    access_log /tmp/access.log custom_log;

    server {
        listen 8080;
        server_name gustave33.com www.gustave33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 8888;
        server_name gustave33.com www.gustave33.com _;
        root /myscripts/myweb;
        index index.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }

    server {
        listen 80;
        return 403;
    }

    server {
        listen 8200;
        server_name gustave33.com www.gustave33.com _;
        root /myscripts/info;
        index info.html;

        location / {
            try_files $uri $uri/ =404;
        }
    }
}
```

kill dan jalankan ulang.
```
pkill nginx 2>/dev/null
bash /myscripts/myconfig/start_http.sh
```

Testing di node `client`
```
curl http://10.78.2.11:8000
curl http://10.78.2.11
curl http://10.78.2.12:8100
curl http://10.78.2.12
curl http://10.78.2.13:8200
curl http://10.78.2.13:8080
curl http://10.78.2.13:8888
curl http://10.78.2.13
```

- Screenshot
<img width="905" height="365" alt="image" src="https://github.com/user-attachments/assets/110397f3-b58c-432d-8d57-cc3e5002398e" />
<img width="908" height="600" alt="image" src="https://github.com/user-attachments/assets/d332b987-8ba4-4f77-bad8-6c41927545e0" />


- Explanation

  `Put your explanation in here`

<br>

## Soal 9

> Untuk mempermudah akses ke profil tiap anggota ekspedisi, buatlah 1 domain lagi yaitu "expeditioners.com" yang akan mengarah ke Alicia. Lalu, untuk mencegah overload dari salah satu web server, konfigurasikan reverse proxy Alicia agar bisa forward request ke server yang sesuai berdasarkan URL profile yang diminta oleh klien dengan ketentuan sebagai berikut:
> -  Request untuk “expeditioners.com/profil_lune” harus dialihkan ke halaman profil web server Lune.
> -  Request untuk “expeditioners.com/profil_sciel” harus dialihkan ke halaman profil web server Sciel.
> -  Request untuk “expeditioners.com/profil_gustave” harus dialihkan ke halaman profil web server Gustave.
> Jika terdapat request ke URL selain profil yang ditentukan, reverse proxy akan mengalihkan ke halaman informasi pada web server Lune.


**Answer:**

Edit konfigurasi di node `Renoir`
```
nano named.conf
```
```
zone "expeditioners.com" {
    type master;
    file "/root/db.expeditioners.com";
    zone "expeditioners.com" {
    allow-transfer { 10.78.3.11; };
    also-notify { 10.78.3.11; };
};
```

```
nano db.expeditioners.com
```
```
$TTL 86400
@   IN  SOA ns1.expeditioners.com. admin.expeditioners.com. (
        2 ; serial
        3600 ; refresh
        1800 ; retry
        604800 ; expire
        86400 ) ; minimum
    IN  NS  ns1.expeditioners.com.
ns1 IN  A   10.78.3.10

@   IN  A   10.78.4.10
www IN  A   10.78.4.10
```
Kill dan jalankan ulang
```
pkill named 2>/dev/null
named -g -c named.conf
```

Edit konfigurasi di node `Verso`
```
nano named.conf
```
```
zone "expeditioners.com" IN {
    type slave;
    masters { 10.78.3.10; };
    file "slv.expeditioners.com";
};
```
Kill dan jalankan ulang
```
pkill named 2>/dev/null
named -g -c named.conf
```

Edit node `Alicia`
```
mkdir -p /myscripts/myconfig
nano /myscripts/myconfig/nginx.conf
```
```
user  www-data;
worker_processes auto;
pid /tmp/nginx.pid;

events { worker_connections 768; }

http {
    access_log /tmp/access.log;
    error_log  /tmp/error.log;

    upstream lune_profile    { server 10.78.2.11:80;   }
    upstream sciel_profile   { server 10.78.2.12:80;   }
    upstream gustave_profile { server 10.78.2.13:8080; }
    upstream lune_info { server 10.78.2.11:8000; }

    map $request_uri $is_profile {
        default          0;
        =/profil_lune    1;
        =/profil_sciel   1;
        =/profil_gustave 1;
    }

    server {
        listen 80;
        server_name expeditioners.com www.expeditioners.com _;

        proxy_set_header Host               $host;
        proxy_set_header X-Real-IP          $remote_addr;
        proxy_set_header X-Forwarded-For    $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto  $scheme;

        location = /profil_lune    { proxy_pass http://lune_profile/; }
        location = /profil_sciel   { proxy_pass http://sciel_profile/; }
        location = /profil_gustave { proxy_pass http://gustave_profile/; }

        location / { proxy_pass http://lune_info/; }
    }
}
```

Buat `start script`
```
nano /myscripts/myconfig/start_http.sh
```
```
#!/bin/bash
nginx -c /myscripts/myconfig/nginx.conf -g 'daemon off;'
```
Buat izin eksekusi
```
chmod +x /myscripts/myconfig/start_http.sh
```
eksekusi
```
cd /myscripts/myconfig
bash start_http.sh
```
Testing di node `client`
```
curl http://expeditioners.com/profil_lune
curl http://expeditioners.com/profil_sciel
curl http://expeditioners.com/profil_gustave
```
- Screenshot
<img width="913" height="616" alt="image" src="https://github.com/user-attachments/assets/a5d11933-1ca5-4016-afbd-a31deb35b08f" />
<img width="913" height="612" alt="image" src="https://github.com/user-attachments/assets/e8ecc437-b3df-4b14-9fdb-257c881426bc" />


- Explanation

  `Put your explanation in here`

<br>

## Soal 10

> Untuk mendistribusikan traffic halaman informasi, atur Reverse Proxy Alicia agar dapat membagi pekerjaan kepada web server Lune, Sciel, dan Gustave secara optimal menggunakan algoritma Round-robin. Pastikan target pembagian load merupakan halaman informasi, bukan halaman profil masing-masing web server.

**Answer:**

Ubah konfigurasi di node `Alicia`
```
rm /myscripts/myconfig/nginx.conf
nano /myscripts/myconfig/nginx.conf
```
```
user  www-data;
worker_processes auto;
pid /tmp/nginx.pid;
error_log /tmp/error.log;

events { worker_connections 768; }

http {
  access_log /tmp/access.log;

  upstream lune_profile    { server 10.78.2.11:80; }
  upstream sciel_profile   { server 10.78.2.12:80; }
  upstream gustave_profile { server 10.78.2.13:8080; }

  upstream info_cluster {
    server 10.78.2.11:8000;
    server 10.78.2.12:8100;
    server 10.78.2.13:8200;
  }

  server {
    listen 80;
    server_name expeditioners.com www.expeditioners.com;

    location = /profil_lune    { proxy_pass http://lune_profile; }
    location = /profil_sciel   { proxy_pass http://sciel_profile; }
    location = /profil_gustave { proxy_pass http://gustave_profile; }

    location = /info {
      proxy_pass http://info_cluster;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
      proxy_pass http://info_cluster;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
}
```
Kill dan jalankan ulang di masing-masing node.
```
pkill nginx 2>/dev/null
bash /myscripts/myconfig/start_http.sh
```

- Screenshot
<img width="908" height="612" alt="image" src="https://github.com/user-attachments/assets/d2892e48-d56f-4ba1-a8f2-f05971c28e11" />

- Explanation

  `Put your explanation in here`

<br>
  
## Problems
Sangat hectic minggu ini ;)

## Revisions (if any)

