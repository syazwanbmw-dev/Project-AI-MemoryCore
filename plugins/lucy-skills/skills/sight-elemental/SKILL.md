---
name: sight-elemental
description: "Use when user says 'elemental', 'sight elemental', 'deep scan', 'scan new project',
             or when starting work on a new project or major refactor. Full deep scan of entire
             codebase. Sonnet-level depth."
---

# Sight Elemental — Deep Scan
*Full codebase scan — sesuai untuk projek baru atau major refactor*

## Activation

When this skill activates, output:

`Elemental deep scan initiated...`

## Bila Guna Elemental

- Mula kerja pada projek baru yang belum pernah di-review
- Selepas major refactor yang melibatkan banyak fail
- Bila nak faham struktur projek secara menyeluruh
- Bila Hone jumpa isu yang mungkin ada dalam keseluruhan projek

## Scan Protocol

### Step 1: Map Codebase
- [ ] Baca struktur folder projek
- [ ] Kenal pasti entry points (main worker file, route files, frontend files)
- [ ] Senaraikan semua fail yang akan di-scan

### Step 2: Deep Scan (8 Aspek)

**Struktur & Arsitektur**
- [ ] Folder structure logik dan konsisten?
- [ ] Separation of concerns — logic, routes, helpers diasingkan?

**Keselamatan**
- [ ] Input validation ada di semua endpoints?
- [ ] Secrets/API keys tidak hardcoded?
- [ ] SQL queries guna parameterized (D1 binding)?
- [ ] CORS, headers, auth configured dengan betul?

**Kualiti Kod**
- [ ] Dead code atau commented-out code yang tak perlu?
- [ ] Duplicate logic yang boleh di-refactor?
- [ ] Error handling konsisten di seluruh projek?

**Database (D1)**
- [ ] Schema konsisten dengan queries?
- [ ] CASCADE delete/update di mana perlu?
- [ ] Index pada columns yang kerap di-query?

**Frontend**
- [ ] XSS vectors dalam HTML/JS?
- [ ] User input di-sanitize sebelum render ke DOM?

**Cloudflare Workers**
- [ ] Worker limits diambil kira (CPU time, memory)?
- [ ] Binding D1/KV/R2 configured betul dalam wrangler.toml?

### Step 3: Report
```
Elemental Report — [nama projek]
=================================
ARCHITECTURE: [OK / Issues found]
SECURITY:     [OK / Issues found]
CODE QUALITY: [OK / Issues found]
DATABASE:     [OK / Issues found]
FRONTEND:     [OK / Issues found]
CF WORKERS:   [OK / Issues found]
---------------------------------
Critical Issues: [X]
Warnings:        [X]
Suggestions:     [X]
=================================
[Detail setiap isu dengan fail + line + cadangan fix]
```

### Step 4: Tindakan
- Susun isu ikut priority: Critical → Warning → Suggestion
- Fix Critical dulu sebelum teruskan development
- Warning boleh plan untuk fix kemudian

## Mandatory Rules

1. Ini scan penuh — ambil masa, jangan skip
2. Security issues adalah Critical secara automatik
3. Orphan data dalam DB adalah Critical (ikut standard Lucy)
4. Lepas Elemental, update project memory fail dengan findings

## Level History

- **Lv.1** — Base: 8-aspek deep scan, D1/Hono/CF Workers aware. (Origin: Kata Pipeline install, 2026-03-31)
