# sistem-olahraga-sekolah
*Coding Project - Updated 2026-04-26*

## Description
Sistem pengurusan olahraga untuk sekolah — manage acara, peserta, dan keputusan. Multi-tenant (setiap sekolah ada data berasingan).

## Project Details
- **Type**: Coding Project
- **Status**: Active
- **Created**: 2026-03-26
- **Last Accessed**: 2026-04-26
- **Position**: #1

## Technical Stack
- **Backend**: Hono.js on Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite)
- **Frontend**: Vanilla HTML/JS + Tailwind CSS
- **Deployment**: Cloudflare (via test branch → confirm → main)
- **Version Control**: Git → test branch → Cloudflare
- **DB Production**: `olahraga-db` (5dd90565)
- **DB Test**: `olahraga-test` (aa971f40)

## Architecture Decisions
### Username Uniqueness — GLOBAL (bukan per-school)
- `id_pengguna` adalah PRIMARY KEY global merentas semua sekolah
- Keputusan: **global uniqueness** — lebih mudah, login hanya perlu username + password
- Per-school uniqueness ditolak kerana login form perlu field tambahan "Kod Sekolah"
- **Wajib ada realtime check** sebelum user daftar ID baru

### Peranan Pengguna
- `super_admin`, `sub_admin`, `guru_rumah`, `hakim`
- Jangan guna nama `admin` — tiada `checkRole('admin')` dalam sistem

## Features Siap
- Realtime username availability check semasa daftar guru rumah & hakim
- `paparToast()` support warna green/red/amber + title dinamik
- Semua `alert()` dalam guru & hakim section digantikan dengan `paparToast`
- **Laporan Kejohanan** — label kategori (L10, P12, dll) dalam header setiap acara
- **Download Semua Laporan** — urutan print betul, groupby kategori, page break antara kategori
- **Ruang tandatangan** — letaknya terus bawah table Kedudukan Markah Akhir

## Laporan — Print Order (Download Semua)
```
1. Kedudukan Markah Akhir Rumah Sukan + tandatangan
2. Senarai Pingat
3. Keputusan Acara (grouped by kategori, page break antara kategori)
4. Statistik Kejohanan (landscape)
```

## Known Issues / Gotchas
- Browser extensions (password managers dengan wfd-id attribute) boleh intercept
  `oninput` dan `addEventListener` pada element level — guna event delegation sebagai fix
- SVG elements: `className` adalah `SVGAnimatedString`, bukan string —
  guna `getAttribute/setAttribute` atau `classList` bukan `.className.replace()`
- `/api/laporan/keputusan` MESTI JOIN `kategori` table untuk dapat `nama_kategori`

## DB Workflow
- Copy production → test: export `wrangler d1 export olahraga-db --remote` → drop tables test → import
- Backup disimpan dalam `backup/` (dalam .gitignore — jangan commit)

## Pending — Post Musim Sukan
- Upgrade ke The Foundation Approach B (workspace CONTEXT.md per folder)
- Hold sehingga habis musim sukan — projek live, jangan restructure semasa aktif
- Rate limiting `/api/login`
- Code DRY extraction

## Progress Log
### 2026-04-26
- Copy data production → test DB (untuk development)
- Fix laporan: tambah label nama_kategori (L10, P08, dll) dalam header acara
- Fix SQL `/api/laporan/keputusan` — JOIN kategori table, select nama_kategori
- Susun semula Download Semua Laporan — urutan baru + group by kategori + page break
- Fix ruang kosong dalam print (margin-bottom 30mm → 6mm)
- Pindah tandatangan ke bawah table Kedudukan Markah Akhir
- Tambah `backup/` ke .gitignore
- Merge ke main, live production

### 2026-04-13
- Fix realtime check hakim ID (event delegation bypass browser extension issue)
- Fix paparToast SVG className error
- Replace alert() dengan paparToast dalam guru & hakim
- Lower minimum check dari 3 huruf ke 1 huruf
- Merge semua changes ke main

### 2026-04-12
- Tambah realtime username availability check untuk guru & hakim
- Endpoint GET /api/pengguna/check-id
- Button disable bila username tidak tersedia

---
*sistem-olahraga-sekolah | Position #1*
