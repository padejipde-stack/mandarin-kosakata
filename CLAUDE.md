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
> **Pencegahan Bug DOM Overflow**:
> Perhatikan dengan teliti penutupan tag di dalam `.bubble-wrap`!
> Format yang benar adalah:
> `<div class="bubble-wrap"><div class="bubble"><div class="bubble-hanzi">...</div><div class="bubble-pinyin">...</div><div class="bubble-arti">...</div></div></div></div>`
> Jangan pernah menutup tag `</div>` ekstra setelah `.bubble-arti` yang menyebabkan `.dialog-box` tertutup sebelum waktunya dan merusak lebar kontainer `.wrap`!

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
  const code = ch.charCodeAt(0);
  return (code >= 0x4e00 && code <= 0x9fa5) || (code >= 0x3400 && code <= 0x4dbf);
}

// 2. Regex Silabel Pinyin dengan Nada
const PINYIN_SYLL_RE = /^[b-df-hj-np-z]*(?:[aāáǎàeēéěèoōóǒò]i?|[uūúǔù]o?|[iīíǐì]a?|[üǖǘǚǜ])[a-z0-9]*/i;

// 3. Mesin Pemetaan & Pewarnaan Karakter
function colorizePair(hanziStr, pinyinStr, charColorMap) {
  // Melakukan segmentasi Hanzi dan pemetaan pinyin per kata/suku kata
  // Menghasilkan tag <span style="color: ..."> yang seragam antara hanzi dan pinyin
}

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

2. **Kotak Aturan Penulisan Pinyin (`.aturan-block`)**:
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
  console.log(`Open: ${opens}, Close: ${closes}, Diff: ${opens - closes}`); // Wajib 0
  ```
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
