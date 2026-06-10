# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-06-10
**Session Focus**: mypwa-v2 — bug fix kedudukan keseluruhan + feature auto-tutup ujian

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-10 tengah hari: Bug fix kedudukan keseluruhan slip keputusan — LIVE production** ✅

  1. **Bug 1** — `kiraRankingSlip` guna `purata` (average) sahaja untuk rank, tapi table guna `jumlah_A DESC + jumlah_markah DESC` → slip rank tidak konsisten dengan table KED
  2. **Bug 2** — `totalKesel` kira semua murid semua tahun → sepatutnya rank dalam tahun yang sama sahaja (Tahun 5 vs Tahun 5 sahaja)
  3. **Fix backend** (`ujian-markah.js`): tambah `k.tahun AS tahun_kelas` dalam SELECT laporan + `tahun: r.tahun_kelas` dalam muridMap
  4. **Fix frontend** (`laporan-ujian.html`): ubah `kiraRankingSlip` — sort by `jumlah_A DESC + jumlah_markah DESC`, group `rankKesel` by `tahun` (bukan semua murid), pass `gredScale` ke function
  5. Commits: `b82f7aa` (test) → `694c6a4` (main, production live)

- **Sesi 2026-06-10 tengah hari: Feature Auto-Tutup Ujian — PLAN SIAP, belum implement** 📋

  1. Admin set `tarikh_tutup DATE` per ujian — guru diblock dari input bila tarikh lepas
  2. Lazy check (tiada cron) — check berlaku on-the-fly setiap request
  3. "Buka Semula" = modal dengan tarikh baru ATAU kosong (buka tanpa had)
  4. Spec: `docs/superpowers/specs/2026-06-10-ujian-auto-tutup-design.md`
  5. Plan: `docs/superpowers/plans/2026-06-10-ujian-auto-tutup.md` — 5 tasks siap ditulis
  6. **Belum execute** — master pilih execution mode (subagent atau inline) sebelum sesi ini berakhir

- **Sesi 2026-05-31 petang: save-topic skill — Lucy Memory System** ✅

  1. Semak upstream Kiyoraka/Project-AI-MemoryCore — jumpa 1 update baru (Topic Diary System, PR#8)
  2. Brainstorm + design spec `save-topic` — skill standalone (Approach A)
  3. Implement via Subagent-Driven Development — 5 tasks selesai
  4. Skill count: 26 → **27 skills aktif**

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Bug fix kedudukan keseluruhan | ✅ LIVE | Merged main 2026-06-10, commit `694c6a4` |
| Auto-Tutup Ujian by Date | 📋 PLAN READY | Plan di `plans/2026-06-10-ujian-auto-tutup.md` — 5 tasks, belum execute |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit test branch: `d5e9268` (docs: implementation plan auto-tutup ujian by date)
- Latest commit main branch: `694c6a4` (merge: test -> main, bug fix kedudukan keseluruhan)
- Next migration: `024_ujian_tarikh_tutup.sql` — ADD COLUMN tarikh_tutup TEXT pada table ujian
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- AppScript file: `docs/appscript/pajsk-upload.gs` — 4 actions: upload, delete, createFolder, deleteFolder
- PWA icon regenerate: jalankan `node scripts/generate-icons.js` setiap kali favicon.svg berubah
- Jana Sijil: Vue.js 2 + PDF.js 3.11.174 CDN, tiada migration, auth via requireAuth() + renderSidebar()
