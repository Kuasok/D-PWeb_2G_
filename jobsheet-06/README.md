# Jobsheet 6 — Fetch API & JSON

Sub-CPMK: Menerapkan komunikasi asinkron (AJAX/fetch, JSON).

## Perubahan dari Jobsheet 5

- Tambah `data/buku.json` (10 objek) dan `data/anggota.json` (4 objek) sebagai pengganti sementara API sungguhan.
- `buku/list.html` & `anggota/list.html`: `<tbody>` dikosongkan, baris kini dirender dinamis oleh `assets/js/buku.js` / `assets/js/anggota.js` menggunakan `fetch` + `async/await`.
- Loading indicator (`#loading-indicator`) tampil selama proses fetch (disimulasikan dengan delay 600ms).
- Penanganan error (`try/catch`) menampilkan pesan di dalam tabel bila fetch gagal.
- `app.js`: `initHapusConfirm` diubah ke **event delegation** (`document.addEventListener("click", ...)`) karena tombol Hapus sekarang berada di baris yang dibuat setelah halaman selesai dimuat.

## Struktur Folder

```
jobsheet-06/
├── index.html              # Beranda (hero + kartu statistik)
├── assets/
│   ├── css/
│   │   └── style.css       # CSS responsif dengan media queries
│   └── js/
│       ├── app.js          # JavaScript DOM & event handling (event delegation)
│       ├── buku.js         # Fetch daftar buku dari JSON
│       └── anggota.js      # Fetch daftar anggota dari JSON
├── data/
│   ├── buku.json           # Data buku (10 objek)
│   └── anggota.json        # Data anggota (4 objek)
├── buku/
│   ├── list.html           # Daftar buku (render dinamis dari JSON)
│   └── tambah.html         # Form tambah buku (validasi client-side)
├── anggota/
│   ├── list.html           # Daftar anggota (render dinamis dari JSON)
│   └── tambah.html         # Form tambah anggota (validasi client-side)
└── README.md               # Dokumentasi ini
```

## Cara menjalankan

**Penting:** `fetch()` ke file lokal akan diblokir kebijakan CORS jika dibuka langsung dengan `file://`. Jalankan lewat server lokal, misalnya:

```bash
php -S localhost:8000
```

lalu buka `http://localhost:8000/index.html`. Bisa juga memakai ekstensi "Live Server" di VSCode.

## Catatan

- Uji error handling dengan mengganti sementara nama file di `fetch(...)` menjadi nama yang salah.
- Pola `fetch` + `async/await` di sini akan dipakai ulang untuk memanggil endpoint PHP sungguhan mulai Jobsheet 9 (setelah back-end PostgreSQL siap di Jobsheet 8), meskipun mulai Jobsheet 7 rendering utama berpindah ke server-side PHP.
