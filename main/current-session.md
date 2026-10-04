# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: `takwim-digital` — master terlepas digest auto minggu ini → bina butang "Hantar Sekarang" → test `@HEAD` → betulkan warna butang → deploy production
**Current Project**: `takwim-digital` (Apps Script + Calendar, akaun DELIMa)
**Status**: 🟢 **SIAP & LIVE `@26`** (2026-10-04 13:06). `master` == `origin/master` @ `9c84006`. Suite **124/0**. Tiada tugasan terbuka.

## Working Memory

### Sesi ni (Ahad 2026-10-04, 10:24–13:26)
1. Master: "terlepas digest ke telegram dan google chat… blast manual atau buat button". Baca `MEMORY.md` + kod `sendWeeklyDigest_`. Bentang dua cara: (1) fungsi sementara `AUTH_SEMENTARA_JANGAN_COMMIT` dlm editor, (2) butang di System Settings.
2. **Master betulkan hipotesis saya:** digest TIDAK rosak — dia lupa tanda "Kongsi" pada aktiviti minggu ni, jadi trigger keluar senyap (`adaHantar` palsu), bukan penanda `DGSENT_` terbakar macam 13 Sept. Master pilih **Cara 2**.
3. Keputusan (master "ok" pada syor): butang tulis penanda selepas berjaya (trigger tak hantar dua kali) TAPI tidak bakar bila semua sasaran gagal; tiada pratonton (KISS); label tanpa perkataan "digest".
4. Dibina: `runDigest_(manual)` teras dikongsi trigger+butang (DRY); `sendDigestNow(token)` gerbang `canManageUsers`; UI butang + `confirm()` + mesej per status. Ujian 100→124, **7 mutasi digigit** (+2 utk warna/garis). Pulih guna `cp` kerana kod baharu belum commit.
5. Master tanya "test `@HEAD` hantar betul2 ke?" → YA (Script Properties dikongsi semua deployment). Bentang 3 pilihan; master teruskan uji.
6. **Butang berfungsi, warna salah — 2 pusingan:** (a) `class="btn"` sahaja = kelabu lalai (`.btn` TIADA warna; perlu `primary/secondary/danger`) — silap saya, tak baca CSS; (b) master pilih `secondary` tapi "takde pun garis biru" — `--line` (#e5ebf3) hampir tak nampak atas kad putih, DAN saya salah huraikan ("putih bergaris" tanpa sebut ia nyaris tak kelihatan). Fix: `border:2px solid var(--primary)` inline. Ujian lama TAK tangkap kerana semak `onclick`, bukan rupa.
7. Master "Deploy production" (eksplisit) → semak git/suite/`list-deployments` → `clasp create-deployment --deploymentId <ID guru>` → `@25`→`@26`, disahkan `list-deployments`.
8. Commit: `dad96cc` (feature), `23d0cde` (kelas warna), `6f9a606` (garis biru), `0b3dc2d`+`9c84006` (docs).

### Pattern berkesan (rujukan sesi depan)
- **Bila master tanya "memang macam tu ke?" tentang rupa — periksa CSS dulu, jangan agak.** Jawapan jujur: "silap saya, sebabnya X" lebih baik drpd pertahan.
- **Gejala yang sama boleh ada dua punca** (penanda terbakar vs tiada aktiviti bertanda). Tanya master fakta dunia-sebenar ("ada tanda Kongsi?") sebelum siasat kod.
- **Butang yang menghantar mesej luar mesti pulang keputusan per status**, bukan senyap — senyap nampak macam rosak.
- **Ujian mesti tuntut RUPA (kelas/warna), bukan sekadar `onclick`** — ujian sumber tak nampak skrin ([[feedback_ujian_buta_skrin]]).
- Regex ditulis melalui `node -e` dlm shell hilang backslash — guna `Edit` terus untuk regex.

## Session Recap (For AI Restart)
- `takwim-digital` SIAP: butang "Hantar Sekarang" LIVE `@26`. Belum smoke di URL production sebenar (master uji di `@HEAD` sahaja) — nota dlm `takwim-digital/MEMORY.md`. SETUP.md/PANDUAN-GURU.md belum sebut butang (ditawar, master belum jawab).
- 🔴 **Tertunggak projek LAIN (tidak disentuh sesi ni):**
  - `celiksains` sub-projek 2 (auth email+OTP): Task 1–8 SIAP @ `b9695c1`; **Task 9** (migrasi jauh + deploy staging + smoke) perlu master — `wrangler login` skop Email, izin `email sending enable`, 3 keputusan tertunggak (had kadar, enumerasi/PDPA, UI login). Email Service = Workers Paid $5 (free: verified-destination sahaja). Butiran: `celiksains/MEMORY.md`.
  - `mypwa-v2` pelan baiki kuota D1 (`GET /api/ujian` = 81% read): master baca pelan, luluskan T0 + jawab 3 soalan. Butiran: `mypwa-v2/MEMORY.md` blok 🆕.
  - `opr-program`: menunggu "ya" master utk deployment UKUR sementara; smoke `@29` tertunggak.
- JANGAN deploy production / merge `main` tanpa izin jelas master.

---
*Session updated: 2026-10-04 13:26 (takwim-digital butang Hantar Sekarang LIVE @26). Sebelum itu: 2026-09-29 ~22:05 (celiksains/mypwa-v2 tertunggak, dibawa ke atas)*
