# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-17
**Session Focus**: mypwa-v2 — Plan PAJSK Bulk Import

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 malam: PAJSK Bulk Import — design & plan selesai** ✅

  1. **Brainstorm selesai** — 5 soalan dijawab master:
     - Siapa: Kedua-dua guru (pajsk.html) + admin (admin.html tab PAJSK)
     - nama_sesi: Pilih dari UI dropdown (bukan dalam CSV)
     - Error handling: Skip rows yang gagal, report detail
     - link_sijil: Optional
     - Duplicate: Upsert (update kalau dah ada)
     - peringkat: WAJIB (master tukar dari optional)

  2. **Spec ditulis** — `docs/superpowers/specs/2026-05-17-pajsk-bulk-import-design.md`
     - 7 kolum CSV (6 wajib, 1 optional: link_sijil)
     - Backend: POST /api/pajsk/bulk, D1 batch lookup + batch upsert
     - Frontend: SheetJS parse, normalize header, validate, result modal
     - Upsert via ON CONFLICT DO UPDATE

  3. **Plan ditulis** — `docs/superpowers/plans/2026-05-17-pajsk-bulk-import.md`
     - 4 tasks: Backend endpoint, pajsk.html frontend, admin.html frontend, test+deploy
     - Kata Pipeline: plan → code → sight-hone → commit-seal → push
     - Belum execute — tunggu arahan master

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PAJSK Bulk Import | ⏳ PLAN SIAP | Spec + plan dah tulis, belum execute. Pilih Subagent atau Inline. |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- SheetJS CDN: `https://cdn.sheetjs.com/xlsx-0.20.3/package/dist/xlsx.full.min.js`
- Unique index PAJSK: `idx_pajsk_unique ON pajsk(murid_id, nama_sesi, kategori, nama_aktiviti, COALESCE(peringkat,''))`
- Plan path: `docs/superpowers/plans/2026-05-17-pajsk-bulk-import.md`
- Spec path: `docs/superpowers/specs/2026-05-17-pajsk-bulk-import-design.md`
