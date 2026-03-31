---
name: sight-eagle
description: "Use for quick code scan when user says 'eagle', 'quick scan', 'sight eagle',
             or needs a fast lightweight review after small changes. Uses Haiku-level
             thinking — fast, low token, catches obvious issues only."
---

# Sight Eagle — Quick Scan
*Imbasan cepat, ringan — tangkap isu yang jelas sahaja*

## Activation

When this skill activates, output:

`Eagle scanning...`

## Bila Guna Eagle

- CSS fix, typo, rename variable
- Perubahan kecil dalam satu fail
- Nak check cepat sebelum commit

## Scan Protocol

### Step 1: Kenal Pasti Skop
- [ ] Tanya master: "Fail mana yang baru diubah?"
- [ ] Fokus HANYA pada fail yang disebut — jangan scan seluruh projek

### Step 2: Quick Scan (4 Perkara Sahaja)
- [ ] **Syntax errors** — ada typo, bracket tak tutup, missing semicolon?
- [ ] **Logic yang jelas salah** — condition terbalik, variable undefined?
- [ ] **Console.log atau debug code** yang tertinggal
- [ ] **Perubahan yang tak disengajakan** — ada baris yang terpadam ke?

### Step 3: Report
```
Eagle Report:
PASS — tiada isu ditemui

atau

Eagle Report:
[!] Line XX: [deskripsi isu]
[!] Line XX: [deskripsi isu]
```

### Step 4: Cadangan
- Kalau tiada isu → "Clear for commit"
- Kalau ada isu kecil → fix terus
- Kalau isu lebih dalam → "Guna Hone untuk scan lebih dalam"

## Mandatory Rules

1. Jangan over-scan — Eagle bukan untuk deep review
2. Fokus pada fail yang berubah sahaja
3. Kalau jumpa isu yang perlu deep analysis → escalate ke Hone

## Level History

- **Lv.1** — Base: Quick scan 4 perkara, fokus fail berubah sahaja. (Origin: Kata Pipeline install, 2026-03-31)
