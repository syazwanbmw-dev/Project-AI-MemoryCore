# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-24
**Session Focus**: mypwa-v2 — Jana Sijil (Certificate Generator) integrated, deployed staging

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 malam: Auto Drive Folder per Sesi — LIVE production** ✅

  1. Migration 022 — `drive_folder_id TEXT` pada table sesi
  2. `sesi.js` POST — auto-create Drive folder via AppScript (fail silently)
  3. `pajsk.js` — DRY helper `getSesiFolderId()` + update upload & edit (null-safe `??`)
  4. `sesi.js` DELETE — auto-padam folder Drive (`deleteFolder` + `waitUntil`)
  5. AppScript — tambah `createFolder` + `deleteFolder` actions
  6. Merged ke main — live production `erpm-sksalor.celikguru.my` (commit `c7a6186`)

- **Sesi 2026-05-20 petang: PWA fix — manifest dinamik + icon eN — LIVE production** ✅

  1. `GET /manifest.json` route dalam Worker — public, no auth, baca `hero_badge` dari D1
  2. `name`/`short_name` = "eNilai" (fixed), `description` = hero_badge value (dinamik dari pillbox admin)
  3. `public/manifest.json` DELETED — supaya Cloudflare routing fall-through ke Worker
  4. `icon-192.png` + `icon-512.png` — regenerate dari `favicon.svg` eN menggunakan Playwright
  5. `scripts/generate-icons.js` — skrip jana icon PNG dari SVG (Playwright)
  6. Merged ke main — live production (commit `2a69cbb`)

- **Sesi 2026-05-20 malam: Tab Dokumen — LIVE production** ✅

  1. Migration 023 — `CREATE TABLE panduan (id, nama, url, created_at)` — applied staging
  2. `src/routes/panduan.js` — GET paginated + POST/PUT/DELETE admin-only
  3. `src/index.js` — mount `/api/panduan` routes
  4. `public/panduan.html` — halaman guru, pagination 10/page, XSS-safe
  5. `public/app.js` — tambah link Dokumen dalam guruLinks
  6. `public/admin.html` — tab Dokumen, CRUD + pagination
  7. Migration 023 applied production DB, deployed production (commit `0c35a8e`)

- **Sesi 2026-05-24 pagi: Jana Sijil — integrated ke mypwa-v2, staging** ⏳ (tunggu confirm sebelum merge ke main)

  1. `src/routes/murid.js` — tambah `GET /api/murid/search?q=` endpoint (LIKE search, JOIN kelas, LIMIT 20)
  2. `public/sijil.html` — halaman Jana Sijil baru:
     - Vue.js 2 CDN (reactive UI certificate generator)
     - Upload template sijil (imej PNG/JPG) — preview dalam upload zone
     - Tetapan medan teks: tab Nama/IC/Extra, slider X/Y/font size/warna
     - Upload font custom (.ttf/.otf/.woff)
     - Pratonton live dengan overlay teks pada template
     - **2-kolum layout** pada desktop (≥992px) — setup kiri, pratonton sticky kanan
     - Cari murid dari DB (autocomplete, debounce 300ms, call `/api/murid/search`)
     - Import CSV + download contoh CSV
     - Jana sijil → `window.print()` (auto detect landscape/portrait)
  3. `public/app.js` — tambah `{ href: '/sijil.html', label: 'Jana Sijil' }` dalam guruLinks
  4. Tiada migration DB — pure client-side certificate generator
  5. Commits: `c422abb` → `11ab5bf` → `40a683d` → `dc0f4ab` → `bd1a830` → `9bf69e6`
  6. Latest commit test branch: `9bf69e6`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PWA manifest dinamik + icon eN | ✅ LIVE | Merged main 2026-05-20, commit 2a69cbb |
| Tab Dokumen | ✅ LIVE | Migration 023 applied, deployed production (commit `0c35a8e`) |
| Jana Sijil | ⏳ STAGING | Test di staging, confirm OK baru merge ke main |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit test branch: `9bf69e6` (fix placeholder + bold label)
- Latest commit main branch: `0c35a8e` (Tab Dokumen)
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- AppScript file: `docs/appscript/pajsk-upload.gs` — 4 actions: upload, delete, createFolder, deleteFolder
- PWA icon regenerate: jalankan `node scripts/generate-icons.js` setiap kali favicon.svg berubah
- Jana Sijil: Vue.js 2 CDN, tiada migration, auth via requireAuth() + renderSidebar()
