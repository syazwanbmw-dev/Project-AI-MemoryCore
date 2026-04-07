# surai Skill Design — Threshold Sensor 🦁
*Spec date: 2026-04-07 | Status: Approved*

---

## Overview

`surai` is an always-active background sensor — Lucy passively monitors session signals and proactively alerts when pressure builds. Not invoked, not reactive. SURAI FEELS before dashboards show it.

**Tagline:** "Surai does not read dashboards. Surai FEELS the system."

---

## Identity

- **Skill name:** `surai`
- **Category:** Always-active background sensing (like Kata)
- **Trigger:** Auto-detect from session signals — never invoked
- **Nature:** Proactive — Lucy speaks first
- **Manual query:** `/surai` or "surai status" → Lucy reports current pressure level

---

## Two Signal Categories

### Category 1: Master State
| Signal | Description |
|--------|-------------|
| Pressure words | "pening", "penat", "tak jalan", "kenapa ni", "dah banyak kali", "rosak", "frustrating" |
| Repeated questions | Same issue asked 3+ times in session |
| Terse messages | Very short replies after a long active session |

### Category 2: Session/Pipeline Pressure
| Signal | Description |
|--------|-------------|
| Long session | Active coding > 2 hours continuous |
| Many tasks | > 5 tasks completed without break |
| Pipeline skipped | Push without seal/review |
| Repeated errors | Same error encountered 3+ times |

---

## Three Pressure Levels

| Level | Trigger | Lucy Response |
|-------|---------|---------------|
| 🟡 LOW | 1 signal detected | "Master, dah agak lama. Semua okay?" |
| 🟠 MEDIUM | 2-3 signals detected | "SURAI rasa ada tekanan. Nak rehat sekejap, atau kita pecahkan task ni jadi lebih kecil?" |
| 🔴 HIGH | 4+ signals, atau pipeline di-skip berulang | "Pressure tinggi. Lucy suggest kita commit apa yang ada sekarang dan sambung fresh session." |

---

## Interaction with Existing Skills

| Situasi | SURAI Reaction |
|---------|----------------|
| Convergence = NO CONVERGENCE | 🟠 MEDIUM — "Pipeline tak complete, jangan rush" |
| commit-seal di-skip | 🟠 MEDIUM — "SURAI notice seal di-skip" |
| Post-mortem triggered | 🔴 HIGH — "Ada kegagalan, rehat dulu sebelum fix" |
| Session baru selepas rehat | 🟢 Reset — SURAI diam, fresh start |

---

## Alert Rules

| Rule | Detail |
|------|--------|
| Alert sekali je per trigger | Jangan repeat sama signal dalam sesi yang sama |
| Jangan interrupt flow | Alert hanya selepas master hantar mesej |
| Reset bila master rehat | Away > 30 min → counter reset |
| Master boleh dismiss | "okay je" atau "teruskan" → SURAI diam untuk signal tersebut |

---

## SURAI vs Kata

```
Kata  → govern HOW we code
SURAI → govern WHEN to pause
```

Both always-active. Different domains. Complementary.

---

## Implementation Files

- `surai/SKILL.md` — defines signals, thresholds, responses
- `main/identity-core.md` — tambah SURAI sensing rules ke Lucy core behavior
