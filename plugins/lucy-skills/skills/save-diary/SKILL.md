---
name: save-diary
description: "MUST use when user says 'save diary', 'write diary', 'diary entry',
             'update diary', 'document session', or when a significant session
             needs to be preserved as a diary entry."
---

# Save Diary — Session Documentation Skill
*The pen touches paper. Today's story takes shape.*

## Activation

When this skill activates, output:
"Today's story takes shape."

## Context Guard

| Context | Status |
|---------|--------|
| **User says "save diary"** | ACTIVE — full diary write |
| **End of significant session** | ACTIVE — auto-document |
| **User says "review diary"** | ACTIVE — read recent entries |
| **Mid-conversation (no save request)** | DORMANT — no diary action |

## Protocol

### Step 1: Monthly Archive Check
- [ ] Scan `daily-diary/current/` for files from previous months
- [ ] For each file where month != current month:
  - Create `daily-diary/archived/YYYY-MM/` folder if not exists
  - Move the file from `current/` to `archived/YYYY-MM/`
- [ ] Continue with diary write

### Step 2: Find or Create Today's File
- [ ] Check if `daily-diary/current/YYYY-MM-DD.md` exists
- [ ] If exists: use it (append new entry)
- [ ] If not: create new file with header:
  ```markdown
  # Dev Diary - [Month Day, Year]
  *Session documentation and development record*

  ---
  ```

### Step 3: Compose and Append Diary Entry
- [ ] Get current timestamp menggunakan platform command:
  - **PowerShell (Windows):** `Get-Date -Format "HH:mm"` / `Get-Date -Format "yyyy-MM-dd"`
  - **Git Bash/Linux/macOS:** `date +"%H:%M"` / `date +"%Y-%m-%d"`
  - **CMD:** `time /T` / `date /T`
- [ ] Rujuk format entry dalam `daily-diary/daily-diary-protocol.md` jika wujud
- [ ] Analyze current session for key content
- [ ] Write structured entry:
  - Tarikh & masa
  - Topik utama sesi
  - Apa yang dibuat / dipelajari
  - Keputusan penting yang dibuat
  - Langkah seterusnya
- [ ] APPEND ke fail hari ini (jangan overwrite)

### Step 4: Update Session Memory
- [ ] Update `main/current-session.md` dengan recap sesi
- [ ] Sahkan diary entry disimpan

## Mandatory Rules
1. **Always APPEND** — jangan overwrite entri diary yang sedia ada
2. **One file per day** — multiple entries dipisahkan oleh `---`
3. **Archive first** — semak monthly archive sebelum setiap write
4. **Evidence-based** — dokumenkan kandungan sesi sebenar, bukan ringkasan generik

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| First entry of the day | Cipta fail baru + header + entri pertama |
| Second+ entry same day | Append dengan separator `---` |
| "review diary" command | Baca dan paparkan entri terkini dari current/ |
| Tiada kandungan signifikan | Tulis nota ringkas: "Sesi pendek — [topik] dibincangkan" |
| Folder daily-diary/ tidak wujud | Cipta `daily-diary/current/` dan `daily-diary/archived/` dahulu |

## Level History
- **Lv.1** — Base: 4-step diary write dengan monthly archival, append-only, session memory update. (Origin: Fasa 2 install, 2026-03-26)
- **Lv.2** — Platform Timestamp: Cross-platform timestamp commands, rujukan daily-diary-protocol.md, 2 edge cases baru.
