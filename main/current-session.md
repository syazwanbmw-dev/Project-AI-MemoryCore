# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: `opr-insaniah` — sesi 2026-10-05 22:41 → 2026-10-06 00:05: code-review `3b04b4c` → fix → T9 deploy → ujian peranti → gabung master. (Sesi lebih awal: lihat bawah.)
**Current Project**: `opr-insaniah` (Apps Script terikat Sheet, DELIMa)
**Status (terkini 2026-10-06 22:56)**: 🟢 **guru `@54` LIVE (ralat Hantar → kotak amaran, `6234e26`, suite 934/0); `@53` = nama program baharu + folder Drive direname. Ujian peranti @54 menunggu master; belum push origin.** Butiran blok sesi 22:17 di bawah.
**Status TERKINI (2026-10-06 23:52)**: 🟢 **guru `@55` LIVE — input MASA dua pemilih jam (`7a8a31e`, suite 958/0, 16/16 mutan); master dipush origin (`54aa4c1`+). Ujian `@55` "dibuat di iPad" — lulus/gagal TIDAK dinyatakan (tanya master). `@54` lulus iPad.** Butiran blok sesi 23:10 di bawah.
**Status sebelumnya (22:56)**: `@54` LIVE (kotak amaran ralat Hantar).
**Status sebelumnya**: gambar @52 selesai.
**Status sebelumnya (17:00)**: `@51` LIVE; baki `d5939d5` belum deploy.
**Status sebelumnya**: 🟢 **Hirisan B SIAP (2026-10-06 14:12): guru @50 LIVE, ujian peranti lulus ("semua lulus"; peranti = LAPTOP, disahkan master 14:14 — telefon/iPad belum diuji utk panel baharu), `panel-kelas` digabung ke `master` @38b9ed4, suite 866/0, DIPUSH origin (master @f3b6363, 14:15). Calon seterusnya: KAUNTER reset · panduan guru · kotak Cari kelas · launch rasmi (keputusan master).**
- T9 selesai: master push 20 fail -> migrasi x2 (true lalu false) -> Lucy create-deployment @50 -> list-versions sahkan -> uji peranti -> merge. Silap Lucy: `cd` ke folder memory -> `!` master "Project settings not found" (tiada apa naik); pulang ke folder projek + beri `cd /c/Users/...` eksplisit.

**Status TERKINI (2026-10-07 00:40)**: 🟡 **guru `@56` LIVE — PDF iOS R4 (`b8573a7`, suite 959/0, 4/4 mutan). Tab kosong HILANG di semua iOS; satu klik hanya bila pop-up dibenarkan (Chrome iPad), Safari + web app masih DUA klik. Menunggu keputusan master: A (berhenti + label "PDF sedia — ketik untuk buka ↗") vs B (kongsi PDF DOMAIN awal, ubah #29, plan dahulu). PPD (`moe.gov.my`) tak boleh buka pautan DOMAIN — sudah begitu sekarang, masalah BERASINGAN; tanya master PPD terima OPR macam mana. master ahead 4 origin (belum push). Jalan balik `@55`.**

**Status TERKINI (2026-10-07 01:27)**: 🟢 **guru `@57` LIVE + digabung master (`71b2cdd`; origin `346153c`; suite 980/0). PDF ANYONE_WITH_LINK semasa cipta + PDF_URL dalam senarai + <a> terus. Ujian @57 lulus iPad Safari+Chrome+Android; PPD (moe.gov.my), iPhone, laptop BELUM. Jalan balik `@56`. Baki: baris amaran launch-checklist (belum diputus), KAUNTER reset, panduan guru, kotak Cari kelas, launch rasmi.**

**Status TERKINI (2026-10-07 19:57)**: 🟡 Master: "1 kekal, 2 nanti aku buat. 3 ok" — `ANYONE_WITH_LINK` KEKAL (disahkan sedar; CLAUDE.md "Bersyarat" dijawab) · `USERS` guru + admin kedua = master buat sendiri · ujian akaun guru biasa = master akan uji (belum dilapor) · guru pertama/saluran sokongan tak dijawab. Belum release rasmi.

**Status TERKINI (2026-10-07 19:5x)**: 🟡 Master tanya "Dah boleh release untuk cikgu guna kan?" — Lucy bentang SYARAT (bukan "ya"): (4) sahkan harga `ANYONE_WITH_LINK` (CLAUDE.md: timbang semula sebelum launch) · (5) `USERS` guru sebenar + admin kedua + Sheet tak dikongsi · (6) ujian akaun guru BIASA · (7) guru pertama/saluran sokongan · + iPad, PPD. 7 ancaman ✅ (XSS senarai diaudit hari ini). `docs/launch-checklist.md` dikemas kini. Master: "1 dah bersih (Sampah Drive). 2 ok. 3 ok nnti aku test."

**Status TERKINI (2026-10-07 19:3x)**: 🟢 **@61 TERBUKTI** — tangkapan Sheet master: RUJUKAN 37 baris (14 NILAI baharu, tiada lama), OPR header sahaja, TETAPAN `KAUNTER_2026`=0 + `BERSIH_KIRA` kosong. Peranti ujian: laptop Chrome + Android Chrome (lulus). Belum: Sampah Drive dikosongkan, laporan `OPR-2026-0001`, iPad/iPhone.

**Status TERKINI (2026-10-07 16:5x)**: 🟢 **@61 DIGABUNG ke master `c0b9723` (merge --no-ff, dipush origin, suite 1059/0).** Master "Lulus gabung master" — peranti tak disebut; angka log langkah editor 1–3 TIDAK dilapor. Cabang `nilai-sebenar` dibiar. Tiada kerja tertunggak utk @61. Calon: uji PPD · Cari kelas/launch tangguh.

**Status TERKINI (2026-10-07 16:1x)**: 🟢 **guru `@61` LIVE** (list-versions 61; cabang `nilai-sebenar` @`eeec0ac` belum digabung master; jalan balik `@60`). Master "selesai 1 hingga 3. create deployment" — angka log TIDAK dilapor (tanya). Pra-deploy: pull dibanding git 20/21 sama (`appsscript.json` beza format+timezone = dijangka). Ujian peranti `@61` BELUM (sebut peranti+pelayar).

**Status TERKINI (2026-10-07 12:4x)**: 🟡 **kod `@61` DISEDIAKAN — cabang `nilai-sebenar` @`765fae1` (dipush origin; belum digabung master); guru kekal `@60`; suite 1059/0. A: 14 nilai sebenar + `migrasiNilai()` · C: ikon SVG `✕` (cip Kelas + butang gambar) · B: `BersihService.gs` (padam SEMUA laporan + fail Drive yatim + reset KAUNTER, dua langkah). `create-deployment` MENUNGGU master di editor (pelayar): `migrasiNilai` ×2 → `kiraBersihLaporan` → `bersihkanSemuaLaporan` (≤60 min) → balas log.** Butiran blok sesi 12:07 di bawah.

**Status TERKINI (2026-10-07 11:3x)**: 🟢 **@60 DIGABUNG ke master (`53ca059`, dipush origin, suite 1013/0); master lulus ujian peranti ("Lulus gabung master", peranti tak disebut). Tiada cabang tertunggak. Calon: ikon ✕ chip · uji PPD · KAUNTER reset · Cari kelas/launch tangguh.** Pengajaran: `is-ancestor` gagal senyap dlm rantaian `&&`; nama fail mesej komit di $TEMP mesti UNIK.

**Status TERKINI (2026-10-07 11:2x)**: 🟢 **guru `@60` LIVE — butang modal seragam (tertib Batal|Padam), ikon emoji→SVG, amaran jangan-kongsi dalam panduan; cabang `butang-modal-svg` dipush origin; suite 1013/0. Ujian peranti `@60` BELUM (sebut peranti+pelayar). Belum digabung master (bawa pautan-panduan+butang-konsisten+butang-modal-svg). Jalan balik `@59`. Penjana PDF: `tools/jana-panduan.js`. Drive Manage versions = BROWSER sahaja.** Butiran: `opr-insaniah/MEMORY.md`.

**Status sebelumnya (10:3x)**: 🟢 **guru `@59` LIVE — butang kepala diseragamkan (`butang-konsisten` @`57d8537`, dipush origin; suite 996/0; 10/10 mutan). Ujian peranti `@59` BELUM. `pautan-panduan` (@58) + `butang-konsisten` belum digabung master. Jalan balik `@58`.** Butiran blok sesi 10:16 di bawah.

**Status TERKINI (2026-10-07 10:0x)**: 🟢 **guru `@58` LIVE — butang "📖 Panduan" (pautan Drive, pemalar `PAUTAN_PANDUAN`); cabang `pautan-panduan` @`1a03698` dipush, BELUM digabung ke master; suite 988/0, 13/13 mutan. Ujian peranti `@58` BELUM (telefon utama; SEBUT peranti+pelayar). Jalan balik `@57`.** Master tiada iPhone (jangan minta). Butiran sesi 08:18 di bawah.

### Sesi 2026-10-07 12:07 → ~12:45 (nilai sebenar + ikon SVG + padam laporan → kod @61 siap, deploy menunggu master)
- Master "Opr insaniah" → Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan; master==origin `f5e9beb`, tiada cabang tertunggak). Brief: calon ikon ✕, PPD, KAUNTER, Cari kelas. Master: "1 tu pada cip apa. Aku nak tukar nilai tu, haritu kita main letak je" → Lucy baca `Setup.gs:63` (BENIH 12 NILAI) + `app.js.html:2303`; soalan bernombor (senarai betul / ELEMEN / laporan ujian). Master: *"1. Padam semua laporan, bersihkan juga gambar yatim dan fail yatim. 2 buat kerja svg. 3. senarai nilai sebenar (14)"*.
- Plan 3 komit (A nilai / B bersih / C ikon) + 4 soalan (ejaan, padam, KAUNTER, fungsi vs sheet). Master "A tu selamat tak?" → jawab dgn kod (Validate.gs:43; laporan simpan TEKS) → "Proceed ikut syor". Cabang `nilai-sebenar`.
- **A** `2d7d27a`: 14 nilai + `migrasiNilai()`/`rancangMigrasiNilai()`; 16 ujian, 14/14 mutan. **C** `56d28c5`: `IKON_TUTUP`; 39 ujian lama gagal pada mulanya (harness stub-DOM + pagar innerHTML) ⇒ diselaraskan (DIKETATKAN, bukan dilonggar); tangkapan 390px → saiz butang gambar 12→16px; 14 mutan. Master tanya "butang gambar bukan dah ada icon x?" → separa betul (ikon kiri SVG, ✕ kanan teks).
- Master "Deploy a+c dulu dan terus b" → commit-seal (suite 1038/0, .claspignore sah, rahsia bersih) → `clasp push --force` → `npx clasp pull` folder sementara SAHKAN penanda → push cabang origin. `create-deployment` TAHAN: master mesti jalankan `migrasiNilai` ×2 di editor (pelayar) dahulu.
- Master "Reset" (KAUNTER). **B** `765fae1`: `BersihService.gs` (kira+padam dua langkah, penanda `BERSIH_KIRA`, ≤60 min), `senaraiFailKerja_`, `kosongkanBarisOpr_`; ALLOW 21→23; 21 ujian, 24/24 mutan. Dua pusingan ketatkan ujian (M9/M10/M22/M24 terselamat; garis dasar gagal sekali). Push editor + cabang origin.
- Silap kecil Lucy: (1) jawab "cip apa" tanpa semak butang gambar; (2) skrip penyunting separuh berjaya (anchor padan 2 tempat); (3) `node -e` rosak escape lagi; (4) `npx --prefix` gagal; (5) "dibunuh" palsu bila garis dasar gagal. Dicatat dlm opr-insaniah/MEMORY.md.
- **Sambung:** master di editor (pelayar): `migrasiNilai` ×2 → `kiraBersihLaporan` → `bersihkanSemuaLaporan` (≤60 min) → balas log → Lucy `create-deployment` `@61` + `list-versions` → master uji peranti (SEBUT peranti+pelayar) → "Gabung master". Disimpan: opr-insaniah/MEMORY.md + CLAUDE.md, auto-memory (project), relationship-memory, fail ini.

### Sesi 2026-10-07 10:3x → 11:2x (butang modal + ikon SVG + amaran panduan → @60)
- Master "Opr insaniah" lalu satu mesej tengah-giliran: seragamkan butang modal, emoji→SVG, baris amaran jangan-kongsi dalam panduan. Lucy baca fail Lucy+CLAUDE.md+MEMORY(sebahagian; fail 314KB)+git (sepadan), bentang plan + 3 soalan bersyor. Master: "1. Ya (Tutup=sekunder), 2. teks amaran ditulis sendiri, 3. ya (cabang baharu)".
- Cabang `butang-modal-svg` dari `eb2166d`. TDD merah dahulu (`butang-modal.test.js` 9 + `ikon-svg.test.js` 3), kod via skrip Node CRLF-selamat, 18 mutan (13 modal + 5 ikon, 0 selamat). Tangkapan sebelum/selepas 9 bingkai 390px → `SendUserFile`. Master "Looks good".
- Tiga komit berasingan dibina semula dari HEAD (modal → ikon → panduan), hash akhir disahkan SAMA. PDF panduan: penjana lama HILANG ⇒ kalibrasi fon via `pdftotext` + pdf.js (worker sebagai <script>); Calibri 12pt+jidar 10mm = 1 muka surat. Master: "Dah berjaya manage version melalui browser, app je tak boleh. Deploy dan save memory dan session"; sebelum itu "jangan lagi" (tahan deploy) + "simpan penjana ikut syor" ⇒ `tools/jana-panduan.js` + ujian.
- Deploy `@60`: pra-terbang → `clasp push --force` (Lucy, diterima) → pull sementara sahkan penanda → create-deployment → list-versions v60 → commit-seal → push cabang origin.
- Silap kecil Lucy: komit A tulis "18/18 mutan" (sebenarnya 13+5) → dibetulkan selepas mutasi diulang per fail; `node -e` rosakkan `d` lagi (gagal=-1 disangka "terselamat"); `cd` ke scratchpad ubah cwd. Tiada silap lain.
- **Sambung:** master uji `@60` (SEBUT peranti+pelayar; admin DAN guru biasa; lihat Batal|Padam, Tutup putih, ikon) → "Gabung master" · calon: ikon ✕ teks pada chip · uji PPD · KAUNTER biar · Cari kelas/launch tangguh.

### Sesi 2026-10-07 10:16 → ~10:35 (butang tak konsisten → @59)
- Master "Opr insaniah. Nk fix button ni... Rujuk gambar" (screenshot telefon + carousel 9 slaid). Lucy baca fail Lucy + CLAUDE.md + git (cabang `pautan-panduan`), baca CSS `style.html`/`index.html`, bentang jadual punca + plan + 2 soalan bersyor. Master "Ok" = syor (skop sempit, emoji kekal).
- Cabang `butang-konsisten`. TDD: `tests/butang-konsisten.test.js` (8, merah dahulu) → kod (skrip Node kekalkan CRLF) → 996/0. Perasan sendiri: `repeat(2,1fr)` ⇒ guru biasa dapat Panduan separuh lebar ⇒ `auto-fit minmax(140px,1fr)`. Mutasi: M7 selamat → ujian ditambah → 10/10 dibunuh.
- Master (tengah kerja): simpan rujukan carousel merentas projek → auto-memory `reference_ui_buttons_carousel` + indeks. Tangkapan 390px (iframe; salah pilih blok skeleton dahulu → `lastIndexOf`) → `SendUserFile`. Master "Ya deploy. Save memory dan session".
- Deploy `@59`: pra-terbang → push → pull sahkan penanda → create-deployment → list-versions v59 → push cabang origin. Komit `57d8537`.
- Silap kecil Lucy: ujian regex diedit melalui `node -e` (escape rosak lagi → guna Edit); ujian awal tak liputi `.kepala-kanan` (ditangkap mutan M7); tangkapan pertama = skeleton.
- **Sambung:** master uji `@59` (SEBUT peranti+pelayar; akaun admin DAN guru biasa) → "Gabung master". Calon: butang modal · ikon SVG · amaran pautan panduan · uji PPD.

### Sesi 2026-10-07 08:18 → ~10:10 (jawapan tertunggak → butang Panduan → @58)
- Master "Jom sambung opr insaniah" → Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan, master == origin @346153c). Brief 7 tertunggak. Master jawab ikut nombor: PPD = share link/folder Drive dikongsi · tiada iPhone, laptop ok · baris amaran launch-checklist "proceed" · KAUNTER biar · panduan guru "letak di mana?" · Cari kelas & launch tangguh.
- Docs (master): launch-checklist (baris DOMAIN lapuk dibetulkan + amaran HARGA) `52ada17`; MEMORY `14619bf`; CLAUDE.md blok "Pautan Drive" ditulis semula `0f01d06` (drift ditemui semasa baca; master "Ya betulkan").
- Panduan: Lucy bentang tiga laluan (PDF WhatsApp / pautan dalam app / repo public) → master pilih pautan dalam app. `superpowers:brainstorming` (bounded). Varian A (Tetapan admin) ditolak master ("rumit") → B pemalar. Cabang `pautan-panduan`: TDD `tests/pautan-panduan.test.js` (8 ujian, merah dahulu) → kod → suite 988/0 → 13/13 mutan (skrip mutasi + pulih bait) → `12f4c0a`.
- `docs/panduan-guru.md` + PDF (Edge headless, 1 muka surat; fon dibesarkan 10→11.5pt selepas tengok tangkapan) `ef330c8`. Master remote ⇒ `SendUserFile` hantar PDF; master muat naik ke Drive + beri pautan; Lucy sahkan `curl` tanpa log masuk (200 + tajuk) → `PAUTAN_PANDUAN` `1a03698`.
- "Ya deploy" → pra-terbang → push → pull sahkan 6 penanda → create-deployment `@58` → list-versions 58.
- Silap kecil Lucy: `sed -i` Windows tukar Config.gs CRLF→LF (dipulih, diff 1 baris); rantaian `&&` dgn `grep -c`=0 berhenti senyap (dikesan sebab ujian tak keluar output). Varian A dibentang dahulu sebagai syor padahal B lebih KISS — master yang menegur.
- **Sambung:** master uji `@58` (telefon): butang nampak (guru BIASA), ketik ⇒ PDF terbuka tanpa log masuk, sejajar dgn butang admin → "Gabung master" ⇒ merge --no-ff + push · amaran "jangan kongsi pautan" dalam panduan? · uji PPD, laptop · KAUNTER biar, Cari kelas/launch rasmi tangguh. Disimpan: opr-insaniah/MEMORY.md, auto-memory (project+index), relationship-memory, fail ini.

### Sesi 2026-10-07 00:46 → 01:27 (ANYONE_WITH_LINK → @57 → merge)
- Master "Opr insaniah" → Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan; master ahead 5 bukan 4 — dibetulkan). Brief: A/B, PPD, uji iPhone/laptop, push.
- Master: kongsi anyone-with-link "masalah selesai" → Lucy jelaskan ia selesaikan PPD bukan dua klik; baca `DriveService.gs` (kongsi MALAS semasa klik) → plan 6 langkah + HARGA (gambar murid) + langkah 0 (DELIMa). Master "Betul, pasang perkongsian semasa cipta".
- TDD: `tests/kongsi-awal.test.js` (20; merah dahulu) → kod (`terap.js` skrip CRLF-selamat) → 980/0; 11 mutan (10 dibunuh; M11 setara). Jumpa sendiri: <a> togol kad telefon → guard + ujian; ujian lama 911 patahkan guard di dalam cabang → pindah sebelum cabang.
- Cabang `kongsi-awal` `63e2d9b`. Master "Deploy and push" → clasp push + pull sahkan → TAHAN deploy → master run `migrasiKongsiPdf` ×2 (`dikongsi:5`) → create-deployment `@57` → push cabang. Master uji lulus → "Gabung master" → merge --no-ff + push.
- Silap kecil Lucy: suite `node --test tests` (folder) gagal → guna glob `tests/*.test.js`; ralat laluan clasp tidak berlaku. Tiada silap lain.
- **Sambung:** sahkan PPD buka pautan · iPhone+laptop · baris amaran launch-checklist (tanya master) · KAUNTER reset · panduan guru · launch rasmi. Disimpan: opr-insaniah/MEMORY.md, auto-memory (project+index), relationship-memory, fail ini.

### Sesi 2026-10-07 00:00 → 00:40 (PDF iOS dua klik → subagent opus → R4 → @56)
- Master "Opr insaniah" (Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git: sepadan, master bersih). Master lapor PDF iPad/iPhone dibuka app Drive, klik Buka dua kali, pertama about:blank; minta subagent opus. Subagent (read-only): punca = pautan datang selepas klik (async) ⇒ gerak isyarat luput; syor luas regex R3. Master: Chrome pun kena ⇒ hipotesis tak cukup; Lucy baca `app.js.html:25,268`.
- Master mahu SATU klik + tiada tab kosong. Dua peringkat dibentang; master "1 dulu" ⇒ R4 TDD (`b8573a7`): regex iOS + iPad Macintosh&&maxTouchPoints>1; window.open(URL) terus bila tab null; komen R3 salah dibetulkan. "Ya deploy" ⇒ `@56` (push 20 fail → pull sahkan 4 penanda → create-deployment → list-versions v56).
- Ujian master (iPad): Chrome satu klik tapi perlu enable pop-up; web app + Safari tiada tab kosong, masih dua klik. Master tanya "kalau kongsi anyone selesai kan?" → jelaskan mekanisme + risiko gambar murid; master: "DOMAIN cikgu je, PPD moe.gov tak boleh" → sudah begitu sekarang, masalah berasingan.
- Silap kecil Lucy: PowerShell Select-String exit-code 255 pada commit (commit tetap berjaya — semak `git log`). Tiada silap lain.
- **Sambung:** keputusan A/B · jawapan PPD · uji iPhone + laptop `@56` (SEBUT peranti+pelayar) · push origin. Disimpan: opr-insaniah/MEMORY.md, auto-memory (project+index), relationship-memory, fail ini.

### Sesi 2026-10-06 23:10 → 23:52 (@54 lulus → push → input Masa → @55)
- Master "Sambung opr insaniah" → Lucy baca fail Lucy + MEMORY projek + git (sepadan, master bersih, ahead 2). Master "@54 lulus di ipad. Push" → suite 934/0 → push origin `0f6cbc2`.
- Master: "ubah sikit cara input masa, sekarang taip manual, ada idea?" → Lucy BACA kod dulu (MASA = teks bebas ≤50 `form.html:37`, `Validate.gs:35`) → cadang dua pemilih jam. Master: format KEKAL · laporan lama akan DIPADAM · Tamat boleh kosong → Lucy bentang plan + 1 soalan tambahan (Tamat<=Mula) → "Ikut syor".
- TDD: 24 ujian (`tests/masa-pemilih.test.js`), pusing-balik 1440 minit, 16/16 mutan. **M10 terselamat** pada ujian pertama (isiMasa tak kosongkan Tamat bila teks tak boleh dihurai) → ujian ditambah. Tangkapan 390px (iframe) muat. Commit `7a8a31e`.
- "Ya deploy" → pra-terbang (.claspignore sah, 20 fail) → push → `clasp pull` folder sementara sahkan penanda → create-deployment → list-versions v55. Lucy push sendiri (diterima).
- Silap kecil Lucy: skrip memori guna backtik tak di-escape dlm template literal (SyntaxError) → guna fail teks berasingan. Gotcha CRLF: tulis skrip penyunting ke fail + kekalkan CRLF.
- Master "Ujian @55 dibuat di ipad" — KEPUTUSAN tak dinyatakan; Lucy catat tepat begitu, tak andai lulus.
- **Sambung:** tanya master lulus/gagal @55 (+ telefon/laptop belum). Calon: KAUNTER reset · panduan guru · kotak Cari kelas · launch rasmi. Jalan balik `@54`.
- Disimpan: opr-insaniah/MEMORY.md (`54aa4c1`+commit ujian), auto-memory (project+index), relationship-memory, fail ini.

### Sesi 2026-10-06 22:17 → 22:56 (tukar nama program → @53 → ralat Hantar → @54)
- Master "Nk fix opr insaniah" (tanpa butiran) → Lucy TANYA, tak teka. Nota lama `aku listkan apa yang nak fix.txt` item 1 sudah selesai (@37–44); fail itu memuat password awal prod dlm teks biasa (diluar git) — amaran security diberi.
- Tukar nama: OPR **Pembentukan Karakter Karamah** Insaniah + label "Tajuk Program"→"Tajuk" + `FOLDER_AKAR` (`b82ee09`). Master rename folder Drive DAHULU, lalu "ya deploy" → push→pull sahkan→create-deployment **@53**. Ujian LULUS (iPad). Dipush origin `67329b7`.
- Ujian @53 iPad: master terlupa tanda Elemen+Nilai → mesej di bawah Hantar tak nampak → kotak amaran umum `bukaAmaran(tajuk,mesej,fokusPulang)`, 3 laluan ralat Hantar (`6234e26`, suite 934/0, 9/9 mutan). Master "Proceed" lalu "Ya deploy" → **@54** LIVE (list-versions sahkan). Ujian peranti @54 BELUM; BELUM dipush origin (6234e26 + commit docs).
- Silap kecil Lucy: regex dalam `node -e` rosak lagi (escape) → guna Edit/Write. Tiada silap lain.
- **Sambung:** master uji @54 (SEBUT PERANTI): klik Hantar tanpa tanda Elemen/Nilai ⇒ kotak "Borang belum lengkap" + OK; Hantar sah ⇒ tiada kotak. Lepas lulus: push origin (master memilih). Calon: KAUNTER reset (laporan ujian @53 guna satu nombor) · panduan guru · kotak Cari kelas · semakan client sebelum jana PDF (ditolak buat masa ini) · launch rasmi.

### Sesi 2026-10-06 15:44 → ~17:00 (gambar upload: tambah bukan ganti → @51 → baki)
- Master "jom opr insaniah" → Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan, master bersih @f3b6363). Master minta fix upload gambar: pilih satu-satu MENGGANTI (punca `gambarKecil = senarai`), mahu butang [ikon SVG | N | ✕], klik = pratonton, ✕ = buang daripada laporan.
- Lucy baca kod dulu, bentang plan + 3 soalan bersyor (lebih had ditolak / 1+2 ditolak sekaligus / ✕ tiada pengesahan). Master "Setuju ikut syor"; tanya "max gambar 2 je ya?" → Lucy SAHKAN dlm kod (`Validate.gs:31` {min:1,maks:2} via SESI.peraturanGambar), bukan agak.
- Kod: `semakTambahGambar`/`buangGambar`/`IKON_GAMBAR` (Kongsi.html tulen), `jalankanKerjaGambar` (satu rantai Promise tambah+buang), `lukisButangGambar`, modal `#tudungGambarPratonton`. Ujian `gambar-tambah.test.js` (stub DOM jalankan pendengar sebenar), 15/15 mutan. Lucy jumpa sendiri 3 perangkap: ✕ dua kali pantas (indeks basi), dua pilihan bertindih (gerbang buka awal), CSS `.cip-gambar button` (0,1,1) kalah `.cip-gambar-buang` → garis ✕ hilang. Tangkapan `%TEMP%\opr-gambar\*.png`; master "Looks good to me. Ya deploy. Baiki baki tu dalam commit berasingan. Save memory dan session".
- 🔴 **Deploy: master di TELEFON.** Baris `! clasp push` yang master taip sampai sebagai TEKS (tidak dijalankan). Lucy sahkan dgn `clasp pull` ke folder sementara (0 penanda) → master tanya "Aku guna phone, tk boleh ke?" → Lucy cuba `clasp push --force` sendiri SEKALI (diterima kali ini; sebelum ini classifier menolak), sahkan dgn pull semula, `create-deployment` ⇒ **`@51`**, `list-versions` sahkan.
- Baki "gambar hantu" (Batal semasa foto diproses) dibaiki `d5939d5`: `generasiGambar` + `gantiSemuaGambar()`. Satu kesilapan kecil Lucy: ujian sumber terlalu lebar (padan `gambarKecil = senarai` dlm gantiSemuaGambar) → disempitkan; `node -e` escape rosak lagi (gotcha lama) → guna Write.
- Lucy tertinggal baris attribution pada commit pertama → `--amend` (belum push). Disimpan: opr-insaniah/MEMORY.md, auto-memory, relationship-memory, fail ini.
- **Sambung:** master uji `@51` di telefon (+iPad), SEBUT PERANTI → "ya deploy" utk `@52` (baki; tertib: push → sahkan dgn pull → create-deployment) → keputusan master: gabung `gambar-tambah` ke master + push origin. Calon lain: KAUNTER reset · panduan guru · kotak Cari kelas · launch rasmi.

### Sesi 2026-10-06 21:43 → 22:0x (kotak amaran lebih-had → @52)
- Master (selepas ujian `@51` di telefon): *"Cuma pilih gambar ketiga tu memang tolak dan label keluar maksimum gambar dua, tapi label tu makluman kat bawah button hantar dan user tak nampak… tolak senyap. Lama baru perasan. Mungkin kena buat toast di tengah skrin… tutup bila user klik button ok. Fix, then deploy semula. Baru deploy @52"* — izin deploy `@52` DIBERI dalam mesej yang sama (tiada tanya semula).
- Lucy: `#tudungGambarAmaran` (alertdialog; OK / klik latar / Escape; TIADA auto-tutup; fokus pulang ke input; tatal dikunci). Mesej had TIDAK lagi ke `#formStatus`. 2 ujian lama (menyemak `#formStatus`) ditukar kerana ia menjaga tingkah laku yang master tolak; 10 ujian baharu; 11/11 mutan; suite 923/0. Tangkapan iframe 390px (Edge headless <500px potong = artifak lama).
- Deploy `@52`: commit `2773a70` → `clasp push` (Lucy) → `clasp pull` ke folder sementara sahkan penanda → `create-deployment` → `list-versions` sahkan v52. Mengandungi baki gambar-hantu `d5939d5` juga.
- 🔴 Pengajaran dicatat dlm `feedback_ujian_buta_skrin`: 906/0 + 15/15 mutan lulus kerana ujian menyemak TEKS wujud dlm `#formStatus`, bukan sama ada ia KELIHATAN; hanya master di telefon menemuinya. Tanya "di mana mata pengguna ketika ini?" bukan "adakah teks ditetapkan?".
- Silap kecil Lucy: `node -e`/tangkapan <500px (artifak, diukur semula dgn iframe 390px). Cwd beralih ke folder memory beberapa kali (gotcha lama) — sentiasa `cd` eksplisit.
- **Sambung:** master uji `@52` di telefon (SEBUT PERANTI): ke-3 ditolak ⇒ kotak + OK · 3 sekali gus ⇒ kotak · pilihan sah ⇒ tiada kotak · Batal semasa foto besar diproses ⇒ tiada gambar hantu. Lepas lulus: keputusan master gabung `gambar-tambah` → master + push origin. Calon lain: KAUNTER reset · panduan guru · kotak Cari kelas · launch rasmi.

- **22:10 "Proceed"** (tafsiran Lucy: syor terakhir = gabung+push): `gambar-tambah` → master `93aaabb` (--no-ff; `git merge -F -` tak diterima → guna `-m`), suite 923/0, dipush origin. **22:14 master: "Uji @52 lulus di ipad dan phone"** (kedua-dua peranti disebut tanpa diminta; butiran per-item tidak dilapor) → fitur gambar SELESAI, tiada tertunggak.
- **Sambung (terkini):** tiada kerja gambar. Calon seterusnya (belum putus master): KAUNTER reset (padam baris ujian + fail Drive yatim dulu) · panduan guru · kotak Cari kelas · salin Escape/ID-pangkas ke `opr-program` · launch rasmi. Kandungan "Sambung" dlm blok ini yang menyebut ujian `@52`/gabung = SUDAH selesai.

### Sesi 2026-10-06 13:14 → 14:10 (Hirisan B: tangkapan -> pelarasan -> T9 bermula)
- Master "sambung opr insaniah". Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan). Master minta buka folder tangkapan; soalan: "panel telefon 390 tak jadi panjang sangat ke?" -> Lucy BACA tangkapan sendiri (bukan agak): ya, ~3 skrin utk 18 kelas.
- Master soal 26 kelas / pagination -> Lucy tolak pagination (bercanggah jawapan #2 paparan serta-merta), syor padat + lipat + (cari ditangguh). Master: "1+2 ikut syor".
- `5ab17d3` lipat NYAHAKTIF dlm `<details>` + baris diketatkan. 🔴 Lucy TERSILAP anggar ~55px; tangkapan 26 kelas tunjuk ~80px (94 -> 80). Dilapor jujur, master pilih "terima".
- Master: "susun ikut nombor tahun 1..6" -> `260c57f` `bandingKelas()` di pelayan (barisKelas + bacaRujukan_ KELAS), membalikkan "urutan sheet" utk KELAS SAHAJA. Tafsiran "semua paparan" disahkan master 14:04.
- Ujian: kelas-panel-lipat (7, DOM stub, 6/6 mutan), kelas-susunan (9, 7/7 mutan); 4 ujian lama dikemas kini. Mutasi dgn skrip fail + pulih bait asal (sahkan hash).
- Master sahkan: benih 18 kelas lulus (semakan Sheet RUJUKAN) -> "proceed t9". Pra-terbang: .claspignore sekat tests/docs/tools/*.md, 20 fail tracked, list-versions 49.
- Silap kecil Lucy: skrip penjana sementara petikan tunggal bersarang (SyntaxError) -> dibetulkan sendiri; Edge headless ralat task_manager = bunyi dalaman, bukan kegagalan.
- Tangkapan: `%TEMP%\opr-panel-kelas-v2\telefon-26-{tutup,buka}.png`. Susunan baharu TIDAK nampak pada tangkapan (penjana tanpa pelayan).
- **Sambung:** master push + migrasi x2 -> Lucy create-deployment `--deploymentId AKfycbxss9...` -> list-versions sahkan v50 -> master uji peranti (SEBUT PERANTI): panel buka/tambah/nyahaktif/aktifkan/padam, lipatan, susunan 1->6 pada panel + modal borang + tapis, Edit laporan lama ber-kelas-nyahaktif, borang tolak nama tanpa huruf. Lepas lulus: merge `panel-kelas` -> master = keputusan master.

### Sesi 2026-10-06 10:10 → 12:45 (Hirisan B: EXECUTE plan, subagent-driven autonomous)
- Master: "sambung opr insaniah, execute latest plan using subagent with autonomous execution". Cabang `panel-kelas` dari master f842747. T1–T8 siap (sonnet implementer + reviewer per task, opus semakan akhir). Suite 691 -> 850/0. TIADA push/clasp/deploy.
- Fix round berlaku pada T1 (aksara tak kelihatan mentah), T3 (mojibake: PowerShell Get-Content/Set-Content utf8 gandakan pengekodan), T4 (orkestrasi tulis perlu catch + baca-semula selamat -> respons data:null MUAT_SEMULA_PANEL), T6 (ujian stub-DOM), T7 (bendera tahan-klik kekal sepanjang baca semula).
- 🔴 Semakan akhir opus jumpa PUNCA data: nama kelas 4-1/01 jadi TARIKH dlm sel KELAS laporan -> Padam kira 0 guna. Fix e59da07: nama mesti ada HURUF. Turut: Logger.log ralat tulis, POLA_AKSARA_KAWALAN diluas, spec §6 #1 dijujurkan.
- Silap Lucy sendiri: (1) arahan shell `cat >> fail` tanpa heredoc tersangkut menunggu stdin; (2) prompt re-review T7 terpotong/laluan salah — dihentikan & dihantar semula; (3) node -e dengan escape u+XXXX gagal parse (gotcha yang sama dicatat).
- Gotcha alat dicatat: Write/Edit menormalkan escape u+XXXX jadi aksara mentah; sebut kepada implementer dlm dispatch.
- Gerbang visual T7 ditangguh (ruling). Master WAJIB tengok %TEMP%opr-panel-kelaspanel-desktop/ipad/telefon-390.png sebelum T9.
- **Sambung:** master tengok tangkapan + semak Sheet RUJUKAN (tiada nama tanpa huruf) -> "ya" deploy -> T9 (clasp push master dgn !, migrasiRujukanStatus() x2, create-deployment ke ID guru, ujian peranti 5 item, sebut PERANTI). Merge ke master = keputusan master.
- Disimpan: opr-insaniah/MEMORY.md (109aef6), auto-memory (project+index), fail ini. Ledger: opr-insaniah/.superpowers/sdd/2026-10-06-panel-kelas/.

### Sesi 2026-10-06 08:47 → 09:40 (Hirisan B: PLAN oleh subagent opus)
- Master: "sambung opr insaniah, tulis plan guna subagent opus". Lucy baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan, master bersih @`8cd55f0`). Spec dianggap lulus (master minta plan terus).
- Subagent opus (~28 min, 72 tool) tulis `docs/superpowers/plans/2026-10-06-panel-kelas.md` (3436 baris, 9 task, tiada TBD). Komit `2387009` (plan) + `5ea7a5a` (MEMORY projek), tempatan, TIADA push/kod/deploy.
- Keputusan plan: §3.5 kotak `data-warisan`; Tukar Status bawah Lock; tulis baris format `@`; ALLOW 14→19 sah (spec tak silap). Lucy BELUM baca plan baris-demi-baris.
- **Jawapan master 09:41:** 1 UBAH (tapis termasuk nyahaktif) · 2 paparan serta-merta lepas Tambah, tiada reload · 3 ok · 4 ok · 5 LANGKAU (diputus 09:42). **Revisi plan SIAP 09:5x (3920 baris, jangka 805 ujian); tunggu master baca + pilih kaedah.** Soalan asal: tapis dropdown aktif sahaja · muat semula utk nampak di borang · kotak Padam 2 butang · `INACTIVE` tangan dikira aktif · ujian peranti #5 (~34 klik) buat/langkau.
- **Sambung:** master baca plan + jawab 5 soalan + pilih subagent-driven (disyor) / native → baru kod. T7 berhenti utk tangkapan; T9 deploy gerbang master (`clasp push` master dgn `!`, migrasiRujukanStatus ×2).
- Disimpan: opr-insaniah/MEMORY.md, auto-memory (project+index), fail ini.

### Sesi 2026-10-06 07:54 → 08:45 (Hirisan B panel admin KELAS: brainstorming → spec)
- Master: "sambung opr insaniah, panel admin untuk rujukan tak silap, semak dulu". Lucy baca Lucy-fail + CLAUDE.md + MEMORY.md + git (sepadan, master bersih @`36f1bee`). Betulkan: panel RUJUKAN lama DIBATALKAN; yang tertunggak = panel KELAS (Hirisan B). Master sahkan.
- `superpowers:brainstorming` (architectural). Soalan satu-satu: Q1 Edit laporan ber-kelas-nyahaktif → master: "laporan masih boleh diakses" (jawapan A). Master soal balas: "cikgu lain make a copy, nama kelas beza" → reka bentuk berubah (panel = cara sekolah baharu set kelas; Padam hanya bila 0 laporan guna). Q2 tambah satu-satu (B). Q3 lajur `STATUS` (Cara 1).
- Bahagian 1–3 reka bentuk diluluskan ("Ok"/"Proceed"). Gotcha ditemui masa tulis spec: KELAS dipisah `;` → nama tak boleh ada `;`; tolak awalan `= + - @`.
- Siap: spec `docs/superpowers/specs/2026-10-06-panel-kelas-design.md` (`4c770bd`), `MEMORY.md` projek (`8cd55f0`). TIADA kod/push/deploy.
- **Sambung:** master baca spec → lulus → plan oleh subagent **opus** → master lulus plan + pilih kaedah → kod. ALLOW 14→19; ritual deploy tambah `migrasiRujukanStatus()` ×2; `clasp push` master jalankan dgn `!`.
- Corak: master UBAH reka bentuk bila soalan datang dari perspektif pengguna lain (sekolah lain) — tanya "sekolah lain macam mana?" awal.

### Sesi 2026-10-05 22:41 → 2026-10-06 00:05 (review → deploy @49 → merge)
- `/code-review 3b04b4c` (subagent): tiada bug pasti, 3 kelemahan rendah. Master "fix 1 dan 2": (1) ID tak dipangkas dua belah dlm `bukaModalKelasPapar` vs `selKelas()`; (2) modal Papar tiada Escape + fokus tak pulang. Isu 3 (overflow bertindih) dibiar. Komit `19b9a2d`, suite 691/0, 3 mutan dibunuh.
- Master tak jumpa tangkapan di `%TEMP%` (AppData tersembunyi) → Lucy buka Explorer terus. Master: "enam tangkapan looks good".
- Master tak jumpa `migrasiKelas` dlm editor → **punca: `clasp push` belum dibuat** (Lucy tersilap susun tertib, langkah 5 sebelum 6). Fix: push dulu.
- **Classifier auto-mode MENOLAK `clasp push` daripada Lucy** ("Blind Apply") → master jalankan sendiri dgn `!`. Lucy TIDAK cuba pintas. Tertib sebenar: push (19 fail, sah) → migrasi editor ×2 (OK) → `create-deployment --deploymentId` → `list-versions` sahkan v49.
- 🟡 Gotcha `!` = Git Bash: `cd C:\Users\...` hilang backslash → "No such file". Guna `/c/Users/...` atau tiada `cd` (sesi sudah di folder projek).
- Master "ya deploy" jelas sebelum `create-deployment`; "ya gabung" sebelum merge. Kedua-dua gerbang dihormati.
- Belum disahkan eksplisit: semakan Sheet manual (KELAS kolum terakhir, RUJUKAN 18 baris); peranti ujian tak disebut master ("semua lulus" sahaja).
- Disimpan: opr-insaniah/MEMORY.md (3 komit), relationship-memory, fail ini, auto-memory.
- **Sambung:** KELAS selesai. Calon seterusnya: Hirisan B (panel admin kelas, mesti halang nama pendua), KAUNTER reset (mesti padam baris ujian + fail Drive yatim dulu), panduan guru (ditangguh), mesej `kemasKiniBarisOpr_`/`padamBarisOpr_` salah-remedi, drift salinan modal ke `opr-program` (Escape/ID-pangkas belum disalin).

### Sesi malam 2026-10-05 22:12–22:35 (Papar kelas)
- Baca fail Lucy + CLAUDE.md + MEMORY.md + git (sepadan). Master "tengok tangkapan skrin" → A4, iPad, kad OK tapi **jadual desktop "memang tak ok"** (18 cip = baris ~450px).
- Master putus: butang **Papar** di kolum Kelas (kad ringkas pun) → modal papar kelas + butang **Tutup**; + tangkap semula telefon ~390px, semak limpah.
- Lucy bentang pelan (jadual sebelum/selepas + 2 soalan: label "Papar (18)", modal baca-sahaja) → master "Proceed".
- Siap: `selKelas()` (Kongsi.html, tulen) · modal `#tudungKelasPapar` · cabang aksi 'kelas' dalam pendengar tbody · CSS (`.cip-papar`; buang cip-kelas + max-width 220 + Kelas-disorok-pada-kad). `tests/kelas-papar.test.js` (13), 4 ujian lama dikemas kini (menjaga reka bentuk yang ditolak). 14/14 mutan.
- **Silap Lucy (ditangkap mutan):** ujian "tiada input dlm modal" menghiris dgn penanda hujung-baris pada fail CRLF ⇒ hirisan kosong ⇒ hijau palsu. Dibetulkan + dicatat `feedback_tetingkap_ujian_sumber` varian ke-3.
- **Telefon "terpotong" = ARTIFAK** (Edge headless ≥500px, sudah dlm laporan T7). Diukur 390px sebenar (iframe+postMessage): scrollWidth 375/390, tiada limpah; modal 16–374px.
- Tangkapan: `%TEMP%\opr-kelas\papar-{desktop,desktop-modal,ipad,ipad-modal,telefon,telefon-modal}.png`; penjana `jana-papar.js` EKSTRAK kod sebenar (bukan salinan templat).
- Komit `3b04b4c` (tempatan). Disimpan: opr-insaniah/MEMORY.md, auto-memory (project + feedback + index), relationship-memory, fail ini.
- **Sambung:** master tengok tangkapan baharu → (pilihan) `code-review` atas `3b04b4c` — final review opus lama SEBELUM perubahan Papar → "ya" deploy → T9 (clasp push → migrasiKelas() x2 → create-deployment → ujian peranti 5 item, sebut PERANTI). Merge/push branch = keputusan master.

### Sesi malam 2026-10-05 19:46–20:24 (EXECUTE plan KELAS)
- Master "mula execute, subagent driven, autonomous" → branch `medan-kelas` dari `master` @`48a01cd`. T1–T8 siap, per-task review (sonnet) + final review (opus) BERSIH. Suite 608→676/0. Komit akhir `5ab0b03`. TIDAK push/deploy/merge.
- T6 & T7: implementer berhenti/menanda gerbang "master tengok"; controller TANGGUH (komit tempatan) — master WAJIB tengok tangkapan skrin di `%TEMP%opr-kelas` sebelum T9. T7 fix round 1: cip Elemen/Nilai pecah perkataan → had lebar blok cip Kelas; baris 18 kelas ~450px (diterima).
- Implementer T8 memadam blok MEMORY lebih luas drpd brief (hilang KAUNTER reset/panduan/@48/XSS) → controller pulih. Dua minor palsu dibuang.
- Ledger + laporan: `opr-insaniah/.superpowers/sdd/2026-10-05-medan-kelas/` (gitignored). `opr-program` dapat komit `d341178` (drift salinan modal+chip), tempatan.
- **Sambung:** master tengok skrin + putus kad ringkas → "ya" deploy → T9 (clasp push → migrasiKelas() x2 → create-deployment → ujian peranti). Merge/push branch = keputusan master.

### Sesi malam 2026-10-05 19:11–19:45 (spec → plan)
- Master "Proceed tulis specs" → spec ditulis (baca kod dulu: Setup/Kod/Validate/Database/ReportService/Kongsi/app.js). Master "Setuju" → minta plan ditulis oleh **subagent opus** (arahan master, mengatasi cadangan aku tulis sendiri).
- Plan `docs/superpowers/plans/2026-10-05-medan-kelas.md`: 9 task (T1 struktur+migrasi · T2 validasi · T3 pelayan · T4 tapis · T5 borang modal+chip · T6 PDF · T7 jadual+penapis · T8 dokumen+jejak · T9 deploy GERBANG MASTER). T6/T7 berhenti utk master tengok.
- Gotcha jumpa: fungsi `_` tak muncul dlm menu Run editor ⇒ `migrasiKelas()` awam + `ALLOW` 13→14 + gerbang pemilik; `isiBorang` jangan guna `tambahKotak` utk KELAS; lajur ke-7 menganjak `nth-child`.
- "Ikut syor": subagent-driven; Hantar tak dimatikan di client; Kelas disorok lalai pada kad. Aku BELUM baca plan baris-demi-baris — hanya semak ringkas.
- **Sambung:** tunggu "mula execute" → `superpowers:subagent-driven-development` atas plan. JANGAN mula/deploy sebelum itu.

### Sesi malam 2026-10-05 (brainstorming KELAS)
- Master: reset `KAUNTER` ke 0001 (belum dibuat; mesti padam baris ujian + fail Drive yatim dulu), panduan guru ditangguh.
- KELAS: 18 kelas (T1–3 Delima/Nilam/Zamrud; T4–6 Delima/Topaz/Zamrud), wajib min. 1, BERBILANG (master ubah drpd "satu" selepas lihat pilihan),
  PDF+jadual+penapis, panel admin. UI = modal+chip macam "Anjuran" `opr-program` (master sebut "checkbox anjuran" — aku grep 0 padanan dlm opr-insaniah, TANYA
  bukan teka; rupanya projek lain). Salinan, tanpa "Tambah". `RUJUKAN` `JENIS=KELAS` ⇒ 0 bacaan Sheet tambahan; lajur `KELAS` di hujung.
- Classify: architectural → Hirisan A (medan) dulu, Hirisan B (panel admin) kemudian. Sambung: spec → master baca → plan (subagent opus) → kod.
- Corak sesi: master UBAH keputusan selepas melihat pilihan konkrit (satu→berbilang kelas); sebut rujukan projek lain tanpa nama projek → grep + tanya.

## Working Memory

### Sesi ni (Isnin 2026-10-05, 12:05–13:48)
1. Baca fail Lucy + `CLAUDE.md` → `MEMORY.md` → `git log` (sepadan, tiada drift). Master: "Proceed 123".
2. **`@46`** deploy (fix `.sorok` `4f79e15`). Suite 560/0, `list-versions` sahkan.
3. **Langkah 3 (lazy-load pustaka PDF) DITUTUP selepas UKUR:** deployment ukur sementara `@47` (penanda masa, kod TIDAK dikomit, pulih `cp`).
   Honor 50: urai pustaka 44–46 ms, `sesi_siap` 2.8–3.8 s. Laptop: 25 ms, 3.7–4.2 s. Kesesakan = PELAYAN `mulakanSesi()`, bukan pustaka.
   `@47` dipadam. Andaian "82% HTML = pustaka" benar utk SAIZ, bukan MASA.
4. `docs/launch-checklist.md` ditulis (7 ancaman dipetakan ke Apps Script). Temui isu: pelayan percaya `mime` client, tiada had saiz.
5. Master "Ok ikut syor" (logo PNG/JPEG, had 15/5/2 MB) → **pagar `af0f43f`**: `validasiFailDataUri`/`validasiFailLaporan` dlm Validate.gs, dipasang
   SEBELUM tulis di 4 titik masuk. 22+12 ujian, 17 mutan dibunuh. **Plan saya SALAH** ("gambar sentiasa JPEG daripada client") — `kecilkanImej` hantar
   fail ASAL bila ≤800px; dikesan SEBELUM commit dgn baca laluan client. GAMBAR kini JPEG+PNG.
6. Master "ikut syor" → **`2830e40`** client tukar WebP/GIF/BMP → JPEG (saiz asal, tak dibesarkan) + lapik PUTIH (canvas JPEG jadikan lutsinar HITAM;
   logo PNG lutsinar >800px dahulu berlatar hitam — pembetulan sampingan, DISEBUT). Ujian stub DOM 12, 8 mutan. Deploy **`@48`**.
7. Disimpan: `opr-insaniah/MEMORY.md`, `docs/launch-checklist.md`, auto-memory, `relationship-memory.md`, fail ini.

### Pattern berkesan (rujukan sesi depan)
- **Jejak nilai yang dihantar ke SUMBER client sebelum jadikan andaian plan** — "gambar sentiasa JPEG" tertangkap hanya kerana saya baca `kecilkanImej`.
  Baca client SEBELUM menulis jadual peraturan pelayan.
- **Ujian mesti menyemak TERTIB, bukan kewujudan** — komen kata "isi sebelum lukis" tapi ujian tak semak; mutan tertib-terbalik terselamat potensi.
- **Ukur MASA bukan SAIZ** sebelum optimum. Penanda masa pada deployment sementara berasingan (URL lain), pulih dgn `cp`, padam lepas ukur.
- **Fix yang menukar tingkah laku (lapik putih) → sebut**, walaupun ia pembetulan. Corak [[feedback_fix_yang_buang_keupayaan]].
- Tulis skrip mutasi ke fail (alat Write) — pulih dari bait asal dlm `finally`, sahkan `Get-FileHash`.

## Session Recap (For AI Restart)
- `opr-insaniah`: guru `@48`. **Tertunggak:** (a) master uji `@48` di peranti (5 item: laporan 2 gambar JPEG+PDF · gambar kecil PNG · Edit tajuk sahaja ·
  Tetapan logo PNG lutsinar besar ⇒ latar putih · pilihan WebP/GIF kecil) — minta PERANTI; (b) `KAUNTER_<tahun>` reset atau biar; (c) panduan guru
  tulis/tangguh; (d) kesesakan pelayan 3–4 s belum disiasat (penanda dlm `mulakanSesi()`: TETAPAN/USERS/RUJUKAN/logo Drive) — boleh tunggu;
  (e) sahkan `cipSenarai/selPdf/selEdit/selPadam` escape (XSS) + `include()` dlm ALLOW pagar `dedah-global`; (f) launch rasmi = keputusan master.
  Jalan balik `@48`→`@46` (deploy semula versi 46 ke ID guru).
- 🔴 **Tertunggak projek LAIN (tidak disentuh):**
  - `celiksains` sub-projek 2 (auth email+OTP): Task 1–8 SIAP @ `b9695c1`; **Task 9** perlu master (`wrangler login` skop Email, izin `email sending enable`, 3 keputusan: had kadar, enumerasi/PDPA, UI login). Email Service = Workers Paid $5.
  - `mypwa-v2` pelan baiki kuota D1 (`GET /api/ujian` = 81% read): master baca pelan, luluskan T0 + jawab 3 soalan.
  - `opr-program`: menunggu "ya" master utk deployment UKUR sementara (kini ada kaedah terbukti dari opr-insaniah); smoke `@29` tertunggak.
  - `takwim-digital`: SETUP.md/PANDUAN-GURU.md belum sebut butang "Hantar Sekarang"; belum smoke di URL production.
- JANGAN deploy production / merge `main` tanpa izin jelas master (opr-insaniah: izin deploy kekal selagi BELUM ada guru — jangan pindah ke projek yg sudah ada guru).

---
*Session updated: 2026-10-07 12:45 (opr-insaniah: kod @61 siap — nilai sebenar + ikon SVG + padam laporan; deploy menunggu master di editor). Sebelum itu: 2026-10-06 22:15 (fitur gambar SELESAI)*
