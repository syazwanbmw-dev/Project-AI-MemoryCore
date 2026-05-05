# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-05
**Session Focus**: mypwa-v2 — PAJSK features (UI polish + analisis tab)

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **PAJSK — 5 feature commits selesai** 🔄 Staging — pending test penuh + merge ke main

  Perubahan yang dibuat hari ini:
  - Tukar subtitle halaman → "Daftar Rekod Pentaksiran Aktiviti Jasmani, Sukan dan Kokurikulum"
  - Mobile camera upload — 2 butang (📄 Pilih PDF / 📷 Snap Gambar) pada skrin ≤768px
  - Filter dropdown kelas — papar Tahun 4-6 sahaja
  - Backend benarkan image MIME types (jpeg/png/heic/webp) selain PDF
  - Toast upload berjaya/gagal guna `showSaveNotify()` overlay tengah skrin (extend type='err')
  - Pagination 10 rekod per halaman pada tab Rekod, backend return `{ data, total }` dengan D1 batch
  - Nama aktiviti auto-uppercase — `oninput` frontend + `toUpperCase()` backend
  - Tab Analisis ke-3 — 6 stat cards (Jumlah Rekod + 5 peringkat), auto load sesi aktif
  - Hide butang Cetak di tab Laporan (pending develop)

- **Commits staging (test branch)**:
  - `01fccb1` — label baru, mobile camera, filter Tahun 4-6
  - `2d63a4a` — toast overlay tengah skrin
  - `6ae555a` — pagination 10 rekod/halaman
  - `b3c28df` — nama_aktiviti auto-uppercase
  - `d0cd8b3` — tab Analisis 6 stat cards

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Test PAJSK penuh di staging | ⏳ PENDING | URL: mypwa-v2-staging.syazwan-skpp82.workers.dev/pajsk.html |
| Fix bugs kalau ada | ⏳ PENDING | Tunggu test result |
| Merge ke main + deploy production | ⏳ PENDING | Selepas staging confirm OK |
| Apply migration 018 + 019 ke production | ⏳ PENDING | `npx wrangler d1 migrations apply DB --env production --remote` |
| Set secrets production | ⏳ PENDING | `APPSCRIPT_URL`, `APPSCRIPT_SECRET`, `DRIVE_FOLDER_ID` (ID folder production Drive) |
| Audit Log (migration 016) — merge ke main | ⏳ BACKLOG | Siap di staging, pending merge |
| Rate limiting `/api/login` | ⏳ BACKLOG | Migration 014 ada, audit log dah siap |
| Cetak tab Laporan PAJSK | ⏳ BACKLOG | Hidden dulu, develop kemudian |

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- mypwa-v2 staging DB: `f87c8bbc-77a5-4d57-88d1-284265de437f`
- Staging folder Drive ID: `1Wxr2acfI8DYPu4MwzR3AWzQkiGFmFLG-`
- Production folder Drive ID: `1eLfVA_N7C-E1C0BHRndmd85rmpmBljBS` (hardcoded dalam Apps Script sebagai fallback)
- Apps Script URL: `https://script.google.com/macros/s/AKfycbwnaqfjz_KmW0UH9rSLXwzKICX-f1HJG7LzdZLDdvh05tI3QDrCGejfuWSZB8GLadiSaQ/exec`
- GET /pajsk kini return `{ data: [...], total: N }` — muatLaporan() dah dikemaskini ke `r?.data?.data`
