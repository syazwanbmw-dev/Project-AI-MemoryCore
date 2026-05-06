# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-05-07
**Session Focus**: mypwa-v2 — Sidebar Accordion + Active Tab Highlight

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi ini (2026-05-07 malam): Sidebar accordion + active tab** ✅

  1. **Sidebar Accordion** — RPM dan Ujian Dalaman jadi accordion group
     - `app.js`: guruLinks baru (group + children), renderSidebar papar accordion HTML, toggleGroup()
     - `app.css`: 8 class baru (.sidebar-group-header, .sidebar-arrow, .sidebar-group-body, .sidebar-sublink)
     - sessionStorage persist open state — group tak collapse bila navigate
     - Auto-expand group bila berada pada child page
     - Latest commit test branch: `b213aa6`

  2. **GitHub Actions CI fixed** — CLOUDFLARE_API_TOKEN dalam GitHub Secrets sudah direnew
     - CI kini berjaya deploy ke staging (test branch → staging auto)
     - Cache-bust ditambah: `/app.css?v=2` dalam semua HTML

  3. **Active tab highlight** — WIP, belum selesai
     - Root cause ditemui: inactive links `color: #fff` = sama dengan active → tiada contrast
     - Fix: inactive links kini `color: rgba(255,255,255,0.7)` (muted), active = `#fff` + background fill + accent border
     - Push ke test (b213aa6) tapi BELUM CONFIRMED working — user start fresh sebelum confirm
     - User nak start fresh — perlu approach baru untuk active tab

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Active tab highlight — confirm working | ⚠ PENDING | b213aa6 pushed ke test, belum confirmed. User nak start fresh. |
| Sidebar accordion — deploy ke production | ⏳ PENDING | Tunggu confirm active tab dulu, baru merge ke main |
| Cetak tab Laporan PAJSK | ⏳ BACKLOG | Hidden dulu, develop kemudian |
| Compact mode Ujian Dalaman | ⏳ BACKLOG | Bar 28px, 6 kad belum muat satu halaman cetak |

### Important Context
- mypwa-v2 production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- mypwa-v2 staging DB: `f87c8bbc-77a5-4d57-88d1-284265de437f`
- Staging folder Drive ID: `1Wxr2acfI8DYPu4MwzR3AWzQkiGFmFLG-`
- Production folder Drive ID: `1eLfVA_N7C-E1C0BHRndmd85rmpmBljBS`
- Apps Script URL: `https://script.google.com/macros/s/AKfycbwnaqfjz_KmW0UH9rSLXwzKICX-f1HJG7LzdZLDdvh05tI3QDrCGejfuWSZB8GLadiSaQ/exec`
- Untuk set wrangler secrets: WAJIB guna Bash `printf 'nilai' | npx wrangler secret put KEY --env production`
- Deploy production: `npx wrangler deploy --env production` (jangan auto-deploy tanpa confirm master)
- test branch = staging auto-deploy via CI (GitHub Actions sudah fixed)

### Sidebar Commits Summary (sesi ini)
| Commit | Perubahan |
|--------|-----------|
| `905d156` | feat: sidebar accordion RPM + Ujian Dalaman |
| `c92b532` | fix: sessionStorage — group kekal open |
| `c103813` | feat: group header active highlight (reverted) |
| `468e967` | fix: revert — accent pada sub-link je |
| `fc1d442` | feat: group header highlight balik |
| `c9c7200` | fix: active tab — background fill + accent border |
| `76cf1c2` | fix: cache-bust app.css?v=2 |
| `b213aa6` | fix: inactive links muted untuk contrast |
