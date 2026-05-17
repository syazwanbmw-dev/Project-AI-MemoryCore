# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-17
**Session Focus**: mypwa-v2 — Auto Drive Folder per Sesi — plan siap, sedia execute

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 petang: PAJSK Bulk Import — implement + live production** ✅

  1. Backend `POST /api/pajsk/bulk` — D1 batch lookup + UPSERT
  2. Frontend pajsk.html + admin.html — button Import Pukal, SheetJS parse, result modal, template download
  3. Merged ke main — live production `erpm-sksalor.celikguru.my`

- **Sesi 2026-05-17 malam: Auto Drive Folder per Sesi — design + plan siap** ✅

  1. Design spec: `docs/superpowers/specs/2026-05-17-auto-drive-folder-design.md`
  2. Implementation plan: `docs/superpowers/plans/2026-05-17-auto-drive-folder.md`
  3. **Belum execute — tunggu sesi baru**

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PAJSK Bulk Import | ✅ LIVE | Merged main 2026-05-17 |
| Auto Drive Folder — Design + Plan | ✅ SIAP | Spec + plan committed ke test branch |
| **Auto Drive Folder — Execute** | ⏳ NEXT | Ikut plan: 5 tasks, commit push staging |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Auto Drive Folder — Context untuk Execute

**Plan:** `docs/superpowers/plans/2026-05-17-auto-drive-folder.md`

**5 Tasks dalam plan:**
1. Migration 022 — `ALTER TABLE sesi ADD COLUMN drive_folder_id TEXT`
2. AppScript — tambah `createFolder` action **(MASTER BUAT MANUAL DULU)**
3. `src/routes/sesi.js` — POST handler call AppScript selepas INSERT sesi
4. `src/routes/pajsk.js` — helper `getSesiFolderId()` + update upload & edit
5. Deploy staging + ujian manual

**Design decisions:**
- Folder name: `PAJSK - {nama_sesi}`
- Fail silently: kalau AppScript gagal, sesi tetap OK, drive_folder_id = NULL
- DRY: helper `getSesiFolderId(db, env, nama_sesi)` dikongsi upload + edit
- Fallback: `drive_folder_id ?? DRIVE_FOLDER_ID env` kalau sesi lama tiada folder

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- AppScript actions sedia ada: `upload`, `delete` — perlu tambah `createFolder` (manual)
- nama_kelas dalam DB simpan nilai penuh: "6 DELIMA"
- Semua rekod PAJSK sekarang tiada fail — tiada backfill diperlukan
