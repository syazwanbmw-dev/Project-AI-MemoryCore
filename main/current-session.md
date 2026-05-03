# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Active
**Last Activity**: 2026-04-30
**Session Focus**: mypwa-v2 — UX fixes (ujian dalaman + kelas saya mobile)

## 💭 Working Memory (RAM)

### Active Context
- **Current Topic**: mypwa-v2 — UX fixes ujian dalaman + kelas saya mobile, di test branch
- **Branch aktif**: `test`
- **Production URL**: `erpm-sksalor.celikguru.my`

### Session Recap (For AI Restart)

- **PWA icon + eRekod name** ✅ (sesi lepas)
  - manifest.json, icon-192.png, icon-512.png — merged ke main

- **Dashboard carta responsive mobile** ✅ (sesi lepas)
  - Media query `!important` override inline grid style — merged ke main

- **Plan Kelas Multi-Tahun Sesi** ✅ SEMUA FASA SELESAI
  - Plan file: `docs/superpowers/plans/2026-04-28-kelas-tahun-sesi.md`
  - Fasa 1 ✅ — migration 015, kelas.js, murid.js, ujian-markah.js (commit 56684aa)
  - Fasa 2 ✅ — admin.html dropdown sesi + semua fungsi kelas (commit fb07d99)
  - Fasa 3 ✅ — guru pages ?latest=1 (commit 06822a7)
  - Fasa 4 ✅ — dashboard + laporan-ujian dropdown Tahun Sesi (commit 88ae2e5)

### Plan Summary (Untuk AI Restart)

**Fix:** Tambah `tahun_sesi INTEGER` pada table `kelas`

**4 Fasa — SEMUA SELESAI:**

| Fasa | Fail | Commit |
|------|------|--------|
| 1 — Foundation | migrations/015, kelas.js, murid.js, ujian-markah.js | 56684aa |
| 2 — Admin Panel | admin.html | fb07d99 |
| 3 — Guru Pages | jadual-guru.js, rekod.html, laporan.html, tetapan.html | 06822a7 |
| 4 — Dashboard + Laporan Ujian | app.css, dashboard.html, laporan-ujian.html | 88ae2e5 |

**Key decisions (untuk rujukan masa depan):**
- `?latest=1` pada `/kelas` dan `/jadual-guru` → kelas dari `MAX(tahun_sesi)`
- rekod.html + laporan.html guna `/jadual-guru?latest=1` (bukan /kelas terus)
- `laporan-ujian.html`: dropdown Tahun Sesi → filter kelas dropdown (frontend only), `_tahunList` state
- `dashboard.html` ujian tab: `sesiParam` dipass ke semua 3 analisis API calls + filter kelasList
- `filter-grid-4` CSS class baru untuk 4-kolum filter bar
- Auto-set rule: currentYear jika wujud dalam DB, else fallback ke max year

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Kelas Multi-Tahun Sesi — semua fasa | ✅ SELESAI | Merged ke main (2026-04-30) |
| Bug fix: ujian tutup guru masih boleh akses laporan & dashboard | ✅ SELESAI | commit a4038f4 |
| UX fix: notis jadual kosong (rekod.html + laporan.html) | ✅ SELESAI | commit 8e0483a |
| Merge ke main + migration 015 production | ✅ SELESAI | commit 54bfc49, migration dah wujud (OK) |
| Audit Log implementation | ✅ SELESAI (staging) | 4 commits d815b0f→9f49a59, migration 016 applied ke staging |
| Test audit log staging | ✅ SELESAI | Tested — tab Log Audit berfungsi |
| UX: Kelas Saya mobile card view | ✅ SELESAI | tetapan.html — card stack ≤768px (commit e79ff05) |
| UX: Ujian Dalaman filter bar 1 baris | ✅ SELESAI | ujian.html + app.css filter-grid-5 (commit fb40cf1) |
| Merge audit log ke main | ⏳ PENDING | Apply migration 016 ke production + merge test → main |
| Rate limiting `/api/login` | ⏳ BACKLOG | Migration 014 ada, audit log dah siap |

### Issue Log

**2026-04-28 — UX: Kelas dropdown kosong tanpa penjelasan**
- Bila admin aktif sesi 2027 + upload kelas 2027, MAX(tahun_sesi)=2027
- `/jadual-guru?latest=1` return kosong kalau guru belum tambah jadual 2027
- Guru confuse sebab tiada mesej — perlu tambah mesej + link ke Tetapan
- Detail: `docs/superpowers/plans/2026-04-28-kelas-tahun-sesi.md` (bahagian Issue)

**2026-04-28 — SILAP WORKFLOW: Migration dirun ke production DB terus**
- Migration 015 dirun ke `mypwa-v2-db` (PRODUCTION) dalam Fasa 1 — SALAH
- Staging DB (`mypwa-v2-staging-db`) tidak dapat migration → admin nampak error semasa test
- Production tidak crash (migration additive) tapi ini PELANGGARAN WORKFLOW
- Fix: Migration 015 sudah dirun ke staging DB (2026-04-28)
- PERATURAN DIPERKEMAS: staging dulu → confirm → production (lihat feedback_mypwa_workflow.md)

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Migration 015 sudah dirun ke remote DB (Fasa 1)
- Semua perubahan ada di `test` branch — staging URL untuk test
- Kata pipeline wajib sebelum execute
