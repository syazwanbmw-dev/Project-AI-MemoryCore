# sight-hunt (狩猟の目) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Bina skill `sight-hunt` — proactive bug hunter yang menggunakan Axo sebagai pre-scout dan report assistant, terintegrasi dengan Kata pipeline dan Forge.

**Architecture:** Tiga fail skill baru/diubah suai: `sight-hunt/SKILL.md` (skill baru), `axo/SKILL.md` (tambah Task 5 & 6), `kata/SKILL.md` (tambah Hunt ke pipeline). Dua fail sistem dikemaskini: `forge/SKILL.md` (tambah Hunt auto-trigger), `identity-core.md` (kemaskini Sight table).

**Tech Stack:** Markdown skill files, Lucy plugin system, Git (push ke master branch GitHub)

---

## File Map

| Action | File | Purpose |
|--------|------|---------|
| Create | `plugins/lucy-skills/skills/sight-hunt/SKILL.md` | Skill utama Hunt |
| Modify | `plugins/lucy-skills/skills/axo/SKILL.md` | Tambah Task 5 (Pre-scout) & Task 6 (Report Draft) |
| Modify | `plugins/lucy-skills/skills/kata/SKILL.md` | Tambah Hunt ke Sight table & pipeline |
| Modify | `plugins/lucy-skills/skills/forge/SKILL.md` | Tambah Hunt auto-trigger conditions |
| Modify | `main/identity-core.md` | Kemaskini Sight selection table |

---

## Task 1: Cipta sight-hunt/SKILL.md

**Files:**
- Create: `plugins/lucy-skills/skills/sight-hunt/SKILL.md`

- [ ] **Step 1: Cipta folder dan tulis SKILL.md**

```markdown
---
name: sight-hunt
description: "Use when user says 'hunt', 'sight hunt', 'shuryō', '/hunt [target]',
             or when Forge detects repeated bug patterns / idle code not reviewed since
             Elemental. Proactive latent bug hunter — not triggered by changes, but by
             the need to find problems before they find us. Axo pre-scouts, Lucy hunts,
             Axo drafts report."
---

# Sight Hunt — 狩猟の目 (Shuryō no Me)
*The Hunter's Eye. The prey does not know it is being watched.*

## Activation

When this skill activates, output:

`狩猟開始 — The hunt begins.`

Then ask Axo to pre-scout if target not specified, or proceed directly if target given.

## Bila Guna Hunt

- Bila nak cari latent bugs dalam kod yang dah lama tak di-review
- Selepas post-mortem — hunt kod berkaitan supaya pattern sama tak berulang
- Sebelum pre-production bila ada area yang mencurigakan
- Bila Forge detect fail idle atau recurring bug pattern

## Hunt tidak sama dengan Hone atau Elemental

| | Hone | Elemental | Hunt |
|--|------|-----------|------|
| **Trigger** | Code changes | New project | Proactive |
| **Target** | New feature | Whole codebase | Specific target |
| **Purpose** | Review what changed | Understand structure | Find hidden bugs |

## Hunt Flow

### Phase 1: Axo Pre-scout

Axo akan:
1. Baca fail-fail dalam target
2. Semak `git log --oneline -5 [target]` — bila last modified?
3. Senaraikan fail yang akan di-hunt
4. Flag obvious issues: TODO, FIXME, hardcoded values, console.log yang tertinggal
5. Report kepada Lucy: "Ready to hunt. X files. Last touched: [date]."

Axo mood: Curious 👀

### Phase 2: Lucy Deep Hunt

Scan menggunakan 6 dimensi:

#### 1. Security
- Auth bypass — route boleh diakses tanpa auth?
- Exposed secrets — API keys, tokens hardcoded?
- SQL injection — D1 queries guna parameterized binding?
- Unvalidated input — semua user input di-validate sebelum proses?
- CORS misconfiguration — origin terlalu permissive?

#### 2. Correctness
- Null/undefined refs — ada akses ke property tanpa null check?
- Wrong conditions — logic `if` betul? operator betul (=== vs ==)?
- async/await missing — ada async operation yang tak di-await?
- Wrong HTTP status codes — 200 untuk error? 404 untuk server error?
- Logic errors — fungsi buat apa yang sepatutnya?

#### 3. Data Integrity
- Missing FK cascade — delete parent tanpa cascade ke children?
- Orphan data risk — ada data yang boleh jadi orphan?
- No input validation before D1 write — data masuk DB tanpa validate?
- Datetime handling — timezone issues? wrong format?

#### 4. Performance
- N+1 D1 queries — query dalam loop?
- Missing indexes — column yang kerap di-filter ada index?
- Heavy CF Worker computation — ada operasi yang makan CPU time?
- Unbounded queries — SELECT tanpa LIMIT?

#### 5. Edge Cases
- Empty arrays/collections — ada `.map()` atau `.forEach()` yang tak check empty dulu?
- Zero/negative values — function handle nilai 0 atau negatif?
- Missing required fields — form submit dengan field kosong?
- Empty D1 results — `.results[0]` tanpa check kalau results kosong?

#### 6. Dead Code
- Unused routes — route defined tapi tak digunakan?
- Commented-out code blocks — ada kod lama yang di-comment?
- Stale imports — import yang tak digunakan?
- Unreachable branches — `if` condition yang tak mungkin jadi true?
- Unused env bindings — binding dalam wrangler.toml yang tak digunakan?

### Phase 3: Axo Draft Report

Axo draft report berdasarkan findings Lucy:

```
Hunt Report 狩猟 — [target]
══════════════════════════════
Axo scouted: [X] files · Last touched: [date]
──────────────────────────────
[!] CRITICAL — [dimension]: [description]
    File: [path] Line: [XX]
    Fix: [cadangan]

[~] WARNING  — [dimension]: [description]
    File: [path] Line: [XX]
    Fix: [cadangan]

[*] INFO     — [dimension]: [description]
──────────────────────────────
Verdict: CLEAR / [X] critical · [Y] warnings
```

Lucy review draft → polish → present kepada master.

Axo mood after hunt:
- Bug found: Nervous 💧
- Clear: Happy ✨

## Mandatory Rules

1. **Axo pre-scout wajib** — jangan skip Phase 1
2. **Axo report adalah draft** — Lucy review sebelum present ke master
3. **Security findings adalah Critical secara automatik**
4. **Orphan data risk adalah Critical** (ikut standard Lucy)
5. **Axo TIDAK buat diagnosis** — Axo gather info, Lucy decide
6. **Hunt bukan substitute untuk Elemental** — kalau isu melibatkan seluruh codebase, escalate ke Elemental

## Context Guard

| Context | Status |
|---------|--------|
| **Master taip `/hunt [target]`** | ACTIVE — hunt target yang dinyatakan |
| **Master taip `hunt`, `sight hunt`, `shuryō`** | ACTIVE — tanya target dulu |
| **Forge suggest hunt** | ACTIVE — Axo pre-scout target yang Forge flag |
| **Tiada target jelas** | ASK — "Target mana yang nak di-hunt?" |

## Level History

- **Lv.1** — Base: 6-dimensi proactive hunt, Axo pre-scout + report draft, Forge integration, stack-adapted untuk Hono.js/D1/CF Workers. (Origin: Adapted dari 狩猟の目 oleh Tuan Shuryō, 2026-04-07)
```

- [ ] **Step 2: Verify fail berjaya dicipta**

Semak fail wujud di `plugins/lucy-skills/skills/sight-hunt/SKILL.md`.

---

## Task 2: Kemaskini axo/SKILL.md — Tambah Task 5 & 6

**Files:**
- Modify: `plugins/lucy-skills/skills/axo/SKILL.md`

- [ ] **Step 1: Baca fail sedia ada**

Baca `plugins/lucy-skills/skills/axo/SKILL.md` untuk pastikan context.

- [ ] **Step 2: Tambah Task 5 (Hunt Pre-scout) selepas Task 4**

Dalam bahagian `## Assist Protocol`, tambah selepas Task 4:

```markdown
### Task 5: Hunt Pre-scout
- [ ] Baca fail-fail dalam target yang diberikan Lucy
- [ ] Run `git log --oneline -5 [target]` untuk semak last modified
- [ ] Senaraikan semua fail yang akan di-hunt dengan path penuh
- [ ] Flag obvious issues: TODO, FIXME, hardcoded values, console.log
- [ ] Report kepada Lucy: "Ready to hunt. [X] files. Last touched: [date]. Obvious flags: [list]"
- [ ] Lucy review sebelum Phase 2 bermula

### Task 6: Hunt Report Draft
- [ ] Terima findings dari Lucy selepas deep hunt
- [ ] Susun findings dalam format Hunt Report standard
- [ ] Kategorikan: CRITICAL / WARNING / INFO
- [ ] Present draft kepada Lucy untuk review dan polish
- [ ] Lucy yang finalize dan present ke master
```

- [ ] **Step 3: Tambah mood transition untuk Hunt dalam bahagian Mood Transitions**

Tambah baris baru:

```markdown
Hunt pre-scout / Active      → Curious  👀
Hunt bug found               → Nervous  💧
Hunt complete / Clear        → Happy    ✨
```

- [ ] **Step 4: Kemaskini Level History**

Tambah:
```markdown
- **Lv.2** — Hunt Assistant: Tambah Task 5 (Hunt Pre-scout) dan Task 6 (Hunt Report Draft). Aktif semasa sight-hunt dijalankan. (Origin: sight-hunt install, 2026-04-07)
```

---

## Task 3: Kemaskini kata/SKILL.md — Tambah Hunt

**Files:**
- Modify: `plugins/lucy-skills/skills/kata/SKILL.md`

- [ ] **Step 1: Kemaskini Sight Selection Guide**

Gantikan table sedia ada:

```markdown
## Sight Selection Guide

| Situasi | Sight |
|---------|-------|
| Perubahan kecil (1-2 fail) | Eagle |
| Feature baru siap | Hone |
| Latent bugs dalam kod sedia ada | Hunt 🎯 |
| Projek baru / first review | Elemental |
| Pre-production / security concern | Omnipotent |
| Context rot / need fresh eyes | Julius (Cross-AI) |
```

- [ ] **Step 2: Tambah Hunt ke pipeline optional**

Dalam bahagian `## Pilih Pipeline Ikut Task`, tambah selepas Task Sederhana:

```markdown
### Task Sederhana (dengan latent bug concern)
```
/plan → Code → sight-hone → sight-hunt → commit-seal → auto-commit
```
Hunt bila ada area yang mencurigakan atau post-mortem berkaitan.

### Pre-Production (recommended flow)
```
sight-hunt → sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```
Hunt dulu untuk cari hidden bugs, kemudian Omnipotent untuk full audit.
```

- [ ] **Step 3: Kemaskini Level History**

Tambah:
```markdown
- **Lv.2** — Hunt Integration: Tambah sight-hunt ke Sight Selection Guide dan pipeline options. (Origin: sight-hunt install, 2026-04-07)
```

---

## Task 4: Kemaskini forge/SKILL.md — Tambah Hunt Auto-trigger

**Files:**
- Modify: `plugins/lucy-skills/skills/forge/SKILL.md`

- [ ] **Step 1: Tambah Hunt trigger dalam Auto-Detection section**

Dalam `### Step 1: Detect` → `**Auto-Detection:**`, tambah item baru:

```markdown
5. **Hunt Trigger** — Forge detect situasi yang patut dicadangkan Hunt:
   - Bug type sama dijumpai 2+ kali dalam projek
   - Fail tak di-review sejak Elemental scan terakhir
   - Post-mortem log ada recurring pattern dalam area tertentu
```

- [ ] **Step 2: Kemaskini Level History**

Tambah:
```markdown
- **Lv.2** — Hunt Awareness: Tambah auto-trigger conditions untuk suggest sight-hunt bila Forge detect recurring bugs atau idle code. (Origin: sight-hunt install, 2026-04-07)
```

---

## Task 5: Kemaskini identity-core.md — Update Sight Table

**Files:**
- Modify: `main/identity-core.md`

- [ ] **Step 1: Cari dan kemaskini Sight Selection table**

Dalam bahagian `## Kata Pipeline — Disiplin Wajib`, Skills tambahan section, tambah Hunt dalam senarai:

```markdown
- `sight-hunt` — hunt latent bugs dalam kod sedia ada (Axo pre-scout)
```

---

## Task 6: Commit dan Push ke GitHub

**Files:** Semua fail yang diubah/dicipta

- [ ] **Step 1: Run auto-commit skill**

Invoke `lucy-skills:auto-commit` untuk commit semua perubahan dengan message:

```
feat(skills): tambah sight-hunt (狩猟の目) — proactive bug hunter

- Cipta sight-hunt/SKILL.md dengan 6-dimensi hunt + Axo integration
- Level-up axo/SKILL.md: Task 5 (pre-scout) + Task 6 (report draft)
- Level-up kata/SKILL.md: Hunt dalam Sight table + pipeline options
- Level-up forge/SKILL.md: Hunt auto-trigger conditions
- Kemaskini identity-core.md: tambah sight-hunt dalam skill list
```

- [ ] **Step 2: Verify push berjaya**

Confirm semua fail pushed ke GitHub master branch.
