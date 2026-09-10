# 6. Rangkuman & Perbandingan dengan CSS Murni

## 6.1 Rangkuman Keseluruhan

| Bagian | Lokasi Kode | Konsep yang Dipelajari |
|---|---|---|
| [Konsep Dasar Bootstrap](01-konsep-dasar-bootstrap.md) | `<link>`/`<script>` CDN | Framework CSS, utility-first, CDN, breakpoint bawaan |
| [Perubahan HTML](02-perubahan-file-html.md) | Semua file `.html` | `.container`, `.card`, pola pembungkus form `.mb-3` |
| [Navbar Responsif](03-navbar-responsive-bootstrap.md) | `.navbar`, `.navbar-toggler`, `.collapse` | Komponen berbasis JavaScript, atribut `data-bs-*`, aksesibilitas (`aria-*`) |
| [Grid & Card](04-grid-dan-card.md) | `.row`, `.col-*`, `.card` | Grid 12 kolom, pendekatan mobile-first Bootstrap |
| [Tabel & Form](05-tabel-dan-form-bootstrap.md) | `.table-*`, `.btn-*`, `.form-control` | Utility class semantik (warna berdasarkan makna), styling form tanpa CSS custom |

## 6.2 Tabel Perbandingan Class Bootstrap vs CSS Murni

Ringkasan seluruh pemetaan yang sudah dibahas di bab-bab sebelumnya:

| Kebutuhan | CSS Murni (jobsheet-03 asli) | Bootstrap (jobsheet ini) |
|---|---|---|
| Membatasi & menengahkan lebar konten | `main { max-width: 1000px; margin: 0 auto; }` | `.container` |
| Kartu putih dengan bayangan | `section { border-radius: 8px; box-shadow: ...; }` | `.card` + `.shadow-sm` |
| Grid 3 kolom kartu statistik | `display: grid; grid-template-columns: repeat(3, 1fr);` | `.row` + `.col-md-4` |
| Navbar hamburger | Checkbox hack (`:checked` + `~`) | `.navbar-toggler` + `.collapse` (JS) |
| Tabel belang & hover | `nth-child(even)`, `:hover` | `.table-striped`, `.table-hover` |
| Tabel scroll horizontal | `.table-responsive { overflow-x: auto; }` (custom) | `.table-responsive` (bawaan, nama sama) |
| Tombol warna | `td button:first-of-type { background: ...; }` | `.btn-warning`, `.btn-danger` |
| Input & select form | `form input { width:100%; padding:...; border:...; }` | `.form-control`, `.form-select` |
| Breakpoint tablet/mobile | `@media (max-width: 768px)` / `(max-width: 480px)` custom | Infix bawaan: `sm`, `md`, `lg`, `xl`, `xxl` |
| Baris CSS custom dibutuhkan | ~240 baris | ~5 baris aktif (sisanya komentar) |

## 6.3 Hasil Latihan 6.4 — Penerapan Class Bootstrap Penuh

Latihan di bab 6.4 sudah dikerjakan seluruhnya. Berikut ringkasan
perubahan dan hasil pengamatannya:

### 1. Ganti warna brand ke tema bawaan Bootstrap

**Yang dilakukan:**
- Dihapus semua `style="background-color:#1d5b8a;"` (navbar, thead, tombol Detail/Simpan)
- Dihapus semua `style="color:#1d5b8a;"` (judul section, angka statistik)
- Dihapus semua `style="background-color:#eef4fa;"` (kartu statistik)
- Diganti dengan class bawaan Bootstrap:

| Inline style (SEBELUM) | Class Bootstrap (SESUDAH) |
|---|---|
| `style="background-color:#1d5b8a;"` di navbar | `.bg-primary` |
| `style="color:#1d5b8a;"` di judul | `.text-primary` |
| `style="background-color:#1d5b8a;"` di thead | `.bg-primary` |
| `style="background-color:#1d5b8a; color:#fff;"` di tombol | `.btn-primary` |
| `style="background-color:#eef4fa;"` di kartu statistik | `.bg-light` |

**Hasil:**
- File `style.css` sekarang berisi **0 baris CSS aktif** (hanya komentar). Semua
  override yang sebelumnya diperlukan untuk memaksa warna brand (`!important`
  untuk `.nav-link`, `.nav-link:hover`, dan `button[type="submit"]:hover`)
  **tidak lagi diperlukan** karena Bootstrap sudah menangani warna secara otomatis
  melalui class `.navbar-dark`, `.btn-primary`, `.text-primary`.
- Jumlah atribut `style=` di seluruh file HTML: **0** (sebelumnya ada ~15)

### 2. Tambah breakpoint ketiga: `col-sm-6`

**Yang dilakukan:**
Ditambahkan `col-sm-6` di antara `col-12` dan `col-md-4` pada kartu statistik
di `index.html`:

```html
<!-- SEBELUM (2 tingkat) -->
<div class="col-12 col-md-3">

<!-- SESUDAH (3 tingkat) -->
<div class="col-12 col-sm-6 col-md-4">
```

**Hasil pengamatan:**
| Lebar layar | Breakpoint | Jumlah kolom | Lebar per kartu |
|---|---|---|---|
| < 576px | xs (default) | 1 kolom | 100% layar |
| ≥ 576px | `sm` | 2 kolom | ~50% layar |
| ≥ 768px | `md` | 3 kolom | ~33% layar |

Progresi 3 tingkat ini **identik** dengan yang dibangun secara manual di
[jobsheet-02 CSS murni](../../jobsheet-02/Dokumentasi/05-css-media-query-breakpoint.md)
menggunakan `@media (max-width: 768px)` dan `@media (max-width: 480px)` —
bedanya hanya 3 class Bootstrap vs ~20 baris CSS manual.

### 3. Ganti breakpoint navbar: `.navbar-expand-lg` → `.navbar-expand-md`

**Yang dilakukan:**
Semua 5 file HTML mengganti class navbar:

```html
<!-- SEBELUM -->
<header class="navbar navbar-expand-lg navbar-dark" style="background-color:#1d5b8a;">

<!-- SESUDAH -->
<header class="navbar navbar-expand-md navbar-dark bg-primary">
```

**Hasil pengamatan:**
| Breakpoint | Navbar horizontal | Navbar hamburger (terlipat) |
|---|---|---|
| `.navbar-expand-lg` (SEBELUM) | ≥ 992px | < 992px |
| `.navbar-expand-md` (SESUDAH) | ≥ 768px | < 768px |

Navbar mulai "terlipat" jadi hamburger di lebar layar **di bawah 768px**
(bukan 992px seperti sebelumnya). **Tidak ada CSS tambahan yang ditulis**
— perubahan ini murni mengganti nama class.

### 4. Komponen Bootstrap baru: `.badge` dan `.alert`

**`.badge` — Status Stok di `buku/list.html`:**

```html
<!-- SEBELUM: angka biasa -->
<td>4</td>

<!-- SESUDAH: badge dengan label status -->
<td>
    <span class="badge bg-success">Tersedia</span>
    <small class="text-muted d-block">(4)</small>
</td>
<!-- Stok 0: -->
<td>
    <span class="badge bg-danger">Kosong</span>
    <small class="text-muted d-block">(0)</small>
</td>
```

Komponen `.badge` menandai status secara visual:
- `badge bg-success` (hijau) = buku tersedia (stok > 0)
- `badge bg-danger` (merah) = buku kosong (stok = 0)

**`.alert` — Pesan Sukses di Form (`buku/tambah.html`, `anggota/tambah.html`):**

```html
<div class="alert alert-success alert-dismissible fade show" role="alert">
    <strong>Berhasil!</strong> Buku baru telah ditambahkan ke sistem.
    <button type="button" class="btn-close" data-bs-dismiss="alert"
            aria-label="Close"></button>
</div>
```

Komponen `.alert` menampilkan pesan kontekstual:
- `alert-success` (hijau) = pesan sukses
- `alert-info` (biru) = informasi umum
- `.alert-dismissible` + `.btn-close` + `data-bs-dismiss` = bisa ditutup user
- `.fade .show` = animasi fade-in saat muncul

### 5. Bandingkan ukuran file

| Metrik | Jobsheet-02 (CSS Murni) | Jobsheet-03 (Bootstrap CDN) |
|---|---|---|
| File `style.css` | 240 baris / **4.8 KB** | 32 baris / **1.2 KB** (hanya komentar) |
| Baris CSS aktif | ~240 baris | ~5 baris |
| `index.html` | 74 baris | 172 baris (lebih panjang karena komponen Bootstrap) |
| CDN yang dimuat | Tidak ada | `bootstrap.min.css` ~23 KB + `bootstrap.bundle.min.js` ~79 KB |
| **Total ukuran unduhan** | ~4.8 KB (1 file) | **~103 KB** (style.css + CDN CSS + CDN JS) |

**Trade-off ukuran unduhan vs kecepatan pengembangan:**

| Aspek | CSS Murni | Bootstrap CDN |
|---|---|---|
| Ukuran unduhan pertama | Sangat kecil (~5 KB) | Lebih besar (~103 KB) |
| Kecepatan pengerjaan | Lambat — setiap komponen ditulis manual dari nol | **Sangat cepat** — komponen tinggal pakai class |
| Konsistensi | Bergantung skill developer | Konsisten (framework sudah standardisasi) |
| Responsif | Harus tulis `@media query` manual | **Otomatis** — class infix `sm`/`md`/`lg` sudah built-in |
| Maintenance | Banyak CSS yang harus dipelihara | Sangat sedikit CSS custom (~5 baris) |
| CDN caching | Hanya file sendiri | **Efisien** — CDN di-cache browser, subsequent load sangat cepat |
| Offline | Berfungsi tanpa internet | **Butuh internet** (pertama kali) |

**Kesimpulan:** Bootstrap CDN menambah ~98 KB overhead, tapi **menghemat
~235 baris CSS** dan waktu pengerjaan yang signifikan. Untuk aplikasi
perpustakaan skala kecil seperti SIMPUS-Mini, trade-off ini **sangat
menguntungkan** — biaya unduhan sekali dibayar oleh browser cache,
sedangkan efisiensi pengerjaan dirasakan terus-menerus.

## 6.4 Kapan Tetap Perlu CSS Custom?

**Setelah migrasi penuh ke class Bootstrap**, file `assets/css/style.css`
sekarang **hanya berisi komentar** (0 baris CSS aktif). Namun dalam
proyek nyata yang lebih besar, masih ada beberapa kasus di mana CSS
custom tetap diperlukan:

1. **Warna brand yang sangat spesifik** — misalnya harus tepat `#1d5b8a`
   (bukan `#0d6efd` default Bootstrap). Bisa diatasi dengan mengganti
   variabel Sass Bootstrap atau menambah sedikit override.
2. **Komponen khusus** yang tidak tersedia di Bootstrap (misal: chart
   interaktif, animasi khusus, tooltip custom).
3. **Tweaking spacing/typography** — margin, padding, atau font yang
   tidak cocok dengan class utility Bootstrap.

Untuk proyek SIMPUS-Mini saat ini, CSS custom **tidak diperlukan lagi** —
semua kecukupan ditangani oleh class bawaan Bootstrap 5.3.

## 6.5 Ide Latihan Tambahan (Opsional)

Semua latihan di bawah ini sudah **dikerjakan** dan hasilnya didokumentasikan
di bagian 6.3 di atas:

1. ✅ **Ganti warna brand ke tema bawaan Bootstrap** — semua inline style
   dihapus, diganti `.bg-primary`/`.text-primary`/`.btn-primary`/`.bg-light`.
2. ✅ **Tambah breakpoint ketiga** — `col-sm-6` ditambahkan untuk progresi
   3 tingkat: 1 kolom → 2 kolom → 3 kolom.
3. ✅ **Ganti breakpoint navbar** — `.navbar-expand-lg` → `.navbar-expand-md`,
   hamburger muncul di < 768px.
4. ✅ **Tambah komponen baru** — `.badge` (status stok) dan `.alert`
   (pesan sukses) ditambahkan.
5. ✅ **Bandingkan ukuran file** — tabel perbandingan ada di bagian 6.3.5.

Kalau ada bagian yang masih membingungkan, terutama soal grid 12 kolom
atau atribut `data-bs-*`, coba baca ulang
[bab 3](03-navbar-responsive-bootstrap.md) dan
[bab 4](04-grid-dan-card.md) sambil membuka
[dokumentasi resmi Bootstrap 5.3](https://getbootstrap.com/docs/5.3/getting-started/introduction/)
di tab browser terpisah untuk menjelajahi komponen lain yang tersedia.

<!-- 
==============================================
CATATAN ISOLASI (SAFE ISOLATION)
==============================================
Dokumen ini adalah bagian terakhir dari seri
Dokumentasi Jobsheet 3 (Bootstrap).

Jangan mengedit bagian ini secara langsung
tanpa memperbarui bab 1-5 terlebih dahulu.

Relasi:
- bab 1: konsep dasar Bootstrap
- bab 2: perubahan file HTML
- bab 3: navbar responsif
- bab 4: grid & card
- bab 5: tabel & form
- bab 6: rangkuman ini

Terakhir diperbarui: 2026-09-10
==============================================
-->
