# sistem-olahraga-sekolah
*Coding Project - Updated 2026-04-13*

## Description
Sistem pengurusan olahraga untuk sekolah — manage acara, peserta, dan keputusan. Multi-tenant (setiap sekolah ada data berasingan).

## Project Details
- **Type**: Coding Project
- **Status**: Active
- **Created**: 2026-03-26
- **Last Accessed**: 2026-04-13
- **Position**: #1

## Technical Stack
- **Backend**: Hono.js on Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite)
- **Frontend**: Vanilla HTML/JS + Tailwind CSS
- **Deployment**: Cloudflare (via test branch → confirm → main)
- **Version Control**: Git → test branch → Cloudflare

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
  - Endpoint: `GET /api/pengguna/check-id?id=xxx`
  - Label hijau ✓ / merah ✗ bawah field ID Pengguna
  - Button daftar disabled bila username tidak tersedia
  - Minimum 1 huruf untuk trigger check (debounce 500ms)
  - Guna event delegation (`document.addEventListener('input', ...)`) untuk hakim
- `paparToast()` support warna green/red/amber + title dinamik
- Semua `alert()` dalam guru & hakim section digantikan dengan `paparToast`

## Known Issues / Gotchas
- Browser extensions (password managers dengan wfd-id attribute) boleh intercept
  `oninput` dan `addEventListener` pada element level — guna event delegation sebagai fix
- SVG elements: `className` adalah `SVGAnimatedString`, bukan string —
  guna `getAttribute/setAttribute` atau `classList` bukan `.className.replace()`

## Pending — Post Musim Sukan
- Upgrade ke The Foundation Approach B (workspace CONTEXT.md per folder)
- Hold sehingga habis musim sukan — projek live, jangan restructure semasa aktif
- Approach A (CLAUDE.md + CONTEXT.md root) dah implement 2026-04-14

## Progress Log
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
