# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: `opr-insaniah` — sesi 2026-10-05 22:41 → 2026-10-06 00:05: code-review `3b04b4c` → fix → T9 deploy → ujian peranti → gabung master. (Sesi lebih awal: lihat bawah.)
**Current Project**: `opr-insaniah` (Apps Script terikat Sheet, DELIMa)
**Status**: 🟢 **Medan KELAS Hirisan A SIAP: guru `@49` LIVE, ujian peranti 7 item LULUS (master), `master` @`c5b28b7` (merge --no-ff `medan-kelas`, suite 691/0, dipush).** Tiada tertunggak untuk KELAS. Ujian peranti `@48` lama (5 item) tidak lagi relevan — `@49` menggantikan.

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
*Session updated: 2026-10-05 22:35 (opr-insaniah KELAS: butang Papar + modal @`3b04b4c`, 689/0, belum deploy). Sebelum itu: 2026-10-05 20:24 (execute plan KELAS T1–T8)*