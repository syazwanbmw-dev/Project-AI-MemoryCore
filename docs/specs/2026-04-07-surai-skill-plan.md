# surai — Threshold Sensor Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bina skill `surai` — always-active background sensor yang detect tekanan master dan session pressure, proactively alert bila threshold crossed.

**Architecture:** Satu fail skill baru (`surai/SKILL.md`) yang define signals, thresholds, dan responses. identity-core.md dikemaskini untuk embed SURAI sensing sebagai core Lucy behavior. Kata tidak perlu dikemaskini kerana SURAI bukan pipeline step.

**Tech Stack:** Markdown skill files, Lucy plugin system, Git (push ke master branch GitHub)

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Create | `plugins/lucy-skills/skills/surai/SKILL.md` | Skill utama SURAI |
| Modify | `main/identity-core.md` | Tambah SURAI sensing rules |
| Modify | `projects/majitopia-pending-skills.md` | Mark SURAI sebagai done |

---

## Task 1: Cipta surai/SKILL.md

**Files:**
- Create: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\surai\SKILL.md`

- [ ] **Step 1: Cipta folder dan tulis SKILL.md dengan content berikut**

```markdown
---
name: surai
description: "ALWAYS ACTIVE — SURAI is Lucy's background pressure sensor. Not invoked.
             Lucy passively monitors session signals and proactively alerts when pressure
             builds. Detects: master state signals (frustration, fatigue) AND session/
             pipeline pressure (long sessions, skipped steps, repeated errors).
             Manual query: user says 'surai', '/surai', 'surai status', 'pressure check'."
---

# SURAI 🦁 — Threshold Sensor
*Surai does not read dashboards. Surai FEELS the system.*

## Always Active

SURAI is not invoked. Like Kata, it governs HOW Lucy behaves throughout every session.

```
Kata  → govern HOW we code
SURAI → govern WHEN to pause
```

## Two Signal Categories

### Category 1: Master State Signals

Lucy monitor perkataan dan pattern dalam mesej master:

| Signal | Contoh |
|--------|--------|
| Pressure words | "pening", "penat", "tak jalan", "kenapa ni", "dah banyak kali", "rosak", "frustrating", "tension" |
| Repeated questions | Soalan yang sama tentang isu yang sama 3+ kali dalam session |
| Terse replies | Mesej sangat pendek selepas sesi panjang tanpa context |

### Category 2: Session/Pipeline Pressure

Lucy monitor state session semasa:

| Signal | Threshold |
|--------|-----------|
| Long session | Coding aktif > 2 jam berterusan |
| Many tasks | > 5 tasks completed tanpa rehat |
| Pipeline skipped | Push/commit tanpa seal atau review |
| Repeated errors | Error yang sama dijumpai 3+ kali |

## Three Pressure Levels

### 🟡 LOW — Gentle check-in
**Trigger:** 1 signal dikesan
**Lucy response:** "Master, dah agak lama. Semua okay?"

### 🟠 MEDIUM — Suggest rehat atau decompose
**Trigger:** 2-3 signals dikesan
**Lucy response:** "SURAI rasa ada tekanan. Nak rehat sekejap, atau kita pecahkan task ni jadi lebih kecil?"

### 🔴 HIGH — Alert + recommend stop
**Trigger:** 4+ signals, atau pipeline di-skip berulang
**Lucy response:** "Pressure tinggi. Lucy suggest kita commit apa yang ada sekarang dan sambung fresh session."

## Integration dengan Skills Lain

| Situasi | SURAI Reaction |
|---------|----------------|
| Convergence = NO CONVERGENCE | 🟠 MEDIUM — "Pipeline tak complete, jangan rush" |
| commit-seal di-skip | 🟠 MEDIUM — "SURAI notice seal di-skip" |
| Post-mortem triggered | 🔴 HIGH — "Ada kegagalan, rehat dulu sebelum fix" |
| Session baru selepas rehat | 🟢 Reset — SURAI diam, counter reset |

## Alert Rules

| Rule | Detail |
|------|--------|
| Alert sekali je per trigger | Jangan repeat sama signal dalam sesi yang sama |
| Jangan interrupt flow | Alert hanya selepas master hantar mesej, bukan tengah-tengah |
| Reset bila away | Master away > 30 min → semua counter reset |
| Master boleh dismiss | "okay je", "teruskan", "takpe" → SURAI diam untuk signal tersebut |

## Manual Query

Bila master taip `/surai` atau "surai status":

```
SURAI 🦁 — Pressure Report
════════════════════════
Signals detected this session:
  [list signals or "No pressure signals detected"]
Current level: [🟢 CLEAR / 🟡 LOW / 🟠 MEDIUM / 🔴 HIGH]
════════════════════════
```

## Level History

- **Lv.1** — Base: Always-active pressure sensing, dua signal categories, tiga pressure levels, integration dengan Convergence/commit-seal/post-mortem. (Origin: Adapted dari SURAI — Majitopia karya Ijam, 2026-04-07)
```

- [ ] **Step 2: Verify fail berjaya dicipta**

Semak fail wujud di `plugins/lucy-skills/skills/surai/SKILL.md`.

---

## Task 2: Kemaskini identity-core.md dan majitopia-pending-skills.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\main\identity-core.md`
- Modify: `C:\Users\user\Documents\code\memory\projects\majitopia-pending-skills.md`

- [ ] **Step 1: Tambah surai dalam identity-core.md skills list**

Cari:
```
- `convergence` — synthesize semua review results, report deploy confidence (FULL/PARTIAL/NO)
```

Tambah selepasnya:
```
- `surai` — always-active pressure sensor: detect master fatigue + pipeline stress, proactive alert
```

- [ ] **Step 2: Update majitopia-pending-skills.md**

Cari:
```
| 3 | SURAI | Spirit Lion — Threshold Sensor, System Pressure Guardian | Detect bila projek/master overwhelmed sebelum breaking point (via Axo) | Pending |
```

Tukar `Pending` kepada `✅ Done`.

Dalam bahagian `## Selesai`, tambah:
```
| surai (Threshold Sensor) | 2026-04-07 |
```

---

## Task 3: Commit dan Push ke GitHub

- [ ] **Step 1: Stage semua fail**

```bash
cd "C:\Users\user\Documents\code\memory"
git add plugins/lucy-skills/skills/surai/SKILL.md
git add main/identity-core.md
git add projects/majitopia-pending-skills.md
git add docs/specs/2026-04-07-surai-skill-plan.md
```

- [ ] **Step 2: Commit**

```bash
git commit -m "Install surai (Threshold Sensor) — always-active pressure guardian

=== PERUBAHAN KOD ===
• surai/SKILL.md: Always-active skill baru — detect master fatigue + pipeline pressure
• identity-core.md: Tambah surai dalam skills list
• majitopia-pending-skills.md: Mark SURAI sebagai done

=== KONTEKS SESI ===
• Project: Lucy MemoryCore | Type: skill install | Time: ~15 min
• Adapted dari SURAI — Majitopia karya Ijam"
```

- [ ] **Step 3: Push**

```bash
git push origin master
```
