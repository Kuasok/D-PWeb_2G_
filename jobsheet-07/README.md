# Jobsheet 7 — PHP Dasar & Form Handling

Sub-CPMK: Mengimplementasikan dasar PHP & pengolahan form.

## Perubahan dari Jobsheet 6

- Semua halaman `.html` diubah menjadi `.php`.
- Tambah `includes/header.php` & `includes/footer.php` — navbar/footer cukup ditulis sekali lalu dipanggil lewat `include`, tidak ada lagi duplikasi di tiap halaman.
- Path CSS/JS/menu memakai path **relatif** (`assets/css/style.css`, `index.php`, dst, tanpa awalan `/`), dihitung otomatis di `includes/header.php` berdasarkan kedalaman folder halaman yang sedang diakses (`$base` = `""` di root, `"../"` untuk halaman satu level ke dalam seperti `buku/`, `anggota/`). Jadi proyek tetap benar walau diakses dari root server (`php -S`) maupun lewat subfolder (mis. XAMPP dengan document root di `htdocs` induk).
- `buku/tambah.php` & `anggota/tambah.php`: form kini `method="post"` mengarah ke `proses_tambah.php` masing-masing.
- `buku/proses_tambah.php` & `anggota/proses_tambah.php`: memvalidasi `$_POST` di server (validasi ini **terpisah** dari validasi JS di Jobsheet 5 — bisa berjalan sendiri walau JS dimatikan), lalu menyimpan sementara ke `$_SESSION['buku']` / `$_SESSION['anggota']` (array), redirect ke `list.php`.
- `buku/list.php` & `anggota/list.php`: tabel dirender dari `$_SESSION` via `foreach` (menggantikan pendekatan fetch/JSON di Jobsheet 6 — rendering utama sekarang di server-side PHP).
- Flash message sukses/gagal ditampilkan lewat `$_SESSION['flash']` (sekali tampil, lalu di-`unset`), dengan style baru `.flash` / `.flash-success` / `.flash-error` di `style.css`.
- Kartu statistik beranda kini `count($_SESSION[...])` — bukan angka dummy 12/8 lagi.
- File `assets/js/buku.js`, `assets/js/anggota.js`, dan folder `data/` dari Jobsheet 6 **dihapus** karena rendering sudah dipindah ke server-side PHP.

## Struktur Folder

```
jobsheet-07/
├── index.php               # Beranda (ringkasan dihitung dari session)
├── includes/
│   ├── header.php          # session_start + hitung $base + <head> + navbar + buka <main>
│   └── footer.php          # tutup <main> + footer + muat app.js (+ $extra_scripts)
├── assets/
│   ├── css/
│   │   └── style.css       # CSS responsif + flash message
│   └── js/
│       └── app.js          # DOM & event (hamburger, filter, hapus, validasi form)
├── buku/
│   ├── list.php            # Daftar buku dari $_SESSION + flash
│   ├── tambah.php          # Form tambah buku (method=post → proses_tambah.php)
│   └── proses_tambah.php   # Validasi server + simpan ke $_SESSION + redirect
├── anggota/
│   ├── list.php            # Daftar anggota dari $_SESSION + flash
│   ├── tambah.php          # Form tambah anggota (method=post → proses_tambah.php)
│   └── proses_tambah.php   # Validasi server + simpan ke $_SESSION + redirect
├── docs/
│   └── wireframe.md        # Wireframe teks + user flow fitur mendatang
├── Dokumentasi/            # Dokumentasi bab 1-7 jobsheet ini
└── README.md               # Dokumentasi ini
```

## Cara menjalankan

Halaman PHP harus diproses server — membuka lewat `file://` tidak akan mengeksekusi kode PHP.

**Opsi 0 — VS Code (satu tekan F5)** — paling praktis; semua langkah di opsi 1
otomatis. Workspace ini punya `.vscode/tasks.json` + `.vscode/launch.json` dengan
**satu server terpadu**: `php -S 127.0.0.1:8080` ber-document-root root repo,
jadi semua jobsheet — termasuk yang belum dibuat — tersaji dari satu port.

1. Buka panel **Run and Debug** (`Ctrl+Shift+D`), pilih konfigurasi
   **`▶ Jobsheet-07 — Buka di Brave`**, tekan **F5**.
   - `preLaunchTask` menyalakan server terpadu lebih dulu dan **menunggu sampai
     server benar-benar siap** (problemMatcher background), baru Brave membuka
     `http://127.0.0.1:8080/jobsheet-07/index.php`.
   - Task server **idempotent**: kalau sudah jalan, F5 tidak mengulang bind
     port — langsung dipakai ulang. Jadi F5 boleh ditekan berkali-kali.
2. Jobsheet lain tinggal pilih di dropdown: **`▶ Jobsheet-08 — Buka di Brave`**,
   atau untuk **jobsheet mendatang** (09, 10, ...) pilih
   **`▶ Jobsheet (lainnya) — pilih halaman`** lalu isi prompt
   (mis. `jobsheet-09/index.php`) — **tanpa mengedit konfigurasi apa pun**.
3. Mau sekalian pasang breakpoint di file `.php`? Pilih compound
   **`▶▶ Jobsheet-07 — Brave + Xdebug`** atau **`▶▶ Jobsheet-08 — Brave
   + Xdebug`** (listener Xdebug port 9003 — sudah aktif di `php.ini` XAMPP,
   mode `debug`, `start_with_request=yes`). Shift+F5 = Stop All (`stopAll`).
4. Task pendukung: `Ctrl+Shift+P` → **Tasks: Run Task** →
   - `▶ Buka Brave (pilih jobsheet)` — buka tanpa debugger; server dinyalakan
     otomatis via `dependsOn`, lalu isi prompt nama halaman.
   - `⏹ Stop server (:8080)` — matikan server terpadu.
   - `✔ Lint semua file PHP (php -l)` — lint semua file PHP di folder
     `jobsheet-*`; jobsheet baru otomatis ikut tanpa diedit.

> Catatan: alur VS Code memakai **port 8080** — satu server untuk semua
> jobsheet; port 8000/8007/8008 tetap bebas untuk cara manual di bawah.

**Opsi 1 — PHP built-in server**, jalankan dari dalam folder `jobsheet-07/`:

```bash
php -S localhost:8000
```

Bila `php` belum ada di PATH (default Linux), pakai PHP bawaan XAMPP:

```bash
/opt/lampp/bin/php -S localhost:8000
```

lalu buka `http://localhost:8000/index.php`.

**Opsi 2 — XAMPP (Apache)**: arahkan document root (atau salin folder) ke `htdocs`, mis. `http://localhost/jobsheet-07/`, atau pakai virtual host. Semua path sudah relatif otomatis (lihat catatan `$base` di atas), jadi halaman tetap benar walau diakses dari subfolder.

## Catatan

- Data yang disimpan di `$_SESSION` akan hilang saat sesi browser berakhir — ini jembatan sementara. Mulai Jobsheet 8, penyimpanan dipindah ke PostgreSQL agar persisten.
- Coba nonaktifkan JavaScript di browser lalu submit form kosong: validasi server tetap mencegah data invalid tersimpan.
