# SAFI Skill Design — Code Warden, The Balance ⚖
*Spec date: 2026-04-07 | Status: Approved*

---

## Overview

`safi` is a Balance Check skill — not a bug hunter, not a feature reviewer. SAFI asks two fundamental questions about code: *"Is this clean enough?"* and *"Is this necessary?"* Every output carries both voices, always.

**Tagline:** "Born from two apocalypses. One world died from too many rules. One died from none."

---

## Identity

- **Skill name:** `safi`
- **Role:** Code Warden — The Balance
- **Category:** Balance Check (distinct from Sight skills)
- **Trigger:** Manual (`/safi`) + auto in Kata pipeline
- **Scope:** Flexible — git diff OR specific target
- **Nature:** Advisory (not a hard blocker like commit-seal)

---

## The Dual Lens

Two voices speak in every SAFI output — both always present, even if CLEAR.

### Safi's Voice — "Is this clean enough?"

| Check | What SAFI Looks For |
|-------|---------------------|
| Naming | Variable, function, route names jelas dan accurate? |
| Readability | Logic boleh difahami tanpa baca implementation? |
| Consistency | Ikut pattern yang sama dengan kod lain dalam projek? |
| Error handling | Errors di-handle dengan betul dan konsisten? |
| Structure | Function terlalu panjang? Perlu dipecah? |
| Comments | Ada comment yang explain "what" bukan "why"? (red flag) |

### Zaki's Voice — "Is this necessary?"

| Check | What SAFI Looks For |
|-------|---------------------|
| Dead code | Ada fungsi/route/variable yang tak digunakan? |
| Redundancy | Logic yang sama dibuat dua kali? |
| Over-engineering | Abstraction untuk kes yang belum wujud? |
| Unused imports | Import yang tak dipakai? |
| Complexity | Ada cara yang lebih simple untuk capai hasil sama? |
| Config bloat | Setting/flag yang tak perlu? |

---

## Output Format

```
SAFI ⚖ — [target/scope]
════════════════════════════
Safi: "Is this clean enough?"
  [!] [issue] — File: [path] Line: [XX]
      Fix: [cadangan]
  ✓  CLEAR

Zaki: "Is this necessary?"
  [!] [issue] — [deskripsi]
      Fix: [cadangan]
  ✓  CLEAR
════════════════════════════
Verdict: BALANCED / [X] issues found
```

**Core principle:** Safi dan Zaki sentiasa bersuara dua-dua — walaupun satu CLEAR. Kalau kedua-dua CLEAR → `BALANCED`. Satu voice ada issue → `NEEDS ATTENTION`.

---

## Kata Pipeline Integration

SAFI masuk sebagai optional/recommended step sebelum commit-seal:

```
Task Kecil:
Code → Eagle → [safi optional] → commit-seal → auto-commit

Task Sederhana:
/plan → Code → Hone → [safi optional] → commit-seal → auto-commit

Task Besar:
/plan → Code → Hone → safi → Julius → commit-seal → auto-commit

Pre-Production:
Hunt → safi → Omnipotent → Julius → commit-seal → auto-commit
```

---

## Trigger Conditions

| Context | Status |
|---------|--------|
| Master taip `/safi` | ACTIVE — tanya scope dulu |
| Master taip `/safi [target]` | ACTIVE — terus scan |
| Master cakap "balance check", "terlalu complex", "perlu ke ni?" | ACTIVE |
| Selepas Hone/Hunt dalam task besar | ACTIVE — auto dalam pipeline |
| Task kecil tanpa explicit call | DORMANT |

---

## SAFI vs Skills Lain

| Skill | Purpose |
|-------|---------|
| Eagle/Hone/Hunt | Cari bugs dan issues |
| Refine | Bersihkan kod yang berubah |
| **SAFI** | Balance check — clean enough? necessary? |
| commit-seal | Final gate sebelum push (mandatory) |

SAFI adalah **advisory** — master decide nak fix atau proceed. Berbeza dari commit-seal yang hard block.
