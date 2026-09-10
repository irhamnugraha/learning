# DESIGN_SYSTEM.md — LearnTajwid

> Dokumen ini adalah sumber kebenaran tunggal untuk semua keputusan visual di aplikasi LearnTajwid.
> Diekstrak dari `index.html` (repo `github.com/irhamnugraha/learning`) per September 2026, plus
> beberapa token baru untuk memperbaiki inkonsistensi yang ditemukan.
>
> **Aturan untuk siapa pun yang mengedit kode (termasuk Claude Code):**
> Semua warna, font, radius, shadow, dan spacing HARUS mengambil dari token di dokumen ini.
> Jangan menulis nilai hardcode baru (misal `#fffaf0` atau `rgba(0,0,0,0.65)`) langsung di CSS —
> tambahkan sebagai token baru di sini dulu, baru dipakai di kode.

---

## 1. Filosofi Desain

Tema visual: **"manuskrip Islami klasik"** — kertas krem, tinta emas, aksen ruby (merah bata),
dengan bingkai berlapis yang mengingatkan pada hiasan mushaf/manuskrip kuno. Nuansa tenang,
elegan, bukan playful/cerah — cocok untuk konteks belajar tajwid yang serius tapi tetap hangat.

---

## 2. Warna (Color Tokens)

### 2.1 Token inti (sudah ada di kode, jangan diubah nilainya tanpa alasan kuat)

```css
:root{
  --ink:        #122b26;   /* dasar background gelap (body) */
  --ink-2:      #0d211d;   /* teks gelap di atas permukaan terang (paper) */
  --paper:      #f3ecd8;   /* krem terang — awal gradient card */
  --paper-2:    #ece2c6;   /* krem gelap — akhir gradient card */
  --gold:       #b9902f;   /* emas — aksen utama, tombol primer */
  --gold-light: #e2c073;   /* emas terang — gradient tombol, teks eyebrow */
  --ruby:       #7a3430;   /* ruby — aksen sekunder, state aktif/selected */
  --ruby-light: #c76a5f;
  --teal-line:  #3f6b60;
  --ok:         #4f7a53;   /* hijau sukses / benar */
  --ok-light:   #9fd1a2;
}
```

### 2.2 Token tambahan — SUDAH DITERAPKAN (2026-09)

Sebelumnya banyak elemen (input, textarea, tombol kategori, tab, dll — ±10 tempat) memakai
`#fffaf0` yang di-hardcode berulang, alih-alih token resmi. Ini rawan drift (satu tempat
ke-update warnanya, tempat lain lupa). Token berikut sudah dipakai di kode:

```css
:root{
  --surface:        #fffaf0;              /* pengganti semua hardcode #fffaf0 */
  --surface-border:  rgba(122,52,48,0.3); /* border standar di atas --surface */
  --overlay-dim:     rgba(13,33,29,0.75); /* SATU warna overlay untuk semua modal/dialog,
                                              menggantikan rgba(0,0,0,0.65) dan
                                              rgba(13,33,29,0.72) yang saat ini beda-beda */
  --idle-surface:    rgba(255,255,255,0.02); /* tab/tombol tidak aktif di atas background gelap */
}
```

> **Kenapa ini penting:** `--paper` (`#f3ecd8`) dan `#fffaf0` itu dua warna krem yang mirip
> tapi TIDAK sama. Kalau keduanya dipakai berdampingan (mis. card dengan background
> `--paper` berisi input dengan background `#fffaf0`), akan terlihat seperti dua lapis
> "putih" yang beda — itulah salah satu sumber kesan tidak konsisten antar menu.

### 2.3 Peran warna (jangan pakai warna di luar peran ini)

| Peran | Token | Dipakai untuk |
|---|---|---|
| Background halaman | `--ink` + radial-gradient (lihat §4.1) | `body` saja |
| Permukaan card/modal | `--paper` → `--paper-2` (gradient) | `.panel`, `.auth-modal` |
| Permukaan input/kontrol kecil di atas card | `--surface` (BARU) | input, textarea, tombol pilihan, tab |
| Aksen utama / CTA | `--gold`, `--gold-light` | tombol utama, progress bar, eyebrow |
| Aksen sekunder / state aktif-error | `--ruby`, `--ruby-light` | tab aktif, badge salah, border selected |
| Sukses / benar | `--ok`, `--ok-light` | badge benar, status hafal |
| Overlay dim | `--overlay-dim` (BARU) | backdrop semua modal/dialog |
| Teks di atas ink (gelap) | `--paper`, `#c9c0a3`, `#d8d0b8` | teks di body gelap |
| Teks di atas paper (terang) | `--ink-2`, `#6b5f45`, `#8a7f5f` | teks di dalam card |

---

## 3. Tipografi

### 3.1 Font family (Google Fonts — sudah di-load di `<head>`)

```html
<link href="https://fonts.googleapis.com/css2?family=Amiri:wght@400;700&family=Fraunces:ital,opsz,wght@0,9..144,500;0,9..144,600;1,9..144,500&family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@500;600&display=swap" rel="stylesheet">
```

| Font | Peran | Kapan dipakai |
|---|---|---|
| **Fraunces** (serif) | Display / heading | `h1`, angka skor besar, tombol CTA utama |
| **Inter** (sans-serif) | Body / UI | Paragraf, label pilihan, input, tombol biasa |
| **JetBrains Mono** (monospace) | Label teknis/kecil | Eyebrow, badge, footer, timestamp, hint |
| **Amiri** (serif Arab) | Teks Arab | Huruf hijaiyah, contoh ayat — selalu `direction:rtl` |

### 3.2 Type scale

| Elemen | Font | Size | Weight | Catatan |
|---|---|---|---|---|
| Angka hasil skor | Fraunces | 46px | 600 | Terbesar di app |
| `h1` (judul halaman) | Fraunces | 26px | 600 | line-height 1.15 |
| Judul modal (`h3`) | Fraunces | 19px | default | |
| Judul kartu belajar | Fraunces | 19px | 600 | line-height 1.5 |
| Tombol CTA utama | Fraunces | 16px | 600 | |
| Body / input | Inter | 14–15px | 400–600 | |
| Tombol sekunder | Inter | 13–14px | 600–700 | |
| Sub-teks / deskripsi | Inter | 12.5–13px | 400 | warna `#6b5f45` / `#8a7f5f` |
| Label kecil | Inter | 11.5–12px | 400 | |
| Eyebrow / kategori label | JetBrains Mono | 11px | uppercase, `letter-spacing:0.18em` | |
| Badge / tab teknis | JetBrains Mono | 10–12px | 500–700 | sering `uppercase` |
| Footer copyright | JetBrains Mono | 10px | | `letter-spacing:0.02em` |
| Huruf Arab kecil (inline) | Amiri | 1.15em | | RTL |
| Huruf Arab besar (kartu) | Amiri | 1.5–1.7em | 700 | RTL |
| Ayat Al-Qur'an | Amiri | 1.3em | | RTL, `line-height:1.7`, `text-align:right` |

---

## 4. Layout & Background

### 4.1 Background halaman (body) — SATU-SATUNYA tempat yang boleh pakai ini

```css
background:
  radial-gradient(circle at 15% 10%, rgba(226,192,115,0.10), transparent 45%),
  radial-gradient(circle at 90% 85%, rgba(122,52,48,0.12), transparent 40%),
  var(--ink);
```

### 4.2 Container

- Lebar app: `max-width: 480px`, center, `padding: 24px 14px 50px`
- Mobile-first, single column

### 4.3 Struktur layar per mode — WAJIB, patokan konsistensi antar-menu

**Aturan utama:** ketiga mode utama (Belajar, Latihan, Challenge) HARUS memakai struktur
yang sama — satu `.panel` (kotak krem, §7) sebagai wadah konten inti. Tidak boleh ada mode
yang elemen intinya diletakkan langsung di atas latar gelap (`--ink`) tanpa panel pembungkus
— itulah akar masalah "warna tiap menu beda-beda" yang pernah ditemukan (mode Latihan
sempat tidak punya `.panel` sama sekali sebelum diperbaiki 2026-09).

Polanya dua lapis, konsisten di ketiga mode:

```
[di luar panel, di atas latar gelap --ink]
  - Tab kategori/chapter (.tabs / .tab)      -> lihat §10, background --idle-surface
  - Info meta ringkas (mis. "Kartu 1/18", "Soal 1/20", skor berjalan)
  - Progress bar tipis (.progress-bar)

[.panel — SATU kotak krem, §7 double-border WAJIB]
  - Konten inti yang sedang difokuskan (kartu, pertanyaan, daftar referensi)
  - Kontrol aksi terkait konten itu (jawab, tandai hafal, tombol lanjut)
  - Info sekunder (statistik, catatan)
```

| Mode | Di luar panel (dark bg) | Di dalam `.panel` (light bg) |
|---|---|---|
| **Challenge** (`screenQuiz`) | `.quiz-header` (label soal & skor) + `.progress-bar` | tag kategori, teks soal, opsi jawaban, penjelasan, tombol lanjut |
| **Latihan** (`#modeFlashcard`) | `#flashcardTabs` (tab bab) + `.progress-row` + `.progress-bar` | kartu flip (`.stage`/`.card`), tombol Hafal/Belum, nav Sebelumnya/Berikutnya/Acak, statistik, reset |
| **Belajar** (`#modeBelajar`) | `#belajarTabs` (tab kategori) | seluruh referensi (accordion makhraj, kartu sifat, dst) |

**Konsekuensi warna:** begitu sebuah elemen pindah ke dalam `.panel` (latar terang), dia
TIDAK BOLEH lagi pakai palet "di atas latar gelap" (teks `var(--paper)`/`#d8d0b8`, background
transparan putih tipis seperti `rgba(255,255,255,0.04)`) — harus ganti ke palet "di atas
paper" (§2.3): teks `var(--ink-2)`, kontrol kecil pakai `var(--surface)` +
`var(--surface-border)`, aksen tetap `var(--ruby)`/`var(--ok)`/`var(--gold)` (token aksen
sama dipakai di kedua latar, cuma kontras terhadap background-nya beda).

Kalau sebuah class dipakai di KEDUA konteks (mis. `.nav-btn` dipakai untuk tombol
"← Kembali" di luar panel, sekaligus tombol "Sebelumnya/Berikutnya" di dalam panel
Latihan), jangan ubah style dasarnya — scope override khusus dengan selector ancestor
(contoh: `#modeFlashcard .panel .nav-btn{...}`), supaya pemakaian di luar panel di
tempat lain tidak ikut berubah.

---

## 5. Border Radius

| Ukuran | Nilai | Dipakai untuk |
|---|---|---|
| Large | 16–18px | Card utama (`.panel` = 18px), modal (`.auth-modal` = 16px) |
| Medium | 10–12px | Tombol, input, badge kotak, choice button |
| Small | 8px | Tombol kecil (auth tab, authbar button) |
| Pill | 999px | Badge bulat, tombol login landing, prog-badge |

---

## 6. Shadow / Elevation

| Level | Nilai | Dipakai untuk |
|---|---|---|
| Card | `0 18px 40px rgba(0,0,0,0.35)` | `.panel` — shadow besar, lembut, netral (hitam) |
| Modal | `0 20px 60px rgba(0,0,0,0.4)` | `.auth-modal` — lebih besar dari card biasa |
| CTA berwarna | `0 6px 18px rgba(185,144,47,0.4)` | Tombol login — shadow **berwarna emas**, bukan hitam |
| Icon/logo | `drop-shadow(0 4px 10px rgba(0,0,0,0.35))` | Logo header |

> Pola: elemen netral (card) pakai shadow hitam transparan; elemen aksen/CTA pakai shadow
> berwarna sesuai warnanya sendiri (glow effect).

---

## 7. Border & Bingkai Khas ("Double Border")

Ciri visual unik aplikasi ini: card punya **bingkai dalam** kedua via pseudo-element:

```css
.panel{
  border: 1px solid rgba(185,144,47,0.35);   /* border luar — emas transparan */
  border-radius: 18px;
}
.panel::before{
  content:"";
  position:absolute; top:9px; left:9px; right:9px; bottom:9px;
  border: 1px solid rgba(122,52,48,0.16);    /* border dalam — ruby transparan */
  border-radius: 12px;
  pointer-events:none;
}
```

**Wajib dipakai di setiap card/panel baru** untuk menjaga identitas visual "manuskrip".

---

## 8. State / Highlight

| State | Treatment |
|---|---|
| Selected / chosen (choice button) | `border-color: var(--ruby)`, `background: rgba(122,52,48,0.08)`, `box-shadow: 0 0 0 1px var(--ruby) inset` |
| Active tab/mode | `background: var(--ruby)` atau `var(--gold-light)` tergantung konteks, `color` kontras |
| Correct / benar | `border-color: var(--ok)`, `background: rgba(79,122,83,0.15)` |
| Wrong / salah | `border-color: var(--ruby)`, `background: rgba(122,52,48,0.13)` |
| Disabled | `opacity: 0.45`, `cursor: not-allowed` |
| Focus (input) | `outline: 2px solid var(--gold)` |
| Hover (link/button ganjil) | `border-color` lebih terang dari base |

---

## 9. Animasi / Motion

| Nama | Durasi | Easing | Dipakai untuk |
|---|---|---|---|
| `spin` | 0.8s | linear infinite | Spinner loading |
| `landingLoginPulse` | 2.4s | ease-in-out infinite | Tombol login landing (shadow membesar-mengecil) |
| Progress bar fill | 0.3s | ease | `width` transition |
| Choice button | 0.15s | ease | `all` transition (hover/select) |

Selalu sertakan `@media (prefers-reduced-motion: reduce)` untuk animasi infinite (sudah
diterapkan di `landingLoginPulse`, jadikan standar untuk animasi baru).

---

## 10. Komponen — Ringkasan Pola

| Komponen | Radius | Background | Border | Shadow |
|---|---|---|---|---|
| Card (`.panel`) | 18px | gradient `--paper`→`--paper-2` | 1px emas transparan + inner ruby | besar, hitam transparan |
| Tombol primer (CTA) | 12px | gradient `--gold-light`→`--gold` | none | berwarna emas |
| Tombol pill (login) | 999px | gradient emas | none | berwarna emas + pulse |
| Input/textarea | 10px | `--surface` (BARU, ganti `#fffaf0`) | 1px ruby transparan | none |
| Tab tidak aktif | 10px | `--idle-surface` (BARU) | 1px emas transparan | none |
| Tab aktif | 10px | `--ruby` atau `--gold-light` | sama dengan bg | none |
| Badge pill kecil | 999px | `--surface` atau transparan | 1px | none |
| Modal | 16px | gradient `--paper`→`--paper-2` | none | besar |
| Overlay backdrop | — | `--overlay-dim` (satukan) | — | — |
| Mode utama (Belajar/Latihan/Challenge) | 18px | **wajib** 1 `.panel` per mode untuk konten inti (lihat §4.3) | sama seperti Card | sama seperti Card |

---

## 11. Checklist Sebelum Menambah Elemen UI Baru

1. Warna baru? → Cek apakah sudah ada token yang cocok di §2. Kalau belum, tambahkan token
   baru di sini dulu (jangan hardcode).
2. Card/panel baru? → Wajib pakai pola double-border di §7.
3. Butuh teks Arab? → Font `Amiri`, `direction:rtl`.
4. Butuh label kecil/teknis (kategori, status, timestamp)? → `JetBrains Mono`, biasanya
   `uppercase` + `letter-spacing`.
5. Tombol CTA utama? → Gradient emas + shadow berwarna (§6), font `Fraunces`.
6. Modal/overlay baru? → Pakai `--overlay-dim`, bukan angka rgba baru.
7. Menambah mode/layar baru? → Wajib ikuti struktur §4.3: konten inti dibungkus `.panel`,
   cuma tab/meta/progress-bar yang boleh di luar panel. Cek juga apakah elemen di dalamnya
   pakai palet "di atas paper" (§2.3), bukan palet untuk latar gelap.

---

## 12. Riwayat

- **2026-09**: Dokumen dibuat dari audit `index.html` LearnTajwid. Ditemukan & didokumentasikan
  3 sumber inkonsistensi: (1) `#fffaf0` hardcode berulang vs token `--paper`, (2) dua warna
  overlay berbeda (`rgba(0,0,0,0.65)` vs `rgba(13,33,29,0.72)`) untuk fungsi yang sama,
  (3) `rgba(255,255,255,0.02)` generik tanpa token untuk elemen idle. Token baru diusulkan
  di §2.2 untuk menyatukan.
- **2026-09 (lanjutan)**: Token §2.2 diterapkan ke kode. Ditemukan sumber inkonsistensi ke-4
  yang lebih besar: mode **Latihan** tidak dibungkus `.panel` sama sekali (elemen-elemennya —
  tombol Hafal/Belum, nav Sebelumnya/Berikutnya/Acak, statistik — didesain untuk latar gelap,
  langsung duduk di atas `--ink`), beda dari Belajar & Challenge yang sudah pakai `.panel`.
  Ditambahkan §4.3 sebagai patokan struktur layar per mode, dan Latihan dibungkus `.panel`
  mengikuti pola yang sama dengan `screenQuiz` (tab/meta/progress-bar di luar panel, konten
  inti + kontrol di dalam panel).
