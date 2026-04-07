# sight-aksara — Wormhole Auditor Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bina skill `sight-aksara` — Canonical Verifier yang detect drift antara plan/docs dengan kod sebenar, integrated dalam Kata pipeline sebelum deploy.

**Architecture:** Satu fail skill baru (`sight-aksara/SKILL.md`) dengan dua dimensi verification (Plan Check + Docs Check). Tiga fail dikemaskini: `kata/SKILL.md` untuk pipeline + Sight table, `identity-core.md` untuk skills list, `majitopia-pending-skills.md` untuk update status.

**Tech Stack:** Markdown skill files, Lucy plugin system, Git (push ke master branch GitHub)

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Create | `plugins/lucy-skills/skills/sight-aksara/SKILL.md` | Skill utama AKSARA |
| Modify | `plugins/lucy-skills/skills/kata/SKILL.md` | Tambah aksara ke pipeline + Sight table |
| Modify | `main/identity-core.md` | Tambah sight-aksara dalam skills list |
| Modify | `projects/majitopia-pending-skills.md` | Mark AKSARA sebagai selesai |

---

## Task 1: Cipta sight-aksara/SKILL.md

**Files:**
- Create: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\sight-aksara\SKILL.md`

- [ ] **Step 1: Cipta folder dan tulis SKILL.md dengan content berikut**

```markdown
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
```

- [ ] **Step 2: Verify fail berjaya dicipta**

Semak fail wujud di `plugins/lucy-skills/skills/sight-aksara/SKILL.md`.

---

## Task 2: Kemaskini kata/SKILL.md — Tambah AKSARA

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\kata\SKILL.md`

- [ ] **Step 1: Baca fail sedia ada**

Baca `plugins/lucy-skills/skills/kata/SKILL.md` untuk pastikan context.

- [ ] **Step 2: Tambah AKSARA dalam Sight Selection Guide table**

Cari baris ini dalam table:
```
| Kod terlalu complex / over-engineered? | SAFI ⚖ |
```

Tambah baris baru SELEPASNYA:
```
| Verify implementation match plan/docs | Aksara 📖 |
```

- [ ] **Step 3: Kemaskini pipeline — Task Sederhana (ada plan)**

Cari section `### Task Sederhana (dengan latent bug concern)` dan tambah pipeline baru SELEPAS section tersebut:

```
### Task Sederhana (ada plan — verify sebelum commit)
```
/plan → Code → sight-hone → sight-aksara → commit-seal → auto-commit
```
Aksara verify kod match plan sebelum commit.
```

- [ ] **Step 4: Kemaskini pipeline Pre-Production**

Cari section `### Pre-Production (recommended flow)` dan UPDATE pipeline:

```
### Pre-Production (recommended flow)
```
sight-hunt → safi → sight-aksara → sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```
Hunt bugs → SAFI balance → Aksara canonical check → Omnipotent full audit.
```

- [ ] **Step 5: Tambah Level History**

Cari Level History section dan tambah selepas Lv.3:
```
- **Lv.4** — Aksara Integration: Tambah sight-aksara ke Sight table + pipeline options. (Origin: sight-aksara install, 2026-04-07)
```

---

## Task 3: Kemaskini identity-core.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\main\identity-core.md`

- [ ] **Step 1: Tambah sight-aksara dalam skills list**

Cari baris:
```
- `safi` — balance check: clean enough? necessary? (Safi + Zaki dual voice)
```

Tambah SELEPASNYA:
```
- `sight-aksara` — canonical verification: kod match plan/docs? detect drift
```

---

## Task 4: Kemaskini majitopia-pending-skills.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\projects\majitopia-pending-skills.md`

- [ ] **Step 1: Update status AKSARA**

Cari baris:
```
| 1 | MAJI:AKSARA | Wormhole Auditor — "The Eye That Reads Every Line" | Verification skill — file audit, drift detection, canonical check sebelum deploy | Pending |
```

Tukar `Pending` kepada `Done`:
```
| 1 | MAJI:AKSARA | Wormhole Auditor — "The Eye That Reads Every Line" | Verification skill — file audit, drift detection, canonical check sebelum deploy | ✅ Done |
```

- [ ] **Step 2: Tambah dalam senarai Selesai**

Cari bahagian `## Selesai` dan tambah:
```
| sight-aksara (Wormhole Auditor) | 2026-04-07 |
```

---

## Task 5: Commit dan Push ke GitHub

- [ ] **Step 1: Stage semua fail**

```bash
cd "C:\Users\user\Documents\code\memory"
git add plugins/lucy-skills/skills/sight-aksara/SKILL.md
git add plugins/lucy-skills/skills/kata/SKILL.md
git add main/identity-core.md
git add projects/majitopia-pending-skills.md
git add docs/specs/2026-04-07-aksara-skill-plan.md
```

- [ ] **Step 2: Commit**

```bash
git commit -m "Install sight-aksara (Wormhole Auditor) — canonical verification skill

=== PERUBAHAN KOD ===
• sight-aksara/SKILL.md: Skill baru — Plan Check + Docs Check drift detection
• kata/SKILL.md: Lv.4 — Aksara dalam Sight table + pipeline options
• identity-core.md: Tambah sight-aksara dalam skills list
• majitopia-pending-skills.md: Mark AKSARA sebagai done

=== KONTEKS SESI ===
• Project: Lucy MemoryCore | Type: skill install | Time: ~20 min
• Adapted dari MAJI:AKSARA — Majitopia karya Ijam"
```

- [ ] **Step 3: Push**

```bash
git push origin master
```
