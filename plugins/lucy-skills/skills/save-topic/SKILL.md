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
