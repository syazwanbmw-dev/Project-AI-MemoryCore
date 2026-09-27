---
name: Certificate Generator (standalone)
description: Tool jana sijil pukal, 100% client-side, hosted standalone di GitHub Pages
type: project
---

# Certificate Generator — Standalone

**Repo lokal:** `C:\Users\user\Documents\code\certificate-generator`
**GitHub:** https://github.com/syazwanbmw-dev/Certificate-Generator (fork dari `Kiyoraka/Certificate-Generator`, remote `upstream`)
**Live:** https://syazwanbmw-dev.github.io/Certificate-Generator/

## Tech Stack
- Pure HTML5 + CSS3 + Vanilla JS (ES6 modules) — **tiada backend, tiada database**
- Vue.js 2.6.14 (CDN) untuk reactivity
- Hosting: **GitHub Pages** (bukan Cloudflare — projek 100% static, tak perlu Workers/D1/Hono)
- Deploy: GitHub Actions (`.github/workflows/static.yml`, warisan upstream) — **push tak auto-trigger pada fork**, kena `gh workflow run static.yml` manual setiap kali nak deploy

## Sejarah
- **2026-05-24** — Fork dibuat sebagai rujukan bina fitur "Jana Sijil" (`sijil.html`) dalam `mypwa-v2` (projek BERLAINAN, ada backend search murid + PDF.js — tiada kaitan dengan repo standalone ni)
- **2026-09-27/28** — Master minta host standalone berasingan dari mypwa-v2:
  1. Clone fork sedia ada, enable GitHub Pages (build type: workflow)
  2. Tambah `CLAUDE.md` + `MEMORY.md` dalam repo (ikut konvensyen wajib setiap projek)
  3. Redesign UI dari flat Bootstrap-blue lama → tema "Ruang Kerja Bersih" (putih + emerald `#0f766e` + font Inter/Manrope via Google Fonts CDN)

## Keputusan Design (brainstorming bounded, 2026-09-28)
Master pilih antara 3 arah: "Studio Cetak Moden" (gelap+emas), "Meja Sijil Elegan" (ivory+navy+emas), **"Ruang Kerja Bersih"** (putih+emerald, sans-serif) — pilih yang terakhir sebab tool ni dibuka lama untuk isi banyak nama, latar terang kurang penat mata berbanding gelap.

Token: `--bg:#f7f8f9 --surface:#fff --border:#e5e7eb --text:#1a1d23 --accent:#0f766e`. Font heading **Manrope**, body **Inter**. Tab position guna underline (bukan kotak folder-tab lama). Slider guna custom thumb warna accent. CSS-only — tiada perubahan struktur HTML/JS.

## Gotcha
- 🔴 **Workflow `.github/workflows/static.yml` TAK auto-register pada fork** sampai ada push pertama lepas GitHub Actions di-enable pada repo (`gh api -X POST .../pages -f build_type=workflow`). Lepas register, **push event pun tak reliably trigger run** — verify dengan `gh api repos/.../actions/runs --jq .total_count` lepas push; kalau stuck di run lama, `gh workflow run static.yml` manual untuk deploy versi terkini.
- Sample image dalam `assets/img/certificate-preview.png` (screenshot UI lama dari README) — kalau test upload guna fail ni, posisi overlay nampak pelik (bukan bug CSS, sebab bukan template sijil sebenar).

## Pending / Belum Buat
- [ ] Tiada backlog spesifik — site berfungsi ikut spec asal upstream + design baru

## Status Deploy
- [x] Live di GitHub Pages, deploy terakhir berjaya (design "Ruang Kerja Bersih") — verify visual di browser sebelum tutup sesi lain-lain

**Why:** Sijil generator standalone perlu wujud berasingan dari mypwa-v2 supaya boleh dikongsi/guna tanpa bergantung sistem sekolah (login, DB murid).
**How to apply:** Kalau sambung kerja UI/fitur di sini, ingat **push tak auto-deploy** — mesti `gh workflow run static.yml` lepas push untuk nampak perubahan live.
