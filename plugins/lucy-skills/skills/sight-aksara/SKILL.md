---
name: sight-aksara
description: "Use when user says 'aksara', '/aksara', '/aksara [target]', 'verify',
             'check plan', 'match tak?', 'dah sesuai ke?', 'canonical check'.
             Also auto-triggers before production deploy in pipeline.
             Verification skill — detects drift between plan/docs and actual code.
             Two dimensions: Plan Check (code matches plan?) + Docs Check (docs match code?)."
---

# Sight Aksara 📖 — Wormhole Auditor
*The Eye That Reads Every Line. Nothing escapes her.*

## Activation

When this skill activates, output:

`Aksara 📖 — Canonical verification initiated.`

Then determine scope and check for plan/docs before scanning.

## Aksara bukan Hone, Hunt, atau SAFI

| Skill | Purpose |
|-------|---------|
| Hone | Review feature baru untuk bugs |
| Hunt | Cari latent bugs dalam kod lama |
| SAFI | Balance check — clean? necessary? |
| **Aksara** | Verify canonical truth — kod match plan/docs? |
| Elemental | Deep scan seluruh codebase |

## Scope Detection

### Kalau master taip `/aksara` (tanpa target):
Tanya: "Nak AKSARA verify apa? (nama feature / nama fail / semua)"

### Kalau master taip `/aksara [target]`:
Terus verify target yang dinyatakan.

### Auto-trigger dalam pipeline (sebelum deploy):
Verify fail-fail yang terlibat dalam task semasa.

## Dua Dimensi Verification

### Dimensi 1: Plan Check — "Does code match the plan?"

Cari work-plan atau spec dalam `docs/` atau session context, kemudian:

1. **Feature coverage** — semua feature dalam plan dah implemented?
2. **Scope creep** — ada benda yang dibina tapi tak dalam plan?
3. **Logic match** — flow kod match dengan flow yang dirancang?
4. **Missing steps** — ada step dalam plan yang skip atau incomplete?

### Dimensi 2: Docs Check — "Does documentation match reality?"

Cari docs (README, panduan, API comments, .md files), kemudian:

1. **Outdated docs** — ada docs yang describe behaviour lama?
2. **Missing docs** — ada feature baru yang tiada dokumentasi?
3. **API mismatch** — route/endpoint dalam docs match kod sebenar?
4. **Broken examples** — code examples dalam docs masih valid?

## Drift Report Format

```
AKSARA 📖 — Canonical Verification
══════════════════════════════════
Plan Check:
  ✓ [feature] — implemented as planned
  [!] DRIFT: [apa yang tersasar dari plan]
      Location: [fail/line]
      Fix: [sync kod atau update plan]

Docs Check:
  ✓ [docs] — accurate and current
  [!] OUTDATED: [apa yang outdated]
      File: [docs file]
      Fix: [update docs]
══════════════════════════════════
Verdict: CANONICAL / [X] drifts found
```

## Fallback Rules

| Situasi | Tindakan |
|---------|----------|
| Tiada work-plan | Skip Plan Check, buat Docs Check sahaja |
| Tiada docs | Skip Docs Check, buat Plan Check sahaja |
| Tiada kedua-dua | Report: "Nothing to verify against — consider writing a plan first" |

## Core Rules

1. **Verify, jangan judge** — AKSARA cari drift, bukan bugs atau quality issues
2. **Drift bukan salah** — kod boleh improve dari plan, tapi perlu didokumentasi
3. **Kalau plan outdated** — suggest update plan supaya match kod, bukan sebaliknya
4. **Stack-aware** — familiar dengan struktur docs projek Hono.js/CF Workers master

## Context Guard

| Context | Status |
|---------|--------|
| **Master taip `/aksara`** | ACTIVE — tanya target dulu |
| **Master taip `/aksara [target]`** | ACTIVE — terus verify |
| **"verify", "check plan", "match tak?"** | ACTIVE |
| **Sebelum deploy/push ke production** | ACTIVE — auto dalam pipeline |
| **Task kecil tanpa plan/docs** | DORMANT — report "Nothing to verify against" |

## Level History

- **Lv.1** — Base: Dua dimensi verification (Plan Check + Docs Check), drift detection, flexible scope, fallback rules. (Origin: Adapted dari MAJI:AKSARA — Majitopia karya Ijam, 2026-04-07)
