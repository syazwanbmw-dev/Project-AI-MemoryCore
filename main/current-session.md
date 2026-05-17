# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-17
**Session Focus**: mypwa-v2 — Auto Drive Folder per Sesi — design spec selesai, plan belum tulis

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 petang: PAJSK Bulk Import — implement + live production** ✅

  1. Backend `POST /api/pajsk/bulk` — D1 batch lookup + UPSERT
  2. Frontend pajsk.html + admin.html — button Import Pukal, SheetJS parse, result modal, template download
  3. Bug fixes: lookup guna nama_kelas penuh ("6 DELIMA"), normalize kategori/peringkat/pencapaian case-insensitive
  4. Merged ke main — live production `erpm-sksalor.celikguru.my`

- **Sesi 2026-05-17 malam: Auto Drive Folder per Sesi — design spec selesai** ✅

  1. Brainstorming selesai — pilih Approach A (inline semasa POST /api/sesi)
  2. Error handling: sesi tetap OK walaupun AppScript gagal (drive_folder_id = NULL)
  3. Folder naming: `PAJSK - {nama_sesi}`
  4. Design doc ditulis + committed: `docs/superpowers/specs/2026-05-17-auto-drive-folder-design.md`
  5. **Next: Tulis implementation plan → implement → commit push staging**

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PAJSK Bulk Import | ✅ LIVE | Merged main 2026-05-17 |
| Auto Drive Folder — Design | ✅ SPEC SIAP | `docs/superpowers/specs/2026-05-17-auto-drive-folder-design.md` |
| Auto Drive Folder — Implement | ⏳ PLAN BELUM TULIS | 4 komponen: migration 022, AppScript, sesi.js, pajsk.js |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- nama_kelas dalam DB simpan nilai penuh: "6 DELIMA" (bukan "DELIMA" sahaja)
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (dalam wrangler secrets)
- AppScript actions sedia ada: `upload`, `delete` — perlu tambah `createFolder`
- Semua rekod PAJSK sekarang tiada fail — tiada backfill diperlukan

### Auto Drive Folder — Design Decisions
- Trigger: POST /api/sesi → AppScript createFolder → UPDATE sesi.drive_folder_id
- Folder name: `PAJSK - {nama_sesi}`
- Fail silently: kalau AppScript gagal, sesi tetap OK, upload fallback ke DRIVE_FOLDER_ID env
- DRY: helper `getSesiFolderId(db, env, nama_sesi)` dikongsi upload + edit dalam pajsk.js
- Fail yang diubah: migrations/022, sesi.js, pajsk.js (+ AppScript manual update)
