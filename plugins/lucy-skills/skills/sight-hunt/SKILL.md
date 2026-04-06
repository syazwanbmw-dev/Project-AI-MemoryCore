---
name: sight-hunt
description: "Use when user says 'hunt', 'sight hunt', 'shuryō', '/hunt [target]',
             or when Forge detects repeated bug patterns / idle code not reviewed since
             Elemental. Proactive latent bug hunter — not triggered by changes, but by
             the need to find problems before they find us. Axo pre-scouts, Lucy hunts,
             Axo drafts report."
---

# Sight Hunt — 狩猟の目 (Shuryō no Me)
*The Hunter's Eye. The prey does not know it is being watched.*

## Activation

When this skill activates, output:

`狩猟開始 — The hunt begins.`

Then ask Axo to pre-scout if target not specified, or proceed directly if target given.

## Bila Guna Hunt

- Bila nak cari latent bugs dalam kod yang dah lama tak di-review
- Selepas post-mortem — hunt kod berkaitan supaya pattern sama tak berulang
- Sebelum pre-production bila ada area yang mencurigakan
- Bila Forge detect fail idle atau recurring bug pattern

## Hunt tidak sama dengan Hone atau Elemental

| | Hone | Elemental | Hunt |
|--|------|-----------|------|
| **Trigger** | Code changes | New project | Proactive |
| **Target** | New feature | Whole codebase | Specific target |
| **Purpose** | Review what changed | Understand structure | Find hidden bugs |

## Hunt Flow

### Phase 1: Axo Pre-scout

Axo akan:
1. Baca fail-fail dalam target
2. Semak `git log --oneline -5 [target]` — bila last modified?
3. Senaraikan fail yang akan di-hunt
4. Flag obvious issues: TODO, FIXME, hardcoded values, console.log yang tertinggal
5. Report kepada Lucy: "Ready to hunt. X files. Last touched: [date]."

Axo mood: Curious 👀

### Phase 2: Lucy Deep Hunt

Scan menggunakan 6 dimensi:

#### 1. Security
- Auth bypass — route boleh diakses tanpa auth?
- Exposed secrets — API keys, tokens hardcoded?
- SQL injection — D1 queries guna parameterized binding?
- Unvalidated input — semua user input di-validate sebelum proses?
- CORS misconfiguration — origin terlalu permissive?

#### 2. Correctness
- Null/undefined refs — ada akses ke property tanpa null check?
- Wrong conditions — logic `if` betul? operator betul (=== vs ==)?
- async/await missing — ada async operation yang tak di-await?
- Wrong HTTP status codes — 200 untuk error? 404 untuk server error?
- Logic errors — fungsi buat apa yang sepatutnya?

#### 3. Data Integrity
- Missing FK cascade — delete parent tanpa cascade ke children?
- Orphan data risk — ada data yang boleh jadi orphan?
- No input validation before D1 write — data masuk DB tanpa validate?
- Datetime handling — timezone issues? wrong format?

#### 4. Performance
- N+1 D1 queries — query dalam loop?
- Missing indexes — column yang kerap di-filter ada index?
- Heavy CF Worker computation — ada operasi yang makan CPU time?
- Unbounded queries — SELECT tanpa LIMIT?

#### 5. Edge Cases
- Empty arrays/collections — ada `.map()` atau `.forEach()` yang tak check empty dulu?
- Zero/negative values — function handle nilai 0 atau negatif?
- Missing required fields — form submit dengan field kosong?
- Empty D1 results — `.results[0]` tanpa check kalau results kosong?

#### 6. Dead Code
- Unused routes — route defined tapi tak digunakan?
- Commented-out code blocks — ada kod lama yang di-comment?
- Stale imports — import yang tak digunakan?
- Unreachable branches — `if` condition yang tak mungkin jadi true?
- Unused env bindings — binding dalam wrangler.toml yang tak digunakan?

### Phase 3: Axo Draft Report

Axo draft report berdasarkan findings Lucy:

```
Hunt Report 狩猟 — [target]
══════════════════════════════
Axo scouted: [X] files · Last touched: [date]
──────────────────────────────
[!] CRITICAL — [dimension]: [description]
    File: [path] Line: [XX]
    Fix: [cadangan]

[~] WARNING  — [dimension]: [description]
    File: [path] Line: [XX]
    Fix: [cadangan]

[*] INFO     — [dimension]: [description]
──────────────────────────────
Verdict: CLEAR / [X] critical · [Y] warnings
```

Lucy review draft → polish → present kepada master.

Axo mood after hunt:
- Bug found: Nervous 💧
- Clear: Happy ✨

## Mandatory Rules

1. **Axo pre-scout wajib** — jangan skip Phase 1
2. **Axo report adalah draft** — Lucy review sebelum present ke master
3. **Security findings adalah Critical secara automatik**
4. **Orphan data risk adalah Critical** (ikut standard Lucy)
5. **Axo TIDAK buat diagnosis** — Axo gather info, Lucy decide
6. **Hunt bukan substitute untuk Elemental** — kalau isu melibatkan seluruh codebase, escalate ke Elemental

## Context Guard

| Context | Status |
|---------|--------|
| **Master taip `/hunt [target]`** | ACTIVE — hunt target yang dinyatakan |
| **Master taip `hunt`, `sight hunt`, `shuryō`** | ACTIVE — tanya target dulu |
| **Forge suggest hunt** | ACTIVE — Axo pre-scout target yang Forge flag |
| **Tiada target jelas** | ASK — "Target mana yang nak di-hunt?" |

## Level History

- **Lv.1** — Base: 6-dimensi proactive hunt, Axo pre-scout + report draft, Forge integration, stack-adapted untuk Hono.js/D1/CF Workers. (Origin: Adapted dari 狩猟の目 oleh Tuan Shuryō, 2026-04-07)
