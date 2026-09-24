# Jobsheet 4 — UI/UX Design

Sub-CPMK: Merancang UI/UX aplikasi (proyek).

## Perubahan dari Jobsheet 3

- Tidak ada perubahan kode — halaman HTML/CSS tetap sama persis dengan Jobsheet 3.
- Tambah `docs/wireframe.md`: wireframe teks + user flow untuk fitur yang **belum dibangun** (Login, Dashboard Petugas, Peminjaman, Pengembalian, Riwayat).
- Tambah `Infografis.png` — infografis ringkas jobsheet ini.

## Cara menjalankan

Sama seperti Jobsheet 3 — buka `index.html`.

## Struktur Folder

```
jobsheet-04/
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
├── docs/
│   └── wireframe.md        # Wireframe teks + user flow fitur mendatang
├── Dokumentasi/            # Dokumentasi bab 1-6 jobsheet ini
├── Infografis.png          # Infografis jobsheet
└── README.md               # Dokumentasi ini
```

## Catatan

Dokumen `docs/wireframe.md` menjadi acuan struktur HTML baru yang mulai diimplementasikan pada Jobsheet 5 dan seterusnya (interaktivitas JS, lalu PHP/PostgreSQL untuk fitur Login & Peminjaman).
