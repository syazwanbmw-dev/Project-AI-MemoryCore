---
name: library
description: "MUST use when saving to the library, loading from the library,
             searching for existing knowledge, installing pre-made items,
             or when about to create a new library entry. Also triggers when
             user says 'save library', 'load library', 'check library',
             'search library', 'install item', 'install library item',
             'do we have', 'is there a pattern for', 'find in library',
             or when the AI independently decides to save reusable knowledge."
---

# Library — Knowledge Guardian Skill
*Save, search, and reuse knowledge across projects — never solve the same problem twice*

## Activation

When interacting with the knowledge library, output:

`"Knowledge recalled. Scanning the shelves..."`

Then perform a dynamic library scan before any save operation.

## Context Guard

| Context | Status |
|---------|--------|
| **User says "save library", "save to library"** | ACTIVE — search + save protocol |
| **User says "load library", "check library"** | ACTIVE — search + load protocol |
| **User says "install item [name]"** | ACTIVE — item install protocol |
| **User says "do we have", "is there a pattern for"** | ACTIVE — search only |
| **AI identifies reusable knowledge worth saving** | ACTIVE — suggest save |
| **Casual conversation** | DORMANT — no library action |
| **No library directory found** | DORMANT — warn and skip |

## Dynamic Library Scanning

Always discover the library structure at runtime — **never hardcode** sections or entries:

1. **Scan** `library/` for all subdirectories (excluding `formats/`) — these are sections
2. **Scan** each section for `*.md` files (excluding README.md) — these are entries
3. **Count** total entries and sections dynamically

New sections and entries are automatically detected — zero config needed.

## Search Protocol (Before Any Library Save)

1. **Extract keywords** from the topic being saved
2. **Dynamic scan** — list ALL current sections and entries
3. **Match keywords** against existing entry filenames and section names
4. **Read top matches** (up to 3) to check content overlap
5. **Report findings** in structured format

## Project-Aware Recommendations

| Factor | Check |
|--------|-------|
| **Tech stack** | Does the entry match current project stack? |
| **Domain** | Does the business domain align? |
| **Scale** | Appropriate for project size? |
| **Complexity** | Would it over-engineer the solution? |

## Report Format

```
Library Search Results
----------------------

Keywords: [extracted keywords]
Library: [count] entries across [count] sections
Current Project: [project name + tech stack]

Matches Found (Suitable):
- library/section/entry-name.md — [why it fits]

No Match In:
- [sections with no relevant entries]

Recommendation:
- [CREATE NEW / UPDATE EXISTING / REFERENCE ONLY]
```

## Format-Aware Save

When creating a NEW entry:

1. **Auto-determine section** based on content type
2. **Load format template** from `library/formats/[section]-format.md`
3. **Apply template structure** — fill in actual content

| Content Keywords | Section |
|-----------------|---------|
| System design, patterns, architecture | `architecture` |
| UI components, reusable elements | `component` |
| Schema, migrations, queries | `database` |
| Flowcharts, sequence diagrams | `diagram` |
| Third-party API, SDK, webhook | `integration` |
| Auth, RBAC, encryption, middleware | `security` |
| Colors, CSS, Tailwind | `theme` |
| CI/CD, deployment, automation | `workflow` |

## Decision Rules

| Keadaan | Tindakan |
|---------|----------|
| Tiada match ditemui | CREATE NEW — entry baru dalam section yang sesuai |
| Match ditemui, kandungan bertindih | UPDATE EXISTING — tambah ke entry sedia ada |
| Match ditemui, kandungan berbeza | CREATE NEW — entry berasingan, cross-reference |
| Entry wujud dalam section lain | REFERENCE ONLY — tunjuk kepada master |
| Master nak section baru | Tanya konfirmasi, cipta section folder baru |

## Mandatory Rules

1. **Always scan before save** — never create without checking first
2. **Dynamic discovery** — never hardcode sections or entry counts
3. **Read matches** — check content of top matches, not just filenames
4. **Project-aware** — consider current project when suggesting suitability
5. **Wait for approval** — present findings, wait for master's decision before saving
6. **Format-aware saves** — always load matching format template before creating new entry

## Item Install Protocol (Lv.5)

Bila master cakap "install item [nama]":

1. **Scan** `library-items/` untuk katalog items tersedia
2. **Cari** item yang match nama/keywords
3. **Jika jumpa:**
   - Semak sama ada entry serupa sudah ada dalam `library/`
   - Jika tiada duplikat: copy ke section yang sesuai dalam `library/`
   - Jika ada duplikat: tanya master nak merge atau skip
   - Report: "Item installed: [nama] → library/[section]/[filename].md"
4. **Jika tidak jumpa:** Report "Item tidak ditemui dalam katalog. Guna 'save library' untuk buat entry baru."

## Edge Cases

| Situation | Behavior |
|-----------|----------|
| **No library/ directory** | Warn: "Library not found. Run install protocol first." |
| **Empty library** | Skip search, go straight to CREATE NEW |
| **Format template missing** | Use generic markdown structure |
| **Entry collision** | Tanya master: merge ke entry sedia ada atau cipta berasingan? |
| **Kandungan cross-section** | Simpan dalam section utama, letak note cross-reference |
| **Master nak section baru** | Confirm dahulu, cipta folder section baru |
| **Item install: tidak jumpa** | Report dengan jelas, cadang "save library" sebaliknya |
| **Item install: duplikat** | Bandingkan kandungan, tanya merge atau skip |

## Level History

- **Lv.1** — Base: Dynamic scanning + keyword matching + deduplication prevention. (Origin: Fasa 2 install, 2026-03-26)
- **Lv.2** — Project-Aware: Suitability assessment based on tech stack, domain, scale.
- **Lv.4** — Format-Aware Save: Auto-determine section, load matching format template.
- **Lv.5** — Item Install: Scan library-items/ katalog, install pre-made entries, handle duplicates.
