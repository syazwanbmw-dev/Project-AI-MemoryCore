# Reminders — Lucy & Master
*Persistent cross-session. Jangan delete — pindah ke Selesai bila siap.*

---

## Terbuka

- **`opr-insaniah` — FASA 2 sedang berjalan (buka semula PDF)** *(dikemas 2026-08-16 pagi)*
  ✅ Dokumen SIAP: spec §2.5/§2.6, §7.3 ditulis semula, `CLAUDE.md` dipinda.
  ⏭️ **Seterusnya ikut urutan:** (1) spike `setSharing(DOMAIN_WITH_LINK)` pada DELIMa —
  spec §13 **#10** · (2) kod (`dapatkanPautanPdf`, butang per baris, senarai terkini di atas) ·
  (3) 🔴 **BUANG `SpikeBacaPdf.gs` + `SpikePdf.html` + baris `include` SEBELUM sebarang
  `create-deployment`** — kalau tidak kod spike sampai kepada guru · (4) deploy + penerimaan.
  🟡 **Keputusan #29/#30 BERSYARAT** — sah kerana app ini seorang pengguna sahaja. Guru **kedua**
  masuk `USERS` ⇒ tengok semula sebelum diaktifkan.

- **`opr-insaniah` — 🧹 PULIHKAN `OPR-2026-0001`** *(diputus master 2026-08-16 pagi)*
  Data ujian penerimaan memakan nombor pertama. Padam: baris `OPR` · PDF + gambar dalam Drive ·
  kunci **`KAUNTER_2026`** dalam `TETAPAN` (`naikkanKaunter()` baca `|| 0`, jadi kunci yang tiada
  bermula semula pada `0001`). Runbook penuh dalam `opr-insaniah/MEMORY.md`.

- **`opr-insaniah` — dua item DIPERHATI, bukan dikejar**
  1. **Kos muat halaman belum diukur** — `mulakanSesi()` ambil blob Drive logo setiap muat +
     ~550 KB pustaka inline (spec §13 **#11**). Belum jadi aduan guru.
  2. 🟡 Backlog kosmetik: #4 gerbang muat frontend sahaja · #8 tab `Sheet1` · #9 `papahRalat()`
     tak sorok `skrinUtama`. Semuanya diterima secara sedar.

---

## Selesai

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
