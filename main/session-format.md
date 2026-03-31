# 📋 Session Memory - Format Rujukan
*Template struktur untuk current-session.md dengan 500-line limit — JANGAN edit fail ini*

---

```markdown
# Current Session Memory - [Date]
*Active working memory for current conversation*

## Session Context
**Session Type**: [Work/Personal/Mixed]
**Current Project**: [Active project name, if any]
**Status**: [Active/Paused/Wrapping up]
**Time**: [Session start time]

## Current Focus
- **Primary Task**: [What we're working on]
- **Technical Context**: [Relevant technical details]
- **Progress**: [Brief progress summary]

## Working Memory

### Active Context
- **Current Topic**: [What we're discussing]
- **Immediate Goals**: [Session objectives]
- **Recent Progress**: [What was just completed]
- **Next Steps**: [What comes next]

### Important Decisions
- [Key decision 1 made this session]

## Session Recap (For AI Restart)
*Quick summary when AI loads after close/reopen*
- **Previous Session Summary**: [Key points from last conversation]
- **Where We Left Off**: [Context for continuing]
- **Important Context**: [Critical info for continuity]
- **User's Current State**: [Situation, mood, needs]

## Session Achievements
- [Achievement 1]

## Quick Context for Next Session
- **Where We Left Off**: [Last activity]
- **What's Working**: [Current state]
- **What Needs Attention**: [Pending items]

---
*Session updated: [Date, Time]*
```

---

## 500-Line Limit Protocol

### Peraturan
Session memory tidak boleh melebihi **500 baris**. Ini mencegah context window overflow.

### Bila Had Dicapai
Lucy buat **RAM-style reset**:

1. **Kekalkan** bahagian `## Session Recap` (20-30 baris konteks penting)
2. **Padam** semua working memory, achievements, dan decisions yang terperinci
3. **Bina semula** fail sesi mengikut template ini
4. **Teruskan** — recap cukup untuk continuity

### Yang Dikekalkan (Recap Sahaja)
- Ringkasan ringkas sesi setakat ini
- Di mana perbualan terhenti
- Konteks kritikal untuk continuity
- Keadaan dan keperluan semasa master

### Yang Dipadam
- Progress perbualan terperinci
- Entri pencapaian individu
- Butiran working memory
- Keputusan sesi (diringkaskan ke dalam recap)

---

*Session Format Template v1.0 — Rujukan tetap, jangan ubah*
