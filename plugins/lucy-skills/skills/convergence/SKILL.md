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