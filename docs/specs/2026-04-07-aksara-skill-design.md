# sight-aksara Skill Design — Wormhole Auditor 📖
*Spec date: 2026-04-07 | Status: Approved*

---

## Overview

`sight-aksara` is a Verification skill — not a bug hunter, not a quality check. AKSARA verifies **canonical truth**: does what we built actually match what we planned and documented? She detects drift before it becomes a problem.

**Tagline:** "The Eye That Reads Every Line. Nothing escapes her."

---

## Identity

- **Skill name:** `sight-aksara`
- **Role:** Wormhole Auditor — Canonical Verifier
- **Category:** Verification (Sight tier — distinct purpose from Review skills)
- **Trigger:** Manual (`/aksara`) + Auto in pipeline before deploy
- **Model:** Sonnet

---

## Position in Sight System

| Situasi | Sight |
|---------|-------|
| Perubahan kecil (1-2 fail) | Eagle |
| Feature baru siap | Hone |
| Latent bugs dalam kod sedia ada | Hunt 🎯 |
| Kod terlalu complex / over-engineered? | SAFI ⚖ |
| **Verify implementation match plan/docs** | **Aksara 📖** |
| Projek baru / first review | Elemental |
| Pre-production / security concern | Omnipotent |

---

## The Two Verification Dimensions

### Dimensi 1: Plan Check — "Does code match the plan?"

AKSARA baca work-plan atau spec (kalau ada), kemudian verify:

| Check | What AKSARA Looks For |
|-------|----------------------|
| Feature coverage | Semua feature dalam plan dah implemented? |
| Scope creep | Ada benda yang dibina tapi tak dalam plan? |
| Logic match | Flow kod match dengan flow yang dirancang? |
| Missing steps | Ada step dalam plan yang skip atau incomplete? |

### Dimensi 2: Docs Check — "Does documentation match reality?"

AKSARA baca docs (panduan, README, API comments), kemudian verify:

| Check | What AKSARA Looks For |
|-------|----------------------|
| Outdated docs | Ada docs yang describe behaviour lama? |
| Missing docs | Ada feature baru yang tiada dokumentasi? |
| API mismatch | Route/endpoint dalam docs match kod sebenar? |
| Broken examples | Code examples dalam docs masih valid? |

---

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

---

## Kata Pipeline Integration

```
Task Sederhana (ada plan):
/plan → Code → Hone → aksara → commit-seal → auto-commit

Task Besar (full pipeline):
/plan → Code → Hone → safi → aksara → Julius → commit-seal → auto-commit

Pre-Production (mandatory):
Hunt → safi → aksara → Omnipotent → Julius → commit-seal → auto-commit
```

---

## Trigger Conditions

| Context | Status |
|---------|--------|
| Master taip `/aksara` | ACTIVE — tanya target dulu |
| Master taip `/aksara [target]` | ACTIVE — terus verify |
| Master cakap "verify", "check plan", "match tak?" | ACTIVE |
| Sebelum deploy/push ke production | ACTIVE — auto dalam pipeline |
| Task kecil tanpa plan/docs | DORMANT — skip |

---

## Fallback Rules

| Situasi | Tindakan |
|---------|----------|
| Tiada work-plan | Skip Plan Check, buat Docs Check sahaja |
| Tiada docs | Skip Docs Check, buat Plan Check sahaja |
| Tiada kedua-dua | Report: "Nothing to verify against — consider writing a plan first" |
