---
name: sight-omnipotent
description: "Use when user says 'omnipotent', 'sight omnipotent', 'full audit', 'security audit',
             or when preparing for production launch. Full codebase audit — security, scalability,
             maintainability. Opus-level. TOKEN INTENSIVE — use sparingly."
---

# Sight Omnipotent — Full Audit
*Audit penuh: security + scalability + maintainability — guna bila betul-betul perlu*

## Activation

When this skill activates, output:

`⚠ Omnipotent audit — token intensive. Initiating full audit...`

## PERINGATAN

**Skill ini guna Opus model — token cost tinggi.**
Guna HANYA untuk:
- Pre-production launch
- Security audit yang serius
- Major architecture review
- Bila Elemental jumpa isu yang memerlukan analisis lebih dalam

Jangan guna untuk task harian atau feature biasa.

## Bila Guna Omnipotent

- Sebelum launch projek ke production
- Bila ada security concern yang serius
- Bila projek dah besar dan nak validate architecture
- Bila Elemental tak cukup untuk isu yang kompleks

## Audit Protocol

### Step 1: Full Codebase Map
- [ ] Scan semua fail dalam projek
- [ ] Bina mental model seluruh sistem
- [ ] Kenal pasti semua integration points

### Step 2: Security Audit (Mendalam)
- [ ] OWASP Top 10 — semak satu persatu
- [ ] Authentication & Authorization flows
- [ ] Data exposure — apa yang boleh bocor ke client?
- [ ] Rate limiting ada?
- [ ] Input sanitization di semua entry points
- [ ] Cloudflare Workers security headers
- [ ] D1 query injection vectors
- [ ] Secrets management — .env, wrangler secrets

### Step 3: Scalability Audit
- [ ] Worker CPU time — ada operasi yang terlalu berat?
- [ ] D1 query efficiency — N+1 problems, missing indexes?
- [ ] Response caching strategy — ada ke?
- [ ] KV usage untuk data yang kerap diakses?
- [ ] Payload sizes — response terlalu besar?

### Step 4: Maintainability Audit
- [ ] Code complexity — fungsi terlalu panjang/complex?
- [ ] Naming consistency seluruh projek
- [ ] Error messages helpful untuk debugging?
- [ ] No magic numbers/strings tanpa konstant
- [ ] Dead code, unused imports, commented blocks

### Step 5: Report
```
OMNIPOTENT AUDIT — [nama projek] — [tarikh]
============================================
SECURITY
  Critical: [senarai]
  High:     [senarai]
  Medium:   [senarai]

SCALABILITY
  Bottlenecks: [senarai]
  Risks:       [senarai]

MAINTAINABILITY
  Issues:      [senarai]
  Suggestions: [senarai]
============================================
OVERALL VERDICT: PRODUCTION READY / NOT READY
Action Required Before Launch: [senarai]
============================================
```

## Mandatory Rules

1. Ini bukan scan harian — guna dengan bijak
2. Semua Critical security issues MESTI difix sebelum production
3. Dokumen findings dalam project memory fail
4. Lepas audit, cadangkan plan untuk address semua Critical items

## Level History

- **Lv.1** — Base: Full audit 3 dimensi (security/scalability/maintainability), OWASP aware, D1/CF Workers specific. (Origin: Kata Pipeline install, 2026-03-31)
