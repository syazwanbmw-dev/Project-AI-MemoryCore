---
name: Projek ADNI
description: Sistem pengurusan markah pertandingan Mahrajan Al-Quran, Nasyid & Khat QIT Salor 2026
type: project
---

# Projek ADNI — Sistem Pertandingan

**Repo:** `C:\Users\user\Documents\code\adni`
**Branch aktif:** `test` (auto-deploy ke Cloudflare Pages via GitHub Actions)
**GitHub:** https://github.com/syazwanbmw-dev/adni

## Tech Stack
- Frontend: Vanilla HTML + JS + Tailwind CSS (static site)
- Backend: Cloudflare Worker + Hono.js
- Database: Google Sheets API v4 (Service Account JWT)
- Auth: Password-based → JWT token → sessionStorage (persist merentasi panel)

## Halaman
| Fail | Fungsi |
|------|--------|
| `admin.html` + `admin.js` | Panel admin — urus tetapan, acara, peserta |
| `keputusan.html` + `keputusan.js` | Panel hakim — masuk markah, auto-ranking |
| `dashboard.html` + `dashboard.js` | Paparan awam — keputusan & pengumuman animasi |

## Google Sheets Structure
- Sheet `Peserta`: kolum A=id, B=nama, C=jantina, D=sekolah, E=acara, F=markah, G=ranking
- Sheet `Tetapan`: tetapan pertandingan + senarai acara (A6:B6 → JSON string)

## Acara (Dynamic — boleh edit dalam admin)
Default: Seni Khat, Tilawah Al-Quran, Hafazan, Da'ie Cilik, Nasyid
- Individu (L/P): Seni Khat, Tilawah Al-Quran, Hafazan, Da'ie Cilik
- Berkumpulan: Nasyid

## Features Siap
- [x] Auth JWT + sessionStorage (login sekali, merentasi semua panel)
- [x] Panel Admin: tetapan program, urus acara (tambah/padam), urus peserta
- [x] Peserta: tambah manual, bulk import CSV (upsert by nama+sekolah), edit inline, padam
- [x] Senarai peserta: scroll tetap, carian nama, filter acara
- [x] Panel Keputusan: pilih acara (card grid dynamic), masuk markah, auto-ranking
- [x] Nasyid (Berkumpulan): panel keputusan group by sekolah, ranking per sekolah
- [x] Dashboard: paparan keputusan animasi (click-to-reveal), fullscreen mode
- [x] Dashboard cetak laporan semua acara (portrait) — top 3 per acara
- [x] Dashboard cetak per acara (landscape, dari overlay) — top 3, satu kategori satu halaman
- [x] Panel Keputusan cetak laporan penuh (one-click) — semua peserta semua acara, sorted by ranking
- [x] Print layout: dua logo (KPM kiri, QIT Salor kanan), nama program, tarikh, tempat, pengesah
- [x] Acara dynamic — admin boleh tambah/padam, semua panel ikut

## API Routes (src/index.js)
- `POST /api/auth` — login
- `GET/POST /api/tetapan` — baca/simpan tetapan
- `GET/POST /api/acara` — baca/simpan senarai acara
- `GET/POST /api/peserta` — senarai/tambah peserta
- `PUT /api/peserta/:id` — edit peserta
- `DELETE /api/peserta/:id` — padam peserta
- `POST /api/peserta/bulk` — bulk import CSV
- `POST /api/keputusan` — simpan markah + auto-ranking
- `GET /api/keputusan` — baca keputusan per acara+jantina (protected)
- `GET /api/dashboard` — data dashboard awam (top 3 per acara)
- `GET /api/laporan` — semua peserta dengan ranking, untuk cetak laporan penuh (protected)

## Data Pertandingan
- Nama: Mahrajan Al-Quran, Nasyid dan Khat QIT Salor 2026
- 11 sekolah: SK KOTA, SK PKUBOR SALOR, SK TIONG, SK SALOR, SK SERI KOTA, SK PENDEK, SK SULTAN ISMAIL 1, SK SULTAN ISMAIL 3, SK BUNUT PAYONG, SK SBRG PASIR MAS, SK PALOH PINTU GANG
- 172 peserta dah diimport via CSV

## Logo
- `public/images/logo-kpm.png` — Kementerian Pendidikan Malaysia
- `public/images/logo-gil.png` — QIT Salor (Excellence Quality Improvement Team)

## Pending / Belum Buat
- [ ] Da'ie Cilik — belum ada data peserta (acara ada tapi tiada pendaftaran)

## Status Deploy
- [x] Production (main branch) — last updated 2026-04-14, semua features working
- Production secrets configured: ADMIN_TOKEN, GOOGLE_SHEET_ID, GOOGLE_SERVICE_ACCOUNT_EMAIL, GOOGLE_PRIVATE_KEY

**Why:** Projek satu-event untuk sekolah rendah QIT Salor. Bukan SaaS — single-use per tahun.
**How to apply:** Kalau sambung session, check git log untuk changes terkini. Acara boleh berubah — selalu loadAcara() dari API, jangan hardcode.
