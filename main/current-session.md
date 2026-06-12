# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-06-12
**Session Focus**: mypwa-v2 — OMR Scanner UI Polish + Bug Fixes + Merge ke Production ✅

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-12 pagi: OMR Scanner — UI Polish, Bug Fixes, Merge Production** ✅ LIVE PRODUCTION

  1. **Template fixes:**
     - 2-column selalu: `rowsPerCol = ceil(n/2)`, ROW_H dinamik
     - Huruf A/B/C/D dalam setiap bubble
     - Center kolum (L_NUM_X 40→85, R_NUM_X 320→355), GRID_BOT 770→740 (elak tindih anchor)
     - Anchor kecil: ANCHOR_SZ 40→22
     - Tajuk "BORANG JAWAPAN" centered
     - Garisan nama panjang hingga hujung (x=350→560)
     - Print margin 8mm (zoom out)

  2. **UI fixes:**
     - Kunci grid layout: `grid-auto-flow:column` — Q1-Q(n/2) kiri, Q(n/2+1)-Qn kanan (match template)
     - Mobile responsive: button 34→28px pada ≤600px, `max-width:100%`
     - Centered wrapper `max-width:680px; margin:0 auto` untuk laptop
     - Tab Scan panggil `mulaScan()` (bukan `showTab` sahaja)
     - Button labels: Semak Jawapan, Edit Jawapan, Reset (merah)
     - Semua action buttons guna `.omr-tab` style (outline navy)
     - Lazy render: input kosong on load, grid muncul bila bilangan soalan dimasukkan

  3. **Bug fixes:**
     - `detectAnchors`: tolak imej gelap — maxDark 25% kuadran
     - Duplicate `const grid` SyntaxError dalam `renderKunci()` — menyebabkan sidebar kosong
     - Mobile navbar: `<nav id="topbar">` → `.mobile-header` + `.sidebar-overlay` (match halaman lain)

  4. **Commits test → main:** `19bad58` → `1c28c18` → `fb92f81` → `1cbf56a` → `ceaff73` → `2bb216b` → `d1bac9d` → `2ccc2fe` → `26fb953` → `77898a2` → `65f0001` → `ff45a54` → `61b693f` → `bfb868f` → `91810d2` → `0ecb7ae`
  5. **Merge:** test → main `83919b7` — LIVE production
  6. **Playwright:** 9/9 PASS (termasuk omr.spec.js dikemaskini)
  7. **Security note:** Admin password staging/production masih `Admin@1234` — belum ditukar

- **Sesi 2026-06-11 tengah hari: OMR Scanner — Execute 6 tasks (Subagent-Driven)** ✅ LIVE PRODUCTION

  1. **Feature:** `omr.html` — pengimbas borang jawapan objektif, pure frontend, tanpa DB
  2. **Latest commit:** `3d80314` (test branch, sebelum merge)
  3. **Status:** Merged ke main dalam sesi 2026-06-12

- **Sesi 2026-06-10 petang: Feature Auto-Tutup Ujian — LIVE PRODUCTION** ✅
  - Migration 024, commit `dcc1cbe`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Tukar password admin | ⚠ URGENT | Staging + production masih guna Admin@1234 |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit main: `83919b7` (merge test→main, OMR Scanner live)
- Latest commit test: `0ecb7ae` (test: update omr.spec.js)
- Migration 024 applied ke staging + production ✅
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- PWA icon regenerate: jalankan `node scripts/generate-icons.js` setiap kali favicon.svg berubah

### OMR Scanner — Ringkasan Teknikal (final)
- **Fail:** `public/omr.html` (inline CSS + JS), `public/app.js` (guruLinks sidebar)
- **Tiada migration, tiada route baru**
- **Koordinat normal:** NORM_W=600, NORM_H=800, GRID_TOP=150, GRID_BOT=740
- **Anchor:** ANCHOR_SZ=22, di penjuru (30,30), (570,30), (30,770), (570,770)
- **Column layout:** L_NUM_X=85, L_BX=[130,165,200,235], R_NUM_X=355, R_BX=[400,435,470,505]
- **Algorithm:** detectAnchors (maxDark 25%) → bilinear mapping → sampleBrightness → kiraMarkah
- **Playwright test:** `tests/omr.spec.js` — 9/9 PASS dengan TEST_PASSWORD=Admin@1234
