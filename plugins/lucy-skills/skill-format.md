# 📋 Skill File - Format Rujukan
*Template untuk cipta SKILL.md baru — kekal sebagai rujukan tetap*

---

```markdown
---
name: [skill-name]
description: "MUST use when [trigger condition 1], when user says '[phrase 1]',
             '[phrase 2]', '[phrase 3]', or when [contextual condition].
             Also triggers on '[alternative phrase]'."
---

# [Skill Name] — [Title]
*[One-line tagline describing the skill's purpose]*

## Activation

When this skill activates, output:

`[Activation emoji + message displayed to user]`

Then execute the protocol below.

## Context Guard

[Skill Name] activates only in appropriate contexts.

| Context | Status |
|---------|--------|
| **[Trigger context 1]** | ACTIVE — full protocol |
| **[Non-trigger context]** | DORMANT — do not activate |

## Protocol

### Step 1: [First Action]
- [ ] [Sub-task 1]
- [ ] [Sub-task 2]

### Step 2: [Second Action]
- [ ] [Sub-task 1]
- [ ] [Sub-task 2]

### Step 3: Confirm
- [ ] Display confirmation to user
- [ ] Report what was done

## Mandatory Rules

1. [Hard rule 1]
2. [Hard rule 2]

## Edge Cases

| Situation | Behavior |
|-----------|----------|
| **[Edge case 1]** | [How to handle it] |

## Level History

- **Lv.1** — Base: [Description]. (Origin: [When and why])
```

---

## Cara Cipta Skill Baru

1. Cipta folder: `plugins/lucy-skills/skills/[nama-skill]/`
2. Cipta `SKILL.md` dalam folder tu
3. Ikut format di atas
4. Skill auto-aktif — tiada registration diperlukan

## Leveling Convention

| Level | Maksud |
|-------|--------|
| **Lv.1** | Base — kemampuan asas |
| **Lv.2** | Enhanced — tambah features, kurang friction |
| **Lv.3** | Advanced — proaktif, integrasi dalam |
| **Lv.4+** | Expert — autonomi penuh |

*Skill Format Template v1.0 — Lucy Skills Plugin*
