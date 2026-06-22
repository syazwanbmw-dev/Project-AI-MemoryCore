# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-06-22
**Session Focus**: mypwa-v2 — Drag susun kedudukan dokumen ✅ LIVE PRODUCTION (merge main 8a4cc86) + CI auto-deploy production FIXED

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-22 tengah hari: mypwa-v2 — Drag Susun Kedudukan Dokumen** ✅ SEALED, PUSHED test, staging live. PENDING master verify → merge main.

  Master nak admin boleh drag-and-drop susun kedudukan dokumen dalam Tab Dokumen (admin sahaja); susunan terpakai untuk paparan guru. Pipeline Kata penuh (brainstorm→spec→plan→subagent-driven→sight-hone/review→commit-seal→push). KISS & DRY.

  - **Konteks penting:** "Tab Dokumen" = table `panduan` (id, nama, url, created_at). Admin urus di `admin.html` tab "Dokumen", guru tengok di `panduan.html`. Fail/route kekal nama `panduan` (bukan `dokumen`).
  - **Keputusan reka bentuk (master pilih):** (1) buang pagination di admin (senarai penuh, drag bebas), (2) drag-and-drop native HTML5 (desktop), (3) guru view kekal pagination — auto ikut susunan baru. Tiada audit log untuk reorder.
  - **Migration 025** (`025_panduan_urutan.sql`): `ALTER TABLE panduan ADD COLUMN urutan INTEGER` + backfill correlated-COUNT ikut `nama ASC` (preserve paparan). ⚠️ Framework `migrations apply` GAGAL pada staging (024 tak tracked, duplicate column tarikh_tutup) → apply terus guna `d1 execute --file`. Sama isu akan berlaku utk production — apply 025 guna `d1 execute` masa merge main.
  - **Backend** (`src/routes/panduan.js`): GET `ORDER BY urutan ASC, id ASC` + sokong `?all=1` (admin-gated, semua rekod tanpa LIMIT); POST set `urutan = MAX+1`; endpoint baru `PUT /api/panduan/urutan` (adminOnly, body `{ids:[]}`, validate Number.isInteger, `db.batch()`) — WAJIB daftar SEBELUM `PUT /:id`.
  - **Frontend admin** (`public/admin.html`): buang pagination (#panduanAdminPg, _panduanPage), `loadPanduanAdmin()` no-arg guna `?all=1`, baris `draggable` + handle ≡, fungsi `initPanduanDrag/renumberPanduan/saveUrutanPanduan`. Guru `panduan.html` tiada perubahan.
  - **Review (Opus) finding Important:** `?all=1` asalnya tak admin-gated → Lucy tambah guard `c.get('user').role !== 'ADMIN' → 403` (commit f9771e4). Minor (drag-luar-table tak persist, test smoke-only, whole-row draggable) — deferred, acceptable.
  - **Test:** `tests/dokumen.spec.js` baru (assert draggable + handle ≡ + #panduanAdminPg count 0). Suite penuh 9/9 PASS lawan staging (a11y flake ERR_NETWORK_CHANGED sekali, re-run hijau).
  - **Credentials staging admin:** `admin` / `fcoy4994` (sama dengan production).
  - **Spec:** `docs/superpowers/specs/2026-06-22-drag-susun-dokumen-design.md`. **Plan:** `docs/superpowers/plans/2026-06-22-drag-susun-dokumen.md`.
  - **Commits test:** `6ad926e`→`f9771e4`. Merge main = `a953d20` (feature) → `8a4cc86` (ci fix).
  - **✅ DEPLOY PRODUCTION (master confirm browser):** migration 025 applied prod DB (`d1 execute`, 9 dokumen backfill 1-9). Merge main + deploy production manual (wrangler, Version b5a8e426). Feature DISAHKAN LIVE oleh master dalam browser (erpm-sksalor.celikguru.my). ⚠️ Mesin master tak boleh curl/Playwright ke worker semasa sesi (HTTP 000/SSL err — rangkaian local flapping); verify via wrangler API + browser master sahaja.
  - **✅ CI FIX (penting — pengajaran):** GitHub Actions auto-deploy PRODUCTION gagal (staging OK). Punca: (1) secret `CLOUDFLARE_API_TOKEN` lama kurang permission **Zone: Workers Routes Edit + Zone Read** untuk `celikguru.my` — production ada custom route `erpm-sksalor.celikguru.my/*`, staging tiada route jadi tak terjejas; (2) `cloudflare/wrangler-action@v3` telan output error. **Fix:** master jana token baru guna template "Edit Cloudflare Workers" + include zone celikguru.my; tukar step production `deploy.yml` dari wrangler-action → `run: npx wrangler deploy --env production` (env CLOUDFLARE_API_TOKEN+ACCOUNT_ID) supaya error nampak penuh. Hasil: run main `8a4cc86` HIJAU. Push main akan datang auto-deploy production OK.

- **Sesi 2026-06-21 petang: sistem-olahraga — EXECUTE Reset Password Sekolah** ✅ KOD SIAP, SEALED, DEPLOY STAGING(-test). PENDING master verify manual → merge main

  Master pilih execute plan reset password sub_admin. Semua 5 task siap:
  - **Backend** (`src/index.js` selepas line 2952): `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password`. Guard 3 lapis (id_pengguna+id_sekolah+peranan='sub_admin') dalam SELECT & UPDATE; validasi min 6 aksara (FE+BE); hash PBKDF2; GET tak dedah hash. Commit `1ccbe72`.
  - **Frontend**: seksyen "Reset Password Sub Admin" dalam modal Urus (`superadmin.html`) + logik `muatSubAdmin`/handler reset+toggle 👁 (`superadmin.js`). ID sebenar: `#urus-pilih-subadmin`, `#urus-password-baharu`, `#btn-toggle-password`, `#btn-reset-password-subadmin`. Commit `3ae75be`.
  - **Ujian**: test 16 ditambah dalam `tests/e2e.spec.js` (gitignored — lokal sahaja, TAK masuk repo). Guna `16` sebab `07` dah dipakai. Assertion utama = kewujudan elemen (deterministik).
  - **Pipeline**: sight-hone CLEAR → commit-seal SEALED (17 ujian sedia ada PASS, wrangler dry-run clean) → push test `3ae75be`.
  - **⚠️ PENEMUAN ENV (penting):** CI deploy push `test` → worker **`sistem-olahraga-sekolah-test`** (db `olahraga-test`). Kod baru DAH live di situ (marker disahkan). TAPI `BASE_URL` ujian + login `dragon`/`f4994` hanya jalan di worker **top-level** `sistem-olahraga-sekolah` (= production, db `olahraga-db`, domain `atletik.celikguru.my`). Db `olahraga-test` nampaknya tiada seed superadmin → ujian automatik & verify tak boleh jalan di `-test`. Ujian 16 'gagal' bukan sebab bug — sebab poll worker salah.
  - **Staging -test URL:** https://sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev
  - **✅ SEED + VERIFY (2026-06-21 petang lewat):** db `olahraga-test` tiada superadmin (sa_cred=0) → Lucy seed `superadmin_credentials` (id=1, username='dragon', password=hash PBKDF2 'f4994') via `wrangler d1 execute olahraga-test --remote`. Hash dijana guna Node webcrypto (params sama: salt 16B, 100000 iter, SHA-256, format `pbkdf2:salt:hash`). Login `-test` kini BERJAYA (peranan super_admin).
  - **VERIFIKASI PENUH lawan -test:** (1) Test 16 Playwright PASS. (2) E2E API: GET sub-admin→reset→login password baru `success=True peranan=sub_admin` (sekolah aktif NBA3003). (3) Guard 404 disahkan utk id bukan sub_admin. Feature FUNCTIONAL end-to-end.
  - **⚠️ DATA TEST DIUBAH semasa verify:** password 2 sub_admin di olahraga-test kini = `ujian123` → `nba3003` (NBA3003, aktif) & `xba3202` (XBA3203, tak aktif). Master boleh reset balik kalau perlu.
  - **Cadangan Lucy:** BASE_URL ujian patut tuding ke worker `-test` (bukan top-level=production) untuk CI hygiene. Sekarang dikekalkan asal (top-level) — keputusan master. olahraga-test = 7 sekolah, 7 sub_admin.
  - **✅ MERGE MAIN (master confirm):** merge test→main `8c5296c` (4 commit: spec, plan, backend, frontend). Push main → GitHub Actions deploy production. DISAHKAN LIVE: marker `urus-pilih-subadmin` ada di workers.dev + `atletik.celikguru.my`; endpoint GET+PATCH pulang 401 (terdaftar, perlu auth). FEATURE LIVE PRODUCTION.
  - **Main commit baru:** `8c5296c` (lama: `1be6002`).
  - **✅ SECURITY REVIEW + FIX (2026-06-21 petang, main `2ae8785`):** master minta semak credential hardcoded terdedah di frontend.
    - Imbasan `public/`: TIADA API key/JWT/token pihak ketiga. Jumpa 1 isu: `superadmin.html:144` ada `value="123456"` (default password lemah untuk daftar sub_admin, nampak dalam page source).
    - **Fix opsyen 1:** buang `value="123456"` + tambah `required minlength="6"` + placeholder; label "Kata Laluan Lalai"→"Awal" (commit `d295d6e`). Backend `POST /api/superadmin/admin` tambah validasi `kata_laluan.length<6 → 400` (commit `4ed2d97`) — sepadan endpoint reset.
    - **JWT_SECRET semakan:** DISET betul di production DAN env test (wrangler secret list) — bersama SUPERADMIN_USERNAME/PASSWORD. Fallback 'sila-tukar-secret-ini' tak pernah dipakai. SELAMAT.
    - **Disahkan LIVE production:** frontend 123456 hilang + minlength ada; backend password 5-aksara → HTTP 400. Merge main `2ae8785`.
  - **✅ FOLLOW-UP (2026-06-21 petang): 2 housekeeping selesai:**
    1. Password `nba3003` + `xba3202` (olahraga-test) di-standard ke `1234` (asal hashed, tak boleh pulih — guna konvensyen test projek). Login nba3003/1234 disahkan berjaya.
    2. `BASE_URL` ujian tukar ke worker `-test` (`sistem-olahraga-sekolah-test...`) untuk CI hygiene — fail `tests/e2e.spec.js` gitignored (lokal sahaja). Suite penuh 18/18 PASS lawan -test → admin_dba1097/1234, gb/1234 semua wujud di olahraga-test. -test kini staging berfungsi.

- **Sesi 2026-06-21 petang (awal): sistem-olahraga — Plan Reset Password Sekolah** ✅ SPEC + PLAN SIAP (kini DAH EXECUTE, lihat atas)

  Master perasan superadmin TAK BOLEH reset password akaun sekolah (sub_admin) bila admin sekolah lupa password. Gap sebenar SaaS multi-tenant — satu-satunya jalan sekarang = padam sekolah (hilang data) atau edit SQL manual.

  - **Keputusan reka bentuk (master pilih):** (1) superadmin taip password baru sendiri, (2) sub_admin sahaja, (3) dropdown senarai sub_admin (perlu endpoint GET).
  - **Design:** 2 endpoint baru dalam `src/index.js` — `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password` (guard 3 lapis: id_pengguna + id_sekolah + peranan='sub_admin'; hash PBKDF2; validasi min 6 aksara). UI dalam modal "Urus Sekolah" sedia ada (dropdown + password + toggle 👁).
  - **Prinsip:** KISS & DRY — guna corak sedia ada (`apiFetch`, `hashPassword`, `serverError`, `selectedTenantId`). Tiada migration/package baru.
  - **Spec:** `docs/superpowers/specs/2026-06-21-reset-password-sekolah-design.md` (commit `fa51bd3`)
  - **Plan:** `docs/superpowers/plans/2026-06-21-reset-password-sekolah.md` (commit `dfa545f`) — 5 task bite-sized.
  - **NEXT:** master pilih cara execute (subagent-driven disyorkan / inline). Branch `test`.
  - **Nota repo:** ujian E2E lawan staging (BASE_URL=staging worker, superadmin `dragon`/`f4994`), bukan localhost. Superadmin boleh ada >1 sub_admin per sekolah.

- **Sesi 2026-06-21 tengah hari: Masa Log Audit → format 24-jam** ✅ LIVE PRODUCTION (merge main `0ff490a`)

  Sambungan dari fix timezone semalam. Master nak masa dipapar dalam sistem 24-jam (bukan PG/PTG).

  - **Fix (`admin.html:1484`):** tambah `hour12:false` dalam options `toLocaleString`. Display-only — logik penukaran UTC→MYT (`replace(' ','T') + 'Z'`) kekal tak berubah.
  - **Kesan:** `11:41 PG` → `11:41` · `4:45 PTG` → `16:45` · tengah malam → `00:xx`.
  - **Pengajaran:** `toLocaleString('ms-MY', ...)` default 12-jam (PG/PTG). Untuk 24-jam guna `hour12:false`. Format paparan terasing dari logik pengiraan masa.
  - Pipeline: Code → Playwright 8/8 PASS → commit-seal (wrangler dry-run CLEAN) → push test `e452827` → master confirm staging → merge main `0ff490a`.
  - **Backlog dicadang:** audit tempat lain yang papar `created_at` — mungkin ada bug timezone/format serupa (BELUM buat).

- **Sesi 2026-06-21 malam: Fix Masa Log Audit → MYT** ✅ LIVE PRODUCTION (merge main `230dfb0`)

  Masalah: master perasan masa log masuk dalam Log Audit (admin.html) salah — tersasar 8 jam.

  - **Punca:** SQLite `CURRENT_TIMESTAMP` simpan UTC format `"YYYY-MM-DD HH:MM:SS"` (guna space, tiada `Z`/penanda zon). String non-ISO macam ni ditafsir `new Date()` sebagai waktu TEMPATAN, bukan UTC. Jadi walaupun `toLocaleString` dah ada `timeZone:'Asia/Kuala_Lumpur'`, input ke `new Date()` dah salah sebelum penukaran zon — hasilnya papar nilai UTC mentah.
  - **Fix (`admin.html:1484`):** `new Date(row.created_at.replace(' ', 'T') + 'Z')` — normalize ke ISO UTC dulu (`"2026-06-20T16:22:00Z"`), baru `toLocaleString` tukar +8 ke MYT betul.
  - **Bukti:** 16:22 UTC → sebelum fix papar 04:22 PTG (salah), selepas fix papar 12:22 PG = 00:22 MYT (betul).
  - **Display-only:** DB tak diubah, data audit lama terus betul, tiada migration.
  - **Pengajaran:** `new Date()` pada string SQLite UTC (space-separated, tiada Z) = ditafsir local time. Sentiasa normalize ke ISO+`Z` sebelum tukar zon. Pattern ni mungkin ada di tempat lain yang papar `created_at` — patut audit masa depan.
  - Commits: `acbb3ac` (test) → merge main `230dfb0`. Playwright 8/8 PASS, wrangler dry-run CLEAN.

### Session Recap (Lama)

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

  4. **Bingkai panduan nisbah A4** (commit `4f5daf9`):
     - Master perasan bingkai putus-putus terlalu panjang utk A4. Punca: `#guideBox` guna 80%x80% saiz video → nisbah ikut kamera.
     - Fix: CSS `aspect-ratio:210/297` + center + max-width 90%. Visual sahaja, detection tetap scan seluruh frame.

  - **Pengajaran:** gejala sama ("markah salah") boleh ada 2 punca berbeza (geometri + threshold). Kumpul data brightness sebenar → buktikan punca, bukan teka.
  - **Status:** SEMUA 3 fix dah merge main `7d1755c` → LIVE PRODUCTION. Anchor tak perlu dalam bingkai putus-putus (scan seluruh frame); bingkai = panduan zon selamat sahaja.
  - **Detail penuh:** mypwa-v2/CLAUDE.md → section "OMR Scanner — Bug Fixes Detection (2026-06-15)".

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
- Latest commit main: `0ff490a` (merge — Log Audit 24-jam live)
- Latest commit test: `e452827` (Log Audit 24-jam)
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
