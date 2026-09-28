# Project List — Overview
*Dikemas 2026-09-28 — disegerak drpd `git log` setiap repo + MEMORY.md projek (git yang betul bila bercanggah).*
*Sumber kebenaran status: `git log` repo → `MEMORY.md`/`CLAUDE.md` repo → `project_<nama>.md` dalam auto-memory (`C:\Users\user\.claude\projects\C--Users-user\memory\`).*

## Coding Projects — Aktif (susun ikut LRU: paling baru disentuh di atas)

| # | Project | Status ringkas | Last touched |
|---|---------|-----------------|---------------|
| 1 | `certificate-generator` | 🟢 LIVE `syazwanbmw-dev.github.io/Certificate-Generator` (GitHub Pages, fork Kiyoraka, static 100%). Redesign "Ruang Kerja Bersih" siap. ⚠️ push TAK auto-deploy — `gh workflow run static.yml` manual. Tiada backlog | 2026-09-28 |
| 2 | `opr-program` | 🟢 LIVE guru `@30` (suite 617) — rangka muat (skeleton) ganti skrin putih, atas `@29` (pagar saiz Buku Program + mesej peringkat). 📱 Master: masih putih sekejap sebelum rangka (HTML ~670 KB, 82% pustaka PDF) — SAMBUNG: tunggu "ya" utk deployment UKUR. Smoke `@29` tertunggak. Backlog: 3 PDF contoh kualiti (langkah 3/3), Fasa 4 Migrasi | 2026-09-28 |
| 3 | `takwim-digital` | 🟢 LIVE `@25` — swipe kalendar tukar bulan + animasi slide + isyarat loading. Digest Telegram+Google Chat LIVE. Suite 100/0. Tiada backlog | 2026-09-20 |
| 4 | `opr-insaniah` | 🟡 Deployed `@44`, suite 526/526, **BELUM launch rasmi**. C1+I5 belum dibaiki (DIHOLD). Onload lambat disiasat, belum dibaiki | 2026-09-18 |
| 5 | `digital-hub` | 🟢 LIVE prod `digitalhubsks.celikguru.my` @`a312f79` — Import Setting + PWA installable + Open Graph. Unit 197 / e2e 71. Backlog: audit log · kategori button · strip pengumuman · WAF rate-limit | 2026-09-02 |
| 6 | `mypwa-v2` (eNilai) | 🟢 LIVE production. Kumpulan Intervensi suite 58/0/2, feature mendarat MATI (`guna_kumpulan=0` semua item) — admin belum hidupkan | 2026-08-10 |
| 7 | `erph` (RENDAH) | 🔄 RPT Sains Tahun 5 separuh jalan (branch `rpt-sains5`). Sambung: re-review `eab609c..ea0fd95` | 2026-08-07 |
| 8 | `celiksains` | 🟡 Fasa 1a live staging. Hardening anti-tipu: spec+plan siap, BELUM kod | 2026-08-07 |
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
