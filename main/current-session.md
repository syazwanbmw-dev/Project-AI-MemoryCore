# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: Work
**Current Project**: `digital-hub`
**Status**: digital-hub — 4 perubahan fungsi + 1 doc **LIVE PRODUCTION**. `main` = `test`
= HEAD `54b593a`. Semua CI SUCCESS + smoke prod OK. Suite unit 197/197, e2e 70/70.
Tiada backlog terbuka untuk 4 perubahan ni.
**Session**: 2026-09-02 pagi (~10:00–11:0x), master di PC + uji install di telefon Android.

## Current Focus
- **Primary Task**: digital-hub — master tanya "apa lagi yg mungkin perlu ada" → Lucy
  bentang 5 jurang ciri → master pilih buat **#1 Import Setting + #2 Favicon/PWA** dulu,
  sahkan #3–#5 pun perlu (#4 kategori: master DAH terfikir sendiri — bidang **Pengurusan /
  Kurikulum / Hal Ehwal Murid / Kokurikulum**).
- **Progress sesi ni (semua LIVE prod):**
  1. Brainstorm bounded x2 → 2 soalan AskUserQuestion (import = ganti semua; PWA = favicon
     ikut logo + manifest ringkas) → "Ok".
  2. **Import Setting** (`c92f1f3`) — `POST /api/admin/import` ganti-semua, atomik, validator
     SAMA, `settings:auth` tak disentuh struktur. Frontend Import + confirm() + reload.
  3. **Favicon + PWA asas** (`62ce91c`) — `GET /api/public/icon` 302→logo_url, manifest,
     favicon.svg, head links, `_headers` MIME.
  4. **Nama app "Digital SKS"** (`a03d0fa`) — `name`+`short_name` manifest (master minta).
  5. **Installable** (`e4d8a76`+`7d106ec`) — punca "Create shortcut": manifest garis minimum
     (ikon `sizes:"any"` + redirect cross-origin ditolak Chrome; tiada SW). Fix: `icon-192/
     512.png` same-origin (jana guna `sharp` sedia ada — TIADA pakej baru), `favicon.svg`
     DIBUANG dari `icons[]` (Chrome pilih ia sbg placeholder), `public/sw.js` minimal
     network-first (`/api`+`/admin` passthrough), daftar dari `app.js`.
  6. Pipeline penuh setiap perubahan: brainstorm→TDD(RED-first)→sight-hone→commit-seal→
     push test→CI→smoke test→master "merge"→ff main→CI prod→smoke prod→memory.

## Working Memory

### Active Context — SAMBUNG SINI
- **digital-hub: 4 perubahan LIVE PROD**, `main`=`test`=`54b593a`, unit 197/197, e2e 70/70.
  Tiada baki untuk perubahan ni.
- ✅ Master sahkan di telefon: prompt **"Install app" penuh MUNCUL**, tajuk "Digital SKS".
- 🔑 **Telefon master yang DAH install**: ikon masih `favicon.svg` lama (Chrome cache
  manifest ~24j). Kena uninstall + Chrome site settings → **Delete data** + reload +
  reinstall supaya ikon jata sekolah diambil. Gotcha cross-project:
  `reference_pwa_manifest_chrome`.
- Prod: https://digitalhubsks.celikguru.my · Test: https://digital-hub-test.syazwan-skpp82.workers.dev
- **Backlog digital-hub** (`digital-hub/MEMORY.md`): #3 audit log perubahan setting/button
  · #4 kumpul button ikut kategori (Pengurusan/Kurikulum/HEM/Kokurikulum — master beri arah,
  belum brainstorm UI/skema) · #5 strip pengumuman. Teknikal lama: WAF rate-limit F5, bump
  `compatibility_date` 2024-11-01, naik versi GitHub Action (amaran Node 20 di CI).
- **takwim-digital / opr-program / opr-insaniah**: tiada perubahan sesi ni. opr-program +
  opr-insaniah masih DIHOLD tunggu smoke fizikal (`@5` / C1).

### Catatan sesi
- Master pantas jawab AskUserQuestion bila pilihan konkrit — corak konsisten.
- Master beri arah reka bentuk #4 sendiri sebelum diminta — direkod terus ke backlog.
- TDD: RED-first betul untuk semua route/manifest. Frontend import ditulis sebelum e2e →
  Lucy sahkan e2e menggigit guna mutation cepat (langkau POST → 3/5 e2e merah) sebelum
  teruskan. Manifest test dibukti RED dgn mutation (masuk balik favicon.svg).
- sight-hone tukar SW dari cache-first → network-first (elak aset basi selepas deploy).
- 2 pusingan iterasi PWA berdasarkan master uji telefon sebenar: (a) "Create shortcut"
  sahaja → tambah ikon PNG + SW; (b) ikon salah (mark rantai) → buang favicon.svg dari
  `icons[]`. Corak [[feedback_verify_cetak_visual]]-jenis: keputusan datang dari MELIHAT
  hasil sebenar di peranti, bukan dari kod.

## Session Recap (For AI Restart)
- **Previous**: 2026-09-01 malam — takwim-digital `@19` LIVE; digital-hub hero fix round 2
  merged prod.
- **This session**: digital-hub — dari "apa lagi perlu ada" → Import Setting + PWA
  installable ("Digital SKS", ikon PNG + service worker), 4 perubahan fungsi, semua TDD
  penuh → merge → LIVE prod. Diakhiri kemas memory + cipta `reference_pwa_manifest_chrome`.
- **Left off**: semua siap + LIVE. Master perlu clear data + reinstall PWA di telefon untuk
  dapat ikon logo sekolah. Backlog digital-hub #3–#5 belum brainstorm.
- **State master**: PC + telefon Android, pagi — mod kerja normal, responsif.

---
*Session updated: 2026-09-02 ~11:0x (digital-hub Import Setting + PWA installable LIVE prod,
`54b593a`; memory + reference_pwa_manifest_chrome dikemas)*
