# Jobsheet 3 (Versi Bootstrap) — Responsive Design dengan Framework

Sub-CPMK: Membangun tampilan responsif memakai framework CSS (Bootstrap 5).

Versi ini adalah alternatif dari [jobsheet-03](../jobsheet-03/README.md)
(CSS murni) — halaman dan fungsionalitasnya identik, tapi seluruh tata
letak dibangun memakai Bootstrap 5 (dimuat via CDN) alih-alih menulis
CSS dari nol.

## Perubahan dari Jobsheet 03 (CSS Murni)
- Bootstrap 5.3 dimuat via CDN (`bootstrap.min.css` + `bootstrap.bundle.min.js`).
- Navbar: hamburger memakai komponen `.navbar`/`.navbar-toggler`/`.collapse` bawaan Bootstrap (butuh JavaScript), menggantikan checkbox hack murni CSS.
- Kartu statistik: grid CSS manual diganti sistem grid 12 kolom Bootstrap (`.row`/`.col-12 .col-sm-6 .col-md-4`) — progresi 3 tingkat: 1 kolom → 2 kolom → 3 kolom.
- Section dibungkus komponen `.card` menggantikan styling `<section>` custom.
- Tabel & form memakai utility class Bootstrap (`.table-striped`, `.table-hover`, `.form-control`, `.btn-primary`, dst).
- Komponen baru: `.badge` untuk status stok, `.alert` untuk pesan sukses.
- **0 atribut `style=` manual** — semua inline style diganti dengan class Bootstrap (`.bg-primary`, `.text-primary`, `.btn-primary`, `.bg-light`).
- **0 baris override warna brand** tersisa di `assets/css/style.css` — CSS aktif yang ada hanya media query orientation (dari tugas mandiri jobsheet ini, lihat [cheatsheet §4.6](cheatsheet.md)).

## Cara menjalankan
Buka `index.html` di browser (butuh koneksi internet karena Bootstrap dimuat dari CDN), uji dengan DevTools responsive mode pada breakpoint Bootstrap:
- **xs** (< 576px): navbar terlipat, kartu 1 kolom
- **sm** (≥ 576px): kartu 2 kolom
- **md** (≥ 768px): navbar horizontal, kartu 3 kolom

## Dokumentasi
Penjelasan lengkap tiap perubahan ada di folder [Dokumentasi/](Dokumentasi/README.md), termasuk tabel perbandingan class Bootstrap vs CSS murni.

## Ukuran Banding
| Metrik | Jobsheet-03 (CSS Murni) | Jobsheet-03 (Bootstrap CDN) |
|---|---|---|
| `style.css` | 311 baris / 6.6 KB | 83 baris / 2.5 KB |
| CSS aktif | ~196 baris | ~32 baris (hanya orientation) |
| CDN Bootstrap | — | ~23 KB CSS + ~79 KB JS |
| Total unduhan | ~6.6 KB | ~105 KB |

Bootstrap menambah ~98 KB overhead, tapi menghemat ~164 baris CSS aktif dan waktu pengerjaan yang signifikan.
