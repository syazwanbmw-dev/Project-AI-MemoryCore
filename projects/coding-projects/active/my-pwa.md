# mypwa-v2 (eNilai)
*Coding Project - Created 2026-03-26 | Last Updated: 2026-05-07*

## Description
Sistem eNilai — platform pengurusan penilaian murid untuk sekolah rendah. Rebuild dari Supabase ke Cloudflare Workers + D1.

## Project Details
- **Type**: Coding Project
- **Status**: Active (Production Live)
- **Created**: 2026-03-26
- **Last Accessed**: 2026-05-07
- **Position**: #1
- **Repo**: https://github.com/syazwanbmw-dev/mypwa-v2.git
- **Production URL**: https://mypwa-v2.syazwan-skpp82.workers.dev
- **Custom Domain**: erpm-sksalor.celikguru.my
- **Staging URL**: https://mypwa-v2-staging.syazwan-skpp82.workers.dev

## Technical Stack
- **Backend**: Hono.js on Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite)
- **Frontend**: Vanilla HTML/JS + CSS custom (app.css)
- **Auth**: JWT (jose) + SHA-256
- **Deployment**: GitHub Actions CI — test → staging, main → production

## Fasa Selesai
| Fasa | Status |
|------|--------|
| Fasa 1-7 — Foundation → Deploy | ✅ Done |
| Fasa 8 — Ujian Dalaman | ✅ Done |
| Fasa 9 — Audit Log | ✅ Done |
| Sidebar Accordion + Active Tab | ✅ Done (2026-05-07) |

## Features Live (Production)
- Login (ADMIN + GURU roles)
- Dashboard — carta purata TP + ujian dalaman analisis
- Rekod RPM — input TP bulk per kelas/subjek
- Laporan RPM — pivot table + inline edit + cetak
- Ujian Dalaman — input markah + laporan + analisis taburan gred
- PAJSK — rekod pertandingan + upload PDF
- Admin Panel — pengguna, kelas, murid, subjek, kurikulum, tetapan, audit log
- Sidebar accordion (RPM + Ujian Dalaman groups)
- Active tab highlight — pill untuk sublink, bar+amber untuk regular link

## Sidebar Active Tab (selesai 2026-05-07)
- Root cause: Cloudflare strip `.html` dari URL → `location.pathname=/profil` vs `l.href=/profil.html`
- Fix: `normPath()` dalam `renderSidebar()` — strip `.html` sebelum compare
- Design: sublink aktif = amber pill (flush kiri, rounded kanan) + 5px bar; regular link = amber text + 5px bar

## Backlog
- [ ] Cetak tab Laporan PAJSK
- [ ] Compact mode Ujian Dalaman (bar 28px, 6 kad belum muat satu halaman)

## DB IDs
- Production: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging: `f87c8bbc-77a5-4d57-88d1-284265de437f`

## CSS Version
- Current: `app.css?v=6`

---
*mypwa-v2 | Position #1 | Last updated 2026-05-07*
