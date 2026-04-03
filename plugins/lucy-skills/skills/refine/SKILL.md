---
name: refine
description: "Use when user says 'refine', 'refine code', 'clean up code', 'review changes',
             'review what I changed', 'sharpen', 'check my code', or after completing
             implementation before committing. Reviews changed code for quality, reuse,
             and efficiency — then fixes with permission. Corrective tier of Observation System."
---

# Refine — Code Quality Review
*Lihat kod yang berubah dengan jelas, betulkan dengan izin.*

## Activation

When this skill activates, output:

`Reviewing changed code...`

## Context Guard

| Context | Status |
|---------|--------|
| **User says "refine", "clean up", "review changes"** | ACTIVE — run Refine |
| **Selepas implementasi, sebelum commit** | ACTIVE — quality gate |
| **User says "check my code", "sharpen"** | ACTIVE — run Refine |
| **Casual conversation** | DORMANT |

## Bila Guna Refine

- Selepas siap kod feature, sebelum `commit-seal`
- Bila rasa kod boleh dipertingkat tapi tak tahu di mana
- Sebagai quality gate harian sebelum commit

## Refine Protocol

### Step 1: Kenal Pasti Skop

| Argument | Skop |
|----------|------|
| Tiada argument | `git diff --name-only` — semua fail yang berubah |
| Nama fail | Fail spesifik tersebut sahaja |
| Keyword area | Semua fail berkaitan area tersebut |

Skip: fail generated, migrations, lock files, `node_modules`

### Step 2: Baca dan Faham

Untuk setiap fail dalam skop:
- [ ] Baca fail penuh — faham konteks, bukan sekadar diff
- [ ] Baca diff — fokus pada apa yang berubah
- [ ] Kenal pasti intent — apa yang author cuba buat?

**Peraturan**: Faham intent sebelum nilai kualiti. Jangan "fix" sesuatu yang disengajakan.

### Step 3: Analisa (Senarai Semak Refinement)

**Kualiti Kod**
- [ ] Dead code — variable tidak guna, cabang tidak boleh dicapai, blok yang di-comment
- [ ] Duplikasi — logic yang sama berulang (extract kalau 3+ kali)
- [ ] Penamaan — jelas, konsisten dengan konvensyen projek
- [ ] Kompleksiti — boleh logik bersarang dipermudah?

**Guna Semula & Struktur**
- [ ] Utiliti sedia ada — adakah helper untuk ini sudah ada?
- [ ] Tahap abstraksi — terlalu rendah (verbose) atau terlalu tinggi (over-engineered)?
- [ ] Single Responsibility — adakah setiap fungsi buat satu perkara?

**Kecekapan**
- [ ] N+1 queries atau loop tidak perlu
- [ ] Early returns yang boleh kurangkan nesting
- [ ] Alokasi tidak perlu

**Khusus Stack Master (Hono.js + D1 + CF Workers)**
- [ ] D1 queries guna parameterized binding?
- [ ] Route handlers structured dengan betul dalam Hono?
- [ ] Tiada operasi berat yang boleh slow down Worker?
- [ ] Frontend: input di-sanitize sebelum render ke DOM?

**Apa yang TIDAK perlu diflag**
- Pilihan gaya yang tidak affect correctness
- Dokumentasi hilang pada kod yang tidak diubah
- Micro-optimizations yang tidak penting
- "Best practices" yang tambah kompleksiti tanpa nilai

### Step 4: Report Findings

```
Refine: [skop]

Reviewed: [N] fail, [N] baris berubah

Found:
  [N] isu untuk fix (dead code, bug, N+1)
  [N] cadangan (boleh ditingkat, optional)
  [N] fail — bersih, tiada tindakan perlu

Details:
  [fail:baris] — [kategori] — [keterangan]
```

### Step 5: Fix (Dengan Izin)

Selepas bentangkan findings, **sentiasa tanya master sebelum apply fix**:

1. Bentangkan findings dengan format report
2. Tanya: "Fix semua? Fix critical sahaja? Skip?"
3. Selepas kelulusan → apply fix yang diminta
4. Verify — semak tiada yang rosak
5. Report apa yang diubah

| Kategori | Tindakan |
|----------|----------|
| Dead code, import tidak guna | Selamat fix — tiada perubahan behavior. Masih tanya dulu. |
| Bug yang jelas | Sentiasa tanya — terangkan bug dan fix yang dicadang |
| Pemudahan | Bentangkan sebagai cadangan — master decide |
| Peluang guna semula | Bentangkan sebagai cadangan — master decide |

## Mandatory Rules

1. **Faham intent dulu** — jangan "fix" sesuatu yang disengajakan
2. **Tanya sebelum fix** — bentangkan findings dulu, apply fix selepas kelulusan
3. **Fokus pada yang berubah** — jangan drift ke kod lama yang tidak disentuh
4. **Stack-aware** — semak D1, Hono, CF Workers specific issues

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Tiada isu ditemui | Report clean: "No issues found. Code is sharp." |
| Tiada git history | Tanya master fail mana nak di-review |
| Isu sistemik dijumpai | Cadang `sight-elemental` untuk scan lebih dalam |

## Level History

- **Lv.1** — Base: Review changed code (git diff), 4-kategori analisa, report + fix with permission, Hono/D1/CF Workers aware. Extracted from Observation System Refine tier. (Origin: Upstream MemoryCore Observation System install, 2026-04-03)
