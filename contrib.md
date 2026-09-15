# 🤝 PANDUAN INISIASI & KONTRIBUSI PENGEMBANGAN (CONTRIB.MD)
## Cara Menjalankan Proyek DPMPTSP Kabupaten Bantaeng di Komputer Lokal
### Panduan Lengkap Langkah-Demi-Langkah untuk Programmer Pemula

---

## 🌟 Selamat Datang, Pengembang!

Terima kasih telah bergabung dalam pembangunan **Sistem Informasi Perizinan, Non-Perizinan, dan Investasi DPMPTSP Kabupaten Bantaeng**. 

Jika Anda seorang programmer pemula dan baru pertama kali menjalankan proyek Laravel 11 dengan Filament v3 dan PostgreSQL di laptop atau komputer Anda, jangan merasa terintimidasi! Panduan ini dirancang dari **nol mutlak (*Zero-to-Hero*)**. Ikuti langkah-langkah di bawah ini secara berurutan, dan sistem Anda akan menyala dengan sempurna.

---

## 💻 1. Kebutuhan Perangkat Keras & Lunak (Prerequisites)

Sebelum mulai menyentuh proyek, pastikan komputer Anda telah terpasang kakas-kakas dasar berikut:

| Kakas / Software | Versi Minimal | Fungsi Utama |
| :--- | :--- | :--- |
| **Sistem Operasi** | Windows 10/11, macOS, atau Linux (Ubuntu 20.04+) | Kompatibel di semua sistem operasi utama. |
| **Git** | Versi 2.40+ | Mengambil kode dari GitHub dan mengelola versi perubahan. |
| **PHP** | **Versi 8.3.x** | Mesin utama pengeksekusi kode backend Laravel. |
| **Composer** | **Versi 2.8+** | Manajer paket dependensi PHP (seperti toko aplikasi untuk backend). |
| **Node.js & npm** | **Versi 20 LTS atau 22 LTS** | Mengompilasi antarmuka frontend (Tailwind CSS, Vite, Leaflet). |
| **PostgreSQL** | **Versi 15 atau 16** | Basis data tempat seluruh transaksi izin dan pengguna disimpan. |

---

## 📥 2. Panduan Instalasi Kakas Dasar (Jika Belum Terpasang)

### A. Untuk Pengguna Windows:

Jika Anda menggunakan Windows, cara termudah adalah menggunakan **Winget** melalui terminal PowerShell:

1. Buka **PowerShell** (tekan tombol `Windows + X` lalu pilih Terminal/PowerShell).
2. Pasang Git:
   ```powershell
   winget install --id Git.Git -e --source winget
   ```
3. Pasang Node.js LTS:
   ```powershell
   winget install --id OpenJS.NodeJS.LTS -e --source winget
   ```
4. Pasang PHP 8.3:
   ```powershell
   winget install --id PHP.PHP.8.3 --skip-dependencies -e --source winget
   ```
5. **Mengaktifkan Ekstensi PHP yang Wajib**:
   Buka file `php.ini` pada folder instalasi PHP Anda, lalu pastikan tanda titik koma (`;`) di depan baris berikut **sudah dihapus**:
   ```ini
   extension_dir = "ext"
   extension=curl
   extension=fileinfo
   extension=gd
   extension=intl
   extension=mbstring
   extension=openssl
   extension=pdo_pgsql
   extension=pgsql
   extension=zip
   ```
6. Pasang Composer:
   Unduh penginstal resmi dari [getcomposer.org](https://getcomposer.org/Composer-Setup.exe) lalu jalankan.
7. Pasang PostgreSQL:
   Unduh installer PostgreSQL 16 dari [enterprisedb.com/downloads/postgres-postgresql-downloads](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads). Saat instalasi, catat password untuk pengguna `postgres` (misal: `root` atau `admin`).

---

## 🚀 3. Langkah Kloning & Pengaturan Repositori

Buka Terminal / Command Prompt Anda dan arahkan ke folder tempat Anda biasa menyimpan kode:

```bash
# 1. Kloning repositori resmi dari GitHub
git clone https://github.com/HaikalAkbar13/dpmptsp.git

# 2. Masuk ke dalam direktori proyek
cd dpmptsp
```

---

## ⚙️ 4. Pengaturan Berkas Lingkungan (`.env`)

Laravel memerlukan berkas konfigurasi bernama `.env`. Berkas ini sengaja tidak dimasukkan ke GitHub demi alasan keamanan kata sandi.

### Salin Template Konfigurasi:
- **Di Windows (PowerShell / CMD)**:
  ```powershell
  copy .env.example .env
  ```
- **Di Linux / macOS**:
  ```bash
  cp .env.example .env
  ```

### Menyesuaikan Isi `.env`:
Buka file `.env` yang baru disalin menggunakan Text Editor (seperti VS Code), lalu cari dan sesuaikan baris-baris berikut:

```env
APP_NAME="DPMPTSP Kabupaten Bantaeng"
APP_ENV=local
APP_KEY=
APP_DEBUG=true
APP_TIMEZONE=Asia/Makassar
APP_URL=http://localhost:8000

# Pengaturan Basis Data PostgreSQL:
DB_CONNECTION=pgsql
DB_HOST=127.0.0.1
DB_PORT=5432
DB_DATABASE=dpmptsp_bantaeng
DB_USERNAME=postgres
DB_PASSWORD=password_postgres_anda_disini
```

> 💡 **TIPS KHUSUS PEMULA (Jika Belum Ingin Setup PostgreSQL):**  
> Jika Anda hanya ingin melihat tampilan frontend secara cepat tanpa menyalakan server PostgreSQL, Anda dapat mengubah koneksi menjadi SQLite dengan sangat mudah:  
> `DB_CONNECTION=sqlite`  
> (Sistem akan otomatis menggunakan file database lokal di `database/database.sqlite`).

---

## 📦 5. Memasang Paket Dependensi Proyek

Jalankan dua perintah berikut di terminal:

```bash
# 1. Unduh pustaka Backend (Laravel, Filament, DomPDF)
composer install

# 2. Unduh pustaka Frontend (Tailwind CSS, Vite, Leaflet)
npm install
```

---

## 🔑 6. Membuat Kunci Aplikasi & Menyiapkan Database

Jalankan perintah-perintah berikut secara bertahap:

```bash
# 1. Buat kunci enkripsi keamanan aplikasi
php artisan key:generate

# 2. Pastikan database 'dpmptsp_bantaeng' sudah dibuat di PostgreSQL Anda
# (Bisa dibuat via software pgAdmin atau jalankan perintah SQL: CREATE DATABASE dpmptsp_bantaeng;)

# 3. Jalankan migrasi tabel dan masukkan data sampel awal (Seeder)
php artisan migrate --seed

# 4. Hubungkan jalan pintas penyimpanan berkas publik (Storage Link)
php artisan storage:link
```

> **Apa itu `storage:link`?**  
> Secara default, file yang diunggah pengguna disimpan di folder aman `storage/`. Perintah ini membuat "jalan pintas (*symbolic link*)" agar aset publik seperti logo dan foto banner wisata Bantaeng dapat ditampilkan di browser.

---

## 🖥️ 7. Menjalankan Server Pengembangan Lokal

Untuk melihat website berjalan di laptop Anda, Anda membutuhkan **Dua Jendela Terminal**:

### Terminal 1 (Menjalankan Mesin Backend):
```bash
php artisan serve
```
*Output:* `Server running on [http://127.0.0.1:8000]`

### Terminal 2 (Menjalankan Mesin Frontend & Vite):
```bash
npm run dev
```
*Output:* `VITE v8.0.0 ready in 150 ms`

🎉 **Selamat!** Sekarang buka browser favorit Anda dan kunjungi:
- **Halaman Depan Publik**: `http://localhost:8000`
- **Panel Backoffice Admin**: `http://localhost:8000/admin`
- **Portal Pemohon / Warga**: `http://localhost:8000/portal`

---

## 👥 8. Akun Percobaan untuk Pengujian (Demo Accounts)

Setelah menjalankan `php artisan migrate --seed`, database Anda telah dilengkapi dengan akun pengujian untuk setiap peran:

| Peran Jabatan | Alamat Email | Kata Sandi | Halaman Akses | Hak Istimewa |
| :--- | :--- | :--- | :--- | :--- |
| **Super Admin** | `admin@bantaengkab.go.id` | `password` | `/admin` | Mengelola jenis izin dinamis, kelola pengguna, & pengaturan sistem. |
| **Front Office (Loket)** | `loket@bantaengkab.go.id` | `password` | `/admin` | Memeriksa kelengkapan berkas pemohon & meminta revisi. |
| **Tim Teknis OPD** | `teknis.dinkes@bantaengkab.go.id` | `password` | `/admin` | Mengunggah rekomendasi teknis izin kesehatan (SIP Dokter/Bidan). |
| **Kepala Bidang (Kabid)** | `kabid@bantaengkab.go.id` | `password` | `/admin` | Memvalidasi dan menyetujui draf izin yang telah lolos uji teknis. |
| **Kepala Dinas (Kadis)** | `kadis@bantaengkab.go.id` | `password` | `/admin` | Pengesahan akhir izin dan penerbitan Surat Izin ber-QR Code. |
| **Pemohon (Warga)** | `pemohon@gmail.com` | `password` | `/portal` | Mengajukan izin baru, memantau riwayat, dan unduh dokumen izin. |

---

## 🛠️ 9. Solusi Masalah Populer (Troubleshooting)

### 1. Pesan Error: *"could not find driver"* saat migrasi
- **Penyebab**: Ekstensi PostgreSQL di PHP Anda belum menyala.
- **Solusi**: Buka file `php.ini`, pastikan baris `extension=pdo_pgsql` dan `extension=pgsql` tidak diawali tanda `;`, simpan file, lalu restart terminal Anda.

### 2. Tampilan Admin Filament atau Tailwind CSS Berantakan (CSS Tidak Muncul)
- **Penyebab**: Aset frontend belum dikompilasi atau asset link belum dipublikasikan.
- **Solusi**: Jalankan perintah pembersihan dan kompilasi:
  ```bash
  php artisan filament:upgrade
  npm run build
  ```

### 3. Port 8000 Sudah Terpakai (*Address already in use*)
- **Solusi**: Jalankan server di nomor port lain yang masih kosong:
  ```bash
  php artisan serve --port=8080
  ```

---

## 🌿 10. Standar Kontribusi Kode (Git Workflow)

Bila Anda ingin berkontribusi menambah fitur atau memperbaiki bug:

1. **Pastikan branch utama Anda selalu segar**:
   ```bash
   git checkout main
   git pull origin main
   ```
2. **Buat branch baru dengan nama yang jelas**:
   ```bash
   git checkout -b fitur/peta-investasi-kiba
   # atau jika perbaikan bug:
   git checkout -b perbaikan/validasi-nik-ktp
   ```
3. **Format Pesan Commit (Conventional Commits)**:
   - `feat: tambah filter sektor pada peta potensi investasi`
   - `fix: perbaiki validasi unggah berkas KTP yang melebihi 2MB`
   - `docs: lengkapi panduan alur disposisi di BACKEND.md`
   - `style: perbaiki kontras warna tombol hero section pada mode gelap`
4. **Kirim Perubahan Anda ke GitHub**:
   ```bash
   git push origin fitur/peta-investasi-kiba
   ```
5. Buka repositori GitHub dan buat **Pull Request (PR)** agar ditinjau oleh tim pengembang utama.

---
*Semoga panduan ini membantu Anda memulai dengan lancar. Selamat berkarya untuk kemajuan pelayanan publik Kabupaten Bantaeng!*
