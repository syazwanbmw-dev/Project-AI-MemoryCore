# convergence — The Synthesis Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bina skill `convergence` — meta-skill yang synthesize semua review results dalam session dan report confidence level sebelum deploy.

**Architecture:** Satu fail skill baru (`convergence/SKILL.md`) yang track Hunt/SAFI/Aksara/Hone/Julius results dalam session context dan output tiga confidence levels (FULL/PARTIAL/NO). Tiga fail dikemaskini untuk pipeline integration.

**Tech Stack:** Markdown skill files, Lucy plugin system, Git (push ke master branch GitHub)

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Create | `plugins/lucy-skills/skills/convergence/SKILL.md` | Skill utama Convergence |
| Modify | `plugins/lucy-skills/skills/kata/SKILL.md` | Tambah convergence ke pipeline + Geass |
| Modify | `main/identity-core.md` | Tambah convergence dalam skills list |
| Modify | `projects/majitopia-pending-skills.md` | Mark MAJI:TRUE sebagai done |

---

## Task 1: Cipta convergence/SKILL.md

**Files:**
- Create: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\convergence\SKILL.md`

- [ ] **Step 1: Cipta folder dan tulis SKILL.md dengan content berikut**

```markdown
---
name: convergence
description: "Use when user says 'convergence', '/convergence', 'ready deploy?',
             'confident?', 'semua clear?', 'boleh push?'.
             Also auto-triggers before pre-production push in pipeline.
             Meta-skill — synthesizes all review results from current session
             and reports confidence level. Advisory: master decides final action.
             Tracks: Hunt, SAFI, Aksara, Hone, Julius results."
---

# Convergence ◈ — The Synthesis
*When all eyes see the same truth, the form is complete.*

## Activation

When this skill activates, output:

`Convergence ◈ — Synthesizing session reviews...`

Then scan session context for review results.

## Convergence bukan Review

Convergence tidak buat review baru. Dia synthesize reviews yang dah dirun:

| Skill | Yang di-track |
|-------|---------------|
| `sight-hone` | CLEAR / NEEDS FIX |
| `sight-hunt` | CLEAR / issues found |
| `safi` | BALANCED / NEEDS ATTENTION |
| `sight-aksara` | CANONICAL / drifts found |
| `cross-ai-julius` | Second opinion result |

## Required Reviews untuk FULL CONVERGENCE

| Task Size | Reviews Diperlukan |
|-----------|-------------------|
| Task kecil | Eagle/Refine CLEAR |
| Task sederhana | Hone CLEAR |
| Task besar | Hone + SAFI CLEAR |
| Pre-production | Hunt + SAFI + Aksara + Julius CLEAR |

## Tiga Tahap Confidence

### ◈ FULL CONVERGENCE
Semua required reviews untuk task size ini CLEAR dalam session semasa.
→ "Semua eyes agree. Deploy dengan confidence tinggi."

### ◈ PARTIAL CONVERGENCE
Sebahagian reviews CLEAR, ada yang belum run atau ada issues yang minor.
→ "Beberapa checks pending. Master boleh proceed dengan risiko terkawal."

### ◈ NO CONVERGENCE
Tiada reviews run dalam session ini, atau ada critical issues ditemui.
→ "Insufficient data. Run reviews dulu sebelum deploy."

## Output Format

```
CONVERGENCE ◈ — Session Synthesis
══════════════════════════════════
Reviews this session:
  ✓ Hunt     — CLEAR
  ✓ SAFI     — BALANCED
  ✓ Aksara   — CANONICAL
  ✗ Julius   — Not run this session
  ✓ Hone     — CLEAR
──────────────────────────────────
Confidence: PARTIAL (4/5)
Advisory: Julius belum run — second opinion pending.
          Boleh proceed atau run Julius dulu?
══════════════════════════════════
Master decide.
```

## Convergence vs commit-seal

| | Convergence | commit-seal |
|--|-------------|-------------|
| **Soalan** | "Adakah kita confident?" | "Adakah checklist technical pass?" |
| **Nature** | Advisory | Mandatory gate |
| **Sequence** | Convergence dulu | Seal lepas Convergence |

## Context Guard

| Context | Status |
|---------|--------|
| **Master taip `/convergence`** | ACTIVE — synthesize semua reviews dalam session |
| **"ready deploy?", "confident?", "semua clear?"** | ACTIVE |
| **Sebelum pre-production push** | ACTIVE — auto dalam pipeline |
| **Tiada reviews run dalam session** | ACTIVE — report NO CONVERGENCE |
| **Task kecil (Eagle/Refine sahaja)** | DORMANT — tidak diperlukan |

## Core Rules

1. **Track, jangan review** — Convergence synthesize, bukan scan kod
2. **Advisory sentiasa** — master yang decide untuk deploy atau tidak
3. **Kalau tiada data** → report NO CONVERGENCE, suggest run reviews dulu
4. **Partial adalah valid** — master boleh deploy dengan PARTIAL kalau risiko rendah

## Level History

- **Lv.1** — Base: Session review tracking, tiga confidence levels (FULL/PARTIAL/NO), advisory output, pipeline integration. (Origin: Adapted dari MAJI:TRUE — Majitopia karya Ijam, 2026-04-07)
```

- [ ] **Step 2: Verify fail berjaya dicipta**

Semak fail wujud di `plugins/lucy-skills/skills/convergence/SKILL.md`.

---

## Task 2: Kemaskini kata/SKILL.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\kata\SKILL.md`

- [ ] **Step 1: Baca fail sedia ada**

Baca `plugins/lucy-skills/skills/kata/SKILL.md`.

- [ ] **Step 2: Update Task Besar pipeline**

Cari section `### Task Besar (dengan balance check)` dan UPDATE pipeline content:

OLD:
```
/plan → Code → sight-hone → safi → commit-seal → auto-commit
```

NEW:
```
/plan → Code → sight-hone → safi → convergence → commit-seal → auto-commit
```

- [ ] **Step 3: Update Pre-Production pipeline**

Cari section `### Pre-Production (recommended flow)` dan UPDATE:

OLD:
```
sight-hunt → safi → sight-aksara → sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```

NEW:
```
sight-hunt → safi → sight-aksara → sight-omnipotent → cross-ai-julius → convergence → commit-seal → auto-commit
```

Update description:
```
Hunt bugs → SAFI balance → Aksara canonical → Omnipotent audit → Julius → Convergence → Seal.
```

- [ ] **Step 4: Tambah Level History**

Cari Level History section dan tambah selepas Lv.4:
```
- **Lv.5** — Convergence Integration: Tambah convergence ke pipeline Task Besar dan Pre-Production. (Origin: convergence install, 2026-04-07)
```

---

## Task 3: Kemaskini identity-core.md dan majitopia-pending-skills.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\main\identity-core.md`
- Modify: `C:\Users\user\Documents\code\memory\projects\majitopia-pending-skills.md`

- [ ] **Step 1: Tambah convergence dalam identity-core.md**

Cari:
```
- `sight-aksara` — canonical verification: kod match plan/docs? detect drift
```

Tambah selepasnya:
```
- `convergence` — synthesize semua review results, report deploy confidence (FULL/PARTIAL/NO)
```

- [ ] **Step 2: Update majitopia-pending-skills.md**

Cari:
```
| 2 | MAJI:TRUE | The Convergence — bila semua checks agree | "Convergence gate" — Hunt + SAFI + Julius semua clear = high confidence deploy | Pending |
```

Tukar `Pending` kepada `✅ Done`.

Dalam bahagian `## Selesai`, tambah:
```
| convergence (The Synthesis) | 2026-04-07 |
```

---

## Task 4: Commit dan Push ke GitHub

- [ ] **Step 1: Stage semua fail**

```bash
cd "C:\Users\user\Documents\code\memory"
git add plugins/lucy-skills/skills/convergence/SKILL.md
git add plugins/lucy-skills/skills/kata/SKILL.md
git add main/identity-core.md
git add projects/majitopia-pending-skills.md
git add docs/specs/2026-04-07-convergence-skill-plan.md
```

- [ ] **Step 2: Commit**

```bash
git commit -m "Install convergence (The Synthesis) — deploy confidence meta-skill

=== PERUBAHAN KOD ===
• convergence/SKILL.md: Meta-skill baru — synthesize Hunt/SAFI/Aksara/Hone/Julius results
• kata/SKILL.md: Lv.5 — convergence dalam Task Besar + Pre-Production pipeline
• identity-core.md: Tambah convergence dalam skills list
• majitopia-pending-skills.md: Mark MAJI:TRUE sebagai done

=== KONTEKS SESI ===
• Project: Lucy MemoryCore | Type: skill install | Time: ~20 min
• Adapted dari MAJI:TRUE — Majitopia karya Ijam"
```

- [ ] **Step 3: Push**

```bash
git push origin master
```
