# Design Spec: save-topic Skill (Topic Diary System)
**Tarikh:** 2026-05-31
**Status:** APPROVED
**Upstream Source:** Kiyoraka/Project-AI-MemoryCore — Feature/Topic-Diary-System (PR #8, 2026-05-29)

---

## Objektif

Adapt upstream Topic Diary System ke dalam ekosistem Lucy sebagai skill baru `save-topic`.
Berbeza dari `save-diary` (chronological, session-based) — `save-topic` organize knowledge
by subject matter untuk recall merentasi semua sesi.

> Daily diary = "apa jadi hari ni?"
> Topic diary = "apa yang kita tahu tentang topik ini?"

---

## Bahagian 1: Struktur Fail

### Folder Baru

```
memory/
└── topic-diary/
    ├── index.md            ← peta semua topik (wajib update setiap kali)
    └── topics/
        ├── cloudflare-workers.md
        ├── hono-patterns.md
        ├── debugging-habits.md
        └── [kebab-case].md
```

### Lokasi Skill

```
memory/plugins/lucy-skills/skills/save-topic/SKILL.md
```

---

## Bahagian 2: Trigger Words

| Master cakap | Lucy buat |
|---|---|
| `save topic [nama]` | Simpan insight ke topik tersebut |
| `simpan topik [nama]` | (Melayu) Simpan insight ke topik |
| `topic [nama]` | Shorthand save topic |
| `remember under [topic]` | English variant |
| `review topic [nama]` | Baca & summarize topik |
| `list topics` | Papar semua topik dalam index |
| `senarai topik` | (Melayu) Papar index |

**Routing ambiguous:** Bila master cakap `save` sahaja tanpa destination yang jelas,
Lucy tanya: *"Save ke: session memory, daily diary, atau topic diary?"*

---

## Bahagian 3: Format Entry

```markdown
## [HH:MM, Hari, DD Bulan YYYY] — [Tajuk ringkas]

**Context:** Kenapa knowledge ni timbul / projek apa
**Discovery:** Apa yang dipelajari / ditemui
**Details:** Kod/command exact yang terbukti kerja
**Pitfalls:** Kesilapan biasa yang perlu elak
**Keywords:** [tag1, tag2, tag3]

---
```

---

## Bahagian 4: Format Index (index.md)

```markdown
# Topic Diary — Index

| Topik | Fail | Keywords | Last Updated |
|-------|------|----------|--------------|
| Cloudflare Workers | topics/cloudflare-workers.md | deploy, wrangler, D1 | 2026-05-31 |
```

---

## Bahagian 5: Protocol Skill

### Save Flow
1. Detect/tanya nama topik
2. Normalize nama → kebab-case (`Cloudflare Workers` → `cloudflare-workers`)
3. Check `topics/[kebab].md` — wujud atau baru?
4. Baru → cipta fail dengan header (nama topik, tarikh cipta, aliases)
5. Compose entry berdasarkan context sesi semasa
6. **APPEND** ke bawah — jangan overwrite
7. Update `index.md` (row baru atau update last_updated)

### Review Flow
1. Baca `topics/[kebab].md`
2. Summarize knowledge terkini tentang topik tersebut
3. Highlight pitfalls penting

### List Flow
1. Baca `index.md`
2. Papar jadual semua topik aktif

---

## Bahagian 6: Rules Wajib

1. **Append only** — tiada delete kecuali master minta explicit
2. **Index kena update** setiap kali topic diubah/dicipta
3. **Sensitive content** (password, token, secret) → tanya master dulu atau redact
4. **Kebab-case** untuk semua nama fail
5. **Tiada duplicate topik** — semak index dulu sebelum cipta fail baru

---

## Bahagian 7: Integrasi

| Skill | Hubungan |
|-------|----------|
| `save-diary` | Berasingan sepenuhnya — layer berbeza |
| `echo-recall` | Boleh extend kemudian untuk search topic-diary juga |
| `auto-commit` | Commit selepas save topic (standard) |
| `session-briefing` | Tiada perubahan |

### Perubahan pada Fail Sedia Ada
- `identity-core.md` → tambah `save-topic` dalam senarai skills aktif
- `MEMORY.md` (auto-memory) → tambah pointer kepada skill baru
- Semua skill lain → tiada perubahan

---

## Out of Scope

- Integrasi `echo-recall` dengan topic-diary (boleh buat sesi lain)
- Archive system untuk topic lama (boleh tambah bila perlu)
- Creative Systems dari upstream (Image-Prompt, Story, Song) — reference sahaja
