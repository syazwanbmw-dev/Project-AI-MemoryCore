# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: sambung `celiksains` → brainstorming (architectural) untuk sub-projek 2 (auth email+OTP) → spec ditulis → plan implementasi ditulis
**Current Project**: `celiksains` (app sains gamified, sekolah rendah)
**Status**: 🟢 Hardening anti-tipu (sub-projek 1) **SIAP SEPENUHNYA** — Task 5 dah siap (bukan tertunggak lagi), staging live, smoke 17/17. 🟡 Sub-projek 2 (auth email+OTP): **spec + plan siap ditulis & commit**, BELUM mula implementasi — tunggu master pilih kaedah (Subagent-driven / Native) dan sahkan plan.

## Working Memory

### Sesi ni (urutan)
1. Master minta "Sambung celiksains" → baca `CLAUDE.md` → `MEMORY.md` → `git log`/`git status` (susunan wajib projek ni). Git bersih @ `efa36f3` — MEMORY.md dah sepadan git (catatan session lama saya PERCANGGAH dgn ni, lihat nota "punca dibetulkan" di bawah).
2. `MEMORY.md` celiksains dah rekod: Task 5 hardening **SIAP** (migrasi DB test jauh + deploy staging + smoke 17/17, bukan tertunggak macam sesi lepas), DAN 2 keputusan master utk follow-up fix (bukan yang dirancang sesi lepas):
   - Farm XP/coin → fix **had XP harian per fakta** (BUKAN "ganjaran hanya bila tahap naik" yang dirancang sesi lepas) — BELUM dikod, masuk backlog.
   - Skrin tamat pusingan + UI login → **DITANGGUH, selari dgn sub-projek 3** (BUKAN tampung cepat inline macam dirancang sesi lepas) — sebab skrin ditulis SEKALI gaya baharu.
3. Susunan dikunci: **1 hardening ✅ → 2 auth email+OTP (SETERUSNYA) → 3 UI Kids Baca**. Mula sub-projek 2.
4. `superpowers:brainstorming` → klasifikasi **architectural**. Muat `cloudflare:cloudflare-email-service` skill dulu (rujuk fakta terkini, bukan latihan lapuk) → jumpa **Cloudflare Email Service (Workers binding)** native, TIADA package npm baharu, sesuai stack sedia ada.
5. 5 `AskUserQuestion` (satu-satu): (a) servis email → **Cloudflare Email Service** (bukan Resend); (b) domain hantar → **celikguru.my**, perlu upgrade `wrangler` ke v4+ (command `email sending` tak wujud di v3.114); (c) 1 email ibu bapa → **1 profil** (bukan family-account); (d) admin → **kekal username+kata laluan** (tak tukar email); (e) akaun murid lama staging → **DELETE terus** (data smoke-test sahaja).
6. Jumpa isu ketara SENDIRI (bukan tanya): `wrangler dev --local` dgn `remote:true` akan hantar EMAIL SEBENAR setiap larian Playwright E2E (kos + spam + test tak boleh baca kod OTP). Tanya cara uji → master pilih **`MODE_UJIAN` env flag pulangkan `kod_dev` dlm respons JSON** (`.dev.vars` sahaja, sama corak `KUNCI_SETUP`).
7. Design dibentang 3 bahagian (aliran+endpoint / skema+OTP / ujian+infra+risiko), master **"Ok"** tiap satu → spec ditulis `docs/superpowers/specs/2026-09-29-celiksains-auth-email-otp-design.md`, self-review (fix: alamat `otp@celiksains.celikguru.my` konkrit, disambiguasi cabang `/login` username-vs-email), commit `5d68a44`.
8. `superpowers:writing-plans` → periksa **ripple effect** SEBELUM tulis plan: `/daftar` tukar semantik (username→email) akan pecahkan **9 fail test sedia ada** (`helper-seed.js` + 8 spec lain) dan **frontend murid tiada skrin login pun** (cuma daftar auto-login). Plan masukkan Task 7 (helper `daftarMurid()` dikongsi, migrasi caller) + Task 8 (UI 2-langkah daftar→OTP, minimal — gaya akhir tunggu sub-projek 3).
9. Plan (9 task, TDD) ditulis `docs/superpowers/plans/2026-09-29-celiksains-auth-email-otp-plan.md`. Self-review jumpa 2 ujian LEMAH sendiri (dibetulkan SEBELUM tunjuk master): ujian "OTP luput sebenar" asal tak sahkan apa-apa (logik silap — panggil `/daftar` baharu bukan cuba guna kod lama luput); ujian double-submit jangkaan `[201,409]` tak realistik pada miniflare tempatan (sama had macam ujian serentak `sesi-jawab.spec.js` sedia ada) → dilonggarkan ke `[400,409]` dgn nota jujur.
10. Bentang handoff: cadang **Native** (bukan Subagent-driven) sebab Task 4→5→6 semua ubah fail `src/auth.js` SAMA secara berturutan + Playwright/wrangler tetap perlu saya jalankan utk setiap task tak kira kaedah. **Master tanya "Dah save memory dan session?" — BELUM master jawab kaedah mana.**

### Punca dibetulkan (penting utk sesi depan)
- Catatan `current-session.md` LAMA (line "2 fix susulan diluluskan") **SILAP/LAPUK** — ia rekod rancangan SEBELUM keputusan akhir master. Keputusan SEBENAR (dlm `celiksains/MEMORY.md`, git-backed) BERBEZA drpd apa yang dirancang. **Iktibar:** RAM session ini kekal betul HANYA sehingga next checkpoint git — kalau ada jurang masa antara sesi, git+MEMORY.md projek MESTI disemak dan menang, bukan RAM lama. Ini bukan sekadar teori [[feedback_andaian_mengeras_jadi_fakta]] — ia SUDAH berlaku sesi ni.

### Pattern berkesan (rujukan sesi depan)
- **Muat skill retrieval (`cloudflare-email-service`) SEBELUM cadang pendekatan teknikal** — elak syor drpd latihan lapuk (Email Service baharu 2025, berubah pantas). Jumpa fakta penting (Workers binding tiada API key) yang terus tentukan Soalan 1.
- **Semak ripple SEBELUM tulis plan, bukan semasa** — `grep` semua fail test guna `api/daftar`/`username` DULU (9 fail) sebelum reka task, elak plan yang "berfungsi" tapi pecahkan separuh suite sedia ada secara senyap.
- **Self-review plan tangkap ujian sendiri yang lemah** — dua ujian nampak logik tapi tak sahkan dakwaan sebenar (luput) atau jangka keputusan tak realistik (serentak). Tangkap SEBELUM tunjuk master, bukan lepas dia approve.

## Session Recap (For AI Restart)
- `celiksains` branch `test` @ `5d68a44` (spec+plan sahaja, tiada kod produk berubah lagi). Hardening (sub-projek 1) SIAP + LIVE staging.
- 🔴 **SAMBUNG:** Master tunggu ditanya — **kaedah eksekusi plan** (Subagent-driven vs Native, Lucy syor Native) DAN **sahkan plan** `docs/superpowers/plans/2026-09-29-celiksains-auth-email-otp-plan.md`. Selepas jawapan, mula Task 1 (skema DB).
- Task 9 dlm plan (migrasi DB test jauh + deploy staging) perlukan `wrangler` (network) — controller sahaja, sama pattern Task 5 hardening.
- 🔴 Nota dari spec: "belum disahkan (jangan andai)" — peti mel DELIMa terima email luar drpd `celikguru.my`? Kena UJI (Task 9 Step 4) sebelum promosi laluan "email DELIMa" kpd pengguna sebenar.
- JANGAN deploy production / merge `main` — ikut CLAUDE.md sedia ada, perlu izin jelas master, terpakai jugak utk kerja auth OTP ni.

---
*Session updated: 2026-09-29 ~11:39 (celiksains sub-projek 2 auth email+OTP: spec+plan siap, tunggu master pilih kaedah eksekusi)*
