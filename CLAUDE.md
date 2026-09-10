# Project: LearnTajwid

Aplikasi kuis & flashcard tajwid untuk program Tahsin Masjid Al Falah Sakura Regency 2.
Live di: https://irhamnugraha.github.io/learning/

## Design

Semua styling (warna, font, radius, shadow, spacing, state/highlight) **wajib** mengikuti
`DESIGN_SYSTEM.md` di root folder ini. Jangan menulis nilai warna/ukuran baru langsung
di CSS (hardcode) — kalau butuh nilai baru, tambahkan sebagai token di `DESIGN_SYSTEM.md`
dulu, baru dipakai di kode.

Ciri khas yang WAJIB dipertahankan di setiap card/panel baru: double border (lihat
§7 `DESIGN_SYSTEM.md`) — border luar emas transparan + inner border ruby transparan
via pseudo-element.

## Stack

- Frontend: HTML/CSS/JS vanilla, single file (`index.html`), tanpa framework/build step
- Hosting: GitHub Pages (branch `main`, root)
- Database: Supabase — project ref `lqkgtqhqfgxxrwwssbvp`
  - Tabel bank soal: `makharij_base`, `sifat_base`, `huruf_profile`, `comparisons`
  - Tabel lain: `leaderboard`, `feedback`
  - Publishable key (`sb_publishable_...`) di kode = aman, memang untuk publik/frontend

## Konvensi Kode

- Vanilla JS, jangan tambah framework tanpa diskusi dulu
- Nama variabel domain-spesifik boleh pakai istilah Indonesia/Arab (huruf, makhraj, tajwid)
- Font: Fraunces (heading/display), Inter (body/UI), JetBrains Mono (label teknis),
  Amiri (teks Arab, selalu `direction:rtl`)

## Alur Kerja — WAJIB DIIKUTI

- **Selalu konfirmasi dulu sebelum mengubah/menerapkan apa pun ke `index.html` atau database**
  ("jangan update aplikasi sebelum konfirmasi"). Tunjukkan perubahan/preview dulu, baru
  eksekusi setelah disetujui secara eksplisit.
- Perubahan ke tabel Supabase (bank soal) **langsung muncul di app setelah refresh** —
  tidak perlu push ulang ke GitHub.
- Perubahan ke `index.html` **butuh push ke GitHub** untuk muncul di live site.

## Push ke GitHub

- Gunakan **GitHub Personal Access Token (Classic)** saja (`ghp_...`) — fine-grained PAT
  (`github_pat_...`) selalu gagal 403 saat push meski terlihat punya izin.
- Minta token baru tiap sesi (expiry pendek, scope `repo`), jangan pernah pakai ulang token lama.
- Jangan simpan token di file atau riwayat chat. Setelah push berhasil, ingatkan user untuk
  segera revoke/hapus token dari `github.com/settings/tokens`.

## Supabase — Operasi Database

- Gunakan `apply_migration` untuk semua DDL/DML (supaya tercatat sebagai migration).
- Gunakan `execute_sql` hanya untuk verifikasi read-only (SELECT).
- Setelah menambah RLS policy, jalankan `get_advisors` (type: `security`) dan perbaiki
  warning `search_path` dengan `ALTER FUNCTION ... SET search_path = public, pg_temp`
  plus `REVOKE EXECUTE` bila perlu.
- **Jangan pernah** menaruh atau meminta `service_role key` — itu kunci penuh ke database,
  beda dengan publishable key yang memang aman untuk frontend.

## Yang Belum Selesai / Perlu Diperhatikan

- Gambar diagram makhraj sudah ada di repo (`images/makharij`) dan sempat ditautkan di
  `huruf_profile`, tapi penampilan gambar di-revert sementara — menunggu kepastian lisensi
  gambar sebelum diaktifkan lagi.
- Token warna baru di `DESIGN_SYSTEM.md` §2.2 (`--surface`, `--overlay-dim`,
  `--idle-surface`, `--surface-border`) sudah diusulkan tapi **belum diterapkan** ke kode —
  cek dengan user dulu sebelum menerapkannya.
