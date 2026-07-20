# mypwa-v2 (eNilai)
*Coding Project - Created 2026-03-26 | Last Updated: 2026-06-12*

## Description
Sistem eNilai — platform pengurusan penilaian murid untuk sekolah rendah. Rebuild dari Supabase ke Cloudflare Workers + D1.

## Project Details
- **Type**: Coding Project
- **Status**: Active (Production Live)
- **Created**: 2026-03-26
- **Last Accessed**: 2026-06-12
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
| PAJSK — Drive Orphan File Cleanup | ✅ Done (2026-05-10) |
| PAJSK — Edit UX (loading state + notify) | ✅ Done (2026-05-10) |
| PAJSK — Bulk Import CSV/xlsx | ✅ Done (2026-05-17) |
| PAJSK — Auto Drive Folder per Sesi (create + delete) | ✅ Done (2026-05-17) |
| Bug fix kedudukan keseluruhan slip keputusan | ✅ Done (2026-06-10) |
| Auto-Tutup Ujian by Date (migration 024) | ✅ Done (2026-06-10) |
| OMR Scanner (omr.html) — pure frontend scan borang jawapan | ✅ Done (2026-06-12) |
| iPad Bug Fixes — dropdown race condition + sidebar logout | ✅ Done (2026-06-12) |

## Features Live (Production)
- Login (ADMIN + GURU roles)
- Dashboard — carta purata TP + ujian dalaman analisis
- Rekod RPM — input TP bulk per kelas/subjek
- Laporan RPM — pivot table + inline edit + cetak
- Ujian Dalaman — input markah + laporan + analisis taburan gred + auto-tutup by date
- PAJSK — rekod pertandingan + upload PDF + kategori Sumbangan + bulk import
- PAJSK — Auto Drive Folder per Sesi (create on tambah sesi, delete on padam sesi)
- Admin Panel — pengguna, kelas, murid, subjek, kurikulum, tetapan, audit log, tab PAJSK
- Sidebar accordion (RPM + Ujian Dalaman groups)
- Active tab highlight — pill untuk sublink, bar+amber untuk regular link
- Laporan PAJSK — semua kelas, carian nama, cetak grouped by murid

## PAJSK Drive Cleanup (selesai 2026-05-10)
- Sebelum: padam rekod tidak padam fail di Google Drive (orphan file)
- Fix: `deleteDriveFile()` helper — ekstrak fileId dari drive_link URL, call Apps Script delete action
- `c.executionCtx.waitUntil()` — guarantee Drive delete selesai sebelum Worker terminate
- 3 titik cover: `DELETE /:id`, `DELETE /by-sesi`, `PUT /:id` (ganti fail lama)
- Apps Script: tambah `action: 'delete'` handler — `setTrashed(true)`
- Fix minor: blob `mimeType` hardcode `application/pdf` → guna `data.mimeType` yang dihantar

## PAJSK Edit UX (selesai 2026-05-10)
- Button "Simpan" disable + teks "Menyimpan..." semasa request in-flight — prevent double-submit
- Re-enable button kalau ada error
- Tukar `toast()` → `showSaveNotify()` untuk edit & padam — konsisten dengan tambah rekod
- Tukar native `confirm()` → `confirmAction()` modal untuk confirm padam
- Teks confirm dikemaskini: "Fail dalam Google Drive juga akan dipadam."

## Tab PAJSK Admin (selesai 2026-05-09)
- Filter: sesi + kelas (Thn 4–6 sahaja) + kategori
- Table: grouped by murid, boleh collapse/expand, carian live
- Padam individual per rekod + bulk by sesi
- Fungsi cetak grouped dengan header logo+sekolah

## API Response Format — PENTING
- Endpoint yang return array terus (`/pengguna`, `/kelas`, `/sesi`): `api()` wrap → `r.data` = array
- Endpoint yang return wrapper object (`/pajsk`, `/ujian-markah/analisis`): `api()` wrap → `r.data` = `{ data:[...], total:N }`, perlu `r.data.data`

## Apps Script (PAJSK Upload)
- URL: dari wrangler secret `APPSCRIPT_URL`
- Secret: dari wrangler secret `APPSCRIPT_SECRET`
- Folder: `DRIVE_FOLDER_ID` (fallback global kalau sesi tiada folder)
- Actions: `upload` (default) + `action:'delete'` + `action:'createFolder'` + `action:'deleteFolder'`
- Storage: Google Drive ~3TB — kekal guna Drive, tidak migrate ke R2
- File: `docs/appscript/pajsk-upload.gs`

## Auto Drive Folder per Sesi (selesai 2026-05-17)
- POST /api/sesi → auto-call AppScript `createFolder` → simpan `drive_folder_id` dalam table sesi
- Folder name: `PAJSK - {nama_sesi}`
- Fail silently: kalau AppScript gagal, sesi tetap OK, `drive_folder_id = NULL`
- DELETE sesi → auto-padam folder Drive (`setTrashed`) via `waitUntil` — fail silently
- Helper `getSesiFolderId(db, env, nama_sesi)` — DRY, guna dalam upload + edit handler
- Fallback chain: `sesi.drive_folder_id ?? env.DRIVE_FOLDER_ID ?? null`
- Migration 022 — `ALTER TABLE sesi ADD COLUMN drive_folder_id TEXT`

## Auto-Tutup Ujian by Date (selesai 2026-06-10)
- `tarikh_tutup TEXT` dalam table ujian — migration 024
- Lazy check on-the-fly (tiada cron job)
- GET /ujian?input=1 — filter expired dari senarai guru
- POST /ujian-markah/bulk — 403 bila status=tutup atau tarikh lepas; hari sama = dibenarkan
- PUT /ujian/:id — db.batch() atomicity; pattern `'tarikh_tutup' in body` untuk clear ke NULL
- Frontend: `effectiveStatus()` badge, kolum Tarikh Tutup, bukaSemula modal
- Commits: `73d121e` → `dcc1cbe` (merge main), version `5be1df74`

## Bug Fix Kedudukan Keseluruhan Slip (selesai 2026-06-10)
- `kiraRankingSlip`: sort by `jumlah_A DESC + jumlah_markah DESC` (konsisten dengan table KED)
- `rankKesel` group by `tahun` (bukan semua murid semua tahun)
- Backend: tambah `tahun_kelas` dalam SELECT laporan ujian-markah
- Commits: `b82f7aa` (test) → `694c6a4` (main)

## iPad Bug Fixes (selesai 2026-06-12)
- **Dropdown race condition** — `selUjian` disabled semasa `init()` load, aktif selepas options siap
- **Loading feedback** — dependent dropdowns tunjuk "Memuatkan..." semasa API fetch
- **Sidebar logout — iOS 100vh bug** — `.sidebar` + `#sidebar` tukar ke `height:100dvh`
  - `100vh` pada iOS = layout viewport (termasuk belakang browser chrome), logout tersembunyi di bawah
  - `100dvh` = dynamic viewport = kawasan visible sebenar
- **Flex min-height bug** — `.sidebar-nav` tambah `min-height:0` — iOS flex child refuse shrink tanpa ini
- **Safe area support** — `.sidebar-logout` padding-bottom guna `env(safe-area-inset-bottom)` untuk iPad tanpa home button
- **Button visibility** — `.sidebar-logout .btn-ghost` opacity naik, tak bergantung pada `:hover`
- Commits: `7aab1f3` → `d1292b2` (test) → `ad1015e` (main), Version `9591deac`

## Backlog
- [ ] Compact mode Ujian Dalaman (bar 28px, 6 kad belum muat satu halaman)
- [ ] LinkedIn setup + dokumentasi journey

## DB IDs
- Production: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging: `f87c8bbc-77a5-4d57-88d1-284195de437f`

## CSS Version
- Current: `app.css?v=6`

---
*mypwa-v2 | Position #1 | Last updated 2026-06-12 (iPad Bug Fixes live)*
