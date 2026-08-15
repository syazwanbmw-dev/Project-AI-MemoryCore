# Reminders — Lucy & Master
*Persistent cross-session. Jangan delete — pindah ke Selesai bila siap.*

---

## Terbuka

- **`opr-insaniah` — sambung Fasa 1 HIRISAN 2 di TASK 9** *(dikemas 2026-08-15 10:1x)*
  🟢 **Task 1–8 SIAP.** `fasa1/hirisan-2` @ `3e16bb2` · 15 commit · suite **68/68/0**.
  Backend lengkap: `Utils.gs` · `Validate.gs` · kolum ke-17 · `DriveService.gs` · `Database.gs` ·
  setup logo · `ReportService.gs` · `mulakanSesi`.
  ⏳ **Tinggal Task 9** (`form.html` + nod A4) · **Task 10** (`app.js.html` — resize 800px,
  gerbang muat 1 muka, jana PDF, hantar) · **Task 11** (penerimaan master).
  🔴 **Task 9 ke atas perlu `clasp push` + `create-deployment` ke web app HIDUP** — gerbang
  master. Kod Task 9–10 boleh ditulis tanpa deploy; hanya pengesahannya perlu deploy.
  🔴 **URUTAN:** kod → `clasp push` → `create-deployment --deploymentId` → **baru** migrasi
  Sheet 16→17. Terbalik ⇒ setup tulis semula 16 di atas 16 sambil lapor *"Selesai"*.
  🔑 Keputusan **#19** (resize 800px) masih **belum wujud dalam kod** — ia mendarat di Task 10.
  🔑 Kontrak wiring Task 10 sudah ditulis (pelan tertinggal 7 bahagian kod):
  `.superpowers/sdd/2026-08-14-opr-insaniah-fasa1-hirisan2/task-10-wiring-contract.md`

- **`opr-insaniah` — DUA soalan menunggu keputusan master** *(dibuka 2026-08-15)*
  1. **`GAMBAR` wajib atau pilihan?** Spec §8 tanda wajib; kod laksana pilihan. Spec §13 #2
     sendiri gelar ia *"satu baris untuk dilonggarkan"*. Murah kedua-dua arah.
  2. **Kos muat halaman belum diukur** — `mulakanSesi()` ambil blob Drive logo setiap muat.
     ➡️ **PERHATIKAN masa muat semasa ujian penerimaan Task 11**, jangan buru dari awal.
  🟡 Backlog kekal: #4 gerbang muat frontend sahaja · #8 tab `Sheet1` · #9 `papahRalat()` tak
  sorok `skrinUtama`. Semuanya kosmetik atau diterima sedar.
  🟡 Lima perkara di-park dengan sebab penuh dalam `opr-insaniah/MEMORY.md`.

---

## Selesai

- ✅ **`opr-insaniah` — ujian penerimaan Hirisan 1 TAMAT 14/14** *(ditutup 2026-08-14 pagi)*
  Langkah 10 · 11a-c · 12 · 14 · 15 diperhatikan master pada Sheet + Drive + sesi Google sebenar.
  `esc()`, peta header dan pagar `ACTIVE` akhirnya **dilihat berjalan DAN dilihat menolak**.
  Susulan siap semua: `Spike.html` dipadam (izin master) · label `Akses ditolak` · 5 drift
  spec/`CLAUDE.md` ditutup · merge `--no-ff` ke `master` @ `940e3f3`.
