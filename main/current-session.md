# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: `opr-insaniah` — sesi petang/malam 2026-10-05 (16:47–19:1x): brainstorming medan KELAS. (Sesi awal hari: deploy `@46`→`@48`, lihat bawah.)
**Current Project**: `opr-insaniah` (Apps Script terikat Sheet, DELIMa)
**Status**: 🟡 Guru **`@48`** (tiada kod baharu sesi ini). `master` == `origin/master` @ `2be1489`. Suite 608/0. **Reka bentuk Hirisan A medan KELAS dibentang — MENUNGGU "setuju" master.** Ujian peranti `@48` masih menunggu.

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
*Session updated: 2026-10-05 13:48 (opr-insaniah `@48`: pagar jenis+saiz fail + kecilkanImej; Langkah 3 ditutup). Sebelum itu: 2026-10-05 (rangka muat + onload + C1 `@45`)*
