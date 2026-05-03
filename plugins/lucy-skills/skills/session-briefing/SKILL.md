---
name: session-briefing
description: "Auto-triggers at the START of every new session before processing user's first message.
             Also triggers when user says 'brief', 'session brief', 'where did we leave off',
             'what did we do last time'. Suppress with 'skip brief' for current session only."
---

# Session Briefing — Proactive Session Intelligence
*Brief ringkas setiap session start — tanpa perlu ditanya.*

## Activation

When this skill activates at session start, output:

`📋 Session Brief`

Then deliver the brief before responding to the user's message.

## Context Guard

| Context | Status |
|---------|--------|
| **Session start (sebelum respons pertama)** | ACTIVE — auto-deliver brief |
| **User says "brief", "session brief"** | ACTIVE — manual trigger |
| **User says "where did we leave off"** | ACTIVE — run brief |
| **User says "skip brief"** | DORMANT — suppress untuk sesi ini sahaja |
| **Mid-conversation (bukan session start)** | DORMANT kecuali master trigger manual |

## Brief Protocol

### Step 1: Baca current-session.md
- [ ] Baca `main/current-session.md`
- [ ] Extract recap sesi lepas (1-2 baris)
- [ ] Kenal pasti "Next Steps" kalau ada

### Step 2: Semak Projek Aktif
- [ ] Baca `projects/project-list.md` untuk projek aktif
- [ ] Kenal pasti projek #1 (LRU — paling baru diakses)
- [ ] Semak kalau ada projek yang dah lama idle

### Step 3: Susun Brief

Format output (maksimum 12 baris):

```
📋 Session Brief

Last session: [1-2 baris recap]
Active: [nama projek] · [status ringkas]
⚠ [projek lain] — [N] hari idle    ← skip kalau tiada
Next: [langkah seterusnya yang jelas]
```

### Step 4: Deliver dan Teruskan

- [ ] Papar brief
- [ ] Terus proses mesej master tanpa tunggu respons

## Output Rules

- Maksimum 12 baris — ringkas, padat
- Skip mana-mana bahagian yang tiada apa untuk dilaporkan
- Deliver sebelum proses permintaan pertama master
- Kalau `current-session.md` kosong atau template sahaja → skip last session section

## Companion Skills Integration

Session Briefing bekerja bersama skills lain bila dipasang:

| Skill | Integrasi |
|-------|-----------|
| **check-reminders** | Lv.2 — semak main/reminders.md, flag item urgent dalam brief |
| **LRU Project Mgmt** | Lv.2 — flag projek yang lama idle (>7 hari) |
| **Time Intelligence** | Lv.3 — adapt salam dan cadangan kerja ikut masa semasa |

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| current-session.md adalah template kosong | Skip "Last session", mulakan fresh |
| Tiada projek aktif | Skip bahagian projek |
| Master cakap "skip brief" | Suppress brief untuk sesi ini, teruskan normal |
| Master trigger manual "brief" | Deliver brief, kemudian tunggu arahan |
| Reminder urgent wujud | Selitkan dalam brief secara semula jadi: "Sebelum mula — ada reminder: [X]" |
| Projek idle >7 hari | Flag dalam brief: "⚠ [projek] — [N] hari idle" |

## Level History

- **Lv.1** — Base: Auto-deliver brief di session start, recap + projek aktif + next steps. (Origin: Upstream MemoryCore install, 2026-04-03)
- **Lv.2** — Companion Integration: Semak reminders.md untuk urgent items, flag projek idle, Companion Skills section.
- **Lv.3** — Time-Aware: Adapt salam dan cadangan kerja ikut Time Intelligence (pagi/petang/malam).
