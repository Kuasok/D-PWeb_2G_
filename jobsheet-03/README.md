# Jobsheet 3 — Responsive Design

Sub-CPMK: Membangun tampilan responsif.

## Perubahan dari Jobsheet 2

- Tambah `<meta name="viewport">` di semua halaman.
- Navbar: hamburger menu memakai teknik **checkbox hack** murni CSS (`input[type=checkbox] + label`), aktif di layar ≤480px.
- Tabel dibungkus `<div class="table-responsive">` agar bisa di-scroll horizontal di layar sempit.
- Tambah media query di `style.css`: grid kartu statistik 3 → 2 → 1 kolom mengikuti breakpoint tablet/mobile.

## Cara menjalankan

Buka `index.html` di browser, uji dengan DevTools responsive mode pada 3 breakpoint:

- **Mobile** ≤480px: hamburger menu aktif, grid 1 kolom, form full width
- **Tablet** ~768px: navbar horizontal, grid 2 kolom
- **Desktop** ≥1024px: navbar horizontal, grid 3 kolom

## Struktur Folder

```
jobsheet-03/
├── index.html              # Beranda (hero + kartu statistik)
├── assets/
│   └── css/
│       └── style.css       # CSS responsif dengan media queries
├── buku/
│   ├── list.html           # Daftar buku (tabel + table-responsive)
│   └── tambah.html         # Form tambah buku
├── anggota/
│   ├── list.html           # Daftar anggota (tabel + table-responsive)
│   └── tambah.html         # Form tambah anggota
└── README.md               # Dokumentasi ini
```

## Catatan

- Hamburger di jobsheet ini masih murni CSS (checkbox hack). Di Jobsheet 5 akan diganti dengan toggle berbasis JavaScript.
- Responsive breakpoints:
  - `≤480px` (mobile): hamburger menu, 1 kolom grid
  - `≤768px` (tablet): 2 kolom grid
  - `>768px` (desktop): 3 kolom grid, navbar horizontal
