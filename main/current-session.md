# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-09
**Session Focus**: mypwa-v2 — Bug fixes iPad + Features PAJSK

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-05-09 petang: Semua fixes + features SELESAI & merged ke main** ✅

  1. **iOS date input border & tinggi tak konsisten** (`app.css`) — `15fde25` → merged main ✅
     - Tambah `input[type="date"].form-input { -webkit-appearance:none; appearance:none; }`
     - Fix border invisible + height inconsistency pada iPad Safari

  2. **Kategori Sumbangan dalam PAJSK** (`pajsk.html`) — `d75df92` → merged main ✅
     - Tambah option "Sumbangan" dalam dropdown form tambah, filter laporan, edit modal
     - Badge warna ungu (`#ede9fe / #6d28d9`)

  3. **Tab PAJSK dalam Admin Panel** (`admin.html`, `pajsk.js`) — merged main ⏳ (staging OK, belum merge)
     - Backend: `DELETE /api/pajsk/by-sesi` — admin bulk delete by sesi
     - Frontend: tab PAJSK, filter (sesi/kelas/kategori), table rekod, padam individual & by sesi
     - Commits: `478a7c8` (feat), `a484f94` (fix api wrapping), `7d7dce7` (fix dropdown kelas), `1c388c3` (fix kolum kelas)
     - Bug fixes: `r.data.data` bukan `r.data`, filter kelas Thn 4-6, nama_kelas tanpa tahun prefix

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Merge tab PAJSK admin ke main | ⏳ PENDING | Staging OK, tunggu confirm |
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

### Commits Sesi Ini (2026-05-09)
| Commit | Perubahan | Status |
|--------|-----------|--------|
| `637dc7b` | fix: hero image first-load | merged main |
| `7b4a5fb` | fix: edit PAJSK — PDF optional | merged main |
| `f36b024` | fix: filter grid rekod RPM iPad | merged main |
| `b38c429` | fix: input date overflow iPad (WebKit) | merged main |
| `e44801a` | fix: tarikh input saiz tak konsisten | merged main |
| `15fde25` | fix: date border & tinggi iOS Safari (-webkit-appearance) | merged main |
| `d75df92` | feat: kategori Sumbangan dalam PAJSK | merged main |
| `478a7c8` | feat: tab PAJSK dalam admin panel | test branch |
| `a484f94` | fix: PAJSK admin api() response wrapping | test branch |
| `7d7dce7` | fix: PAJSK admin dropdown kelas Thn 4-6 | test branch |
| `1c388c3` | fix: PAJSK admin kolum kelas nama sahaja | test branch |
