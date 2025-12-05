# Cobek Bakar — Dokumentasi Proyek

Dokumentasi ini disusun untuk memberi gambaran lengkap dan terstruktur tentang proyek "cobekbakar". Tujuannya agar pengembang baru, reviewer, atau pemilik proyek dapat memahami modul, fitur, tampilan, alur logika, dan titik-titik penting lain dengan cepat dan nyaman dibaca.

> Ringkasan singkat: proyek ini adalah aplikasi web berbasis PHP untuk manajemen produk dan transaksi (kasir/penjualan), dengan fitur laporan, histori transaksi, dan cetak struk/laporan. Struktur file utama terlihat pada repository, termasuk file backend PHP, endpoint AJAX, file JavaScript untuk interaksi UI, dan stylesheet.

Daftar berkas utama (ditemukan di root repository)
- config.php
- index.php
- login.php, proses_login.php, logout.php
- product.php, proses_produk.php, get_products.php
- proses_transaksi.php, struk.php
- report.php, get_reports.php, cetak_laporan.php, script-report.js
- history.php
- script.js, script-product.js
- style.css

---

## Daftar Isi
1. Tujuan & Ringkasan Fitur  
2. Arsitektur & Alur Kerja Singkat  
3. Persiapan & Menjalankan Lokal  
4. Penjelasan Modul / File (per-file)  
5. Tampilan & Alur Antarmuka (UI flows)  
6. Logika Bisnis & Database (ringkasan)  
7. Endpoint / API (AJAX)  
8. Keamanan & Best Practices  
9. Troubleshooting umum  
10. Cara berkontribusi & kontak

---

## 1. Tujuan & Ringkasan Fitur
Aplikasi ini tampak dibuat untuk mengelola produk dan transaksi penjualan serta menghasilkan laporan. Fitur inti yang dapat diidentifikasi:

- Autentikasi pengguna (login, logout)
- Manajemen produk (tambah/ubah/hapus)
- Proses transaksi (kasir): input produk, hitung total, simpan transaksi
- Cetak struk/nota transaksi
- Laporan penjualan (filter berdasarkan tanggal atau parameter lain)
- Cetak laporan (PDF/print friendly)
- Histori transaksi
- Frontend interaktif dengan JavaScript (script.js, script-product.js, script-report.js)
- Styling lewat style.css

---

## 2. Arsitektur & Alur Kerja Singkat
- Frontend: HTML/PHP yang dirender server + JavaScript untuk permintaan AJAX dan interaksi dinamis.
- Backend: PHP procedural (berdasarkan nama file-file .php) menangani autentikasi, CRUD produk, proses transaksi, dan pembuatan laporan.
- Data: MySQL (diperkirakan) diakses via config.php yang kemungkinan berisi pengaturan koneksi database.
- Output cetak: struk.php dan cetak_laporan.php menyediakan tampilan yang siap untuk dicetak.

Alur umum:
1. Pengguna membuka aplikasi (index.php / login.php)
2. Login melalui proses_login.php -> sesi PHP dikelola
3. Setelah autentikasi: akses halaman utama (index.php) untuk transaksi, product.php untuk manajemen produk, report.php untuk laporan
4. Interaksi dinamis (mencari produk, menambah ke keranjang) dilakukan lewat script-product.js / script.js
5. Proses transaksi disimpan melalui proses_transaksi.php; struk/nota disajikan melalui struk.php
6. Report diakses melalui report.php dan data diambil via get_reports.php; cetak di cetak_laporan.php

---

## 3. Persiapan & Menjalankan Lokal
Langkah-langkah umum untuk menjalankan proyek di lingkungan lokal:

1. Siapkan server PHP + MySQL (mis. XAMPP, MAMP, LAMP).
2. Salin repository ke folder server (htdocs atau www).
3. Buat database baru dan import struktur/tabel yang sesuai (struktur contoh di bagian 6).
4. Edit config.php sesuai kredensial database Anda (host, user, password, dbname).
5. Buka browser: http://localhost/<folder>/login.php atau index.php.
6. Jika ada masalah izin atau sesi, periksa konfigurasi PHP (session.save_path) dan hak akses file.

Contoh SQL skeleton (contoh perkiraan; sesuaikan dengan implementasi nyata):
```sql
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(100) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  role VARCHAR(50),
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  price DECIMAL(12,2) NOT NULL,
  stock INT DEFAULT 0,
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE transactions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  invoice_no VARCHAR(100) NOT NULL,
  user_id INT,
  total DECIMAL(12,2),
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE transaction_items (
  id INT AUTO_INCREMENT PRIMARY KEY,
  transaction_id INT,
  product_id INT,
  qty INT,
  price DECIMAL(12,2)
);
```

---

## 4. Penjelasan Modul / File (per-file)
Di bawah ini penjelasan singkat fungsi tiap file berdasarkan nama dan peran umum:

- config.php  
  Berisi konfigurasi koneksi database dan mungkin pengaturan global (base URL, session start). Sangat penting untuk dikonfigurasi dengan aman.

- login.php  
  Halaman form login (HTML + PHP). Menampilkan input username/password.

- proses_login.php  
  Logika untuk memproses form login: memverifikasi kredensial, memulai session, redirect.

- logout.php  
  Menghancurkan session dan mengarahkan pengguna keluar.

- index.php  
  Halaman utama (dashboard/antarmuka transaksi). Biasanya menampilkan area kasir: daftar produk, keranjang, total, tombol bayar.

- product.php  
  Halaman manajemen produk: daftar produk, form tambah/edit, aksi hapus.

- proses_produk.php  
  Proses backend untuk create/update/delete produk. Menerima POST dari product.php.

- get_products.php  
  Endpoint AJAX untuk mengembalikan daftar produk (mis. JSON) untuk pencarian atau autocomplete di UI kasir.

- proses_transaksi.php  
  Script yang menyimpan transaksi (master + items) ke database setelah pembayaran selesai.

- struk.php  
  Tampilan yang menampilkan struk/nota transaksi yang dapat dicetak. Biasanya mengambil data transaction_id via GET.

- report.php  
  Halaman untuk melihat laporan penjualan; menyediakan filter tanggal, export atau tombol cetak.

- get_reports.php  
  Endpoint AJAX mengembalikan data laporan sesuai filter; mungkin mendukung export CSV/JSON.

- cetak_laporan.php  
  Versi print-friendly dari report.php; dipakai untuk mencetak laporan.

- history.php  
  Menampilkan histori transaksi, mungkin dengan pencarian atau pagination.

- script.js  
  File JS utama: event handling umum (sidebar, modal, AJAX helper, validasi UI).

- script-product.js  
  JS khusus untuk interaksi produk di halaman kasir/product: menambahkan produk ke keranjang, update qty, kalkulasi subtotal.

- script-report.js  
  JS khusus untuk halaman report: menangani filter, permintaan data via AJAX, menampilkan tabel, dan memanggil cetak.

- style.css  
  File stylesheet yang menangani tampilan keseluruhan: layout, form, tabel, media queries.

Catatan: beberapa file berekstensi .php menggabungkan HTML + PHP (tampilan) sehingga mereka berfungsi ganda sebagai view + controller pada pola procedural.

---

## 5. Tampilan & Alur Antarmuka (UI flows)
Berikut deskripsi alur tampilan yang diasumsikan agar pengguna (kasir/admin) dapat memahami interaksi utama.

1. Login
   - Halaman login (login.php)
   - Setelah login valid -> redirect ke index.php (kasir/dashboard)

2. Halaman Kasir (index.php)
   - Pencarian produk (search) yang memanggil get_products.php
   - Menambahkan produk ke keranjang (JS menambah baris keranjang)
   - Menentukan qty, diskon, dll (jika ada)
   - Tombol "Bayar" yang memicu proses_transaksi.php via form submit atau AJAX
   - Setelah sukses -> redirect/show struk.php (nota) dan opsi cetak

3. Manajemen Produk (product.php)
   - Daftar produk (nama, harga, stock)
   - Form untuk tambah/edit produk
   - Hapus produk
   - Perubahan tersimpan via proses_produk.php

4. Laporan (report.php)
   - Filter tanggal (dari-sampai) / filter lain
   - Tampilkan hasil dalam tabel
   - Tombol "Cetak" yang membuka cetak_laporan.php atau memanggil print()

5. History (history.php)
   - Daftar transaksi sebelumnya
   - Link view detail -> menampilkan isi transaksi / struk

Antarmuka akan lebih interaktif karena adanya script.js / script-product.js / script-report.js.

---

## 6. Logika Bisnis & Database (ringkasan)
- Autentikasi: proses_login.php memeriksa username/password. Pastikan penggunaan hashing (password_hash/password_verify) bila belum diterapkan.
- Produk: CRUD sederhana dengan validasi input, pengelolaan stok (opsional). proses_produk.php melakukan insert/update/delete.
- Transaksi: proses_transaksi.php membuat rekaman transaksi beserta item. Seharusnya dilakukan dalam transaksi DB (BEGIN/COMMIT/ROLLBACK) untuk menjaga konsistensi.
- Laporan: agregasi transaksi berdasarkan tanggal, menghitung total penjualan, volume item, dll.
- Cetak: struk dan cetak laporan harus menampilkan data yang lengkap dan dalam format print-friendly.

Perhatian arsitektur:
- Pastikan operasi penyesuaian stok dilakukan atomik saat transaksi dicatat.
- Gunakan prepared statements untuk semua query untuk mencegah SQL injection.

---

## 7. Endpoint / API (AJAX)
Berdasarkan nama file, endpoint yang tersedia:

- get_products.php  
  - Tujuan: mengembalikan daftar produk atau pencarian produk.
  - Input: query (mis. q atau id)
  - Output: JSON (array produk) atau HTML snippet

- get_reports.php  
  - Tujuan: mengembalikan hasil laporan berdasarkan filter (tanggal dsb).
  - Input: start_date, end_date, filter lain
  - Output: JSON atau HTML tabel

- proses_produk.php  
  - Tujuan: menerima form POST untuk menambah/edit/hapus produk.

- proses_transaksi.php  
  - Tujuan: menerima data transaksi (items, total, user) dan menyimpannya ke DB.

Catatan: Pastikan endpoint memvalidasi hak akses (cek session / role) sebelum mengembalikan data sensitif.

---

## 8. Keamanan & Best Practices (rekomendasi)
Berikut beberapa rekomendasi untuk meningkatkan kualitas dan keamanan aplikasi:

1. Authentication & Session
   - Jangan simpan password dalam plaintext. Gunakan password_hash() dan password_verify().
   - Regenerasi session_id() pada login untuk mencegah session fixation.
   - Batasi sesi/inaktivitas (session timeout).

2. Database
   - Gunakan prepared statements / parameterized queries (mysqli_stmt atau PDO).
   - Tangani transaksi DB (BEGIN/COMMIT/ROLLBACK) saat menyimpan transaksi dan mengurangi stok.

3. Validasi Input
   - Sanitasi semua input (server-side) — jangan hanya mengandalkan validasi client-side.
   - Validasi tipe data (angka, tanggal) dan batas string.

4. Cross Site Scripting (XSS) & CSRF
   - Escape output di HTML (htmlspecialchars).
   - Gunakan token CSRF pada form yang melakukan perubahan (POST).

5. Error Handling
   - Jangan tampilkan pesan error DB mentah ke pengguna. Log error ke file log yang aman.

6. File Access & Konfigurasi
   - Jangan commit kredensial sensitif ke repository publik.
   - Pastikan config.php tidak dapat diakses dari web root jika berisi info sensitif; gunakan .env atau simpan di luar webroot bila memungkinkan.

---

## 9. Troubleshooting umum
- Halaman blank atau 500 error: aktifkan display_errors sementara (development) atau cek error_log untuk detail.
- Koneksi DB gagal: periksa config.php (host/user/password/dbname) dan periksa hak user DB.
- Session tidak persisten: cek folder session PHP dan izin menulis.
- File JavaScript tidak bekerja: buka devtools (Console) untuk melihat error JS atau request gagal (Network tab).

---

## 10. Cara berkontribusi & kontak
- Jika ingin berkontribusi: fork repo, buat branch feature/<deskripsi>, buat perubahan, dan ajukan Pull Request dengan penjelasan perubahan.
- Sertakan unit test atau minimal penjelasan manual testing untuk fitur baru.
- Document setiap perubahan API/endpoint di README atau CHANGELOG.

---

Penutup singkat  
Dokumentasi ini memberikan peta untuk memahami proyek berdasarkan struktur file dan nama-nama modul yang ada. Untuk membuat dokumentasi yang lebih rinci (mis. diagram entity-relationship DB aktual, contoh permintaan/respon JSON, atau panduan instalasi step-by-step yang memuat SQL dump), saya butuh akses ke isi file implementasi (kode di setiap .php / .js) atau file SQL schema. Jika Anda ingin, saya dapat menghasilkan dokumen yang lebih terperinci dengan meninjau isi file-file tersebut.
