# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-06-15
**Session Focus**: mypwa-v2 — OMR Scanner Bug Fixes (anchor detect + bubble reading) ⏳ DI TEST BRANCH, pending master test staging

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-14/15: OMR Scanner — 3 Bug Fixes** ⏳ TEST BRANCH (belum merge main)

  Konteks: master test OMR scanner, jumpa 2 isu berturutan. Debug guna data sebenar (Playwright + foto master), bukan teka.

  1. **"Anchor tidak ditemui"** (commit `e67b8c1`):
     - Punca: `detectAnchors` lama cari anchor dalam kuadran TETAP di penjuru gambar. Gagal bila borang kecil di tengah frame (anchor tak sampai penjuru).
     - Fix: auto-detect — Otsu adaptive threshold + connected-components (DFS flood fill) + tapis bentuk kotak pejal + pilih 4 penjuru + validasi segi empat.
     - Terbukti kesan 4 anchor tepat pada foto sebenar (19ms).

  2. **Markah tak tepat (baca kosong)** (commit `92f6344`):
     - Punca: threshold MUTLAK `< 110` terlalu ketat. Murid guna PENSEL (grafit kelabu, brightness ~101-132) → hampir semua bubble dibaca kosong. Brightness per soalan terbukti BETUL (geometri OK), cuma ambang salah.
     - Fix: `detectBubbles` tukar ke KONTRAS RELATIF — banding bubble paling gelap vs purata 3 lain dalam soalan sama (margin >= 18). Auto-laras ikut kertas/cahaya. Terbukti baca 10/10 betul.

  3. **Frame terpotong → hasil salah senyap** (commit `92f6344`):
     - Punca: bila anchor atas keluar frame, algoritma pilih dot bubble sebagai anchor palsu → mapping rosak → markah salah tanpa amaran (bahaya untuk guru).
     - Fix: guard saiz seragam dalam `detectAnchors` — tolak kalau 4 anchor ratio saiz > 3.5 (_found = -2). Mesej baru: "Borang tak masuk penuh dalam bingkai."

  - **Pengajaran:** gejala sama ("markah salah") boleh ada 2 punca berbeza (geometri + threshold). Kumpul data brightness sebenar → buktikan punca, bukan teka.
  - **Pending:** master nak test di staging (scan penuh + sengaja terpotong). Kalau OK → merge ke main.

### Session Recap (Lama)

- **Sesi 2026-06-12 petang/malam: iPad Bug Fixes — LIVE PRODUCTION** ✅

  1. **Dropdown race condition (ujian.html + laporan-ujian.html):**
     - `selUjian` disabled + "Memuatkan..." semasa `init()` fetch API
     - Loading feedback "Memuatkan..." pada dependent dropdowns semasa `onUjianChange()`
     - Root cause: iPad user tap dropdown sebelum `init()` siap load options

  2. **Sidebar logout button tidak kelihatan pada iPad:**
     - Fix 1: `.sidebar` height → `height:100vh; height:100dvh` (iOS 100vh bug)
     - Fix 2: `#sidebar` → `height:100dvh` (outer container masih 100vh)
     - Fix 3: `.sidebar-nav` tambah `min-height:0` (iOS flex child refuse shrink bug)
     - Fix 4: `.sidebar-logout` → `padding-bottom: env(safe-area-inset-bottom)` (iPad no home button)
     - Fix 5: Button visibility naik — opacity 0.6→0.85, border 0.15→0.3, background subtle
     - Root cause chain: iOS 100vh != visible area, flex min-height bug, button faint on touch device

  3. **Confirmed working on iPad by master** ✅
  4. **Playwright test:** 8/8 PASS (TEST_USER=test TEST_PASSWORD=test123)
  4. **Commits test:** `7aab1f3` → `64507f3` → `2dfaca4` → `59015df` → `d1292b2`
  5. **Merge:** test → main `ad1015e`
  6. **Deploy production:** Version `9591deac-6627-484b-8efe-8fe764b7b925`

- **Sesi 2026-06-12 pagi: OMR Scanner — UI Polish, Bug Fixes, Merge Production** ✅ LIVE PRODUCTION
  - Template, UI, bug fixes (anchor dark detect, duplicate const grid, mobile navbar)
  - Merge test→main `83919b7` — LIVE production
  - Playwright 9/9 PASS

- **Sesi 2026-06-11 tengah hari: OMR Scanner — Execute 6 tasks** ✅ LIVE PRODUCTION

- **Sesi 2026-06-10 petang: Feature Auto-Tutup Ujian — LIVE PRODUCTION** ✅
  - Migration 024, commit `dcc1cbe`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit main: `ad1015e` (merge — iPad bug fixes live)
- Latest commit test: `d1292b2` (comprehensive iOS sidebar fix)
- Playwright test credentials: TEST_USER=test TEST_PASSWORD=test123
- Migration 024 applied ke staging + production ✅
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- CSS version: app.css?v=6
- Production version ID: `9591deac-6627-484b-8efe-8fe764b7b925`

### iPad Bug Notes (untuk rujukan masa depan)
- iOS `100vh` ≠ visible viewport — guna `100dvh` untuk fixed elements
- iOS flex children perlu `min-height:0` untuk scroll properly dalam flex column
- iOS touch device tiada `:hover` — jangan design UI yang bergantung pada hover untuk visibility
- `env(safe-area-inset-bottom)` untuk iPad/iPhone tanpa home button
