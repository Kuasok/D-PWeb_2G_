# Jobsheet 5 — JavaScript DOM & Event

Sub-CPMK: Menerapkan manipulasi DOM & event JavaScript.

## Perubahan dari Jobsheet 4

- Tambah `assets/js/app.js`.
- Hamburger menu: checkbox hack (CSS) diganti tombol + JS (`nav.classList.toggle("nav-open")`).
- Form Tambah Buku & Tambah Anggota: validasi client-side (`initValidasiForm`) — field wajib, rentang tahun, stok non-negatif — pesan error tampil inline via manipulasi DOM (`insertAdjacentElement`).
- Tabel Daftar Buku & Daftar Anggota: kolom pencarian real-time (`initTableFilter`) yang menyaring baris via `keyup`.
- Tombol Hapus (`.btn-hapus`): menampilkan `confirm()` lalu menghapus baris dari tampilan (masih front-end saja, belum ke server).

## Cara menjalankan

Buka `index.html` di browser. Coba: submit form kosong (muncul error), ketik di kolom cari (tabel tersaring), klik Hapus (muncul konfirmasi).

## Struktur Folder

```
jobsheet-05/
├── index.html              # Beranda (hero + kartu statistik)
├── assets/
│   ├── css/
│   │   └── style.css       # CSS responsif dengan media queries
│   └── js/
│       └── app.js          # JavaScript DOM & event handling
├── buku/
│   ├── list.html           # Daftar buku (tabel + pencarian + hapus)
│   └── tambah.html         # Form tambah buku (validasi client-side)
├── anggota/
│   ├── list.html           # Daftar anggota (tabel + pencarian + hapus)
│   └── tambah.html         # Form tambah anggota (validasi client-side)
└── README.md               # Dokumentasi ini
```

## Catatan

- Validasi di sini murni client-side dan bisa dilewati (nonaktifkan JS). Validasi server-side ditambahkan di Jobsheet 7 sebagai lapisan kedua yang wajib.
- Hapus baris di jobsheet ini hanya menghilangkan dari tampilan (belum persisten) — akan diganti proses hapus sungguhan ke database mulai Jobsheet 9.
