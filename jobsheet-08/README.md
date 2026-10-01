# Jobsheet 8 — Koneksi PostgreSQL

Sub-CPMK: Menghubungkan aplikasi dengan basis data PostgreSQL.

## Perubahan dari Jobsheet 7

- Tambah `sql/01_buku_anggota.sql` — DDL tabel `buku` dan `anggota` (ERD dasar).
- Tambah `includes/koneksi.php` — koneksi `PDO` driver `pgsql`.
- `buku/proses_tambah.php` & `anggota/proses_tambah.php`: `$_SESSION['buku'][] = ...` (Jobsheet 7) diganti `INSERT ... RETURNING id` via prepared statement.
- `buku/list.php` & `anggota/list.php`: sumber data diganti dari `$_SESSION` menjadi `SELECT * FROM ... ORDER BY id DESC`.
- `index.php`: kartu statistik Total Buku/Anggota kini `SELECT COUNT(*)` dari database (bukan dummy/session lagi).

## Struktur Folder

```
jobsheet-08/
├── index.php               # Beranda (ringkasan via SELECT COUNT(*))
├── includes/
│   ├── header.php          # session_start + hitung $base + <head> + navbar + buka <main>
│   ├── footer.php          # tutup <main> + footer + muat app.js (+ $extra_scripts)
│   └── koneksi.php         # Koneksi PDO ke PostgreSQL (driver pgsql)
├── assets/
│   ├── css/
│   │   └── style.css       # CSS responsif + flash message
│   └── js/
│       └── app.js          # DOM & event (hamburger, filter, hapus, validasi form)
├── sql/
│   └── 01_buku_anggota.sql # DDL tabel buku & anggota (SERIAL id, UNIQUE no_anggota)
├── buku/
│   ├── list.php            # SELECT * FROM buku ORDER BY id DESC + flash
│   ├── tambah.php          # Form tambah buku (method=post → proses_tambah.php)
│   └── proses_tambah.php   # Validasi server + INSERT prepared statement (RETURNING id)
├── anggota/
│   ├── list.php            # SELECT * FROM anggota ORDER BY id DESC + flash
│   ├── tambah.php          # Form tambah anggota (method=post → proses_tambah.php)
│   └── proses_tambah.php   # Validasi server + INSERT prepared statement (RETURNING id)
├── docs/
│   └── wireframe.md        # Wireframe teks + user flow fitur mendatang
├── Dokumentasi/            # Dokumentasi bab 1-8 jobsheet ini
└── README.md               # Dokumentasi ini
```

## Persiapan database

1. Pastikan PostgreSQL berjalan dan ekstensi PHP `pdo_pgsql` aktif:
   ```bash
   php -m | grep pgsql
   ```
   Di XAMPP biasanya sudah tersedia; bila belum, aktifkan `extension=pdo_pgsql` di `php.ini` lalu restart Apache.
2. Buat database:
   ```bash
   createdb simpus_mini
   ```
   (bila lewat TCP seperti pada environment lokal: `PGPASSWORD=postgres createdb -U postgres -h localhost simpus_mini`)
3. Jalankan skema:
   ```bash
   psql -d simpus_mini -f sql/01_buku_anggota.sql
   ```
4. Sesuaikan kredensial di `includes/koneksi.php` (`$user`, `$pass`) dengan environment lokal.

## Cara menjalankan

**Opsi 0 — VS Code (satu tekan F5)** — alur yang sama untuk semua jobsheet
(lihat README jobsheet-07 bagian "Cara menjalankan"):

1. Prasyarat khusus jobsheet ini: PostgreSQL berjalan dan DB `simpus_mini`
   sudah dibuat (bagian **Persiapan database** di atas). Bila belum, halaman
   akan tampil `Koneksi database gagal: ...`.
2. Panel **Run and Debug** (`Ctrl+Shift+D`) → pilih
   **`▶ Jobsheet-08 — Buka di Brave`** → **F5**. Server terpadu
   `127.0.0.1:8080` dinyalakan otomatis, lalu Brave membuka
   `http://127.0.0.1:8080/jobsheet-08/index.php`.
3. Debug query/PHP dengan breakpoint: compound
   **`▶▶ Jobsheet-08 — Brave + Xdebug`** (Shift+F5 = Stop All).
4. Jobsheet berikutnya tanpa edit konfigurasi: config
   **`▶ Jobsheet (lainnya) — pilih halaman`** → isi prompt (mis.
   `jobsheet-09/index.php`).

**Opsi 1 — PHP built-in server**, jalankan dari dalam folder `jobsheet-08/`:

```bash
php -S localhost:8000
```

Bila `php` belum ada di PATH (default Linux), pakai PHP bawaan XAMPP:

```bash
/opt/lampp/bin/php -S localhost:8000
```

lalu buka `http://localhost:8000/index.php`.

**Opsi 2 — XAMPP (Apache)**: arahkan document root (atau salin folder) ke `htdocs`, mis. `http://localhost/jobsheet-08/`, atau pakai virtual host — path CSS/JS/link sudah relatif otomatis (lihat `includes/header.php`), jadi semuanya jalan.

## Catatan

- Data yang diinput sekarang **persisten** — coba tutup-buka browser, data tetap ada (beda dengan Jobsheet 7 yang hilang saat sesi berakhir).
- Query memakai prepared statement (`:nama_parameter`) — bukan concatenation string — sebagai fondasi keamanan yang diperdalam di Jobsheet 11.
- Kolom `id` sudah ikut ter-fetch dari `SELECT *` meski belum dipakai di tampilan — akan digunakan untuk link Edit/Hapus mulai Jobsheet 9.
