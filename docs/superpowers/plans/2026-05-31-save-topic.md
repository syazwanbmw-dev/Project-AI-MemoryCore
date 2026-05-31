# Save Topic Skill Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Adapt upstream Topic Diary System (Kiyoraka PR#8) ke dalam Lucy sebagai skill baru `save-topic` — untuk simpan knowledge merentasi sesi berdasarkan subjek, bukan kronologi.

**Architecture:** Skill standalone baru (`save-topic/SKILL.md`) dengan folder `topic-diary/` dalam `memory/`. Berasingan sepenuhnya dari `save-diary`. Update `identity-core.md` untuk daftarkan skill baru.

**Tech Stack:** Markdown files only — tiada kod, tiada dependencies.

**Spec:** `memory/docs/superpowers/specs/2026-05-31-save-topic-design.md`

---

### Task 1: Cipta Folder Structure + Index

**Files:**
- Create: `memory/topic-diary/index.md`

- [ ] **Step 1: Cipta fail index.md**

  Tulis kandungan ini ke `C:\Users\user\Documents\code\memory\topic-diary\index.md`:

  ```markdown
  # Topic Diary — Index
  *Buku ilmu tersusun mengikut subjek. "The index is the map; topic files are the memory."*

  ---

  ## Active Topics

  | Topik | Fail | Keywords | Last Updated |
  |-------|------|----------|--------------|

  ## Archived Topics

  | Topik | Fail | Sebab Arkib | Tarikh |
  |-------|------|-------------|--------|
  ```

- [ ] **Step 2: Verify fail wujud dan kandungan betul**

  Baca semula `topic-diary/index.md` dan pastikan:
  - Header ada
  - Dua jadual ada (Active + Archived)
  - Kedua-dua jadual kosong (belum ada topics)

- [ ] **Step 3: Commit**

  ```
  git add memory/topic-diary/index.md
  git commit -m "feat(lucy): add topic-diary folder + empty index"
  ```

---

### Task 2: Cipta Skill save-topic

**Files:**
- Create: `memory/plugins/lucy-skills/skills/save-topic/SKILL.md`

- [ ] **Step 1: Cipta SKILL.md**

  Tulis kandungan ini ke `C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\save-topic\SKILL.md`:

  ````markdown
  ---
  name: save-topic
  description: "MUST use when user says 'save topic [nama]', 'simpan topik [nama]', 'topic [nama]',
               'remember under [topic]', 'review topic [nama]', 'list topics', atau 'senarai topik'."
  ---

  # Save Topic — Topic Diary Skill
  *Buku ilmu yang hidup — tersusun mengikut subjek, bukan tarikh.*

  ## Activation

  When this skill activates, output:
  "Buku ilmu dibuka."

  ## Context Guard

  | Context | Status |
  |---------|--------|
  | **User says "save topic [nama]"** | ACTIVE — save insight ke topik |
  | **User says "simpan topik [nama]"** | ACTIVE — save insight ke topik |
  | **User says "topic [nama]"** (shorthand) | ACTIVE — save insight ke topik |
  | **User says "remember under [topic]"** | ACTIVE — save insight ke topik |
  | **User says "review topic [nama]"** | ACTIVE — baca dan summarize topik |
  | **User says "list topics" / "senarai topik"** | ACTIVE — papar index |
  | **Ambiguous "save" tanpa destination** | ASK — tanya destination dulu |
  | **Mid-conversation (no trigger)** | DORMANT |

  ## Protocol

  ### Save Flow

  - [ ] **Step 1: Determine topic name**
    - Detect nama topik dari request master
    - Kalau tiada → tanya: "Topik apa yang nak disimpan?"

  - [ ] **Step 2: Normalize topic name**
    - Convert ke kebab-case: `Cloudflare Workers` → `cloudflare-workers`
    - File path: `topic-diary/topics/[kebab-case].md`

  - [ ] **Step 3: Check atau cipta fail topik**
    - Jika `topic-diary/topics/[kebab].md` wujud → append je
    - Jika fail baru → cipta dengan header:
      ```markdown
      # [Nama Topik]
      *Created: YYYY-MM-DD*

      ---
      ```

  - [ ] **Step 4: Compose entry**
    ```markdown
    ## [HH:MM, Hari, DD Bulan YYYY] — [Tajuk ringkas]

    **Context:** [Kenapa knowledge ni timbul / projek apa]
    **Discovery:** [Apa yang dipelajari / ditemui]
    **Details:** [Kod/command exact yang terbukti kerja]
    **Pitfalls:** [Kesilapan biasa yang perlu elak]
    **Keywords:** [tag1, tag2, tag3]

    ---
    ```

  - [ ] **Step 5: APPEND ke fail topik**
    - Jangan overwrite entri lama
    - Sensitive content (password, token, secret) → redact atau tanya master dulu

  - [ ] **Step 6: Update index.md**
    - Row baru jika topik baru
    - Update kolum `Last Updated` jika topik sedia ada
    ```markdown
    | [Nama Topik] | topics/[kebab].md | [keywords] | [YYYY-MM-DD] |
    ```

  ### Review Flow

  - [ ] Baca `topic-diary/topics/[kebab].md`
  - [ ] Summarize semua knowledge dalam topik tersebut
  - [ ] Highlight pitfalls penting untuk master ingat

  ### List Flow

  - [ ] Baca `topic-diary/index.md`
  - [ ] Papar jadual Active Topics
  - [ ] Kalau index kosong → scan `topic-diary/topics/` untuk fail .md sedia ada

  ### Ambiguous Routing

  Bila master cakap `save` sahaja tanpa destination jelas → tanya:
  > "Save ke mana? (1) Session memory, (2) Daily diary, atau (3) Topic diary?"

  ## Mandatory Rules

  1. **Append only** — jangan overwrite entri sedia ada dalam topic file
  2. **Index wajib update** setiap kali topik dicipta atau diubah
  3. **Kebab-case** untuk semua nama fail dalam `topic-diary/topics/`
  4. **Tiada duplicate** — semak index dulu sebelum cipta topik baru
  5. **Sensitive content** — redact atau tanya master sebelum simpan

  ## Edge Cases

  | Situasi | Tindakan |
  |---------|----------|
  | Topik baru, tiada fail | Cipta fail + header + entry pertama |
  | Topik sedia ada | Append entry di bawah yang lama |
  | Nama topik tidak jelas | Tanya master: "Topik apa?" |
  | `list topics` tapi index.md kosong | Scan `topic-diary/topics/` untuk fail .md |
  | Content mengandungi credentials | Redact atau tanya master dulu |

  ## Level History
  - **Lv.1** — Base: Save/Review/List protocol, append-only, index management, ambiguous routing. Adapted from Kiyoraka/Project-AI-MemoryCore PR#8, 2026-05-31.
  ````

- [ ] **Step 2: Verify kandungan SKILL.md**

  Baca semula fail dan pastikan:
  - Frontmatter ada (name + description)
  - Context Guard ada semua 8 baris
  - Save Flow ada semua 6 steps
  - Review Flow + List Flow ada
  - Mandatory Rules ada 5 rules
  - Edge Cases ada 5 baris
  - Level History ada

- [ ] **Step 3: Commit**

  ```
  git add memory/plugins/lucy-skills/skills/save-topic/SKILL.md
  git commit -m "feat(lucy): add save-topic skill — Topic Diary System"
  ```

---

### Task 3: Daftarkan skill dalam identity-core.md

**Files:**
- Modify: `memory/main/identity-core.md` (baris 68–69, selepas `echo-recall`)

- [ ] **Step 1: Tambah save-topic ke senarai skills**

  Dalam `identity-core.md`, cari baris ini:
  ```
  - `echo-recall` — cari diary sebelum jawab soalan tentang masa lepas
  - `mulahazah` — rules yang dipelajari dari pattern kerja master
  ```

  Tambah baris baru antara `echo-recall` dan `mulahazah`:
  ```
  - `echo-recall` — cari diary sebelum jawab soalan tentang masa lepas
  - `save-topic` — simpan knowledge/insight ke topic diary berdasarkan subjek, review dan list topik
  - `mulahazah` — rules yang dipelajari dari pattern kerja master
  ```

- [ ] **Step 2: Verify perubahan**

  Pastikan `save-topic` ada dalam senarai skills tambahan, antara `echo-recall` dan `mulahazah`.

- [ ] **Step 3: Commit**

  ```
  git add memory/main/identity-core.md
  git commit -m "feat(lucy): register save-topic in identity-core skills list"
  ```

---

### Task 4: Update auto-memory MEMORY.md + upstream adaptation plan

**Files:**
- Modify: `C:\Users\user\.claude\projects\C--Users-user\memory\MEMORY.md`
- Modify: `memory/docs/plans/2026-05-03-upstream-adaptation.md`

- [ ] **Step 1: Update Lucy Skills entry dalam MEMORY.md**

  Cari baris ini dalam `MEMORY.md`:
  ```
  - [Lucy Skills v2.0](project_lucy_skills.md) — 26 skills aktif, 4 fasa adaptasi upstream selesai 2026-05-03, hooks dipasang (UserPromptSubmit + SessionStart)
  ```

  Tukar kepada:
  ```
  - [Lucy Skills v2.0](project_lucy_skills.md) — 27 skills aktif, adaptasi upstream Kiyoraka PR#8 selesai 2026-05-31 (save-topic), hooks dipasang
  ```

- [ ] **Step 2: Update upstream adaptation plan**

  Dalam `memory/docs/plans/2026-05-03-upstream-adaptation.md`, tambah row baru dalam jadual Log Kemajuan:
  ```
  | 2026-05-31 | Upstream PR#8 | Topic Diary System → save-topic skill | ✅ SELESAI |
  ```

- [ ] **Step 3: Update project_lucy_skills.md dalam auto-memory**

  Baca `C:\Users\user\.claude\projects\C--Users-user\memory\project_lucy_skills.md` dan update:
  - Tukar "26 skills" → "27 skills"
  - Tambah `save-topic` dalam senarai skills

- [ ] **Step 4: Verify semua perubahan**

  - MEMORY.md: bilangan skills betul (27)
  - upstream plan: row baru ada dalam Log Kemajuan
  - project_lucy_skills.md: save-topic ada dalam senarai

- [ ] **Step 5: Commit**

  ```
  git add memory/docs/plans/2026-05-03-upstream-adaptation.md
  git commit -m "docs(lucy): mark Topic Diary System as adapted, update skill count to 27"
  ```

  *(MEMORY.md dalam `.claude/` tidak perlu di-git add — ia auto-managed)*

---

### Task 5: Functional Verification

Tiada automated tests untuk sistem memory. Verification adalah manual.

- [ ] **Step 1: Semak semua fail wujud**

  Pastikan fail-fail berikut wujud:
  - `memory/topic-diary/index.md` ✓
  - `memory/plugins/lucy-skills/skills/save-topic/SKILL.md` ✓

- [ ] **Step 2: Semak skill terdaftar**

  Buka `memory/main/identity-core.md` dan sahkan `save-topic` ada dalam senarai skills.

- [ ] **Step 3: Simulate save topic**

  Cuba trigger: *"save topic cloudflare — cara set wrangler secret guna bash printf"*
  
  Verify Lucy:
  1. Papar "Buku ilmu dibuka."
  2. Normalize: `cloudflare` → `topic-diary/topics/cloudflare.md`
  3. Cipta fail baru (kalau belum ada)
  4. Compose entry dengan format yang betul (Context/Discovery/Details/Pitfalls/Keywords)
  5. Append ke fail
  6. Update `topic-diary/index.md`

- [ ] **Step 4: Simulate list topics**

  Cuba trigger: *"list topics"* atau *"senarai topik"*
  
  Verify Lucy papar jadual dari `index.md`.
