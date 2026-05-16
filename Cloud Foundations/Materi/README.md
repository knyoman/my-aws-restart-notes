# Cloud Foundations

> **Modul:** Cloud Foundations — AWS Academy / AWS Educate
> **Topik:** Konsep Dasar Cloud Computing & Infrastruktur Global AWS
> **Tanggal:** Mei 2024
> **Status:** ✅ Selesai Dipelajari

---

## 📋 Ringkasan Eksekutif

Dokumentasi ini merangkum pemahaman komprehensif mengenai konsep dasar **Cloud Computing** dan **infrastruktur global AWS** yang telah saya pelajari. Materi mencakup definisi, model layanan, keuntungan bisnis, serta arsitektur infrastruktur global yang mendasari platform AWS.

---

## 📌 Materi yang Dipelajari

### 1. Pengenalan Cloud Computing

**Definisi Resmi:**
> Cloud Computing adalah pengiriman sumber daya IT *on-demand* melalui internet dengan model harga **pay-as-you-go** — hanya bayar apa yang digunakan, kapan digunakan.

#### 💡 Transformasi Model Biaya

| Sebelum (On-Premise) | Sesudah (Cloud) |
|---|---|
| Biaya Modal / **CapEx** | Biaya Operasional / **OpEx** |
| Investasi awal besar | Bayar sesuai pemakaian |
| Kapasitas tetap (over/under provisioning) | Kapasitas elastis sesuai kebutuhan |
| Butuh waktu lama untuk setup | Deploy dalam hitungan menit |

#### ✅ Keuntungan Utama Cloud Computing

| Keuntungan | Penjelasan |
|---|---|
| **Agilitas** | Mendeploy sumber daya baru dengan sangat cepat, mempercepat eksperimen dan inovasi |
| **Skalabilitas** | Menambah atau mengurangi kapasitas secara manual sesuai kebutuhan bisnis |
| **Elastisitas** | Penyesuaian kapasitas secara **otomatis** dan real-time berdasarkan beban trafik |
| **Ekonomi Skala** | Harga per unit lebih murah karena infrastruktur digunakan oleh jutaan pelanggan secara bersamaan |
| **Go Global** | Deploy aplikasi ke seluruh dunia dalam hitungan menit tanpa investasi infrastruktur tambahan |

---

### 2. Infrastruktur Global AWS

AWS memiliki infrastruktur yang tersebar secara global dengan tiga komponen utama:

```
┌─────────────────────────────────────────────────┐
│                  AWS Global Network             │
│                                                 │
│  ┌───────────┐    ┌───────────┐                 │
│  │  Region   │    │  Region   │   ...           │
│  │           │    │           │                 │
│  │  ┌─────┐  │    │  ┌─────┐  │                 │
│  │  │ AZ  │  │    │  │ AZ  │  │                 │
│  │  ├─────┤  │    │  ├─────┤  │                 │
│  │  │ AZ  │  │    │  │ AZ  │  │                 │
│  │  └─────┘  │    │  └─────┘  │                 │
│  └───────────┘    └───────────┘                 │
│                                                 │
│  🌐 Edge Locations (tersebar lebih luas)        │
└─────────────────────────────────────────────────┘
```

#### 🗺️ AWS Regions
- Lokasi fisik di seluruh dunia tempat AWS memiliki **klaster pusat data**
- Setiap Region bersifat **independen** satu sama lain
- Pemilihan Region mempertimbangkan: regulasi data, latensi, biaya, dan ketersediaan layanan

#### 🏢 Availability Zones (AZ)
- Satu atau lebih **pusat data terisolasi** di dalam satu Region
- Dirancang untuk **High Availability (HA)** — jika satu AZ gagal, AZ lain tetap berjalan
- Terhubung satu sama lain via jaringan berkecepatan tinggi dan low-latency

#### 🌐 Edge Locations
- Titik lokasi tambahan yang tersebar lebih luas dari Region
- Digunakan untuk **pengiriman konten (CDN)** dengan latensi rendah kepada pengguna akhir
- Mendukung layanan seperti **Amazon CloudFront**

---

### 3. Model Layanan Cloud

Terdapat tiga model layanan utama dalam Cloud Computing, dibedakan berdasarkan **tingkat kontrol dan tanggung jawab**:

```
                      ← Lebih banyak kontrol
┌──────────┬──────────┬──────────┐
│   IaaS   │   PaaS   │   SaaS   │
└──────────┴──────────┴──────────┘
                      Lebih mudah dikelola →
```

| Model | Kepanjangan | Fokus Pengguna | Contoh Layanan AWS |
|---|---|---|---|
| **IaaS** | Infrastructure as a Service | Kontrol penuh pada infrastruktur virtual (OS, storage, networking) | Amazon EC2, Amazon S3 |
| **PaaS** | Platform as a Service | Fokus pada pengembangan & pengelolaan aplikasi, tanpa mengurus OS | AWS Elastic Beanstalk, AWS Lambda |
| **SaaS** | Software as a Service | Menggunakan produk matang yang sepenuhnya dikelola penyedia | Amazon WorkMail, Salesforce |

---

## 🔑 Poin Kunci & Takeaway

- [ ] Cloud computing mengubah cara pandang terhadap **investasi infrastruktur IT**
- [ ] Model **pay-as-you-go** menghilangkan risiko *over-provisioning* atau *under-provisioning*
- [ ] Infrastruktur AWS terdiri dari **Regions → AZs → Edge Locations** (hierarki dari besar ke kecil)
- [ ] Pilih model layanan (**IaaS / PaaS / SaaS**) sesuai kebutuhan kontrol dan kompleksitas aplikasi
- [ ] **Elastisitas** adalah keunggulan utama cloud dibanding infrastruktur tradisional

---

## Catatan Materi Lebih Lanjutan

<a href= https://canva.link/cloudfoundations>Klik Materi lebih lanjut ➡️</a>

---

*📅 Dicatat pada: Mei 2026 | 🔄 Terakhir diperbarui: Mei 2026*
*✍️ Bagian dari seri catatan: **AWS Cloud Foundations***

