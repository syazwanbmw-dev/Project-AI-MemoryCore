# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-17
**Session Focus**: mypwa-v2 — PAJSK Bulk Import (implement + deploy)

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-17 petang: PAJSK Bulk Import — implement + live production** ✅

  1. **Execute plan** — 3 fail diubah: `src/routes/pajsk.js`, `public/pajsk.html`, `public/admin.html`
  2. **Backend** `POST /api/pajsk/bulk` — D1 batch lookup murid + batch UPSERT, normalize kategori/peringkat/pencapaian case-insensitive
  3. **Frontend** pajsk.html — button Import Pukal (muncul bila sesi dipilih), SheetJS parse, result modal, link Muat Turun Template
  4. **Frontend** admin.html — sama, guna `cariPajskAdmin()` untuk refresh
  5. **Bug fixes** dalam sesi ni:
     - Lookup murid guna `nama_kelas` penuh ("6 DELIMA") bukan split tahun+kelas
     - Button Import Pukal dipindah ke `#rImportWrap` berasingan (Hone catch)
     - Normalize kategori/peringkat/pencapaian ikut canonical dropdown values
  6. **Merge ke main** — live production `erpm-sksalor.celikguru.my`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| PAJSK Bulk Import | ✅ LIVE | Merged main 2026-05-17 |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- nama_kelas dalam DB simpan nilai penuh: "6 DELIMA" (bukan "DELIMA" sahaja)
- UPSERT logic: drive_link kosong dalam CSV tak padam link lama dalam DB
