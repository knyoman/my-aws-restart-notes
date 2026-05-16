# Linux Fundamentals & Administration

> **Modul:** Linux Fundamentals & Command Line — AWS re/Start
> **Topik:** Arsitektur Sistem, Manajemen Pengguna/Grup, File System, Otomatisasi, dan Manajemen Log
> **Tanggal:** Mei 2026
> **Status:** ✅ Selesai Dipelajari

---

## Ringkasan Eksekutif

Dokumentasi yang saya buat merangkuman komprehensif mengenai **Dasar-Dasar Linux dan Administrasi Sistem** yang menjadi tulang punggung pengelolaan infrastruktur di lingkungan Cloud dan DevOps — khususnya pada instans Amazon EC2.

Materi mencakup:

- Anatomi perintah terminal
- Manajemen hak akses pengguna dan grup
- Struktur hierarki berkas sistem
- Otomasi tugas (*scripting* & *cron*)
- Taktik investigasi masalah (*troubleshooting*) lewat analisis log

---

## 1. Pengenalan Arsitektur & Anatomi Perintah Linux

### Definisi Inti

Linux adalah sistem operasi *open-source* modular yang dikembangkan di bawah lisensi **GNU GPL**. Dua komponen utamanya adalah:

| Komponen | Peran |
|----------|-------|
| **Kernel** | Pengontrol memori, waktu prosesor CPU, dan perangkat keras fisik |
| **Daemons** | Proses senyap yang berjalan di latar belakang sistem |

---

### Anatomi Perintah Terminal

> **Penting:** Linux bersifat *case-sensitive* — setiap instruksi mengikuti pola baku yang membedakan huruf besar dan kecil.

```
Command  →  Option  →  Argument
```

| Komponen | Penjelasan | Analogi | Contoh |
|----------|------------|---------|--------|
| **Command** | Tugas yang ingin dijalankan | *Drive* (Mengemudi) | `whoami`, `ls`, `ps` |
| **Option** | Modifikasi atau aturan tambahan | *Turn on headlights* (Nyalakan lampu) | `-i`, `-l`, `-ef` |
| **Argument** | Objek atau target yang dikenai perintah | *Car* (Mobilnya) | `/etc/passwd`, `file.txt` |

---

## 2. Manajemen Pengguna, Grup, dan Hak Akses

Linux mengontrol akses sumber daya secara ketat untuk mencegah kebobolan sistem melalui pembagian level akun.

```
← Lebih banyak kontrol (Super-Admin)
┌────────────────────────┬────────────────────────┐
│    Root User  (#)      │   Standard User  ($)   │
└────────────────────────┴────────────────────────┘
                          Lebih terbatas & aman →
```

---

### Hak Akses Administratif: `su` vs `sudo`

| Perintah | Cara Kerja | Kebutuhan Autentikasi |
|----------|------------|-----------------------|
| `su` *(Substitute User)* | Berpindah ke akun lain, termasuk *root* | Password **akun tujuan** |
| `sudo` *(SuperUser Do)* | Menjalankan satu perintah dengan hak tinggi | Password **akun sendiri** |

> **Catatan Keamanan:** Setiap aksi `sudo` otomatis dicatat di file log audit keamanan sistem.

---

### File Konfigurasi Identitas Utama

| File | Fungsi |
|------|--------|
| `/etc/passwd` | Buku daftar akun pengguna: `Username:Password(x):UID:GID:Home:Shell` |
| `/etc/shadow` | Menyimpan kata sandi terenkripsi yang sesungguhnya |
| `/etc/group` | Daftar grup sistem untuk distribusi izin akses massal |
| `/etc/sudoers` | Delegasi hak akses tinggi — **wajib** diedit via `visudo` |

---

## 3. Standar Hierarki File System & Standard Streams

Linux mengadopsi standar pengaturan berkas terpusat dan menganggap **segala hal** — termasuk perangkat keras — sebagai sebuah *file*.

```
/  (Root Directory)
├── /etc     →  Konfigurasi sistem
├── /var     →  Log & data dinamis
├── /home    →  Direktori pengguna
├── /dev     →  File representasi perangkat keras
├── /bin     →  Binary perintah esensial
├── /usr     →  Aplikasi dan utilitas pengguna
└── /tmp     →  File sementara
```

---

### Standard Streams — Aliran Data I/O

Otomatisasi DevOps sangat bergantung pada manipulasi tiga aliran data internal Linux:

| Kode | Stream | Sumber/Tujuan Default | Simbol Pengalihan |
|------|--------|-----------------------|-------------------|
| `0` | **stdin** *(Standard Input)* | Keyboard | `<` |
| `1` | **stdout** *(Standard Output)* | Layar terminal | `>` atau `>>` |
| `2` | **stderr** *(Standard Error)* | Layar terminal | `2>` atau `2>/dev/null` |

> **Tips:** Arahkan `stderr` ke `/dev/null` agar pesan error tidak mengotori tampilan CLI saat skrip berjalan.

---

## 4. Manajemen Proses, Layanan (Services), dan Otomatisasi

### Siklus Hidup Layanan Sistem — `systemctl`

Alat pengontrol untuk mengelola aplikasi atau layanan latar belakang (*services/daemons*) di Linux modern:

| Perintah | Fungsi |
|----------|--------|
| `systemctl status <service>` | Memeriksa status layanan: berjalan, berhenti, atau *error* |
| `systemctl start <service>` | Menyalakan layanan saat ini juga |
| `systemctl stop <service>` | Mematikan layanan saat ini juga |
| `systemctl restart <service>` | Memuat ulang layanan secara penuh |
| `systemctl reload <service>` | Memuat ulang konfigurasi tanpa *downtime* |
| `systemctl enable <service>` | Aktifkan layanan saat server *booting* |
| `systemctl disable <service>` | Nonaktifkan layanan saat server *booting* |

---

### Penjadwalan Tugas Otomatis

| Utilitas | Kegunaan | Karakteristik |
|----------|----------|---------------|
| `at` | Eksekusi skrip satu kali pada waktu tertentu | Non-berulang |
| `cron` & `crontab` | Eksekusi skrip secara rutin dan berulang | Berulang (periodik) |

#### Sintaks Crontab

```
[Menit] [Jam] [Tanggal] [Bulan] [Hari] [Perintah/Skrip]
```

**Contoh:**
```bash
# Jalankan backup setiap hari pukul 02:00
0 2 * * * /home/ec2-user/scripts/backup.sh

# Jalankan setiap Senin pukul 08:30
30 8 * * 1 /home/ec2-user/scripts/weekly-report.sh
```

---

## 5. Investigasi Masalah Melalui Log Sistem

Seluruh aktivitas krusial sistem dicatat di dalam direktori `/var/log` dengan tingkat keparahan (*severity levels*):

```
Level 0  →  Emergency   (Sistem tidak dapat digunakan)
Level 1  →  Alert       (Tindakan segera diperlukan)
Level 2  →  Critical    (Kondisi kritis)
Level 3  →  Error       (Kondisi error)
Level 4  →  Warning     (Kondisi peringatan)
Level 5  →  Notice      (Kondisi normal namun signifikan)
Level 6  →  Info        (Pesan informatif)
Level 7  →  Debug       (Informasi debug detail)
```

---

### Peta File Log Vital

| File Log | Kegunaan Investigasi | Perintah Analisis |
|----------|----------------------|-------------------|
| `/var/log/secure` atau `auth.log` | Kegagalan login, serangan siber, audit `sudo` | `tail -f /var/log/secure \| grep "FAILED"` |
| `/var/log/syslog` atau `messages` | Error sistem umum dan aplikasi | `less /var/log/syslog` |
| `/var/log/cron.log` | Verifikasi jadwal eksekusi skrip otomatis | `cat /var/log/cron.log` |

> **Manajemen Log:** Utilitas **`logrotate`** bekerja secara otomatis untuk melakukan kompresi, rotasi nama, dan penghapusan berkas log lama agar kapasitas disk server cloud tetap terjaga.

---

## Poin Kunci & Takeaway

- [ ] Linux bersifat **case-sensitive** — kesalahan satu huruf kapital dapat menggagalkan perintah sepenuhnya
- [ ] Gunakan tombol **Tab** untuk *Tab Completion* — melengkapi perintah/nama file dengan cepat dan akurat
- [ ] Selalu jalankan `systemctl enable` pada layanan kritis (Nginx/Apache) di AWS EC2 agar server pulih otomatis saat terjadi *restart* mendadak
- [ ] Izin eksekusi (`chmod +x` atau `chmod 744`) adalah **syarat mutlak** sebelum file Bash Script `.sh` dapat dijalankan dengan `./`
- [ ] Utilitas `wget` lebih andal dari `curl` untuk unduhan besar karena mendukung *auto-retry* saat koneksi terputus

---

## Catatan Materi Lebih Lanjutan

<a href= https://canva.link/cloudfoundations>Klik Materi lebih lanjut ➡️</a>

---

*📅 Dicatat pada: Mei 2026 | 🔄 Terakhir diperbarui: Mei 2026*
*✍️ Bagian dari seri catatan: **AWS re/Start Linux Fundamentals***