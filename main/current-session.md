# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: brainstorm architectural (hosting) → bounded (redesign UI) — projek `certificate-generator` (2026-09-27 malam → 2026-09-28 pagi)
**Current Project**: `certificate-generator` (baharu — standalone, berasingan dari `mypwa-v2`)
**Status**: 🟢 LIVE https://syazwanbmw-dev.github.io/Certificate-Generator/ — hosting + redesign UI dua-dua siap dan deployed.

## Working Memory

### Sesi ni
1. Master minta host GitHub repo `Kiyoraka/Certificate-Generator` standalone (rujukan: pernah masukkan versi serupa dalam `mypwa-v2` sebagai `sijil.html` haritu 2026-05-24).
2. Brainstorming architectural: fork sedia ada (dari 2026-05-24) dikesan → clone local → enable GitHub Pages (`build_type: workflow`) → trigger deploy pertama. Keputusan: GitHub Pages (bukan Cloudflare) sebab master eksplisit minta ".io" dan projek 100% static.
3. Tulis `CLAUDE.md` + `MEMORY.md` dalam repo (ikut konvensyen wajib setiap projek — [[feedback_memory_per_projek]] di Claude auto-memory).
4. Master minta redesign UI ("nampak canggih") — brainstorming bounded: tawar 3 arah dengan preview ASCII konkrit (bukan jargon teknikal). Master pilih "Studio Cetak Moden" (gelap+emas) mula-mula, tukar fikiran ke **"Ruang Kerja Bersih"** (putih+emerald) selepas dibanding — sesuai sebab tool dibuka lama untuk isi banyak nama, latar terang kurang penat mata.
5. Implement: CSS token system (`--bg #f7f8f9 --accent #0f766e`), font Inter+Manrope (Google Fonts CDN), redesign semua komponen (upload area, tabs jadi underline, slider custom thumb, button states). CSS-only, tiada perubahan struktur HTML/JS.
6. Verify visual guna local static server sementara (`node -e` http server port 8791) + Chrome extension screenshot — semua state (upload, tabs, slider, names list) nampak konsisten sebelum push.
7. **Gotcha ditemui:** push ke `main` TAK auto-trigger GitHub Actions pada fork ni (`total_count` kekal 1 selepas 2 push). Fix: `gh workflow run static.yml` manual selepas setiap push untuk deploy versi terkini.
8. Commit + push dua kali (CLAUDE.md/MEMORY.md, kemudian redesign) + manual workflow_dispatch dua kali untuk deploy.

### Pattern berkesan (rujukan sesi depan)
- Soalan reka bentuk guna preview ASCII konkrit dalam `AskUserQuestion` (bukan istilah teknikal macam "glassmorphism") — master boleh banding terus dan tukar fikiran dengan mudah ("kalau clean?").
- Verify visual browser SEBELUM push — tangkap isu (kalau ada) sebelum ia live, bukan lepas.
- Fork GitHub — kalau workflow lama tak pernah register/trigger, jangan andai push akan jalan; verify `actions/runs` total_count lepas push, guna `workflow_dispatch` sebagai fallback reliable.

## Session Recap (For AI Restart)
- `certificate-generator` SIAP: hosting standalone (GitHub Pages) + redesign UI "Ruang Kerja Bersih" LIVE. Tiada backlog terbuka.
- Projek ni **berasingan sepenuhnya** dari `mypwa-v2` — jangan gabung/rujuk balik masa depan melainkan diminta.
- Reminder operasi: repo ni **push tak auto-deploy** — mesti `gh workflow run static.yml --repo syazwanbmw-dev/Certificate-Generator` manual lepas setiap push untuk site update.
- Konteks projek lain — rujuk `MEMORY.md`/fail projek masing-masing, sesi ni fokus certificate-generator sahaja.

---
*Session updated: 2026-09-28 ~06:15 (certificate-generator standalone + redesign LIVE)*
