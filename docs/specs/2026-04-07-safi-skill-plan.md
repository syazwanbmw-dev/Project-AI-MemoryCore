# SAFI — Code Warden, The Balance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bina skill `safi` — Balance Check dengan dua suara (Safi's Voice + Zaki's Voice) yang tanya "Is this clean enough?" dan "Is this necessary?" pada setiap kod review.

**Architecture:** Satu fail skill baru (`safi/SKILL.md`) dengan dual voice output format. Dua fail dikemaskini: `kata/SKILL.md` untuk pipeline integration, `identity-core.md` untuk skills list.

**Tech Stack:** Markdown skill files, Lucy plugin system, Git (push ke master branch GitHub)

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Create | `plugins/lucy-skills/skills/safi/SKILL.md` | Skill utama SAFI dengan dual voice |
| Modify | `plugins/lucy-skills/skills/kata/SKILL.md` | Tambah SAFI ke pipeline + skill table |
| Modify | `main/identity-core.md` | Tambah safi dalam skills tambahan list |

---

## Task 1: Cipta safi/SKILL.md

**Files:**
- Create: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\safi\SKILL.md`

- [ ] **Step 1: Cipta folder dan tulis SKILL.md dengan content berikut**

```markdown
---
name: safi
description: "Use when user says 'safi', '/safi', '/safi [target]', 'balance check',
             'terlalu complex', 'perlu ke ni?', 'is this necessary?', 'clean enough?'.
             Also auto-triggers after Hone/Hunt in large tasks before commit-seal.
             Balance Check skill — two voices always speak: Safi (clean enough?) and
             Zaki (necessary?). Advisory, not a hard blocker."
---

# SAFI ⚖ — Code Warden, The Balance
*Born from two apocalypses. One world died from too many rules. One died from none.*

## Activation

When this skill activates, output:

`SAFI ⚖ — The Balance awakens.`

Then determine scope before scanning.

## SAFI bukan Sight, bukan Refine

| Skill | Purpose |
|-------|---------|
| Eagle/Hone/Hunt | Cari bugs dan issues |
| Refine | Bersihkan kod yang berubah |
| **SAFI** | Balance check — clean enough? necessary? |
| commit-seal | Final gate sebelum push (mandatory) |

SAFI adalah **advisory** — master decide nak fix atau proceed. Berbeza dari commit-seal yang hard block.

## Scope Detection

### Kalau master taip `/safi` (tanpa target):
Tanya: "Nak SAFI check apa? (git diff / nama fail / nama feature)"

### Kalau master taip `/safi [target]`:
Terus scan target yang dinyatakan.

### Auto-trigger dalam pipeline (selepas Hone/Hunt):
Scan fail-fail yang terlibat dalam task semasa.

## The Two Voices

### Safi's Voice — "Is this clean enough?"

Semak aspek-aspek berikut:

1. **Naming** — variable, function, route names jelas dan accurate? Nama mesti describe "what", bukan "how"
2. **Readability** — logic boleh difahami tanpa baca implementation dalam? Kalau kena baca 3x, ada masalah
3. **Consistency** — ikut pattern yang sama dengan kod lain dalam projek? (naming convention, error handling style)
4. **Error handling** — errors di-handle dengan betul dan konsisten? Jangan silent fail
5. **Structure** — function terlalu panjang (>30 baris)? Perlu dipecah kepada fungsi lebih kecil?
6. **Comments** — ada comment yang explain "what" bukan "why"? Comment yang explain "why" = good. "What" = code smell

### Zaki's Voice — "Is this necessary?"

Semak aspek-aspek berikut:

1. **Dead code** — ada fungsi, route, atau variable yang defined tapi tak digunakan?
2. **Redundancy** — logic yang sama dibuat dua kali? DRY violation?
3. **Over-engineering** — ada abstraction, interface, atau pattern untuk kes yang belum wujud? YAGNI violation
4. **Unused imports** — import yang tak dipakai dalam fail?
5. **Complexity** — ada cara yang lebih simple untuk capai hasil sama? Less code = less bugs
6. **Config bloat** — ada setting, flag, atau option yang tak perlu atau tak digunakan?

## Output Format

```
SAFI ⚖ — [target/scope]
════════════════════════════
Safi: "Is this clean enough?"
  [!] [issue] — File: [path] Line: [XX]
      Fix: [cadangan penyelesaian]
  ✓  CLEAR

Zaki: "Is this necessary?"
  [!] [issue] — [deskripsi masalah]
      Fix: [cadangan penyelesaian]
  ✓  CLEAR
════════════════════════════
Verdict: BALANCED / [X] issues found
Advisory: Master decide — fix sekarang atau proceed?
```

## Core Rules

1. **Dua suara sentiasa bersuara** — walaupun satu CLEAR, kedua-dua mesti ada dalam output
2. **Advisory, bukan blocker** — SAFI report, master decide
3. **Kalau kedua-dua CLEAR** → `BALANCED`, teruskan ke commit-seal
4. **Kalau ada issues** → present findings, tunggu arahan master
5. **Stack-aware** — semak konsisten dengan Hono.js/D1/CF Workers patterns dalam projek

## Context Guard

| Context | Status |
|---------|--------|
| **Master taip `/safi`** | ACTIVE — tanya scope dulu |
| **Master taip `/safi [target]`** | ACTIVE — terus scan |
| **"balance check", "terlalu complex", "perlu ke ni?"** | ACTIVE |
| **Selepas Hone/Hunt dalam task besar** | ACTIVE — auto dalam pipeline |
| **Task kecil tanpa explicit call** | DORMANT |

## Level History

- **Lv.1** — Base: Dual voice balance check (Safi + Zaki), flexible scope, advisory verdict, Kata pipeline integration. (Origin: Adapted dari SAFI — Majitopia karya Ijam, 2026-04-07)
```

- [ ] **Step 2: Verify fail berjaya dicipta**

Semak fail wujud di `plugins/lucy-skills/skills/safi/SKILL.md` dan content lengkap.

---

## Task 2: Kemaskini kata/SKILL.md — Tambah SAFI

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\kata\SKILL.md`

- [ ] **Step 1: Baca fail sedia ada**

Baca `plugins/lucy-skills/skills/kata/SKILL.md` untuk pastikan context dan lokasi yang betul.

- [ ] **Step 2: Tambah SAFI dalam pipeline options**

Cari section `### Task Sederhana (dengan latent bug concern)` dan tambah pipeline baru SEBELUM section `### Task Besar`:

```markdown
### Task Besar (dengan balance check)
```
/plan → Code → sight-hone → safi → commit-seal → auto-commit
```
SAFI check balance selepas Hone — clean enough? necessary?
```

Cari section `### Pre-Production (recommended flow)` dan UPDATE jadi:

```markdown
### Pre-Production (recommended flow)
```
sight-hunt → safi → sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```
Hunt bugs → SAFI balance → Omnipotent full audit.
```

- [ ] **Step 3: Tambah SAFI dalam Sight Selection Guide**

Cari table Sight Selection Guide dan tambah row baru selepas Hunt:

```markdown
| Code terlalu complex / over-engineered? | SAFI ⚖ |
```

- [ ] **Step 4: Tambah Level History**

Cari Level History section dan tambah:

```markdown
- **Lv.3** — SAFI Integration: Tambah safi balance check ke pipeline options dan Sight table. (Origin: safi install, 2026-04-07)
```

---

## Task 3: Kemaskini identity-core.md

**Files:**
- Modify: `C:\Users\user\Documents\code\memory\main\identity-core.md`

- [ ] **Step 1: Baca fail sedia ada**

Baca `main/identity-core.md` untuk locate bahagian Skills tambahan.

- [ ] **Step 2: Tambah safi dalam skills list**

Cari baris:
```
- `sight-hunt` — hunt latent bugs dalam kod sedia ada (Axo pre-scout)
```

Tambah selepasnya:
```
- `safi` — balance check: clean enough? necessary? (Safi + Zaki dual voice)
```

---

## Task 4: Commit dan Push ke GitHub

**Files:** Semua fail yang dicipta/diubah

- [ ] **Step 1: Stage semua fail**

```bash
git add plugins/lucy-skills/skills/safi/SKILL.md
git add plugins/lucy-skills/skills/kata/SKILL.md
git add main/identity-core.md
git add docs/specs/2026-04-07-safi-skill-plan.md
```

- [ ] **Step 2: Commit**

```bash
git commit -m "Install safi (The Balance) — dual voice code quality check

=== PERUBAHAN KOD ===
• safi/SKILL.md: Skill baru — Safi's Voice (clean enough?) + Zaki's Voice (necessary?)
• kata/SKILL.md: Lv.3 — SAFI dalam pipeline options + Sight table
• identity-core.md: Tambah safi dalam skills tambahan list

=== KONTEKS SESI ===
• Project: Lucy MemoryCore | Type: skill install | Time: ~20 min
• Adapted dari SAFI — Majitopia karya Ijam"
```

- [ ] **Step 3: Push ke GitHub**

```bash
git push origin master
```

- [ ] **Step 4: Verify push berjaya**

Confirm output tunjuk `master -> master` dan commit hash baru.
