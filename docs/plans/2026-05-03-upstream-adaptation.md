# Plan: Adaptasi Upstream Features → Lucy System
**Tarikh:** 2026-05-03
**Status:** IN PROGRESS

---

## Objektif
Adapt semua 24 feature dari upstream Kiyoraka/Project-AI-MemoryCore ke dalam sistem Lucy.

---

## Fasa 1 — Infrastruktur (Foundation)
> Kena siap dulu — Fasa 3 bergantung pada ni.

- [ ] `User-Prompt-Hook` — master hook framework (enables inject systems)
- [ ] `Auto-Load-Hook` — auto-restore Lucy memory bila session start

---

## Fasa 2 — Skill Baru untuk Lucy
> Feature genuinely baru, tambah ke plugins/lucy-skills/skills/

- [ ] `Decision-Log` — log keputusan arkitek/teknikal
- [ ] `Reminders` — persistent reminder cross-session
- [ ] `Time-based Aware` — Lucy adapt behaviour ikut masa (pagi/petang/malam)
- [ ] `Mulahazah` — behavioral learning dari pattern master
- [ ] `Echo Memory Recall` — recall dan narrate memory lama

---

## Fasa 3 — Inject Systems
> Requires Fasa 1 selesai dulu.

- [ ] `Time-Prompt-Inject` — auto-inject timestamp setiap prompt
- [ ] `Tone-Prompt-Inject` — tukar tone Lucy
- [ ] `Mood-Prompt-Inject` — tukar mood/suasana Lucy

---

## Fasa 4 — Overlap Skills (Check & Enhance)
> Banding upstream vs Lucy. Update jika ada improvement, skip jika sama.

| Skill | Lucy Versi | Upstream Ada Update? | Action |
|-------|-----------|----------------------|--------|
| session-briefing | ✅ | UPDATE | Lv.3 progression, Companion Skills section, reminders + project health |
| forge | ✅ | UPDATE | Skill File Template YAML, Synergy section, cleaner mandatory rules |
| library | ✅ | UPDATE | Lv.5 Item Install catalog, Decision Rules table, 5 edge cases baru |
| post-mortem | ✅ | SKIP | Lucy version lebih lengkap — domain reference dah ada |
| save-diary | ✅ | UPDATE | Platform timestamp cmds, daily-diary-protocol ref, 2 edge cases |
| work-plan | ✅ | UPDATE | Lv.2 Wave Execution parallel, Command Dispatch table, Recovery Context |
| auto-commit | ✅ | UPDATE | Format configurable, Minimal/WIP format, 4 edge cases, multi-commit |
| Observation (sight-*) | ✅ | SKIP | sight-* family lebih detail dari upstream Observation System |
| LRU Project Mgmt | partial | UPDATE | Fasa 2 — jadikan skill baru |
| Memory Consolidation | partial | SKIP | Struktur Lucy memory dah cukup |

---

## Creative Systems (Reference Only)
> Ada dalam repo sebagai reference. Install bila master mahu.

- `Image-Prompt-System`
- `Interactive-Story-System`
- `Song-Creation-System`

---

## Log Kemajuan

| Tarikh | Fasa | Task | Status |
|--------|------|------|--------|
| 2026-05-03 | Setup | Sync upstream Feature/ folders | ✅ SELESAI |
| 2026-05-03 | Fasa 1 | User-Prompt-Hook + Auto-Load-Hook | ✅ SELESAI |
| 2026-05-03 | Fasa 2 | 4 skills baru + Time-Aware ke identity-core | ✅ SELESAI |
| 2026-05-03 | Fasa 3 | Time/Tone/Mood injectors | ✅ SELESAI |
| 2026-05-03 | Fasa 4 | 6 skills diupdate (1 skip: post-mortem) | ✅ SELESAI |
