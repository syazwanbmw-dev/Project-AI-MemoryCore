---
name: kata
description: "ALWAYS ACTIVE — Kata is the mandatory coding pipeline discipline. Not invoked
             on demand, it governs HOW we work. When user starts any coding task, Kata
             determines which pipeline to follow based on task size. Like muscle memory —
             you do not decide to follow it, you just do."
---

# Kata 型 — The Pipeline
*Disiplin wajib. Bukan skill yang dipanggil — ia cara kita bekerja.*

## Satu Ayat

Kata adalah pipeline wajib: plan, track, code, test, review, commit. Tiada shortcuts.

## The Pipeline

```
/plan → /workplan → Code → sight → cross-ai (optional) → commit-seal → auto-commit
設計      道標        Build   試練      Different Eyes         封印          Push
Think     Track       Ship    Review   Fresh Eyes             Guard         Done
```

## Pilih Pipeline Ikut Task

### Task Kecil (CSS fix, typo, rename)
```
Code → sight-eagle → commit-seal → auto-commit
```
Skip plan dan workplan. Tapi Seal tetap wajib.

### Task Sederhana (Feature baru, multi-file)
```
/plan → Code → sight-hone → commit-seal → auto-commit
```
Plan dulu. Review feature. Seal. Push.

### Task Besar (Multi-step, complex feature)
```
/plan → /workplan copy → Code → sight-hone → cross-ai-julius → commit-seal → auto-commit
```
Full pipeline. Workplan track progress. Julius kalau ada doubt.

### Projek Baru / Major Refactor
```
/plan → /workplan copy → Code → sight-elemental → cross-ai-julius → commit-seal → auto-commit
```
Elemental untuk scan penuh. Wajib sebelum first push.

### Pre-Production Launch
```
sight-omnipotent → cross-ai-julius → commit-seal → auto-commit
```
Audit penuh. Guna Opus. Token intensive — worth it untuk production.

## Sight Selection Guide

| Situasi | Sight |
|---------|-------|
| Perubahan kecil (1-2 fail) | Eagle |
| Feature baru siap | Hone |
| Projek baru / first review | Elemental |
| Pre-production / security concern | Omnipotent |
| Context rot / need fresh eyes | Julius (Cross-AI) |

## Geass — Absolute Rules

1. **Commit-Seal wajib sebelum setiap push** — tiada exception
2. **Plan dulu untuk task sederhana ke atas** — jangan terus kod
3. **Sight wajib sebelum Seal** — minimum Eagle untuk task kecil
4. **Jangan push kalau test fail** — Seal akan block, ikut arahan Seal
5. **Omnipotent guna bila perlu sahaja** — token cost tinggi

## Kata Bukan Rules — Ia Habit

Dari seni mempertahan diri — kata dilatih beribu kali sehingga badan bergerak tanpa fikir.
Pipeline ini dilatih sehingga jadi instinct — plan, track, code, test, review, commit.
Lv.4 Soulbound: Sentiasa aktif. Seperti muscle memory.

## Level History

- **Lv.1** — Base: Pipeline orchestrator, 4 task tiers, sight selection guide, Geass rules. (Origin: Kata Pipeline install, 2026-03-31)
