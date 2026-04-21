---
name: Projek Sprint
description: Sistem pengurusan kejohanan olahraga peringkat PPD/JPN — multi-tenant SaaS
type: project
---

# Projek Sprint — Sistem Kejohanan Olahraga PPD/JPN

**Repo:** `C:\Users\user\Documents\code\sprint`
**Domain:** `sprint.celikguru.my`
**Status:** Dalam perancangan — plan selesai, belum mula code
**Plan:** `docs/plans/sprint-mvp.md`

## Tech Stack
- Backend: Hono.js + Cloudflare Workers
- Database: Cloudflare D1 (SQLite)
- Frontend: Vanilla HTML/JS + Tailwind CSS
- Version Control: Git → test branch → auto-deploy Cloudflare

## Konsep
Sistem pengurusan kejohanan olahraga peringkat PPD (inter-sekolah) dan JPN (inter-PPD).
Lebih besar dari sistem-olahraga-sekolah — fokus kepada multi-tenant, bukan satu sekolah sahaja.
Target market: PPD dan JPN seluruh Malaysia. Produk berbayar (subscription), payment gateway Phase 2.

## Hierarki Peranan
```
master (provider/Lucy's master)
  └── host (PPD atau JPN) — urus kejohanan
        └── admin (sekolah jika PPD, PPD jika JPN) — daftar peserta
              └── hakim — input keputusan
                    └── public — baca sahaja
```

## Features MVP (Phase 1)
- Multi-tenant: setiap PPD/JPN adalah tenant berasingan
- Competition setup: kumpulan umur + acara configurable (host yang tetapkan)
- Jenis acara: `track`, `field`, `relay`, `merentas_desa`
- Pendaftaran atlet: Borang M01 (menengah) / R01 (rendah)
- Pendaftaran pegawai: Borang P01
- View kontijen: Borang P02
- Carian by MyKAD (hash di browser sebelum hantar ke API)
- Input keputusan hakim by nombor bib
- Public results page (live, auto-refresh)
- Laporan 4 jenis: sekolah, acara, kategori umur, jenis sekolah
- Sijil PDF generate dengan logo tenant
- Tukar kata laluan (semua role)
- Subscription tier flag (trial/active/expired) — payment gateway Phase 2

## Features Phase 2 (Kemudian)
- JPN escalation: data PPD naik ke JPN, atlet boleh tambah acara
- Payment gateway / billing
- Email notifications

## Database Tables
tenants, users, schools, competitions, age_categories, events,
athletes, competition_athletes, event_entries, officials, teams,
team_members, results, sessions

## Security Penting
- MyKAD: AES-256-GCM encrypt dalam DB (`ic_encrypted`), SHA-256+salt hash untuk lookup (`ic_hash`)
- Secrets: `ENCRYPTION_KEY` + `IC_SALT` via Cloudflare Workers secrets (BUKAN dalam kod)
- Frontend: tiada API keys, tiada plain IC melintas network
- CORS: allow sprint.celikguru.my sahaja
- `.dev.vars` dalam `.gitignore` — tidak upload ke GitHub

## Reference System
Rujukan: `bsukanspmoe.com/ekejohanan` (MSSM e-Kejohanan)
Gambar rujukan: `D:\Download\ekejohanan\` (page1.jpg hingga page6-4.jpg)

**Why:** Sistem ini lebih besar dari sistem-olahraga-sekolah. Multi-tenant bermakna satu deployment boleh serve ratusan PPD/JPN. Data atlet persistent dan boleh naik ke peringkat lebih tinggi.
**How to apply:** Selalu enforce tenant isolation dalam setiap query. MyKAD handling mesti ikut protokol security yang ditetapkan. Jangan hardcode secret.
