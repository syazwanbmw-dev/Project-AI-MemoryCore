# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Active
**Last Activity**: 2026-04-27
**Session Focus**: mypwa-v2 — UX fixes (save notification + dashboard carta grid)

## 💭 Working Memory (RAM)

### Active Context
- **Current Topic**: mypwa-v2 — UX fixes live production
- **Branch aktif**: `test` (dah merge ke main)
- **Last commit**: `6fab29c` (test), `34e568b` (main)
- **Production URL**: `erpm-sksalor.celikguru.my`

### Session Recap (For AI Restart)

- **Save notification — overlay tengah skrin** ✅
  - Sebelum: toast kecil di sudut kanan bawah, guru tak nampak
  - Fix: overlay hijau tengah skrin (pattern sama macam logout confirm), auto-hilang 2 saat
  - Files: `public/app.js` (showSaveNotify()), `public/app.css` (.save-notify-*), `public/ujian.html`

- **Dashboard Ujian Dalaman — grid carta layout** ✅
  - Masalah asal: master tanya bar tak penuh → sebenarnya CARD yang sempit (3 kolum sempit)
  - Fix sebenar: `setGridCols()` tukar dari `repeat(auto-fill,minmax(280px,1fr))` ke logik:
    - 1 card → `1fr` (penuh)
    - 2 card → `1fr 1fr`
    - 3+ card → `repeat(3, 1fr)` (max 3 per baris, baris kedua bila 4+)
  - File: `public/dashboard.html`

- **Lesson learned**: Lucy silap diagnosis awal — ingat masalah bar rendering, tapi sebenarnya masalah card width. Fix yang betul cuma 1 baris dalam setGridCols.

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Audit Log implementation | ⏳ PENDING | Plan dah siap: docs/superpowers/plans/2026-04-25-audit-log.md |
| Rate limiting `/api/login` | ⏳ BACKLOG | Dah ada (migration 014), tapi audit log belum |
| sistem-olahraga: test arkib | ⏳ PENDING | Verify laporan arkib papar nama murid betul |
| sistem-olahraga: merge ke main | ⏳ PENDING | Selepas confirm arkib OK |

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- showSaveNotify() dalam app.js — boleh guna semula untuk page lain
- Audit Log plan ada 4 tasks — belum execute langsung
