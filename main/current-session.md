# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: Work
**Current Project**: `opr-program`
**Status**: 🟢 **Modal picker Tempat/Anjuran DEPLOYED ke guru — LIVE `@16`** (URL guru tak
berubah). Smoke `@13` PASS → master minta ubah rupa picker → reka bentuk modal (bounded) →
TDD → 1 bug smoke ([hidden] vs display:flex) dibetulkan pada deployment ujian → smoke ujian
PASS → deploy guru. `master`==`origin/master` @ `ee1481c` (kod `c09f470`). Suite 391/391.

**Aliran sesi (2026-09-06 pagi):**
1. Brief → master lapor smoke `@13` PASS + screenshot corak modal picker.
2. `brainstorming` — klasifikasi **Bounded** (server/storan/format `;`/praIsi tak berubah,
   cuma widget client). `AskUserQuestion` 3 soalan: butang+chip / multi checkbox / input gabung.
3. Reka bentuk in-chat → master lulus ("proceed. push dan kita test di /dev dulu").
4. `test-driven-development`: RED `tests/pilih-modal.test.js` → GREEN. Helper tulen
   `cocokCari_`/`bolehTambah_` di `Kongsi.html` (boleh `muat()`); fungsi DOM di `app.js.html`.
   3 ujian lama dikemas. Suite 365→390. `commit-seal` → `b3e707e` → push origin.
5. `clasp push` /dev. `/dev` degil untuk master (multi-akaun DELIMa) → buat **deployment ujian
   `/exec` berasingan** (URL sendiri, tak sentuh guru).
6. Master smoke deployment ujian → **bug: modal auto-buka, Done tak tutup, senarai kosong**.
   `systematic-debugging`: punca = atribut `hidden` kalah pada `.modal-tindih{display:flex}`
   (author CSS atasi UA `[hidden]{display:none}`). Fix `c09f470`:
   `.modal-tindih[hidden]{display:none}`. Suite 390→391. Rekod memory global
   `reference_hidden_attr_display`.
7. Re-deploy deployment ujian (`@15`) → master re-smoke → **PASS**.
8. Master: "deploy ke guru" → pre-check (sync, `.claspignore` 0 `.js`, appsscript.json tak
   berubah = tiada re-consent) → `clasp push` + `create-deployment --deploymentId
   AKfycbyd85qp…08Zi5LPY` → guru **`@16`** → sahkan `list-versions`+`list-deployments` sepadan
   → `clasp delete-deployment` deployment ujian sementara. Kemas semua memory.

## Working Memory

### Active Context — SAMBUNG SINI
**Modal picker Tempat/Anjuran LIVE guru `@16`.** Kalau master mula sesi baharu pasal
opr-program: tanya **"ada isu apa-apa dengan picker Tempat/Anjuran baru tu?"** Kalau tiada →
fasa ni TUTUP. Pilihan seterusnya: **Fasa 3b** (Panel Pengguna & Rujukan) atau **3c** (Migrasi
100 rekod) — kedua belum mula.

⚠️ Master smoke pada deployment UJIAN (kod byte-identical, versi 15 vs 16 beza deskripsi
sahaja) — bukan URL guru sebenar. Kalau guru lapor apa-apa, checklist 11 langkah dlm
`opr-program/MEMORY.md` (blok status teratas) masih terpakai.

🔑 **Reka bentuk modal:** `#kotakTempat`/`#kotakAnjuran` = sumber kebenaran tersembunyi
(`display:none`). Modal + chip cuma paparan. Jangan tukar `.kotak-nilai` balik ke `flex`
tanpa buang modal dulu.

### Ranjau baharu sesi ni
- **Atribut `hidden` + kelas yang set `display:`** → `hidden` tak berkesan (author CSS atasi
  UA). Fix `.kelas[hidden]{display:none}`. `node --test` bentuk-sumber TAK tangkap — cascade
  pelayar. → `reference_hidden_attr_display` (global memory).
- **`#inpCari` sudah wujud** (carian senarai laporan) — input modal dinamakan `#modalCari`.
  Semak ID sedia ada sebelum tambah.
- **`#modalPilih` diletak DI LUAR `<form>`** (aras body, selepas `</template>` dlm form.html)
  — elak Enter=submit senyap (ranjau `0ae7940`). Handler `#modalCari` tetap `preventDefault`.
- `/dev` GAS degil bila browser multi-akaun (authuser≠0). Jalan keluar: deployment `/exec`
  ujian berasingan, padam selepas smoke.

## Session Recap (For AI Restart)
- **Sesi lepas** (2026-09-06 pagi awal): fix visual `@12`→`@13`.
- **Sesi ni** (~09:2x–10:32): modal picker Tempat/Anjuran — brainstorm bounded + TDD + 1 bug
  CSS dibetulkan + smoke ujian PASS → **deploy guru `@16` LIVE**. Deployment ujian sementara
  dipadam. Suite 391/391.
- **Left off**: fasa Anjuran/Tempat picker SIAP + LIVE guru. Backlog projek lain tak disentuh.
  Fasa 3b/3c belum mula.
- **State master**: Pagi, momentum tinggi, langsung — smoke pantas, terus arah deploy bila PASS.

---
*Session updated: 2026-09-06 ~10:32 (modal picker Tempat/Anjuran DEPLOYED guru @16 LIVE,
deployment ujian dipadam, suite 391/391, master==origin/master @ ee1481c)*
