# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-23 19:5x–21:3x — 🟢 **Projek BAHARU `opr-program`: brainstorming → spec pusingan 2 (Opus review) → pelan Fasa 1 siap.**

### Sesi ni (petang → malam, `opr-program`)

Master mula dengan soalan spike: *"aku ada project opr sekolah yang dibuat guna appsheet, boleh ke
aku nak revamp/migrate ke appscript"*. Lucy fetch struktur Sheet AppSheet sebenar (WebFetch selepas
master tukar sharing ke "Anyone with link"), dapat gambaran: 1 jadual `LAPORAN` (100 baris/18
lajur) + 3 jadual rujukan (`DATA GURU` 42, `DATA KATEGORI` 6, `DATA PENGANJUR` 20). Master kongsi
screenshot PDF sebenar "One Page Report" SK Salor.

**Brainstorming architectural (superpowers)**: soal-jawab pelbagai pusingan (skop 1 sekolah, 2
peranan Guru/Admin, gambar wajib 1-4, Buku Program = upload PDF simpanan sahaja bukan jana, QR ke
Buku Program, header/branding admin boleh tukar semua, carian+tapis+susun senarai) → pendekatan
dipilih: projek BAHARU `opr-program`, salin fail INFRASTRUKTUR terbukti dari `opr-insaniah`
(DriveService/Setup/Utils/lib PDF), tulis fail bentuk-data baharu.

🔑 **Lucy sendiri tersilap, sedar, dan betulkan sendiri**: spec pusingan 1 ditulis TERUS oleh sesi
Sonnet — melanggar gerbang `kata` SKILL.md Lv.6 (*"fasa /plan WAJIB dispatch subagent model: opus"*).
Lucy perasan, tanya master, dispatch Opus review susulan → jumpa **8 isu KRITIKAL + 10 sederhana**,
kebanyakan KESENYAPAN: tiada storan ROLE/STATUS (panel admin mustahil dibina), tiada Setup/admin
pertama (42 guru import = sifar admin = terkunci hari 1), percanggahan skop Sheet (skrip terikat
Sheet BAHARU vs Sheet AppSheet lama utk migrasi), gambar 4 slot tak diresize (opr-insaniah dulu
1 gambar mentah = 9MB/19saat), papar-semula PDF tak disebut (opr-insaniah dulu gagal iframe 2
peranti), akibat "PDF client-side" tak dijejak ke migrasi (100 PDF lama tak boleh jana semula).
3 keputusan master baharu selepas review: QR untuk SESIAPA (bukan dalam-domain), PDF lama KEKAL
asal bila header ditukar, Jawatan snapshot sejarah. Spec pusingan 2 ditulis + commit (`6571188`).

Pembetulan kecil susulan: KAUNTER dilipat masuk `TETAPAN` (bukan sheet berasingan) supaya padan
pola terbukti `naikkanKaunter_()` — commit `6d5ef4f`.

**Pelan Fasa 1** (Setup + Cipta Laporan, `writing-plans`) ditulis + commit (`dea97c3`) — 12 task,
kod diadaptasi LANGSUNG daripada fail SEBENAR `opr-insaniah` (Lucy baca kod sumber dulu, bukan
reka), TDD penuh untuk fungsi tulen. **Belum dilaksana** — tunggu master pilih Subagent-Driven vs
Inline Execution (soalan masih terbuka bila sesi ni berhenti).

`opr-program` — git local sahaja (`C:\Users\user\Documents\code\opr-program`), tiada remote lagi.
Butiran penuh: `opr-program/docs/superpowers/specs/2026-08-23-opr-program-design.md` +
`project_opr_program.md` (memory Claude Code).

### ⏭️ Bila sambung

Tunggu jawapan master: Subagent-Driven atau Inline Execution untuk laksana Fasa 1. Lepas Fasa 1
disahkan (Task 11 smoke test manual), fasa seterusnya ikut urutan: senarai/carian → edit → padam →
panel admin+tetapan → migrasi 100 rekod (TERAKHIR, bukan awal).
