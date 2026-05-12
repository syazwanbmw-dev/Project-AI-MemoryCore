# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-12
**Session Focus**: mypwa-v2 — Bug fixes + Feature Muat Turun Senarai Murid

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-12 petang/malam: 2 bug fix + 1 feature baru, semua merged ke main** ✅

  1. **Bug Fix: Normalize Paparan Jantina** (`admin.html`, `murid.js`) — merged main ✅
     - DB ada data campuran: import lama simpan 'Perempuan', rekod manual simpan 'P'
     - Tambah helper `normJantina()` dalam admin.html UTILS section
     - Display badge normalize semua variasi → papar 'Lelaki'/'Perempuan' penuh
     - Import bulk (frontend + backend) normalize ke 'L'/'P' sebelum simpan
     - Commit: `485f94e`

  2. **Feature: Muat Turun Senarai Murid PDF** (`tetapan.html`, `app.css`) — merged main ✅
     - Button hijau "Muat Turun" di sebelah "Padam" dalam tab Kelas Saya
     - Tambah `btn-success` CSS class dalam app.css
     - Modal pilih orientasi Portrait/Landscape
     - PDF layout: header (logo 45px + nama sekolah + tagline), nama kelas, table (Bil/Nama/No.Kad/Jantina + 6 kolum kosong), footer stats (Lelaki/Perempuan/Jumlah)
     - Jantina papar L/P (singkatan) dalam PDF
     - Layout responsive ikut orientasi — portrait vs landscape ada saiz kolum berbeza
     - Bug kritikal dijumpai via sight-hunt: `</script>` dalam template literal menutup outer script tag — init() tidak pernah dijalankan → loading spinner kekal. Fix: pindah ke `<body onload="...">`
     - Commits: `f66e4bf`, `ee7d82a`, `eee0ebd`, `009300a`, `d68f5fa`, `44b7d46`, `a1f9238`, `58bc0e6`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman cetak |
| LinkedIn setup + dokumentasi journey | ⏳ PENDING | Untuk build portfolio & trust bagi training |

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- mypwa-v2 staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Production URL: `https://mypwa-v2.syazwan-skpp82.workers.dev` / `erpm-sksalor.celikguru.my`
- Untuk set wrangler secrets: WAJIB guna Bash `printf 'nilai' | npx wrangler secret put KEY --env production`
- Deploy production: merge test → main (CI auto-deploy via GitHub Actions)
- test branch = staging auto-deploy via CI
- Apps Script perlu redeploy manual bila ada perubahan pada pajsk-upload.gs
- PENTING: Jangan letak `</script>` dalam template literal JS yang berada dalam `<script>` block HTML — guna `<body onload>` atau split string

### Commits Sesi Ini (2026-05-12)
| Commit | Perubahan | Status |
|--------|-----------|--------|
| `485f94e` | fix: normalize paparan jantina murid dalam table admin | merged main |
| `f66e4bf` | feat: Muat Turun Senarai Murid PDF dalam tab Kelas Saya | merged main |
| `ee7d82a` | fix: buang </script> dalam template literal — init() tidak dipanggil | merged main |
| `eee0ebd` | fix: kolum kosong 0.6cm, padding td 3px | merged main |
| `009300a` | fix: kolum No. Kad 1.5cm | merged main |
| `d68f5fa` | fix: kolum kosong portrait 0.6cm, landscape 2cm; Nama responsive | merged main |
| `44b7d46` | fix: kolum Nama landscape 8cm | merged main |
| `a1f9238` | fix: jantina L/P, kolum Jantina portrait 1cm, Nama portrait 5.5cm | merged main |
| `58bc0e6` | fix: landscape margin 1cm, No. Kad landscape 2cm | merged main |
