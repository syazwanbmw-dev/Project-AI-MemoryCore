# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-17
**Session Focus**: mypwa-v2 — PAJSK Bulk Import selesai, next: Auto Drive Folder per Sesi

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 petang: PAJSK Bulk Import — implement + live production** ✅

  1. Backend `POST /api/pajsk/bulk` — D1 batch lookup + UPSERT
  2. Frontend pajsk.html + admin.html — button Import Pukal, SheetJS parse, result modal, template download
  3. Bug fixes: lookup guna nama_kelas penuh ("6 DELIMA"), normalize kategori/peringkat/pencapaian case-insensitive
  4. Merged ke main — live production `erpm-sksalor.celikguru.my`

- **Next task dirancang: Auto Google Drive Folder per Sesi**
  - Bila sesi baru dibuat → auto-create folder dalam Google Drive
  - Sijil upload masuk ke folder sesi berkaitan
  - Semua rekod sekarang TIADA fail diupload lagi
  - Plan belum ditulis — tunggu sesi baru

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PAJSK Bulk Import | ✅ LIVE | Merged main 2026-05-17 |
| Auto Drive Folder per Sesi | ⏳ PLAN BELUM TULIS | Trigger: POST /api/sesi → createFolder AppScript → simpan drive_folder_id dalam sesi table |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- nama_kelas dalam DB simpan nilai penuh: "6 DELIMA" (bukan "DELIMA" sahaja)
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (dalam wrangler secrets)
- AppScript actions sedia ada: `upload`, `delete`
- Semua rekod PAJSK sekarang tiada fail — tiada migration untuk data lama diperlukan
