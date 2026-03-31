# Plan File - Format Rujukan
*Template struktur untuk project-plan.md — rujukan tetap, jangan ubah*

---

```markdown
# Project Plan - [Project Name]
Created: [YYYY-MM-DD]
Source: [nama fail plan atau "manual"]

## Instructions
- Auto-commit kod selepas setiap todo item selesai (chain dengan Auto-Commit)
- Update fail ini setiap 5 items selesai (checkpoint save)
- JANGAN commit fail plan ini — ia adalah working reference Lucy

## Architecture
[Optional: diagram, wireframes, ASCII art, mermaid dari plan asal]
[Kekalkan semua visual elements — membantu recovery selepas context reset]

## Implementation Plan

### Phase 1: [Nama Phase]
- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

### Phase 2: [Nama Phase]
- [ ] Task 4
- [ ] Task 5

## Progress Log

[Tarikh] - [Ringkasan items yang siap sesi ini]
```

---

## Checkbox Convention

| Symbol | Maksud |
|--------|--------|
| `- [ ]` | Pending — belum dimulakan |
| `- [x]` | Selesai — siap dan di-commit |
| `- [~]` | Blocked — ditandakan, skip buat masa ini |

## Line Limit Rule
- Maximum **1,000 baris** per plan file
- Bila melebihi semasa append:
  1. Rename fail semasa ke `project-plan-YYYYMMDD.md` (archived)
  2. Cipta `project-plan.md` baru dengan content baru
  3. Report: "Previous plan archived, new plan created"

## Resume Convention
Selepas context reset, `"resume plan"` restore semua:
1. Kira `[x]` — tahu apa dah siap
2. Jumpa `[ ]` pertama — tahu di mana nak resume
3. Baca Architecture — restore technical context
4. Semak Progress Log — faham aktiviti sesi terbaru

## Nota Penting
- Fail plan TIDAK di-commit — kekal sebagai working reference Lucy
- Kod yang ditulis semasa execute AKAN di-commit (via Auto-Commit)
- Plan file adalah satu-satunya sumber kebenaran untuk recovery

---
*Plan Format Template v1.0 — Lucy Work Plan System*
