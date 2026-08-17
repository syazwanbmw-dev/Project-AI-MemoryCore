# Reminders — Lucy & Master
*Persistent cross-session. Jangan delete — pindah ke Selesai bila siap.*

---

## Terbuka

- **`opr-insaniah` — SAHKAN kaunter selepas pembersihan** *(dibuka 2026-08-17 12:4x)*
  🔴 Master bersihkan sheet `OPR` + folder `PDF/` + `GAMBAR/` selepas ujian penerimaan. Tetapi
  kaunter **tidak** dikira daripada baris — ia hidup dalam sheet **`TETAPAN`** sebagai kunci
  **`KAUNTER_2026`**, dan `naikkanKaunter()` baca `|| 0` (`Database.gs:175`). Nilainya **3**.
  **Tanpa memadam kunci itu, laporan seterusnya keluar `OPR-2026-0004`, bukan `0001`.**
  ✅ **Gerbang pengesahan — satu langkah, tiada semakan lain perlu:** hantar laporan seterusnya dan
  lihat nombornya. `OPR-2026-0001` ⇒ bersih. `OPR-2026-0004` ⇒ kunci masih ada.
  🔴 Urutan: padam kunci **SELEPAS** baris `OPR`, bukan sebelum — kalau laporan masuk antara dua
  langkah, kaunter kosong menjana `OPR-2026-0001` **kedua** dan menulis ganti PDF sedia ada.
  ⚠️ Jangan keliru: ada **folder Drive** juga bernama `TETAPAN` (tempat logo). Kaunter dalam **sheet**.
  🟡 Belum disahkan juga: *"skrin melompat sendiri ke atas"* semasa borang Edit dibuka (bahagian
  soalan 4 yang master tidak sebut). Kosmetik — semak bila master guna sistem lain kali.

- **`opr-insaniah` — KEPERLUAN Fasa 2c, dalam ayat master sendiri** *(2026-08-17)*
  *"aku harap padam melalui sistem akan padam sekali gambar dan pdf"* — dan ini **DIPERHATI**,
  bukan diteori: memadam baris melalui sheet meninggalkan PDF + gambar **yatim** dalam Drive,
  kerana tiada kod berjalan dan `padamFail()` tidak pernah dicetuskan. Fasa 2c mesti membuang
  baris **DAN** fail serentak, dan padam separuh mesti tidak mungkin.

- 🔴 **DAKWAAN LAMA DIBATALKAN 2026-08-17: `OPR-2026-0001` BUKAN laporan sebenar sekolah.**
  Memory kita mencatatnya berulang kali sebagai *"laporan SEBENAR pertama sekolah (latihan hoki)"*
  dan meraikan ia *"ditebus"* pada 16 Ogos. Master sahkan ia **dummy juga**. Sistem ini **belum
  pernah** memegang satu pun laporan sebenar. ➡️ Andaian (*"ada dalam sheet ⇒ tentu guru hantar"*)
  mengeras menjadi "fakta" melalui pengulangan; tiada catatan pernah menyebut **siapa** yang
  mengesahkannya.

- ~~**`opr-insaniah` — FASA 2b (Edit): PELAN SIAP, menunggu TIGA jawapan master**~~ ✅ **SELESAI
  2026-08-17** — ketiga-tiga dijawab (#37 dan #38 diluluskan, Subagent-Driven), 13 task siap,
  suite **123 → 164**, deploy **`@23`**, ujian penerimaan **13/13** disahkan mata master.
  *(butiran asal dikekalkan di bawah sebagai rekod)* &#x20;
  Pelan `69f2912` — `docs/superpowers/plans/2026-08-16-opr-insaniah-fasa2b-edit.md`, **13 task**,
  suite dijangka **123 → 164**. Spec `6a341cd` (§2.7 `#31`–`#36`) disahkan master 16:1x.
  🔴 **JANGAN mula Task 1 sebelum tiga soalan ini dijawab** — dua yang pertama mengubah Task 1/3/7:
  1. **`#37`** — `dapatkanLaporan()` pulangkan **medan borang sahaja**, bukan baris penuh?
     (balut tiga tarikh = *pertahanan*; senarai tertutup = masalah **dipadam**)
  2. **`#38`** — **kemas kini baris SEBELUM padam fail lama**? (urutan spec ada tetingkap di mana
     baris menunjuk fail dalam sampah ⇒ `PDF_HILANG_DRIVE`, tiada laluan pemulihan)
  3. Cara laksana: **Subagent-Driven** (disyorkan) atau **Inline**?
  ⏭️ Selepas dijawab: **Task 0 spike** (ukur muatan gambar arah server→client — tidak pernah
  diukur; yang diukur 13 Ogos ialah arah bertentangan), kemudian **Task 1 dokumen dahulu**.
  🔴 Kegagalan paling bahaya, dikunci mutasi Task 10: `SESI.modEdit` tak reset ⇒ *+ Laporan Baharu*
  seterusnya **menulis ganti** laporan yang baru diedit, dengan banner hijau.
  🔴 **RANJAU KEKAL:** sheet `OPR` ada **TIGA** kolum tarikh — `TARIKH`, `TIMESTAMP`,
  `UPDATED_AT`. `dapatkanLaporan(id)` memulangkan baris **PENUH** ⇒ dua yang belum dibalut akan
  membunuh `google.script.run` **senyap**, sama seperti pepijat 16 Ogos. Guna `tarikhSheetKeTeks()`
  pada **ketiga-tiganya**.
  🔴 Juga: `tamatHantar()` **SEBELUM** `tutupBorang()` · `naikkanKaunter()` **tidak boleh** muncul
  dalam `kemasKiniLaporan()` · `TIMESTAMP` tidak disentuh.
  ✅ ~~§7.5 mesti pasang semula perkongsian pada fail gantian~~ **DIBATALKAN 2026-08-16 petang** —
  amaran itu ditulis daripada membaca **spec**, bukan **kod**. `dapatkanPautanPdf()` baca
  `PDF_FILE_ID` pada masa klik dan pasang perkongsian **secara malas** di situ juga ⇒ sudah
  berfungsi tanpa kod tambahan. Yang betul-betul pecah hanya pautan yang sudah **disalin keluar**
  (keputusan #32: dibiar mati, pautan baharu dipapar).
  🟡 Keputusan #29/#30/#32/#36 **BERSYARAT** — guru **kedua** masuk `USERS` ⇒ tengok semula.

- **`opr-insaniah` — pagar kolum `USERS`: DIPERHATI ✅, tetapi masih belum di-deploy**
  *(dibuka 2026-08-16 petang · separuh ditutup 2026-08-17 09:5x)*
  `03061cb` · ✅ **Diperhati pada sistem SEBENAR 2026-08-17** melalui URL `/dev` (HEAD): header
  `USERS!A1` dirosakkan, mesej keluar seperti dijangka, `A1` dibetulkan semula — disahkan master.
  🔑 Ia diuji pada HEAD **tanpa menyentuh `@22`**: `clasp push` menulis ke HEAD, deployment
  bernombor menghidangkan snapshotnya sendiri. Corak makmal percuma yang sama seperti spike.
  ✅ **HIDUP 2026-08-17 12:0x** — dibundel dengan Fasa 2b dalam deploy **`@23`** pada ID yang sama,
  seperti master pilih. Pagar itu kini melindungi guru, bukan hanya HEAD. **Item ini DITUTUP.**

- **`opr-insaniah` — dua item DIPERHATI, bukan dikejar**
  1. **Kos muat halaman belum diukur** — `mulakanSesi()` ambil blob Drive logo setiap muat +
     ~550 KB pustaka inline (spec §13 **#11**). Belum jadi aduan guru.
  2. 🟡 Backlog kosmetik: #4 gerbang muat frontend sahaja · #8 tab `Sheet1` · #9 `papahRalat()`
     tak sorok `skrinUtama`. Semuanya diterima secara sedar.

---

## Selesai

- ✅ **`opr-insaniah` — FASA 2 TAMAT** *(ditutup 2026-08-16 11:4x)*
  Merge `master` @ `12d1361` · deploy **`@22`** · suite **114/114/0** · `sight-hone` CLEAR.
  Guru boleh buka semula PDF melalui pautan Drive.
  ✅ `OPR-2026-0001` **ditebus** — laporan sebenar pertama sekolah kini sah memegang nombor itu.
  🔴 Sepanjang jalan ditemui pepijat **lima hari**: objek `Date` membunuh `google.script.run`
  secara senyap ⇒ jadual senarai **tidak pernah** dipapar sejak Fasa 1. Hanya laporan **sebenar
  pertama** boleh menemuinya.

- ✅ **`opr-insaniah` — FASA 1 TAMAT** *(ditutup 2026-08-15 22:4x)*
  Merge `--no-ff` ke `master` @ `4a36a44` (47 commit) · suite **102/102/0** · deploy **`@20`**
  disahkan mata **4/4** oleh master.
  🔴 Pepijat terakhir ditemui **master dengan MENGGUNAKAN sistem**, bukan oleh 102 ujian: borang
  tiada jalan KELUAR ⇒ senarai yang sudah siap kekal tidak dapat dicapai.
  ✅ Soalan `GAMBAR` wajib/pilihan **DITUTUP** — kekal **WAJIB, minimum 1** (keputusan master
  2026-08-15): laporan tanpa gambar bukan bukti program itu berlaku.

- ✅ **`opr-insaniah` — ujian penerimaan Hirisan 1 TAMAT 14/14** *(ditutup 2026-08-14 pagi)*
  Langkah 10 · 11a-c · 12 · 14 · 15 diperhatikan master pada Sheet + Drive + sesi Google sebenar.
  `esc()`, peta header dan pagar `ACTIVE` akhirnya **dilihat berjalan DAN dilihat menolak**.
  Susulan siap semua: `Spike.html` dipadam (izin master) · label `Akses ditolak` · 5 drift
  spec/`CLAUDE.md` ditutup · merge `--no-ff` ke `master` @ `940e3f3`.
