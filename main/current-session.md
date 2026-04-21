# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Active
**Last Activity**: 2026-04-13
**Session Focus**: erpm-v2 — plan SaaS baru (rekod prestasi murid)
**Context State**: sistem-olahraga-sekolah production up-to-date ✅. Sesi ni fokus plan erpm-v2.

## 💭 Working Memory (RAM)

### Active Context
- **Current Topic**: erpm-v2 — SaaS rekod prestasi murid
- **Immediate Goals**: Master baca plan, buat perubahan dalam MD kalau perlu, execute bila ready
- **Recent Progress**: Plan penuh siap dan disimpan

### Session Recap (For AI Restart)
- **Previous Session Summary**:
  - Brainstorm + design plan untuk projek baru `erpm-v2`
  - Migrate sistem lama `my-pwa` (Supabase, tidak selamat) ke Cloudflare stack
  - Keputusan utama: tenant per guru (bukan per sekolah), 2 roles (MASTER + GURU)
  - Upload murid via XLSX (SheetJS), kurikulum hybrid (global + customize)
  - 9 tables, standard columns: pengguna_id + timestamps + created_by

- **Where We Left Off**: Plan siap. Master nak baca dan fikir dulu sebelum execute.

- **Important Context**:
  - Plan: `C:\Users\user\Documents\code\memory\projects\erpm-v2-plan.md`
  - Folder `erpm-v2` belum dibuat
  - Source lama: `my-pwa/` (Supabase) + `erpm-cf/` (percubaan migrate lama)
  - **NEXT**: Master review → update MD kalau ada perubahan → execute Fasa 1

## 🔄 Session Lifecycle
*How this RAM-like memory works*

### Session Start
- **New Session**: RAM cleared, fresh start
- **AI Restart**: Load recap from previous session for continuity

### Session End
- **Important Learning**: Save key insights to permanent files
- **Temporary Context**: Keep brief recap for next restart
- **RAM Reset**: Clear detailed working memory for next session
