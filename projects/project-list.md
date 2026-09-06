# Project List — Overview
*Dikemas 2026-08-27 — disegerak drpd MEMORY.md sebenar (auto-memory `.claude\projects\...\memory\`).*
*Sumber kebenaran status: `project_<nama>.md` dalam auto-memory, atau `CLAUDE.md`/`MEMORY.md` masing-masing repo.*

## Coding Projects — Aktif (10/10 slot)

| # | Project | Status ringkas | Last touched |
|---|---------|-----------------|---------------|
| 1 | `takwim-digital` | 🟢 LIVE `@22` — reminder aktiviti jadi email digest H-1/2/3 + Guru Penerima + notifikasi admin pendaftaran. Trigger dicipta 2026-09-06. Tiada backlog | 2026-09-06 |
| 2 | `opr-insaniah` | 🟢 LIVE `@44` — suite 526/526. `/dev` TAK BOLEH uji akaun kedua (had platform) | 2026-08-25 |
| 3 | `digital-hub` | 🟡 Brainstorming architectural — keputusan storan(KV)/tema/login siap, belum spec/plan, belum wujud fizikal | 2026-08-24 |
| 4 | `opr-program` | 🟡 Fasa 1 Task 0-10 SIAP (41/41 lulus, 2 Critical ditangkap). Final review dihentikan master — Task 11 deploy belum jalan | 2026-08-23 |
| 5 | `mypwa-v2` (eNilai) | 🟢 LIVE production. Kumpulan Intervensi suite 58/0/2, feature mendarat MATI (`guna_kumpulan=0` semua item) — admin belum hidupkan | 2026-08-10 |
| 6 | `erph` (RENDAH) | 🔄 RPT Sains Tahun 5 separuh jalan | 2026-08-05 |
| 7 | `celiksains` | 🟡 Fasa 1a live staging. Hardening anti-tipu: spec+plan siap, BELUM kod | 2026-07-25 |
| 8 | `erph-menengah-v2` | ✅ Tiada tertunggak. 🔴 menu "Baikpulih Tapak" MENULIS — jangan klik | 2026-07-23 |
| 9 | `sistem-olahraga-sekolah` | ✅ Multi-tenant live. Throttle brute-force LIVE, tiada kerja tertunggak | 2026-07-19 |
| 10 | `idme-pajsk-ext` | 🟡 Spec+plan siap (8 task TDD), BELUM execute. Task 7 gated tunggu selector master | 2026-07-18 |

## Kurang Aktif (tak kira dalam slot 10 — ikut keputusan master)

| Project | Nota |
|---------|------|
| `adni` | ✅ Live `db35628` — Mahrajan QIT Salor 2026 (172 peserta). Event dah lepas, dorman |
| `erpm-v2` | ⏳ BELUM MULA — scaffolding sahaja, idle sejak 14 Apr 2026. **BUKAN** gantian `mypwa-v2` (lihat `project_erpm_v2.md`) |
| `balapan`, `sprint`, `myportfolio`, `erpm-cf` | Tiada aktiviti direkod baru-baru ini |

## Projek lain (tak masuk kiraan slot Aktif/Kurang Aktif)

- `BrightMe` — Flutter Android, 4 modul kesihatan mental+bakat. Pending keputusan master: format modul BAKAT.

## Dipadam
- ~~`my-pwa`~~ — digantikan sepenuhnya oleh `mypwa-v2`, repo lama dipadam. Jangan rujuk lagi.

---

## ⚠️ Isu dijumpai semasa kemas kini ni (2026-08-27)

Folder `coding-projects/active/*.md` (sistem LRU lama, `lru-manager.md`) **tidak diselaraskan sejak
~2026-04/06** — `my-pwa.md` masih guna nama lama, `erpm-cf.md`/`myportfolio.md` cuma stub kosong,
tiada fail langsung untuk projek baharu (`takwim-digital`, `opr-insaniah`, `digital-hub`, dll).
Jadual table di atas sengaja **tidak** link ke fail-fail itu — ia dianggap **duplicate source**
berbanding auto-memory. Fail lama **tidak dipadam** (tunggu keputusan master) — cuma tidak
dijadikan rujukan lagi. Cadangan: lupuskan sistem `coding-projects/active/` ini terus, atau
kekalkan sebagai arkib sahaja.

---
## LRU Rules (rujukan, tak dikuatkuasakan oleh fail berasingan buat masa ini)
- Position 1 = paling baru disentuh
- Max 10 projek Aktif; lebih daripada itu → turun ke Kurang Aktif
