# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: `opr-insaniah` — master "opr insaniah" → "proceed 1 2 3 + skeleton loader macam opr-program" → deploy production → fix pepijat → save
**Current Project**: `opr-insaniah` (Apps Script terikat Sheet, DELIMa)
**Status**: 🟢 Guru **`@45` LIVE** (2026-10-05 11:41). `master` == `origin/master` @ commit docs terakhir. Suite **560/0**. Fix `4f79e15` di HEAD Apps Script, **BELUM `@46`**.

## Working Memory

### Sesi ni (Isnin 2026-10-05, 10:41–12:0x)
1. Baca fail Lucy + `CLAUDE.md` → `MEMORY.md` → `git log`. Brief: onload lambat (belum baiki), C1 belum, iPad belum disahkan.
2. Master: "proceed 1 2 3, masukkan skeleton loader". **Plan ditunjuk dulu** (A rangka → B1/B2 onload → C1), master "Ok". B2: serentak (cermin opr-program) bukan gabung fungsi server.
3. Siap, satu commit satu perkara: `2a49149` rangka muat · `849c278` B1 baca-sekali (RUJUKAN 2x→1x, USERS 2x→1x) · `6d9cedd` B2 pra-muat serentak · `1fe0089` C1 (19 fungsi `_`, pagar `dedah-global` ALLOW=13). Semua pagar dibuktikan GAGAL via mutasi (jumlah ~29).
4. Master: "Deploy production dulu pun ok kan. Tiada user lagi" → `clasp push` + `create-deployment --deploymentId` → **`@45`**, disahkan `list-versions`.
5. Master: "1234 ok" (rangka, onload, iPad menegak, fungsi lepas rename C1 — semua OK). Teruskan baiki `.sorok`.
6. Imbas KELAS `.sorok`-lawan-`display:`: jumpa `.tapis` DAN **`#jadualSenarai{display:block}` dlm @media mod kad** (telefon/iPad: carian tanpa padanan tunjuk kad LAMA — desktop betul). Fix `4f79e15` + pagar `sorok-kekhususan`. Disahkan visual Edge headless 500px sebelum/selepas.
7. Disimpan: `opr-insaniah/MEMORY.md`, auto-memory (`project_opr_insaniah`, 2 reference baharu, indeks dipadat 20.8→18.9 KB), `relationship-memory.md`.

### Pattern berkesan (rujukan sesi depan)
- **Imbas KELAS masalah, bukan satu kes** — screenshot rangka tunjuk `.tapis`; imbasan skrip jumpa `#jadualSenarai` yg lebih serius. Jadikan imbasan itu UJIAN (bukan senarai tetap).
- **Kod dialih ⇒ ujian lama yg baca badan fungsi patah** (`muatSenarai` → `bilaSenaraiSampul_`): halakan ujian, JANGAN longgarkan invarian.
- **Mutan setara/bukan-setara**: tulis mutasi yg benar-benar SALAH; pulih dgn `cp` (kerja belum commit).
- **Heredoc Bash makan `\\`** → skrip regex ke fail guna alat Write. `.gs`/`.html` CRLF → normalkan dulu. → [[reference_heredoc_backslash_bash]]
- **Fakta dunia-sebenar dari master** menggerakkan keputusan (belum launch ⇒ deploy dulu OK).

## Session Recap (For AI Restart)
- `opr-insaniah`: guru `@45`. **Tertunggak:** deploy `4f79e15` jadi `@46` bila master kata "deploy"; Langkah 3 (lazy-load pustaka PDF) tunggu ukuran telefon (opr-program: 82% HTML = pustaka PDF); Hirisan Edit/Padam sudah ada; launch rasmi = keputusan master.
- 🔴 **Tertunggak projek LAIN (tidak disentuh):**
  - `celiksains` sub-projek 2 (auth email+OTP): Task 1–8 SIAP @ `b9695c1`; **Task 9** perlu master (`wrangler login` skop Email, izin `email sending enable`, 3 keputusan: had kadar, enumerasi/PDPA, UI login). Email Service = Workers Paid $5.
  - `mypwa-v2` pelan baiki kuota D1 (`GET /api/ujian` = 81% read): master baca pelan, luluskan T0 + jawab 3 soalan.
  - `opr-program`: menunggu "ya" master utk deployment UKUR sementara; smoke `@29` tertunggak. (Pepijat `.sorok`/`display:` mungkin ada juga di projek lain yg guna toggle kelas — semak `sorok()` set kelas atau inline.)
  - `takwim-digital`: SETUP.md/PANDUAN-GURU.md belum sebut butang "Hantar Sekarang"; belum smoke di URL production.
- JANGAN deploy production / merge `main` tanpa izin jelas master.

---
*Session updated: 2026-10-05 (opr-insaniah rangka muat + onload + C1 LIVE @45; fix .sorok di HEAD). Sebelum itu: 2026-10-04 13:26 (takwim-digital butang Hantar Sekarang LIVE @26)*
