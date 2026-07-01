# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-07-01 (tengah hari, ~13:59)
**Session Focus**: ✅ mypwa-v2 — Feature "Trend Markah Merentasi Ujian + ETR" **SIAP & LIVE PRODUCTION** (main `f07d059`). Playwright PASS, merge main --no-ff. Pending: master verify production visual + GitHub Actions deploy hijau.

### 🆕 Sesi 2026-07-01 (pagi–tengah hari): mypwa-v2 — Trend Markah + ETR ✅ LIVE PRODUCTION (main `f07d059`)
**Sambung dari spec 30 Jun.** Execute penuh guna subagent-driven-development (4 task + review setiap satu + final opus review).
- **Plan:** `docs/superpowers/plans/2026-07-01-trend-markah-etr.md` commit `49b8152`. Kod lengkap dalam plan (endpoint + trend.html + test).
- **Task 1 (backend, haiku):** endpoint `GET /api/ujian-markah/trend` — `3f66dd7`. Review sonnet ✅ Approved 0C/0I. Guna `ui.tahun=k.tahun` affinity, TOV/ETR/status on-the-fly.
- **Task 2 (trend.html, sonnet):** `6e37994`. Review sonnet Approved + 1 Important (logoUrl tak esc dlm cetak) + 1 Minor (is_td order path cetak) → FIX `a3d2b7a`.
- **Task 3 (routing app.js, haiku):** `2cc99c0` — sidebar link "Trend Markah" (guru grup 'ujian' + pelawat) + `/trend` dlm PELAWAT_PAGES. Controller self-review (3-baris mekanikal).
- **Task 4 (test, haiku):** `43e3c55` — `tests/trend.spec.js` smoke resilient (skip guard, assert errors===[]). Self-review.
- **Final review (opus):** WITH FIXES, 0 Critical/0 security. 1 IMPORTANT: `ujian_list` tak ditapis ikut `kelas.tahun` → kolum kosong utk ujian tahun lain (projek namakan ujian per-tahun → AKAN muncul). → FIX `c1cf928` (EXISTS filter `ui.tahun=kelas.tahun` + susun murid `localeCompare`).
- **Seal:** node --check ALL OK, wrangler dry-run CLEAN (192KiB), secrets clean.
- **Playwright:** ✅ 1 passed 9.2s lawan staging (sandbox-disabled izin master, TEST_USER=test/test123) — tak skip, 0 ralat JS, jadual trend terbentuk.
- **Deploy:** push test (`4b091c0..c1cf928`) → merge main `--no-ff` `f07d059` (6 fail, 1129+) → push main auto-deploy production. **0 migration.** Lucy balik branch test.
- **PENDING master:** verify production `erpm-sksalor.celikguru.my/trend` (visual: kolum betul ikut tahun, cetak landscape, PELAWAT reachability) + confirm GitHub Actions deploy hijau.
- **Backlog (review, belum buat):** normalisasi markah_penuh berbeza; ETR boleh-laras; carta garis per murid; eksport Excel.
- **Pengajaran:** report file subagent (haiku) kerap TAK di-overwrite (kekal basi dari feature lama) — controller WAJIB nilai diff sebenar via git, bukan percaya report. Betul semua kali ni (commit subjek + diff disahkan sendiri).

### 🆕 Sesi 2026-06-30 (tengah hari ~13:21–14:08): mypwa-v2 — Trend Markah + ETR ⏳ SPEC SIAP, BELUM EXECUTE
**Idea master:** feature trend markah merentasi beberapa ujian, **per subjek per individu murid**, + masukkan **ETR** (sasaran).
- **Keputusan brainstorm (master pilih):**
  1. **ETR dikira AUTO** (bukan input manual) = **TOV + 15, maks 100**. Tiada jadual DB baru, tiada migration — kira on-the-fly.
  2. **TOV = ujian PERTAMA** (kaedah headcount KPM standard, konsisten sepanjang tahun).
  3. **Urutan ujian ikut `created_at`** (KISS, tiada migration). Trend = semua ujian dalam sesi sama yang kelas terlibat.
  4. **Surface = page BARU** `public/trend.html`, **carian per KELAS** (bukan per murid satu-satu). Pengguna = **GURU + PELAWAT** (+ admin).
  5. **Susun atur = grouped per MURID** (collapse macam tab PAJSK `murid-group-header`): baris = SUBJEK, kolum = TOV–ETR + setiap ujian (markah+gred) + Status (✅ Capai / ❌ -jurang). Master tunjuk screenshot PAJSK + lakaran sebagai rujukan.
  6. **GURU nampak SEMUA subjek** (gaya kad laporan penuh, bukan tapis jadual_guru). ⚠️ Nota security: longgarkan akses GURU melebihi jadual_guru — master terima (markah bukan sulit antara staf, pelawat/dashboard pun dedah). Dokumen dalam spec.
- **Reka bentuk:** 1 endpoint baru `GET /api/ujian-markah/trend?nama_sesi=&kelas_id=` (authMiddleware, baca-sahaja). Guna semula pattern join `ui.tahun = k.tahun` + `appKiraGred()`. Frontend `trend.html` + sidebar link (guru+pelawat) + `/trend` dlm PELAWAT_PAGES. Cetak termasuk v1. Test `tests/trend.spec.js`. **0 migration, 0 package.**
- **Andaian (limitasi):** markah dianggap skala /100. Normalisasi markah_penuh berbeza = backlog.
- **Status data model disahkan:** `ujian`(created_at,nama_sesi), `ujian_item`(tahun INT,subjek_id), `ujian_markah`(markah,is_td,tarikh), `ujian_gred`(per ujian). `kelas`(tahun TEXT, tahun_sesi INT). SQLite numeric affinity buat `k.tahun`='5' match `ui.tahun`=5.
- **Spec:** `docs/superpowers/specs/2026-06-30-trend-markah-etr-design.md` commit `26fb6d4` (base `4b091c0`, branch test).
- **NEXT:** master review spec → writing-plans → execute (subagent-driven/inline). Branch test.

### 🆕 Sesi 2026-06-29 (malam ~00:45–04:57): mypwa-v2 — Cache Drill-down Gred (optimization) ✅ LIVE PRODUCTION (main `4b091c0`)
**Asal soalan master:** "bila klik gred, jika 100 murid, berapa kali request?" → Lucy semak kod `bukaSenaraiGred()` (dashboard.html:756). Jawapan: 1 request/klik (server pulang SEMUA murid sekali gus via `detail=1`, frontend `.filter()` ikut gred). Tiada N+1. Master minta optimize.
- **Masalah halus:** klik gred BERLAINAN pada carta SAMA fetch semula tiap kali walaupun `detail=1` dah pulang semua gred. 6 gred = 6 request data identik.
- **Fix (`public/dashboard.html` `bukaSenaraiGred`, +9/-2):** cache response pada `ctx._cache`. Klik 1 → fetch & simpan; klik gred lain carta sama → guna cached, 0 request. **Auto-invalidate** sebab `_gredCtx={}` reset dalam muatAnalisis/Semua/Trend (baris 497/547/637) — tiada data basi, tiada logik invalidation manual. KISS.
- **Refine CLEAR:** disahkan `ctxId` dikira sekali per carta (dashboard.html:727, luar `.map()`) → semua baris gred carta sama kongsi ctx object sama → kongsi cache. Carta lain (Keseluruhan/Tahun/Kelas/Trend) ctxId asing → cache asing, fetch sekali setiap satu.
- **Behavior-preserving:** klik pertama tetap 1 fetch macam dulu; cuma klik ke-2+ pada carta sama yang berubah. Risiko regresi rendah.
- **Seal:** Build CLEAN (wrangler dry-run, 19 files, D1 OK), Secrets CLEAN, Logic VERIFIED. Playwright Lucy tak run (sandbox tiada network) — feature asal dah disahkan master staging, perubahan ni frontend-only.
- **Commit:** `4b091c0` (base `ed1ea74`). Push `ed1ea74..4b091c0 test->test`.
- **✅ MERGE MAIN (master verify staging OK):** ff-only berjaya kali ni (test linear atas main — `ed1ea74`+`4b091c0` terus dari `ae609e2`). Merge `ae609e2..4b091c0 main->main`, push main → GitHub Actions auto-deploy production. Frontend-only, 0 migration. **Latest main = `4b091c0`.** Lucy balik branch test selepas merge. TIADA kerja tertunggak.

### 🆕 Sesi 2026-06-28 (petang ~13:12–14:05): mypwa-v2 — Drill-down Carta Gred ke Paparan GURU ⏳ PUSH TEST, PENDING TEST STAGING
**Asal soalan master:** "ingat tak kita buat boleh klik carta keluar nama murid?" → echo-recall jumpa feature drill-down dalam **pelawat.html** (dibuat 2026-06-26). Master minta **extend ke paparan guru**.
- **Apa:** port feature klik baris gred (count>0) → modal senarai murid (nama·kelas·markah) dari `pelawat.html` ke `dashboard.html` (tab "Ujian Dalaman"). Backend `/api/ujian-markah/analisis?...&detail=1` SEDIA ADA → **0 backend, 0 migration, 0 package, 1 fail + 1 test.**
- **Fail (`public/dashboard.html`):** +CSS `.gredRowKlik`/`.gred-modal-*`; +modal `#gredModal`; +`esc()`, `bukaSenaraiGred()`, `tutupGredModal()`, state `_gredCtx`/`_gredCtxSeq`; `buildChartHtml()` tambah param ke-4 `ctx`. **5 carta jadi clickable** (Keseluruhan, Tahun, Kelas, Semua Subjek, **Trend** — Trend bonus luar plan, master OK). Reset `_gredCtx={}` di `muatAnalisis`/`muatAnalisisSemua`/`muatTrend`. Salin tepat corak pelawat (KISS/DRY).
- **Test baru:** `tests/dashboard.spec.js` — login guru → tab Ujian Dalaman → pilih ujian+subjek → klik `.gredRowKlik` → sahkan `#gredModal` + table muncul → tutup. Mirror `pelawat.spec.js`.
- **Pipeline Kata:** brainstorm(skip, port jelas) → plan(lulus master) → Code → sight-hone CLEAR → commit-seal (Build dry-run CLEAN, secrets CLEAN, **Tests PENDING staging**) → auto-commit.
- **Commit test:** `ed1ea74` (base `ae609e2`). Push `ae609e2..ed1ea74 test->test`. **BELUM merge main.**
- **⚠️ NEXT (master jalankan sendiri — Pilihan A):** tunggu staging deploy (~1-2 min), run:
  `! cd C:/Users/user/Documents/code/mypwa-v2 && TEST_PASSWORD='test123' PELAWAT_PASSWORD='<pelawat>' npx playwright test tests/dashboard.spec.js tests/pelawat.spec.js`
  Perhati: test "Dashboard guru — klik gred..." mesti PASS; "Klik gred dalam Analisis" (pelawat) PASS (regresi). Skip "Tiada ujian/subjek" = data staging kosong, bukan bug. Kalau hijau → master confirm → merge main.

### 🆕 Sesi 2026-06-28 (tengah hari): mypwa-v2 — Nama Fail PDF Slip Individu Ikut Nama Murid ✅ LIVE PRODUCTION (main `ae609e2`)
**Masalah master:** bila cikgu download slip keputusan individu sebagai PDF, nama fail tak ikut nama murid.
- **Diagnosis:** slip individu BUKAN jana PDF sebenar — guna cetak browser (`window.print()`). Nama fail cadangan "Save as PDF" diambil dari `<title>` dokumen. Title lama (`wrapSlipHTML`) sama untuk semua murid (`Slip Keputusan — {nama ujian}`).
- **Fix (`public/laporan-ujian.html`):** tambah param ke-3 opsyen `tajukFail` pada `wrapSlipHTML(slips, ujian, tajukFail)` → guna sebagai `<title>` kalau ada, else default. `cetakSlipMurid()` hantar nama murid. `cetakSemuaSlip()` KEKAL default (banyak murid).
- **Format akhir (master revise 2x):** awalnya `Slip - {nama} ({kelas})` → master minta tukar prefix ke nama ujian → **`{nama ujian} - {nama murid} ({kelas})`** (cth "Peperiksaan Pertengahan Tahun - Ahmad Ali (5 DELIMA)").
- **Nota penting:** ini "Save as PDF" browser — bukan auto-download. Browser tetap papar dialog cetak; hanya nama fail CADANGAN yang berubah. Nak auto-download nama tepat perlu library jana PDF (jsPDF) = perubahan besar, TAK dibuat.
- **Commits test:** `745e9d2` (ikut nama murid, prefix "Slip") → `ae609e2` (prefix tukar ke nama ujian). Dua-dua MERGE MAIN ff-only (`745e9d2`, `ae609e2`). Push main auto-deploy production. 0 migration.
- **Nota git:** ff-only BERJAYA kali ni (test linear bersambung terus dari main `11c287c`). `git pull` sempat gagal sekali (network timeout intermittent) → retry sandbox biasa berjaya.

### 🆕 Sesi 2026-06-26 (malam ~19:00): mypwa-v2 — Label 'Tutup' + Progress Bar Ikut Kelas Sebenar ✅ LIVE PRODUCTION (main `f44cfd4`)
**Apa:** Dua perubahan pada paparan PELAWAT (sambung kerja drill-down petang tadi).
- **Perubahan 1 — Label butang:** card ujian tab Progress, butang toggle detail 'Sembunyi'→'Tutup' (`pelawat.html` togglePelawatDetail). Komit awal `be1d69d`.
- **Perubahan 2 — Progress bar kira kelas sebenar:** master report bar pening — kira ikut "item" (Tahun×Subjek) & 1 murid je dah dikira siap. Master pilih (via AskUserQuestion) kira ikut **kelas sebenar**, kelas lengkap = SEMUA subjek (item tahun itu) × SEMUA murid kelas tu bermarkah/TD.
  - **PENTING (pengajaran):** mula-mula tersilap kira **slot subjek×kelas** (DIAGNOSTIK=12, padahal kelas cuma 3, item cuma 4). Master perasan "12 tu bukan kelas". Verify data sebenar DB → betulkan ke kelas sebenar. **Bukti > teka.**
  - **SQL (`src/routes/ujian.js` query `/ujian`):** tambah `jumlah_kelas` (kelas ada murid yg tahunnya terlibat) + `kelas_lengkap` (`(bil_item_tahun × bil_murid) <= bil_markah_diisi`). Medan lama `item_diisi/jumlah_item` DIKEKALKAN (backward-compat).
  - **Frontend:** `pelawat.html` + `admin.html` guna `kelas_lengkap/jumlah_kelas`, label "X / Y kelas lengkap", header admin "Progress Subjek"→"Progress Kelas". Detail view TAK diusik (master kata dah betul).
- **Bonus fix test:** `pelawat.spec.js` semua gagal di login — bukan kod, app guna **clean URL `/pelawat`** tapi test tunggu `/pelawat.html`. Betulkan `waitForURL` ke regex `\/pelawat(\.html)?`. User pelawat staging = **PKP** (bukan 'pelawat'), creds via env var (lihat memory reference_mypwa_pelawat_test).
- **Verify:** SQL diuji terus DB staging (6 & 3 kelas) + production (12 & 3, DIAGNOSTIK 100%). Playwright pelawat 5 passed/1 skip (sandbox-disabled, izin master). node --check OK.
- **Commits test:** `be1d69d` (Tutup) → `298849a` (progress kelas + fix test) → `aa8b70b` (true-kelas SQL). **MERGE MAIN `f44cfd4`** (`305b3e9`..`f44cfd4`, --no-ff, 4 fail). Push main auto-deploy production. Tiada migration.

### 🆕 Sesi 2026-06-26 (petang/malam): mypwa-v2 PELAWAT — Drill-down Senarai Murid Mengikut Gred ✅ LIVE PRODUCTION
**Masalah master:** dalam tab Analisis pelawat, carta cuma papar BILANGAN murid per gred — tak tahu SIAPA murid gagal (E/F), kelas mana, markah berapa.
- **Penyelesaian:** setiap baris gred (count>0) boleh klik → popup modal papar nama·kelas·markah murid dalam gred+skop tu. Susun ikut kelas→markah rendah-tinggi (murid lemah/berisiko dulu). Semua gred A-F+TD, kedua-dua mod (satu-subjek carta Keseluruhan/Tahun/Kelas + "Semua Subjek" per kad subjek).
- **Pendekatan A (dipilih):** lanjut `/api/ujian-markah/analisis` dengan param opsyen `&detail=1` → respons tambah `data.murid: [{id,nama,nama_kelas,markah,is_td,gred}]`. Tanpa detail = respons BYTE-IDENTICAL (dashboard.html selamat). Lazy fetch bila gred diklik. Senarai modal guna query+param SAMA dgn carta → dijamin reconcile dgn kiraan (termasuk tahun_sesi). Tolak B (`/laporan` tak sokong tahun_sesi → mismatch) & C (route baru, langgar DRY).
- **Fail (3, tiada migration/route/package):** `src/routes/ujian-markah.js` (handler /analisis, +17 baris), `public/pelawat.html` (carta boleh klik + `_gredCtx` registry + modal + bukaSenaraiGred/tutupGredModal + helper esc()), `tests/pelawat.spec.js` (smoke test resilient, skip guard kalau data jarang).
- **Pipeline:** brainstorm→spec→plan→subagent-driven (Task1 backend haiku, Task2 frontend sonnet, Task3 test haiku). Setiap task lulus review spec+kualiti. Task2 Important (innerHTML modal tak escape) → FIX 812d9d3 helper esc() pada kod modal BARU sahaja (interpolasi unescaped sedia ada seluruh fail = luar skop, corak projek). Review whole-branch opus: ✅ READY MERGE, 0 Critical/0 Important, invarian parity+backward-compat dibukti.
- **Commits test:** 3443c0c (t1) → 1021de2 (t2) → 812d9d3 (fix) → ae2c426 (t3). Base b94960d.
- **Deploy:** SEAL (4/5, Playwright tertangguh — sandbox tiada network) → push test (Lucy guna sandbox-disabled, izin master) → master verify staging OK → **MERGE MAIN `305b3e9`** (af7d978..305b3e9, --no-ff) → push main auto-deploy production. Spec/plan: docs/superpowers/{specs,plans}/2026-06-26-pelawat-senarai-murid-gred*.
- **✅ MASTER SAHKAN PRODUCTION OK (2026-06-26 petang):** drill-down berfungsi live erpm-sksalor.celikguru.my. TIADA kerja tertunggak — feature SELESAI.

### 🆕 Sesi 2026-06-25/26 (malam): mypwa-v2 PELAWAT — 3 fix selepas master uji staging
**Konteks DRIFT (R1):** memo lama kata feature "KOD SIAP, BELUM PUSH" — sebenarnya DAH push ke test (`origin/test = b49ea74`, disahkan git fetch). Belum merge main je. Master uji staging → jumpa bug.
- **Fix #1 — Pelawat sangkut "Memuatkan data"** ✅ master sahkan jadi. Punca: `requireAuth` allowlist `PELAWAT_PAGES=['/pelawat.html','/profil.html']` guna `.html`, TAPI Cloudflare hidang **clean URL** `/pelawat` → `location.pathname` (`/pelawat`) tak match → requireAuth pulang null → `init()` henti baris pertama → sidebar+progress kosong. Fix `app.js`: normalize pathname `.replace(/\.html$/,'')` + `PELAWAT_PAGES=['/pelawat']`. (Page lain selamat — guna `normPath()`; cuma blok PELAWAT baru terlepas.) Commit `535df00`.
- **Fix #2 — Buang "Profil Saya"** (master: pelawat dikawal admin sepenuhnya). `app.js`: pelawatLinks tinggal Pemantauan Ujian; PELAWAT_PAGES=['/pelawat'] (buang /profil → pelawat tak boleh akses profil.html). Admin urus password via reset-password. Commit `f0ae5f4`. ⏳ verify.
- **Fix #3 — Skeleton loader tab Progress.** `pelawat.html`: CSS `.skel` shimmer + helper `skeletonProgress(3)`, papar 3 kad placeholder masa init sebelum fetch, diganti data bila loadProgress siap. Self-contained (tiada sentuh app.css). Commit `5f67b69`. ⏳ verify.
- **Diagnosis:** systematic-debugging — kumpul bukti (sidebar kosong+tab ada → _user null; URL /pelawat tanpa .html → punca). Soalan visual + 1-baris console pecahkan masalah, BUKAN navigate DevTools. → **R3 di-forge** (dahulukan bukti rendah-friction). Bash sandboxed (curl staging HTTP 000) — tak boleh verify sendiri, master hard-refresh staging.
- **Fix #4 — Tab Progress 2 kad sebaris** ✅ master sahkan jadi. `pelawat.html` `<style>`: `#progressList{grid-template-columns:1fr 1fr}` desktop, turun `1fr` pada ≤768px. Skeleton ikut grid sama. Commit `b22df5a`.
- **Fix #5 — Bump app.js v6→v7** (pelawat.html sahaja) — elak browser guna app.js?v=6 cached (lama, tiada fix clean-URL) di production. Query `?v=7` baru = pasti fetch segar. panduan.html kekal v6 (page guru, tak terkesan). Commit `a654107`.
- **Commits test malam ni:** b49ea74 → 535df00 → f0ae5f4 → 5f67b69 → b22df5a → a654107. **MERGE MAIN `74bfb3f`** (4fd9ada→74bfb3f, --no-ff, 14 commit, 10 fail +1544/-13). Push main → GitHub Actions auto-deploy production. Tiada migration.
- **⚠️ NEXT (master):** (1) tunggu Actions deploy siap (~1-2 min); (2) verify production `erpm-sksalor.celikguru.my` — login pelawat OK + sidebar 1 link + skeleton + 2 kad sebaris; (3) admin cipta akaun pelawat di UI production (role Pelawat) untuk GB/GPK/PPD/JPN. Production DB asing — akaun staging tak pindah.
- **⚠️ OUTSTANDING:** master verify #2 (sidebar tinggal 1 link) + #3 (skeleton shimmer) di staging (hard-refresh Ctrl+Shift+R sebab app.js?v=6 cached). Lepas semua OK → merge main (pertimbang bump ?v=7 supaya client dapat app.js baru). Pelawat seed akaun staging: SUDAH (master daftar sendiri via admin).

### 🆕 Sesi 2026-06-25 (tengah hari): mypwa-v2 — Paparan Pemantauan PELAWAT [⚠️ DRIFT: sebenarnya DAH push test b49ea74 — lihat entry malam 25/26 Jun di atas]
**Apa:** Peranan baru `PELAWAT` (baca-sahaja) + page baru `public/pelawat.html` (2 tab: Progress + Analisis) untuk Guru Besar/GPK + pemantau PPD/JPN tengok progress & analisis Ujian Dalaman tanpa boleh ubah data.
- **Brainstorm (master pilih):** akaun login khas (bukan link awam); skop = progress + analisis; papar nama murid OK (pegawai sah); SATU peranan PELAWAT; progress ada drill-down detail; Pendekatan A (page berasingan, page live tak diusik); security GET-endpoint lain = terima dulu (audiens dipercayai).
- **Backend (3 fail, tiada migration):** `src/middleware/auth.js` middleware baru `adminOrPelawat`; `src/routes/ujian.js` `progress-detail` longgar ke adminOrPelawat; `src/routes/pengguna.js` benarkan role PELAWAT. Semua mutation kekal adminOnly → PELAWAT terblock auto. `pengguna.role` TEXT bebas (tiada CHECK constraint).
- **Frontend:** `app.js` (homeFor + requireAuth allowlist `['/pelawat.html','/profil.html']` + renderSidebar pelawatLinks/label), `index.html` (login redirect 3-hala), `admin.html` (opsyen role Pelawat + badge biru). BARU `public/pelawat.html` (Progress: bar + drill-down; Analisis: carta gred Keseluruhan/Tahun/Kelas + cetak — TIADA Trend/Semua-Subjek, YAGNI).
- **Pipeline:** brainstorm→spec→plan(6 task)→subagent-driven. Spec `docs/superpowers/specs/2026-06-25-pelawat-pemantauan-ujian-design.md` (commit 05b87dd). Plan `docs/superpowers/plans/2026-06-25-pelawat-pemantauan-ujian.md` (commit 1542095).
- **Commits feature (branch test, BELUM push):** 5031ab3 (backend) → 316b44c (routing/sidebar) → 93ee736 (admin) → 85d6d1f (Progress) → a44b802 (Analisis) → 945716b (fix window.open guard) → b49ea74 (test pelawat.spec.js). Base 1542095.
- **Review:** setiap task lulus spec+quality review. Final whole-branch (opus) = ✅ READY MERGE, 0 Critical/0 Important. Security disahkan kukuh. Semua Minor diterima.
- **⚠️ OUTSTANDING (master/human):** (1) seed akaun `pelawat`/`pelawat123` DB staging (UI admin atau wrangler d1 execute); (2) run Playwright `tests/pelawat.spec.js` (perlu deploy staging + sandbox-disabled + izin master). Lepas tu: push test → master verify staging → merge main.
- **Backlog dicadang:** dokumen kewujudan PELAWAT + trade-off GET-endpoint dalam memory/CLAUDE.md (supaya kerja endpoint masa depan ingat PELAWAT boleh capai mana-mana GET authMiddleware-only).
- **Ledger:** `.superpowers/sdd/progress.md` (tracking penuh 6 task).

### 🆕 Sesi 2026-06-25 (pagi): mypwa-v2 — Buang Kedudukan dari Slip Cetak Individu ✅ LIVE PRODUCTION
**Apa:** Master minta buang **Kedudukan Kelas** + **Kedudukan Keseluruhan** dari slip cetak keputusan individu murid. Ranking KEKAL dipapar dalam table pivot tab laporan — cuma "hide" dari slip cetak sahaja.
- **Fail:** `public/laporan-ujian.html` — fungsi `buildSlipHTML()`. Buang 2 baris display (`<div>Kedudukan Kelas...`, `<div>Kedudukan Keseluruhan...`) dalam `.stats-section` (tinggal Purata Markah sahaja) + buang 2 pembolehubah yatim `rankKelasStr`/`rankKeselStr`. **1 fail, 4 baris dibuang, 0 backend, 0 migration.**
- **KEKAL tak diusik:** fungsi `kiraRankingSlip()` — sebab ia masih kira `purata`. Objek `rank` masih ada field rankKelas/rankKesel tapi tak dipakai display (KISS, kos buang tinggi). Table pivot tab laporan render di fungsi berasingan → ranking di skrin tak terjejas.
- **Pipeline (task kecil):** Code → refine (grep clean, tiada rujukan yatim) → commit-seal (wrangler dry-run BERSIH, 18 assets) → push test `08bfb08`.
- **Deploy:** master confirm staging OK → merge test→main `--no-ff` (`fe77e63..4fd9ada`) → push main → GitHub Actions auto-deploy production. Frontend sahaja.
- **Nota git:** ff-only gagal sebab main kumpul 22 merge commit "Merge branch 'test'" yang test tiada (test = branch linear, main = integrasi). Guna `--no-ff` ikut corak sejarah. Kandungan sebenar 1 fail/4 baris, merge auto tanpa konflik.

### 🆕 Sesi 2026-06-25 (malam): sistem-olahraga — Slider Penilaian Hakim ⏳ STAGING, PENDING VERIFY
**Apa:** Tukar input markah hakim (page penilaian) dari **5 butang toggle (1–5)** → **slider native 0–20** setiap kriteria (slide/drag, mesra-sentuh telefon). Markah penuh/kriteria 5 → 20 (jumlah max auto = bilangan kriteria × 20).
- **Keputusan brainstorm (master pilih):** (1) granularity = nombor bulat 0–20 (`step=1`); (2) makna 0 = belum nilai, gate butang Hantar KEKAL (semua kriteria mesti > 0); (3) Pendekatan A = `<input type="range">` native (bukan custom drag); (4) tambah validasi julat backend.
- **DRY (master minta):** konstan `MARKAH_PENUH = 20` dalam `hakim.js` = sumber tunggal frontend (skor max + slider markup). Backend `src/index.js` kekal literal `20` dgn komen cermin (tak kongsi merentas tier — kekal KISS).
- **Fail disentuh:** `public/hakim.js` (slider render + event + reset, −31 baris lebih ringkas), `public/hakim.html` (CSS slider, buang CSS radio lama), `src/index.js` (validasi `POST /api/hakim/markah`). **0 migration, 0 package, 0 ubah skema.** Backend simpan total sahaja (`markah_penilaian.markah`).
- **Backend validasi:** kira bilangan kriteria aktif dari DB → max = ×20; tolak markah bukan integer / null / <1 / > max / id_rumah kosong / pertandingan tiada kriteria. Server jadi sumber kebenaran julat (tutup jurang sedia ada).
- **Pipeline:** brainstorm → spec → plan (+revise DRY) → subagent-driven (3 implementer + final whole-branch review). Final review: SPEC ✅ KUALITI Approved, 2 Important (id_rumah + markah<1) dah fix.
- **Spec:** `docs/superpowers/specs/2026-06-25-slider-penilaian-hakim-design.md`. **Plan:** `docs/superpowers/plans/2026-06-25-slider-penilaian-hakim.md`.
- **Commits test (push):** `236abda` (backend) → `474ba4f` (slider js) → `4e66a32` (css) → `7e7c66a` (fix) → `be8ccce` (filled track warna). Base sebelum kerja = `657ff9b` (docs). Branch `test`, BELUM merge main.
- **🔨 FORGE (2026-06-25 pagi):** Rule **R2** ditambah dalam `~/.claude/mulahazah/rules.md` — guna `100dvh` (bukan `100vh`) untuk page full-height pada phone, susunan `height:100vh; 100dvh` (dvh menang). Origin: bug `100vh` muncul 2 kali (iPad sidebar mypwa-v2 + hakim.html butang Hantar). Diluluskan master manual. (rules.md = fail tempatan, bukan git, auto-load via mulahazah.)
- **🆕 Fix#2 (2026-06-25 pagi):** master report butang Hantar tersorok di phone (terlalu ke bawah). Punca: `hakim.html` baris 76 `#main-app style="height:100dvh; height:100vh"` — `100vh` (terakhir) MENANG → container lebih tinggi dari viewport nampak, footer `absolute bottom-0` tersorok di sebalik toolbar browser. Fix: tukar susunan → `100vh; 100dvh` (100dvh menang, fallback 100vh). 1 baris, `public/hakim.html`. Sama pattern iPad bug memory (100vh≠visible). Push test `19ce4c7`. ⏳ master verify phone.
- **🆕 Fix#1 (2026-06-25 pagi):** master report slider tak warnakan ruang 0→nilai. Punca: WebKit (Chrome/Safari/mobile) tiada filled track native (`::-moz-range-progress` Firefox-only). Fix: helper `warnaSlider()` dalam `hakim.js` set `linear-gradient` inline ikut `value/MARKAH_PENUH`, panggil di 4 tempat (render, input, reset-belum-pilih-rumah, resetBorang). 1 fail (`public/hakim.js`), 0 CSS. SEALED (wrangler dry-run CLEAN, node --check OK, Playwright skip=visual) → push test `be8ccce`. ⏳ master verify staging.
- **Staging:** `https://sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev` (login hakim → page penilaian).
- **⚠️ NEXT (esok 2026-06-25):** master test sendiri di staging (slider + hantar markah). Kalau OK → master bagi isyarat → Lucy merge test→main (auto-deploy production `atletik.celikguru.my`). Playwright TAK dijalankan (perlu sandbox-disabled + izin master).

### 🏁 Ringkasan Akhir Sesi (2026-06-24 pagi)
Feature **Cetak Markah dari Page Keputusan** (mypwa-v2) — siap, dipoles, teruji, live production.
- **Apa:** butang Cetak dalam `ujian.html` → helaian A4 markah subjek (header sekolah + nama ujian/kelas/tahun, jadual Bil·Nama·Markah·Gred + ringkasan gred + purata/min/max). Boleh cetak ujian yang dah ditutup (mod baca-sahaja).
- **Kos:** 1 fail frontend (`public/ujian.html`) + 1 test. **0 backend, 0 migration, 0 page baru.**
- **Pengesahan:** self-review → wrangler dry-run → Playwright E2E suite **10/10 PASS** (mod sandbox-disabled, izin master) → master confirm browser + phone.
- **Commits main:** `d5c0dd9` (feature + buang Markah Penuh) → `cdd4061` (UI polish: butang seragam navy + buang garis kelabu mobile).
- **Memory baru:** `feedback_sandbox_mode.md` (mod khas guna izin dulu). Diary: 3 entri dalam `daily-diary/current/2026-06-24.md`.
- **Status:** TIADA kerja tertunggak untuk feature ni. Backlog "Compact mode Ujian Dalaman" ✅ DISELESAIKAN (cara lain — preset zoom, bukan compact mode).

**Tambahan (~11:31):** Backlog "Compact mode Ujian Dalaman" diselesaikan — bukan dengan compact mode, tapi **preset `zoom:0.88` + max 6 kad/halaman** dalam cetak analisis dashboard (`cetakSekyen` + `cetakAnalisis3`, helper `binaKadGrid`). Master test print preview OK → merge main `519bf0a`. LIVE PRODUCTION.

**Tambahan (~11:40):** Cetak analisis dashboard — border card + label `.sub` + tagline `.hdr p` kelabu pucat (#e2e8f0/#64748b) → hitam `#1e293b` (seragam dgn nama sekolah/garis header). Master confirm print preview OK → merge main `fe77e63`. LIVE PRODUCTION. **Latest main = `fe77e63`.**

**💡 Backlog dicadang Lucy (belum buat):** satu pass semakan page cetak lain (laporan-ujian slip, pajsk, laporan RPM) untuk teks/border pucat serupa — master belum putuskan.

---

### 🆕 PROJEK BARU (2026-06-24 petang): Chrome Extension — Import PAJSK → IDMe KPM ⏳ BRAINSTORM (belum execute)

**Idea master:** Chrome extension untuk cikgu pindah data dari Tab PAJSK (eNilai/mypwa-v2) ke laman KPM `https://idme.moe.gov.my/login`. Alat produktiviti sah — cikgu pindah data sendiri (yg wajib hantar KPM) lebih pantas, akaun + data sendiri.

**Keputusan brainstorm setakat ni (via soalan):**
- **Q1 IDMe:** taip satu-satu (borang per murid/aktiviti), TIADA upload pukal → extension auto-isi justified.
- **Q2 Cara isi:** SEPARA-AUTO — extension isi semua medan, CIKGU tekan Hantar sendiri (selamat + lembut ToS).
- **Q3 Sumber data:** baca tab eNilai yg terbuka (tiada backend/token/CORS, guna sesi login sedia ada).
- **Q4 Interaksi:** cikgu KLIK rekod dari panel extension → isi borang IDMe yg terbuka (bukan auto-padan murid).

**Pendekatan dicadang (tunggu master pilih):**
- **A ⭐ (Lucy syor):** MV3 Side Panel + eNilai PAJSK page tambah blok JSON tersembunyi (frontend sahaja) utk extension baca data bersih. 2 content script (baca eNilai + isi IDMe) + service worker. Kebal, kemas.
- **B:** Panel terapung + korek (scrape) table eNilai. Tak sentuh eNilai tapi rapuh.

**⚠️ Kebergantungan utama:** bahagian isi borang IDMe perlu PETA MEDAN (field mapping) — master kena bagi struktur borang IDMe (screenshot + inspect element), sebab Lucy tak boleh log masuk laman KPM. Bina berperingkat (config boleh-ubah).

**✅ BRAINSTORM + SPEC + PLAN SELESAI (2026-06-24 petang):** Master pilih Pendekatan A + edaran 2 fasa (unpacked → Web Store Unlisted). Repo baru `C:\Users\user\Documents\code\idme-pajsk-ext` (git init, branch master). Spec commit `ca3abcc`, Plan commit `890821f`.
- **Spec:** `idme-pajsk-ext/docs/superpowers/specs/2026-06-24-extension-pajsk-idme-design.md`
- **Plan:** `idme-pajsk-ext/docs/superpowers/plans/2026-06-24-extension-pajsk-idme.md` — 8 task TDD.
- **Bentuk data eNilai disahkan** (pajsk.html renderLaporan, `_lLastData.list`): rekod ada `nama_murid, nama_kelas, kategori, nama_aktiviti, peringkat, pencapaian, catatan, drive_link, nama_sesi`. Blok JSON `#pajsk-export` (Task 3) dalam mypwa-v2.
- **Urutan:** Task 1,2,5 boleh mula serta-merta (tulen, tiada external); 3-4 perlu eNilai; 6 perlu 5 (uji lawan mock-idme.html); **Task 7 GATED** — master kena inspect borang IDMe sebenar bekal selector. Task 8 sedia Web Store.
- **Ujian:** Node built-in `node --test` (TIADA npm package — hormat pantang). Fail `lib/*.js` dual-mode (content script + require).
- **NEXT:** master pilih cara execute (subagent-driven / inline / work-plan). Boleh siapkan Task 1-6 tanpa tunggu IDMe.

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-24 pagi: mypwa-v2 — Cetak Markah dari Page Keputusan (REVISI penempatan)** ✅ SELESAI & LIVE PRODUCTION (main `cdd4061`), teruji E2E 10/10, master confirm browser + phone

  Master revise plan "Cetak Markah Subjek" semalam. **Tukar penempatan:** buang idea page baru `markah-subjek.html` → letak butang **Cetak terus dalam `ujian.html` (page Keputusan)**.

  - **Keputusan master (via revise):** (1) butang Cetak dalam ujian.html, bukan page baru; (2) skop = satu kelas+subjek dipapar (buang "Semua Kelas Saya"); (3) KENA boleh cetak ujian DITUTUP; (4) kandungan = jadual + ringkasan gred + purata/min/max; (5) header = logo+nama sekolah, nama ujian, kelas, tahun.
  - **Penemuan teknikal:** `GET /ujian` (tanpa `?input=1`) DAH pulangkan semua ujian utk guru termasuk tutup (`src/routes/ujian.js:12-13` — where kosong). Jadi cuma tukar `/ujian?input=1`→`/ujian`. **0 baris backend, tiada migration, tiada page baru.**
  - **Fail disentuh:** `public/ujian.html` (6 suntingan: endpoint, ujianMap, detect tutup, mod baca-sahaja `terapkanModTutup()`, butang #btnCetak, `cetakMarkah()`+`kiraGredCounts()`). BARU `tests/cetak-markah.spec.js`. Spec+plan dikemas (revisi). `.gitignore` tambah `.superpowers/`.
  - **Mod baca-sahaja:** ujian tutup → input markah disable + Simpan disorok + nota 🔒, Cetak kekal aktif (markah tersimpan tetap boleh cetak). Elak guru Simpan ke ujian tutup (backend tolak 403).
  - **Ringkasan gred + purata dikira CLIENT-SIDE** dari markahMap/tdMap guna appKiraGred() — tiada panggil /analisis (DRY, elak ownership-gate).
  - **Pipeline:** Code → sight-hone(self) CLEAR → wrangler dry-run BERSIH → commit-seal → push test. Commit `0bcf380` (test). Push a7b6950→0bcf380.
  - **⚠️ TAK DAPAT run Playwright:** sandbox Bash tiada network keluar (curl github.com pun code=000). Verify E2E TERTANGGUH — master kena uji di browser.
  - **✅ MASTER VERIFY STAGING OK → MERGE MAIN:** master confirm staging OK (+ minta buang 'Markah Penuh: 100' dari baris sub cetak, commit `164dda3`). Merge test→main `d5c0dd9` (8a4cc86→d5c0dd9), push main → GitHub Actions auto-deploy production (frontend sahaja, tiada migration). Commits feature: 0bcf380 (cetak) + 164dda3 (buang markah penuh).
  - **✅ MASTER CONFIRM PRODUCTION OK (2026-06-24 pagi):** ujian.html → Cetak berfungsi live di production. Diary disimpan (daily-diary/current/2026-06-24.md). E2E Playwright suite penuh 10/10 PASS (guna mod sandbox-disabled dgn izin master — lihat feedback_sandbox_mode.md).
  - **✅ UI POLISH (2026-06-24 ~10:19):** master report 2 isu dari phone — (1) butang Cetak vs Simpan tak konsisten saiz/warna; (2) garis kelabu td tak align dalam kad mobile. Fix: Cetak btn-ghost→btn-primary (master pilih dua-dua navy solid saiz sama); mobile media query `#muridTable td{border-top:none}` (punca = global `td{border-top}` app.css:209 bocor ke layout flex card). Commit test `2ad3414` → master confirm phone OK → merge main `cdd4061`. LIVE PRODUCTION. FEATURE + POLISH SELESAI.

- **Sesi 2026-06-23 petang/malam: mypwa-v2 — Cetak Markah Subjek Saya (SPEC + PLAN SIAP, BELUM execute)** ⏳ NEXT: execute esok

  Seorang cikgu minta boleh cetak markah Ujian Dalaman yang direkod — **tapi subjek dia sahaja**. Page sedia ada `laporan-ujian.html` papar pivot SEMUA subjek, jadi cikgu tak boleh cetak helaian subjek dia seorang.

  - **Keputusan reka bentuk (master pilih via brainstorm):**
    1. Jenis = **Markah Ujian Dalaman** (markah nombor + gred), BUKAN RPM/TP.
    2. Skop = boleh pilih **satu kelas** ATAU **semua kelas cikgu ajar**.
    3. Isi helaian = **Bil · Nama · Markah · Gred** + **ringkasan gred** (bilangan per gred + purata/min/max). TIADA ranking, TIADA ruang tandatangan.
    4. Penempatan = **page baru khas** `public/markah-subjek.html` (bukan ubah laporan-ujian.html).
    5. "Semua Kelas Saya" = **page-break per kelas** (setiap kelas helaian berasingan + ringkasan sendiri, Bil mula semula 1).
  - **Penemuan teknikal penting:** data DAH WUJUD lewat endpoint sedia ada — **tiada backend baru, tiada migration**:
    - `/api/ujian-markah/jadual?ujian_id=` → kelas+subjek cikgu (kunci skop "subjek dia", tapis ikut jadual_guru)
    - `/api/ujian-markah/murid?ujian_item_id=&kelas_id=` → murid+markah+is_td
    - `/api/ujian-markah/analisis?ujian_id=&subjek_id=&kelas_id=` → counts per gred + gredScale + markah_penuh
    - `appKiraGred()` (app.js shared) untuk gred; `/api/tetapan` untuk header cetak
    - `GET /api/ujian` (tanpa `?input=1`) → guru nampak SEMUA ujian termasuk yang DITUTUP (boleh cetak markah lepas peperiksaan tamat)
  - **Fail disentuh:** BARU `public/markah-subjek.html` + BARU `tests/markah-subjek.spec.js` + MODIFY `public/app.js` (1 link dalam guruLinks grup 'ujian').
  - **Spec:** `docs/superpowers/specs/2026-06-23-markah-subjek-saya-design.md` (commit f6e6127)
  - **Plan:** `docs/superpowers/plans/2026-06-23-markah-subjek-saya.md` (commit 95c48af) — 5 task bite-sized, kod konkrit penuh.
  - **NEXT (ESOK 2026-06-24):** master pilih cara execute — **subagent-driven disyorkan** atau inline. Branch `test`.
  - **Nota test:** page guru-only → test MESTI login GURU `TEST_USER=test TEST_PASSWORD=test123`. Bahagian render markah data-dependent → smoke test direka resilient (assert struktur + tiada JS error). Page perlu sampai staging dulu sebelum Playwright lawan staging sah (push test auto-deploy, atau guna `wrangler dev`).
  - **Nota security (highlighted):** `/murid` & `/analisis` tiada ownership-gate (pre-existing, sama macam laporan-ujian.html). Page baru lebih ketat di UI. TAK diubah dalam kerja ni.
  - **Commits test setakat ni (spec+plan sahaja, BELUM push):** 50441b7 (spec) → f6e6127 (spec perjelas) → 95c48af (plan). Kod BELUM ditulis.

- **Sesi 2026-06-22 malam: Audit Memory Drift + Forge Rule R1 + Penjelasan erpm-v2** ✅ SELESAI (housekeeping, bukan kerja kod)

  Master perasan session brief tunjuk status basi ("audit log belum execute" sedangkan dah live). Siasat → jumpa baris index MEMORY.md DRIFT dari realiti (kerja siap & merge main tanpa memo dikemaskini).

  - **4 entry basi dibetulkan (semua disahkan via git):** (1) mypwa-v2 Audit Log Fasa 9 ✅ live main; (2) sistem-olahraga Arkib Tahunan ✅ live main (7 fasa, 76 is_arkib + 6 endpoint); (3) ADNI ✅ deploy main (main==test==db35628); (4) eRPM v2 — dijelaskan keliru folder.
  - **Disahkan TEPAT (tak diusik):** BrightMe, celikguru vision, sistem asas olahraga.
  - **Rule R1 di-forge** dalam `~/.claude/mulahazah/rules.md` (folder dicipta baru): verify status "pending/belum execute" dengan git SEBELUM percaya/papar dalam brief. Betulkan memo serta-merta bila jumpa drift.
  - **erpm-v2 vs mypwa-v2 dijelaskan dalam memory:** mypwa-v2 = produk khas per-SEKOLAH (DIKEKALKAN, live, erpm-sksalor.celikguru.my). erpm-v2 = SaaS multi-tenant per-GURU (MASTER+GURU, self-register) — BELUM MULA (folder scaffolding sahaja, 1 commit, 0 fail kod, idle sejak 14 Apr). Plan: memory/projects/erpm-v2-plan.md.
  - **⚠️ Security nota erpm-v2:** plan guna SHA-256 kosong — regresi vs PBKDF2 100k+salt projek lain. Bila execute, ikut PBKDF2.
  - **Pengajaran teras:** memory ≠ realiti. Snapshot masa-lampau; status kerja berubah, memo tak auto-update.

### Session Recap (Lama)

- **Sesi 2026-06-22 tengah hari: mypwa-v2 — Drag Susun Kedudukan Dokumen** ✅ SEALED, PUSHED test, staging live. PENDING master verify → merge main.

  Master nak admin boleh drag-and-drop susun kedudukan dokumen dalam Tab Dokumen (admin sahaja); susunan terpakai untuk paparan guru. Pipeline Kata penuh (brainstorm→spec→plan→subagent-driven→sight-hone/review→commit-seal→push). KISS & DRY.

  - **Konteks penting:** "Tab Dokumen" = table `panduan` (id, nama, url, created_at). Admin urus di `admin.html` tab "Dokumen", guru tengok di `panduan.html`. Fail/route kekal nama `panduan` (bukan `dokumen`).
  - **Keputusan reka bentuk (master pilih):** (1) buang pagination di admin (senarai penuh, drag bebas), (2) drag-and-drop native HTML5 (desktop), (3) guru view kekal pagination — auto ikut susunan baru. Tiada audit log untuk reorder.
  - **Migration 025** (`025_panduan_urutan.sql`): `ALTER TABLE panduan ADD COLUMN urutan INTEGER` + backfill correlated-COUNT ikut `nama ASC` (preserve paparan). ⚠️ Framework `migrations apply` GAGAL pada staging (024 tak tracked, duplicate column tarikh_tutup) → apply terus guna `d1 execute --file`. Sama isu akan berlaku utk production — apply 025 guna `d1 execute` masa merge main.
  - **Backend** (`src/routes/panduan.js`): GET `ORDER BY urutan ASC, id ASC` + sokong `?all=1` (admin-gated, semua rekod tanpa LIMIT); POST set `urutan = MAX+1`; endpoint baru `PUT /api/panduan/urutan` (adminOnly, body `{ids:[]}`, validate Number.isInteger, `db.batch()`) — WAJIB daftar SEBELUM `PUT /:id`.
  - **Frontend admin** (`public/admin.html`): buang pagination (#panduanAdminPg, _panduanPage), `loadPanduanAdmin()` no-arg guna `?all=1`, baris `draggable` + handle ≡, fungsi `initPanduanDrag/renumberPanduan/saveUrutanPanduan`. Guru `panduan.html` tiada perubahan.
  - **Review (Opus) finding Important:** `?all=1` asalnya tak admin-gated → Lucy tambah guard `c.get('user').role !== 'ADMIN' → 403` (commit f9771e4). Minor (drag-luar-table tak persist, test smoke-only, whole-row draggable) — deferred, acceptable.
  - **Test:** `tests/dokumen.spec.js` baru (assert draggable + handle ≡ + #panduanAdminPg count 0). Suite penuh 9/9 PASS lawan staging (a11y flake ERR_NETWORK_CHANGED sekali, re-run hijau).
  - **Credentials staging admin:** `admin` / `fcoy4994` (sama dengan production).
  - **Spec:** `docs/superpowers/specs/2026-06-22-drag-susun-dokumen-design.md`. **Plan:** `docs/superpowers/plans/2026-06-22-drag-susun-dokumen.md`.
  - **Commits test:** `6ad926e`→`f9771e4`. Merge main = `a953d20` (feature) → `8a4cc86` (ci fix).
  - **✅ DEPLOY PRODUCTION (master confirm browser):** migration 025 applied prod DB (`d1 execute`, 9 dokumen backfill 1-9). Merge main + deploy production manual (wrangler, Version b5a8e426). Feature DISAHKAN LIVE oleh master dalam browser (erpm-sksalor.celikguru.my). ⚠️ Mesin master tak boleh curl/Playwright ke worker semasa sesi (HTTP 000/SSL err — rangkaian local flapping); verify via wrangler API + browser master sahaja.
  - **✅ CI FIX (penting — pengajaran):** GitHub Actions auto-deploy PRODUCTION gagal (staging OK). Punca: (1) secret `CLOUDFLARE_API_TOKEN` lama kurang permission **Zone: Workers Routes Edit + Zone Read** untuk `celikguru.my` — production ada custom route `erpm-sksalor.celikguru.my/*`, staging tiada route jadi tak terjejas; (2) `cloudflare/wrangler-action@v3` telan output error. **Fix:** master jana token baru guna template "Edit Cloudflare Workers" + include zone celikguru.my; tukar step production `deploy.yml` dari wrangler-action → `run: npx wrangler deploy --env production` (env CLOUDFLARE_API_TOKEN+ACCOUNT_ID) supaya error nampak penuh. Hasil: run main `8a4cc86` HIJAU. Push main akan datang auto-deploy production OK.

- **Sesi 2026-06-21 petang: sistem-olahraga — EXECUTE Reset Password Sekolah** ✅ KOD SIAP, SEALED, DEPLOY STAGING(-test). PENDING master verify manual → merge main

  Master pilih execute plan reset password sub_admin. Semua 5 task siap:
  - **Backend** (`src/index.js` selepas line 2952): `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password`. Guard 3 lapis (id_pengguna+id_sekolah+peranan='sub_admin') dalam SELECT & UPDATE; validasi min 6 aksara (FE+BE); hash PBKDF2; GET tak dedah hash. Commit `1ccbe72`.
  - **Frontend**: seksyen "Reset Password Sub Admin" dalam modal Urus (`superadmin.html`) + logik `muatSubAdmin`/handler reset+toggle 👁 (`superadmin.js`). ID sebenar: `#urus-pilih-subadmin`, `#urus-password-baharu`, `#btn-toggle-password`, `#btn-reset-password-subadmin`. Commit `3ae75be`.
  - **Ujian**: test 16 ditambah dalam `tests/e2e.spec.js` (gitignored — lokal sahaja, TAK masuk repo). Guna `16` sebab `07` dah dipakai. Assertion utama = kewujudan elemen (deterministik).
  - **Pipeline**: sight-hone CLEAR → commit-seal SEALED (17 ujian sedia ada PASS, wrangler dry-run clean) → push test `3ae75be`.
  - **⚠️ PENEMUAN ENV (penting):** CI deploy push `test` → worker **`sistem-olahraga-sekolah-test`** (db `olahraga-test`). Kod baru DAH live di situ (marker disahkan). TAPI `BASE_URL` ujian + login `dragon`/`f4994` hanya jalan di worker **top-level** `sistem-olahraga-sekolah` (= production, db `olahraga-db`, domain `atletik.celikguru.my`). Db `olahraga-test` nampaknya tiada seed superadmin → ujian automatik & verify tak boleh jalan di `-test`. Ujian 16 'gagal' bukan sebab bug — sebab poll worker salah.
  - **Staging -test URL:** https://sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev
  - **✅ SEED + VERIFY (2026-06-21 petang lewat):** db `olahraga-test` tiada superadmin (sa_cred=0) → Lucy seed `superadmin_credentials` (id=1, username='dragon', password=hash PBKDF2 'f4994') via `wrangler d1 execute olahraga-test --remote`. Hash dijana guna Node webcrypto (params sama: salt 16B, 100000 iter, SHA-256, format `pbkdf2:salt:hash`). Login `-test` kini BERJAYA (peranan super_admin).
  - **VERIFIKASI PENUH lawan -test:** (1) Test 16 Playwright PASS. (2) E2E API: GET sub-admin→reset→login password baru `success=True peranan=sub_admin` (sekolah aktif NBA3003). (3) Guard 404 disahkan utk id bukan sub_admin. Feature FUNCTIONAL end-to-end.
  - **⚠️ DATA TEST DIUBAH semasa verify:** password 2 sub_admin di olahraga-test kini = `ujian123` → `nba3003` (NBA3003, aktif) & `xba3202` (XBA3203, tak aktif). Master boleh reset balik kalau perlu.
  - **Cadangan Lucy:** BASE_URL ujian patut tuding ke worker `-test` (bukan top-level=production) untuk CI hygiene. Sekarang dikekalkan asal (top-level) — keputusan master. olahraga-test = 7 sekolah, 7 sub_admin.
  - **✅ MERGE MAIN (master confirm):** merge test→main `8c5296c` (4 commit: spec, plan, backend, frontend). Push main → GitHub Actions deploy production. DISAHKAN LIVE: marker `urus-pilih-subadmin` ada di workers.dev + `atletik.celikguru.my`; endpoint GET+PATCH pulang 401 (terdaftar, perlu auth). FEATURE LIVE PRODUCTION.
  - **Main commit baru:** `8c5296c` (lama: `1be6002`).
  - **✅ SECURITY REVIEW + FIX (2026-06-21 petang, main `2ae8785`):** master minta semak credential hardcoded terdedah di frontend.
    - Imbasan `public/`: TIADA API key/JWT/token pihak ketiga. Jumpa 1 isu: `superadmin.html:144` ada `value="123456"` (default password lemah untuk daftar sub_admin, nampak dalam page source).
    - **Fix opsyen 1:** buang `value="123456"` + tambah `required minlength="6"` + placeholder; label "Kata Laluan Lalai"→"Awal" (commit `d295d6e`). Backend `POST /api/superadmin/admin` tambah validasi `kata_laluan.length<6 → 400` (commit `4ed2d97`) — sepadan endpoint reset.
    - **JWT_SECRET semakan:** DISET betul di production DAN env test (wrangler secret list) — bersama SUPERADMIN_USERNAME/PASSWORD. Fallback 'sila-tukar-secret-ini' tak pernah dipakai. SELAMAT.
    - **Disahkan LIVE production:** frontend 123456 hilang + minlength ada; backend password 5-aksara → HTTP 400. Merge main `2ae8785`.
  - **✅ FOLLOW-UP (2026-06-21 petang): 2 housekeeping selesai:**
    1. Password `nba3003` + `xba3202` (olahraga-test) di-standard ke `1234` (asal hashed, tak boleh pulih — guna konvensyen test projek). Login nba3003/1234 disahkan berjaya.
    2. `BASE_URL` ujian tukar ke worker `-test` (`sistem-olahraga-sekolah-test...`) untuk CI hygiene — fail `tests/e2e.spec.js` gitignored (lokal sahaja). Suite penuh 18/18 PASS lawan -test → admin_dba1097/1234, gb/1234 semua wujud di olahraga-test. -test kini staging berfungsi.

- **Sesi 2026-06-21 petang (awal): sistem-olahraga — Plan Reset Password Sekolah** ✅ SPEC + PLAN SIAP (kini DAH EXECUTE, lihat atas)

  Master perasan superadmin TAK BOLEH reset password akaun sekolah (sub_admin) bila admin sekolah lupa password. Gap sebenar SaaS multi-tenant — satu-satunya jalan sekarang = padam sekolah (hilang data) atau edit SQL manual.

  - **Keputusan reka bentuk (master pilih):** (1) superadmin taip password baru sendiri, (2) sub_admin sahaja, (3) dropdown senarai sub_admin (perlu endpoint GET).
  - **Design:** 2 endpoint baru dalam `src/index.js` — `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password` (guard 3 lapis: id_pengguna + id_sekolah + peranan='sub_admin'; hash PBKDF2; validasi min 6 aksara). UI dalam modal "Urus Sekolah" sedia ada (dropdown + password + toggle 👁).
  - **Prinsip:** KISS & DRY — guna corak sedia ada (`apiFetch`, `hashPassword`, `serverError`, `selectedTenantId`). Tiada migration/package baru.
  - **Spec:** `docs/superpowers/specs/2026-06-21-reset-password-sekolah-design.md` (commit `fa51bd3`)
  - **Plan:** `docs/superpowers/plans/2026-06-21-reset-password-sekolah.md` (commit `dfa545f`) — 5 task bite-sized.
  - **NEXT:** master pilih cara execute (subagent-driven disyorkan / inline). Branch `test`.
  - **Nota repo:** ujian E2E lawan staging (BASE_URL=staging worker, superadmin `dragon`/`f4994`), bukan localhost. Superadmin boleh ada >1 sub_admin per sekolah.

- **Sesi 2026-06-21 tengah hari: Masa Log Audit → format 24-jam** ✅ LIVE PRODUCTION (merge main `0ff490a`)

  Sambungan dari fix timezone semalam. Master nak masa dipapar dalam sistem 24-jam (bukan PG/PTG).

  - **Fix (`admin.html:1484`):** tambah `hour12:false` dalam options `toLocaleString`. Display-only — logik penukaran UTC→MYT (`replace(' ','T') + 'Z'`) kekal tak berubah.
  - **Kesan:** `11:41 PG` → `11:41` · `4:45 PTG` → `16:45` · tengah malam → `00:xx`.
  - **Pengajaran:** `toLocaleString('ms-MY', ...)` default 12-jam (PG/PTG). Untuk 24-jam guna `hour12:false`. Format paparan terasing dari logik pengiraan masa.
  - Pipeline: Code → Playwright 8/8 PASS → commit-seal (wrangler dry-run CLEAN) → push test `e452827` → master confirm staging → merge main `0ff490a`.
  - **Backlog dicadang:** audit tempat lain yang papar `created_at` — mungkin ada bug timezone/format serupa (BELUM buat).

- **Sesi 2026-06-21 malam: Fix Masa Log Audit → MYT** ✅ LIVE PRODUCTION (merge main `230dfb0`)

  Masalah: master perasan masa log masuk dalam Log Audit (admin.html) salah — tersasar 8 jam.

  - **Punca:** SQLite `CURRENT_TIMESTAMP` simpan UTC format `"YYYY-MM-DD HH:MM:SS"` (guna space, tiada `Z`/penanda zon). String non-ISO macam ni ditafsir `new Date()` sebagai waktu TEMPATAN, bukan UTC. Jadi walaupun `toLocaleString` dah ada `timeZone:'Asia/Kuala_Lumpur'`, input ke `new Date()` dah salah sebelum penukaran zon — hasilnya papar nilai UTC mentah.
  - **Fix (`admin.html:1484`):** `new Date(row.created_at.replace(' ', 'T') + 'Z')` — normalize ke ISO UTC dulu (`"2026-06-20T16:22:00Z"`), baru `toLocaleString` tukar +8 ke MYT betul.
  - **Bukti:** 16:22 UTC → sebelum fix papar 04:22 PTG (salah), selepas fix papar 12:22 PG = 00:22 MYT (betul).
  - **Display-only:** DB tak diubah, data audit lama terus betul, tiada migration.
  - **Pengajaran:** `new Date()` pada string SQLite UTC (space-separated, tiada Z) = ditafsir local time. Sentiasa normalize ke ISO+`Z` sebelum tukar zon. Pattern ni mungkin ada di tempat lain yang papar `created_at` — patut audit masa depan.
  - Commits: `acbb3ac` (test) → merge main `230dfb0`. Playwright 8/8 PASS, wrangler dry-run CLEAN.

### Session Recap (Lama)

- **Sesi 2026-06-14/15: OMR Scanner — 3 Bug Fixes** ⏳ TEST BRANCH (belum merge main)

  Konteks: master test OMR scanner, jumpa 2 isu berturutan. Debug guna data sebenar (Playwright + foto master), bukan teka.

  1. **"Anchor tidak ditemui"** (commit `e67b8c1`):
     - Punca: `detectAnchors` lama cari anchor dalam kuadran TETAP di penjuru gambar. Gagal bila borang kecil di tengah frame (anchor tak sampai penjuru).
     - Fix: auto-detect — Otsu adaptive threshold + connected-components (DFS flood fill) + tapis bentuk kotak pejal + pilih 4 penjuru + validasi segi empat.
     - Terbukti kesan 4 anchor tepat pada foto sebenar (19ms).

  2. **Markah tak tepat (baca kosong)** (commit `92f6344`):
     - Punca: threshold MUTLAK `< 110` terlalu ketat. Murid guna PENSEL (grafit kelabu, brightness ~101-132) → hampir semua bubble dibaca kosong. Brightness per soalan terbukti BETUL (geometri OK), cuma ambang salah.
     - Fix: `detectBubbles` tukar ke KONTRAS RELATIF — banding bubble paling gelap vs purata 3 lain dalam soalan sama (margin >= 18). Auto-laras ikut kertas/cahaya. Terbukti baca 10/10 betul.

  3. **Frame terpotong → hasil salah senyap** (commit `92f6344`):
     - Punca: bila anchor atas keluar frame, algoritma pilih dot bubble sebagai anchor palsu → mapping rosak → markah salah tanpa amaran (bahaya untuk guru).
     - Fix: guard saiz seragam dalam `detectAnchors` — tolak kalau 4 anchor ratio saiz > 3.5 (_found = -2). Mesej baru: "Borang tak masuk penuh dalam bingkai."

  4. **Bingkai panduan nisbah A4** (commit `4f5daf9`):
     - Master perasan bingkai putus-putus terlalu panjang utk A4. Punca: `#guideBox` guna 80%x80% saiz video → nisbah ikut kamera.
     - Fix: CSS `aspect-ratio:210/297` + center + max-width 90%. Visual sahaja, detection tetap scan seluruh frame.

  - **Pengajaran:** gejala sama ("markah salah") boleh ada 2 punca berbeza (geometri + threshold). Kumpul data brightness sebenar → buktikan punca, bukan teka.
  - **Status:** SEMUA 3 fix dah merge main `7d1755c` → LIVE PRODUCTION. Anchor tak perlu dalam bingkai putus-putus (scan seluruh frame); bingkai = panduan zon selamat sahaja.
  - **Detail penuh:** mypwa-v2/CLAUDE.md → section "OMR Scanner — Bug Fixes Detection (2026-06-15)".

### Session Recap (Lama)

- **Sesi 2026-06-12 petang/malam: iPad Bug Fixes — LIVE PRODUCTION** ✅

  1. **Dropdown race condition (ujian.html + laporan-ujian.html):**
     - `selUjian` disabled + "Memuatkan..." semasa `init()` fetch API
     - Loading feedback "Memuatkan..." pada dependent dropdowns semasa `onUjianChange()`
     - Root cause: iPad user tap dropdown sebelum `init()` siap load options

  2. **Sidebar logout button tidak kelihatan pada iPad:**
     - Fix 1: `.sidebar` height → `height:100vh; height:100dvh` (iOS 100vh bug)
     - Fix 2: `#sidebar` → `height:100dvh` (outer container masih 100vh)
     - Fix 3: `.sidebar-nav` tambah `min-height:0` (iOS flex child refuse shrink bug)
     - Fix 4: `.sidebar-logout` → `padding-bottom: env(safe-area-inset-bottom)` (iPad no home button)
     - Fix 5: Button visibility naik — opacity 0.6→0.85, border 0.15→0.3, background subtle
     - Root cause chain: iOS 100vh != visible area, flex min-height bug, button faint on touch device

  3. **Confirmed working on iPad by master** ✅
  4. **Playwright test:** 8/8 PASS (TEST_USER=test TEST_PASSWORD=test123)
  4. **Commits test:** `7aab1f3` → `64507f3` → `2dfaca4` → `59015df` → `d1292b2`
  5. **Merge:** test → main `ad1015e`
  6. **Deploy production:** Version `9591deac-6627-484b-8efe-8fe764b7b925`

- **Sesi 2026-06-12 pagi: OMR Scanner — UI Polish, Bug Fixes, Merge Production** ✅ LIVE PRODUCTION
  - Template, UI, bug fixes (anchor dark detect, duplicate const grid, mobile navbar)
  - Merge test→main `83919b7` — LIVE production
  - Playwright 9/9 PASS

- **Sesi 2026-06-11 tengah hari: OMR Scanner — Execute 6 tasks** ✅ LIVE PRODUCTION

- **Sesi 2026-06-10 petang: Feature Auto-Tutup Ujian — LIVE PRODUCTION** ✅
  - Migration 024, commit `dcc1cbe`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Compact mode Ujian Dalaman | ✅ SELESAI (2026-06-24) | Diselesaikan cara lain: preset zoom:0.88 + max 6 kad/halaman (page-break) dalam cetak dashboard. Live main 519bf0a |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit main: `4b091c0` (cache drill-down gred + drill-down carta gred GURU — LIVE production, ff-only merge dari test)
- Latest commit test: `4b091c0` (sama dengan main — synced selepas merge)
- Playwright test credentials (GURU): TEST_USER=test TEST_PASSWORD=test123
- Playwright PELAWAT creds: user PKP (staging), creds via env PELAWAT_USER/PELAWAT_PASSWORD — lihat memory reference_mypwa_pelawat_test
- App guna clean URL: pelawat mendarat `/pelawat` (bukan `/pelawat.html`)
- Migration 024 applied ke staging + production ✅
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- CSS version: app.css?v=6
- Production version ID: `9591deac-6627-484b-8efe-8fe764b7b925`

### iPad Bug Notes (untuk rujukan masa depan)
- iOS `100vh` ≠ visible viewport — guna `100dvh` untuk fixed elements
- iOS flex children perlu `min-height:0` untuk scroll properly dalam flex column
- iOS touch device tiada `:hover` — jangan design UI yang bergantung pada hover untuk visibility
- `env(safe-area-inset-bottom)` untuk iPad/iPhone tanpa home button
