# Reminders — Lucy & Master
*Persistent cross-session. Jangan delete — pindah ke Selesai bila siap.*

---

## Terbuka

- **`opr-insaniah` — FASA 2b (Edit + Padam) belum dirancang** *(dibuka 2026-08-16)*
  🔴 **RANJAU SUDAH TERPASANG:** sheet `OPR` ada **TIGA** kolum tarikh — `TARIKH`, `TIMESTAMP`,
  `UPDATED_AT`. `dapatkanLaporan(id)` memulangkan baris **PENUH** ⇒ dua yang belum dibalut akan
  membunuh `google.script.run` **senyap**, sama seperti pepijat 16 Ogos. Guna `tarikhSheetKeTeks()`
  pada **ketiga-tiganya**.
  🔴 §7.5 (ganti PDF) mesti **pasang semula perkongsian** pada fail gantian — File ID berubah,
  pautan lama mati. Gejalanya: *"tadi boleh buka, sekarang tak boleh"*.
  🟡 Keputusan #29/#30 **BERSYARAT** — guru **kedua** masuk `USERS` ⇒ tengok semula.

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
