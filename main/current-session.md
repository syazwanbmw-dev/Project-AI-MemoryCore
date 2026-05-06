# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-06
**Session Focus**: mypwa-v2 — PAJSK Owner Controls (design + plan siap, belum implement)

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **PAJSK — Sesi lepas (2026-05-05): Full deploy ke production selesai** ✅

- **Sesi ini (2026-05-06): PAJSK Owner Controls — design & plan siap** ✅

  3 features dirancang dalam satu package:
  - **F1 — Owner-only controls**: Butang Edit/Padam hanya visible kepada guru yang masukkan rekod
  - **F2 — Edit rekod**: PUT /api/pajsk/:id — ubah field teks + optional ganti PDF (tanpa re-upload wajib)
  - **F3 — Simpan tanpa dokumen**: PDF optional masa tambah rekod, simpan drive_link = '' untuk upload kemudian

  Design decisions:
  - Owner row → Dokumen: "Buka PDF"/"⚠ Belum Upload" | Tindakan: [Edit][Padam]
  - Bukan-owner row → Dokumen: "Buka PDF"/"Tiada fail" | Tindakan: kosong
  - Guna localStorage `uid` untuk kenal pasti owner (tambah `id` dalam login response)
  - Tiada migration — guna `drive_link = ''` sebagai sentinel "belum upload"
  - KISS & DRY: `getCurrentUserId()` dalam app.js, reuse `openModal()` sedia ada, reuse `toBase64()` dalam pajsk.js

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| **PAJSK Owner Controls — implement** | ⏳ READY | Plan siap, belum implement. Guna plan di bawah |
| Audit Log (migration 016) — merge ke main | ⏳ BACKLOG | Siap di staging, pending merge |
| Rate limiting `/api/login` | ⏳ BACKLOG | Migration 014 ada, audit log dah siap |
| Cetak tab Laporan PAJSK | ⏳ BACKLOG | Hidden dulu, develop kemudian |

### Plan & Spec

- **Plan**: `docs/superpowers/plans/2026-05-06-pajsk-owner-controls.md`
- **Spec**: `docs/superpowers/specs/2026-05-06-pajsk-owner-controls-design.md`

Plan ada 4 tasks:
1. Backend: login response tambah `id` + uid helper dalam app.js
2. Backend: POST pajsk — buat file optional
3. Backend: PUT /api/pajsk/:id (edit route baru)
4. Frontend: pajsk.html — conditional buttons, `_allRekod` cache, edit modal

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- mypwa-v2 staging DB: `f87c8bbc-77a5-4d57-88d1-284265de437f`
- Staging folder Drive ID: `1Wxr2acfI8DYPu4MwzR3AWzQkiGFmFLG-`
- Production folder Drive ID: `1eLfVA_N7C-E1C0BHRndmd85rmpmBljBS`
- Apps Script URL: `https://script.google.com/macros/s/AKfycbwnaqfjz_KmW0UH9rSLXwzKICX-f1HJG7LzdZLDdvh05tI3QDrCGejfuWSZB8GLadiSaQ/exec`
- Untuk set wrangler secrets: WAJIB guna Bash `printf 'nilai' | npx wrangler secret put KEY --env production`
