# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: 2026-07-18 (pagi) — Memory Compaction transition ke Option A (hibrid A+snapshot) ✅ SIAP
**Last Work Activity**: 2026-07-16 (petang — celiksains: brainstorm + spec + pelan SIAP, belum kod)
> Fail ini kini dilayan sebagai **RAM** (Option A). Sejarah lama diringkas ke `## Compacted History` di bawah; detail penuh dalam snapshot. Lihat `compaction/compaction-policy.md`.

### 🆕 Sesi 2026-07-16 (petang ~16:49–18:08): celiksains — PROJEK BARU: brainstorm + spec + pelan ✅ SIAP (belum kod)
**Projek BARU** di `Documents/code/celiksains/`. Aplikasi pembelajaran **sains gamified** murid sekolah rendah (Tahun 1–6), **terbuka umum**, satu pemilik content (celikguru). Belajar sambil main: daftar → pilih avatar → kuiz KSSR → kumpul coin + XP.

**FLOW: brainstorming → spec → writing-plans. Tiada kod lagi. Git init, 3 commit dokumen (`2bca615` spec, `44484a4` pelan, + CLAUDE.md).**

**Keputusan reka bentuk (via AskUserQuestion, master pilih):**
1. Skop = **terbuka umum** (bukan multi-tenant, bukan per-sekolah). Satu pemilik content → seni bina RINGKAS, tiada `id_sekolah`, tiada bug JOIN silang-tenant.
2. Content = **KSSR Tahun+Topik**. Soalan = **campur pelbagai jenis** (Fasa 1 mula MCQ + Benar/Salah dulu, Lucy cadang).
3. Coin = **3 peranan**: kedai avatar + papan pemuka/badge + ganjaran visual. 🔑 Lucy flag & master terima: **pisah dua mata wang** — `jumlah_xp` (kekal, ranking) vs `baki_coin` (belanja). Direka dari Fasa 1 walau kedai Fasa 2 → elak migration.
4. Auth = **DELIMa (Google OAuth KPM) + fallback username/kata laluan**. 🔴 Lucy flag RISIKO: OAuth DELIMa selalu **disekat admin Workspace KPM** untuk app pihak ketiga — mesti uji akaun sebenar dulu, JANGAN bina seluruh app atasnya. → DELIMa ditangguh **Fasa 3**, fallback dulu.
5. Kata laluan = **min 6 aksara, huruf+angka** (master ubah dari cadangan PIN 4-angka Lucy). Lucy nota mesra: mungkin susah untuk Tahun 1, boleh semak masa uji.
6. Content dimasuk via **panel admin celikguru** (CMS ringkas, Fasa 1 teks sahaja, gambar Fasa 4).

**ROADMAP 4 FASA (master setuju):** F1 gelung teras (akaun, avatar percuma, kuiz, coin+XP, admin CMS) → F2 kedai+badge+papan pemuka → F3 DELIMa OAuth → F4 content kaya (padan/isi-kosong, gambar, CSV).

**Reka bentuk Fasa 1 dibentang 3 bahagian (semua master lulus):** (1) seni bina + 5 jadual D1 (`pengguna`/`topik`/`soalan`/`percubaan_kuiz`/`kemajuan_topik`) + auth PBKDF2/JWT-throw/lockout; (2) gelung kuiz — ≤10 soalan, maklum balas serta-merta, **tiada nyawa/timer**, ganjaran +10 coin & +10 XP, **anti-tipu (server kira, sorok jawapan)**; (3) admin CMS CRUD topik+soalan cascade + rancangan ujian Playwright.

**PELAN: 16 task TDD** (`docs/superpowers/plans/2026-07-16-celiksains-fasa1.md`), setiap satu kod penuh + ujian + commit. Stack: Hono + Workers Assets + D1 + hono/jwt + Playwright. Self-review pelan: semua §spec ada task, tiada placeholder, nama konsisten.

**⏭️ NEXT:** master pilih cara laksana — **Subagent-Driven (disyor)** atau Inline. Task 1 perlu `npm install` + `npx wrangler d1 create` (sentuh akaun Cloudflare) → **minta izin master dulu**. Fail projek: `celiksains/CLAUDE.md` sudah dibuat.

--- (rekod sesi lepas) ---
### 🆕 Sesi 2026-07-16 (pagi ~08:32–11:00): mypwa-v2 — ADMIN MUAT TURUN SENARAI MURID ✅ LIVE PRODUCTION (main `a40c638`, custom domain `erpm-sksalor.celikguru.my`)
**main == test == `a40c638`. TIADA kerja tertunggak.** Master mula "sambung mypwa-v2" → aku bawa plan ETR lama, master kata **bukan**, nak feature LAIN. Feature: **admin muat turun (cetak-PDF) senarai murid** — satu kelas per baris + "⬇ Muat Turun Semua" 1 klik (guru dah ada di Kelas Saya, admin tiada). Frontend sahaja, 0 backend/migration/package.

**FLOW: brainstorming → spec → writing-plans → subagent-driven (4 task) → verify → merge production.**

**Reka bentuk (keputusan master):** (1) skop "Semua" = **sesi terpilih sahaja**; (2) fungsi jana-PDF dipindah ke **`app.js` kongsi** (guru+admin satu sumber, DRY); (3) **langkau kelas kosong**; (4) **satu kelas satu muka surat** (master TEGASKAN) via `page-break-before:always`. Guna semula endpoint `GET /kelas?tahun_sesi=`, `/murid?kelas_id=`, `/tetapan`.

**4 TASK subagent-driven (implementer + reviewer setiap satu, semua review BERSIH):**
- T1 `58a8f02` app.js: `borangMuridSeksyen`/`janaMuridPdf`/`janaMuridPdfSemua`/`pilihOrientasiMuatTurun`+`escHtml` (haiku, transkripsi).
- T2 `47dc03b` tetapan.html guru delegate ke app.js (buang salinan lama, +1/-100 DRY).
- T3 `805e5ce` admin.html butang per-baris + header + `muatTurunSemuaKelas()` (tapis `jumlah_murid>0`).
- T4 `99c4268` smoke `tests/admin-muatturun.spec.js`.
- Final whole-branch review: **Ready to merge, 0 Critical**. Reviewer jumpa refactor ini = **peningkatan keselamatan** (borang guru dulu tak escape nama murid langsung).

**HARDENING `0764e9b` (master "proceed"):** fix penemuan final review — tetapan.html helper `esc` (JS-string) untuk onclick `muatTurunMurid`/`hapusJadual`; app.js `escHtml` pada `logo_sekolah` src.

**VERIFY (3 lapis, sandbox-disabled izin master, admin/fcoy4994):** (1) smoke Playwright **2/2 PASS** lawan staging (dua kali — sebelum+selepas hardening, betul-betul run bukan skip); (2) **🔑 VISUAL PDF sebenar** — jana `janaMuridPdfSemua` 3 kelas via stub `window.open`→capture HTML→`page.pdf({preferCSSPageSize})`→**Read PDF** → **3 kelas = 3 muka surat BERASINGAN disahkan mata** (1 DELIMA/1 NILAM/1 ZAMRUD, borang identik guru, stats L/P betul). PDF dibuang lepas (ada nama murid). (3) node --check + wrangler dry-run.

**🔴 DEPLOY GOTCHA PENTING (catat memory):** push `main` (Actions `deploy.yml` sepatutnya `wrangler deploy --env production`) TAPI selepas ~7 min production custom domain **masih kod lama** — Actions lambat/tak pasti siap. Aku deploy **manual** `npx wrangler deploy --env production` (wrangler OAuth login sedia ada) → **custom domain `erpm-sksalor.celikguru.my` TERUS LIVE** (janaMuridPdfSemua✓ escHtml✓ Muat Turun Semua✓). **`mypwa-v2.syazwan-skpp82.workers.dev` masih papar asset LAMA walau cf-cache=null + cache-bust query** — base & prod env **sama nama `mypwa-v2`** tapi workers.dev preview cache asset ikut PATH (abai query). **URL sebenar user = custom domain, itu yang LIVE.** JANGAN deploy tanpa `--env` (top-level tiada route → akan buang custom domain).

**🔑 PENGAJARAN:** (1) master "sambung" ≠ sambung kerja tertunggak — TANYA dulu, jangan andai plan lama; (2) verify cetak = **jana PDF sebenar + Read PDF**, bukan smoke/assertion (smoke cuma sahkan butang buka overlay, TAK sahkan page-break); (3) production `erpm-sksalor.celikguru.my` = authoritative, workers.dev = preview boleh lag.

**Spec:** `docs/superpowers/specs/2026-07-16-admin-muat-turun-murid-design.md`. **Plan:** `docs/superpowers/plans/2026-07-16-admin-muat-turun-murid.md`. **Ledger:** `.superpowers/sdd/progress.md`.

--- (rekod sesi lepas) ---
### 🆕 Sesi 2026-07-15/16 (malam ~22:15–00:05): sistem-olahraga — 38% tweak + 3 FIX UI + label kedudukan + kolum markah akhir ✅ LIVE PRODUCTION (main `d0ede13` merge, doc `03374df`)
**main == test == `03374df`. TIADA kerja tertunggak.** Sambungan sesi petang, via /remote-control (telefon). Frontend sahaja.

**BHG 4 — kolum table Laporan Markah Akhir seragam (`4cac017`, merge `d0ede13`):** master perasan cetak markah akhir kolum tak sama saiz. Punca: `renderMarkahAkhirExtra` (laporan.js) guna **auto-layout** (tiada `table-layout:fixed`/colgroup) → kolum rumah ikut kandungan (label kedudukan Johan/Ketiga/Keempat beza panjang + nombor beza digit). Fix: `table-layout:fixed` + `<colgroup>` dinamik (Kategori 30%, rumah bahagi sama rata `70/dataRumah.length`%). **🔑 VERIFY KAEDAH BETUL (redeem silap semalam):** `setViewportSize(718)` (=190mm A4 portrait @96dpi) + `emulateMedia('print')` DULU, baru ukur → 3 rumah **167px sama (beza 0px)** + screenshot ditinjau (HIJAU/KUNING/MERAH seragam, nama penilaian panjang wrap elok). Spec `tests/verify-markah-akhir-kolum.spec.js`.

**BHG 3 — label kedudukan perkataan penuh (`868e50a`, merge `d3b5a64`):** master tanya patut `Ke-3/Ke-4` kekal atau jadi `Ketiga/Keempat`. Lucy semak konvensyen: **`keputusan.js` (skrin keputusan) DAH guna perkataan penuh** `Johan/Naib Johan/Ketiga/Keempat/Kelima/Keenam` — jadi tukar = selaras, bukan reka baru. Diselaraskan **3 fail** (`laporan.js`+`pengumuman.js`+`arkib-cetak.js`) → array sama `['🥇 Johan','🥈 Naib Johan','Ketiga','Keempat','Kelima','Keenam']` (kekal emoji Johan/Naib). `pengumuman.js`/`arkib-cetak.js` sebelum ni masih ada `'🥉 Tempat Ke-3'` lama (sesi awal tukar laporan.js sahaja) — kini semua selaras. Butang `pengumuman.js:448` "Dedahkan Tempat Ke-3 >" DIKEKAL (frasa butang, master tak minta ubah). Verify: syntax + grep konsistensi (tukar teks label = 0 risiko layout, master pilih skip Playwright).

**BHG 1 — kolum pingat 36→38%, rumah 12→10% (`4af4654` merge, kod `b5b8e5d`):** master minta nama lebih lega. 

**🔴 PENGAJARAN BESAR — MASTER TANGKAP VERIFY AKU TERSILAP KAEDAH:** aku dakwa "46 char muat 1 line, 0/107 wrap" untuk 36% DAN 38%. Master buka **print preview sebenar (Ctrl+P)** → **banyak nama WRAP 2-line**. Punca: spec guna **element screenshot + `emulateMedia('print')`** → render pada lebar **VIEWPORT (~1280px)**, BUKAN A4 portrait (**190mm ≈ 718px**). `emulateMedia('print')` apply CSS print TAPI **TAK ubah saiz halaman jadi A4**. Jadi 38% × 1240px ≈ 470px (muat 46 char) vs 38% × 718px ≈ 273px (wrap). **Ukuran & anggaran "46 char" semua kena lebar SALAH.** Pengajaran kita sendiri kata *"jana PDF sebenar, render jadi imej, TENGOK"* — aku guna element screenshot, tersasar. **Kaedah BETUL: `page.pdf({preferCSSPageSize:true})` → render pdf.js jadi PNG → TENGOK.** [[feedback-verify-cetak-visual]] dikemas.
**Keputusan master:** tolak opsyen landscape, **kekal 38%/10%, TERIMA nama panjang wrap 2-line dalam portrait.** (Portrait 190mm + 9 kolum memang sempit untuk nama 46-char.)

**BHG 2 — 3 FIX UI (`9e1ac87` merge, kod `2754336`):**
1. **`laporan.js`** — baris "Kedudukan Keseluruhan" (`renderMarkahAkhirExtra`) label tempat ke-3 `'🥉 Tempat Ke-3'` → `'Ke-3'` (selaras Ke-4/5/6).
2. **`laporan.js`** — header table pingat cetak (`renderPingatExtra`) `🥇🥈🥉` → **E/P/G** (huruf lebih bersih dari emoji dalam cetakan).
3. **`dashboard.js`+`dashboard.html`** — table Pemenang Terkini admin mobile dulu sesak (4 kolum scroll). Awam (`papan.html`) ada `@media (max-width:639px)` stack jadi **kad** (thead sorok, td block, badge `absolute` sudut kanan) — admin TIADA → disalin. Kolum Kelas **dilipat** ke sel Pemenang (`● Rumah · Kelas`) → 3 kolum sama struktur awam; `id="winners-table"` ditambah. Master hantar screenshot awam sebagai rujukan.

**VERIFY (spec `tests/verify-fix-3batch.spec.js`, gitignored, read-only):** Fix1 assert `seksyen-markah-extra` tiada "Tempat Ke-3" ada "Ke-3"; Fix2 assert header th = E/P/G; Fix3 assert `#winners-table thead display:none` + badge `absolute` + 3 kolum, screenshot mobile ditinjau mata (kad P08/badge/nama/● MERAH · 1 ARIF — sepadan awam). Production disahkan LIVE node fetch (`winners-table`+`max-width:639px`+`'Ke-3','Ke-4'`+`text-xs">E</th>`, ~24s).

**FLOW git:** 38% (b5b8e5d→merge 4af4654) tergantung tanpa doc/ff → sesi ni guna **stash → checkout test → ff → pop** untuk selaras dulu, baru commit 3 fix `2754336` push test → merge main `9e1ac87` → verify LIVE → doc `b5f9bda` (betulkan rekod 38%) → ff test. main==test==`b5f9bda`.

--- (rekod sesi lepas) ---
### 🆕 Sesi 2026-07-15 (petang ~21:06–21:40): sistem-olahraga — UI FIX MOBILE (3 fix) ✅ LIVE PRODUCTION (main `c1841d4` merge, doc `efb29b8`)
**main == test == `efb29b8`. TIADA kerja tertunggak.** Master minta via /remote-control (telefon). Frontend sahaja, 0 backend/migration.

**3 FIX:**
1. **Butang Daftar (Ekspres+Manual)** dalam card *Pendaftaran Acara Baharu* (`admin.html`) → `w-full md:w-auto`. Manual ada kembar "Batal Edit" → wrapper tukar `flex flex-col md:flex-row md:justify-end gap-3` (stack di telefon), batal buang `ml-3` jadi `w-full md:w-auto`.
2. **Butang Simpan** dalam card *Pengurusan Penilaian* (`btn-simpan-penilaian`) → `w-full md:w-auto`.
3. **Table pemegang pingat cetak** (`laporan.js` `renderPingatExtra` colgroup): Nama 30→**36%**, Kelas 8→**12%**, Rumah 20→**12%**, Jumlah 9→**7%** (=100%). Master minta nama & kelas panjangkan + 1 line, rumah pendekkan.

**🔑 PENGAJARAN — "berapa char muat?" → VERIFY, bukan agak:** Master tanya 36% muat berapa char. Lucy anggar matematik ~38 char (A4 portrait 190mm, 8pt bold). **Realiti Playwright: 46 char muat 1 line** (`MUHAMMAD ARYAN DANISH FARID BIN MUHAMMAD FARID`), 0/107 wrap. Font cetak sebenar lebih rapat dari kira-kira. **Keputusan reka: BUANG idea `nowrap` (paksa 1 line + risiko melimpah) → guna lebar kolum + `table-layout:fixed` sebagai limit semula jadi; CSS auto-wrap yang panjang gila je.** Tiada kod limit-char (KISS) — lebar ITU limitnya.

**VERIFY (spec `tests/verify-ui-mobile-fix.spec.js`, gitignored, READ-ONLY 0 tulisan DB):** sajikan admin.html+laporan.{html,js} LOKAL lawan API staging (nba3003/1234). Butang: 308=308px telefon 390 / auto 90·157·107px desktop 1440 (klik tab utama `tab-admin-acara`/`tab-admin-penilaian` DULU sebab butang `hidden` sampai tab aktif). Pingat: screenshot 3 browser ditinjau mata + kira wrap. Production disahkan LIVE node fetch (`width:36%`+`width:12%` ADA, ~8s selepas push).

**FLOW:** commit `6bdf865` push test → (master confirm) merge `--no-ff` `c1841d4` push main → deploy → verify production LIVE → doc `efb29b8` → ff test ke main. Seal: node --check OK, wrangler dry-run 186.77 KiB, 0 secret.


--- (rekod lama diringkaskan) ---

## Compacted History
*Diringkas dari ~35 rekod sesi (2026-06-24 → 2026-07-15) pada 2026-07-18. Detail penuh: `compaction/snapshots/current-session-2026-07-18.md`. Pengajaran teknikal terperinci **sudah hidup dalam auto-memory** (`feedback_*`, `project_*`) — sengaja tak disalin semula (DRY). Ini pointer kesinambungan sahaja.*

**sistem-olahraga** (majoriti sesi 2026-07-05 → 2026-07-15, SEMUA ✅ live `atletik.celikguru.my`):
- **Markah per-hakim** — jadual `markah_hakim` PK 5-lajur, corak RAW+DERIVED (`markah_penilaian` = nilai terbitan), skala turun 20→10, agregat SUM. Deploy 4-fasa manual (jadual→kod→kosongkan), data skala-20 lama dikosongkan (hakim wajib nilai semula). Pengajaran → [[feedback_nilai_terbitan_arkib]], [[feedback_guard_mutation_test]], [[project_sistem_olahraga]].
- **Hardening multi-tenant** — bug `id_kategori` PK komposit, JOIN separuh kunci = baris berganda (LIVE production: NBA3003 papar 84 baris sebenar 42). Guard `npm run guard` dijadikan gerbang CI sebenar (guard lama gitignored = palsu). → [[feedback_pk_komposit_join]], [[feedback_ujian_tolak_dan_terima]], [[feedback_guard_mutation_test]].
- **Cetak laporan** — header berulang guna `<thead>` (bukan `position:fixed`), verify guna PDF sebenar + render imej + tengok. → [[feedback_verify_cetak_visual]], [[reference_curl_ssl_revoke]].
- UI polish mobile bertimbun, senarai pingat pemegang-sahaja, kad responsif tab Keputusan, susunan + cetak senarai peserta/mula, slider penilaian hakim, banner tarikh tutup, acara 50m — semua frontend, live.

**mypwa-v2 / erpm** (sesi 2026-06-25 → 2026-07-16, semua ✅ live `erpm-sksalor.celikguru.my`):
- Logik ETR baru berasaskan gred (`TOV<50 → ETR=50; TOV≥50 → ETR=min(TOV+10,100)`), Trend Markah, drill-down carta gred (+cache), role PELAWAT, nama fail PDF slip ikut nama murid, admin muat turun senarai murid. → [[project_erpm_v2]], [[reference_mypwa_deploy]], [[reference_mypwa_pelawat_test]].

**🔴 ITEM TERBUKA dari tier lama (belum buat — JANGAN kambus):**
- **PROJEK: `idme-pajsk-ext` (Chrome Extension PAJSK → IDMe KPM)** — spec + plan **SIAP** (8 task TDD), repo `Documents/code/idme-pajsk-ext` (spec `ca3abcc`, plan `890821f`). Pendekatan A (MV3 Side Panel + blok JSON `#pajsk-export` dalam mypwa-v2 pajsk.html). Task 1-6 boleh mula tanpa external; **Task 7 GATED** — master kena inspect borang `idme.moe.gov.my` bekal selector medan. Ujian `node --test` (tiada npm). **NEXT: master pilih cara execute.** *(Tiada dalam auto-memory — patut jadi entri `project_` sendiri.)*
- **Rate limiting `/api/login` (sistem-olahraga)** — isu security tertinggi. Sedia ada cuma `setTimeout 1s`, **tiada had cubaan** → brute-force terbuka. Keputusan perlu (mula `brainstorming`): per-IP vs per-username; Cloudflare WAF Rate Limiting (level edge, tiada kod) vs Durable Object/KV counter (boleh kunci akaun). Nota: multi-tenant, SATU `/api/login` untuk semua sekolah → per-IP sahaja mungkin tak cukup. Boleh sekalikan: `POST /api/login` pulangkan **500** bila medan body hilang (patut **400**).
- Backlog projek (Lompat Tinggi Fasa 2 kad responsif, backend DELETE-sebelum-validate C2, guard scan `src/**/*.js` sebelum Code-DRY-extraction, normalisasi markah_penuh, ETR boleh-laras, eksport Excel) **tercatat dalam CLAUDE.md projek masing-masing** — takkan hilang.
- Cadangan audit kecil Lucy (belum master putus): `created_at` timezone di tempat lain; teks/border pucat page cetak lain (slip ujian, pajsk, RPM). **Kekal dalam snapshot.**

*Kredensial ujian yang berulang dalam rekod lama (test/demo DB sahaja) sengaja TIDAK dibawa masuk sini. Tiada `JWT_SECRET`/token API dalam ringkasan ini.*
