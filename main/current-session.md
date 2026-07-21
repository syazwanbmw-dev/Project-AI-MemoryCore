# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: 2026-07-21 (malam) — **eRPH MENENGAH (PROJEK BARU `erph-menengah`)**: 3 bug import dibaiki, ujian 8/8 hijau, PUSHED ke Apps Script & disahkan via clone bebas. ⏳ **MENUNGGU master uji pada sheet sebenar** (terima + tolak).
**Last Work Activity**: 2026-07-21 (~22:45 — push + verify siap, serah ujian pada master)
> Fail ini kini dilayan sebagai **RAM** (Option A). Sejarah lama diringkas ke `## Compacted History` di bawah; detail penuh dalam snapshot. Lihat `compaction/compaction-policy.md`.

### 🆕 Sesi 2026-07-21 (malam ~21:41–22:45): eRPH MENENGAH — PROJEK BARU, 3 bug import ✅ DIBAIKI & PUSHED (⏳ belum disahkan master)
**Projek BARU `Documents/code/erph-menengah/`** (git lokal, tiada remote). Sheet **erph-fix** — master sahkan **salinan ujian**. Script ID `1Rc9Stj1...`. 2 commit: `108ee16` (baseline) → `b6e2b5c` (fix). Auto-memory: [[project_erph_menengah]].

**KONTEKS:** gejala sama macam erph rendah semalam ("Import Berjaya tapi kosong") tapi **fail berlainan** — ini untuk sekolah **menengah**, semalam sekolah **rendah**. Master minta folder baru.

**🔴 GOTCHA AKSES — clasp 403 `The caller does not have permission`:** clone gagal. **Ujian kawalan menyelamatkan diagnosis** — clone script LAMA (rendah) pada saat sama **BERJAYA** → memadam 4 kemungkinan serentak (network/login/API/ID salah), tinggal satu: akaun tiada akses. clasp log masuk sebagai `g-77420159@moe-dl.edu.my` (DELIMa); sheet erph-fix dibuat bawah akaun Google lain. **Selesai: master share sheet ke akaun DELIMa sebagai Editor** (bukan tukar `clasp login` — login clasp GLOBAL, tukar-tukar = risiko silap push antara dua projek). ID script sah = **57 aksara** (sama panjang dgn rendah) — cara cepat bezakan dari ID spreadsheet.

**🔑 KOD MENENGAH BUKAN KEMBAR RENDAH — jangan port buta.** 14,173 bytes vs 34,543 (rendah). Diff 312+/786−. **4 fail** (rendah 3) — ada `baiki dropdown.js` (baik pulih data validation, TIDAK disentuh; ia sahkan susun atur: 8 blok, jarak **31 baris**, mula baris 7).

**3 BUG (semua wujud, satu LEBIH TERUK dari rendah):**
1. `parseRPHStrict` `/^OBJEKTIF:/` vs `**OBJEKTIF:**` ChatGPT → tambah `bersihkanMarkdown()` (buang `**`/`__`/`#`) sebelum padanan tajuk DAN sebelum simpan isi; regex juga terima `OBJEKTIF :`.
2. **LEBIH TERUK:** `importRPHStrict` kosongkan **KESEMUA 8 blok tanpa syarat** sebelum parse → import gagal padam 8 RPH; import separa padam 7 yang lain walau "berjaya". Fix: pecah **FASA 1 parse → FASA 2 tulis**.
3. `runImport()` tiada `withFailureHandler` + `alert("✅ Import Berjaya!")` **tanpa syarat** → gejala "Berjaya tapi kosong" datang dari sini. Fix: `importRPHStrict` pulangkan laporan sebenar (bil. RPH + bil. sel), dialog papar laporan itu, +`withFailureHandler`, butang pulih bila gagal.

**🔑 KEPUTUSAN REKA MASTER (AskUserQuestion):** skop padam = **hanya blok yang ada dalam teks** (bukan semua 8, bukan ikut bulat-bulat fix rendah) → master boleh jana ulang SATU RPH tanpa melesapkan yang lain.

**TDD:** `tests/import.test.js` port dari rendah + disesuaikan (stub `getActiveSheet`, rekod `clearContent` ikut julat). **Merah 7/8 dulu → hijau 8/8**. Termasuk ujian SKOP (import RPH 3 → hanya `B84:K87`+`B91:K93` dikosongkan, blok 1 TIDAK) dan ujian berpasangan terima/tolak.

**VERIFY PUSH:** `clasp push --force` → 4 fail, `tests/` disekat `.claspignore` ✅. **Disahkan `clasp clone` ke folder bebas**: 4 fail sahaja di server, diff kosong lawan lokal, `bersihkanMarkdown`/FASA 1/FASA 2/`withFailureHandler` betul-betul ada. (Gotcha clasp semalam tidak berulang.)

**🔴 GOTCHA PowerShell:** here-string `@'...'@` untuk `git commit -m` **PECAH** bila mesej ada petikan berganda `"` — git terima pathspec berpecah, commit tak jadi. **Guna `git commit -F fail.txt`** untuk mesej panjang/ada petikan.

**⏭️ TERTUNGGAK:**
- ⏳ **Master uji sheet sebenar** — (a) TERIMA: import betul → `✅ N RPH diimport (RPH …) — X sel diisi`; (b) TOLAK: tampal sampah → `❌ Tiada RPH sah dikesan — TIADA sel diubah` **dan RPH sedia ada mesti kekal**.
- ✅ **`runJana` SELESAI (`d188183`, pushed+verified 23:08).** Master tanya "rosak juga ke" — jawapan jujur: **TIDAK**. Ia cuma MEMBACA sheet → tak boleh padam data, tak pernah tipu "berjaya", laluan berjaya pulihkan label betul. Satu-satunya masalah = butang tersangkut senyap "Menjana..." bila gagal. Dibaiki sebab senyap itu sendiri musuh. JS dialog **tiada ujian unit** (browser-side) — syntax + bacaan sahaja.
- 🚫 **Sengaja TIDAK disentuh:** `PadamRPH()` ada julat mati sampai baris 496 (RPH menengah habis ~254) + komen `// RPH 1` sebenarnya lindungi 2 blok. **Ia berfungsi betul** (baris 9–248 tercakup) — kod tapak pihak ketiga, ada amaran "TAPAK AKAN ROSAK". Biar.

### 🆕 Sesi 2026-07-20 (11:52–18:05): eRPH — PROJEK BARU, bug import ✅ SIAP LIVE + brainstorm RPT belum selesai
**Projek BARU `Documents/code/erph/`** (git lokal, tiada remote). Google Apps Script terikat pada Google Sheet eRPH cikgu. Ditarik guna **clasp** (v3.3.0 dah sedia ada, `.clasprc.json` sedia login — tiada npm install). 5 commit `cf97854`(baseline) → `9024b10`. Auto-memory: [[project_erph]], [[feedback_bukti_saluran_lossy]], [[feedback_pdf_ke_md]].

**BHG 1 — BUG "Import Berjaya tapi kosong" ✅ SIAP, master sahkan berjaya pada sheet sebenar.**
Satu gejala → **tiga bug**:
1. **Punca yang master rasa:** ChatGPT keluarkan `**OBJEKTIF:**` (markdown bold); `parseRPHStrict` guna `/^OBJEKTIF:/` → tak padan → tiada isi terkumpul. Fix: `bersihkanMarkdown()` buang `**`/`__`/`#` sebelum padanan tajuk DAN sebelum simpan isi.
2. **Lebih bahaya, master tak tahu wujud:** `clearContent()` dipanggil SEBELUM parse → setiap import gagal MEMADAM RPH sedia ada. Betul-betul berlaku (SELASA RPH 1-3 terpadam masa ujian aku suruh; dipulih via Version history). Fix: parse dulu, kosongkan hanya bila ada isi sah.
3. **Sebab 1&2 boleh bersembunyi:** `runImport()` satu-satunya pemanggil `google.script.run` tanpa `withFailureHandler` → ralat ditelan. Ditambah + laporan jujur ("Berjaya" hanya bila ada sel benar-benar diisi).
Ujian `tests/import.test.js` (`node --test`, tiada npm), harness `new Function(kod)` + stub SpreadsheetApp = uji kod SEBENAR. Merah 2 → hijau 6/6.

**🔴 PENGAJARAN BESAR — SALURAN BUKTI LOSSY:** teks yang GAGAL pada master **LULUS** dalam ujian aku, dua kali. Sebab **chat render markdown dan buang `**`** — teks yang sampai pada aku bukan teks yang gagal. Aku hampir salahkan regex penanda dan tulis fix yang SALAH. Penyelesaian: berhenti minta master paste, tambah diagnostik yang **dump aksara mentah dari DALAM Apps Script** (`<U+200B>`, `<U+2022>`). Satu klik → punca terpampang. → [[feedback_bukti_saluran_lossy]]. Sepupu [[feedback_verify_cetak_visual]].

**🔴 GOTCHA clasp:** `clasp push` TIDAK buang fail lebihan di server bila ia anggap "already up to date" — `tests/import.test.js` tersangkut di sana ~90 saat. BAHAYA: Apps Script muat SEMUA fail ke satu skop global; `require()` akan lumpuhkan seluruh skrip master. Paksa push sebenar (ubah fail) + **sahkan dengan `clasp clone` ke folder lain**. `.claspignore` kini sekat `tests/**`.

**2 SILAP AKU, diakui pada master:** (a) suruh master klik Import sebelum aku baca urutan `clearContent()`→`parse()` → RPH master terpadam; (b) fail ujian terpush ke skrip master.

**BHG 2 — BRAINSTORM "Lucy, buat RPH minggu 24" (BELUM SIAP, tunggu master).**
Master mahu cakap je → Lucy rujuk MENU (subjek ikut hari) + RPT (SK/SP ikut minggu) → jana terus. **Master pilih seni bina: Lucy buat dalam chat**, bukan butang dalam sheet → **TIADA API key, TIADA kos**. Fasa 3 Gemini/API **DIBATALKAN**.
- Spreadsheet ujian `14GiD3iY...AuF0` (master sahkan salinan ujian, selamat). Tab: MENU/AHAD/ISNIN/SELASA/RABU/KHAMIS/RPT/DSKP/DATA. Minggu sekolah **AHAD–KHAMIS**.
- Baca sheet tanpa API: **gviz `out:html`** via browser (master log masuk Chrome). `out:csv` mencetuskan muat turun — jangan guna.
- Struktur RPT: blok tiap **148 baris** (RPT1 baris 8, RPT2 baris 156); D8=subjek; **Minggu N = baris 9+N** (M24 = baris 33). DSKP = pangkalan data penuh semua subjek.
- **🔴 PENEMUAN UBAH KEUTAMAAN: RPT master KOSONG.** Jana RPH bukan masalah sebenar — **isi RPT** yang kerja sebenar. Kalau aku ikut je permintaan asal, master dapat M24 dan minggu depan berdiri di tempat sama.
- **🔴 TAPAK PIHAK KETIGA** (`@zairi_erphadmin`) ada amaran "TAPAK AKAN ROSAK" — reka bentuk mesti hanya tulis ke sel yang Import sedia ada dah tulis.
- Keputusan master: agihan ikut **RPT rasmi PPD/panitia** (menyalin, bukan mereka — Lucy tak buat keputusan pedagogi); mula **1 subjek perintis** (Lucy cadang SAINS 5).
- Reka dicadang (belum lulus): Lucy jana blok **TSV** 2 kolum × ~40 baris → master Ctrl+V pada `D10`. Sifar kod baharu, sifar risiko tapak.

**⏭️ NEXT SESI:** master bekalkan **dokumen RPT rasmi** (path/URL). Kalau PDF → WAJIB tukar `.md` dulu ([[feedback_pdf_ke_md]], peraturan tetap baru master; `pdftotext -layout` dah dipasang, `-layout` wajib untuk jadual). Lepas tukar: silang-semak baris rawak vs asal, baru bina TSV.

### 🆕 Sesi 2026-07-18/19 (malam ~17:37–00:15): sistem-olahraga — THROTTLE BRUTE-FORCE `/api/login` ✅ LIVE PRODUCTION (main `5468d85`)
**Tutup gap security paling tinggi (rate limiting login yang di-flag berkali). Kata pipeline PENUH: brainstorm→spec→plan→subagent-driven→final review opus→verify→deploy. main==test==`5468d85` (+doc `f9896b1` test).**

**MASALAH:** `/api/login` cuma ada `setTimeout(1000)` — tiada had cubaan → brute-force terbuka. Multi-tenant, satu endpoint semua sekolah, username unik global. Infra: **hanya D1** (tiada KV/DO).

**KEPUTUSAN REKA (master via AskUserQuestion):** (1) throttle sementara keyed **IP+username**, auto-pulih, tiada lockout kekal; (2) storan **D1** (strongly-consistent, KISS — bukan KV eventually-consistent/raceable, bukan DO over-eng); (3) ambang Sederhana user 5/15min; (4) buang setTimeout sepenuhnya; (5) skop sistem-olahraga sahaja. **Final review naikkan ip 20→50** (master faham+setuju — elak kunci SELURUH sekolah di belakang satu NAT masa kejohanan; username=5 kekal pertahanan utama). Timing side-channel enumeration → **tangguh backlog** (terima sedar).

**IMPLEMENTASI (7 commit `d1cd734..5468d85`):** jadual D1 `login_attempts` (fixed-window upsert `ON CONFLICT`), block-check SEBELUM verify (fail-closed), fail-open pada ralat D1, reset `user:` bila kredential SAH (bukan `ip:`), increment kedua-dua bila salah, sweep oportunistik 5% via `waitUntil`, helper `resetKaunterLogin` (DRY), fix bonus 500→400.

**SUBAGENT-DRIVEN:** Task 1-4 implementer (haiku/sonnet) dalam sandbox (tiada network); controller (Lucy) handle verify network + deploy. Task 3 (teras) reviewer subagent: spec ✅ + tangkap flake ujian (nba3003 dikongsi vs fullyParallel) → fix. Final whole-branch review **opus: merge-ready, 0 Critical**.

**VERIFY (izin sandbox master):** fail-first (kod lama 500/403≠400) → migration olahraga-test → deploy test → **Playwright 5/5 PASS** (edge+D1 sebenar, reject 429 + accept 200+reset berpasangan). LIVE prod: body{}→400, 5 gagal→401×5, ke-6→**429** "Terlalu banyak cubaan... 15 minit".

**🔴 GOTCHA UJIAN:** Cloudflare edge TOLAK header `CF-Connecting-IP` palsu (403 "error code:1000") — spec ditulis semula (buang suntik IP, username unik, fail-closed test LAST, reset table). `wrangler dev --remote` juga edge (sama isu).
**🔴 GOTCHA DEPLOY:** push main trigger Actions TAPI deploy manual serentak = RACE `assets-upload-session` (error 10013). **Pengajaran: push main SAHAJA, tunggu Actions (~7min lag); manual hanya jika Actions confirm gagal — JANGAN serentak.** (Master tegur: "bukan merge trigger deploy ke" — betul, aku patut biar Actions.)

**⏭️ BACKLOG SECURITY (CLAUDE.md projek, terima sedar):** (a) timing side-channel enumeration username; (b) sustained per-username lockout DoS.

--- (rekod sesi lepas) ---
### 🆕 Sesi 2026-07-18 (pagi ~11:31–11:50): Reference baru — Palet Warna UI + Security Checklist WAJIB ✅ SIAP
**Kerja atas auto-memory global (`.claude/projects/.../memory/`) + CLAUDE.md. Bukan projek kod. Tiada git push (dir memory ni tak bertrack).**

**1. PALET WARNA UI — `reference_palet_warna_ui.md`:** master share 8 gambar "7 Color Combinations That Always Work" (@designbyawais4). Aku extract 7 palet + hex penuh + **petakan ke class Tailwind** (nilai tambah — majoriti hex = default Tailwind: `slate-900`/`blue-500`/`emerald-800`/`violet-700` dll → terus guna class, tiada hardcode). Warna cream/ivory/sand/coral = custom (tiada padanan Tailwind tepat). Calon guna: **celiksains** (belum kod). Pointer masuk MEMORY.md.

**2. SECURITY CHECKLIST WAJIB — `reference_security_checklist.md`:** master minta panduan tetap "**project kita mesti kalis**" 7 ancaman. Aku petakan ke stack serverless kita ikut risiko sebenar:
- 🔴 **Tier 1 hidup:** SQL Injection (`.bind()` wajib), XSS (escHtml/textContent+CSP), Brute Force.
- 🟡 **Tier 2 ikut kes:** CSRF (JWT header Bearer = auto kalis; cookie = SameSite+token), File Upload (validate jenis/saiz + CSV formula injection escape).
- 🟢 **Tier 3 kalis semula jadi Workers:** Command Injection (tiada shell/exec — jaga jangan `eval` input) & File Inclusion (tiada filesystem — jaga R2 key whitelist).
- Ada corak kod boleh salin + **Gerbang Pra-Deploy** (checklist tick) + link ke [[feedback_wrangler_secrets]]/[[feedback_gitignore_patterns]]/[[feedback_pk_komposit_join]]. Pointer masuk MEMORY.md.

**3. KUAT KUASA — CLAUDE.md:** tambah seksyen `## Security` (izin master) → semak `reference_security_checklist.md` setiap projek baru + sebelum deploy; senarai 7 ancaman. Sebab CLAUDE.md auto-baca tiap sesi → jadi arahan WAJIB, bukan nota pasif.

**⚠️ ITEM TERBUKA (aku flag 2× — jujur):** checklist tetapkan STANDARD, tapi **sistem-olahraga `/api/login` MASIH `setTimeout 1s` sahaja, belum kalis brute-force**. → ✅ **DITUTUP 2026-07-19** (throttle D1 LIVE production, lihat sesi malam di atas). Pilihan: D1 counter (bukan WAF/KV/DO), keyed IP+username.

--- (rekod sesi lepas) ---
### 🆕 Sesi 2026-07-18 (pagi ~10:20–11:27): Sistem Memori & Skill Lucy — Compaction + image-prompt ✅ SIAP, PUSHED
**Kerja atas repo memori Lucy sendiri (bukan projek kod). 2 commit pushed ke `origin` (`5d618f0..b692510`).**

**1. MEMORY COMPACTION — transisi ke Option A (hibrid A+snapshot) — commit `ddeaa87`:**
- Masalah: `current-session.md` bengkak 159,524 aksara / 1022 baris / ~40 rekod = **399% atas bajet 40k** → brief pagi kena potong (read limit ~25k token).
- Trade-off A vs B dibincang. Pilih **Option A (RAM, selari Kiyoraka)** + **pinjam snapshot dari B** untuk tampal kelemahan A (padam-buta tiada undo). current-session.md kekal DALAM compaction (satu mekanisme — KISS) tapi dilayan RAM: ilmu dialir KELUAR ke fail kekal, blok `## Compacted History` nipis (DRY — pengajaran sudah hidup di auto-memory).
- Hasil: **159k → ~17.6k aksara (−89%)**, 4 sesi terkini verbatim + blok sejarah. Snapshot restore-able `compaction/snapshots/current-session-2026-07-18.md` (**gitignored** — git dah versionkan fail tracked; retention max 5).
- 🔴 **PENGAJARAN BESAR (master tangkap):** aku declare "PASS" atas **ANGKA** (saiz/secret) sebelum semak **KANDUNGAN**. Bila master suruh *"kau semak ok tak"*, perbandingan lawan snapshot dedah aku **terlepas projek `idme-pajsk-ext`** (spec+plan SIAP, tiada dalam auto-memory — hampir terkambus). Sama silap dgn [[feedback_verify_cetak_visual]]: *verify angka lulus sambil kandungan tercicir*. → dibetulkan + cipta auto-memory [[project_idme_pajsk_ext]].

**2. BACKLOG SKILL — commit `b692510`:**
- Semak `Feature/` vs skill dipasang. **Pasang `image-prompt`** (user-scope, dalam plugin `lucy-skills` global) — jana **teks prompt** Midjourney/Niji sahaja, TIADA API/kos/risiko.
- **Skip `Observation-System`** + FLAG kekal: pipeline `sight-*` Lucy **setara & LEBIH BAIK** (`sight-elemental` ditala D1/Hono/CF vs `.NET` generik upstream; ada escalation eagle→hone→elemental; + `sight-aksara`/`safi`/`convergence` yang upstream tiada). Pasang = gandakan + downgrade (langgar DRY). Flag dalam [[project_lucy_skills]] + MEMORY.md.
- Tangguh (API berbayar + risiko security): Image-Generation, Video-Generation. Skip hiburan: Interactive-Story, Song-Creation.
- Skills **29 → 30**.

**⏭️ TERTUNGGAK (tak mendesak):** `main/session-format.md` protokol 500-baris lama masih WUJUD tapi **DINETRALKAN** (compaction Rule #7 + policy menggantikannya). Fail bertanda "JANGAN edit" — tunggu izin master kalau nak kemas teks lama itu.

--- (rekod sesi lepas) ---
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
- ~~**Rate limiting `/api/login` (sistem-olahraga)**~~ ✅ **DITUTUP 2026-07-19** — throttle D1 `login_attempts` keyed IP+username LIVE production (main `5468d85`). Fix 500→400 sekali. Lihat rekod sesi malam 2026-07-18/19 di atas.
- Backlog projek (Lompat Tinggi Fasa 2 kad responsif, backend DELETE-sebelum-validate C2, guard scan `src/**/*.js` sebelum Code-DRY-extraction, normalisasi markah_penuh, ETR boleh-laras, eksport Excel) **tercatat dalam CLAUDE.md projek masing-masing** — takkan hilang.
- Cadangan audit kecil Lucy (belum master putus): `created_at` timezone di tempat lain; teks/border pucat page cetak lain (slip ujian, pajsk, RPM). **Kekal dalam snapshot.**

*Kredensial ujian yang berulang dalam rekod lama (test/demo DB sahaja) sengaja TIDAK dibawa masuk sini. Tiada `JWT_SECRET`/token API dalam ringkasan ini.*
