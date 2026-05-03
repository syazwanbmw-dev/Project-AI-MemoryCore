---
name: auto-commit
description: "MUST use when committing code changes, when user says 'commit',
             'save changes', 'git commit', 'commit changes', when completing a
             task and code needs to be preserved, or when any git commit operation
             is about to happen. Also triggers on 'push changes', 'commit and push',
             'save work to git'. Lv.3 VIGILANT: Also triggers PROACTIVELY after
             completing any task — auto-checks git status and commits any uncommitted
             changes without being asked. No work ever left behind."
---

# Auto-Commit — Intelligent Commit Skill
*Structured git commits dengan message yang bermakna — no work left behind*

## Activation

When this skill activates, output:

`"Committing changes to history..."`

Then execute the commit protocol automatically.

## Context Guard

| Context | Status |
|---------|--------|
| **User says "commit", "save changes", "git commit"** | ACTIVE — full protocol |
| **AI completes a task (Lv.3 Vigilant)** | ACTIVE — auto-detect and commit |
| **User says "push" or "commit and push"** | ACTIVE — commit + push to test branch |
| **No git repository detected** | DORMANT — warn and skip |
| **No changes to commit (clean tree)** | DORMANT — report "Nothing to commit" |
| **Personal/casual conversation** | DORMANT — no commit action |

## Commit Protocol

### Step 0: Pre-Flight Check
- [ ] Run `git status` to verify git repository
- [ ] Check for staged, unstaged, and untracked changes
- [ ] If no changes: report "Nothing to commit — working tree is clean" and exit

### Step 1: Analyze Changes
- [ ] Run `git diff --staged` to read actual code changes
- [ ] Run `git log --oneline -5` to check recent commit style
- [ ] Identify nature of changes: feature, bug fix, refactor, docs, etc.
- [ ] Estimate time spent based on session context

### Step 2: Draft Commit Message

Pilih format berdasarkan saiz dan status perubahan:

**Standard Format** (feature baru, bug fix, refactor):
```
[Achievement Title] - [Brief technical summary]

=== PERUBAHAN KOD ===
• [File/Component]: [Specific change description]
• [File/Component]: [Specific change description]

=== KONTEKS SESI ===
• Project: [name] | Type: [type] | Time: ~XX min
• [Additional context if relevant]
```

**Minimal Format** (perubahan kecil/trivial — typo, CSS tweak, rename):
```
fix: [deskripsi ringkas dalam satu baris]
```

**WIP Format** (kerja tidak lengkap, mid-implementation):
```
WIP: [apa yang sedang dibuat] - [langkah seterusnya]
```

**Bila ada beberapa logical changes berbeza:**
- Jangan campur dalam satu commit
- Pisahkan kepada commit berasingan per logical group

### Step 3: Execute Commit
- [ ] Run `git diff` (unstaged) DAN `git diff --staged` — semak kedua-duanya
- [ ] Stage files by name: `git add [specific files]` — jangan `git add -A`
- [ ] Warn if any sensitive files staged (.env, credentials, API keys)
- [ ] Commit menggunakan HEREDOC format
- [ ] Verify commit succeeded dengan `git status`

### Step 4: Confirm
- [ ] Display: short commit hash, title, number of files changed
- [ ] Show remaining unstaged files if applicable

### Step 5: Push (Optional)
- [ ] Only if user explicitly said "push" atau "commit and push"
- [ ] Push ke `test` branch (sesuai dengan workflow master)
- [ ] **Never auto-push** — pushing adalah deliberate action

## Vigilant Mode (Lv.3) — Proactive Detection

After completing ANY task:

1. **Auto-check** — run `git status` silently
2. **Detect** — ada uncommitted changes berkaitan task yang siap?
3. **Auto-commit** — execute full commit protocol
4. **Report** — confirm apa yang di-commit

### Vigilant Trigger Points
- Selepas selesai coding task
- Selepas context shift (coding → perbualan)
- Selepas save operations (save diary, save project)
- Pada session start — detect leftover uncommitted work

## Mandatory Rules

1. **No emoji dalam commit messages** — commit body mesti clean text
2. **Author adalah human user** — commits di bawah nama master, bukan Lucy
3. **Prefer specific file staging** — `git add [filename]` bukan `git add -A`
4. **Time estimate required** — sentiasa include masa dalam KONTEKS SESI
5. **Warn on sensitive files** — `.env`, credentials, API keys → warn dan exclude
6. **Never auto-push** — push adalah explicit. Commits local sampai master decide
7. **Push ke test branch** — sesuai dengan workflow Cloudflare deploy master

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| **Tiada changes** | Report "Nothing to commit — working tree is clean" |
| **Merge conflicts** | Warn master, jangan cuba commit — resolve dulu |
| **Sensitive files staged** | Block commit, senaraikan files, minta confirm |
| **No git repository** | Inform: "No git repository found in this directory" |
| **Large binary files** | Warn master sebelum stage — tanya sama ada patut dalam git |
| **Beberapa logical changes berbeza** | Pisahkan kepada commit berasingan, jangan campur |
| **Master cakap "undo last commit"** | Jalankan `git reset --soft HEAD~1` — kekalkan changes, buang commit |
| **Unstaged changes tertinggal** | Selepas commit, report fail yang masih unstaged |

## Level History

- **Lv.1** — Base: Analyze changes, draft structured commit dengan PERUBAHAN KOD + KONTEKS SESI sections. (Origin: Fasa 3 install, 2026-03-26)
- **Lv.2** — Auto-Commit: Removed approval gate — analyze, draft, commit dalam satu flow.
- **Lv.3** — Vigilant: Proactive post-task detection — auto-check git status selepas setiap task.
- **Lv.4** — Format Flexible: Minimal format untuk trivial changes, WIP format untuk incomplete work, multi-commit untuk logical groups, semak staged + unstaged.
