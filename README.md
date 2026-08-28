# Panduan Update Aplikasi Kuis Tajwid

Dokumen ini berisi semua info yang dibutuhkan supaya kamu bisa minta Claude
mengupdate **bank soal (database)** atau **kode aplikasi (GitHub)** dari
chat manapun — nggak harus lanjut di chat yang sama.

---

## 1. Update Bank Soal (Database Supabase)

**Info project:**
- Nama project: `LearnTajwid`
- Project ref: `lqkgtqhqfgxxrwwssbvp`
- URL: `https://lqkgtqhqfgxxrwwssbvp.supabase.co`

**Tabel yang ada:**

| Tabel | Isi | Kolom penting |
|---|---|---|
| `makharij_base` | Bank soal Makharijul Huruf | `huruf`, `makhraj`, `kelompok`, `urutan` |
| `sifat_base` | Bank soal Sifatul Huruf | `istilah`, `pengertian`, `huruf`, `pasangan`, `kelompok`, `urutan` |
| `huruf_profile` | Profil per huruf (buat soal Campuran) | `huruf`, `hams_jahr`, `tebal_tipis`, `makhraj_singkat`, `lazimah`, `urutan` |
| `comparisons` | Soal perbandingan huruf mirip | `huruf_a`, `huruf_b`, `atribut`, `jawaban`, `alasan`, `urutan` |
| `leaderboard` | Skor pemain | Terisi otomatis dari app, **tidak perlu diedit manual** |

**Cara update dari chat Claude manapun (paling gampang):**
1. Buka chat baru dengan Claude
2. Kalau Claude belum terhubung ke Supabase kamu, dia akan munculkan tombol connect — tinggal tap
3. Bilang langsung apa yang mau diubah, misalnya:
   > "Update tabel makharij_base di project LearnTajwid (ref lqkgtqhqfgxxrwwssbvp): ubah makhraj huruf ض jadi 'Tepi lidah kiri menyentuh geraham atas'"
4. Claude akan jalankan perubahannya langsung ke database

**Cara manual (kalau mau edit banyak sekaligus / tanpa Claude):**
- Buka [supabase.com](https://supabase.com) → login → pilih project **LearnTajwid** → menu **Table Editor** → pilih tabel → edit seperti spreadsheet

> Catatan: perubahan di database **otomatis muncul** di aplikasi begitu halaman di-refresh — nggak perlu update kode/push ulang ke GitHub.

---

## 2. Update Kode Aplikasi & Push ke GitHub

**Info repo:**
- Repo: `github.com/irhamnugraha/learning`
- File utama: `index.html` (branch `main`)
- Live di: `https://irhamnugraha.github.io/learning/`

**Cara push dari chat Claude manapun:**
1. Minta Claude buat perubahan yang diinginkan pada kode
2. Bikin **GitHub Personal Access Token (Classic)** baru — token lama **tidak dipakai ulang**, selalu bikin baru tiap sesi:
   - Buka `https://github.com/settings/tokens/new`
   - **Expiration**: 7 hari cukup
   - **Select scopes**: centang kotak besar `repo`
   - Klik **Generate token**, copy tokennya (diawali `ghp_...`)
3. Kasih token itu ke Claude, bilang: *"push ke git dengan token ini: [token]"*
4. Setelah Claude konfirmasi berhasil push, **langsung hapus token itu** dari `github.com/settings/tokens` (klik Delete) — demi keamanan, token nggak boleh dibiarkan aktif kalau nggak dipakai

---

## Catatan Keamanan

- **Publishable key Supabase** (`sb_publishable_...`) yang tertanam di kode aplikasi itu **aman**, memang didesain untuk publik/frontend — bukan rahasia.
- **Token GitHub** (`ghp_...`) **HARUS** dihapus setelah dipakai. Jangan pernah biarkan token lama masih aktif "buat jaga-jaga" — kalau butuh push lagi nanti, tinggal bikin baru (30 detik doang).
- Jangan pernah kasih **service_role key** Supabase ke siapapun/ditanam di kode — itu kunci penuh ke database, beda dengan publishable key.
