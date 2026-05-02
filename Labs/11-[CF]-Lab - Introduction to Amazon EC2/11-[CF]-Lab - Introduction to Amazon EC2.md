<div align="center">

# ☁️ Hands-on Lab — Introduction to Amazon EC2

**AWS re/Start Program · Hands-on Lab**

![EC2](https://img.shields.io/badge/Amazon%20EC2-Launch%20%26%20Manage-FF9900?style=flat-square&logo=amazon-ec2&logoColor=white)
![Linux](https://img.shields.io/badge/Amazon%20Linux%202023-FCC624?style=flat-square&logo=linux&logoColor=black)
![Status](https://img.shields.io/badge/Status-Completed-4ADE80?style=flat-square)

</div>

---

> Amazon EC2 menyediakan kapasitas komputasi virtual yang fleksibel dengan model **pay-as-you-go**. Dalam lab ini kamu akan meluncurkan, mengonfigurasi, memantau, melakukan scaling, dan menghapus sebuah web server di AWS.

---

## 🎯 Objektif Lab

![images](imgs/14.png)

|  |  |
|:-:|----------|
| `1` | Luncurkan instans EC2 dengan **Termination Protection** & **User Data** script |
| `2` | Pantau metrik instans via **CloudWatch** |
| `3` | Buka port HTTP di **Security Group** |
| `4` | **Scaling Up** — ubah instance type & perbesar EBS volume |
| `5` | Uji & nonaktifkan **Termination Protection** |

---

## 🚀 Tugas 1  Meluncurkan Instans EC2

> Launch web server dengan Apache yang terinstal otomatis via User Data script.

**Langkah 1  Akses dashboard**

Di AWS Console cari dan pilih **EC2**, lalu klik **Launch Instance**.

**Langkah 2  Konfigurasi dasar**

| Pengaturan | Nilai |
|---|---|
| Name | `Web Server` |
| AMI | `Amazon Linux 2023` *(default)* |
| Instance Type | `t3.micro` |
| Key Pair | `Proceed without a key pair` |
| Storage | `8 GiB` *(default)* |

**Langkah 3  Network settings**

Klik **Edit**, lalu isi:

- **VPC** → `Lab VPC`
- **Security Group Name** → `Web Server security group`
- **Description** → `Security group for my web server`
- Hapus aturan **SSH** default

**Langkah 4  Advanced details · User Data**

Aktifkan **Termination Protection** → `Enable`, lalu tempel script berikut:

```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

**Langkah 5  Launch & verifikasi**

Klik **Launch instance** → **View all instances**.

> ✅ **Sukses** — Instance State: `Running` dan Status Checks: `2/2 checks passed`

![images](imgs/1.png)

---

## 📊 Tugas 2 Memantau Instans

> Pelajari cara memantau kesehatan dan metrik dasar instans EC2.

**Langkah 1  Status check**

Klik instans **Web Server** → tab **Status checks** untuk melihat metrik kesehatan hardware & software.

**Langkah 2  Monitoring CloudWatch**

Tab **Monitoring** → lihat grafik metrik:

- CPU Utilization
- Network In / Out
- Disk Read / Write

![images](imgs/3.png)

**Langkah 3  Instance screenshot**

```
Actions → Monitor and troubleshoot → Get instance screenshot
```
![images](imgs/2.png)

> 💡 **Info** — Screenshot instans berguna untuk troubleshooting tampilan konsol tanpa perlu koneksi SSH langsung.

---

## 🔓 Tugas 3  Update Security Group

> Buka port 80 (HTTP) agar web server dapat diakses publik.

**Langkah 1  Uji akses awal**

Salin **Public IPv4 address** dari tab Details, buka di tab browser baru.

> ⚠️ **Peringatan** — Halaman akan timeout. Normal! Port 80 masih tertutup di Security Group.

**Langkah 2  Edit inbound rules**

```
Panel kiri EC2 → Security Groups → Web Server security group
→ Tab Inbound rules → Edit inbound rules
```
![images](imgs/4.png)

**Langkah 3  Tambah aturan HTTP**

| Field | Nilai |
|---|---|
| Type | `HTTP` |
| Source | `Anywhere-IPv4 (0.0.0.0/0)` |

Klik **Save rules**.

![images](imgs/5.png)

**Langkah 4  Verifikasi**

Refresh tab browser sebelumnya.

> ✅ **Sukses** — Muncul teks: **"Hello From Your Web Server!"**

---

## ⚙️ Tugas 4  Scaling Up

> Simulasi peningkatan performa komputasi dan kapasitas penyimpanan.

![images](imgs/6.png)

### A  Hentikan instans

```
Instances → centang Web Server → Instance state → Stop instance
```
Tunggu status berubah menjadi `Stopped`.

![images](imgs/7.png)

### B  Ubah tipe instans

```
Actions → Instance settings → Change instance type
```

| | Sebelum | Sesudah |
|---|:-:|:-:|
| Instance Type | `t3.micro` | `t3.small` |

Klik **Apply**.

![images](imgs/8.png)

### C  Perbesar volume EBS

```
Elastic Block Store → Volumes → centang volume
→ Actions → Modify volume
```

| | Sebelum | Sesudah |
|---|:-:|:-:|
| Storage | `8 GiB` | `10 GiB` |

Klik **Modify** → konfirmasi **Modify**.

![images](imgs/9.png)

### D  Nyalakan kembali

```
Instances → centang Web Server → Instance state → Start instance
```

> 💡 **Info** — Scaling selesai: `t3.micro → t3.small` · storage `8 GiB → 10 GiB`

![images](imgs/10.png)

---

## 🛡️ Tugas 5  Uji Termination Protection

> Verifikasi lapisan keamanan dari penghapusan instans secara tidak sengaja.

**Langkah 1  Uji hapus (akan gagal)**

```
Instance state → Terminate instance → Terminate
```
![images](imgs/11.png)

> ❌ **Error** — Penghapusan diblokir oleh Termination Protection yang masih aktif.

**Langkah 2  Nonaktifkan proteksi**

```
Actions → Instance settings → Change termination protection
→ Hapus centang Enable → Save
```
![images](imgs/12.png)

**Langkah 3  Hapus permanen**

```
Instance state → Terminate instance → Terminate
```
![images](imgs/13.png)

> ✅ **Sukses** — Instans berhasil di-terminate setelah proteksi dinonaktifkan.

---
![images](imgs/15.png)
---

## 📋 Ringkasan Konfigurasi

| Komponen | Sebelum | Sesudah |
|---|:-:|:-:|
| Instance Type | `t3.micro` | `t3.small` |
| Storage (EBS) | `8 GiB` | `10 GiB` |
| Port HTTP | Tertutup | `0.0.0.0/0` |
| Termination Protection | `Enabled` | `Disabled` |

---

<div align="center">

☁️ **AWS re/Start Program** &nbsp;·&nbsp; Hands-on Lab: Introduction to Amazon EC2 &nbsp;·&nbsp; ✅ Completed

</div>