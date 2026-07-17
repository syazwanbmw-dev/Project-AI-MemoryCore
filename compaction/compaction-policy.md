# Compaction Policy
*Budgets and retention tiers for memory compaction.*

> **Adaptasi Lucy** — dipasang 2026-07-17 dari upstream Kiyoraka (PR #9, aimanmalib).
> Berbeza dari default upstream: `main/current-session.md` ditambah dengan bajet **aksara**.
> Sebab dicatat dalam Maintenance Notes di bawah.

## Active Budgets

| File | Budget | Newest Tier (verbatim) | Notes |
|------|--------|------------------------|-------|
| main/current-session.md | 40000 chars | last 4 session records | log sesi — fail paling laju membesar |
| main/main-memory.md | 500 lines | last 20 entries | core identity + user profile |
| main/relationship-memory.md | 400 lines | last 15 entries | user preferences |
| topic-diary/topics/*.md | 300 lines each | last 10 entries | per-topic journals |

### Entry Boundaries — main/current-session.md

Fail ini tiada "entri" berbaris tunggal. Satu **session record** ialah:

- Bermula pada heading `### 🆕 Sesi YYYY-MM-DD ...` (atau `### Sesi YYYY-MM-DD ...`)
- Berakhir pada pemisah `--- (rekod sesi lepas) ---` atau heading sesi berikutnya
- Blok header fail (`# 🌟 Current Session Memory` + `## Session RAM Status`) **bukan** entri —
  ia sentiasa kekal di puncak, tidak pernah dicompact

## Retention Tiers

- **Newest tier** — kept verbatim, never compacted.
- **Mid-age tier** — light summarization (merge near-duplicate lines, trim filler).
- **Oldest tier** — heavy compaction into a single `## Compacted History` block.

## Compaction Triggers

- Before any `save` that targets a budgeted file.
- On the explicit `compact memory` command.
- Never automatically on files not listed in this policy.

## Exclusions

- Never compact files under `compaction/snapshots/`.
- Never compact secrets, tokens, passwords, or credentials.
- Never compact the newest tier.

### Detecting Secrets

Treat an entry as a secret (and exclude it from any `## Compacted History` block) if it matches
any of these:

- Key names ending in `_KEY`, `_TOKEN`, `_SECRET`, `_PASSWORD`, or `_CREDENTIAL`
- High-entropy values that look like API keys, tokens, or private keys
- Anything under a `## Secrets`, `## Credentials`, or `## Tokens` section header

When a secret is found in the oldest tier, leave it verbatim in place (do not summarize it) or
ask the user how to handle it. Never merge a secret value into a shared summary.

### Kredential ujian — konteks Lucy

Rekod sesi kita mengandungi kredential ujian (cth `admin_dba1097`/`1234`, `nba3003`/`1234`).
Ini akaun **test/demo DB sahaja**, bukan secret production — ia memang sudah wujud dalam repo
projek. Ia **tidak** menghalang compaction. Tetapi:

- Jangan sekali-kali salin nilai `JWT_SECRET`, token API, atau output `wrangler secret` ke dalam
  blok `## Compacted History`.
- Bila ragu → tanya master, jangan agak.

## Maintenance Notes

- Budgets are line-based by default; character-based budgets are allowed (e.g. `8000 chars`).
- Adjust budgets to fit your model's context window.
- Lower the budget for files you want kept very tight; raise it for reference-heavy files.

### Kenapa current-session.md guna `chars`, bukan `lines` (keputusan 2026-07-17)

Ketumpatan baris fail ini **~193 aksara/baris** — hampir 3× prosa biasa (~70) yang diandaikan
bajet default upstream. Bajet 500 baris = ~96,000 aksara untuk kita = masih melebihi had baca
satu-kali Lucy (~100,000 aksara / 25k token, dan itu belum kira baki context).

Kekangan sebenar ialah **token**, bukan baris. Baris hanyalah proksi — dan proksi yang bocor
sebaik ketumpatan berubah. Jadi fail ini diukur dalam unit yang sepadan dengan kekangan sebenar.

**40,000 aksara (~10k token)** dipilih master: muat penuh dalam satu bacaan dengan ruang lega
~60% untuk fail membesar antara compaction.

### Fail yang SENGAJA tiada bajet (bukan terlepas pandang)

| File | Sebab |
|------|-------|
| `daily-diary/*.md` | Dipetak ikut tarikh — setiap fail terbatas secara semula jadi (terbesar 85 baris). Tiada pertumbuhan tak terhad. |
| `main/decisions.md` | Tokok-tambah tapi sangat perlahan (8 baris). Semak semula bila lepas ~300 baris. |
| `main/post-mortems.md` | Sama — 5 baris. Semak semula bila lepas ~300 baris. |
| `main/axo.md`, `main/reminders.md` | Saiz tetap mengikut reka bentuk. |
| `main/*-format.md`, `main/identity-core.md` | Fail rujukan/template — bukan log. |

> Ingat: fail yang tiada dalam jadual **Active Budgets tidak pernah dicompact**. Senarai di atas
> ialah keputusan sedar, direkod supaya sesi akan datang tak tersilap sangka ia terlepas.

---

*The policy is the contract; compaction only does what the policy allows.*
