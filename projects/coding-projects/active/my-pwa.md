# mypwa-v2 (eNilai)
*Coding Project - Created 2026-03-26 | Last Updated: 2026-05-09*

## Description
Sistem eNilai — platform pengurusan penilaian murid untuk sekolah rendah. Rebuild dari Supabase ke Cloudflare Workers + D1.

## Project Details
- **Type**: Coding Project
- **Status**: Active (Production Live)
- **Created**: 2026-03-26
- **Last Accessed**: 2026-05-09
- **Position**: #1
- **Repo**: https://github.com/syazwanbmw-dev/mypwa-v2.git
- **Production URL**: https://mypwa-v2.syazwan-skpp82.workers.dev
- **Custom Domain**: erpm-sksalor.celikguru.my
- **Staging URL**: https://mypwa-v2-staging.syazwan-skpp82.workers.dev

## Technical Stack
- **Backend**: Hono.js on Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite)
- **Frontend**: Vanilla HTML/JS + CSS custom (app.css)
- **Auth**: JWT (jose) + SHA-256
- **Deployment**: GitHub Actions CI — test → staging, main → production

## Fasa Selesai
| Fasa | Status |
|------|--------|
| Fasa 1-7 — Foundation → Deploy | ✅ Done |
| Fasa 8 — Ujian Dalaman | ✅ Done |
| Fasa 9 — Audit Log | ✅ Done |
| Sidebar Accordion + Active Tab | ✅ Done (2026-05-07) |
| Laporan PAJSK — Semua Kelas + Cari + Cetak | ✅ Done (2026-05-08) |
| iPad Bug Fixes + iOS date input | ✅ Done (2026-05-09) |
| PAJSK — Kategori Sumbangan | ✅ Done (2026-05-09) |
| Admin Panel — Tab PAJSK | ✅ Done (2026-05-09) |

## Features Live (Production)
- Login (ADMIN + GURU roles)
- Dashboard — carta purata TP + ujian dalaman analisis
- Rekod RPM — input TP bulk per kelas/subjek
- Laporan RPM — pivot table + inline edit + cetak
- Ujian Dalaman — input markah + laporan + analisis taburan gred
- PAJSK — rekod pertandingan + upload PDF + kategori Sumbangan
- Admin Panel — pengguna, kelas, murid, subjek, kurikulum, tetapan, audit log, **tab PAJSK**
- Sidebar accordion (RPM + Ujian Dalaman groups)
- Active tab highlight — pill untuk sublink, bar+amber untuk regular link
- Laporan PAJSK — semua kelas, carian nama, cetak grouped by murid

## Tab PAJSK Admin (selesai 2026-05-09)
- Filter: sesi + kelas (Thn 4–6 sahaja) + kategori
- Table: murid, kelas, sesi, kategori (badge), aktiviti, peringkat, pencapaian, guru, link dokumen
- Padam individual per rekod
- Padam bulk by sesi — `DELETE /api/pajsk/by-sesi` (admin only)
- Badge warna: Unit Beruniform (kuning), Kelab/Persatuan (hijau), Sukan/Permainan (biru), Sumbangan (ungu), lain (kelabu)

## iPad Bug Fixes (selesai 2026-05-09)
- `input[type="date"].form-input { -webkit-appearance:none; appearance:none; }` dalam app.css
  - Fix: border invisible + tinggi tak konsisten pada iOS Safari
- filter-grid explicit breakpoints (rekod.html) — iPad 3-col, desktop 5-col
- `min-width:0` pada filter-grid children (app.css) — prevent overflow
- `line-height:1.5` pada form-input/form-select — height konsisten iOS

## PAJSK Kategori Sumbangan (selesai 2026-05-09)
- Tambah option dalam form tambah, filter laporan, edit modal
- Badge `.badge-sumbangan { background:#ede9fe; color:#6d28d9 }` — warna ungu

## Laporan PAJSK (selesai 2026-05-08)
- Dropdown kelas tambah "Semua Kelas (Thn 4–6)" — fetch tanpa kelas_id, filter frontend by tahun_kelas
- Carian nama murid — live filter hide/show group + rows, state kekal selepas edit rekod
- Group header tunjuk nama kelas bila semua kelas dipilih
- Fungsi cetak `cetakLaporan()` — window baru, grouped by murid, header logo+sekolah
- Kolum cetak: #, Kategori, Aktiviti, Peringkat, Pencapaian, Catatan

## Sidebar Active Tab (selesai 2026-05-07)
- Root cause: Cloudflare strip `.html` dari URL → `location.pathname=/profil` vs `l.href=/profil.html`
- Fix: `normPath()` dalam `renderSidebar()` — strip `.html` sebelum compare
- Design: sublink aktif = amber pill (flush kiri, rounded kanan) + 5px bar; regular link = amber text + 5px bar

## API Response Format — PENTING
- Endpoint yang return array terus (`/pengguna`, `/kelas`, `/sesi`): `api()` wrap → `r.data` = array
- Endpoint yang return wrapper object (`/pajsk`, `/ujian-markah/analisis`): `api()` wrap → `r.data` = `{ data:[...], total:N }`, perlu `r.data.data`

## Backlog
- [ ] Compact mode Ujian Dalaman (bar 28px, 6 kad belum muat satu halaman)
- [ ] LinkedIn setup + dokumentasi journey

## DB IDs
- Production: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging: `f87c8bbc-77a5-4d57-88d1-284195de437f`

## CSS Version
- Current: `app.css?v=6`

---
*mypwa-v2 | Position #1 | Last updated 2026-05-09*
