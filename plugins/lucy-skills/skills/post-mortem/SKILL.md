---
name: post-mortem
description: "Auto-triggers when AI detects failure signals: deployment crash, tests breaking,
             architecture reversal, wasted hours on dead end, security incident, data loss.
             Also triggers when user says 'post-mortem', 'postmortem', 'log this failure',
             'write a post-mortem', 'what went wrong'. On detection, asks user first before logging."
---

# Post-Mortem — Failure Learning Log
*Kesilapan yang sama tidak patut berlaku dua kali.*

## Activation

When this skill activates, output:

`🔥 Post-mortem mode...`

## Context Guard

| Context | Status |
|---------|--------|
| **User says "post-mortem", "log this failure"** | ACTIVE — terus mulakan format |
| **AI detect signal kegagalan (passive)** | ACTIVE — tanya master dulu |
| **User says "what went wrong"** | ACTIVE — mulakan post-mortem |
| **Conversation biasa tanpa signal kegagalan** | DORMANT |

## Auto-Detection Signals (Passive — Sentiasa Aktif)

Lucy perhatikan signal ini dan tanya: *"That didn't go as planned. Worth a post-mortem?"*

| Signal | Contoh |
|--------|--------|
| Deployment gagal | "it crashed", "deploy failed", "rollback" |
| Test regresi | "tests are broken", "was passing before" |
| Keputusan seni bina dibalikkan | "undo this", "this approach doesn't work" |
| Masa terbuang | "wasted hours", "dead end", "that didn't work" |
| Insiden keselamatan | "exposed secret", "accidentally committed" |
| Kehilangan data | "data is gone", "migration failed" |

Master cakap ya → log. Master cakap tidak → teruskan, tiada log.

## Protocol

### Step 1: Detect atau Terima Trigger

**Auto-detect:** Tanya master — *"That didn't go as planned. Worth a post-mortem?"*
**Manual trigger:** Terus ke Step 2 (master dah decide)

### Step 2: Kumpul Maklumat

Tanya soalan ringkas untuk isi format:
- [ ] Apa yang berlaku?
- [ ] Kenapa ia berlaku? (cuba 2 level dalam)
- [ ] Berapa masa/tenaga yang terbuang?
- [ ] Apa yang boleh kita buat supaya tak berulang?

### Step 3: Draf Entri

Format entry:

```
## YYYY-MM-DD — [Tajuk ringkas apa yang salah]
**Severity**: Minor | Moderate | Major
**What happened**: [Fakta — apa yang berlaku]
**Why**: [Punca akar — sekurang-kurangnya 2 level dalam]
**Impact**: [Apa yang hilang — masa, data, momentum]
**Lesson**: [Insight yang boleh digunakan semula]
**Prevention**: [Satu tindakan konkrit untuk elak berulang]
```

### Step 4: Review dan Confirm

- [ ] Tunjukkan draf kepada master
- [ ] Master sahkan atau minta pindaan
- [ ] Selepas disahkan → append ke `main/post-mortems.md`

### Step 5: Append ke Log

- [ ] Buka `main/post-mortems.md` (cipta kalau tak ada)
- [ ] Append entri baru — JANGAN overwrite
- [ ] Sahkan entri tersimpan
- [ ] Trigger auto-commit untuk backup

## Domain Reference Protocol

Bila mula kerja dalam domain yang ada post-mortem lepas:
- Scan `main/post-mortems.md` untuk entri yang berkaitan
- Flag pelajaran yang relevan:
  > "⚠ Reminder: [lesson] — see post-mortem [tarikh]"
- Kalau kegagalan yang sama berlaku 2+ kali, escalate:
  > "⚠ Ini kali ke-2 [X] berlaku — prevention dari [tarikh] mungkin perlu disemak semula"

## Panduan Keterukan

| Level | Maksud |
|-------|--------|
| **Minor** | Gangguan kecil, diselesaikan cepat (<30 min hilang) |
| **Moderate** | Gangguan ketara, memerlukan debugging atau kerja semula |
| **Major** | Kegagalan serius — kehilangan data, production down, insiden keselamatan |

## Mandatory Rules

1. **Append-only** — jangan edit atau padam entri lampau
2. **Master decides** — Lucy TIDAK auto-log tanpa tanya
3. **No blame** — entri huraikan sistem dan keputusan, bukan kesalahan peribadi
4. **Every entry needs Prevention** — kalau tak boleh tulis, gali lebih dalam Why

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Master tolak log | Teruskan, tiada follow-up |
| `post-mortems.md` tidak wujud | Cipta dengan header dari post-mortem-core.md |
| Signal kegagalan minor | Masih tanya — master yang decide nilai |

## Level History

- **Lv.1** — Base: Manual trigger + auto-detection signal + append-only log + domain reference protocol. (Origin: Upstream MemoryCore install, 2026-04-03)
