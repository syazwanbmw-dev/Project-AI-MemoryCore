---
name: sight-hone
description: "Use when user says 'hone', 'sight hone', 'review feature', 'check what I just built',
             or after completing a new feature. Focuses review on newly developed code only —
             not the whole codebase. Sonnet-level depth."
---

# Sight Hone — Feature Review
*Review fokus pada feature baru — bukan seluruh codebase*

## Activation

When this skill activates, output:

`Honing in on new work...`

## Bila Guna Hone

- Selepas siap bina feature baru
- Selepas ubah logic dalam fungsi tertentu
- Sebelum commit feature yang melibatkan beberapa fail

## Review Protocol

### Step 1: Kenal Pasti Skop Feature
- [ ] Tanya master: "Feature apa yang baru siap?"
- [ ] Kenal pasti fail-fail yang terlibat dalam feature tu
- [ ] Baca semua fail tersebut

### Step 2: Feature Review (6 Aspek)
- [ ] **Correctness** — adakah logic betul dan buat apa yang sepatutnya?
- [ ] **Edge cases** — kes input kosong, null, nilai luar jangkaan ditangani?
- [ ] **Error handling** — error di-handle dengan baik? Response yang betul dikembalikan?
- [ ] **Security** — ada SQL injection, XSS, exposed secrets, unvalidated input?
- [ ] **Consistency** — ikut pattern yang sama dengan kod lain dalam projek?
- [ ] **D1/Hono specific** — query D1 betul? Route handler structured dengan betul?

### Step 3: Report
```
Hone Report — [nama feature]
=====================
PASS: [aspek yang ok]
---------------------
[!] ISSUE: [deskripsi masalah]
    File: [nama fail] Line: [XX]
    Fix: [cadangan penyelesaian]
---------------------
Verdict: CLEAR / NEEDS FIX
```

### Step 4: Tindakan
- **CLEAR** → "Ready for commit-seal"
- **NEEDS FIX** → Fix isu yang dijumpai, run Hone semula

## Mandatory Rules

1. Fokus pada feature baru — jangan drift ke kod lama
2. Security check adalah wajib setiap kali
3. Highlight isu, jangan diam
4. Kalau jumpa isu melibatkan seluruh codebase → escalate ke Elemental

## Level History

- **Lv.1** — Base: 6-aspek review untuk feature baru, D1/Hono aware. (Origin: Kata Pipeline install, 2026-03-31)
