# Cheatsheet Lengkap Jobsheet 03 — SIMPUS-Mini (Bootstrap 5)

> Dokumen ini menjelaskan **seluruh kode** di jobsheet-03: struktur file,
> alur navigasi, setiap baris HTML/CSS, class Bootstrap yang dipakai,
> logika responsive, dan cara kerja komponen.

---

## 1. Struktur Folder

```
jobsheet-03/
├── index.html                 ← Beranda (halaman utama)
├── assets/
│   └── css/
│       └── style.css          ← CSS custom (override orientation)
├── buku/
│   ├── list.html              ← Daftar Buku (tabel)
│   └── tambah.html            ← Form Tambah Buku
├── anggota/
│   ├── list.html              ← Daftar Anggota (tabel)
│   └── tambah.html            ← Form Tambah Anggota
├── Dokumentasi/               ← Dokumentasi bab 1-6
│   ├── README.md
│   ├── 01-konsep-dasar-bootstrap.md
│   ├── 02-perubahan-file-html.md
│   ├── 03-navbar-responsive-bootstrap.md
│   ├── 04-grid-dan-card.md
│   ├── 05-tabel-dan-form-bootstrap.md
│   └── 06-rangkuman-dan-perbandingan.md
├── cheatsheet.md              ← Dokumen ini
└── README.md
```

---

## 2. Alur Navigasi (Sitemap)

```
index.html (Beranda)
├──→ buku/list.html (Daftar Buku)
│       └──→ buku/tambah.html (Tambah Buku)
├──→ buku/tambah.html (Tambah Buku)
├──→ anggota/list.html (Daftar Anggota)
│       └──→ anggota/tambah.html (Tambah Anggota)
└──→ anggota/tambah.html (Tambah Anggota)
```

**Cara kerja navigasi:**
- Setiap halaman punya **navbar identik** di bagian atas (`<header>`)
- Navbar menggunakan class `.active` pada `<a class="nav-link active">` untuk
  menandai halaman yang sedang dibuka
- Path href berbeda tergantung kedalaman folder:
  - Dari `index.html`: `href="buku/list.html"`
  - Dari `buku/list.html`: `href="../index.html"` (naik 1 folder)
  - Dari `anggota/list.html`: `href="../buku/list.html"` (naik 1, masuk buku)

---

## 3. Bootstrap CDN — Cara Kerja

### 3.1 Yang Dimuat

```html
<!-- CSS: styling semua komponen Bootstrap -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
      rel="stylesheet">

<!-- JS: interaktifitas (navbar toggle, collapse, dll) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js">
</script>
```

### 3.2 Kenapa CDN?

| Aspek | CDN | npm/local |
|---|---|---|
| Install | Tidak perlu | `npm install bootstrap` |
| Internet | Wajib (pertama kali) | Tidak perlu |
| Update | Otomatis (ikuti versi CDN) | Manual |
| Cache | Browser cache otomatis | — |

### 3.3 Apa yang Terjadi Saat Halaman Dimuat?

1. Browser download `bootstrap.min.css` (~23 KB) → semua class CSS aktif
2. Browser download `bootstrap.bundle.min.js` (~79 KB) → JS + Popper.js
3. Browser render HTML → class seperti `.container`, `.card`, `.btn-primary`
   langsung berfungsi tanpa CSS tambahan
4. JS Bootstrap aktif → navbar toggler bisa diklik untuk buka/tutup menu

---

## 4. Penjelasan File per File

### 4.1 `index.html` — Beranda

**Tujuan:** Halaman utama menampilkan sambutan dan ringkasan statistik.

**Struktur HTML (dari atas ke bawah):**

```
<!DOCTYPE html>                    ← Deklarasi dokumen HTML5
<html lang="id">                   ← Bahasa Indonesia
<head>
    <meta charset="UTF-8">         ← Encoding karakter UTF-8
    <meta name="viewport"...>      ← Membuat halaman responsif
    <title>...</title>             ← Judul tab browser
    <link bootstrap.min.css>       ← Bootstrap CSS via CDN
    <link style.css>               ← CSS custom (orientation)
</head>
<body>
    <header>                       ← Navbar
    <main>                         ← Konten utama
        <section> (Hero)           ← Kartu sambutan
        <section> (Statistik)      ← 4 kartu statistik
    </main>
    <footer>                       ← Hak cipta
    <script bootstrap.bundle>      ← Bootstrap JS via CDN
</body>
```

#### A. Navbar (Header)

```html
<header class="navbar navbar-expand-md navbar-dark bg-primary">
```

| Class | Fungsi |
|---|---|
| `.navbar` | Komponen navbar Bootstrap |
| `.navbar-expand-md` | Horizontal di ≥768px, hamburger di <768px |
| `.navbar-dark` | Teks navbar otomatis putih |
| `.bg-primary` | Background biru (#0d6efd) |

**Tombol Hamburger:**

```html
<button class="navbar-toggler" data-bs-toggle="collapse"
        data-bs-target="#navMenu">
    <span class="navbar-toggler-icon"></span>
</button>
```

| Atribut | Fungsi |
|---|---|
| `data-bs-toggle="collapse"` | Toggle visibility saat diklik |
| `data-bs-target="#navMenu"` | Target yang di-toggle (nav dengan id `navMenu`) |
| `aria-controls="navMenu"` | Aksesibilitas: hubungkan tombol ke nav |
| `aria-expanded="false"` | State awal: menu tertutup |

**Menu Navigasi:**

```html
<nav class="collapse navbar-collapse" id="navMenu">
    <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link active" href="index.html">Beranda</a></li>
        ...
    </ul>
</nav>
```

| Class | Fungsi |
|---|---|
| `.collapse` | Default: tersembunyi (display:none) |
| `.navbar-collapse` | Khusus navbar, show saat expanded |
| `.navbar-nav` | Styling ul sebagai menu navigasi |
| `.ms-auto` | Margin-start: auto → dorong menu ke kanan |
| `.nav-item` | Wrapper setiap menu item |
| `.nav-link` | Styling link sebagai menu item |
| `.active` | Tandai halaman aktif (warna lebih terang) |

#### B. Hero / Sambutan

```html
<section class="card shadow-sm mb-4">
    <div class="card-body">
        <h2 class="card-title text-primary">Selamat Datang...</h2>
        <p class="card-text mb-0">Aplikasi sederhana...</p>
    </div>
</section>
```

| Class | Fungsi |
|---|---|
| `.card` | Komponen kartu dengan border & border-radius |
| `.shadow-sm` | Bayangan kecil (small) |
| `.mb-4` | Margin-bottom: 1.5rem |
| `.card-body` | Padding internal card |
| `.card-title` | Styling judul card |
| `.text-primary` | Warna teks biru brand |
| `.card-text` | Styling paragraf di card |
| `.mb-0` | Margin-bottom: 0 (hilangkan spasi bawah) |

#### C. Kartu Statistik (Grid Responsive)

```html
<div class="row g-3 text-center">
    <div class="col-6 col-sm-3">...</div>  ← Kartu 1
    <div class="col-6 col-sm-3">...</div>  ← Kartu 2
    <div class="col-6 col-sm-3">...</div>  ← Kartu 3
    <div class="col-6 col-sm-3">...</div>  ← Kartu 4
</div>
```

**Grid System Bootstrap:**

| Class | Breakpoint | Kolom |
|---|---|---|
| `.col-6` | xs (< 576px) | 2 kolom per baris |
| `.col-sm-3` | sm (≥ 576px) | 4 kolom per baris |

**Progresi layout:**
```
Portrait (< 576px):          Landscape (≥ 576px):
┌─────────┬─────────┐        ┌──────┬──────┬──────┬──────┐
│  Buku   │ Anggota │        │ Buku │Anggota│Pinjam│Tlmbt│
├─────────┼─────────┤        └──────┴──────┴──────┴──────┘
│ Pinjam  │ Tlmbt   │        (semua sejajar 1 baris)
└─────────┴─────────┘
   2 × 2 grid                   1 × 4 baris
```

| Class | Fungsi |
|---|---|
| `.row` | Container flexbox untuk grid |
| `.g-3` | Gutter: jarak antar kolom 1rem |
| `.text-center` | Teks rata tengah |
| `.col-6` | Lebar 50% di xs (2 kolom) |
| `.col-sm-3` | Lebar 25% di sm+ (4 kolom) |
| `.p-3` | Padding: 1rem semua sisi |
| `.rounded-3` | Border-radius: 1rem |
| `.bg-light` | Background abu-abu muda |
| `.h6` | Ukuran teks seperti heading 6 |
| `.text-secondary` | Warna teks abu-abu |
| `.fs-2` | Font-size: besar (1.5rem) |
| `.fw-bold` | Font-weight: tebal |

---

### 4.2 `buku/list.html` — Daftar Buku

**Tujuan:** Menampilkan tabel daftar buku perpustakaan.

**Yang unik dibanding index.html:**
- Ada komponen **tabel** dengan class `.table`
- Ada komponen **badge** untuk status stok
- Header tabel pakai `.bg-primary`

#### Tabel Bootstrap

```html
<div class="table-responsive">
    <table class="table table-striped table-hover align-middle">
        <thead class="bg-primary text-white">
            <tr>
                <th>Judul</th>
                <th>Pengarang</th>
                <th>Tahun</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Laskar Pelangi</td>
                <td>Andrea Hirata</td>
                <td>2005</td>
                <td>
                    <span class="badge bg-success">Tersedia</span>
                    <small class="text-muted d-block">(4)</small>
                </td>
                <td>
                    <button class="btn btn-warning btn-sm text-white">Edit</button>
                    <button class="btn btn-sm btn-primary">Detail</button>
                    <button class="btn btn-danger btn-sm">Hapus</button>
                </td>
            </tr>
            ...
        </tbody>
    </table>
</div>
```

| Class | Fungsi |
|---|---|
| `.table-responsive` | Horizontal scroll di mobile |
| `.table` | Base styling tabel |
| `.table-striped` | Baris bergantian warna (zebra) |
| `.table-hover` | Highlight saat cursor di atas baris |
| `.align-middle` | Konten sel rata tengah vertikal |
| `.bg-primary` | Background header: biru |
| `.text-white` | Teks header: putih |

#### Komponen Badge (Status Stok)

```html
<span class="badge bg-success">Tersedia</span>   ← Stok > 0
<span class="badge bg-danger">Kosong</span>        ← Stok = 0
<small class="text-muted d-block">(4)</small>     ← Angka stok
```

| Class | Fungsi |
|---|---|
| `.badge` | Komponen label kecil |
| `.bg-success` | Background hijau (stok tersedia) |
| `.bg-danger` | Background merah (stok kosong) |
| `.text-muted` | Teks abu-abu redup |
| `.d-block` | Display: block (baris baru) |

#### Tombol Aksi

```html
<button class="btn btn-warning btn-sm text-white">Edit</button>
<button class="btn btn-sm btn-primary">Detail</button>
<button class="btn btn-danger btn-sm">Hapus</button>
```

| Class | Fungsi |
|---|---|
| `.btn` | Base styling tombol |
| `.btn-warning` | Background kuning (edit) |
| `.btn-primary` | Background biru (detail) |
| `.btn-danger` | Background merah (hapus) |
| `.btn-sm` | Ukuran kecil |
| `.text-white` | Teks putih (pada btn-warning) |

---

### 4.3 `buku/tambah.html` — Form Tambah Buku

**Tujuan:** Formulir untuk input data buku baru.

**Yang unik:**
- Komponen **form** Bootstrap
- Validasi HTML5 (`required`, `min`, `max`)
- Dropdown `.form-select`

#### Struktur Form

```html
<form>
    <div class="mb-3">
        <label for="judul" class="form-label fw-semibold">Judul</label>
        <input type="text" class="form-control" id="judul" name="judul" required>
    </div>
    ...
    <button type="submit" class="btn btn-primary">Simpan</button>
</form>
```

**Polanya selalu sama untuk setiap field:**

```html
<div class="mb-3">                    ← Wrapper: margin bawah 1rem
    <label class="form-label fw-semibold">Label</label>  ← Label tebal
    <input class="form-control" required>                 ← Input penuh lebar
</div>
```

| Class | Fungsi |
|---|---|
| `.mb-3` | Margin-bottom: 1rem (jarak antar field) |
| `.form-label` | Styling label: margin-bottom, font-size |
| `.fw-semibold` | Font-weight: semi-bold (600) |
| `.form-control` | Input penuh lebar dengan styling Bootstrap |
| `.form-select` | Dropdown select dengan styling Bootstrap |
| `.btn-primary` | Tombol biru (submit) |

#### Validasi HTML5

| Atribut | Field | Fungsi |
|---|---|---|
| `required` | Judul, Pengarang, Tahun, Stok | Wajib diisi |
| `min="1900"` | Tahun | Minimal tahun 1900 |
| `max="2026"` | Tahun | Maksimal tahun 2026 |
| `min="0"` | Stok | Minimal stok 0 |
| `type="number"` | Tahun, Stok | Hanya angka, spinner naik/turun |
| `type="text"` | Judul, Pengarang, ISBN | Teks bebas |

#### Dropdown Kategori

```html
<select class="form-select" id="kategori" name="kategori">
    <option value="fiksi">Fiksi</option>
    <option value="non-fiksi">Non-Fiksi</option>
    <option value="referensi">Referensi</option>
</select>
```

---

### 4.4 `anggota/list.html` — Daftar Anggota

**Tujuan:** Menampilkan tabel daftar anggota perpustakaan.

**Struktur sama dengan buku/list.html** tapi:
- Kolom berbeda: No. Anggota, Nama, Alamat, No. HP, Aksi
- Tidak ada badge (tidak perlu status)
- Hanya 2 tombol aksi: Edit + Hapus (tidak ada Detail)

---

### 4.5 `anggota/tambah.html` — Form Tambah Anggota

**Tujuan:** Formulir untuk input data anggota baru.

**Struktur sama dengan buku/tambah.html** tapi:
- Field berbeda: Nama, No. Anggota, Alamat, No. HP
- Tidak ada dropdown (semua text input)
- Hanya 1 tombol: Simpan

---

### 4.6 `assets/css/style.css` — CSS Custom

**Tujuan:** Override orientation (portrait vs landscape) karena Bootstrap
tidak punya class built-in untuk orientasi.

#### Landscape Mode

```css
@media (orientation: landscape) {
    .container.my-4 {
        padding-top: 1rem;
        padding-bottom: 1rem;
    }
    .card.mb-4 {
        margin-bottom: 1.25rem !important;
    }
    .rounded-3.p-3 {
        padding-top: 0.75rem !important;
        padding-bottom: 0.75rem !important;
    }
    .navbar {
        padding-top: 0.35rem;
        padding-bottom: 0.35rem;
    }
    .table thead th {
        font-size: 0.85rem;
    }
}
```

**Logika:** Di landscape, layar lebar tapi pendek. Spacing harus lebih
compact supaya konten tidak terasa renggang vertikal.

| Selector | Yang Diubah | Efek |
|---|---|---|
| `.container.my-4` | padding-top/bottom | Konten lebih rapat |
| `.card.mb-4` | margin-bottom | Jarak antar card lebih kecil |
| `.rounded-3.p-3` | padding-top/bottom | Stat card lebih compact |
| `.navbar` | padding-top/bottom | Navbar lebih tipis |
| `.table thead th` | font-size | Header tabel lebih kecil |

#### Portrait Mode

```css
@media (orientation: portrait) {
    .fs-2.fw-bold {
        font-size: 1.5rem;
    }
    .table td, .table th {
        padding: 0.6rem 0.5rem;
    }
}
```

**Logika:** Di portrait, layar sempit tapi tinggi. Font angka stat card
dibatasi supaya tidak terpotong. Padding tabel ditambah supaya tidak sesak.

---

## 5. Daftar Lengkap Semua Class Bootstrap yang Dipakai

### 5.1 Layout & Grid

| Class | Fungsi | Dipakai di |
|---|---|---|
| `.container` | Wrapper max-width & center | Semua halaman |
| `.row` | Flexbox row untuk grid | index.html |
| `.col-6` | 50% width di xs | index.html |
| `.col-sm-3` | 25% width di sm+ | index.html |

### 5.2 Komponen

| Class | Fungsi | Dipakai di |
|---|---|---|
| `.card` | Kartu dengan border & shadow | Semua halaman |
| `.card-body` | Padding internal card | Semua halaman |
| `.card-title` | Judul card | Semua halaman |
| `.card-text` | Paragraf card | index.html |
| `.navbar` | Komponen navbar | Semua halaman |
| `.navbar-brand` | Logo/nama di navbar | Semua halaman |
| `.navbar-toggler` | Tombol hamburger | Semua halaman |
| `.navbar-collapse` | Container menu (collapse) | Semua halaman |
| `.navbar-nav` | List menu navigasi | Semua halaman |
| `.nav-item` | Wrapper menu item | Semua halaman |
| `.nav-link` | Link menu | Semua halaman |
| `.table` | Base tabel | list.html |
| `.table-striped` | Baris zebra | list.html |
| `.table-hover` | Highlight hover | list.html |
| `.table-responsive` | Scroll horizontal | list.html |
| `.badge` | Label kecil | buku/list.html |
| `.form-label` | Label form | tambah.html |
| `.form-control` | Input penuh lebar | tambah.html |
| `.form-select` | Dropdown select | buku/tambah.html |
| `.btn` | Base tombol | Semua halaman |

### 5.3 Utility

| Class | Fungsi | Dipakai di |
|---|---|---|
| `.navbar-expand-md` | Breakpoint navbar: md | Semua halaman |
| `.navbar-dark` | Teks navbar putih | Semua halaman |
| `.bg-primary` | Background biru | Navbar, thead |
| `.bg-light` | Background terang | Stat cards |
| `.bg-success` | Background hijau | Badge tersedia |
| `.bg-danger` | Background merah | Badge kosong |
| `.text-primary` | Teks biru | Judul, angka stat |
| `.text-secondary` | Teks abu-abu | Label stat, footer |
| `.text-white` | Teks putih | Header tabel, btn-warning |
| `.text-muted` | Teks redup | Angka stok |
| `.text-center` | Rata tengah | Footer, stat cards |
| `.fw-semibold` | Semi-bold | Label form, brand |
| `.fw-bold` | Bold | Angka stat |
| `.fs-2` | Font-size besar | Angka stat |
| `.h6` | Ukuran h6 | Label stat |
| `.small` | Font-size kecil | Footer |
| `.mb-0` | Margin-bottom: 0 | Card text |
| `.mb-3` | Margin-bottom: 1rem | Form fields |
| `.mb-4` | Margin-bottom: 1.5rem | Sections |
| `.my-4` | Margin top+bottom: 1.5rem | Main |
| `.g-3` | Gutter: 1rem | Grid row |
| `.p-3` | Padding: 1rem | Stat cards |
| `.py-3` | Padding top+bottom: 1rem | Footer |
| `.ms-auto` | Margin-start: auto | Navbar menu |
| `.shadow-sm` | Bayangan kecil | Cards |
| `.rounded-3` | Border-radius: 1rem | Stat cards |
| `.btn-sm` | Ukuran kecil | Tombol aksi |
| `.d-block` | Display: block | Angka stok |

### 5.4 Aksesibilitas

| Atribut | Fungsi | Dipakai di |
|---|---|---|
| `aria-label="Toggle navigation"` | Label screen reader untuk hamburger | Semua navbar |
| `aria-controls="navMenu"` | Hubungkan tombol ke menu | Semua navbar |
| `aria-expanded="false"` | State awal: tertutup | Semua navbar |
| `role="alert"` | Asumsikan alert untuk screen reader | — |
| `for="id"` | Hubungkan label ke input | Semua form |
| `required` | Validasi wajib diisi | Semua form |

---

## 6. Responsive Behavior — Apa yang Terjadi di Setiap Lebar

### 6.1 Navbar

```
< 768px (xs, sm):
┌──────────────────────────┐
│ SIMPUS-Mini        [☰]   │  ← Hamburger muncul
│                           │  ← Menu tersembunyi
└──────────────────────────┘

Klik ☰:
┌──────────────────────────┐
│ SIMPUS-Mini        [☰]   │
│ Beranda                  │  ← Menu muncul vertikal
│ Daftar Buku              │
│ Tambah Buku              │
│ Daftar Anggota           │
│ Tambah Anggota           │
└──────────────────────────┘

≥ 768px (md, lg, xl):
┌──────────────────────────────────────────────────┐
│ SIMPUS-Mini    Beranda  Daftar Buku  Tambah ...  │  ← Menu horizontal
└──────────────────────────────────────────────────┘
```

### 6.2 Kartu Statistik

```
< 576px (xs):                 ≥ 576px (sm):
┌─────────┬─────────┐         ┌──────┬──────┬──────┬──────┐
│  Buku   │ Anggota │         │ Buku │Anggota│Pinjam│Tlmbt│
├─────────┼─────────┤         └──────┴──────┴──────┴──────┘
│ Pinjam  │ Tlmbt   │
└─────────┴─────────┘
  2 kolom × 2 baris            4 kolom × 1 baris
```

### 6.3 Tabel

```
< 768px:                       ≥ 768px:
┌─────────────────────┐        ┌──────┬──────┬──────┬──────┬─────┐
│ Judul | Pengarang   │        │Judul │Pengar│ Tahun│ Stok │Aksi │
│ ------ scroll →     │        │------│------│------│------│-----│
│ (horizontal scroll) │        │ data │ data │ data │ data │btn  │
└─────────────────────┘        └──────┴──────┴──────┴──────┴─────┘
  Scroll horizontal              Full width
```

---

## 7. Logic & Data Flow

### 7.1 Data

Data di hardcode langsung di HTML (tidak ada backend/database).

**Data Buku (buku/list.html):**
| Judul | Pengarang | Tahun | Stok | Badge |
|---|---|---|---|---|
| Laskar Pelangi | Andrea Hirata | 2005 | 4 | Tersedia (hijau) |
| Bumi Manusia | Pramoedya Ananta Toer | 1980 | 2 | Tersedia (hijau) |
| Negeri 5 Menara | Ahmad Fuadi | 2009 | 0 | Kosong (merah) |
| Filosofi Teras | Henry Manampiring | 2018 | 5 | Tersedia (hijau) |
| Ronggeng Dukuh Paruk | Ahmad Tohari | 1982 | 1 | Tersedia (hijau) |

**Data Anggota (anggota/list.html):**
| No. Anggota | Nama | Alamat | No. HP |
|---|---|---|---|
| A001 | Siti Aminah | Malang | 0812xxxx |
| A002 | Budi Santoso | Batu | 0813xxxx |

**Data Statistik (index.html):**
| Label | Nilai |
|---|---|
| Total Buku | 12 |
| Total Anggota | 8 |
| Sedang Dipinjam | 3 |
| Buku Terlambat | 2 |

### 7.2 Form Submit

Ketika tombol **Simpan** diklik:
1. HTML5 validation berjalan (cek `required`, `min`, `max`)
2. Jika valid → form submit (page reload, tidak ada backend)
3. Jika invalid → browser tampilkan pesan error bawaan

### 7.3 Navbar Toggle (JavaScript)

Ketika tombol hamburger diklik:
1. Event handler dari `bootstrap.bundle.min.js` menangkap klik
2. Toggle class `.show` pada element dengan id `#navMenu`
3. Update `aria-expanded` dari `false` ke `true` (atau sebaliknya)
4. CSS Bootstrap animasi: `height: 0` → `height: auto` dengan transisi

---

## 8. Perbandingan: CSS Murni vs Bootstrap

| Aspek | CSS Murni (jobsheet-02) | Bootstrap (jobsheet-03) |
|---|---|---|
| File CSS | 240 baris / 4.8 KB | 83 baris / 1.2 KB (orientation only) |
| CSS aktif | ~240 baris | ~30 baris (sisa komentar) |
| Navbar hamburger | Checkbox hack (murni CSS) | `.navbar-toggler` + JS |
| Grid stat cards | `grid-template-columns` | `.row` + `.col-6 .col-sm-3` |
| Tabel zebra | `nth-child(even)` | `.table-striped` |
| Tombol warna | `td button:first-of-type` | `.btn-primary`, `.btn-warning`, `.btn-danger` |
| Form styling | Manual padding/border/width | `.form-control`, `.form-select` |
| Responsive | `@media (max-width:...)` | Class infix: `sm`, `md` |
| Komponen baru | — | `.badge`, `.alert`, `.card` |

---

## 9. Cheat Class — Referensi Cepat

### Ingin... | Pakai...
---|---
Bikin konten di tengah halaman | `.container`
Bikin kartu putih dengan bayangan | `.card .shadow-sm`
Bikin tombol biru | `.btn .btn-primary`
Bikin tombol merah | `.btn .btn-danger`
Bikin tombol kuning | `.btn .btn-warning`
Bikin tabel belang | `.table .table-striped`
Bikin tabel hover | `.table .table-hover`
Bikin form input | `.form-control`
Bikin dropdown | `.form-select`
Bikin label form | `.form-label`
Bikin badge hijau | `.badge .bg-success`
Bikin badge merah | `.badge .bg-danger`
Bikin teks biru | `.text-primary`
Bikin background biru | `.bg-primary`
Bikin background terang | `.bg-light`
Bikin margin bawah | `.mb-3` (1rem) / `.mb-4` (1.5rem)
Bikin padding | `.p-3` (1rem)
Bikin teks tebal | `.fw-bold` / `.fw-semibold`
Bikin teks kecil | `.small`
Bikin teks abu-abu | `.text-secondary`
Bikin teks merah redup | `.text-muted`

---

## 10. Breakpoints Bootstrap 5.3

| Infix | Min-width | Contoh |
|---|---|---|
| _(xs)_ | 0 | `.col-6` |
| `sm` | 576px | `.col-sm-3` |
| `md` | 768px | `.navbar-expand-md` |
| `lg` | 992px | — |
| `xl` | 1200px | — |
| `xxl` | 1400px | — |

**Cara baca:** `.col-sm-3` artinya "mulai 25% width di layar ≥ 576px,
sebelumnya ikut parent".

---

*Terakhir diperbarui: 2026-09-10*
