# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-06-11
**Session Focus**: mypwa-v2 — Brainstorm + Design + Plan OMR Scanner feature

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-11 pagi: OMR Scanner — Brainstorm + Spec + Plan** 🔄 PENDING EXECUTE

  1. **Feature:** `omr.html` — pengimbas borang jawapan objektif (macam ZipGrade), pure frontend, tanpa DB
  2. **Approach:** Custom lightweight OMR — template tetap 40 soalan (2 kolum × 20), kamera live, bilinear inverse mapping untuk detect bubble tanpa full image warp
  3. **Flow:** Print borang → input kunci jawapan → scan kamera → papar `X / N betul`
  4. **Spec:** `docs/superpowers/specs/2026-06-11-omr-scanner-design.md` — commit `07f37fd` (main)
  5. **Plan:** `docs/superpowers/plans/2026-06-11-omr-scanner.md` — commit `c02af44` (main) — 6 tasks
  6. **Status:** Plan siap, belum execute — master perlu pilih: Subagent-Driven atau Inline Execution

- **Sesi 2026-06-10 tengah hari: Bug fix kedudukan keseluruhan slip keputusan — LIVE production** ✅

  1. Fix `kiraRankingSlip` — sort by `jumlah_A DESC + jumlah_markah DESC`, group `rankKesel` by tahun
  2. Fix `totalKesel` — rank dalam tahun yang sama sahaja
  3. Commits: `b82f7aa` (test) → `694c6a4` (main, production live)

- **Sesi 2026-06-10 petang: Feature Auto-Tutup Ujian — LIVE PRODUCTION** ✅

  1. Migration 024 — `ALTER TABLE ujian ADD COLUMN tarikh_tutup TEXT`
  2. Merge test→main, push, deploy production: commit `dcc1cbe`, version `5be1df74`
  3. Live URL: `erpm-sksalor.celikguru.my`

- **Sesi 2026-05-31 petang: save-topic skill — Lucy Memory System** ✅

  1. Implement `save-topic` skill standalone — Skill count: 26 → **27 skills aktif**

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| OMR Scanner — Execute plan | ⏳ NEXT | Plan: `2026-06-11-omr-scanner.md` — 6 tasks, pilih Subagent atau Inline |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit main branch: `c02af44` (docs: implementation plan OMR Scanner)
- Latest commit test branch: `91b275f` (fix: admin.html — esc() pada tarikhTutup dalam onclick editUjian)
- Migration 024 applied ke staging + production ✅
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- PWA icon regenerate: jalankan `node scripts/generate-icons.js` setiap kali favicon.svg berubah
- Jana Sijil: Vue.js 2 + PDF.js 3.11.174 CDN, tiada migration, auth via requireAuth() + renderSidebar()

### OMR Scanner — Ringkasan Teknikal (untuk rujukan execute)
- **Fail baru:** `public/omr.html` (inline CSS + JS)
- **Fail disentuh:** `public/app.js` (tambah guruLinks sidebar)
- **Tiada migration, tiada route baru**
- **Koordinat normal:** NORM_W=600, NORM_H=800, 4 anchor di penjuru, bubble grid GRID_TOP=150
- **Algorithm:** detectAnchors → bilinear inverse mapping → sampleBrightness → kiraMarkah
- **Spec:** `docs/superpowers/specs/2026-06-11-omr-scanner-design.md`
- **Plan:** `docs/superpowers/plans/2026-06-11-omr-scanner.md`
