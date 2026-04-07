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
