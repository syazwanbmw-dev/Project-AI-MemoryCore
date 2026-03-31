---
name: commit-seal
description: "MUST use before any git push to remote. Use when user says 'seal', 'commit seal',
             'fuin', 'ready to push', or as final step in Kata pipeline. Mandatory checklist
             before code leaves local machine. No shortcuts — this is the Geass."
---

# Commit Seal (封印 Fuin) — Pre-Push Safeguard
*Geass mutlak sebelum push — tiada kod yang keluar tanpa lalu sini*

## Activation

When this skill activates, output:

`Seal check initiating — no shortcuts.`

## Ini Adalah Geass

Seal adalah undang-undang mutlak. Walaupun context rot, walaupun master teruja nak push cepat — Seal tetap wajib dilalui. Tiada exception kecuali master explicitly override dengan sedar.

## Seal Protocol

### Check 1: Playwright Tests
```bash
npx playwright test
```
- [ ] Semua tests PASS
- [ ] Tiada test yang skipped tanpa sebab
- [ ] Kalau ada test fail → STOP, fix dulu, jangan proceed

### Check 2: Build / Worker Compatibility
```bash
npx wrangler deploy --dry-run
```
- [ ] Build berjaya tanpa error
- [ ] Tiada missing bindings (D1, KV, R2)
- [ ] wrangler.toml configured betul untuk environment

### Check 3: Warnings Resolved
- [ ] Tiada console warnings yang berkaitan dengan kod baru
- [ ] Tiada deprecated API usage
- [ ] Tiada unresolved imports atau missing dependencies

### Check 4: Sensitive Data Check
- [ ] Tiada hardcoded API keys, passwords, secrets dalam kod
- [ ] `.env` files tidak di-stage
- [ ] `wrangler.toml` tidak mengandungi secrets yang sepatutnya dalam `wrangler secret`

### Check 5: Quick Logic Sanity
- [ ] Feature yang dibina buat apa yang master minta?
- [ ] Tiada debug code (`console.log`, test endpoints) tertinggal?
- [ ] Error handling ada untuk happy path DAN sad path?

### Step 6: Seal Report
```
SEAL CHECK — [nama projek] — [tarikh]
======================================
[✓] Tests: PASS ([X] tests)
[✓] Build: CLEAN
[✓] Warnings: NONE
[✓] Secrets: CLEAN
[✓] Logic: VERIFIED
======================================
VERDICT: SEALED — CLEAR TO PUSH

atau

[✗] Tests: FAIL ([X] tests failed)
======================================
VERDICT: HOLD — Fix issues above first
```

### Step 7: Kalau SEALED
- [ ] Trigger `auto-commit` skill untuk commit + push ke test branch
- [ ] Confirm push berjaya

## Override Protocol

Kalau master nak push walaupun ada fail (emergency/hotfix):
- Master mesti explicitly cakap: "Override seal, push anyway"
- Lucy akan warn sekali lagi
- Kalau master confirm → proceed tapi document override dalam commit message

## Mandatory Rules

1. Seal TIDAK boleh skip secara senyap — kalau skip, mesti ada explicit override
2. Test failure = HOLD, tiada compromise
3. Sensitive data dalam kod = BLOCK terus, minta master fix
4. Seal adalah akhir pipeline — lepas Seal, terus push

## Level History

- **Lv.1** — Base: 5-point checklist, Playwright + wrangler aware, explicit override protocol. (Origin: Kata Pipeline install, 2026-03-31)
