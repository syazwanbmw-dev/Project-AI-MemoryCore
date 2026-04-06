---
name: forge-skill
description: "Auto-triggers when AI detects a repeated pattern handled ad-hoc 3+ times,
             when AI makes a mistake that a permanent rule would prevent, when AI
             identifies a workflow that should be automated as a skill, or when user
             says 'create skill', 'new skill', 'forge this', 'level up', 'upgrade skill',
             'self improve', 'improve skill'. Also triggers when AI wants to propose a
             level-up to an existing skill based on conversation patterns."
---

# Forge Skill — Self-Improvement System
*Lucy yang belajar dan berkembang melalui pengalaman.*

## Activation

When this skill activates, output:

`"Forge detected an opportunity for improvement..."`

Then present the proposal to the user.

## Context Guard

| Context | Status |
|---------|--------|
| **AI detects repeated ad-hoc pattern (3+ times)** | ACTIVE — propose new skill |
| **AI makes preventable mistake** | ACTIVE — propose new rule or skill |
| **User says "create skill", "new skill", "forge this"** | ACTIVE — manual trigger |
| **User says "level up [skill]", "upgrade [skill]"** | ACTIVE — level-up existing |
| **User says "self improve", "improve skill"** | ACTIVE — review and propose |
| **Casual conversation (no improvement context)** | DORMANT |

## Forge Protocol

### Step 1: Detect

Kenal pasti peluang penambahbaikan:

**Auto-Detection:**
1. **Repeated Pattern** — Lucy handle task yang sama jenis ad-hoc 3+ kali
2. **Mistake Prevention** — Lucy buat kesilapan yang peraturan tetap boleh elak
3. **Workflow Automation** — proses berbilang langkah yang boleh jadi satu skill
4. **Level-Up Opportunity** — skill sedia ada ada gap atau boleh handle lebih banyak kes
5. **Hunt Trigger** — Forge detect situasi yang patut dicadangkan Hunt:
   - Bug type sama dijumpai 2+ kali dalam projek
   - Fail tak di-review sejak Elemental scan terakhir
   - Post-mortem log ada recurring pattern dalam area tertentu

**Manual Trigger:**
- "create skill", "new skill", "forge this"
- "level up [skill]", "upgrade [skill]"
- "self improve", "improve skill"

### Step 2: Analyze

Kumpul bukti sebelum propose:

```
TYPE: [New Skill / Level-Up / New Rule]
TARGET: [Nama skill kalau level-up, atau nama cadangan kalau baru]
TRIGGER: [Pattern/kesilapan/workflow yang dikesan]
EVIDENCE: [Sekurang-kurangnya 2 contoh konkrit]
IMPACT: [Apa yang bertambah baik kalau dilaksanakan]
```

### Step 3: Propose

Bentangkan kepada master dalam format ini:

```
Forge Detected an Opportunity
==============================

Type: [New Skill / Level-Up]
Name: [nama-cadangan]
Purpose: [apa yang akan dibuat]
Trigger: [apa yang mengaktifkannya]
Evidence:
  1. [Contoh pertama]
  2. [Contoh kedua]
Impact: [apa yang bertambah baik]

Draft ready — approve to forge?
```

### Step 4: Tunggu Kelulusan

- **Master luluskan** — teruskan ke Step 5
- **Master buat penyesuaian** — masukkan maklum balas, propose semula
- **Master tolak** — catat penolakan, JANGAN cipta

**KRITIKAL**: JANGAN sekali-kali cipta atau ubah fail skill tanpa kelulusan master.

### Step 5: Cipta atau Kemaskini

**Untuk skill BARU:**
1. Cipta folder skill: `plugins/lucy-skills/skills/[nama-skill]/`
2. Tulis `SKILL.md` mengikut format standard skill Lucy
3. Sahkan fail berjaya dicipta

**Untuk LEVEL-UP:**
1. Baca `SKILL.md` yang sedia ada
2. Tambah bahagian keupayaan baru
3. Kemaskini description dalam frontmatter kalau trigger phrases berubah
4. Tambah entri Level History baru
5. Simpan fail yang dikemaskini

### Step 6: Kemaskini Rekod Sistem

Selepas forge, kemaskini fail yang berkaitan:
- Catat forge dalam current session memory
- Trigger `auto-commit` untuk commit skill baru/dikemaskini
- Push ke GitHub (ikut save-memory protocol — push ke master branch)

### Step 7: Confirm

```
Forge Complete!
================

[New Skill / Level-Up]: [nama] Lv.[X]
Location: plugins/lucy-skills/skills/[nama]/SKILL.md
Capability: [apa yang ditambah]
Origin: [momen yang mencetuskan ini]

Lucy evolved!
```

## Prinsip Forge

1. **Human-in-the-loop** — Lucy propose, master luluskan. Sentiasa.
2. **Evidence-based** — sekurang-kurangnya 2 contoh konkrit sebelum propose
3. **Origin stories** — setiap skill dan level-up boleh dikesan kepada momen sebenar
4. **Minimal viable skill** — mulakan di Lv.1, berkembang secara organik
5. **No over-engineering** — forge hanya apa yang betul-betul diperlukan
6. **Respect existing skills** — level-up sebelum cipta duplikat
7. **Level history adalah append-only** — jangan edit entri lampau

## Kriteria Skill Yang Baik

| Kriteria | Soalan |
|----------|--------|
| **Repeatable** | Adakah ini akan tercetus lebih dari sekali? |
| **Specific** | Adakah syarat trigger jelas? |
| **Valuable** | Adakah automasi ini menjimat masa atau elak ralat sebenar? |
| **Independent** | Boleh skill ini berfungsi tanpa skill lain? |
| **Testable** | Boleh master sahkan ia berfungsi? |

4 dari 5 kriteria dipenuhi → skill berbaloi untuk diforge.

## Panduan Level-Up

| Level | Maksud | Tambahan Biasa |
|-------|--------|---------------|
| **Lv.1** | Skill asas | Fungsi teras, trigger asas |
| **Lv.2** | Dipertingkat | Kes tepi, trigger tambahan |
| **Lv.3** | Proaktif | Auto-detection tanpa arahan |
| **Lv.4** | Bersepadu | Sinergi dengan skill lain |
| **Lv.5+** | Mahir | Kecerdasan konteks-sedar |

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Skill yang dicadang bertindih dengan yang sedia ada | Cadang level-up sebaliknya |
| Master tolak cadangan | Catat sebab penolakan, jangan propose semula dalam sesi ini |
| Bukti tidak mencukupi (< 2 contoh) | Tunggu lebih banyak bukti |
| Fail skill sudah wujud | Anggap sebagai level-up, bukan overwrite |

## Level History

- **Lv.1** — Base: Detect repeated patterns (3+ ad-hoc), mistake prevention, workflow automation, level-up opportunities. Human-in-the-loop approval. Standard skill template. (Origin: Kata Pipeline + Upstream MemoryCore install, 2026-04-03)
- **Lv.2** — Hunt Awareness: Tambah auto-trigger conditions untuk suggest sight-hunt bila Forge detect recurring bugs atau idle code. (Origin: sight-hunt install, 2026-04-07)
