# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-27 — ✅ **Projek `takwim-digital`: Auto-login + pendaftaran satu-langkah (buang OTP) — SELESAI, LIVE production `@16`.**

### Sesi ni (`takwim-digital`)

1. **Sambung spike petang semalam** (auto-detect email guru): cipta deployment sementara `@13`
   untuk test akaun guru BUKAN-pemilik → gagal baca log (Executions dashboard sekat log
   pengguna lain, `clasp logs` pun gagal tiada GCP project) → fix: probe tulis ke
   `PropertiesService` (bukan `Logger.log`) → `@14` → **DISAHKAN**: email guru sebenar
   (`g-76509323@moe-dl.edu.my`) keluar BETUL. Cara A **feasible**, DELIMa tak sekat scope email.
2. Cleanup spike: kod probe dibuang, deployment `@13`/`@14` dipadam, git balik clean.
3. **Reka bentuk sebenar**: master soal "buang OTP maksudnya bypass approval admin ke?" — Lucy
   jelaskan dgn petik kod sebenar (status check jalan SEBELUM OTP), master pilih skop
   **"Plan A+B"** (auto-fill DAN buang OTP terus untuk registration+login, bukan sekadar
   auto-fill).
4. **Kod**: `submitRegistration()` (satu langkah, email dari client diabaikan — server override
   guna `Session.getActiveUser()`, elak spoof identiti) ganti `requestRegistrationOtp`+
   `verifyRegistrationOtp`. `attemptAutoLogin()` (zero-klik, semakan status sama persis) ganti
   `requestLoginOtp`+`verifyLoginOtp`. Buang kod mati OTP (helper + config + tab UI + CSS).
   Net **-170 baris**.
5. `sight-hone`: 4 isu teks lama sebut "OTP" (email approval, wizard setup, label test-koneksi)
   — semua dibetulkan.
6. **Ujian hujung-ke-hujung** (3 deployment sementara berasingan, semua dipadam lepas guna):
   auto-login akaun approved sedia ada ✅ · pendaftaran akaun baharu (guru sebenar, borang
   satu-langkah, email readonly) ✅ · admin approve → auto-login zero-klik akaun baharu ✅.
7. Commit `5ed6e37` + push GitHub. **Deploy production `@12`→`@16`** (kelulusan eksplisit
   master "Ok deploy production"), disahkan `list-deployments`+`list-versions` dua kali.

**Gotcha baharu ditemui** (dicatat `reference_clasp_gotcha` #10-11): Executions dashboard sekat
log pengguna lain (fix: PropertiesService) · function Run/Debug dropdown Apps Script Editor
kadang tak papar fungsi baharu walau kod disahkan wujud (punca tak pasti, tiada fix confirmed).

### ⏭️ Backlog terbuka (tak urgent)

- **Button Delete User** dalam admin panel — tiada sekarang (cuma Suspend). Rekod guru test
  `g-76509323@moe-dl.edu.my` di-**suspend** (bukan delete, sebab one-off script `ONE_OFF_
  deleteTestUser_` gagal dijalankan via dropdown yang degil — kod dah dibuang balik dari
  `Code.js` selepas itu, tak sempat jalan).
- **Screen putih lepas Log Keluar** (`logoutUI()` → `location.reload()` tersekat sehingga
  refresh manual) — kod LAMA, tak disentuh sesi ni, belum disiasat puncanya.
- Script Property `PROBE_ACTIVE_USER_EMAIL` (sampah lama dari spike) — master belum sahkan
  padam manual via Project Settings ⚙️.

Butiran penuh: `project_takwim_digital.md`.
