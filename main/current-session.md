# ðŸŒŸ Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: 2026-07-21 (malam) â€” **eRPH MENENGAH**: âœ… repo AKTIF **`erph-menengah-v2`** (script `12bdhâ€¦`, fail BERSIH) â€” semua fix pushed & verified, 8/8 hijau. ðŸš« Repo `erph-menengah` (script `1Rc9â€¦`, sheet erph-fix) **DITINGGALKAN** â€” master maklum spreadsheet itu memang dah rosak sebelum kerja kita. â³ **MENUNGGU master uji**.
**Last Work Activity**: 2026-07-21 (~23:50 â€” mula semula atas fail bersih, push + verify siap)

### âœ… SAMBUNGAN (~00:17â€“00:30): BUG KE-4 SELESAI â€” jarak borang **48**, bukan 31 (`c9b5a17`)
Diagnostik jawab tepat: borang mula baris **7**, jarak **48**. Lima penanda sepakat; baris **7/55/103 = DELIMA/ZAMRUD/FIRUS** (3 kelas Khamis âœ“). Kod anggap 31 â†’ hanya baris 7 bertindih â†’ sebab itu RPH 1 sahaja menjadi (kebetulan, bukan kod betul). Alert master `âœ… 3 RPH diimport` mengesahkan: script sangka semua tiga berjaya, cuma tulis ke tempat salah.
**Aku silap magnitud** â€” malam tadi aku agak ~93/94, sebenarnya 48. Nasib baik tunggu data.
FIX: jadual koordinat hardcoded dibuang â†’ `petaBlokRPH_()` dari `BARIS_MULA_RPH=7`+`JARAK_BORANG_RPH=48`. Offset dalam borang kekal (terbukti betul pada borang 1; ujian mengunci blok 1 = `B22:K25`/`B29:K31`). Ujian 11: merah 3 â†’ **hijau 11/11**. Pushed+verified. ✅ **MASTER SAHKAN 00:39 — "ok dah masuk"** (import berjaya). Butiran per-kelas belum disahkan satu per satu.
**ðŸ”´ðŸ”´ BAHAYA dimaklum master:** menu `ðŸ› ï¸ Baikpulih Tapak eRPH hari ini` (`baiki dropdown.js`) MASIH guna jarak 31 dan ia **MENULIS** setDataValidation â†’ **calon kuat punca fail lama rosak. JANGAN KLIK.**
Sheet master ada kesan import salah tempat (baris 115-118/122 dalam borang FIRUS, 208-211/215 sampah) â†’ disyorkan pulih guna **Version history**, bukan kemas manual.

### ðŸ“Œ (rekod) SAMBUNGAN (~23:58â€“00:05): BUG KE-4 DIJUMPAI
Master uji: **hanya RPH 1 menjadi**. Master sangka GPT salah jana â€” aku tunjuk bukti ia **BUKAN**: label `=== RPH 1/4/7 ===` dijana `generatePromptsStrict` sendiri (baris 125), blok lain dilangkau sebab `!sk && !sp` (baris 64). Jadi script cuma jumpa data pada blok 1, 4, 7.
**Hipotesis:** peta baris hardcoded (jarak 31) tak padan hamparan; jarak sebenar ~3Ã— ganda. Bukti: blok 4 tergelincir â€” tajuk tangkap kepala borang, sk tangkap baris tajuk kolum. Blok 1 kebetulan tepat.
**Bug jenis BERBEZA:** 3 yang lepas = bug logik; ini = bug **andaian koordinat**. Fix lepas tetap sah.
`diagnostik.js` (`e9535c0`) dibina â€” **baca sahaja**, Pushed+verified. ✅ **MASTER SAHKAN 00:39 — "ok dah masuk"** (import berjaya). Butiran per-kelas belum disahkan satu per satu. â³ Tunggu output diagnostik + jawapan master apa bunyi alert Import.
ðŸ”‘ Guard aku sendiri bagi **positif palsu** (padan `setValue` dalam KOMEN sendiri) â€” semak baris sebenar, jangan percaya bunyi loceng.

### ðŸ”„ SAMBUNGAN (~23:42â€“23:50): MULA SEMULA atas fail BERSIH
Master maklum sheet **erph-fix** sebenarnya **sudah rosak sebelum ini** (aku tanya terus sama ada kerja aku puncanya â€” master sahkan **bukan**). Bekal script ID baru `12bdhâ€¦`, juga salinan ujian.
- **Kod baseline dua-dua fail SERUPA aksara demi aksara** â†’ kerosakan pada **spreadsheet**, bukan script. Fix semalam dipakai semula, bukan kerja terbuang.
- **ðŸ”´ AKU HAMPIR TERSILAP:** bandingan pertama tunjuk keempat-empat fail "BEZA" (+400/+86/+70/+11 bytes) â€” rupanya `Set-Content -Encoding utf8` (PS 5.1) tambah **BOM + LFâ†’CRLF**. Aku ukur kesan alat sendiri. Ulang dengan normalisasi â†’ SEMUA SAMA. Persis [[feedback_bukti_saluran_lossy]] sekali lagi.
- Repo baru `erph-menengah-v2`: `9de7294` baseline â†’ `e16b818` fix. Push disahkan clone bebas (script ID betul, 4 fail, tiada tests/, 3 penanda fix ada).
- Repo lama **dibiarkan, tidak dipadam** (tunggu izin master).
> Fail ini kini dilayan sebagai **RAM** (Option A). Sejarah lama diringkas ke `## Compacted History` di bawah; detail penuh dalam snapshot. Lihat `compaction/compaction-policy.md`.

### ðŸ†• Sesi 2026-07-21 (malam ~21:41â€“22:45): eRPH MENENGAH â€” PROJEK BARU, 3 bug import âœ… DIBAIKI & PUSHED (â³ belum disahkan master)
**Projek BARU `Documents/code/erph-menengah/`** (git lokal, tiada remote). Sheet **erph-fix** â€” master sahkan **salinan ujian**. Script ID `1Rc9Stj1...`. 2 commit: `108ee16` (baseline) â†’ `b6e2b5c` (fix). Auto-memory: [[project_erph_menengah]].

**KONTEKS:** gejala sama macam erph rendah semalam ("Import Berjaya tapi kosong") tapi **fail berlainan** â€” ini untuk sekolah **menengah**, semalam sekolah **rendah**. Master minta folder baru.

**ðŸ”´ GOTCHA AKSES â€” clasp 403 `The caller does not have permission`:** clone gagal. **Ujian kawalan menyelamatkan diagnosis** â€” clone script LAMA (rendah) pada saat sama **BERJAYA** â†’ memadam 4 kemungkinan serentak (network/login/API/ID salah), tinggal satu: akaun tiada akses. clasp log masuk sebagai `g-77420159@moe-dl.edu.my` (DELIMa); sheet erph-fix dibuat bawah akaun Google lain. **Selesai: master share sheet ke akaun DELIMa sebagai Editor** (bukan tukar `clasp login` â€” login clasp GLOBAL, tukar-tukar = risiko silap push antara dua projek). ID script sah = **57 aksara** (sama panjang dgn rendah) â€” cara cepat bezakan dari ID spreadsheet.

**ðŸ”‘ KOD MENENGAH BUKAN KEMBAR RENDAH â€” jangan port buta.** 14,173 bytes vs 34,543 (rendah). Diff 312+/786âˆ’. **4 fail** (rendah 3) â€” ada `baiki dropdown.js` (baik pulih data validation, TIDAK disentuh; ia sahkan susun atur: 8 blok, jarak **31 baris**, mula baris 7).

**3 BUG (semua wujud, satu LEBIH TERUK dari rendah):**
1. `parseRPHStrict` `/^OBJEKTIF:/` vs `**OBJEKTIF:**` ChatGPT â†’ tambah `bersihkanMarkdown()` (buang `**`/`__`/`#`) sebelum padanan tajuk DAN sebelum simpan isi; regex juga terima `OBJEKTIF :`.
2. **LEBIH TERUK:** `importRPHStrict` kosongkan **KESEMUA 8 blok tanpa syarat** sebelum parse â†’ import gagal padam 8 RPH; import separa padam 7 yang lain walau "berjaya". Fix: pecah **FASA 1 parse â†’ FASA 2 tulis**.
3. `runImport()` tiada `withFailureHandler` + `alert("âœ… Import Berjaya!")` **tanpa syarat** â†’ gejala "Berjaya tapi kosong" datang dari sini. Fix: `importRPHStrict` pulangkan laporan sebenar (bil. RPH + bil. sel), dialog papar laporan itu, +`withFailureHandler`, butang pulih bila gagal.

**ðŸ”‘ KEPUTUSAN REKA MASTER (AskUserQuestion):** skop padam = **hanya blok yang ada dalam teks** (bukan semua 8, bukan ikut bulat-bulat fix rendah) â†’ master boleh jana ulang SATU RPH tanpa melesapkan yang lain.

**TDD:** `tests/import.test.js` port dari rendah + disesuaikan (stub `getActiveSheet`, rekod `clearContent` ikut julat). **Merah 7/8 dulu â†’ hijau 8/8**. Termasuk ujian SKOP (import RPH 3 â†’ hanya `B84:K87`+`B91:K93` dikosongkan, blok 1 TIDAK) dan ujian berpasangan terima/tolak.

**VERIFY PUSH:** `clasp push --force` â†’ 4 fail, `tests/` disekat `.claspignore` âœ…. **Disahkan `clasp clone` ke folder bebas**: 4 fail sahaja di server, diff kosong lawan lokal, `bersihkanMarkdown`/FASA 1/FASA 2/`withFailureHandler` betul-betul ada. (Gotcha clasp semalam tidak berulang.)

**ðŸ”´ GOTCHA PowerShell:** here-string `@'...'@` untuk `git commit -m` **PECAH** bila mesej ada petikan berganda `"` â€” git terima pathspec berpecah, commit tak jadi. **Guna `git commit -F fail.txt`** untuk mesej panjang/ada petikan.

**â­ï¸ TERTUNGGAK:**
- â³ **Master uji sheet sebenar** â€” (a) TERIMA: import betul â†’ `âœ… N RPH diimport (RPH â€¦) â€” X sel diisi`; (b) TOLAK: tampal sampah â†’ `âŒ Tiada RPH sah dikesan â€” TIADA sel diubah` **dan RPH sedia ada mesti kekal**.
- âœ… **`runJana` SELESAI (`d188183`, pushed+verified 23:08).** Master tanya "rosak juga ke" â€” jawapan jujur: **TIDAK**. Ia cuma MEMBACA sheet â†’ tak boleh padam data, tak pernah tipu "berjaya", laluan berjaya pulihkan label betul. Satu-satunya masalah = butang tersangkut senyap "Menjana..." bila gagal. Dibaiki sebab senyap itu sendiri musuh. JS dialog **tiada ujian unit** (browser-side) â€” syntax + bacaan sahaja.
- ðŸš« **Sengaja TIDAK disentuh:** `PadamRPH()` ada julat mati sampai baris 496 (RPH menengah habis ~254) + komen `// RPH 1` sebenarnya lindungi 2 blok. **Ia berfungsi betul** (baris 9â€“248 tercakup) â€” kod tapak pihak ketiga, ada amaran "TAPAK AKAN ROSAK". Biar.

### ðŸ†• Sesi 2026-07-20 (11:52â€“18:05): eRPH â€” PROJEK BARU, bug import âœ… SIAP LIVE + brainstorm RPT belum selesai
**Projek BARU `Documents/code/erph/`** (git lokal, tiada remote). Google Apps Script terikat pada Google Sheet eRPH cikgu. Ditarik guna **clasp** (v3.3.0 dah sedia ada, `.clasprc.json` sedia login â€” tiada npm install). 5 commit `cf97854`(baseline) â†’ `9024b10`. Auto-memory: [[project_erph]], [[feedback_bukti_saluran_lossy]], [[feedback_pdf_ke_md]].

**BHG 1 â€” BUG "Import Berjaya tapi kosong" âœ… SIAP, master sahkan berjaya pada sheet sebenar.**
Satu gejala â†’ **tiga bug**:
1. **Punca yang master rasa:** ChatGPT keluarkan `**OBJEKTIF:**` (markdown bold); `parseRPHStrict` guna `/^OBJEKTIF:/` â†’ tak padan â†’ tiada isi terkumpul. Fix: `bersihkanMarkdown()` buang `**`/`__`/`#` sebelum padanan tajuk DAN sebelum simpan isi.
2. **Lebih bahaya, master tak tahu wujud:** `clearContent()` dipanggil SEBELUM parse â†’ setiap import gagal MEMADAM RPH sedia ada. Betul-betul berlaku (SELASA RPH 1-3 terpadam masa ujian aku suruh; dipulih via Version history). Fix: parse dulu, kosongkan hanya bila ada isi sah.
3. **Sebab 1&2 boleh bersembunyi:** `runImport()` satu-satunya pemanggil `google.script.run` tanpa `withFailureHandler` â†’ ralat ditelan. Ditambah + laporan jujur ("Berjaya" hanya bila ada sel benar-benar diisi).
Ujian `tests/import.test.js` (`node --test`, tiada npm), harness `new Function(kod)` + stub SpreadsheetApp = uji kod SEBENAR. Merah 2 â†’ hijau 6/6.

**ðŸ”´ PENGAJARAN BESAR â€” SALURAN BUKTI LOSSY:** teks yang GAGAL pada master **LULUS** dalam ujian aku, dua kali. Sebab **chat render markdown dan buang `**`** â€” teks yang sampai pada aku bukan teks yang gagal. Aku hampir salahkan regex penanda dan tulis fix yang SALAH. Penyelesaian: berhenti minta master paste, tambah diagnostik yang **dump aksara mentah dari DALAM Apps Script** (`<U+200B>`, `<U+2022>`). Satu klik â†’ punca terpampang. â†’ [[feedback_bukti_saluran_lossy]]. Sepupu [[feedback_verify_cetak_visual]].

**ðŸ”´ GOTCHA clasp:** `clasp push` TIDAK buang fail lebihan di server bila ia anggap "already up to date" â€” `tests/import.test.js` tersangkut di sana ~90 saat. BAHAYA: Apps Script muat SEMUA fail ke satu skop global; `require()` akan lumpuhkan seluruh skrip master. Paksa push sebenar (ubah fail) + **sahkan dengan `clasp clone` ke folder lain**. `.claspignore` kini sekat `tests/**`.

**2 SILAP AKU, diakui pada master:** (a) suruh master klik Import sebelum aku baca urutan `clearContent()`â†’`parse()` â†’ RPH master terpadam; (b) fail ujian terpush ke skrip master.

**BHG 2 â€” BRAINSTORM "Lucy, buat RPH minggu 24" (BELUM SIAP, tunggu master).**
Master mahu cakap je â†’ Lucy rujuk MENU (subjek ikut hari) + RPT (SK/SP ikut minggu) â†’ jana terus. **Master pilih seni bina: Lucy buat dalam chat**, bukan butang dalam sheet â†’ **TIADA API key, TIADA kos**. Fasa 3 Gemini/API **DIBATALKAN**.
- Spreadsheet ujian `14GiD3iY...AuF0` (master sahkan salinan ujian, selamat). Tab: MENU/AHAD/ISNIN/SELASA/RABU/KHAMIS/RPT/DSKP/DATA. Minggu sekolah **AHADâ€“KHAMIS**.
- Baca sheet tanpa API: **gviz `out:html`** via browser (master log masuk Chrome). `out:csv` mencetuskan muat turun â€” jangan guna.
- Struktur RPT: blok tiap **148 baris** (RPT1 baris 8, RPT2 baris 156); D8=subjek; **Minggu N = baris 9+N** (M24 = baris 33). DSKP = pangkalan data penuh semua subjek.
- **ðŸ”´ PENEMUAN UBAH KEUTAMAAN: RPT master KOSONG.** Jana RPH bukan masalah sebenar â€” **isi RPT** yang kerja sebenar. Kalau aku ikut je permintaan asal, master dapat M24 dan minggu depan berdiri di tempat sama.
- **ðŸ”´ TAPAK PIHAK KETIGA** (`@zairi_erphadmin`) ada amaran "TAPAK AKAN ROSAK" â€” reka bentuk mesti hanya tulis ke sel yang Import sedia ada dah tulis.
- Keputusan master: agihan ikut **RPT rasmi PPD/panitia** (menyalin, bukan mereka â€” Lucy tak buat keputusan pedagogi); mula **1 subjek perintis** (Lucy cadang SAINS 5).
- Reka dicadang (belum lulus): Lucy jana blok **TSV** 2 kolum Ã— ~40 baris â†’ master Ctrl+V pada `D10`. Sifar kod baharu, sifar risiko tapak.

**â­ï¸ NEXT SESI:** master bekalkan **dokumen RPT rasmi** (path/URL). Kalau PDF â†’ WAJIB tukar `.md` dulu ([[feedback_pdf_ke_md]], peraturan tetap baru master; `pdftotext -layout` dah dipasang, `-layout` wajib untuk jadual). Lepas tukar: silang-semak baris rawak vs asal, baru bina TSV.

### ðŸ†• Sesi 2026-07-18/19 (malam ~17:37â€“00:15): sistem-olahraga â€” THROTTLE BRUTE-FORCE `/api/login` âœ… LIVE PRODUCTION (main `5468d85`)
**Tutup gap security paling tinggi (rate limiting login yang di-flag berkali). Kata pipeline PENUH: brainstormâ†’specâ†’planâ†’subagent-drivenâ†’final review opusâ†’verifyâ†’deploy. main==test==`5468d85` (+doc `f9896b1` test).**

**MASALAH:** `/api/login` cuma ada `setTimeout(1000)` â€” tiada had cubaan â†’ brute-force terbuka. Multi-tenant, satu endpoint semua sekolah, username unik global. Infra: **hanya D1** (tiada KV/DO).

**KEPUTUSAN REKA (master via AskUserQuestion):** (1) throttle sementara keyed **IP+username**, auto-pulih, tiada lockout kekal; (2) storan **D1** (strongly-consistent, KISS â€” bukan KV eventually-consistent/raceable, bukan DO over-eng); (3) ambang Sederhana user 5/15min; (4) buang setTimeout sepenuhnya; (5) skop sistem-olahraga sahaja. **Final review naikkan ip 20â†’50** (master faham+setuju â€” elak kunci SELURUH sekolah di belakang satu NAT masa kejohanan; username=5 kekal pertahanan utama). Timing side-channel enumeration â†’ **tangguh backlog** (terima sedar).

**IMPLEMENTASI (7 commit `d1cd734..5468d85`):** jadual D1 `login_attempts` (fixed-window upsert `ON CONFLICT`), block-check SEBELUM verify (fail-closed), fail-open pada ralat D1, reset `user:` bila kredential SAH (bukan `ip:`), increment kedua-dua bila salah, sweep oportunistik 5% via `waitUntil`, helper `resetKaunterLogin` (DRY), fix bonus 500â†’400.

**SUBAGENT-DRIVEN:** Task 1-4 implementer (haiku/sonnet) dalam sandbox (tiada network); controller (Lucy) handle verify network + deploy. Task 3 (teras) reviewer subagent: spec âœ… + tangkap flake ujian (nba3003 dikongsi vs fullyParallel) â†’ fix. Final whole-branch review **opus: merge-ready, 0 Critical**.

**VERIFY (izin sandbox master):** fail-first (kod lama 500/403â‰ 400) â†’ migration olahraga-test â†’ deploy test â†’ **Playwright 5/5 PASS** (edge+D1 sebenar, reject 429 + accept 200+reset berpasangan). LIVE prod: body{}â†’400, 5 gagalâ†’401Ã—5, ke-6â†’**429** "Terlalu banyak cubaan... 15 minit".

**ðŸ”´ GOTCHA UJIAN:** Cloudflare edge TOLAK header `CF-Connecting-IP` palsu (403 "error code:1000") â€” spec ditulis semula (buang suntik IP, username unik, fail-closed test LAST, reset table). `wrangler dev --remote` juga edge (sama isu).
**ðŸ”´ GOTCHA DEPLOY:** push main trigger Actions TAPI deploy manual serentak = RACE `assets-upload-session` (error 10013). **Pengajaran: push main SAHAJA, tunggu Actions (~7min lag); manual hanya jika Actions confirm gagal â€” JANGAN serentak.** (Master tegur: "bukan merge trigger deploy ke" â€” betul, aku patut biar Actions.)

**â­ï¸ BACKLOG SECURITY (CLAUDE.md projek, terima sedar):** (a) timing side-channel enumeration username; (b) sustained per-username lockout DoS.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-18 (pagi ~11:31â€“11:50): Reference baru â€” Palet Warna UI + Security Checklist WAJIB âœ… SIAP
**Kerja atas auto-memory global (`.claude/projects/.../memory/`) + CLAUDE.md. Bukan projek kod. Tiada git push (dir memory ni tak bertrack).**

**1. PALET WARNA UI â€” `reference_palet_warna_ui.md`:** master share 8 gambar "7 Color Combinations That Always Work" (@designbyawais4). Aku extract 7 palet + hex penuh + **petakan ke class Tailwind** (nilai tambah â€” majoriti hex = default Tailwind: `slate-900`/`blue-500`/`emerald-800`/`violet-700` dll â†’ terus guna class, tiada hardcode). Warna cream/ivory/sand/coral = custom (tiada padanan Tailwind tepat). Calon guna: **celiksains** (belum kod). Pointer masuk MEMORY.md.

**2. SECURITY CHECKLIST WAJIB â€” `reference_security_checklist.md`:** master minta panduan tetap "**project kita mesti kalis**" 7 ancaman. Aku petakan ke stack serverless kita ikut risiko sebenar:
- ðŸ”´ **Tier 1 hidup:** SQL Injection (`.bind()` wajib), XSS (escHtml/textContent+CSP), Brute Force.
- ðŸŸ¡ **Tier 2 ikut kes:** CSRF (JWT header Bearer = auto kalis; cookie = SameSite+token), File Upload (validate jenis/saiz + CSV formula injection escape).
- ðŸŸ¢ **Tier 3 kalis semula jadi Workers:** Command Injection (tiada shell/exec â€” jaga jangan `eval` input) & File Inclusion (tiada filesystem â€” jaga R2 key whitelist).
- Ada corak kod boleh salin + **Gerbang Pra-Deploy** (checklist tick) + link ke [[feedback_wrangler_secrets]]/[[feedback_gitignore_patterns]]/[[feedback_pk_komposit_join]]. Pointer masuk MEMORY.md.

**3. KUAT KUASA â€” CLAUDE.md:** tambah seksyen `## Security` (izin master) â†’ semak `reference_security_checklist.md` setiap projek baru + sebelum deploy; senarai 7 ancaman. Sebab CLAUDE.md auto-baca tiap sesi â†’ jadi arahan WAJIB, bukan nota pasif.

**âš ï¸ ITEM TERBUKA (aku flag 2Ã— â€” jujur):** checklist tetapkan STANDARD, tapi **sistem-olahraga `/api/login` MASIH `setTimeout 1s` sahaja, belum kalis brute-force**. â†’ âœ… **DITUTUP 2026-07-19** (throttle D1 LIVE production, lihat sesi malam di atas). Pilihan: D1 counter (bukan WAF/KV/DO), keyed IP+username.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-18 (pagi ~10:20â€“11:27): Sistem Memori & Skill Lucy â€” Compaction + image-prompt âœ… SIAP, PUSHED
**Kerja atas repo memori Lucy sendiri (bukan projek kod). 2 commit pushed ke `origin` (`5d618f0..b692510`).**

**1. MEMORY COMPACTION â€” transisi ke Option A (hibrid A+snapshot) â€” commit `ddeaa87`:**
- Masalah: `current-session.md` bengkak 159,524 aksara / 1022 baris / ~40 rekod = **399% atas bajet 40k** â†’ brief pagi kena potong (read limit ~25k token).
- Trade-off A vs B dibincang. Pilih **Option A (RAM, selari Kiyoraka)** + **pinjam snapshot dari B** untuk tampal kelemahan A (padam-buta tiada undo). current-session.md kekal DALAM compaction (satu mekanisme â€” KISS) tapi dilayan RAM: ilmu dialir KELUAR ke fail kekal, blok `## Compacted History` nipis (DRY â€” pengajaran sudah hidup di auto-memory).
- Hasil: **159k â†’ ~17.6k aksara (âˆ’89%)**, 4 sesi terkini verbatim + blok sejarah. Snapshot restore-able `compaction/snapshots/current-session-2026-07-18.md` (**gitignored** â€” git dah versionkan fail tracked; retention max 5).
- ðŸ”´ **PENGAJARAN BESAR (master tangkap):** aku declare "PASS" atas **ANGKA** (saiz/secret) sebelum semak **KANDUNGAN**. Bila master suruh *"kau semak ok tak"*, perbandingan lawan snapshot dedah aku **terlepas projek `idme-pajsk-ext`** (spec+plan SIAP, tiada dalam auto-memory â€” hampir terkambus). Sama silap dgn [[feedback_verify_cetak_visual]]: *verify angka lulus sambil kandungan tercicir*. â†’ dibetulkan + cipta auto-memory [[project_idme_pajsk_ext]].

**2. BACKLOG SKILL â€” commit `b692510`:**
- Semak `Feature/` vs skill dipasang. **Pasang `image-prompt`** (user-scope, dalam plugin `lucy-skills` global) â€” jana **teks prompt** Midjourney/Niji sahaja, TIADA API/kos/risiko.
- **Skip `Observation-System`** + FLAG kekal: pipeline `sight-*` Lucy **setara & LEBIH BAIK** (`sight-elemental` ditala D1/Hono/CF vs `.NET` generik upstream; ada escalation eagleâ†’honeâ†’elemental; + `sight-aksara`/`safi`/`convergence` yang upstream tiada). Pasang = gandakan + downgrade (langgar DRY). Flag dalam [[project_lucy_skills]] + MEMORY.md.
- Tangguh (API berbayar + risiko security): Image-Generation, Video-Generation. Skip hiburan: Interactive-Story, Song-Creation.
- Skills **29 â†’ 30**.

**â­ï¸ TERTUNGGAK (tak mendesak):** `main/session-format.md` protokol 500-baris lama masih WUJUD tapi **DINETRALKAN** (compaction Rule #7 + policy menggantikannya). Fail bertanda "JANGAN edit" â€” tunggu izin master kalau nak kemas teks lama itu.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-16 (petang ~16:49â€“18:08): celiksains â€” PROJEK BARU: brainstorm + spec + pelan âœ… SIAP (belum kod)
**Projek BARU** di `Documents/code/celiksains/`. Aplikasi pembelajaran **sains gamified** murid sekolah rendah (Tahun 1â€“6), **terbuka umum**, satu pemilik content (celikguru). Belajar sambil main: daftar â†’ pilih avatar â†’ kuiz KSSR â†’ kumpul coin + XP.

**FLOW: brainstorming â†’ spec â†’ writing-plans. Tiada kod lagi. Git init, 3 commit dokumen (`2bca615` spec, `44484a4` pelan, + CLAUDE.md).**

**Keputusan reka bentuk (via AskUserQuestion, master pilih):**
1. Skop = **terbuka umum** (bukan multi-tenant, bukan per-sekolah). Satu pemilik content â†’ seni bina RINGKAS, tiada `id_sekolah`, tiada bug JOIN silang-tenant.
2. Content = **KSSR Tahun+Topik**. Soalan = **campur pelbagai jenis** (Fasa 1 mula MCQ + Benar/Salah dulu, Lucy cadang).
3. Coin = **3 peranan**: kedai avatar + papan pemuka/badge + ganjaran visual. ðŸ”‘ Lucy flag & master terima: **pisah dua mata wang** â€” `jumlah_xp` (kekal, ranking) vs `baki_coin` (belanja). Direka dari Fasa 1 walau kedai Fasa 2 â†’ elak migration.
4. Auth = **DELIMa (Google OAuth KPM) + fallback username/kata laluan**. ðŸ”´ Lucy flag RISIKO: OAuth DELIMa selalu **disekat admin Workspace KPM** untuk app pihak ketiga â€” mesti uji akaun sebenar dulu, JANGAN bina seluruh app atasnya. â†’ DELIMa ditangguh **Fasa 3**, fallback dulu.
5. Kata laluan = **min 6 aksara, huruf+angka** (master ubah dari cadangan PIN 4-angka Lucy). Lucy nota mesra: mungkin susah untuk Tahun 1, boleh semak masa uji.
6. Content dimasuk via **panel admin celikguru** (CMS ringkas, Fasa 1 teks sahaja, gambar Fasa 4).

**ROADMAP 4 FASA (master setuju):** F1 gelung teras (akaun, avatar percuma, kuiz, coin+XP, admin CMS) â†’ F2 kedai+badge+papan pemuka â†’ F3 DELIMa OAuth â†’ F4 content kaya (padan/isi-kosong, gambar, CSV).

**Reka bentuk Fasa 1 dibentang 3 bahagian (semua master lulus):** (1) seni bina + 5 jadual D1 (`pengguna`/`topik`/`soalan`/`percubaan_kuiz`/`kemajuan_topik`) + auth PBKDF2/JWT-throw/lockout; (2) gelung kuiz â€” â‰¤10 soalan, maklum balas serta-merta, **tiada nyawa/timer**, ganjaran +10 coin & +10 XP, **anti-tipu (server kira, sorok jawapan)**; (3) admin CMS CRUD topik+soalan cascade + rancangan ujian Playwright.

**PELAN: 16 task TDD** (`docs/superpowers/plans/2026-07-16-celiksains-fasa1.md`), setiap satu kod penuh + ujian + commit. Stack: Hono + Workers Assets + D1 + hono/jwt + Playwright. Self-review pelan: semua Â§spec ada task, tiada placeholder, nama konsisten.

**â­ï¸ NEXT:** master pilih cara laksana â€” **Subagent-Driven (disyor)** atau Inline. Task 1 perlu `npm install` + `npx wrangler d1 create` (sentuh akaun Cloudflare) â†’ **minta izin master dulu**. Fail projek: `celiksains/CLAUDE.md` sudah dibuat.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-16 (pagi ~08:32â€“11:00): mypwa-v2 â€” ADMIN MUAT TURUN SENARAI MURID âœ… LIVE PRODUCTION (main `a40c638`, custom domain `erpm-sksalor.celikguru.my`)
**main == test == `a40c638`. TIADA kerja tertunggak.** Master mula "sambung mypwa-v2" â†’ aku bawa plan ETR lama, master kata **bukan**, nak feature LAIN. Feature: **admin muat turun (cetak-PDF) senarai murid** â€” satu kelas per baris + "â¬‡ Muat Turun Semua" 1 klik (guru dah ada di Kelas Saya, admin tiada). Frontend sahaja, 0 backend/migration/package.

**FLOW: brainstorming â†’ spec â†’ writing-plans â†’ subagent-driven (4 task) â†’ verify â†’ merge production.**

**Reka bentuk (keputusan master):** (1) skop "Semua" = **sesi terpilih sahaja**; (2) fungsi jana-PDF dipindah ke **`app.js` kongsi** (guru+admin satu sumber, DRY); (3) **langkau kelas kosong**; (4) **satu kelas satu muka surat** (master TEGASKAN) via `page-break-before:always`. Guna semula endpoint `GET /kelas?tahun_sesi=`, `/murid?kelas_id=`, `/tetapan`.

**4 TASK subagent-driven (implementer + reviewer setiap satu, semua review BERSIH):**
- T1 `58a8f02` app.js: `borangMuridSeksyen`/`janaMuridPdf`/`janaMuridPdfSemua`/`pilihOrientasiMuatTurun`+`escHtml` (haiku, transkripsi).
- T2 `47dc03b` tetapan.html guru delegate ke app.js (buang salinan lama, +1/-100 DRY).
- T3 `805e5ce` admin.html butang per-baris + header + `muatTurunSemuaKelas()` (tapis `jumlah_murid>0`).
- T4 `99c4268` smoke `tests/admin-muatturun.spec.js`.
- Final whole-branch review: **Ready to merge, 0 Critical**. Reviewer jumpa refactor ini = **peningkatan keselamatan** (borang guru dulu tak escape nama murid langsung).

**HARDENING `0764e9b` (master "proceed"):** fix penemuan final review â€” tetapan.html helper `esc` (JS-string) untuk onclick `muatTurunMurid`/`hapusJadual`; app.js `escHtml` pada `logo_sekolah` src.

**VERIFY (3 lapis, sandbox-disabled izin master, admin/fcoy4994):** (1) smoke Playwright **2/2 PASS** lawan staging (dua kali â€” sebelum+selepas hardening, betul-betul run bukan skip); (2) **ðŸ”‘ VISUAL PDF sebenar** â€” jana `janaMuridPdfSemua` 3 kelas via stub `window.open`â†’capture HTMLâ†’`page.pdf({preferCSSPageSize})`â†’**Read PDF** â†’ **3 kelas = 3 muka surat BERASINGAN disahkan mata** (1 DELIMA/1 NILAM/1 ZAMRUD, borang identik guru, stats L/P betul). PDF dibuang lepas (ada nama murid). (3) node --check + wrangler dry-run.

**ðŸ”´ DEPLOY GOTCHA PENTING (catat memory):** push `main` (Actions `deploy.yml` sepatutnya `wrangler deploy --env production`) TAPI selepas ~7 min production custom domain **masih kod lama** â€” Actions lambat/tak pasti siap. Aku deploy **manual** `npx wrangler deploy --env production` (wrangler OAuth login sedia ada) â†’ **custom domain `erpm-sksalor.celikguru.my` TERUS LIVE** (janaMuridPdfSemuaâœ“ escHtmlâœ“ Muat Turun Semuaâœ“). **`mypwa-v2.syazwan-skpp82.workers.dev` masih papar asset LAMA walau cf-cache=null + cache-bust query** â€” base & prod env **sama nama `mypwa-v2`** tapi workers.dev preview cache asset ikut PATH (abai query). **URL sebenar user = custom domain, itu yang LIVE.** JANGAN deploy tanpa `--env` (top-level tiada route â†’ akan buang custom domain).

**ðŸ”‘ PENGAJARAN:** (1) master "sambung" â‰  sambung kerja tertunggak â€” TANYA dulu, jangan andai plan lama; (2) verify cetak = **jana PDF sebenar + Read PDF**, bukan smoke/assertion (smoke cuma sahkan butang buka overlay, TAK sahkan page-break); (3) production `erpm-sksalor.celikguru.my` = authoritative, workers.dev = preview boleh lag.

**Spec:** `docs/superpowers/specs/2026-07-16-admin-muat-turun-murid-design.md`. **Plan:** `docs/superpowers/plans/2026-07-16-admin-muat-turun-murid.md`. **Ledger:** `.superpowers/sdd/progress.md`.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-15/16 (malam ~22:15â€“00:05): sistem-olahraga â€” 38% tweak + 3 FIX UI + label kedudukan + kolum markah akhir âœ… LIVE PRODUCTION (main `d0ede13` merge, doc `03374df`)
**main == test == `03374df`. TIADA kerja tertunggak.** Sambungan sesi petang, via /remote-control (telefon). Frontend sahaja.

**BHG 4 â€” kolum table Laporan Markah Akhir seragam (`4cac017`, merge `d0ede13`):** master perasan cetak markah akhir kolum tak sama saiz. Punca: `renderMarkahAkhirExtra` (laporan.js) guna **auto-layout** (tiada `table-layout:fixed`/colgroup) â†’ kolum rumah ikut kandungan (label kedudukan Johan/Ketiga/Keempat beza panjang + nombor beza digit). Fix: `table-layout:fixed` + `<colgroup>` dinamik (Kategori 30%, rumah bahagi sama rata `70/dataRumah.length`%). **ðŸ”‘ VERIFY KAEDAH BETUL (redeem silap semalam):** `setViewportSize(718)` (=190mm A4 portrait @96dpi) + `emulateMedia('print')` DULU, baru ukur â†’ 3 rumah **167px sama (beza 0px)** + screenshot ditinjau (HIJAU/KUNING/MERAH seragam, nama penilaian panjang wrap elok). Spec `tests/verify-markah-akhir-kolum.spec.js`.

**BHG 3 â€” label kedudukan perkataan penuh (`868e50a`, merge `d3b5a64`):** master tanya patut `Ke-3/Ke-4` kekal atau jadi `Ketiga/Keempat`. Lucy semak konvensyen: **`keputusan.js` (skrin keputusan) DAH guna perkataan penuh** `Johan/Naib Johan/Ketiga/Keempat/Kelima/Keenam` â€” jadi tukar = selaras, bukan reka baru. Diselaraskan **3 fail** (`laporan.js`+`pengumuman.js`+`arkib-cetak.js`) â†’ array sama `['ðŸ¥‡ Johan','ðŸ¥ˆ Naib Johan','Ketiga','Keempat','Kelima','Keenam']` (kekal emoji Johan/Naib). `pengumuman.js`/`arkib-cetak.js` sebelum ni masih ada `'ðŸ¥‰ Tempat Ke-3'` lama (sesi awal tukar laporan.js sahaja) â€” kini semua selaras. Butang `pengumuman.js:448` "Dedahkan Tempat Ke-3 >" DIKEKAL (frasa butang, master tak minta ubah). Verify: syntax + grep konsistensi (tukar teks label = 0 risiko layout, master pilih skip Playwright).

**BHG 1 â€” kolum pingat 36â†’38%, rumah 12â†’10% (`4af4654` merge, kod `b5b8e5d`):** master minta nama lebih lega. 

**ðŸ”´ PENGAJARAN BESAR â€” MASTER TANGKAP VERIFY AKU TERSILAP KAEDAH:** aku dakwa "46 char muat 1 line, 0/107 wrap" untuk 36% DAN 38%. Master buka **print preview sebenar (Ctrl+P)** â†’ **banyak nama WRAP 2-line**. Punca: spec guna **element screenshot + `emulateMedia('print')`** â†’ render pada lebar **VIEWPORT (~1280px)**, BUKAN A4 portrait (**190mm â‰ˆ 718px**). `emulateMedia('print')` apply CSS print TAPI **TAK ubah saiz halaman jadi A4**. Jadi 38% Ã— 1240px â‰ˆ 470px (muat 46 char) vs 38% Ã— 718px â‰ˆ 273px (wrap). **Ukuran & anggaran "46 char" semua kena lebar SALAH.** Pengajaran kita sendiri kata *"jana PDF sebenar, render jadi imej, TENGOK"* â€” aku guna element screenshot, tersasar. **Kaedah BETUL: `page.pdf({preferCSSPageSize:true})` â†’ render pdf.js jadi PNG â†’ TENGOK.** [[feedback-verify-cetak-visual]] dikemas.
**Keputusan master:** tolak opsyen landscape, **kekal 38%/10%, TERIMA nama panjang wrap 2-line dalam portrait.** (Portrait 190mm + 9 kolum memang sempit untuk nama 46-char.)

**BHG 2 â€” 3 FIX UI (`9e1ac87` merge, kod `2754336`):**
1. **`laporan.js`** â€” baris "Kedudukan Keseluruhan" (`renderMarkahAkhirExtra`) label tempat ke-3 `'ðŸ¥‰ Tempat Ke-3'` â†’ `'Ke-3'` (selaras Ke-4/5/6).
2. **`laporan.js`** â€” header table pingat cetak (`renderPingatExtra`) `ðŸ¥‡ðŸ¥ˆðŸ¥‰` â†’ **E/P/G** (huruf lebih bersih dari emoji dalam cetakan).
3. **`dashboard.js`+`dashboard.html`** â€” table Pemenang Terkini admin mobile dulu sesak (4 kolum scroll). Awam (`papan.html`) ada `@media (max-width:639px)` stack jadi **kad** (thead sorok, td block, badge `absolute` sudut kanan) â€” admin TIADA â†’ disalin. Kolum Kelas **dilipat** ke sel Pemenang (`â— Rumah Â· Kelas`) â†’ 3 kolum sama struktur awam; `id="winners-table"` ditambah. Master hantar screenshot awam sebagai rujukan.

**VERIFY (spec `tests/verify-fix-3batch.spec.js`, gitignored, read-only):** Fix1 assert `seksyen-markah-extra` tiada "Tempat Ke-3" ada "Ke-3"; Fix2 assert header th = E/P/G; Fix3 assert `#winners-table thead display:none` + badge `absolute` + 3 kolum, screenshot mobile ditinjau mata (kad P08/badge/nama/â— MERAH Â· 1 ARIF â€” sepadan awam). Production disahkan LIVE node fetch (`winners-table`+`max-width:639px`+`'Ke-3','Ke-4'`+`text-xs">E</th>`, ~24s).

**FLOW git:** 38% (b5b8e5dâ†’merge 4af4654) tergantung tanpa doc/ff â†’ sesi ni guna **stash â†’ checkout test â†’ ff â†’ pop** untuk selaras dulu, baru commit 3 fix `2754336` push test â†’ merge main `9e1ac87` â†’ verify LIVE â†’ doc `b5f9bda` (betulkan rekod 38%) â†’ ff test. main==test==`b5f9bda`.

--- (rekod sesi lepas) ---
### ðŸ†• Sesi 2026-07-15 (petang ~21:06â€“21:40): sistem-olahraga â€” UI FIX MOBILE (3 fix) âœ… LIVE PRODUCTION (main `c1841d4` merge, doc `efb29b8`)
**main == test == `efb29b8`. TIADA kerja tertunggak.** Master minta via /remote-control (telefon). Frontend sahaja, 0 backend/migration.

**3 FIX:**
1. **Butang Daftar (Ekspres+Manual)** dalam card *Pendaftaran Acara Baharu* (`admin.html`) â†’ `w-full md:w-auto`. Manual ada kembar "Batal Edit" â†’ wrapper tukar `flex flex-col md:flex-row md:justify-end gap-3` (stack di telefon), batal buang `ml-3` jadi `w-full md:w-auto`.
2. **Butang Simpan** dalam card *Pengurusan Penilaian* (`btn-simpan-penilaian`) â†’ `w-full md:w-auto`.
3. **Table pemegang pingat cetak** (`laporan.js` `renderPingatExtra` colgroup): Nama 30â†’**36%**, Kelas 8â†’**12%**, Rumah 20â†’**12%**, Jumlah 9â†’**7%** (=100%). Master minta nama & kelas panjangkan + 1 line, rumah pendekkan.

**ðŸ”‘ PENGAJARAN â€” "berapa char muat?" â†’ VERIFY, bukan agak:** Master tanya 36% muat berapa char. Lucy anggar matematik ~38 char (A4 portrait 190mm, 8pt bold). **Realiti Playwright: 46 char muat 1 line** (`MUHAMMAD ARYAN DANISH FARID BIN MUHAMMAD FARID`), 0/107 wrap. Font cetak sebenar lebih rapat dari kira-kira. **Keputusan reka: BUANG idea `nowrap` (paksa 1 line + risiko melimpah) â†’ guna lebar kolum + `table-layout:fixed` sebagai limit semula jadi; CSS auto-wrap yang panjang gila je.** Tiada kod limit-char (KISS) â€” lebar ITU limitnya.

**VERIFY (spec `tests/verify-ui-mobile-fix.spec.js`, gitignored, READ-ONLY 0 tulisan DB):** sajikan admin.html+laporan.{html,js} LOKAL lawan API staging (nba3003/1234). Butang: 308=308px telefon 390 / auto 90Â·157Â·107px desktop 1440 (klik tab utama `tab-admin-acara`/`tab-admin-penilaian` DULU sebab butang `hidden` sampai tab aktif). Pingat: screenshot 3 browser ditinjau mata + kira wrap. Production disahkan LIVE node fetch (`width:36%`+`width:12%` ADA, ~8s selepas push).

**FLOW:** commit `6bdf865` push test â†’ (master confirm) merge `--no-ff` `c1841d4` push main â†’ deploy â†’ verify production LIVE â†’ doc `efb29b8` â†’ ff test ke main. Seal: node --check OK, wrangler dry-run 186.77 KiB, 0 secret.


--- (rekod lama diringkaskan) ---

## Compacted History
*Diringkas dari ~35 rekod sesi (2026-06-24 â†’ 2026-07-15) pada 2026-07-18. Detail penuh: `compaction/snapshots/current-session-2026-07-18.md`. Pengajaran teknikal terperinci **sudah hidup dalam auto-memory** (`feedback_*`, `project_*`) â€” sengaja tak disalin semula (DRY). Ini pointer kesinambungan sahaja.*

**sistem-olahraga** (majoriti sesi 2026-07-05 â†’ 2026-07-15, SEMUA âœ… live `atletik.celikguru.my`):
- **Markah per-hakim** â€” jadual `markah_hakim` PK 5-lajur, corak RAW+DERIVED (`markah_penilaian` = nilai terbitan), skala turun 20â†’10, agregat SUM. Deploy 4-fasa manual (jadualâ†’kodâ†’kosongkan), data skala-20 lama dikosongkan (hakim wajib nilai semula). Pengajaran â†’ [[feedback_nilai_terbitan_arkib]], [[feedback_guard_mutation_test]], [[project_sistem_olahraga]].
- **Hardening multi-tenant** â€” bug `id_kategori` PK komposit, JOIN separuh kunci = baris berganda (LIVE production: NBA3003 papar 84 baris sebenar 42). Guard `npm run guard` dijadikan gerbang CI sebenar (guard lama gitignored = palsu). â†’ [[feedback_pk_komposit_join]], [[feedback_ujian_tolak_dan_terima]], [[feedback_guard_mutation_test]].
- **Cetak laporan** â€” header berulang guna `<thead>` (bukan `position:fixed`), verify guna PDF sebenar + render imej + tengok. â†’ [[feedback_verify_cetak_visual]], [[reference_curl_ssl_revoke]].
- UI polish mobile bertimbun, senarai pingat pemegang-sahaja, kad responsif tab Keputusan, susunan + cetak senarai peserta/mula, slider penilaian hakim, banner tarikh tutup, acara 50m â€” semua frontend, live.

**mypwa-v2 / erpm** (sesi 2026-06-25 â†’ 2026-07-16, semua âœ… live `erpm-sksalor.celikguru.my`):
- Logik ETR baru berasaskan gred (`TOV<50 â†’ ETR=50; TOVâ‰¥50 â†’ ETR=min(TOV+10,100)`), Trend Markah, drill-down carta gred (+cache), role PELAWAT, nama fail PDF slip ikut nama murid, admin muat turun senarai murid. â†’ [[project_erpm_v2]], [[reference_mypwa_deploy]], [[reference_mypwa_pelawat_test]].

**ðŸ”´ ITEM TERBUKA dari tier lama (belum buat â€” JANGAN kambus):**
- **PROJEK: `idme-pajsk-ext` (Chrome Extension PAJSK â†’ IDMe KPM)** â€” spec + plan **SIAP** (8 task TDD), repo `Documents/code/idme-pajsk-ext` (spec `ca3abcc`, plan `890821f`). Pendekatan A (MV3 Side Panel + blok JSON `#pajsk-export` dalam mypwa-v2 pajsk.html). Task 1-6 boleh mula tanpa external; **Task 7 GATED** â€” master kena inspect borang `idme.moe.gov.my` bekal selector medan. Ujian `node --test` (tiada npm). **NEXT: master pilih cara execute.** *(Tiada dalam auto-memory â€” patut jadi entri `project_` sendiri.)*
- ~~**Rate limiting `/api/login` (sistem-olahraga)**~~ âœ… **DITUTUP 2026-07-19** â€” throttle D1 `login_attempts` keyed IP+username LIVE production (main `5468d85`). Fix 500â†’400 sekali. Lihat rekod sesi malam 2026-07-18/19 di atas.
- Backlog projek (Lompat Tinggi Fasa 2 kad responsif, backend DELETE-sebelum-validate C2, guard scan `src/**/*.js` sebelum Code-DRY-extraction, normalisasi markah_penuh, ETR boleh-laras, eksport Excel) **tercatat dalam CLAUDE.md projek masing-masing** â€” takkan hilang.
- Cadangan audit kecil Lucy (belum master putus): `created_at` timezone di tempat lain; teks/border pucat page cetak lain (slip ujian, pajsk, RPM). **Kekal dalam snapshot.**

*Kredensial ujian yang berulang dalam rekod lama (test/demo DB sahaja) sengaja TIDAK dibawa masuk sini. Tiada `JWT_SECRET`/token API dalam ringkasan ini.*
