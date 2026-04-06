# Hunt Skill Design — 狩猟の目 (Shuryō no Me)
*Spec date: 2026-04-07 | Status: Approved*

---

## Overview

`sight-hunt` is a proactive bug-hunting skill that stalks latent bugs in existing code. Unlike other Sight skills which are **reactive** (triggered by code changes), Hunt is **proactive** — it goes looking for problems before they surface in production.

**Tagline:** "The prey does not know it is being watched."

---

## Identity

- **Skill name:** `sight-hunt`
- **Alias:** 狩猟の目 (Shuryō no Me) — The Hunter's Eye
- **Model:** Sonnet (same as Hone/Elemental)
- **Trigger:** Manual (`/hunt`) OR Forge auto-suggest
- **Target:** Specific file OR feature (flexible)

---

## Position in Sight System

| Situasi | Sight |
|---------|-------|
| Perubahan kecil (1-2 fail) | Eagle |
| Feature baru siap | Hone |
| **Latent bugs dalam kod sedia ada** | **Hunt 🎯** |
| Projek baru / first review | Elemental |
| Pre-production / security concern | Omnipotent |

---

## Axo's Role

Axo participates in two phases under Lucy's supervision:

### Phase 1: Pre-scout
1. Baca target files
2. Semak git log — bila last modified?
3. Senaraikan fail yang akan di-hunt
4. Flag obvious issues (TODO, FIXME, hardcoded values)
5. Laporkan kepada Lucy: "Ready to hunt. X files, last touched Y days ago."

### Phase 2: Report Assistant
- Axo draft hunt report summary selepas Lucy's deep scan
- Lucy review dan polish
- Present final report kepada master

**Axo rules:** Axo hanya gather info dan draft. Lucy yang decide semua findings. Axo TIDAK buat diagnosis sendiri.

**Axo mood transitions:**
- Pre-scout: Curious 👀
- Bug found: Nervous 💧
- Hunt complete: Happy ✨

---

## The 6 Hunt Dimensions (Stack-Adapted)

Adapted untuk Hono.js + D1 + CF Workers + Vanilla JS:

| Dimension | What Hunt Looks For |
|-----------|---------------------|
| **Security** | Auth bypass, exposed secrets, SQL injection (D1), unvalidated input, CORS misconfiguration |
| **Correctness** | Null/undefined refs, wrong conditions, logic errors, async/await missing, wrong HTTP status codes |
| **Data Integrity** | Missing FK cascade, orphan data risk, no input validation before D1 write, datetime handling |
| **Performance** | N+1 D1 queries, missing indexes, heavy computation in CF Worker (CPU time limit), unbounded queries |
| **Edge Cases** | Empty arrays, zero/negative values, missing required fields, concurrent access, empty D1 results |
| **Dead Code** | Unused routes, commented-out blocks, stale imports, unreachable branches, unused env bindings |

---

## Hunt Flow

```
[Axo Pre-scout]
  1. Baca target files
  2. Semak git log — bila last modified?
  3. Senaraikan fail yang akan di-hunt
  4. Flag obvious issues (TODO, FIXME, hardcoded values)
  5. Laporkan: "Ready to hunt. X files, last touched Y days ago."

[Lucy Deep Hunt]
  → 6 dimensions scan pada target

[Axo Draft Report]
  → Axo draft hunt report summary
  → Lucy review dan polish
  → Present final report kepada master
```

---

## Hunt Report Format

```
Hunt Report 狩猟 — [target]
══════════════════════════════
Axo scouted: [X] files · Last touched: [N] days ago
──────────────────────────────
[!] CRITICAL — [dimension]: [description]
    File: [path] Line: [XX]
    Fix: [cadangan]

[~] WARNING  — [dimension]: [description]
──────────────────────────────
Verdict: CLEAR / [X] issues found
```

---

## Kata Pipeline Integration

**Optional step dalam task sederhana (dengan latent bug concern):**
```
/plan → Code → sight-hone → sight-hunt → commit-seal → auto-commit
```

**Pre-production (recommended):**
```
sight-hunt → sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```

**On-demand anytime:**
```
/hunt [target file atau feature name]
```

---

## Forge Integration

| Forge Detects | Action |
|---|---|
| Bug type sama dijumpai 2+ kali | Suggest `/hunt` pada kod berkaitan |
| Fail tak di-review sejak Elemental | Flag untuk Hunt |
| Post-mortem log ada recurring pattern | Suggest Hunt pada area berkaitan |

---

## Skill Trigger Conditions

| Context | Status |
|---------|--------|
| Master taip `/hunt [target]` | ACTIVE — manual hunt |
| Forge detect pattern/idle code | ACTIVE — auto-suggest hunt |
| Master taip `hunt`, `sight hunt`, `shuryō` | ACTIVE |
| Mid-task tanpa specific target | DORMANT — tanya target dulu |
