# Cobek Bakar — Dokumentasi Proyek

Ringkasan:  
Cobek Bakar adalah aplikasi kasir sederhana berbasis web untuk mengelola penjualan, produk, transaksi kasir, dan laporan. Aplikasi ini cocok untuk usaha kuliner kecil yang membutuhkan sistem kasir cepat dan mudah dipakai.

## 1. Deskripsi Aplikasi
Cobek Bakar adalah sistem kasir yang memungkinkan:
- Pengelolaan produk (menu), harga, dan stok.
- Proses transaksi kasir (pemilihan item, hitung total, bayar).
- Pencetakan struk transaksi (print receipt).
- Pembuatan laporan penjualan harian/bulanan dan pencetakan laporan.

Aplikasi ini dikembangkan untuk membantu usaha kecil menengah (UMKM) agar proses kasir menjadi lebih cepat, rapi, dan terekam.

## 2. Tujuan & Masalah yang Diselesaikan
Tujuan utama:
- Mempercepat proses transaksi penjualan.
- Menyediakan rekam transaksi dan laporan untuk pengambilan keputusan.
- Mengurangi kesalahan perhitungan manual dan pencatatan.

Masalah yang diselesaikan:
- Pencatatan penjualan manual yang rawan salah.
- Sulitnya mendapatkan laporan penjualan yang terstruktur.
- Butuh struk fisik untuk pelanggan secara cepat.

## 3. Fitur Utama
- Login / autentikasi pengguna (level: kasir, admin — sesuai implementasi).
- Kelola produk/menu (tambah, edit, hapus).
- Antarmuka kasir untuk input pesanan dan pembayaran.
- Cetak struk transaksi.
- Laporan penjualan (harian/bulanan) dan kemampuan cetak/ekspor.
- (Opsional) Manajemen pengguna, riwayat transaksi.

## 4. Teknologi yang Digunakan
(Deskripsi umum — sesuaikan dengan isi repo)
- Bahasa server: PHP (file .php)
- Database: MySQL / MariaDB
- Frontend: HTML, CSS, JavaScript (mungkin menggunakan Bootstrap / jQuery)
- Webserver: Apache / Nginx
- Library yang mungkin dipakai untuk cetak/PDF: FPDF / TCPDF, atau hanya memanfaatkan window.print()

> Catatan: Periksa struktur repo untuk melihat paket atau library spesifik yang dipakai (mis. folder `vendor/`, file `composer.json`, atau library JS di `assets/`).

## 5. Prasyarat (untuk menjalankan di localhost)
- PHP 7.x atau terbaru (sesuai yang diperlukan proyek)
- MySQL / MariaDB
- Web server (XAMPP, MAMP, Laragon, atau LAMP stack)
- Ekstensi PHP umum: mysqli atau PDO, mbstring, gd (jika diperlukan)
- Browser modern untuk fitur cetak

## 6. Cara Menjalankan Aplikasi di Localhost (panduan cepat)
1. Clone repositori:
   - git clone https://github.com/NovianaSafitri/cobekbakar.git

2. Salin/move folder proyek ke document root webserver Anda:
   - Untuk XAMPP: tempatkan di `C:\xampp\htdocs\cobekbakar`
   - Untuk Linux (Apache): `/var/www/html/cobekbakar`

3. Buat database MySQL:
   - Masuk ke MySQL/MariaDB dan buat database baru, misal `cobekbakar_db`.
   - mysql -u root -p
   - CREATE DATABASE cobekbakar_db CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

4. Import struktur dan data awal database:
   - Cari file dump/SQL di repo (umumnya bernama `database.sql`, `dump.sql`, atau berada di folder `db/`/`sql/`).
   - Jika menemukan file SQL:
     - mysql -u root -p cobekbakar_db < path/to/file.sql
   - Jika tidak ada file SQL, periksa README proyek di repo untuk instruksi atau hubungi maintainer.

5. Sesuaikan konfigurasi koneksi database:
   - Cari file config / koneksi, mis. `config.php`, `koneksi.php`, atau `inc/config.php`.
   - Ubah credential DB menjadi milik Anda (host, username, password, database). Contoh sederhana:
     ```php
     <?php
     // contoh konfigurasi (sesuaikan nama file/variabel sesuai repo)
     $db_host = 'localhost';
     $db_user = 'root';
     $db_pass = '';
     $db_name = 'cobekbakar_db';
     $conn = new mysqli($db_host, $db_user, $db_pass, $db_name);
     if ($conn->connect_error) {
         die("Koneksi gagal: " . $conn->connect_error);
     }
     ```
   - Simpan perubahan.

6. Akses aplikasi di browser:
   - http://localhost/cobekbakar/login.php (atau alamat root proyek sesuai struktur)
   - Jika menggunakan virtual host, akses sesuai konfigurasi Anda.

7. Debugging:
   - Jika halaman error, cek log PHP / webserver.
   - Pastikan ekstensi PHP yang diperlukan aktif dan DB sudah terimport.

## 7. Akun Demo
- Role: Kasir
  - Username: admin
  - Password: admincobekbakar

> Catatan: Jika akun demo tidak berfungsi, pastikan data pengguna telah diimport dari file SQL atau cek tabel pengguna pada database.

## 8. Link Deployment (Live)
- Aplikasi live / demo dapat diakses di:
  - https://websitecobekbakar.wuaze.com/login.php

Gunakan akun demo di atas untuk masuk (jika tersedia pada server publik).

## 9. Catatan Tambahan untuk Fitur Cetak Struk & Cetak Laporan
- Cetak Struk:
  - Implementasi sederhana biasanya membuka jendela baru (print preview) berisi layout struk dan memanggil window.print() sehingga user dapat mencetak lewat browser.
  - Untuk printer thermal (ESC/POS), sering diperlukan software atau library khusus untuk mengirim perintah ESC/POS ke printer. Opsi:
    - Gunakan aplikasi bridge (mis. QZ Tray) untuk mengirim perintah langsung dari browser ke printer thermal.
    - Atau generate PDF/HTML struk yang dapat diunduh dan dicetak via sistem operasi.
  - Pastikan juga ukuran kertas sudah diatur (mis. 80mm) agar struk tercetak rapi.

- Cetak Laporan:
  - Laporan biasanya dihasilkan dalam format HTML dan dapat langsung dicetak lewat browser.
  - Untuk distribusi dan arsip, sediakan opsi ekspor ke PDF dan/atau CSV.
  - Jika server menghasilkan PDF (FPDF/TCPDF/dompdf), pastikan ekstensi PHP yang diperlukan terpasang dan hak akses folder untuk menyimpan file.

- Kinerja & Keamanan Cetak:
  - Matikan op-display errors di production (setiap sensitive info harus disembunyikan).
  - Batasi akses ke fungsi cetak laporan untuk user dengan hak yang sesuai (mis. admin).
  - Untuk menjalankan cetak otomatis atau batch report, pastikan server memiliki resource yang cukup atau gunakan job queue.

## 10. Troubleshooting Umum
- Halaman kosong / error 500:
  - Aktifkan sementara display_errors atau cek file log PHP / webserver.
- Koneksi database gagal:
  - Cek kredensial di file konfigurasi, pastikan database sudah dibuat dan diimport.
- CSS/JS tidak muncul:
  - Pastikan path asset (folder `assets/`, `css/`, `js/`) benar relatif ke lokasi file yang diakses.
- Fitur cetak tidak rapi:
  - Periksa CSS khusus untuk media print (`@media print`) dan ukuran kertas.

## 11. Cara Berkontribusi
- Fork repo, buat branch fitur/bugfix, buat PR dengan penjelasan perubahan.
- Sertakan langkah reproduksi untuk bug dan screenshot bila perlu.
- Ikuti konvensi penamaan dan struktur yang ada di repo.

## 12. Lisensi & Kontak
- Lisensi: (Jika ada file LICENSE di repo, lihat file tersebut. Jika tidak ada, tambahkan lisensi yang diinginkan, mis. MIT.)
- Kontak maintainer: lihat profil GitHub repo (pemilik: NovianaSafitri) atau tambahkan email di file CONTRIBUTORS/README jika ingin kontak langsung.

---

Jika Anda mau, saya bisa:
- Menyusun README yang langsung siap dipasang ke repo dengan penyesuaian nama file konfigurasi yang tepat kalau Anda beri tahu lokasi file koneksi (mis. `koneksi.php` atau `config.php`).
- Membuat panduan langkah demi langkah yang disesuaikan jika Anda kirim struktur folder (daftar file penting seperti file SQL, file config, folder assets).
