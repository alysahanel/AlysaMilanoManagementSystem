# AMMS (Alysa Milano Management System)

Sistem Manajemen General Affairs (GA) yang komprehensif untuk mengelola permintaan barang, inventaris, dan alur kerja persetujuan.

## 🚀 Fitur Utama

- **Manajemen Inventaris**: Pelacakan stok barang real-time, peringatan stok rendah.
- **Sistem Permintaan (Request)**: Alur kerja pengajuan barang oleh user, persetujuan oleh atasan/admin.
- **Dashboard Interaktif**: Statistik ringkas, grafik, dan widget kalender.
- **Manajemen Pengguna & Peran**: Akses kontrol berbasis peran (Admin, CS, User).
- **Laporan**: Ekspor data stok dan permintaan ke Excel.

## 🛠️ Teknologi yang Digunakan

- **Backend**: Node.js, Express.js
- **Database**: MySQL
- **Frontend**: HTML, Tailwind CSS, JavaScript (Vanilla)
- **Autentikasi**: JWT (JSON Web Tokens), Express Session

## 📂 Struktur Proyek

- `/public`: Halaman frontend (HTML), aset gambar, dan file JS klien.
- `/src`: Kode sumber backend (Controllers, Models, Routes, Config).
- `/scripts`: Skrip utilitas untuk setup database dan migrasi data.

## 📦 Cara Menjalankan

1.  **Clone Repository**
    ```bash
    git clone https://github.com/username/amms-system.git
    cd amms-system
    ```

2.  **Install Dependencies**
    ```bash
    npm install
    ```

3.  **Konfigurasi Database**
    - Buat database MySQL baru bernama `ga_system`.
    - Salin file `.env.example` menjadi `.env`.
    - Sesuaikan konfigurasi database di file `.env`.

4.  **Jalankan Aplikasi**
    ```bash
    npm start
    ```
    Akses aplikasi di `http://localhost:3001`

## 📝 Lisensi

Project ini dibuat untuk keperluan manajemen internal.
