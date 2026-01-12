# 💒 Wedding WA Sender

Aplikasi pengiriman undangan pernikahan via WhatsApp secara massal maupun manual. Dibangun dengan Laravel 12 dan terintegrasi dengan [WAHA (WhatsApp HTTP API)](https://waha.devlike.pro/).

![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)

---

## 📋 Daftar Isi

- [Fitur](#-fitur)
- [Prasyarat](#-prasyarat)
- [Instalasi](#-instalasi)
- [Konfigurasi](#-konfigurasi)
- [Cara Penggunaan](#-cara-penggunaan)
  - [Kirim Massal via Excel](#1-kirim-massal-via-excel)
  - [Kirim Manual](#2-kirim-manual)
- [Format File Excel](#-format-file-excel)
- [Struktur Proyek](#-struktur-proyek)
- [Log Pengiriman](#-log-pengiriman)
- [Troubleshooting](#-troubleshooting)
- [Lisensi](#-lisensi)

---

## ✨ Fitur

- 📤 **Kirim Undangan Massal** - Upload file Excel dengan daftar tamu dan kirim undangan ke semua kontak sekaligus
- ✍️ **Kirim Manual** - Kirim undangan satu per satu dengan input nama dan nomor WhatsApp
- 📋 **Verifikasi Data** - Preview dan verifikasi data sebelum pengiriman
- 📥 **Download Template** - Template Excel siap pakai untuk format yang benar
- ⚡ **Pengiriman Paralel** - Kirim pesan secara paralel (10 kontak per batch) untuk efisiensi
- 📝 **Logging** - Catatan lengkap setiap pengiriman berhasil
- 📱 **Contact Picker** - Pilih kontak langsung dari HP (pada browser yang mendukung)
- 📱 **Responsive** - Tampilan optimal di desktop maupun mobile

---

## 📦 Prasyarat

Sebelum menginstal, pastikan sistem Anda memiliki:

- **PHP** >= 8.2
- **Composer** >= 2.x
- **Node.js** >= 18.x (untuk build assets)
- **WAHA** (WhatsApp HTTP API) berjalan di `http://localhost:3000`

### Menginstal WAHA

WAHA dapat dijalankan menggunakan Docker:

```bash
docker run -d --name waha \
  -p 3000:3000 \
  -e WHATSAPP_DEFAULT_ENGINE=WEBJS \
  devlikeapro/waha
```

Atau lihat dokumentasi lengkap di: https://waha.devlike.pro/docs/how-to/

---

## 🚀 Instalasi

### 1. Clone Repository

```bash
git clone https://github.com/wahyukurniaaaa/wedding-wa-sender.git
cd wedding-wa-sender
```

### 2. Install Dependencies

```bash
# Install PHP dependencies
composer install

# Install Node dependencies
npm install
```

### 3. Setup Environment

```bash
# Copy file environment
cp .env.example .env

# Generate application key
php artisan key:generate
```

### 4. Setup Database

```bash
# Buat file database SQLite
touch database/database.sqlite

# Jalankan migrasi
php artisan migrate
```

### 5. Build Assets

```bash
# Development
npm run dev

# Atau untuk production
npm run build
```

### 6. Jalankan Aplikasi

```bash
php artisan serve
```

Aplikasi akan berjalan di `http://localhost:8000`

---

## ⚙️ Konfigurasi

### Environment Variables

Edit file `.env` dan tambahkan konfigurasi berikut:

```env
# WAHA API Configuration
WAHA_API_KEY=your_waha_api_key_here
```

### Konfigurasi WAHA

1. Buka WAHA Dashboard di `http://localhost:3000`
2. Klik "Start Session" untuk memulai sesi WhatsApp
3. Scan QR Code dengan WhatsApp di HP Anda
4. Pastikan sesi bernama `default` (atau sesuaikan di controller)

---

## 📖 Cara Penggunaan

### 1. Kirim Massal via Excel

#### Langkah 1: Persiapkan File Excel

Download template terlebih dahulu atau buat file Excel dengan format:

| Nama | Nomor HP |
|------|----------|
| Budi Santoso | 6281234567890 |
| Siti Aminah | 6289876543210 |

> ⚠️ **Penting:** Nomor HP harus dalam format internasional (awalan `62` tanpa tanda `+`)

#### Langkah 2: Upload File

1. Buka aplikasi di `http://localhost:8000`
2. Klik tombol **"Choose File"** atau **"Pilih File"**
3. Pilih file Excel (.xlsx atau .xls)
4. Klik **"Upload & Lihat Data"**

#### Langkah 3: Verifikasi Data

1. Periksa tabel preview yang muncul
2. Pastikan nama dan nomor HP sudah benar
3. Jika ada kesalahan, klik **"Batalkan"** dan perbaiki file Excel

#### Langkah 4: Kirim Undangan

1. Jika data sudah benar, klik **"Lanjutkan Kirim Pesan"**
2. Tunggu proses pengiriman selesai
3. Lihat hasil pengiriman (berhasil/gagal)

---

### 2. Kirim Manual

1. Dari halaman utama, klik **"Kirim Manual"**
2. Isi **Nama Tamu** (wajib)
3. Isi **Nomor WhatsApp** (opsional)
   - Format: `6281234567890` (tanpa tanda `+`)
   - Atau gunakan tombol **"Pilih dari Kontak"** (jika di HP)
4. Klik **"Buat Undangan"**
5. Anda akan mendapat link WhatsApp yang bisa langsung dibuka
6. Klik link tersebut untuk membuka WhatsApp dengan pesan yang sudah terisi

---

## 📊 Format File Excel

### Template Excel

Download template langsung dari aplikasi dengan klik tombol **"Download Template"**.

### Struktur Kolom

| Kolom | Nama Kolom | Contoh | Keterangan |
|-------|------------|--------|------------|
| A | Nama | Budi Santoso | Nama lengkap tamu |
| B | Nomor HP | 6281234567890 | Format internasional |

### Contoh File Excel

| Nama | Nomor HP |
|------|----------|
| Keluarga Besar Pak Haji | 6281111222333 |
| Budi dan Istri | 6282222333444 |
| Sari Dewi | 6283333444555 |

### ⚠️ Hal yang Perlu Diperhatikan

1. **Baris pertama adalah header** - Akan otomatis dilewati
2. **Format nomor HP:**
   - ✅ Benar: `6281234567890`
   - ❌ Salah: `081234567890`, `+6281234567890`, `08-123-456-7890`
3. **Jangan ada baris kosong** di tengah data
4. **Maksimal kolom:** Hanya 2 kolom (A dan B)

---

## 📁 Struktur Proyek

```
wedding-wa-sender/
├── app/
│   ├── Exports/
│   │   └── WaInvitationTemplateExport.php  # Template Excel
│   ├── Http/
│   │   └── Controllers/
│   │       └── WaInvitationController.php  # Controller utama
│   └── Models/
│       └── User.php
├── resources/
│   └── views/
│       ├── upload.blade.php         # Halaman upload
│       ├── verify.blade.php         # Halaman verifikasi
│       ├── manual.blade.php         # Form kirim manual
│       └── manual_result.blade.php  # Hasil link WA
├── routes/
│   └── web.php                      # Definisi routes
├── storage/
│   ├── app/private/temp/            # File Excel sementara
│   └── logs/                        # Log pengiriman
└── ...
```

---

## 📝 Log Pengiriman

Setiap pengiriman berhasil akan dicatat di folder `storage/logs/` dengan format:

**Nama File:** `wa_invitation_log_YYYYMMDD_HHMMSS.txt`

**Contoh Isi Log:**
```
[2026-01-12 10:30:45] STATUS: success | Nama: Budi Santoso | No: 6281234567890 | ChatID: 6281234567890@c.us | MsgID: ABC123XYZ
Links: https://event.digitainvite.id/miftah-wahyu/?to=Budi%20Santoso
----------------------
```

---

## 🔧 Troubleshooting

### 1. Error: "File tidak ditemukan!"

**Penyebab:** Session expired atau file temp sudah dihapus.

**Solusi:** Upload ulang file Excel.

### 2. Pesan tidak terkirim

**Penyebab:**
- WAHA tidak berjalan
- Session WhatsApp belum terhubung
- Nomor HP tidak valid

**Solusi:**
1. Pastikan WAHA berjalan di `http://localhost:3000`
2. Cek status session di WAHA Dashboard
3. Pastikan format nomor HP benar (awalan `62`)

### 3. Error: "WAHA_API_KEY not set"

**Solusi:** Tambahkan `WAHA_API_KEY` di file `.env`

### 4. Halaman loading lama saat kirim massal

**Penyebab:** Banyak data yang diproses.

**Solusi:** Ini normal. Pengiriman dilakukan secara paralel 10 kontak per batch.

### 5. Format nomor tidak valid

**Solusi:** Gunakan format internasional:
- Ubah `08xxxx` menjadi `628xxxx`
- Hilangkan tanda `+`, spasi, dan strip

---

## 🛠️ Development

### Menjalankan Development Server

```bash
# Jalankan semua service sekaligus
composer dev
```

Ini akan menjalankan:
- PHP development server
- Queue listener
- Laravel Pail (log viewer)
- Vite dev server

### Testing

```bash
# Jalankan test
composer test
```

---

## 📄 Lisensi

Proyek ini dilisensikan di bawah [MIT License](LICENSE).

---

## 👨‍💻 Kontributor

- **Wahyu Kurnia Prambudi** - *Developer*

---

<p align="center">
  Made with ❤️ untuk pernikahan <b>Siti Miftahul Jannah & Wahyu Kurnia Prambudi</b>
</p>
