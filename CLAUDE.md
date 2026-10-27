# 📘 Panduan Pengembangan & Standar Arsitektur (CLAUDE.md)
## Modul Pembelajaran Bahasa Mandarin (Buku Teks & Workbook)

Dokumen ini adalah pedoman baku dan spesifikasi arsitektur resmi untuk memelihara, memperluas, dan membuat halaman **Buku Teks (`BabN.html`)** serta **Buku Latihan (`WorkBookBabN.html`)** baru pada proyek Mandarin ini. Seluruh file yang dibuat di masa mendatang **WAJIB** mematuhi aturan, konvensi penamaan, struktur DOM, dan skema warna yang didefinisikan di sini.

---

## 📂 1. Arsitektur Proyek & Struktur Direktori

### 1.1 Lokasi Penyimpanan Ganda (Dual-Path Sync)
Proyek ini memiliki dua salinan direktori yang harus selalu **sinkron 100% byte-per-byte**:

| Salinan | Path | Deskripsi |
|---|---|---|
| **Lokal (Web Server)** | `C:\laragon\www\Power\Mandarin\` | Lingkungan pengembangan aktif di Laragon |
| **Git Repository** | `C:\laragon\www\Power\Mandarin\mandarin-kosakata\` | Folder repositori Git yang tersambung ke GitHub |

> [!CAUTION]
> Setiap pembuatan atau pengeditan file (`BabN.html`, `WorkBookBabN.html`, `Index.html`, gambar, dll.) **HARUS langsung disalin ke folder `mandarin-kosakata`**. Pastikan hasil salinan memiliki hash SHA-256 yang identik sebelum melakukan `git commit`.

### 1.2 Peta File Proyek
```text
C:\laragon\www\Power\Mandarin\
├── Index.html                  # Portal beranda utama (daftar bab & workbook)
├── Bab1.html - Bab7.html       # Modul buku teks percakapan (Bab 1 s.d. Bab 7)
├── WorkBookBab1.html - ...     # Modul workbook latihan (Master CSS: WorkBookBab1.html)
├── kosakata kelistrikan.html   # Modul kosakata khusus teknis
├── favicon.ico                 # Favicon portal
├── images/                     # Aset gambar pembelajaran & kelistrikan
├── CLAUDE.md                   # Dokumen pedoman ini
├── workbook.md                 # Ringkasan cepat spesifikasi workbook
└── mandarin-kosakata/          # Cermin repositori Git (mirror identik)
```

### 1.3 Aturan File Bersih (Clean Workspace)
- **DILARANG** meninggalkan file sementara atau skrip uji coba sekali pakai (seperti `analyze.js`, `download_*.js`, `temp.html`, `style2.css`, dll.) di root folder.
- File yang boleh berada di root hanyalah: `Index.html`, `BabN.html`, `WorkBookBabN.html`, `kosakata kelistrikan.html`, `favicon.ico`, `images/`, `CLAUDE.md`, `workbook.md`, dan folder git.

---

## 🎨 2. Standar Desain & Skema Warna

Setiap bab memiliki identitas warna tema (`--primary`) yang unik, dengan struktur CSS yang seragam.

### 2.1 Palet Warna Buku Teks (`BabN.html`)

| Bab | Tema Utama | `--primary` | `--primary-dark` | `--primary-soft` |
|:---:|---|:---:|:---:|:---:|
| **Bab 1** | Merah Kaisar | `#c0392b` | `#8e1a12` | `rgba(192, 57, 43, 0.08)` |
| **Bab 2** | Ungu Royal | `#8e44ad` | `#5b2c6f` | `rgba(142, 68, 173, 0.08)` |
| **Bab 3** | Biru Samudra | `#2980b9` | `#1a5276` | `rgba(41, 128, 185, 0.08)` |
| **Bab 4** | Hijau Emerald | `#059669` | `#047857` | `rgba(5, 150, 105, 0.08)` |
| **Bab 5** | Jingga / Amber | `#d35400` | `#a04000` | `rgba(211, 84, 0, 0.08)` |
| **Bab 6** | Biru Teal | `#2980b9` | `#1a5276` | `rgba(41, 128, 185, 0.08)` |
| **Bab 7** | Crimson / Rose | `#be123c` | `#881337` | `rgba(190, 18, 60, 0.08)` |
| **Bab 8+** | Disarankan Indigo/Teal | `#4f46e5` / `#0d9488` | Sesuai turunan | Sesuai turunan 0.08 |

### 2.2 Palet Warna Workbook (`WorkBookBabN.html`)

| Bab | `--primary` | `--primary-dark` | Header Gradient | Badge Header |
|:---:|:---:|:---:|:---:|:---:|
| **Bab 1** | `#c0392b` | `#8e1a12` | `#d35400 → #a04000` | `#f39c12` |
| **Bab 2** | `#7c3aed` | `#5b21b6` | `#6d28d9 → #4c1d95` | `#8b5cf6` |
| **Bab 3** | `#0369a1` | `#0c4a6e` | `#0284c7 → #075985` | `#38bdf8` |
| **Bab 4** | `#15803d` | `#14532d` | `#16a34a → #166534` | `#4ade80` |
| **Bab 5** | `#b45309` | `#78350f` | `#d97706 → #92400e` | `#fbbf24` |

### 2.3 Variabel Global Baku (`:root`)
```css
:root {
  --primary: #...;        /* Warna aksen utama bab */
  --primary-dark: #...;   /* Varian gelap untuk border/gradient */
  --primary-soft: rgba(...); /* Varian transparan untuk background badge/card */
  --gold: #f39c12;
  --bg: #f4f6f9;
  --card-bg: #ffffff;
  --text: #2d3436;
  --sub: #636e72;
  --border: #e2e8f0;
  --radius: 16px;
  --shadow: 0 4px 20px rgba(0, 0, 0, 0.06);
}
```

---

## 🏛️ 3. Arsitektur Halaman Buku Teks (`BabN.html`)

Setiap file bab buku teks adalah dokumen mandiri (*self-contained*) dengan struktur standar berikut.

### 3.1 Kerangka Dasar & Header
```html
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Dialog Bahasa Mandarin - Bab N: [JUDUL_HANZI] ([JUDUL_PINYIN])</title>
  <link rel="icon" type="image/x-icon" href="favicon.ico" />
  <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🈶</text></svg>" />
  <style>
    /* Google Fonts Import */
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;600;700&family=Poppins:wght@300;400;600;700&display=swap');
    
    /* CSS Variables & Global Resets */
    :root { ... }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Poppins', sans-serif; background: var(--bg); color: var(--text); padding-bottom: 75px; }
    
    /* Header, Nav, Wrap, Dialog, Classroom, Cards, Toolbar Styles */
    ...
  </style>
</head>
<body class="show-pinyin show-arti show-vocab">

<!-- HEADER RESMI -->
<header>
  <h1>🈶 Dialog Bahasa Mandarin</h1>
  <p>Latihan percakapan · 对话练习</p>
  <div class="bab">Bab N — [JUDUL_HANZI] ([JUDUL_PINYIN]) · [TERJEMAHAN_INDONESIA]</div>
  
  <!-- Tautan Cepat ke Workbook jika sudah tersedia -->
  <div style="margin-top:0.55rem;">
    <a href="WorkBookBabN.html" style="display:inline-flex;align-items:center;gap:0.35rem;background:rgba(255,255,255,0.22);color:#fff;text-decoration:none;padding:0.3rem 0.9rem;border-radius:20px;font-size:0.78rem;font-weight:600;border:1px solid rgba(255,255,255,0.4);transition:all 0.2s ease;">
      📝 Buka Workbook Bab N ([Deskripsi Materi]) →
    </a>
  </div>
</header>
```

> [!NOTE]
> Header menggunakan pseudo-element `header::before` dengan watermark Hanzi besar (misalnya `content: '三';` untuk Bab 3) beropacity rendah (`0.06 - 0.08`).

### 3.2 Navigasi Sticky 3-Kolom Simetris
Bar navigasi menempel di bagian atas (`position: sticky; top: 0; z-index: 100;`) dengan 3 slot simetris:
```html
<nav class="nav-bar">
  <div class="nav">
    <div class="nav-side-left">
      <!-- Jika ada Bab sebelumnya -->
      <a href="Bab[N-1].html" class="nav-pager-btn prev" title="Kembali ke Bab [N-1]">
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"></polyline></svg>
        <span>Bab [N-1]</span>
      </a>
      <!-- Jika tidak ada (Bab 1), biarkan kosong: <div class="nav-side-left"></div> -->
    </div>
    
    <div class="nav-center">
      <a href="index.html" class="nav-home-btn" title="Kembali ke Daftar Bab">
        <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg>
        <span class="label">Menu Utama</span>
      </a>
    </div>
    
    <div class="nav-side-right">
      <!-- Jika ada Bab selanjutnya -->
      <a href="Bab[N+1].html" class="nav-pager-btn next" title="Lanjut ke Bab [N+1]">
        <span>Bab [N+1]</span>
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"></polyline></svg>
      </a>
      <!-- Jika Bab terakhir, biarkan kosong: <div class="nav-side-right"></div> -->
    </div>
  </div>
</nav>
```

### 3.3 Container Konten (`<div class="wrap">`)
Semua konten pembelajaran diletakkan di dalam satu pembungkus utama berukuran maksimal 860px:
```html
<div class="wrap">
  <!-- Semua section judul, percakapan, kelas Xiaoyu, dan latihan -->
</div>
```

---

## 🧩 4. Komponen Modul Buku Teks

### 4.1 Tokoh & Karakter Utama (Avatar Standar)
Daftar avatar tokoh yang telah memiliki identitas gradien baku:
- `.av-xiaoyu` (小语 - AI Tutor): `linear-gradient(135deg, #f39c12, #d68910)`
- `.av-wang` (王一飞 - Guru): `linear-gradient(135deg, #c0392b, #8e1a12)`
- `.av-chen` (陈天中 - Mahasiswa): `linear-gradient(135deg, #8e44ad, #5b2c6f)`
- `.av-bai` (白家月 - Mahasiswi): `linear-gradient(135deg, #16a085, #0e6655)`
- `.av-li` (李文 - Teman/Karakter): `linear-gradient(135deg, #e67e22, #d35400)`
- `.av-anni` (安妮 - Mahasiswi Asing): `linear-gradient(135deg, #e84393, #fd79a8)`
- `.av-xue` / `.av-stu` (学生 - Siswa/Umum): `linear-gradient(135deg, #27ae60, #1e8449)`

### 4.2 Struktur Kotak Dialog Percakapan (`.dialog-box`)
Setiap dialog bab (umumnya Teks 1, Teks 2, Teks 3) dibungkus dalam `.dialog-box`:
```html
<div class="section-title">💬 Teks [N]</div>
<div class="dialog-box">
  <div class="dialog-header">
    <div class="dialog-title">[Judul Teks / Lokasi / Konteks]</div>
    <div class="dialog-actions">
      <!-- Tombol aksi jika diperlukan -->
    </div>
  </div>
  
  <div class="dialog-body">
    <!-- Baris Dialog A -->
    <div class="dialog-line">
      <div class="avatar av-wang" title="王一飞 (Wang Yifei)">王</div>
      <div class="bubble-wrap">
        <div class="bubble">
          <div class="bubble-hanzi">你好！</div>
          <div class="bubble-pinyin">Nǐ hǎo!</div>
          <div class="bubble-arti">Halo!</div>
        </div>
      </div>
    </div>
    
    <!-- Baris Dialog B -->
    <div class="dialog-line">
      <div class="avatar av-chen" title="陈天中 (Chen Tianzhong)">陈</div>
      <div class="bubble-wrap">
        <div class="bubble">
          <div class="bubble-hanzi">老师，你好！</div>
          <div class="bubble-pinyin">Lǎoshī, nǐ hǎo!</div>
          <div class="bubble-arti">Guru, halo!</div>
        </div>
      </div>
    </div>
  </div>
</div>
```

> [!CAUTION]
> **Pencegahan Bug DOM Overflow & Penutupan Prematur `.dialog-box`**:
> Perhatikan dengan teliti hierarki penutupan tag di setiap baris dialog (`.dialog-line`):
> ```html
> <div class="dialog-line">                         <!-- 1. Buka dialog-line -->
>   <div class="avatar av-xxx">...</div>             <!-- 2. Avatar (langsung tutup) -->
>   <div class="bubble-wrap">                       <!-- 3. Buka bubble-wrap -->
>     <span class="speaker-name">...</span>
>     <div class="bubble">                          <!-- 4. Buka bubble -->
>       <div class="bubble-hanzi">...</div>         <!-- 5. Hanzi (langsung tutup) -->
>       <div class="bubble-pinyin">...</div>       <!-- 6. Pinyin (langsung tutup) -->
>       <div class="bubble-arti">...</div>          <!-- 7. Arti (HANYA SATU </div> !) -->
>     </div>                                        <!-- 8. Tutup bubble -->
>   </div>                                          <!-- 9. Tutup bubble-wrap -->
> </div>                                            <!-- 10. Tutup dialog-line -->
> ```
> 
> 🚫 **ANTI-PATTERN FATAL (DILARANG KERAS)**:
> Menulis penutup ganda pada baris `.bubble-arti`:
> ```html
> <!-- SALAH BESAR (FATAL BUG): -->
> <div class="bubble-arti">...</div></div>
> ```
> **Dampak Kerusakan Berantai (Domino Effect)**:
> 1. Tag `</div>` ekstra tersebut langsung **menutup `.dialog-box` secara prematur**.
> 2. Semua dialog berikutnya serta kotak kosakata `.sub-breakdown` terlempar keluar dari `.dialog-box`.
> 3. Tag penutup `</div>` di akhir teks dialog akan menutup kontainer induk `.wrap`.
> 4. Seluruh konten setelah teks tersebut (Xiaoyu's Classroom, latihan, dll.) meluber keluar ke tepi layar selebar **100vw (DOM Overflow Bug)**.
> 
> ✅ **ATURAN WAJIB**:
> - Baris `.bubble-arti` **HANYA BOLEH MEMILIKI SATU `</div>`**.
> - Selalu pastikan jumlah tag `<div>` yang dibuka dan ditutup dalam `.dialog-line` seimbang (3 buka : 3 tutup).

### 4.3 Rincian Arti Per Kata (`.sub-breakdown`)
Diletakkan tepat di bawah `.dialog-box` untuk membedah kosakata baru di teks tersebut:
```html
<div class="sub-breakdown">
  <div class="sub-title">Arti Per Kata · Teks [N]</div>
  <div class="breakdown-grid">
    <div class="word-card">
      <div class="word-char">老师</div>
      <div class="word-pinyin">lǎoshī</div>
      <div class="word-arti">guru</div>
    </div>
    <div class="word-card">
      <div class="word-char">忙</div>
      <div class="word-pinyin">máng</div>
      <div class="word-arti">sibuk</div>
    </div>
  </div>
</div>
```

### 4.4 Catatan & Analisis Tata Bahasa (`.de-box`, `.de-rule-card`)
Digunakan untuk menjelaskan pola kalimat (seperti partikel `的`, negasi `不`, kalimat tanya `吗`, pola `太……了`):
```html
<div class="de-box">
  <div class="de-header">
    <div class="de-badge">Tata Bahasa</div>
    <div class="de-title">Penggunaan Partikel Tanya 吗 (ma)</div>
    <div class="de-desc">Penjelasan ringkas konsep tata bahasa...</div>
  </div>
  
  <div class="de-rules">
    <div class="de-rule-card">
      <div class="de-rule-title">Rumus Pembentukan</div>
      <div class="de-formula">
        <span class="de-chip chip-subj">Kalimat Pernyataan</span>
        <span class="de-plus">+</span>
        <span class="de-chip chip-de">吗？</span>
      </div>
    </div>
  </div>
</div>
```

### 4.5 Ruang Belajar Xiaoyu (`.classroom-box`)
Kotak panduan materi intensif dengan border kiri tebal (`border-left: 5px solid var(--gold)` atau tema bab):
```html
<div class="section-title">🎓 Xiaoyu's Classroom · 小语讲堂</div>
<div class="classroom-box">
  <div class="classroom-header">
    <div class="avatar av-xiaoyu">语</div>
    <div class="classroom-intro">
      <div class="classroom-badge">Xiaoyu's Classroom · 小语讲堂</div>
      <div class="classroom-desc">Fokus materi, kaidah gramatika, atau pemecahan karakter</div>
    </div>
  </div>
  
  <div class="classroom-list">
    <div class="classroom-item">
      <div class="item-number">1</div>
      <div class="item-title">Contoh Kalimat untuk Dibaca Nyaring (Read Aloud)</div>
      <div class="item-body">
        <div class="char-breakdown">
          <div class="char-card">
            <div class="char-header">
              <span class="char-badge">Contoh 1</span>
              <div class="char-name-group">
                <span class="char-name-hanzi">你也很忙吗？</span>
                <span class="char-name-pinyin">Nǐ yě hěn máng ma?</span>
              </div>
            </div>
            <div class="char-desc-arti">Apakah kamu juga sangat sibuk?</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

### 4.6 Latihan Komprehensif (`Comprehensive Exercises · 综合练习`)
Bagian evaluasi berisi latihan isi bagian rumpang (*selection box*) dan deskripsi gambar:
```html
<div class="section-title">📝 Comprehensive Exercises · 综合练习</div>
<div class="classroom-box" style="border-left: 5px solid #8e44ad;">
  <!-- Header Latihan -->
  
  <!-- Bank Kosakata Pilihan (A - E) -->
  <div class="selection-box">
    <div class="selection-box-title">Pilihan Kata (Vocabulary Bank):</div>
    <div class="selection-grid">
      <div class="selection-item"><span class="selection-letter">A.</span> 哪 <span class="selection-pinyin">(nǎ)</span> = yang mana</div>
      <div class="selection-item"><span class="selection-letter">B.</span> 吗 <span class="selection-pinyin">(ma)</span> = partikel tanya (kah?)</div>
    </div>
  </div>
  
  <!-- Butir Soal dan Pembahasan Lengkap -->
  <div class="classroom-list">
    <div class="classroom-item">
      <div class="item-number">1</div>
      <div class="item-title">nadia (1) 你工作忙 <strong>吗</strong> ？</div>
      <div class="item-body">
        <p><strong>Pilihan yang tepat:</strong> B. 吗 (ma)</p>
        <p><strong>Alasan:</strong> Kalimat ini menanyakan kabar atau kondisi...</p>
      </div>
    </div>
  </div>
</div>
```

### 4.7 Tongue Twister (`.twister-box`)
Diakhiri dengan latihan artikulasi lidah dan intonasi khas Tiongkok:
```html
<div class="section-title">👅 Tongue Twister · 绕口令</div>
<div class="twister-box">
  <div class="twister-title">Judul Twister · [Hanzi]</div>
  <div class="twister-lines">
    <div class="twister-line">
      <span class="tw-hz">四是四，十是十。</span>
      <span class="tw-py">Sì shì sì, shí shì shí.</span>
      <span class="tw-id">Empat adalah empat, sepuluh adalah sepuluh.</span>
    </div>
  </div>
</div>
```

### 4.8 Floating Toolbar Bawah (`.dialog-toolbar`)
Toolbar melayang di bagian bawah dengan 4 tombol toggle kendali tampilan:
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

## ⚙️ 5. Standar Mesin JavaScript (Colorizer & Toggles)

Setiap bab buku teks menyertakan JavaScript engine standar di akhir file sebelum `</body>`:

```javascript
// 1. Deteksi Karakter Hanzi
function isHanziChar(ch) {
  return /[\u3007\u4e00-\u9fa5]/.test(ch);
}

// 2. Regex Silabel Pinyin dengan Nada (WAJIB: regex r? di akhir untuk Erhua support)
const _A = '[aāáǎàĀÁǍÀ]', _O = '[oōóǒòŌÓǑÒ]', _E = '[eēéěèĒÉĚÈ]', _I = '[iīíǐìĪÍǏÌ]', _U = '[uūúǔùŪÚǓÙ]', _V = '[üǖǘǚǜǕǗǙǛv]';
const _finals = [
  _I+_A+'ng', _I+_O+'ng', _U+_A+'ng', _U+_E+'ng',
  _I+_A+'n', _I+_A+_O, _I+'ng', _U+_A+'i', _U+_A+'n', _U+_E+'n',
  _A+'ng', _E+'ng', _O+'ng',
  _A+'i', _E+'i', _A+_O, _O+_U, _A+'n', _E+'n',
  _I+_A, _I+_E, _I+'n', _I+_U, _U+_A, _U+_O, _U+_E, _U+_I, _U+'n',
  _V+_E, 'er',
  _A, _O, _E, _I, _U, _V
].join('|');
// ⚡ r? di akhir regex = support Erhua (suku kata berakhiran -r seperti nǎr, tiáor)
const PINYIN_SYLL_RE = new RegExp('^(?:zh|ch|sh|[bpmfdtnlgkhjqxzcsywr])?(?:' + _finals + ')r?', 'i');

// 3. Mesin Pemetaan & Pewarnaan Karakter (Erhua-aware)
// ✅ WAJIB: engine HARUS mendeteksi Erhua sebelum memetakan suku kata ke Hanzi
function colorizePair(hanziStr, pinyinStr, charColorMap) {
  // Melakukan segmentasi Hanzi dan pemetaan pinyin per kata/suku kata via PINYIN_SYLL_RE
  // Menghasilkan tag <span style="color: ..."> yang seragam antara hanzi dan pinyin
  // Mengandung logika Erhua: jika suku kata berakhir -r (bukan "er/ér/ěr/èr")
  //   DAN Hanzi berikutnya = 儿, maka root silabel → Hanzi sebelum 儿, 'r' → 儿
}
```

> [!CAUTION]
> **Kewajiban Penggunaan Syllabic Regex Engine (Pencegahan Bug Kata Majemuk)**:
> Mesin `colorizePair()` **DILARANG KERAS** menggunakan metode pemecahan spasi sederhana (`pinyin.split(/\s+/)` dengan syarat `hanziChars.length === pinyinWords.length`). Metode naif tersebut menyebabkan **bug fatal** di mana kata majemuk (2 atau 3 karakter Hanzi seperti `手机` shǒujī, `医生` yīshēng, `桌子` zhuōzi, `出租车` chūzūchē) **TIDAK TERWARNAI** saat tombol 🎨 ditekan karena jumlah kata tanpa spasi tidak sama dengan jumlah karakter Hanzi!
> **WAJIB** gunakan tokenizer berbasis `PINYIN_SYLL_RE` yang memotong suku kata Pinyin secara modular dan memasangkannya 1-to-1 ke karakter Hanzi, serta berikan spasi antar-suku kata pada teks `.word-pinyin` (misalnya: `shǒu jī`, `yī shēng`, `zhuō zi`, `chū zū chē`).

> [!CAUTION]
> **Aturan Erhua (儿化) — Pencegahan Bug Pergeseran Warna Berantai**:
> Erhua adalah fenomena fonetik di mana karakter `儿` melebur ke dalam suku kata sebelumnya membentuk sufiks `-r`. Ini menyebabkan **2 Hanzi** dipasangkan ke **1 suku kata Pinyin** berakhiran `-r`.
>
> | Kata | Hanzi | Pinyin | Hanzi units | Pinyin syllables |
> |:---:|:---:|:---:|:---:|:---:|
> | *nǎr* | 哪儿 | nǎr | 1 | 1 ✅ |
> | *miàntiáor* | 面条儿 | miàntiáor | 2 | 2 ✅ |
>
> **Perbedaan Erhua vs. 儿 Mandiri:**
> - `儿` sebagai **Erhua sufiks** (di akhir kata): `哪儿` nǎr, `面条儿` tiáor, `点儿` diǎnr — Pinyin berakhir `-r`, karakter `儿` tidak berbunyi sendiri.
> - `儿` sebagai **morfem mandiri**: `儿子` érzi (anak lelaki), `女儿` nǚ'ér (anak perempuan), `幼儿园` yòuéryuán — Pinyin-nya tetap `ér`, karakter `儿` dihitung sebagai unit sendiri.
>
> **Aturan Implementasi di `colorizePair()`:**
> Saat memproses suku kata Pinyin berakhiran `-r` yang **bukan** `er/ér/ěr/èr`, periksa apakah Hanzi berikutnya adalah `儿`.
> - Jika YA → ini Erhua: split suku kata, root → Hanzi sebelum 儿, 'r' → karakter 儿. Konsumsi 2 Hanzi.
> - Jika TIDAK → ini `-r` biasa: pasangkan suku kata penuh ke 1 Hanzi.
> ```javascript
> const isErhua = /r$/i.test(syll) &&
>                 !/^[eēéěè]r$/i.test(syll) &&
>                 hzIdx + 1 < hzList.length &&
>                 hzList[hzIdx + 1] === '儿';
> if (isErhua) {
>   const rootSyll = syll.slice(0, -1);  // tanpa 'r'
>   // rootSyll → Hanzi[hzIdx] (warna sama)
>   // 'r'      → Hanzi[hzIdx+1] yaitu 儿 (warna sendiri)
>   hzIdx += 2;
> }
> ```

> [!CAUTION]
> **Aturan Penulisan Angka dalam Hanzi — Pencegahan Bug Offset Warna**:
> Angka **DILARANG** ditulis dalam format ASCII Latin (`1`, `2`, `5`, dll.) di dalam elemen `.bubble-hanzi` atau `.word-char` yang akan diproses `colorizePair()`.
> Karena `isHanziChar()` mengabaikan digit ASCII, penulisan `今天5月1号` menyebabkan offset perhitungan Hanzi vs Pinyin sehingga suku kata `wǔ` dan `yī` salah terpasang ke Hanzi yang bukan miliknya.
>
> | ❌ Salah | ✅ Benar |
> |:---:|:---:|
> | `今天5月1号` | `今天五月一号` |
> | `2025年5月1号` | `二〇二五年五月一号` |
>
> **Gunakan karakter Hanzi** untuk angka: `〇一二三四五六七八九十百千万`.

> [!CAUTION]
> **Aturan Kartu Kata dengan Garis Miring (Pencegahan Bug Hanzi-Pinyin Mismatch)**:
> Jangan pernah menulis dua karakter Hanzi berbeda dalam satu `.word-char` yang dibungkus dengan `/` (garis miring), seperti `他/她`, lalu memasangkannya hanya dengan satu suku kata Pinyin (`tā`).
> Hal ini menyebabkan `colorizePair()` mendeteksi 2 Hanzi vs 1 suku kata → warna tidak sinkron.
>
> | ❌ Salah | ✅ Benar |
> |:---:|:---:|
> | `<div class="word-char">他/她</div>` + `<div class="word-pinyin">tā</div>` | Buat **dua kartu terpisah**: kartu `他` (tā, laki-laki) dan kartu `她` (tā, perempuan) |

// 4. State & Handler Toggle
let colorActive = false;

function togglePinyin() { document.body.classList.toggle('show-pinyin'); updateToggleUI(); }
function toggleArti()   { document.body.classList.toggle('show-arti');   updateToggleUI(); }
function toggleVocab()  { document.body.classList.toggle('show-vocab');  updateToggleUI(); }
function toggleColor()  {
  colorActive = !colorActive;
  colorActive ? applyColors() : removeColors();
  updateToggleUI();
}

function updateToggleUI() {
  document.getElementById('btnPinyin').classList.toggle('active', document.body.classList.contains('show-pinyin'));
  document.getElementById('btnArti').classList.toggle('active',   document.body.classList.contains('show-arti'));
  document.getElementById('btnVocab').classList.toggle('active',  document.body.classList.contains('show-vocab'));
  document.getElementById('btnColor').classList.toggle('active',  colorActive);
}

// 5. Text-To-Speech (Web Speech API)
function playPinyin(text) {
  if (!('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = 'zh-CN';
  utter.rate = 0.85;
  window.speechSynthesis.speak(utter);
}
```

---

## 📝 6. Standar Buku Latihan (Workbook: `WorkBookBabN.html`)

Buku latihan berfokus pada fonetik, aturan pinyin (spelling rules), pembacaan 4 nada, dan latihan mandiri.

### 6.1 Template Master
- **`WorkBookBab1.html` adalah MASTER TEMPLATE CSS.**
- Saat membuat workbook baru (`WorkBookBabN.html`), salin seluruh blok `<style>` dari `WorkBookBab1.html`, lalu cukup ubah variabel warna tema (`--primary`, `--primary-dark`, dsb.) dan header gradient.

### 6.2 Komponen Khusus Workbook
1. **Grid 4 Nada (`.phonetic-row`)**:
   Setiap sel nada (t1, t2, t3, t4) dapat diklik untuk menyuarakan audio via `playPinyin(...)`.
   ```html
   <div class="phonetic-row">
     <button type="button" class="phonetic-cell t1" onclick="playPinyin('jī')">jī</button>
     <button type="button" class="phonetic-cell t2" onclick="playPinyin('jí')">jí</button>
     <button type="button" class="phonetic-cell t3" onclick="playPinyin('jǐ')">jǐ</button>
     <button type="button" class="phonetic-cell t4" onclick="playPinyin('jì')">jì</button>
   </div>
   ```

   > [!IMPORTANT]
   > **Konsistensi Inisial & Final pada Baris Latihan 4 Nada**:
   > Setiap baris latihan 4 nada (`.phonetic-row`) **WAJIB** melatih kombinasi inisial dan final yang **IDENTIK** di keempat sel nada (Nada 1, 2, 3, dan 4).
   > - ✅ **Benar**: `zhā` (t1), `zhá` (t2), `zhǎ` (t3), `zhà` (t4) — seluruhnya konsisten menggunakan inisial `zh` + final `a`.
   > - 🚫 **Dilarang**: Mencampur final berbeda dalam satu baris, misalnya nada 1 menggunakan `zhū` (final u) sementara nada 2, 3, 4 menggunakan `zhá, zhǎ, zhà` (final a).
   > - Selalu pastikan label deskripsi suku kata (misal: "Suku kata zh-a:") selaras 100% dengan suku kata pada tombol nada 1 hingga 4.

3. **Peta CSS Class Konten Workbook per Bab (Wajib untuk `applyColors()`)**:

   > [!CAUTION]
   > **BUG FATAL: `applyColors()` Copas dari Buku Teks Tidak Akan Berfungsi di Workbook**
   >
   > Workbook (`WorkBookBabN.html`) menggunakan **class HTML yang berbeda** dari Buku Teks (`BabN.html`). Jika `applyColors()` dan `removeColors()` di-copas mentah dari Bab teks tanpa disesuaikan, tombol 🎨 tidak akan mewarnai apapun karena selector CSS-nya tidak menemukan elemen yang ada di workbook.
   >
   > **Root Cause**: Buku teks memakai `.bubble-hanzi`, `.word-char`, dll. Workbook tiap bab memakai class tersendiri sesuai tipe konten fonetiknya.
   >
   > **WAJIB**: Setiap workbook HARUS memiliki versi `applyColors()` dan `removeColors()` yang disesuaikan dengan class konten fonetik bab tersebut. Lihat tabel pemetaan di bawah:

   | Bab Workbook | Konten | Class Hanzi | Class Pinyin |
   |:---:|---|---|---|
   | **Bab 1–3** | Dialog & word card standar | `.bubble-hanzi` | `.bubble-pinyin` |
   | **Bab 4** | Tone Sandhi cards | `.sandhi-hz` | `.py-read`, `.py-orig` |
   | **Bab 4** | Neutral Tone cards | `.neutral-hz` | `.neutral-py` |
   | **Bab 4** | Semantic pair | `.semantic-pair-title > span:first-child` | `.sem-pinyin` |
   | **Bab 4** | Soal & Contoh Pilihan Ganda | `.soal-hz`, `.dialog-soal-hz`, `.soal-option-hz`, `.latihan-intro .hz` | `.soal-py`, `.dialog-soal-py`, `.soal-option-py`, `.latihan-intro .py` |

   **Template `applyColors()` untuk WorkBookBab4** (Tone Sandhi, Nada Netral & Latihan Soal HSK):
   ```javascript
   function applyColors() {
     const charColorMap = { '你': '#e11d48', '好': '#16a34a' };

     // 1. Tone Sandhi cards
     document.querySelectorAll('.sandhi-card').forEach(card => {
       const hzEl = card.querySelector('.sandhi-hz');
       const pyRead = card.querySelector('.py-read');
       const pyOrig = card.querySelector('.py-orig');
       if (!hzEl || !pyRead) return;
       if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
       if (!pyRead.dataset.origHtml) pyRead.dataset.origHtml = pyRead.innerHTML;
       if (pyOrig && !pyOrig.dataset.origHtml) pyOrig.dataset.origHtml = pyOrig.innerHTML;
       const res = colorizePair(hzEl.dataset.origHtml, pyRead.dataset.origHtml, charColorMap);
       hzEl.innerHTML = res.hanzi;
       pyRead.innerHTML = res.pinyin;
       if (pyOrig) pyOrig.innerHTML = colorizePair(hzEl.dataset.origHtml, pyOrig.dataset.origHtml, charColorMap).pinyin;
     });

     // 2. Neutral Tone cards
     document.querySelectorAll('.neutral-card').forEach(card => {
       const hzEl = card.querySelector('.neutral-hz');
       const pyEl = card.querySelector('.neutral-py');
       if (!hzEl || !pyEl) return;
       if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
       if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
       const res = colorizePair(hzEl.dataset.origHtml, pyEl.dataset.origHtml, charColorMap);
       hzEl.innerHTML = res.hanzi;
       pyEl.innerHTML = res.pinyin;
     });

     // 3. Semantic pair cards
     document.querySelectorAll('.semantic-pair-card').forEach(card => {
       const titleEl = card.querySelector('.semantic-pair-title > span:first-child');
       card.querySelectorAll('.sem-pinyin').forEach(pyEl => {
         if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
         if (!titleEl) return;
         const hzText = titleEl.textContent.replace(/^\d+\.\s*/, '').trim();
         pyEl.innerHTML = colorizePair(hzText, pyEl.dataset.origHtml, charColorMap).pinyin;
       });
     });

     // 4. Soal Pilihan Ganda & Contoh (.soal-item)
     document.querySelectorAll('.soal-pertanyaan').forEach(sp => {
       const hzEl = sp.querySelector('.soal-hz'), pyEl = sp.querySelector('.soal-py');
       if (hzEl && pyEl) {
         if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
         if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
         const res = colorizePair(hzEl.dataset.origHtml, pyEl.dataset.origHtml, charColorMap);
         hzEl.innerHTML = res.hanzi; pyEl.innerHTML = res.pinyin;
       }
     });
     document.querySelectorAll('.dialog-soal-line').forEach(dsl => {
       const hzEl = dsl.querySelector('.dialog-soal-hz'), pyEl = dsl.querySelector('.dialog-soal-py');
       if (hzEl && pyEl) {
         if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
         if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
         const res = colorizePair(hzEl.dataset.origHtml, pyEl.dataset.origHtml, charColorMap);
         hzEl.innerHTML = res.hanzi; pyEl.innerHTML = res.pinyin;
       }
     });
     document.querySelectorAll('.soal-option').forEach(opt => {
       const hzEl = opt.querySelector('.soal-option-hz'), pyEl = opt.querySelector('.soal-option-py');
       if (hzEl && pyEl) {
         if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
         if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
         const cleanPy = pyEl.dataset.origHtml.replace(/^[()\s]+|[()\s]+$/g, '');
         const res = colorizePair(hzEl.dataset.origHtml, cleanPy, charColorMap);
         hzEl.innerHTML = res.hanzi; pyEl.innerHTML = '(' + res.pinyin + ')';
       }
     });
     document.querySelectorAll('.latihan-intro').forEach(intro => {
       const hzEl = intro.querySelector('.hz'), pyEl = intro.querySelector('.py');
       if (hzEl && pyEl) {
         if (!hzEl.dataset.origHtml) hzEl.dataset.origHtml = hzEl.innerHTML;
         if (!pyEl.dataset.origHtml) pyEl.dataset.origHtml = pyEl.innerHTML;
         const rawHz = hzEl.dataset.origHtml.replace(/^🔊\s*/, '');
         const res = colorizePair(rawHz, pyEl.dataset.origHtml, charColorMap);
         hzEl.innerHTML = '🔊 ' + res.hanzi; pyEl.innerHTML = res.pinyin;
       }
     });
   }

   function removeColors() {
     const selector = '.sandhi-hz, .py-read, .py-orig, .neutral-hz, .neutral-py, .sem-pinyin, .semantic-pair-title > span:first-child, .soal-hz, .soal-py, .dialog-soal-hz, .dialog-soal-py, .soal-option-hz, .soal-option-py, .latihan-intro .hz, .latihan-intro .py';
     document.querySelectorAll(selector).forEach(el => {
       if (el.dataset.origHtml !== undefined) el.innerHTML = el.dataset.origHtml;
       el.style.color = '';
       el.style.fontWeight = '';
     });
   }
   ```

   > [!IMPORTANT]
   > Saat membuat WorkBookBab baru dengan tipe konten fonetik berbeda (misalnya konten Bab 5 dengan grid nada baru), **WAJIB** mengidentifikasi class yang benar dari HTML konten baru tersebut dan menuliskan `applyColors()` yang menarget class-class itu. Jangan pernah copas fungsi ini dari buku teks tanpa verifikasi class.

4. **Kotak Aturan Penulisan Pinyin (`.aturan-block`)**:
   Menjelaskan kaidah penggabungan konsonan/vokal (seperti $j, q, x$ dengan $\ddot{u}$).
   ```html
   <div class="aturan-block">
     <div class="aturan-block-title">
       <span class="aturan-block-badge">Aturan 1</span>
       <span>Penggabungan Inisial j, q, x dengan Final ü</span>
     </div>
     <div class="aturan-rule rule-ok">
       <span class="aturan-icon">✅</span>
       <div class="aturan-text">Kaidah benar...</div>
     </div>
     <div class="aturan-rule rule-no">
       <span class="aturan-icon">🚫</span>
       <div class="aturan-text">Bentuk salah yang tidak diperbolehkan...</div>
     </div>
   </div>
   ```

---

## 🔄 7. Prosedur Integrasi Antar-Halaman (Siklus Pembaruan)

Setiap kali modul Bab atau Workbook baru dibuat:

1. **Update `Index.html`**:
   - Tambahkan kartu bab baru dengan tautan `BabN.html`.
   - Tambahkan tombol "Buka Workbook Bab N 📝" mengarah ke `WorkBookBabN.html`.
2. **Update Navigasi Bab Sebelumnya (`Bab[N-1].html`)**:
   - Perbarui `.nav-side-right` agar tombol "Next" mengarah ke `BabN.html`.
3. **Update Navigasi Workbook Sebelumnya (`WorkBookBab[N-1].html`)**:
   - Perbarui `.nav-side-right` agar mengarah ke `WorkBookBabN.html`.
4. **Update Header Bab Utama (`BabN.html`)**:
   - Pastikan terdapat badge tautan menuju `WorkBookBabN.html`.

---

## ✅ 8. Checklist Verifikasi Kualitas (Quality Assurance)

Sebelum menyerahkan pekerjaan atau melakukan `git push`, lakukan langkah verifikasi berikut:

- [ ] **Encoding**: File tersimpan dalam format **UTF-8 tanpa BOM**.
- [ ] **Keseimbangan DOM (DOM Depth = 0)**:
  Jalankan skrip verifikasi untuk memastikan tidak ada tag `<div>` yang kurang atau berlebih.
  ```javascript
  const fs = require('fs');
  const html = fs.readFileSync('BabN.html', 'utf8');
  const opens = (html.match(/<div[\s>]/gi) || []).length;
  const closes = (html.match(/<\/div>/gi) || []).length;
  console.log(`Open: ${opens}, Close: ${closes}, Diff: ${opens - closes}`); // Wajib 0 mutlak!
  ```
- [ ] **Audit Deteksi Penutup Tag Ganda pada Dialog**:
  Pastikan tidak ada kesalahan ketik penutup ganda `</div></div>` pada baris `.bubble-arti` yang dapat menutup `.dialog-box` secara prematur:
  ```javascript
  const badClosers = html.match(/class=["'][^"']*bubble-arti[^"']*["'][^>]*>[^<]*<\/div><\/div>/gi);
  if (badClosers) {
    console.error("FATAL ERROR: Ditemukan penutup </div> ganda pada .bubble-arti:", badClosers);
  }
  ```
- [ ] **Validasi Konsistensi 4 Nada (`.phonetic-row`)**:
  Periksa setiap baris 4 nada agar suku kata nada 1 hingga 4 menggunakan inisial dan final yang sama persis (contoh: *zhā, zhá, zhǎ, zhà* — tidak boleh tertukar dengan *zhū*).
- [ ] **Verifikasi Pewarnaan Kata Majemuk (Mode 🎨)**:
  Uji mode warna per karakter untuk memastikan kata 2 atau 3 karakter Hanzi (seperti `手机`, `医生`, `桌子`, `出租车`) terwarnai per karakter dan per silabel pinyin secara presisi menggunakan regex `PINYIN_SYLL_RE`.
- [ ] **Verifikasi `applyColors()` Workbook — Class Selector Sesuai Konten**:
  Setiap `WorkBookBabN.html` yang memiliki konten fonetik khusus (misal Tone Sandhi, Nada Netral, semantic pair, dll.) **WAJIB** memiliki `applyColors()` dan `removeColors()` yang menarget class HTML konten tersebut secara tepat.
  Lakukan verifikasi dengan cara:
  ```javascript
  // Di browser Console saat halaman workbook terbuka, klik 🎨 lalu cek:
  document.querySelectorAll('.sandhi-hz').length  // Harus > 0 untuk WorkBookBab4
  document.querySelectorAll('.neutral-hz').length // Harus > 0 untuk WorkBookBab4
  // Pastikan elemen yang diberi warna jumlahnya tidak 0!
  ```
  🚫 **DILARANG**: Copas `applyColors()` mentah dari Buku Teks (`BabN.html`) ke Workbook tanpa menyesuaikan selectornya — kode tersebut menarget `.bubble-hanzi`, `.word-char`, dll. yang **tidak ada** di workbook.
- [ ] **Verifikasi Erhua (儿化)**:
  Pastikan setiap kata berakhiran 儿 (seperti `哪儿`, `面条儿`, `点儿`) diuji menggunakan `deep_audit_v3.js` di scratch folder. Suku kata Pinyin berakhir `-r` (bukan `er/ér/ěr/èr`) harus dipasangkan ke 2 Hanzi: root → Hanzi sebelum 儿, 'r' → karakter 儿.
- [ ] **Verifikasi Angka dalam Hanzi**:
  Semua angka dalam `.bubble-hanzi` dan `.word-char` **WAJIB** menggunakan karakter Hanzi (`〇一二三四五六七八九十百千万`), bukan digit ASCII Latin (`0123456789`).
- [ ] **Verifikasi Kartu Kata Garis Miring**:
  Dilarang menyimpan dua Hanzi berbeda dalam satu `.word-char` yang dipisahkan `/` (seperti `他/她`). Setiap varian Hanzi yang berbeda harus menjadi kartu terpisah.
- [ ] **Jalankan Audit Simulasi colorizePair**:
  Setelah menambahkan konten baru, jalankan skrip `deep_audit_v3.js` di folder scratch dan pastikan semua file menunjukkan "0 bug – colorizePair engine sinkron 100%".
- [ ] **Tampilan Tidak Rusak**: Pastikan `.dialog-box` dan `.classroom-box` berada di dalam `.wrap` (tidak meluber ke tepi layar selebar 100vw).
- [ ] **Penyalinan Ganda (Dual-Path Sync)**:
  Salin file ke `C:\laragon\www\Power\Mandarin\mandarin-kosakata\`.
- [ ] **Verifikasi Hash Byte-per-Byte**:
  Pastikan hash SHA-256 kedua file identik di Laragon dan folder git.
- [ ] **Git Synchronization**:
  ```bash
  cd C:\laragon\www\Power\Mandarin\mandarin-kosakata
  git status
  git add .
  git commit -m "Update / Add Bab N & Workbook Bab N"
  git push -4 origin main
  ```
