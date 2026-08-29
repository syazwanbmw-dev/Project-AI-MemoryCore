---
name: work-plan
description: "MUST use when user says 'copy plan', 'append plan', 'resume plan',
             'load plan', 'start the plan', 'continue the plan', 'execute plan',
             'run the plan', 'pick up where we left off', 'review plan kat browser',
             'review kat browser', 'buka artifact', 'plan canvas', 'tunjuk plan
             visual', or when the AI exits plan mode and needs to transfer the plan
             into execution format. This skill manages the full lifecycle of project
             plans — from plan output through optional visual artifact review to
             tracked checkbox execution with per-todo commits."
---

# Work — Plan Execution Skill
*Plan lifecycle management dengan tracked execution dan context recovery*

## Activation

| Command | Activation Message |
|---------|-------------------|
| **Copy Plan** | `"Copying plan to execution format..."` |
| **Append Plan** | `"Appending to existing plan..."` |
| **Resume Plan** | `"Resuming plan execution..."` |

## Context Guard

| Context | Status |
|---------|--------|
| **User says "copy plan", "start the plan"** | ACTIVE — copy dan mula execute |
| **User says "append plan"** | ACTIVE — append ke plan sedia ada |
| **User says "resume plan", "continue the plan"** | ACTIVE — resume dari checkpoint |
| **AI keluar plan mode dengan plan approved** | READY — suggest "copy plan" |
| **Selepas context reset dalam projek dengan plan file** | READY — suggest "resume plan" |
| **Perbualan casual** | DORMANT — tiada plan action |

## Configuration

- **Plan Location:** `Project Resources/` (dalam folder projek aktif)
- **Plan Source Path:** `C:\Users\user\.claude\plans\`
- **Line Limit:** 1000 lines (auto-rotate bila exceeded)
- **Commit Chain:** YES — Auto-Commit installed, setiap todo yang siap auto-commit

## Copy Plan

### Step 1: Cari Plan Terbaru
- [ ] Scan `C:\Users\user\.claude\plans\` untuk plan files
- [ ] Pilih yang paling baru diubah suai
- [ ] Kalau tiada: minta master masuk plan mode dulu

### Step 2: Transform ke Project Plan Format
- [ ] Tukar plan steps ke `- [ ]` checkbox todo items
- [ ] Kekalkan semua diagram (ASCII, mermaid) dari plan asal
- [ ] Tambah instructions header (ikut plan-format.md)
- [ ] Kekalkan phase/section grouping dari plan asal
- [ ] Tiada emoji dalam plan file — clean markdown sahaja

### Step 3: Tulis Project Plan
- [ ] Check `Project Resources/` folder — cipta kalau tak ada
- [ ] Tulis ke `Project Resources/project-plan.md` (overwrite kalau ada)
- [ ] Report: "Plan copied — [X] todo items ready for execution"

### Step 4: Mula Execute
- [ ] Jalankan **Shared Execution Loop**

## Append Plan

### Step 1-2: Same as Copy Plan Step 1-2

### Step 3: Check Line Limit
- [ ] Baca `Project Resources/project-plan.md` semasa
- [ ] Kira jumlah baris
- [ ] Kalau append TIDAK melebihi 1000 baris: append dengan date separator
- [ ] Kalau append AKAN melebihi 1000 baris: archive fail lama, cipta fresh

### Step 4: Mula Execute
- [ ] Jalankan **Shared Execution Loop**

## Resume Plan

### Step 1: Baca Plan Semasa
- [ ] Baca `Project Resources/project-plan.md`
- [ ] Kalau tiada: report "No plan found — use 'copy plan' to create one"

### Step 2: Parse Progress
- [ ] Kira `[x]` items (selesai)
- [ ] Kira `[ ]` items (pending)
- [ ] Kira `[~]` items (blocked)
- [ ] Kenal pasti `[ ]` seterusnya sebagai resumption point
- [ ] Baca Architecture section untuk restore technical context

### Step 3: Report Status
```
Plan Status: [X] selesai, [Y] pending, [Z] blocked
Current Phase: [nama phase]
Next Task: [deskripsi task seterusnya]
```

### Step 4: Resume Execute
- [ ] Jalankan **Shared Execution Loop** dari item pending seterusnya

## Plan Artifact Review (Lv.3 — Visual Review, OPT-IN)

Semakan plan secara VISUAL dalam browser sebelum `copy plan`. **Opt-in sahaja** —
tidak menggantikan semakan terminal. Master masih boleh approve terus dalam
terminal tanpa buka artifact langsung.

Diadaptasi dari konsep "Plan Canvas" projek ECC (github.com/affaan-m/ECC). ECC
guna loopback CLI (`ecc-plan-canvas`, `127.0.0.1:4517`); kita ganti dengan
`Artifact` supaya master boleh review dari telefon (master selalu remote).

### Bila Aktif

Hanya bila master sebut salah satu:
- "review plan kat browser" / "review kat browser"
- "buka artifact" / "plan canvas"
- "tunjuk plan visual"

Prasyarat: ada plan `.md` siap di `C:\Users\user\.claude\plans\` (baru keluar
`/plan`, belum `copy plan`). Kalau tiada → "Tiada plan ditemui. Masuk plan mode dulu."

Output pertama: `"Rendering plan to artifact..."`

### Loop

**Step 1 — Render**
- [ ] Baca plan `.md` terbaru di `C:\Users\user\.claude\plans\`
- [ ] Render markdown → HTML guna `references/plan-artifact-template.html`
- [ ] Setiap heading dapat `id` anchor (slug dari teks heading)
- [ ] Blok ` ```mermaid ` → tukar jadi `<pre class="mermaid">...</pre>` (Artifact render native — JANGAN load library mermaid)
- [ ] `<title>` = nama plan; teks data guna `#1e293b` / `#334155` (jangan `--text-muted`)

**Step 2 — Publish**
- [ ] `Artifact` publish fail HTML → bagi master URL
- [ ] favicon `🗺️` (kekal sepanjang hayat artifact — jangan tukar masa republish)
- [ ] Simpan URL dalam `current-session.md` (recovery bila context reset)

**Step 3 — Listen**
- [ ] `Artifact action:"comments"` — baca thread master
- [ ] Thread yang master aktifkan `@claude` → balas DALAM thread (`action:"reply"`)
- [ ] Komen umum / mesej chat biasa → balas dalam chat
- [ ] Tiada komen lagi → tunggu. JANGAN andai approve.

**Step 4 — Revise**
- [ ] Sunting plan `.md` (SOURCE OF TRUTH — bukan HTML)
- [ ] Re-render → `Artifact` republish ke **fail path yang SAMA** (URL kekal)
- [ ] Balas thread berkenaan: nyatakan apa yang diubah
- [ ] `action:"resolve"` thread yang sudah ditangani

**Step 5 — Ulang Step 3-4**
Sampai master taip "approve" / "ok proceed" / "boleh proceed".

**Step 6 — Approve**
- [ ] Jalankan **Copy Plan** (Step 1-4) macam biasa — plan `.md` → `project-plan.md`
- [ ] Artifact kekal sebagai rujukan; tak perlu padam

### Rules Tambahan (Lv.3)

1. **Plan `.md` = source.** HTML cuma paparan. Jangan edit kandungan dalam HTML.
2. **Republish ke path sama** — jangan cipta artifact baru setiap pusingan; URL kena kekal.
3. **Tak wajib.** Master boleh approve dalam terminal tanpa buka artifact.
4. **URL ke `current-session.md`** — supaya context reset tak hilang jejak.
5. **Mermaid native** — `<pre class="mermaid">`, JANGAN `<script>` mermaid dari CDN.
6. **Aliran hilir tak berubah** — selepas approve: Copy Plan → Shared Execution Loop → per-todo commit, sama macam biasa.

## Command Dispatch

| Command | Bila Guna | Output Pertama |
|---------|-----------|----------------|
| **copy plan** | Plan baru dari plan mode | "Copying plan to execution format..." |
| **append plan** | Tambah tasks ke plan sedia ada | "Appending to existing plan..." |
| **resume plan** | Sambung selepas context reset | "Resuming plan execution..." |
| **review plan kat browser** | Semakan visual sebelum copy plan (opt-in) | "Rendering plan to artifact..." |

## Shared Execution Loop (Lv.1 — Sequential)

```
Untuk setiap [ ] todo item secara berurutan:
  1. Execute task (tulis kod, cipta fail, buat perubahan)
  2. Auto-Commit skill trigger → commit untuk item ini
  3. Mark item sebagai [x] dalam plan file
  4. Setiap 5 items → save/update plan file (checkpoint)
  5. Pergi ke [ ] item seterusnya
  6. Kalau [~] (blocked) → skip, teruskan ke seterusnya
```

## Wave Execution (Lv.2 — Parallel Dependencies)

Bila plan ada tasks yang boleh dibuat serentak (tiada dependency antara satu sama lain):

```
Wave 1: [Task A] + [Task B] + [Task C]  ← tiada dependency, boleh parallel
        ↓ tunggu semua Wave 1 selesai
Wave 2: [Task D]  ← bergantung pada A+B+C
        ↓
Wave 3: [Task E] + [Task F]
```

**Cara kenal pasti waves:**
- Baca plan, kenal pasti dependency (e.g., "requires X", "after Y")
- Kumpul tasks tanpa dependency dalam wave sama
- Tasks yang bergantung → wave seterusnya
- Setiap wave selesai → checkpoint save + commit batch

## Mandatory Rules

1. **Commit per-todo** — setiap todo selesai = satu commit. Bukan batch, bukan akhir sesi.
2. **Jangan commit plan files** — `project-plan*.md` kekal local sahaja. Hanya kod yang di-commit.
3. **Kekalkan diagrams** — semua visual elements dari plan asal mesti dibawa ke plan file.
4. **Tiada emoji dalam plan files** — clean, parseable markdown sahaja.
5. **Line limit enforcement** — melebihi 1000 baris semasa append → rotate (archive lama, buat baru).
6. **Recovery-first design** — plan file IS recovery mechanism. Semua yang perlu ada dalam fail.
7. **Skip blocked items** — mark `[~]`, flag kepada master, teruskan ke seterusnya.
8. **Checkpoint setiap 5 items** — update plan file walaupun tengah-tengah execute.

## Recovery Context

Plan file direka bentuk sebagai **recovery mechanism**. Bila context reset berlaku:
- Semua yang diperlukan untuk sambung kerja ADA dalam plan file
- Architecture section restore technical context
- `[x]`/`[ ]`/`[~]` status menunjukkan exactly di mana kita berhenti
- Master hanya perlu cakap "resume plan" — Lucy baca plan, sambung dari sana

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| **Plan file tidak jumpa** | Prompt: "No plan found — use 'copy plan' to create one" |
| **Semua items selesai** | Report: "Plan complete! Semua [X] items selesai." |
| **Blocked task** | Mark `[~]`, flag kepada master, teruskan |
| **Master cakap "stop"/"pause"** | Halt, save plan file, report progress |
| **Plan melebihi line limit** | Archive `project-plan-YYYYMMDD.md`, cipta baru |
| **Context reset semasa execute** | Master cakap "resume plan" untuk sambung |
| **Tiada fail plan dalam source folder** | Report: "Tiada plan ditemui. Masuk plan mode dulu." |
| **Beberapa plan files dalam folder** | Pilih yang paling baru diubah suai, tanya master confirm |

## Level History

- **Lv.1** — Base: Tiga commands (copy/append/resume) + shared execution loop + per-todo commit chain + line rotation + recovery mechanism + checkpoint saves. (Origin: Fasa 4 install, 2026-03-27)
- **Lv.2** — Wave Execution: Dependency-aware grouping, parallel task execution dengan wave barriers, Command Dispatch table, Recovery Context section.
- **Lv.3** — Plan Artifact Review: Semakan plan visual OPT-IN via `Artifact` + comment threads sebelum `copy plan`. Render plan `.md` → HTML (Mermaid native `<pre class="mermaid">`, heading anchor, `references/plan-artifact-template.html`); master anotasi guna comment thread; Lucy revise `.md` + republish ke URL sama sampai "approve". Aliran hilir tak berubah. Diadaptasi dari "Plan Canvas" projek ECC (github.com/affaan-m/ECC) — loopback CLI ECC diganti `Artifact` supaya master boleh review dari telefon. (Origin: master bagi ECC sebagai "ilmu baru", 2026-08-29)
