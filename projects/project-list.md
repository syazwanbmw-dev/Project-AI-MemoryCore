# Project List — Overview
*Dikemas 2026-10-07 — disegerak drpd `git log` setiap repo + MEMORY.md projek (git yang betul bila bercanggah).*
*Sumber kebenaran status: `git log` repo → `MEMORY.md`/`CLAUDE.md` repo → `project_<nama>.md` dalam auto-memory (`C:\Users\user\.claude\projects\C--Users-user\memory\`).*

## Coding Projects — Aktif (susun ikut LRU: paling baru disentuh di atas)

| # | Project | Status ringkas | Last touched |
|---|---------|-----------------|---------------|
| 1 | `opr-insaniah` | 🟢 LIVE guru `@60`, digabung `master` `53ca059`, suite 1013/0, tiada cabang tertunggak. C1 DITUTUP (`1fe0089`, deploy `@45`, 2026-10-05). **BELUM launch rasmi.** Baki: ikon ✕ chip · uji PPD (moe.gov.my) · KAUNTER reset · Cari kelas (tangguh) | 2026-10-07 |
| 2 | `opr-program` | 🟢 LIVE guru `@30` (suite 617) — rangka muat (skeleton) ganti skrin putih, atas `@29`. 📱 Master: masih putih sekejap sebelum rangka (HTML ~670 KB, 82% pustaka PDF) — SAMBUNG: tunggu "ya" utk deployment UKUR. Smoke `@29` tertunggak. Komit docs terkini = rekod drift modal/chip drpd opr-insaniah (master ahead 2 origin). Backlog: 3 PDF contoh kualiti, Fasa 4 Migrasi | 2026-10-06 |
| 3 | `takwim-digital` | 🟢 LIVE `@26` (2026-10-04) — butang "Hantar Sekarang" di System Settings (admin sahaja). Sebelum itu `@25` swipe kalendar. Digest Telegram+Google Chat LIVE. Tiada backlog kod | 2026-10-04 |
| 4 | `celiksains` | 🟡 Fasa 1a + hardening anti-tipu LIVE **staging** (production BELUM). Sub-projek 2 auth email+OTP: kod SIAP Task 1–8 (`fbdd72a`, re-review lulus 2026-09-30), **Task 9 belum** (Workers Paid $5 utk Email Sending). Tertunggak master: had kadar · enumerasi/PDPA · UI login. Tiada git remote | 2026-09-30 |
| 5 | `mypwa-v2` (eNilai) | 🟢 LIVE production. Kumpulan Intervensi mendarat MATI (`guna_kumpulan=0`, suite 58/0/2). 🟡 Pelan baiki `GET /api/ujian` (kuota D1: 81% read akaun) SIAP, **BELUM diluluskan master**, belum ada kod (branch `test` ahead 1 origin) | 2026-09-29 |
| 6 | `certificate-generator` | 🟢 LIVE `syazwanbmw-dev.github.io/Certificate-Generator` (GitHub Pages, fork Kiyoraka, static 100%). Redesign "Ruang Kerja Bersih" siap. ⚠️ push TAK auto-deploy — `gh workflow run static.yml` manual. Tiada backlog | 2026-09-28 |
| 7 | `digital-hub` | 🟢 LIVE prod `digitalhubsks.celikguru.my` @`a312f79` — Import Setting + PWA installable + Open Graph. Unit 197 / e2e 71. Backlog: audit log · kategori button · strip pengumuman · WAF rate-limit | 2026-09-02 |
| 8 | `erph` (RENDAH) | 🔄 RPT Sains Tahun 5 separuh jalan (branch `rpt-sains5`). Sambung: re-review `eab609c..ea0fd95` | 2026-08-07 |
| 9 | `erph-menengah-v2` | ✅ Tiada tertunggak. 🔴 menu "Baikpulih Tapak" MENULIS — jangan klik | 2026-08-07 |
| 10 | `sistem-olahraga-sekolah` | ✅ Multi-tenant live. Throttle brute-force LIVE, tiada kerja tertunggak | 2026-07-19 |
| 11 | `idme-pajsk-ext` | 🟡 Spec+plan siap (8 task TDD), BELUM execute. Task 7 gated tunggu selector master | 2026-06-24 |

⚠️ **11 projek** — melebihi had 10. Peraturan LRU akan turunkan `idme-pajsk-ext` (paling lama tak disentuh) ke Kurang Aktif, tapi CLAUDE.md global masih senarai ia Aktif → **dibiarkan, tunggu keputusan master**.

## Kurang Aktif (tak kira dalam slot — ikut keputusan master)

| Project | Nota |
|---------|------|
| `adni` | ✅ Live `db35628` — Mahrajan QIT Salor 2026 (172 peserta). Event dah lepas, dorman |
| `erpm-v2` | ⏳ BELUM MULA — scaffolding sahaja, idle sejak 14 Apr 2026. **BUKAN** gantian `mypwa-v2` (lihat `project_erpm_v2.md`) |
| `balapan`, `sprint`, `myportfolio`, `erpm-cf` | Tiada aktiviti direkod baru-baru ini |

## Projek lain (tak masuk kiraan slot)

- `BrightMe` — Flutter Android, 4 modul kesihatan mental+bakat. Pending keputusan master: format modul BAKAT.
- `motion-video-skill` — 🟡 HOLD. Perlu GPU 8GB+ (laptop cuma MX450 2GB).

## Dipadam
- ~~`my-pwa`~~ — digantikan sepenuhnya oleh `mypwa-v2`, repo lama dipadam. Jangan rujuk lagi.

---

## ⚠️ Nota sistem lama (2026-08-27, masih terpakai)

Folder `coding-projects/active/*.md` (sistem LRU lama, `lru-manager.md`) **tidak diselaraskan sejak
~2026-04/06** — `my-pwa.md` masih guna nama lama, `erpm-cf.md`/`myportfolio.md` cuma stub kosong,
tiada fail langsung untuk projek baharu. Jadual di atas sengaja **tidak** link ke fail-fail itu —
dianggap **duplicate source**. Fail lama **tidak dipadam** (tunggu keputusan master): lupuskan terus,
atau kekalkan sebagai arkib sahaja.

---
## LRU Rules (rujukan, tak dikuatkuasakan oleh fail berasingan buat masa ini)
- Position 1 = paling baru disentuh
- Max 10 projek Aktif; lebih daripada itu → turun ke Kurang Aktif
