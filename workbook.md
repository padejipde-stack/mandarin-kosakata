# 📝 Panduan Pembuatan Workbook Mandarin Baru

> Dokumen ini adalah aturan dan template standar untuk membuat file `WorkBookBabN.html` baru agar **konsisten** dengan workbook yang sudah ada.

---

## 📂 Lokasi File

| Lokasi | Path |
|--------|------|
| **Lokal (Laragon)** | `C:\laragon\www\Power\Mandarin\WorkBookBabN.html` |
| **Git Repository** | `C:\laragon\www\Power\Mandarin\mandarin-kosakata\WorkBookBabN.html` |

> [!IMPORTANT]
> File **HARUS identik byte-per-byte** di kedua lokasi. Selalu tulis ke kedua path dan verifikasi dengan `f1.equals(f2)`.

---

## 🎨 Skema Warna Per Bab

Setiap workbook memiliki warna tema (`--primary`) yang **berbeda** dari bab lain, tetapi **seluruh CSS structuralnya identik** (copy dari WorkBookBab1.html sebagai base).

| Bab | `--primary` | `--primary-dark` | Header Gradient | Badge |
|-----|-------------|-------------------|-----------------|-------|
| **Bab 1** | `#c0392b` (Merah) | `#8e1a12` | `#d35400 → #a04000` | `#f39c12` |
| **Bab 2** | `#7c3aed` (Ungu) | `#5b21b6` | `#6d28d9 → #4c1d95` | `#8b5cf6` |
| **Bab 3** | `#0369a1` (Biru) | `#0c4a6e` | `#0284c7 → #075985` | `#38bdf8` |
| **Bab 4** | `#15803d` (Hijau) | `#14532d` | `#16a34a → #166534` | `#4ade80` |
| **Bab 5** | `#b45309` (Amber) | `#78350f` | `#d97706 → #92400e` | `#fbbf24` |

---

## 🏗️ Struktur HTML Wajib

### 1. `<head>` — Identik Untuk Semua Bab

```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Buku Latihan (Workbook) Mandarin — Bab N: [JUDUL]</title>
  <link rel="icon" href="data:image/svg+xml,...">
  <style>
    /* COPY SELURUH <style> DARI WorkBookBab1.html */
    /* HANYA GANTI 3 variabel warna di :root */
    /* TAMBAHKAN CSS kelas khusus bab ini di bagian bawah */
  </style>
</head>
```

### 2. `<body>` — Class Wajib

```html
<body class="show-pinyin show-arti show-vocab">
```

### 3. `<header>` — Format Baku

```html
<header style="background: linear-gradient(135deg, [WARNA1] 0%, [WARNA2] 100%);">
  <h1>📝 Buku Latihan Mandarin · 练习册</h1>
  <p>HSK 1 练习册 · Workbook Pelajaran N</p>
  <div class="bab" style="background: [BADGE_COLOR];">Bab N — [JUDUL BAB]</div>
</header>
```

### 4. Navigasi 3-Kolom Simetris (WAJIB)

```html
<div class="nav-bar">
  <div class="nav">
    <div class="nav-side-left">
      <a href="WorkBookBab[N-1].html" class="nav-pager-btn prev" title="Kembali ke Workbook Bab [N-1]">
        <svg ...chevron-left...></svg>
        <span>Workbook Bab [N-1]</span>
      </a>
    </div>
    <div class="nav-center">
      <a href="index.html" class="nav-home-btn" title="Kembali ke Daftar Bab">
        <svg ...home-icon...></svg>
        <span class="label">Menu Utama</span>
      </a>
    </div>
    <div class="nav-side-right">
      <a href="WorkBookBab[N+1].html" class="nav-pager-btn next" title="Lanjut ke Workbook Bab [N+1]">
        <span>Workbook Bab [N+1]</span>
        <svg ...chevron-right...></svg>
      </a>
    </div>
  </div>
</div>
```

> [!WARNING]
> - Jika **tidak ada bab sebelumnya**, link kiri mengarah ke `Bab[N].html` (Buku Utama).
> - Jika **tidak ada bab selanjutnya**, gunakan `<div class="nav-side-right"></div>` (kosong).
> - Tombol Home **SELALU** di tengah.

### 5. Konten Utama — `<div class="wrap">`

Semua bagian konten dibungkus dalam:
```html
<div class="wrap">
  <!-- Bagian 1, 2, 3, dst... -->
</div>
```

### 6. Floating Toolbar — Selalu di Paling Bawah

```html
<div class="dialog-toolbar">
  <div class="toolbar-title">
    <span class="icon">⚙️</span>
    <span>Bantuan:</span>
  </div>
  <div class="toolbar-buttons">
    <button id="btnPinyin" class="toggle-switch-btn" onclick="togglePinyin()" title="Tampilkan / Sembunyikan Pinyin">🔤</button>
    <button id="btnArti" class="toggle-switch-btn" onclick="toggleArti()" title="Tampilkan / Sembunyikan Arti">🇮🇩</button>
    <button id="btnVocab" class="toggle-switch-btn active" onclick="toggleVocab()" title="Tampilkan / Sembunyikan Detail">📖</button>
    <button id="btnColor" class="toggle-switch-btn" onclick="toggleColor()" title="Warnai Karakter Per Kata">🎨</button>
  </div>
</div>
```

---

## 📦 Komponen Konten yang Tersedia

### A. Bagian Kosakata (Word Cards)

```html
<div class="section-title">⭐ Bagian N: [JUDUL] · [汉字标题] (X Kosakata)</div>
<div class="classroom-box" style="border-left: 5px solid [WARNA];">
  <div class="classroom-header">
    <div class="avatar" style="background: linear-gradient(135deg, [W1], [W2]);">字</div>
    <div class="classroom-intro">
      <div class="classroom-badge" style="background: [WARNA]; color: #fff;">[BADGE]</div>
      <div class="classroom-desc">[DESKRIPSI]</div>
    </div>
  </div>
  <div class="breakdown-grid">
    <div class="word-card" onclick="playPinyin('[PINYIN]')" title="Klik untuk mendengar suara">
      <div class="word-char">[汉字]</div>
      <div class="word-pinyin">[pīnyīn]</div>
      <div class="word-arti">[Arti Indonesia]</div>
    </div>
    <!-- ... lebih banyak word-card ... -->
  </div>
</div>
```

### B. Latihan 4 Nada (Phonetic Tones Grid)

```html
<div class="phonetic-tone-headers">
  <div class="phonetic-tone-header-item tone-h1">Nada 1 ( ˉ )</div>
  <div class="phonetic-tone-header-item tone-h2">Nada 2 ( ˊ )</div>
  <div class="phonetic-tone-header-item tone-h3">Nada 3 ( ˇ )</div>
  <div class="phonetic-tone-header-item tone-h4">Nada 4 ( ˋ )</div>
</div>
<div class="phonetic-row">
  <button type="button" class="phonetic-cell t1" onclick="playPinyin('[X̄]')">[X̄]</button>
  <button type="button" class="phonetic-cell t2" onclick="playPinyin('[X́]')">[X́]</button>
  <button type="button" class="phonetic-cell t3" onclick="playPinyin('[X̌]')">[X̌]</button>
  <button type="button" class="phonetic-cell t4" onclick="playPinyin('[X̀]')">[X̀]</button>
</div>
```

### C. Aturan Pinyin (Rules Block)

```html
<div class="aturan-block">
  <div class="aturan-block-title">
    <span class="aturan-block-badge" style="background: [BG]; color: [FG];">[BADGE]</span>
    <span>[JUDUL ATURAN]</span>
  </div>
  <div class="aturan-rule rule-ok|rule-no|rule-info|rule-tip">
    <span class="aturan-icon">✅|🚫|ℹ️|📝</span>
    <div class="aturan-text">[PENJELASAN ATURAN]
      <div class="aturan-example">
        <button class="aturan-pill aturan-ok" onclick="playPinyin('[X]')">[X]</button>
        <span class="aturan-pill aturan-no">[Y]</span>
      </div>
    </div>
  </div>
</div>
```

### D. Dialog Percakapan

```html
<div class="dialog5-pair">
  <div class="dialog5-pair-num">Dialog N</div>
  <div class="dialog5-line dialog5-line-a">
    <div class="dialog5-avatar av5-a">A</div>
    <div class="dialog5-content">
      <div class="dialog5-hanzi" onclick="playPinyin('[汉字]')">[汉字]</div>
      <div class="dialog5-pinyin">[Pīnyīn]</div>
      <div class="dialog5-arti">[Arti]</div>
    </div>
  </div>
  <div class="dialog5-line dialog5-line-b">
    <div class="dialog5-avatar av5-b">B</div>
    <div class="dialog5-content">...</div>
  </div>
</div>
```

---

## 📜 Script Wajib (Identik di Semua Workbook)

Script di `<script>` tag harus memiliki fungsi-fungsi berikut:

| Fungsi | Kegunaan |
|--------|----------|
| `isHanziChar(ch)` | Deteksi karakter Hanzi |
| `colorizePair(hanzi, pinyin, map)` | Pewarnaan per-karakter |
| `applyColors()` | Terapkan warna ke semua elemen |
| `removeColors()` | Hapus warna / kembalikan ke asli |
| `updateToggleUI()` | Sinkronkan status tombol toolbar |
| `togglePinyin()` | Toggle tampilan pinyin |
| `toggleArti()` | Toggle tampilan arti |
| `toggleVocab()` | Toggle tampilan detail kosakata |
| `toggleColor()` | Toggle pewarnaan per-karakter |
| `playPinyin(text)` | Mainkan audio lafal via Web Speech API |

> [!TIP]
> Copy seluruh `<script>...</script>` dari template mesin terbaru (menggunakan regex silabel `PINYIN_SYLL_RE`), lalu sesuaikan `charColorMap` di dalam `applyColors()`.

> [!CAUTION]
> **Pewarnaan Kata Majemuk:**
> Mesin `colorizePair()` WAJIB menggunakan regex silabel `PINYIN_SYLL_RE`, BUKAN `split(' ')` sederhana. Hal ini mencegah bug di mana kata 2 atau 3 karakter (seperti `手机` shǒujī, `医生` yīshēng, `桌子` zhuōzi, `出租车` chūzūchē) tidak terwarnai karena ketiadaan spasi!

---

## ✅ Checklist Sebelum Selesai

- [ ] CSS di-copy dari WB1, hanya ganti 3 variabel warna `--primary`, `--primary-dark`, `--primary-soft`
- [ ] Header gradient & badge warna sesuai tabel skema warna
- [ ] Navigasi 3-kolom simetris: Prev ← Home → Next
- [ ] Link ke workbook sebelumnya & selanjutnya sudah benar
- [ ] Konsistensi 4 Nada: Pastikan semua 4 nada di `.phonetic-row` memiliki inisial dan final yang sama (contoh: *zhā, zhá, zhǎ, zhà*, bukan *zhū*)
- [ ] Mesin Pewarnaan: Menggunakan regex silabel `PINYIN_SYLL_RE` dan berhasil mewarnai kata majemuk (2/3 karakter) saat tombol 🎨 aktif
- [ ] Floating toolbar dengan 4 tombol (🔤 🇮🇩 📖 🎨)
- [ ] Script `playPinyin()` dan semua toggle berfungsi
- [ ] `charColorMap` di `applyColors()` sudah diisi kosakata bab baru
- [ ] Body class: `show-pinyin show-arti show-vocab`
- [ ] File identik di **kedua lokasi** (Laragon & Git repo)
- [ ] Update `WorkBookBab[N-1].html`: tambah tombol next → WorkBookBabN
- [ ] Update `BabN.html`: tambah badge link ke WorkBookBabN di header
- [ ] Update `Index.html`: tambah tombol "Buka Workbook Bab N" di card bab
- [ ] `git add`, `git commit`, `git push -4 origin main`
- [ ] Verifikasi byte match di kedua folder

---

## 🔗 Integrasi Antar-Halaman

Setiap kali workbook baru dibuat, update file berikut:

### 1. `WorkBookBab[N-1].html`
Tambah tombol next di `nav-side-right`:
```html
<a href="WorkBookBabN.html" class="nav-pager-btn next" title="Lanjut ke Workbook Bab N">
  <span>Workbook Bab N</span>
  <svg ...chevron-right...></svg>
</a>
```

### 2. `BabN.html`
Tambah badge link di `<header>` setelah `<div class="bab">`:
```html
<div style="margin-top:0.55rem;">
  <a href="WorkBookBabN.html" style="display:inline-flex;align-items:center;gap:0.35rem;background:rgba(255,255,255,0.22);color:#fff;text-decoration:none;padding:0.3rem 0.9rem;border-radius:20px;font-size:0.78rem;font-weight:600;border:1px solid rgba(255,255,255,0.4);">
    📝 Buka Workbook Bab N ([Deskripsi Singkat]) →
  </a>
</div>
```

### 3. `Index.html`
Tambah tombol di card bab setelah tombol "Mulai Belajar Bab N →":
```html
<a href="WorkBookBabN.html" class="btn" style="background:[WARNA_PRIMARY];color:#fff;margin-top:0.45rem;">
  Buka Workbook Bab N 📝
</a>
```

---

## 📁 Struktur File Proyek

```
C:\laragon\www\Power\Mandarin\
├── Index.html              ← Daftar semua bab & link workbook
├── Bab1.html               ← Buku teks bab 1 (link ke WB1)
├── Bab2.html               ← Buku teks bab 2 (link ke WB2)
├── WorkBookBab1.html       ← Workbook bab 1 (TEMPLATE CSS MASTER)
├── WorkBookBab2.html       ← Workbook bab 2
├── WorkBookBab3.html       ← (future)
└── mandarin-kosakata/      ← Git repo (mirror identik)
    ├── Index.html
    ├── Bab1.html
    ├── Bab2.html
    ├── WorkBookBab1.html
    ├── WorkBookBab2.html
    └── ...
```

> [!CAUTION]
> **WorkBookBab1.html adalah MASTER TEMPLATE CSS.** Jika ada perubahan CSS structural, ubah di WB1 terlebih dahulu, lalu propagasi ke semua workbook lain.
