# convergence Skill Design — The Synthesis ◈
*Spec date: 2026-04-07 | Status: Approved*

---

## Overview

`convergence` is a meta-skill — not a review, but a synthesis. It tracks all review skills run in the current session and reports a confidence level before deploy. Inspired by MAJI:TRUE from Majitopia: when all eyes see the same truth, the form is complete.

**Tagline:** "When all eyes see the same truth, the form is complete."

---

## Identity

- **Skill name:** `convergence`
- **Category:** Meta-skill — synthesizes review results
- **Trigger:** Manual (`/convergence`) + Auto sebelum pre-production push
- **Output:** Confidence level + advisory (master decide)
- **Nature:** Advisory — master decides final action

---

## Reviews Tracked Per Session

| Review | Signal |
|--------|--------|
| `sight-hone` | CLEAR / NEEDS FIX |
| `sight-hunt` | CLEAR / issues found |
| `safi` | BALANCED / NEEDS ATTENTION |
| `sight-aksara` | CANONICAL / drifts found |
| `cross-ai-julius` | Second opinion result |

Lucy tracks all reviews run in the current session. When `/convergence` is invoked, Lucy synthesizes all results found in session context.

---

## Three Confidence Levels

| Level | Condition | Signal |
|-------|-----------|--------|
| **◈ FULL CONVERGENCE** | Semua required reviews CLEAR | Deploy dengan confidence tinggi |
| **◈ PARTIAL CONVERGENCE** | Sebahagian CLEAR, ada pending atau issues | Proceed dengan risiko terkawal |
| **◈ NO CONVERGENCE** | Tiada reviews run atau critical issues | Run reviews dulu |

---

## Required Reviews for FULL CONVERGENCE

| Task Size | Required Reviews |
|-----------|-----------------|
| Task kecil | Eagle/Refine CLEAR |
| Task sederhana | Hone CLEAR |
| Task besar | Hone + SAFI CLEAR |
| Pre-production | Hunt + SAFI + Aksara + Julius CLEAR |

---

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

---

## Kata Pipeline Integration

```
Task Besar (full pipeline):
/plan → Code → Hone → SAFI → Aksara → Julius → convergence → commit-seal → auto-commit

Pre-Production (mandatory):
Hunt → SAFI → Aksara → Omnipotent → Julius → convergence → commit-seal → auto-commit
```

---

## Trigger Conditions

| Context | Status |
|---------|--------|
| Master taip `/convergence` | ACTIVE — synthesize semua reviews dalam session |
| "ready deploy?", "confident?", "semua clear?" | ACTIVE |
| Sebelum pre-production push | ACTIVE — auto dalam pipeline |
| Tiada reviews run dalam session | ACTIVE — report NO CONVERGENCE |
| Task kecil (Eagle/Refine sahaja) | DORMANT |

---

## Convergence vs commit-seal

| | Convergence | commit-seal |
|--|-------------|-------------|
| **Soalan** | "Adakah kita confident?" | "Adakah checklist technical pass?" |
| **Nature** | Advisory | Mandatory gate |
| **Output** | Confidence level + recommendation | PASS / BLOCK |
| **Sequence** | Convergence dulu | Seal lepas Convergence |
