# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Active
**Last Activity**: 2026-04-21
**Session Focus**: erpm-v2 — finalize plan sebelum execute
**Context State**: Plan erpm-v2 dikemaskini dengan keputusan reka bentuk baru. Sedia untuk execute Fasa 1.

## 💭 Working Memory (RAM)

### Active Context
- **Current Topic**: erpm-v2 — finalize design decisions dalam plan
- **Immediate Goals**: Execute Fasa 1 bila master ready
- **Recent Progress**: Plan dikemaskini dengan 4 keputusan baru sesi ini

### Session Recap (For AI Restart)
- **Previous Session Summary**:
  - Laptop master rosak, memory folder di-sync + push ke GitHub (commit 0a9582c)
  - Plan erpm-v2 dikemaskini dengan keputusan reka bentuk:
    1. **Timestamps** — `updated_at` ditambah ke `permohonan` + `arkib` (semua 9 table lengkap)
    2. **Strategi data tahunan** — Pilihan 2: semua tahun kekal dalam `rekod`, filter by `tahun`. `arkib` optional sahaja
    3. **Logik kiraan purata TP** — Formula A (Sains & Matematik: purata per tajuk) + Formula B (subjek lain: average of averages). Disimpan dalam plan
    4. **Kurikulum** — Model hybrid: global (pengguna_id=NULL) + customize per guru. Scope awal: SR sahaja (Tahun 1–6), subjek teras dalam seed.sql

- **Where We Left Off**: Plan finalized. Master confirm tiada perkara lain sebelum execute.

- **Important Context**:
  - Plan: `C:\Users\user\Documents\code\memory\projects\erpm-v2-plan.md`
  - Folder `erpm-v2` belum dibuat — belum execute lagi
  - Source lama: `my-pwa/` (Supabase) — ada bug data tak papar di dashboard
  - my-pwa guna 7 tables: `data_guru`, `data_kelas`, `senarai_murid`, `data_subjek`, `data_kurikulum`, `rekod`, `tetapan`
  - **NEXT**: Execute Fasa 1 — setup folder, wrangler, D1, migration SQL

## 🔄 Session Lifecycle
*How this RAM-like memory works*

### Session Start
- **New Session**: RAM cleared, fresh start
- **AI Restart**: Load recap from previous session for continuity

### Session End
- **Important Learning**: Save key insights to permanent files
- **Temporary Context**: Keep brief recap for next restart
- **RAM Reset**: Clear detailed working memory for next session
