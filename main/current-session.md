# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-15 12:57 → 22:4x — 🎉 **`opr-insaniah` FASA 1 TAMAT, DI-MERGE.**
`master` @ **`4a36a44`** (merge `--no-ff`, 47 commit) · suite **102/102/0** · deploy **`@20`**
pada ID kekal, **disahkan mata 4/4** oleh master.
**Last Work Activity**: 2026-08-15 (~22:4x — merge selesai, memory dikemas)
🔵 **mypwa-v2 TIDAK disentuh** — blok sesi 2026-08-10 kekal utuh di bawah.

### 🔴 PEPIJAT TERBESAR MALAM INI DITEMUI **MASTER**, BUKAN 102 UJIAN

Master: *"Senarai laporan tak pernah dipapar selepas laporan dihantar. Aku ingat akan dibuat pada
fasa lain."* — **ia sudah siap sejak Hirisan 1.** `senaraiLaporan()` + `muatSenarai()` berfungsi
penuh dan senarai **memang** dimuat semula selepas setiap hantar. Tetapi kad senarai duduk
**~1,200px di ATAS** butang Hantar (pratonton A4 sahaja 1123px) dan **tiada butang menutup borang**.

🔑 **Tiada ujian boleh menangkapnya:** suite menguji fungsi; tiada satu pun tahu **di mana skrin
pengguna sedang berada**. Setiap semakan *"ciri itu ada?"* menjawab **YA** — yang hilang ialah
**laluan manusia sampai kepadanya**.
🔑 **Master melaporkannya sebagai soalan SKOP, bukan pepijat.** Jawapan yang salah — *"ya, itu Fasa
2"* — akan menguburkan ciri yang sudah siap. ➡️ Bila master kata sesuatu *"belum dibina"*,
**semak kod dahulu**. → [[feedback_ujian_buta_skrin]] (BARU)

### 🔴 TIGA PEPIJAT, SATU TEMPAT: selepas guru tekan HANTAR

1. Borang tiada jalan keluar (atas) — fix: butang **Batal** + `tutupBorang()` + banner hijau
2. **`"Belum ada laporan."` tidak pernah disorok** — *toggle SEBELAH*: setiap cabang hanya
   **MENUNJUK**, tiada satu pun menyorok yang lain. Menggigit tepat pada laluan guna **KALI
   PERTAMA setiap sekolah** — laluan yang ujian tak pernah lalui sebab tapak ujian sentiasa
   **sudah ada baris**. → [[feedback_toggle_sebelah]] (BARU)
3. Nilai tersuai berulang kini **DITANDAKAN**, bukan ditolak (keputusan master). Satu mesej yang
   melayan **dua** sebab dipisahkan; kotak taip kosong **tidak buat apa-apa** (keputusan master)

🔴 **URUTAN WAJIB: `tamatHantar()` SEBELUM `tutupBorang()`.** Pada borang `display:none`,
`nodA4.offsetWidth` = **0** dan `kiraDimensiPdf()` **campak** — **DI DALAM** `withSuccessHandler`,
jadi `sedangHantar` tak pernah ditutup dan butang Hantar **MATI kekal**. Laporan sudah selamat
tersimpan ⇒ **tiada apa dalam sistem yang mengadu**. Dikunci ujian (mutasi M2).

### 🟢 MUTASI M5 MENDEDAHKAN UJIAN **AKU SENDIRI** LONGGAR

`\.jaya\b` **PADAN** dengan `.jaya-header` — `-` ialah **sempadan perkataan**. Ujian kekal HIJAU
sepanjang rule dinamakan semula dan `#senaraiStatus` tidak bergaya. Ditukar kepada padanan **SET
token penuh**.
🔑 Mutasi bukan untuk cari pepijat dalam **kod** — ia untuk cari **ujian yang berpura-pura menjaga**.
Tanpa M5 aku akan lapor *"5 pagar baharu, 102 hijau"* sedangkan satu daripadanya kosong.

### 🟢 PENGESAHAN `@20` — 4/4, dan TIGA daripadanya PERCUMA

🔑 **Butang Batal memanggil `tutupBorang()` yang SAMA PERSIS dengan laluan hantar-berjaya** ⇒
aliran tutup + tatal diuji **tanpa menulis satu baris pun ke Sheet**. Hanya soalan ke-4 berbayar.
🟢 **`window.scrollTo(0,0)` MEMANG berkesan dalam iframe Apps Script** — satu-satunya bahagian
yang aku tak dapat ramal, kini fakta yang **diperhatikan**. Fasa 2 akan perlukan corak sama.
🟢 Soalan 4 membuktikan sesuatu yang **tidak dirancang**: `senaraiLaporan()` + `esc()` + peta
header berjalan pada baris **SEBENAR pertama** sistem ini.

### 🟡 `git merge` TIDAK terima `-F -` (stdin) — berbeza daripada `git commit`

`error: could not read file '-'`. Checkout **berlaku**, merge **tidak** ⇒ gejalanya berada pada
`master` dengan kod LAMA dan suite 38/38. ➡️ Tulis mesej ke **fail**, `git merge -F <fail>`.

### ⏭️ SAMBUNG — DUA perkara

1. 🧹 **BERSIH:** `OPR-2026-0001` dimakan ujian penerimaan. Padam baris `OPR` + PDF/gambar Drive +
   kunci **`KAUNTER_2026`** dalam `TETAPAN` (kunci tiada ⇒ `naikkanKaunter()` baca `|| 0` ⇒ mula
   semula `0001`). **Master belum putuskan** sama ada nak pulihkan atau biar `0002` jadi yang pertama
2. 🆕 **Fasa 2 (Edit + Padam)** — pelan **belum ditulis**. Fasa 3 = panel admin + cari/tapis, dan ia
   **gerbang pengedaran**: sebelum itu sekolah lain tak boleh tambah guru tanpa menyunting Sheet

---

## ~~Sesi 2026-08-15 petang (blok lama)~~ *(rekod sejarah)*

### ✅ PEMBERSIHAN SIAP · SEMUA DEPLOY DISAHKAN MATA (21:0x)

Sheet `OPR` kosong · fail Drive dipadam · `KAUNTER_2026` dipadam. **Sekolah mula bersih pada
`OPR-2026-0001`.** `@15` `@17` `@18` `@19` kesemuanya disahkan mata oleh master.

### ~~⏭️ SAMBUNG — review + merge sahaja~~ ✅ **SELESAI 2026-08-15 22:4x**

`sight-hone` (3 isu, semua dibaiki) → `safi` (1 fix, 2 advisory) → `convergence` **FULL** →
izin master → merge `--no-ff` ke `master` @ **`4a36a44`**.
🟢 Ramalan *"merge tidak menukar apa yang berjalan"* **disahkan** — `@20` sudah menjalankan kod
itu sebelum merge. Sebab merge kekal seperti yang ditulis: Fasa 2 tidak bercabang daripada pokok
tanpa borang/PDF/`DriveService`.
🔴 Tetapi review itu **bukan** yang menemui pepijat terbesar — **master yang menemuinya, dengan
menggunakan sistem.** Lihat blok atas.

### 🔑 PENGESAHAN VISUAL TIDAK PERLU MENGHANTAR LAPORAN — aku lambat sedar

Pratonton A4 pada skrin **ITULAH** PDF: `html2canvas` merakam nod `.a4` yang sama persis.
Soalan **susun atur** boleh dijawab dengan **memandang borang** — tanpa baris, tanpa fail Drive,
tanpa menaikkan kaunter. Empat daripada lima pusingan pengesahan malam ini mengotorkan sheet
**tanpa perlu**, dan setiap satu memaksa pembersihan tambahan.
➡️ Tanya **pratonton** untuk susun atur; simpan klik **Hantar** untuk menguji **laluan simpan**.

### 🟢 UJIAN PEMALSUAN `EMAIL_GURU` — LULUS

Borang hantar `penyerang@contoh.com`; yang mendarat = emel master. `AuthService.gs:75` guna
`Session.getActiveUser()` (**pelawat**), bukan `getEffectiveUser()` (pemilik).

🔴 **DELIMa SEKAT DevTools** — dan akaun bukan-DELIMa **bukan** jalan keluar: tersekat dua kali
**sebelum** konsol dicapai (`Anyone within domain` menolak pada peringkat platform; emel tiada
dalam `USERS`). Melonggarkan kepada *"Anyone"* memecahkan keputusan #1.
🟢 **Jalan betul: fungsi sementara dalam editor Apps Script.** Dan ia SELAMAT — `clasp push` tulis
ke **HEAD**, deployment bernombor hidangkan snapshotnya sendiri. Sifat yang biasanya menyusahkan
kita menjadi **pagar percuma**. ⚠️ Syaratnya: **jangan** `create-deployment` selagi fail ujian ada.

### 🔴 PEPIJAT KEHILANGAN DATA — ditemui master dengan menaip `mmmm` tanpa henti

Teks tanpa ruang melepasi tepi kanan A4 dan **dipotong oleh `overflow:hidden`** — hilang daripada
PDF tanpa ralat. 🔴 **Gerbang muat-satu-muka BUTA:** `semakMuat()` baca `scrollHeight` = **TINGGI**;
limpahan **MENDATAR** tidak menaikkan tinggi ⇒ gerbang lapor *"muat ✓"* sambil ayat guru lenyap.

🟢 `overflow-wrap:break-word` **memulangkan liputan gerbang**: limpahan mendatar bertukar jadi
baris **tambahan** (tinggi), yang gerbang memang pandai tangkap.
🔑 **Lolos SEMUA ujian sampai malam ini kerana setiap perenggan ujian kita di-PASTE** — teks paste
ada **ruang**. Ujian tidak lemah; ia tidak pernah menaip seperti manusia tergesa-gesa.
🔑 Tempat **kedua** (`MASA`, jadual meta) ditemui dengan **BERTANYA** *"di mana LAGI teks tanpa
ruang boleh masuk?"* — bukan menunggu ia pecah. `@19` = `table-layout:fixed` + `overflow-wrap`;
**kedua-duanya wajib** (satu menahan **bekas**, satu menahan **kandungan**).
🔴 Ertinya `18/30/20/32` **baru sekarang** boleh dipercayai — sebelum `@19` ia cuma cadangan, dan
**setiap ujian lebar tetap lulus** kerana ia baca **CSS**, bukan susun atur **terhasil**.

⚠️ `Set-Content -Encoding utf8` **merosakkan emoji** semasa mutasi. Guna alat `Edit`, atau
`Copy-Item` untuk salinan pulih.

### 🔑 SOALAN PENDAPAT vs SOALAN PEMERHATIAN — corak paling berguna malam ini

Deploy `@15`, master jana PDF 18 minit kemudian — **keluar versi LAMA** (tab pelayar basi).
Perubahan CSS tulen **tiada penanda kandungan**, jadi *"nampak sama"* dan *"tidak terpakai"* beri
skrin yang **serupa**.
🟢 Selesai dengan menukar *"nampak lain tak?"* (pendapat) kepada **"kotak `Hari` lebih lebar
daripada `Tarikh`?"** (pemerhatian) — kerana versi lama kunci **kedua-duanya 16%**, jadi jawapan
"ya" **mustahil** pada versi lama.
➡️ Untuk perubahan visual tanpa penanda, cari **hubungan yang TERBALIK** antara dua versi.

### ~~⏭️ SAMBUNG — SATU ujian + bersih~~ ✅ *(rekod sejarah — SELESAI 2026-08-15 malam)*

🔴 **Ujian pemalsuan `EMAIL_GURU`** — kod konsol siap-taip ada dalam `opr-insaniah/MEMORY.md`
blok STATUS SEMASA. Jangkaan `{ok:true}` + kolum `EMAIL_GURU` = **emel master**.
Kriteria bunuh: emel penyerang muncul ⇒ setiap pagar `bolehEdit`/`bolehPadam` **Fasa 2 sudah
runtuh sebelum ia ditulis**.

🧹 **Bersih:** padam `OPR-2026-0001` `0003` `0004` daripada sheet + Drive, **dan `KAUNTER_2026`
dalam `TETAPAN`** — disemak dalam kod: `naikkanKaunter()` baca `|| 0`, jadi kunci yang tiada
memberi laporan sebenar pertama `OPR-2026-0001`. Sekolah mula bersih.

⚠️ **JURANG BUKTI:** pengesahan visual `@14` belum dilaporkan. Master jalankan ujian kaunter
selepas deploy tanpa mengadu — itu **ketiadaan aduan, bukan pemerhatian**. Belum disahkan mata:
**"Elemen Teras"** satu baris atau pecah dua · lajur meta sejajar.

### ✅ SIAP SESI INI

| Perkara | Hasil |
|---|---|
| Task 10 | **SIAP** — 5 commit, 3 round fix, Spec ✅ / Quality Approved, **0 parked** |
| Suite | 84 → **96** |
| Deploy | `@12` → `@13` (label) → `@14` (lebar jadual), semua ID kekal |
| Task 11 | deploy · migrasi 16→17 · 4 gerbang · **kaunter naik-sahaja** — semua LULUS |

### 🔴 TIGA PENEMUAN — semuanya satu bentuk: DUA FAIL MESTI BERSETUJU, TIADA APA MERAPATKAN

1. **`index.html` tidak pernah `include()` pustaka PDF.** Wujud sejak Task 0, dimuat naik oleh
   setiap `clasp push` — jadi setiap semakan *"pustaka itu ada?"* menjawab **YA**. Yang tiada
   ialah **sambungan**. Bertahan Task 0→9. Kegagalan pertama = klik HANTAR pertama guru.
2. **`nilaiGerbang()` pulangkan kelas CSS yang `style.html` tiada peraturan untuknya.** Suite
   94/94 hijau sepanjang masa lencana tolak tidak bergaya.
3. **Lajur jadual A4 tidak muat label baharu.** Commit label semak **enam** tempat perkataan itu
   hidup, **sifar** tempat yang mesti **memuatkannya**. → [[feedback_teks_tukar_ruang]] (BARU)

🔴 **Punca #1 ialah aku.** Aku taip dalam dispatch subagent bahawa pustaka *"dimuatkan oleh
index.html"* **tanpa semak**, sambil menyemak brief warisan dengan teliti (9 percanggahan
dijumpai). **Brief ada gerbang; ayat yang aku karang sendiri tiada.**
🟢 Diselamatkan oleh satu ayat dalam dispatch yang sama: *"berhenti dan beritahu aku, jangan
teka"*. → [[feedback_konteks_dispatch_tak_disemak]] (BARU)

### 🔴 PDF PROJEK INI HANYA BOLEH DISAHKAN OLEH MATA — kekal

`pdftoppm` tiada · `pdftotext` pulangkan **kosong** — dan kosong itu **bukan kegagalan alat**:
PDF kita ialah satu imej JPEG daripada `html2canvas`. Memasang poppler **tidak** akan membantu.
➡️ **Minta master hantar tangkap layar.** Itulah yang menemui pepijat lebar lajur hari ini.
🟢 Tangkap layar itu juga membuktikan `19/08/2026` → **Rabu** ⇒ kembar `kiraHari` berfungsi
hujung-ke-hujung pada laluan sebenar.

### 🎯 DUA KEPUTUSAN MASTER

- **13:11 butang Hantar MATI bila gambar tiada.** Gerbang kini tolak atas TIGA sebab; dua
  daripadanya arahan **BERTENTANGAN**. Mesej salah **tidak campak ralat** — guru pendekkan teks
  selama-lamanya. Jadi `nilaiGerbang()` diletak dalam **`Kongsi.html`** (tulen, boleh diuji).
- **13:22 `kosongkanBorang()` masuk skop** — tanpanya guru klik *+ Laporan Baharu*, nampak data
  yang baru dihantar, tukar tajuk, hantar ⇒ **laporan KEDUA**.
- **16:11 label** *"Nilai Murni"* → **Nilai** · *"Elemen"* → **Elemen Teras** (6 tempat).

### 📊 KIRAAN TASK PROJEK — dijawab untuk master 12:5x

| Peringkat | Task | Status |
|---|---|---|
| Task 0 Spike | 5 (`0.1`–`0.5`) | ✅ |
| Fasa 1 Hirisan 1 | 7 | ✅ |
| Fasa 1 Hirisan 2 | 11 | 9 siap · **Task 10 berjalan** · Task 11 tinggal |
| **Dirancang** | **23** | **21 siap** |

Fasa 2 (Edit+Padam) dan Fasa 3 (panel admin + cari/tapis) **belum ada bilangan task** — pelan
sengaja belum ditulis. 🔴 Fasa 3 = **gerbang pengedaran**; sebelum itu sekolah lain tak boleh
tambah guru tanpa menyunting Sheet terus.

### 🔴 PENEMUAN BESAR 13:27 — PUSTAKA PDF TIDAK PERNAH DIMUATKAN

`LibHtml2canvas.html` + `LibJspdf.html` wujud sejak Task 0 dan dimuat naik oleh **setiap** `clasp
push`. Tetapi `index.html` cuma ada **empat** `include()`: `style` · `Kongsi` · `form` · `app.js`.
Bertahan **Task 0 → Task 9** tanpa dikesan. Klik HANTAR pertama guru ⇒ `html2canvas is not defined`.

🔑 Lebih sukar dilihat daripada `oauthScopes`: di sana nilainya **salah**; di sini setiap nilai
**betul** dan yang tiada ialah **sambungan**. Semakan *"pustaka itu ada?"* menjawab **YA**.
➡️ Soalan yang menangkapnya: **"apa yang MEMUATKANNYA?"**

🔴 **Puncanya aku.** Aku taip dalam dispatch bahawa pustaka *"dimuatkan oleh index.html"* **tanpa
semak** — sambil menyemak brief warisan dengan teliti (9 percanggahan dijumpai). Brief ada
gerbang; ayat yang aku karang sendiri **tiada**.
🟢 Diselamatkan oleh satu ayat dalam dispatch yang sama: *"berhenti dan beritahu aku, jangan teka"*.
→ [[feedback_konteks_dispatch_tak_disemak]] (BARU) · [[feedback_izin_bukan_peraturan]] (varian 2)

### ✅ DUA KEPUTUSAN MASTER SESI INI

1. **13:11 — butang Hantar MATI bila gambar tiada** (pilihan a). Gerbang kini tolak atas TIGA
   sebab; dua daripadanya arahan **BERTENTANGAN** (*"tambah gambar"* lawan *"pendekkan teks"*).
   Mesej salah **tidak campak ralat** — guru pendekkan teks selama-lamanya. Jadi `nilaiGerbang()`
   diletak dalam **`Kongsi.html`** (tulen, boleh diuji), bukan `app.js.html` (DOM).
   🔴 Gambar disemak **DAHULU** — gambar menaikkan `scrollHeight`, jadi tinggi akhir belum
   diketahui; menyuruh tala teks dahulu = tala terhadap sasaran bergerak.
2. **13:22 — `kosongkanBorang()` masuk skop Task 10** (pilihan A, dibentang berasingan).
   Tanpanya guru klik *+ Laporan Baharu*, nampak data yang baru dihantar, tukar tajuk, hantar ⇒
   **laporan KEDUA**. Ia sengaja **tidak** panggil `semakGerbang()` — itu akan cat lencana MERAH
   sebelah mesej *"Tersimpan: OPR-2026-0012"*.

### ⏭️ SAMBUNG SELEPAS SUBAGENT SIAP

Review package → task reviewer → fix loop → **Task 11**.

🔴 **URUTAN WAJIB Task 11:** tukar kod → `clasp push` → `create-deployment --deploymentId` →
**baru** migrasi Sheet 16→17. Terbalik ⇒ setup menulis semula 16 di atas 16 sambil melapor
*"Selesai"*.
🔴 **Kos muat halaman belum diukur** — dan kini ~550 KB pustaka ditambah inline. Perhati masa
muat semasa ujian penerimaan Task 11.

### ~~⏭️ SAMBUNG — Task 10~~ *(rekod sejarah — brief disemak semula 13:1x)*

Sepuluh percanggahan brief ditutup sebelum/semasa dispatch. Tiga paling berbahaya:
`git checkout` memadam kembar · pendengar **kedua** pada `btnBorangBaharu` · kontrak dilabel
"Step 0" tetapi memanggil fungsi Step 1. Ledger penuh:
`.superpowers/sdd/2026-08-14-opr-insaniah-fasa1-hirisan2/progress.md`

### ✅ KEPUTUSAN MASTER — **`GAMBAR` WAJIB, minimum 1** *(`1cefb86`)*

Soalan terbuka #1 **DITUTUP**. Sebab master: **laporan tanpa gambar bukan bukti program itu
berlaku.** Spec §8 tanda wajib; kod Task 7 laksana **pilihan** — dan aku pilih sendiri tanpa
tanya. Punca drift: satu ayat spec §13 #2, *"satu baris untuk dilonggarkan"*.

🔑 **Ayat itu ialah JEMPUTAN BERTULIS untuk membatalkan keputusan tanpa sedar ia satu keputusan.**
Bila dua ayat spec bercanggah, yang **paling longgar** menang — ia tidak perlukan sesiapa membuat
keputusan. → [[feedback_prosa_menjemput_pembatalan]] (BARU)

`validasiGambar()` + `PERATURAN_GAMBAR {min:1, maks:2}` dalam **`Validate.gs`** (bukan
`ReportService.gs` — fail itu sentuh Sheets/Drive/Lock, pagarnya **tak boleh diuji** di sana; sebab
sama dengan `pilihPengguna()` keputusan #21). Nota anti-undur bertarikh di **TIGA** tempat.
🔴 `GAMBAR_TIADA` ≠ `GAMBAR_TERLALU_BANYAK` — dua arahan **BERTENTANGAN** kepada guru.

### 🟢 TASK 9 — DISAHKAN HIDUP, dan DUA pindaan kepada brief

Master sahkan pada laptop: butang · borang muncul · **A4 bersebelahan** · tiada tatal mendatar.

Kedua-dua pindaan ditemui dengan **mengira lebar sebenar**, bukan mempercayai lakaran:
1. **`.bekas` 1000px → 1280px.** A4 `794px` + lajur borang perlu ~1154px; pada 1000px ia **tidak
   pernah** boleh muat. Brief sembunyikannya di sebalik `overflow:auto` ⇒ master menatal
   **mendatar** untuk lihat pratonton yang sepatutnya berkata *"inilah PDF anda"*.
2. **`flex-wrap:wrap`** — A4 tak boleh dikecutkan **dan** tak boleh diskalakan.

🔴 **`transform:scale()` DILARANG dan kini dipolis ujian.** Ia pembaikan paling **jelas** dan
paling **merosakkan**: html2canvas rakam apa yang ada pada skrin, moyang ditransformasi ⇒ kanvas
bersaiz **SALAH** ⇒ `kiraDimensiPdf()` kira mm daripada nombor salah ⇒ **gerbang muat-satu-muka
jadi PEMBOHONG**.

### 🟢 DIUKUR DALAM SANDBOX — penemuan paling bernilai hari ini

```
iframe=1536  bekas=1280  kad=1246  ada=1204  perlu=1154  (baki 50px)
nodA4 = 794 × 1123   ← DISAHKAN di dalam iframe Apps Script
```

🔴 Nombor terakhir **bebas** daripada soalan susun atur: `TOLERANSI_MM = 0.5` **ditala kepada**
lebihan `0.016mm` yang `794×1123` hasilkan. Kalau sandbox ubah saiz nod, gerbang jadi pembohong
**senyap** sampai PDF keluar terpotong. **Tidak pernah disemak dalam sandbox sebelum ini.**
🟢 Google **TIDAK** hadkan lebar iframe — kebimbangan asal aku tidak berasas.

### 🔑 "KEGAGALAN" PERTAMA IALAH PAGAR YANG BERFUNGSI — aku hampir tersilap

Master lapor *"A4 turun ke bawah"* = tepat kriteria bunuh aku. Aku catat ramalan **SALAH** dan mula
rancang pembetulan CSS. 🔴 Sebenarnya master buka pada **TELEFON mod desktop-site**.

Susun atur **tidak pernah rosak** — itu `flex-wrap` **berfungsi**, dan `flex-wrap` **tiada dalam
brief**. Menala CSS untuk "membaiki"nya akan **memecahkan laptop yang sedang berfungsi**.
🟢 Yang selamatkan: aku **enggan meneka** dan pasang pengukur dahulu. Nombornya serta-merta tidak
konsisten dengan "skrin gagal". → [[feedback_laporan_manual_peranti]] (BARU)

### 🟡 GOTCHA — `clasp list-deployments` boleh BASI

`create-deployment` lapor `@9`; `list-deployments` serta-merta selepasnya papar `@8` keterangan
**lama**. Larian kedua betul. ➡️ Sahkan dengan `npx clasp list-versions` (tak pernah basi) atau baca
**dua kali**. Bahayanya bukan kelewatan — ia **kesimpulan**: memburu deploy yang berjaya, atau
deploy semula dan naikkan versi tanpa sebab. → [[reference_clasp_gotcha]] (gotcha ke-5)

### 🔴 SATU SOALAN TERBUKA KEKAL

**Kos muat halaman belum diukur.** `mulakanSesi()` ambil blob Drive logo **tanpa syarat setiap
muat**. `Setup.gs:95-97` rekod keputusan lama yang **menjauhkan** Drive daripada laluan ini (6.3s
diukur). ➡️ **PERHATIKAN masa muat semasa ujian penerimaan Task 11.**

### 📦 9 COMMIT SESI INI

`1cefb86` GAMBAR wajib · `1fe16dd` memory · `a0b257d` **T9 form.html** · `b323a38` memory ·
`9de938a` pembuka butang · `1837b92` gotcha clasp · `aa8eff4` diagnostik · `fcab249` buang
diagnostik · `4230cf6` memory.

---

### ~~⏭️ SAMBUNG — Task 9~~ ✅ **SIAP 2026-08-15 12:1x** *(rekod sejarah)*

🔑 Ledger penuh + 16 ruling + kontrak wiring Task 10:
`.superpowers/sdd/2026-08-14-opr-insaniah-fasa1-hirisan2/` (gitignored, kekal di disk).

### 🔴 SATU CRITICAL DITEMUI DAN DIBAIKI — Lock 2 tanpa `catch`

`ciptaLaporanUntuk` Lock 2 ada `finally` tetapi **tiada `catch`**. Kalau `kemasKiniFailId` campak,
baris **sudah** ditulis dan PDF + gambar **sudah** dalam Drive — tetapi pengecualian terlepas
sampul `{ok,kunci,mesej}`. Guru nampak ralat mentah **tanpa ID**, hantar semula ⇒ **laporan KEDUA**.

🔑 **Pagar yang baik di satu lapisan MENDEDAHKAN lubang di lapisan atasnya.** Pencetusnya
dipertajam oleh pagar yang kita sendiri tambah dalam Task 5. Lubang itu **sudah sedia ada** —
cuma tersembunyi di sebalik kegagalan `getRange(baris, NaN)` yang lebih kelam-kabut.
→ [[feedback_pagar_dedah_lapisan_atas]] (BARU)

### 🔑 PENGAJARAN BESAR — mutasi boleh hasilkan kod yang MASIH BETUL

Mutasi pagar zon waktu yang **aku** arahkan kekal **HIJAU**. Puncanya bukan ujian lemah: menukar
`Date.UTC`+`getUTC*` ke waktu tempatan pada **kedua-dua** belah adalah **simetri** — offset batal,
jadi mutan itu **setara**, bukan rosak. Aku tukar API tanpa tukar **tingkah laku**.

➡️ Sebelum menyimpul *"ujian tak menangkapnya"*, tanya: **adakah versi bermutasi ini sebenarnya
SALAH?** Mutasi betul = **asimetri** (`getUTCDay()` → `getDay()` sahaja) ⇒ MERAH pada Midway.
🟢 Yang menyelamatkannya: arahan mutasi ditulis dengan **hijau dinyatakan sebagai keputusan yang
LEBIH penting**, jadi pelaksana melapor jujur dan bukan mereka fix untuk memaksa merah.
→ [[feedback_guard_mutation_test]] (dikemas)

### 🔴 PELAN ADA TIGA DEFEK — ditemui SEBELUM satu baris kod ditulis

1. **Mutasi Task 1 & 10 guna `git checkout` untuk pulih** — commit ialah langkah **selepasnya**,
   jadi ia memadam kerja task itu sendiri. Pelan menaakul **tracked**; sifat yang penting ialah
   **committed**. Task 2 betul hanya **secara kebetulan** (fail baharu ⇒ dua-dua sifat palsu).
2. **Task 10 panggil fungsi yang tiada task pun menulis** — `kutipBorang()`, `nilaiDipilih()`,
   `SESI`, seluruh wiring borang. Placeholder secara **KETIADAAN**, lebih sukar dilihat daripada
   `TODO`. Ditutup dengan `task-10-wiring-contract.md`.
3. **`node --check` tolak fail `.gs`** (`ERR_UNKNOWN_FILE_EXTENSION`).

### 🆕 DUA KEPUTUSAN MASTER 2026-08-14 malam *(calon spec #25/#26)*

- **Gambar >2 ⇒ TOLAK + kosongkan input + minta pilih semula**, bukan "ambil 2 pertama".
  Sebab master: *"pertama"* ikut susunan fail, bukan guru — gambar penting tercicir senyap.
- **Nilai tersuai ⇒ checkbox baharu yang terus bertanda**, bukan chip dengan ✕. Guru belajar
  **satu** mekanisme buang, bukan dua.

### 📦 15 COMMIT

`5a76f82` `075acfc` `7a2ab63` T1 · `fea150a` `ef56667` T2 · `e889864` T3 · `8051f55` `4eca6f0` T4 ·
`90a70e2` `9e1c2a4` T5 · `ca0f7ac` T6 · `49d2ee1` `faeea07` T7 · `857b40a` T8 · `3e16bb2` docs.

---

### ~~⏭️ SAMBUNG — laksana pelan Hirisan 2, mula Task 1~~ ✅ **TASK 1–8 SIAP** *(rekod sejarah)*

`docs/superpowers/plans/2026-08-14-opr-insaniah-fasa1-hirisan2.md`
Master pilih **Subagent-Driven** pada 2026-08-14 23:0x.

### 🔴 GOTCHA 10:0x — MIGRASI DIJALANKAN SEBELUM KOD, gejalanya "TIADA APA-APA BERLAKU"

Master padam `SETUP_SIAP` + Sediakan semula, jangka kolum jadi 17. **Kekal 16.**

`jalankanSetup()` **tidak menambah kolum** — ia menyalin `STRUKTUR[SHEET.OPR]` **daripada KOD**
ke baris 1. Task 3 belum dilaksana ⇒ senarai masih 16 nama ⇒ setup menulis semula 16 di atas 16.
**Prosedur betul; kod belum sampai.**

🔴 Setup tetap melapor **"Selesai"** — **mesej kejayaan bukan bukti**, sama seperti langkah 14
penerimaan Hirisan 1. Membacanya sebagai pepijat migrasi akan menyebabkan kita memburu
`sheetIkutNama()` atau kebenaran Drive — **dua tempat yang memang elok sepenuhnya**.
➡️ Urutan: tukar kod → `clasp push` → **`create-deployment --deploymentId`** → *baru* migrasi.
🟢 Larian itu tetap membuktikan sesuatu tanpa dirancang: nama sekolah **enggan** bertukar ·
`RUJUKAN` kekal 16 baris · tiada sheet berganda ⇒ gerbang benih disahkan **kali kedua**.

### 🆕 TIGA KEPUTUSAN — lubang dalam SPEC, bukan idea baharu (spec §2.4)

Ditemui dengan **membaca spec bersebelahan kod Hirisan 1 yang sudah hidup**, sebelum menulis kod.

- **#22 `GAMBAR_FILE_ID_2` kolum berasingan di HUJUNG (Q).** #18 benarkan 2 gambar tetapi model
  data ada **satu** ruang ⇒ gambar kedua **hilang senyap** semasa Edit. 🔴 Kedudukan HUJUNG wajib:
  `jalankanSetup()` menulis **seluruh** baris header, jadi menyisip di tengah menggeser header
  tetapi **bukan** data ⇒ header `EMAIL_GURU` atas data `NAMA_GURU`, tanpa satu ralat pun.
- **#23 kaunter `KAUNTER_<tahun>` dalam `TETAPAN`, NAIK SAHAJA.** Kaunter ikut bilangan baris
  **mengitar semula** nombor selepas Padam (Fasa 2) ⇒ ID bertembung, PDF lama **ditulis ganti**.
- **#24 logo dimuat naik dalam skrin Persediaan, PILIHAN.** Tutup drift #16.

🔑 **Master TOLAK syor aku** (`;` dipisah seperti `NILAI`) dan pilih dua kolum. Sebabnya sah: sheet
ini antara muka yang pentadbir sekolah lain **buka dan baca**. Aku menilai dari sudut **kod**;
master menilai dari sudut **orang yang membacanya**.

### 🔑 PEPIJAT DALAM PELAN — ditangkap oleh self-review pelan itu sendiri

Draf pertama Task 10 menyuruh **PINDAHKAN** `kiraHari` ke `Kongsi.html` (client perlukannya —
PDF dijana di **browser**). 🔴 `Kongsi.html` masuk frontend via `include()`; ia **TIDAK** wujud
dalam runtime `.gs` ⇒ `ReportService.gs` kehilangannya pada **runtime**.

**Ia akan lolos SETIAP pagar:** `node --check` lulus (pengenal tak-terisytihar = sintaks **sah**) ·
`node --test` lulus (tiada ujian sentuh `ReportService`) · kegagalan pertama = **klik HANTAR
pertama guru**. Kelas **sama** dengan `oauthScopes` tidur sejak hari pertama.
Dibetulkan jadi **kembar dipolis suite** (`tests/hari-kembar.test.js`, 8 tarikh).
➡️ **"Pindahkan fungsi ini" mesti sentiasa disusuli: fail itu berjalan di runtime yang MANA?**

### 🔑 SATU CORAK YANG BERKESAN PAGI INI — ramalan DAHULU, baru perhati

Lima langkah berbaki semuanya menguji **KEGAGALAN**, dan kegagalan mudah dirasionalkan selepas
melihatnya. Jadi setiap langkah ditulis sebagai **ramalan + kriteria bunuh SEBELUM** master
menyentuh Sheet, dan **kriteria bunuh dicatat sekali** dalam `MEMORY.md`.
➡️ Rekod *"lulus"* yang tidak menyatakan **apa yang akan kelihatan kalau ia gagal** tidak boleh
diperiksa semula kemudian — ia jadi dakwaan, bukan bukti.
🔑 Langkah 14 contoh terbaiknya: ia lulus dengan memerhati sesuatu yang **TIDAK berlaku** (nama
sekolah enggan bertukar). Setup yang rosak pun papar *"Selesai"* — mesej kejayaan bukan bukti.

### 🔴 DUA BACAAN KOD MENJIMATKAN PUSINGAN PENUH

Kedua-duanya ditemui dengan **membaca kod sebelum menulis runbook**, bukan semasa menguji:
1. Setup yang disambung akan **abaikan** nama sekolah baharu. Tanpa diramal, master akan baca itu
   sebagai pepijat dan kita buru sesuatu yang memang betul.
2. `text-transform:uppercase` ada pada **`th` sahaja** (`style.html:21`) — jadi huruf besar dalam
   jadual datang daripada Sheet. Percanggahan (`<b>Ujian</b>` kekal huruf kecil) **ditanya**, bukan
   diandaikan.

### ✅ SIAP SESI INI

| Perkara | Hasil |
|---|---|
| Penerimaan langkah 10 · 11a-c · 12 · 14 · 15 | **LULUS**, semua langkah pulih disahkan |
| `6812108` | label `Tidak dapat masuk` → **`Akses ditolak`** (diminta master) |
| `56b38d7` | **`Spike.html` dipadam** (izin master) — grep dulu, tiada rujukan hidup |
| `f0ef154` | **5 drift** spec/`CLAUDE.md` ditutup (direkod cuma 1) |
| `940e3f3` | merge `--no-ff` ke `master`, 16 commit |
| Deploy | `@8` pada ID kekal, **disahkan hidup** oleh master |

🟢 **Apps Script menyalurkan mesej `throw` pelayan sampai ke `withFailureHandler`** tanpa ditapis.
Menolak dengan **LANTANG** memang didengar. Tidak membatalkan sentinel `{berganda:true}` — di sana
kita mahu gagal **TERTUTUP**.

### ~~⏭️ SAMBUNG — **Fasa 1 Hirisan 2**~~ ✅ **PELAN DITULIS 2026-08-14 10:0x** *(rekod sejarah)*
Skop: borang · `Validate.gs` · `ReportService.gs` · **`DriveService.gs`** · PDF di browser ·
`bacaRujukan()` · `jana ID` · `kira HARI`. Kini pecah jadi **11 task** — lihat blok aktif di ATAS.
🔴 Keputusan **#19** (resize 800px) mendarat dalam **Task 10**, masih **belum wujud dalam kod**.

### ~~⏭️ SAMBUNG — ujian penerimaan LANGKAH 10~~ ✅ **SELESAI 2026-08-14, 14/14** *(rekod sejarah)*

Langkah **1–9 LULUS**. `1–3` diperhatikan 22:44 (skrin utama papar `SEKOLAH KEBANGSAAN SALOR` ·
`Pentadbir` · `ADMIN` · *"Belum ada laporan."*) ⇒ seluruh laluan `Setup`→`Database`→`AuthService`
→`Kod`→frontend berjalan hujung ke hujung. `4–9` diperhatikan **23:0x** (`⚠️ BACA DULU` tab
PERTAMA · 4 ELEMEN + 12 NILAI · `USERS` satu baris tanpa password · 16 header `OPR` beku ·
3 folder Drive kosong · muat semula → skrin utama). Commit memory `26705cc`.

🟡 **PENEMUAN: tab `Sheet1` tertinggal** — `Setup.gs` tidak pernah memadam tab lalai Google.
**Kosmetik sahaja** (bacaan ikut NAMA, bukan kedudukan); `BACA DULU` kekal pertama ⇒ mitigasi utuh.
Relevan kerana keputusan #13 (sekolah lain **salin fail**). Backlog #8, sengaja **tidak** dibaiki.

URL: `https://script.google.com/macros/s/AKfycbxss9BfkhH…/exec` (ID kekal, tidak pernah berubah)

✅ **Kesemua yang disenaraikan di bawah SUDAH diuji dan LULUS 2026-08-14 pagi.** Senarai dikekalkan
kerana ia merakam **kenapa** setiap satu bernilai — bukan sebagai kerja tertunggak:
- **10–11** `esc()` + peta header. Senarai kosong ⇒ kod pemapar baris, `esc()` dan peta header
  **dihantar tanpa pernah diperhatikan berjalan**. Perlu **satu baris palsu** dalam sheet `OPR`
  (`TAJUK`=`<b>Ujian</b>`, `NAMA_GURU`=`Cikgu & Rakan`), kemudian sisip kolum di hadapan `ID_OPR`
- **12** pagar `INACTIVE` — **satu-satunya** ujian pagar `ACTIVE` pada Sheet sebenar melalui sesi
  Google sebenar. 38 ujian `node --test` semuanya menguji objek yang **kita bina sendiri**
- **14** padam baris `SETUP_SIAP` → setup mesti **menyambung**, tiada sheet berganda *(fix #20)*
- **15** baris kedua `USERS` email sama → mesti `EMAIL_BERGANDA`, bukan "tidak aktif" *(fix #21)*

Kemudian: padam `Spike.html` (**perlu izin master**) → kemas spec §10 (`Code.gs`→`Kod.gs`) →
semakan akhir seluruh branch → merge ke `master`.

Ledger penuh: `opr-insaniah/.superpowers/sdd/2026-08-13-opr-insaniah-fasa1-hirisan1/progress.md`

### 📦 TUJUH TASK SIAP — commit

`d566eb9` T1 `Utils.gs`+pemuat ujian · `b0354e5` T2 `Config`+`Setup` · `7b32865` T3 `Database` ·
`811351a` T4 `AuthService` · `780b3d3` T5 `Kod.gs` · `2148402` T6 frontend+`esc()` ·
`92df945` fix skop OAuth · `f475d6c` memory.

### 🔴 PENEMUAN BESAR MALAM INI — `oauthScopes` tidur sejak hari pertama

Klik **pertama** master gagal: *"Specified permissions are not sufficient to call
`SpreadsheetApp.getActiveSpreadsheet`"*. `appsscript.json` ada **hanya** `/auth/drive`.

🔑 **Ia TIDAK MUNGKIN ditemui lebih awal.** Spike Task 0 hanya guna `DriveApp` ⇒ `drive` memang
cukup ⇒ 6/6 lulus bersih. Hirisan 1 ialah kod **PERTAMA dalam sejarah projek** yang menyentuh
Sheets.
🔴 **38 ujian, 4 penyemak, 3 fix round, 4 mutasi — semuanya terlepas.** Bukan kerana lemah:
`node --test` berjalan di mesin master, di mana tiada Google. Ia menguji **peraturan**; ia tidak
boleh menguji **izin**. Semua semakan memeriksa **kod**; tiada sesiapa memeriksa **fail tetapan**.
➡️ Yang menangkapnya: **master klik satu butang.** → [[feedback_izin_bukan_peraturan]] (BARU)

Fix: **tiga** skop — `spreadsheets.currentonly` + `userinfo.email` + `drive`.
🟢 `currentonly` **disahkan berfungsi**, dan ia **menguatkuasakan keputusan #13 pada peringkat
platform**: kod tak boleh buka spreadsheet lain walaupun `SPREADSHEET_ID` ditambah semula.

### 🆕 DUA KEPUTUSAN MASTER — kedua-duanya membaiki PELAN, bukan kod

- **#20 setup boleh DISAMBUNG.** Gerbang bertukar daripada *"ada sheet?"* kepada *"setup SIAP?"*
  (penanda `TETAPAN.SETUP_SIAP` ditulis paling akhir) + `sheetIkutNama()` cari-dahulu-cipta-
  kalau-tiada. Dahulu: Drive gagal ⇒ sheet sudah wujud ⇒ larian kedua pulang `SUDAH_DISEDIAKAN`
  ⇒ folder **tak pernah** dicipta, meletup dalam Hirisan 2 jauh dari puncanya.
  🔴 `sudahDisediakan()` **sengaja tidak sentuh Drive** — ia dipanggil setiap muat halaman.
- **#21 email berganda `USERS` DITOLAK.** Dahulu baris pertama menang senyap ⇒ turun pangkat
  dengan **menambah** baris (bukan menyunting) gagal tanpa amaran.
  🔑 Hujah pemutus: `binaPetaHeader()` **sudah** campak untuk kolum berganda atas sebab sama.
  🔑 Sentinel `{berganda:true}` atas `throw` kerana ia **gagal-tertutup sendiri**.

### 🔑 MUTASI YANG MENGGIGIT SIFAR = ujian tak pernah sentuh pagar

Mutasi 2 Task 4 membunuh **sifar** ujian. Fixture `PEMILIK` ada email **tak kosong**, jadi
`'' === 'guru1@…'` sudah pulangkan `false` tanpa pagar. Kes bahaya sebenar ialah **kosong lawan
kosong** (`'' === ''` ⇒ **true** ⇒ sesiapa boleh edit baris rosak) — dan **komen pada pagar itu
sendiri** sudah menulis amaran itu dengan tepat.
➡️ Jangan simpul *"pagar tak perlu"*. Tanya **baris MANA yang memutuskan**.
Selepas satu assertion ditambah: **4/4 mutasi menggigit ujian yang diramal.**
→ [[feedback_guard_mutation_test]] (dikemas)

### 🎯 SKOP HIRISAN 1 — dan DUA sempadan bersih yang jatuh dengan sendirinya

Setup larian pertama + Auth + senarai **kosong**. Master pilih hiris Fasa 1 (7 kawasan) jadi
**2 keping**, bukan satu pelan besar.

1. **Sempadan KESELAMATAN** — Hirisan 1 mengandungi semua yang memutuskan *siapa awak, apa awak
   boleh buat*; Hirisan 2 semuanya *buat kerja*. Kalau auth silap, ia salah **sebelum** ada 4
   kawasan lain di atasnya
2. **Sempadan DRIVE** — Hirisan 1 **tidak menulis satu fail pun** ke Drive, cuma cipta 3 folder
   kosong. `DriveService.gs` lahir sekali dengan pemiliknya dalam Hirisan 2

### ✅ SIAP SESI INI

| Perkara | Hasil |
|---|---|
| Ukur `gambarB64` | **9.19 MB lulus, 19.5 s** (transport ~10.4 s + backend 9.1 s) |
| Perbandingan visual ASAL vs 800px | Master: **"tak nampak beza"** |
| **Keputusan #19** (spec §2.3) | `gambarB64` = resize client **800px JPEG 0.85** |
| Backlog #6 | ✅ **DITUTUP** |
| Pelan Hirisan 1 | ✅ ditulis, 7 task |

### 🔑 TIGA PENGAJARAN SESI INI *(butiran penuh dalam auto-memory)*

**1. Ukuran ada TIGA keputusan, bukan dua — dan yang ketiga senyap.**
`9.19 MB` **lulus**. Tiada apa-apa dalam sistem yang akan mengadu tentang 19.5 saat — **guru yang
mengadu**. → [[feedback_lulus_tapi_lambat]]

**2. Soalan siling DIPADAMKAN, bukan dijawab.** Kita tak pernah jumpa di mana muatan patah, dan
kini **tak perlu tahu**: resize meletakkan muatan di bawah 1 MB. Siling itu **milik Google** dan
boleh berubah tanpa memberitahu kita. → [[feedback_lulus_tapi_lambat]]

**3. Perbandingan A/B boleh memberi kesimpulan SALAH, bukan sekadar gagal.** Dua pagar yang
menyelamatkannya: satu laluan kod dikongsi, dan `pasangGambar()` **menunggu `img.onload`**.
Tanpa yang kedua, PDF kedua keluar separuh dan kita akan baca itu sebagai *"versi kecil nampak
teruk"*. Bug itu **tidak wujud** pada butang asal — manusia ambil masa nak klik; dua larian
berturut-turut tidak beri jeda itu. → [[feedback_perbandingan_jujur]]

### 🟡 SATU ANGKA MASIH ANGGARAN — sengaja tidak dikejar
`~0.2 MB` / `~2 s` selepas resize. Larian Banding **memang** mengukurnya, cuma keputusannya
tinggal pada skrin master. **Akan keluar sendiri semasa Hirisan 1/2** — jangan buat kerja
berasingan untuknya. Yang **diukur dan dicatat**: mentah 8.77 MB / muatan 9.19 MB / 19.5 s.

### 🔴 JANGAN BACA UKURAN ITU LEBIH LUAS DARIPADA BUKTINYA
Yang diuji **9.19 MB**, **bukan** 21 MB — foto master lebih kecil daripada anggaran 8 MB sebiji.
Selepas keputusan #19 ia tidak penting, tetapi jangan petik sebagai *"21 MB terbukti lulus"*.

### 🔴 `Spike.html` — padam hanya dalam Task 7 Hirisan 1
Backlog #6 sudah dijawab, jadi sebab asal menyimpannya sudah tamat. Tetapi ia dipadam **selepas**
`doGet` disahkan menghidangkan `index` — memadam sumber sebelum penggantinya terbukti berjalan
bermakna tiada apa-apa untuk kembali kepadanya. **Perlu confirm master** (`CLAUDE.md`).

### 🎯 OPR PEMBANGUNAN KARAKTER INSANIAH — asas yang tidak berubah

🔴 **BUKAN stack biasa master.** Bukan Hono, bukan Workers, bukan D1. Google Apps Script Web App
**TERIKAT pada Sheet** + Drive. Login **DELIMa** percuma, sekolah sudah ada Drive.
Precedent = `erph` (clasp, `node --test` **bogel**, pagar `.claspignore`).
🔴 `clasp` **3.3.0**, bukan 2.x — nama arahan `erph` akan gagal.

**Baca ikut susunan:** `opr-insaniah/CLAUDE.md` → `MEMORY.md` →
`docs/superpowers/specs/2026-08-11-opr-insaniah-design.md` (**§2.3 = keputusan #19 baharu**).

Identiti: `scriptId` `1zxr2YV7...` · Sheet `parentId` `1bod2wL6...` · akaun
**`g-77420159@moe-dl.edu.my`** (DELIMa — `access: DOMAIN` tiada makna di bawah Gmail peribadi).
Deployment satu ID kekal, kini **`@5`**.

### 🔴 EMPAT KEPUTUSAN YANG PALING MUDAH DIUNDUR TANPA SEDAR
1. **Google Docs template SENGAJA dibuang** — PDF dijana di **browser**; backend cuma menyimpan.
   Memulihkan template = memulangkan **enam** masalah sekali gus
2. **Semua pengguna ACTIVE boleh BACA semua laporan** — keputusan master, bukan terlepas pandang
3. **PDF tidak pernah jadi pautan Drive** — backend baca fail, hantar base64
4. 🆕 **`gambarB64` dihantar KECIL (800px), bukan mentah** — sesiapa yang cadang *"hantar mentah
   je, kita dah uji ia lulus"* sedang tukar 19.5 s dengan ~2 s untuk kualiti yang master sendiri
   **tak boleh bezakan**, sambil memulangkan semula soalan siling yang keputusan #19 buang

### 🔒 KESELAMATAN — disahkan dengan master
**Tiada password di mana-mana.** Google sahkan **siapa**, kita putuskan **apa boleh dibuat**.
`USERS` = `EMAIL · NAMA · ROLE · STATUS` sahaja.
🔴 Satu perbuatan **manusia** boleh memecahkan segalanya: **berkongsi spreadsheet**. `Execute as: Me`
menutupnya secara semula jadi. Risiko **NAIK** selepas keputusan #13 (Sheet kini dalam Drive
pentadbir dengan butang *Share*). Mitigasi wajib: sheet **pertama** `⚠️ BACA DULU` — sudah
dimasukkan ke dalam pelan Hirisan 1 Task 2.

---
## 🎯 TITIK SAMBUNG — mypwa-v2 (sesi 2026-08-10, tidak disentuh 2026-08-11)
**Sesi lepas**: 2026-08-10 (pagi, 09:24→10:0x) — 🟢 **mypwa-v2: KUMPULAN INTERVENSI LIVE PRODUCTION.**
**`test` == `main` == `origin/*` @ `b008fe6`** (fast-forward). `main` lama `8542f96` digantikan.
Suite penuh **58/0/2**, unit 99/99, spek kumpulan 24/24.
**Last Work Activity**: 2026-08-10 (~10:0x — merge + deploy production disahkan)

🟢 **SIAP SESI INI:** (1) skema production dibanding lawan staging · (2) migration `026`–`028`
mendarat pada `mypwa-v2-db` · (3) merge `main` atas izin master · (4) deploy Actions
**`f278b825`** disahkan melalui **penanda kandungan**, bukan timestamp.
`convergence` kekal ◈ **PARTIAL 4/5** (`Hunt` ✗) — master pilih merge tetap.

✅ **MIGRATION PRODUCTION SIAP (09:3x)** — `026` (3 jadual kumpulan) · `027` (36/36 item
`guna_kumpulan=0`, 0 NULL) · `028` (2624/2624 `kumpulan_id` NULL). `ujian_markah` kekal 2624
baris. 3 baris `markah IS NULL` = `is_td=1` bertarikh `2026-04-27`, **pra-wujud** (diperiksa
satu per satu, bukan diandaikan). Skema production kini **sepadan tepat** dengan staging.
🔑 Pra-semakan yang berbaloi: **banding** skema production lawan staging, bukan tanya kewujudan.
🔑 `rows_written: 1` untuk 2624 baris = normal (skema sahaja, nilai lalai dikira masa BACA).
→ [[feedback_banding_skema_sebelum_migration]] (BARU)

🔴 **URUTAN MIGRATION DIBETULKAN SESI LEPAS — memory lama SALAH, kini sudah dibetulkan.**
Untuk migration **aditif**: **staging → uji → banding skema production → migration production
→ merge `main`.** DB boleh menunggu kod; kod tidak boleh menunggu DB.

🔴 **SIFAR PALSU HAMPIR MENIPU AKU** semasa verify: jadual papar `PROD = 0` pada 3 fail —
sebenarnya `curl` gagal DNS/sambungan dan `grep -c` mengira fail kosong. Pembetulan: rakam
`%{http_code}` + saiz dalam jadual yang sama. Semakan yang mengira **KETIADAAN** mesti
membuktikan ia berjaya **BERTANYA** dahulu. → [[feedback_sifar_palsu]]

⚠️ **HAD BUKTI:** yang terbukti = **aset** baharu dihidangkan. Laluan **API** production belum
diuji berautentikasi (`401` bukan bukti route wujud). Ujian penerimaan sebenar = master log
masuk, buka `/analisis` `/laporan` `/trend`, dan cuba tab Kumpulan dalam admin.

⏭️ **SAMBUNG:** (1) master sahkan visual di `erpm-sksalor.celikguru.my` · (2) dua baki Julius
(master pilih **baiki selepas merge**): `r.murid_id` `/bulk` tak disahkan · `markah` teks
merosakkan `AVG` Markah Rujukan · (3) empat baki Hone (500 → 409/400, `<title>` cetak).
🟢 Feature mendarat **MATI** — `guna_kumpulan = 0` pada 36/36 item production. Selamat.

🟡 **DIPARKIR (kekal):** Markah per-subjek tab Kumpulan — spec BELUM ditulis. Butiran dalam
`mypwa-v2/MEMORY.md` blok BACKLOG. 🔴 **TIDAK** perlu migration — `ujian_item.subjek_id` sudah wujud.

🟡 **DIPARKIR 18:24 — Markah per-subjek tab Kumpulan** (master: "simpan dulu"). Brainstorm siap,
**3/3 keputusan reka bentuk dibuat**, spec BELUM ditulis, kod BELUM. Semua butiran + 3 jerat +
2 soalan terbuka ada dalam **`mypwa-v2/MEMORY.md`** blok BACKLOG. Jangan ulang bincang.
🔴 Master pernah sangka ia perlu **migration** — **tidak**. `ujian_item.subjek_id` sudah wujud.

---
## 🎯 TITIK SAMBUNG — mypwa-v2: **PIPELINE PRA-MERGE SIAP** (dikemas 2026-08-10 **09:1x**)

**`test` @ `65782b7` == `origin/test`** · `main` @ `8542f96` tidak disentuh.
Tiga commit sesi ini: `a6d2977` (fix komen+rename) · `6d02b7f` (memory) · `65782b7` (7 drift docs).

### 🔑 PENEMUAN BESAR SESI INI — ralat berhijrah KELUAR dari kod ke prosa
`Hone` jumpa **3 komen** menyimpang · `SAFI` **2** · `Aksara` **7 dokumen** · **kod: 0 kecacatan**.
Struktural, bukan nasib: kod ada 58 ujian yang menghukumnya; prosa **tiada apa-apa**.
Yang paling mahal: komen dakwa `kelas.tahun` INTEGER (ia **TEXT**) — tepat kelas pepijat yang
pernah gigit projek ni (`'4.0' != '4'` ⇒ setiap guru 403). → [[feedback_ralat_berhijrah_ke_prosa]]
🔑 **Nota anti-undur mesti pada SETIAP tempat keputusan muncul.** §7 ada nota "jangan betulkan
balik"; §14 (Ringkasan) senyap-senyap masih berhujah untuk reka bentuk yang **dibatalkan**.

### 🔑 JULIUS 5/6 — dan yang ke-6 SALAH SEPENUHNYA
Dakwa `COALESCE` halang cap dikemas bila murid pindah. **Arahnya terbalik.** Diuji pada D1:
hantar `20` → cap `20` · hantar `NULL` → cap kekal. Hujahnya lengkap dan meyakinkan; yang
membezakan hanya **dua baris SQL**. → [[feedback_cross_ai_hujah_munasabah]]
🟢 Dua penemuan terbaiknya datang dari soalan **TERBUKA** ("apa yang aku tak sebut?"), bukan
dari 5 soalan berfokus yang aku sendiri rangka.

### 🔴 DUA BAKI SAH DARI JULIUS — master pilih **merge dulu, baiki selepas** (bukan regresi)
1. `r.murid_id` dalam `POST /bulk` **langsung tak disahkan**. Bentuknya jahat: `/laporan`+`/trend`
   ada `ui.tahun = k.tahun` ⇒ markah yatim **lenyap**; `/analisis` **tiada** ⇒ ia merosakkan carta
   gred sekolah sambil **tak kelihatan** dalam laporan yang guru buka untuk menyiasat.
2. `markah` teks disimpan senyap ke kolum INTEGER. Diukur: `'85','A',''` ⇒ **AVG 28.33**, COUNT 3.
   Ia menyuap lajur **Markah Rujukan** yang admin guna MEMILIH ahli intervensi.
   → [[reference_d1_nombor_real]] (dikemas dengan arah songsang)

### ⚠️ Perubahan neutral tak boleh dibukti perambatan melalui tingkah laku
Rename + komen = tiada penanda kandungan. Bukti datang dari **sistem deploy** (`fb6900c4` dicipta
Actions 20 saat selepas push; deployment sebelumnya **14 jam** lebih awal ⇒ tiada push saingan)
+ **kesan songsang** (rename tersilap ⇒ `/trend` 500). → [[feedback_perubahan_neutral_perambatan]]

### 🟡 EMPAT BAKI HONE — master pilih tidak baiki (sedar)
`PUT /kumpulan/:id` tiada catch UNIQUE (500 bukan 409) · `PUT /:id/guru` tiada dedup (500) ·
`kumpulan_id` tiada validasi jenis (500 bukan 400) · `<title>` cetak `laporan-ujian.html:471`
tak di-escape (**pra-wujud**, input ADMIN-sahaja ⇒ pengerasan bukan lubang).

---
## 🎯 ~~TITIK SAMBUNG~~ ✅ DISAMBUNG — mypwa-v2: **T10 SIAP, 10/10** (dikemas 2026-08-09 **16:4x**)

**`test` @ `b586a8c` == `origin/test`** · `main` @ `8542f96` tidak disentuh.
`tests/kumpulan-e2e.spec.js` (`53a0133`) + docs (`e0ae3e5`, `b586a8c`). 🆕 **BACA `mypwa-v2/MEMORY.md` DULU.**

### ⏭️ SAMBUNG — baki Kata pipeline (BUKAN sebahagian pelan, belum dibuat)
`sight-hone` → `safi` → `cross-ai-julius` → `convergence` → **izin master** → merge `main`.
🔴 **Migration `026`–`028` BELUM dijalankan pada production `mypwa-v2-db`** — mesti dijalankan
**sebelum** kod production dihidangkan, kalau tidak setiap query menyentuh `kumpulan_id` campak.

### 🧪 KEADAAN STAGING UNTUK UJIAN MANUAL (diukur 2026-08-09 16:4x)
🔴 **TIADA ujian yang guru boleh isi.** `1` tutup · `50` tutup (0 item) · `133` sampah lama
(`UJIAN MODAL 1786234346272`, 0 item, **tak dipadam — tunggu izin**) · **`2` status `buka`
TAPI `tarikh_tutup = 2026-06-17` sudah lepas ⇒ auto-tutup**. Skrin guru akan nampak KOSONG
dan itu **bukan** bug kumpulan. → [[reference_mypwa_ujian_tertutup]] (BARU)
**0 kumpulan / 0 ahli** — suite bersihkan semuanya; kumpulan mesti dicipta semula untuk uji.
Tahun 4: **4 DELIMA (32) · 4 TOPAZ (30) · 4 ZAMRUD (31)** — cukup untuk bukti merentas kelas.

### 🔴 LARANGAN 1 LOLOS 9 TASK TANPA DIUJI
`DELETE /kumpulan/:id` = `409` bila ada cap — 6 spek kumpulan, tiada satu pun mengujinya.
`bersihFixtur` memanggil endpoint itu setiap `afterAll`, jadi ia **nampak** berliputan — tetapi
ia mengharap `200`, iaitu dakwaan yang BERLAWANAN. → [[feedback_route_sama_dakwaan_beza]] (BARU)

### 🔑 DUA PENEMUAN MUTASI MEMBETULKAN TANGGAPAN LAMA
1. Padam kumpulan bercap **tidak** menyebabkan kehilangan senyap — **D1 FK komposit menolak**
   dengan `500`. Pagar `409` ialah lapisan **mesej boleh baca**, bukan satu-satunya perlindungan.
2. `PUT /ahli` boleh lapor `{"ok":true,"diubah":3}` sambil menulis **sifar** baris. `binaFixtur`
   sendiri tak nampak (assert `200` sahaja) — hanya **baca-balik** menangkapnya.

### 🔴 DUA GOTCHA OPERASI BAHARU
1. Deploy + ujian dalam SATU arahan ⇒ keputusan dari build **LAMA**. Corak mustahil (pagar
   dibuang tapi ujian lulus) = syak **perambatan**. → [[feedback_deploy_ujian_serentak]] (BARU)
2. **`wrangler deploy` tanpa `--env` bind DB PRODUCTION** (`wrangler.toml` aras atas =
   `mypwa-v2-db`). Maut semasa ujian mutasi. `CLAUDE.md` projek dikemas.

**Verify T10:** 3 mutasi → 3 gigitan BERASINGAN · 4/4 hijau atas kod dipulihkan (`git diff`
kosong) · suite penuh **58/0/2** · unit **99/99** · 24/24 spek kumpulan disahkan semula atas
**deployment Actions `66b377f5`** (bukan deploy manual kita) · staging bersih 0/0/0.

---
## 🎯 ~~TITIK SAMBUNG~~ ✅ DISAMBUNG — mypwa-v2: **T9 SIAP** (dikemas 2026-08-09 **15:2x**)

**`test` @ `6f50899` == `origin/test`** · `main` @ `8542f96` tidak disentuh. 🆕 **BACA `mypwa-v2/MEMORY.md` DULU.**
`T1`–`T9` ✅ · ⏳ **`T10`** (E2E laluan penuh — pelan ada dalam `docs/superpowers/plans/`).

### 🔑 KEPUTUSAN MASTER DIPINDA — semua skrin ikut KEAHLIAN SEMASA
Asal: `laporan`+`dashboard` ikut **cap**, `trend` ikut keahlian semasa. Master pinda:
**ketiga-tiganya ikut `kumpulan_ahli`**. Sebab — guru intervensi sentiasa bertanya bermula
dari kumpulan yang dia pegang **hari ini**; ikut cap, murid yang baru masuk nampak macam
tiada markah langsung sebelum itu (iaitu garis dasar yang dia perlukan). **Kesan diterima:**
cetakan lama tidak boleh dihasilkan semula bila murid dipindah — markah tak berubah, cuma
**siapa masuk senarai mana**. Cap KEKAL ditulis + kekal asas larangan `409`. Spec §7 dikemas.

### 🔴 PELAN T9 ASAL ADA 3 KECACATAN — ditangkap SEBELUM dilaksana
1. `WHERE um.kumpulan_id=?` atas **`LEFT JOIN`** → INNER JOIN senyap; mutasi: **3 ahli → 1**
   pada laporan ber-**RANKING**. → [[reference_left_join_where_inner]] (memory BARU)
2. Sisip `JOIN ... = ?` di tengah SQL menggeser `.bind()` — guna subkueri `IN`, `?` di hujung
3. `const allKelas = !kelas_id` — satu boolean, dua keadaan ⇒ `?kumpulan_id=` pulang SEMUA kelas

### 🔴 GREP NAMA LAMA TANGKAP BUG SEBENAR — kali ini SEBELUM dihantar
Dropdown bertukar ke kunci komposit `K`/`C` (konvensyen T8), tapi `cetakSemuaSlip()` masih
`m.kelas_id == selKelas.value` ⇒ `== 'C12'` tak pernah padan ⇒ **Cetak Semua Slip mati**
dengan toast "Tiada murid" pada laporan penuh murid. Mutasi UI sahkan spek menangkapnya.
Juga: `d.kelas` NULL dalam mod kumpulan ⇒ `trend.html:268,284` TypeError, cetakan mati senyap.

### ⚠️ DUA KEGAGALAN PERSEKITARAN (bukan kod)
`getaddrinfo ENOTFOUND` staging ⇒ pembersihan fixtur gagal ⇒ **kumpulan hantu** tertinggal
dan memecahkan spek T5 pada larian seterusnya (`item 94` dapat DUA baris kumpulan).
Dibersih manual (`DELETE ... LIKE '[AUTO]%'`, ujian dahulu baru kumpulan) → 20/20 hijau.
🔑 Kegagalan rangkaian tidak berhenti pada larian itu — ia **meninggalkan keadaan** yang
menyalahkan larian berikutnya. Sahkan DB bersih sebelum menuduh kod.

### 🔴 `git add -A` pada commit mutasi = spek baharu terpadam oleh revert
→ [[feedback_revert_seluas_commit]] (memory BARU)

**Verify T9:** merah 5/6 atas sebab masing-masing (ke-6 lulus atas kod tanpa ciri — dikuatkan
sebelum jadi hijau) · 2 ujian mutasi lawan kod sebenar, kedua-duanya menggigit · spek kumpulan
**20/20** · suite penuh **54/0/2** · **visual cetak DITENGOK**: `@page` laporan = **landskap**
(fail ini ada DUA `@page`), muat `1123==1123`, logo utuh, lajur KELAS papar `4 DELIMA · 4
DELIMA · 4 ZAMRUD` = bukti merentas-kelas atas kertas.

🔴 Baki: cap `kumpulan_id` **boleh dipalsukan** · pagar `/bulk` tak semak `murid_id` milik
guru · **`pelawat.html` kekal mod kelas** (master pilih skop 3 skrin, bukan terlupa).

> Fail ini dilayan sebagai **RAM** (Option A). Ilmu kekal dialir KELUAR ke auto-memory + `MEMORY.md`
> projek; blok `## Compacted History` di bawah kekal **nipis** — pointer kesinambungan sahaja.
> Lihat `compaction/compaction-policy.md`.

---
## 🎯 ~~TITIK SAMBUNG~~ ✅ DISAMBUNG — mypwa-v2: **T5 + T8 SIAP** (dikemas 2026-08-09 **13:0x**)
*T9 sudah siap 15:2x — lihat blok TITIK SAMBUNG aktif di ATAS. Blok ini rekod sejarah sahaja.*

**Repo `test` @ `61e05cb`** (bukan HEAD lagi) · `main` @ `8542f96` tidak disentuh.
**Kumpulan Intervensi 8/10** — `T1`–`T8` ✅ · ~~⏳ `T9`~~ ✅ · ⏳ `T10`.

### 🟢 T8 — SKRIN GURU SEDAR KUMPULAN (`61e05cb`)
✅ **Amaran operasi T5 DITUTUP** — suis `guna_kumpulan` kini selamat dihidupkan dalam admin.
Kunci komposit `K<id>`/`C<id>` (awalan huruf perlu: kumpulan 7 ≠ kelas 7, akan bertembung senyap) ·
label ✦ · nota intervensi + lajur **Kelas Asal** skrin **dan** cetak · `simpanSemua()` hantar cap.

🔴 **AKU ULANG KESILAPAN T5 DALAM TASK SELEPASNYA.** Rename `kelas_id` → `kunci` tertinggal
`selSubjek.disabled = !kelas_id`. `node --check` lulus · `--dry-run` lulus · deploy berjaya ·
dropdown DIISI tetapi kekal **disabled** — nampak macam "ujian ini memang tiada subjek".
➡️ **Selepas SETIAP rename, grep nama LAMA dalam skop itu.** Menulis pengajaran ≠ memasangnya.
🟢 Ditangkap oleh ujian pelayar yang ditulis **MERAH dahulu** — gagal pada langkah tepat.

🔒 Pengerasan XSS: `m.nama` tanpa escape `:252` skrin + `:384` cetak (`escHtml` memang ada).
ADMIN→GURU = **turun** keistimewaan ⇒ pengerasan, BUKAN tutup lubang silang-keistimewaan.
Terlepas oleh kerja 22-sink kerana inventorinya dari medan `tetapan`, bukan dari **sink**.

🟢 Fixtur diekstrak ke `tests/fixtur-kumpulan.js` (T5+T8, T10 nanti). Bukan sekadar elak salinan —
setiap semakan "sahkan alat ukur sebelum mengukur" mesti terpakai kepada SETIAP spek. T5 kekal 5/5.

**Verify:** merah 3/3 sebab masing-masing → hijau 3/3 · mutasi bunuh HANYA ujian cap · suite penuh
**46/0/2** · staging kosong · disahkan atas binaan **Actions** `97420494` (9/9).
**Visual cetak** (`@page` dibaca DAHULU = A4 portrait): kumpulan **5 lajur** sejajar, logo utuh,
✦ terbawa, muat A4 · kelas kekal **4 lajur**.
🔑 `pdftoppm` **tiada** pada mesin ni ⇒ `Read` PDF gagal. Guna `page.pdf()` + PNG `fullPage`
viewport **794×1123** (A4 @96dpi) untuk ditengok.

### ⏭️ SAMBUNG: **T9** (laporan/analisis/trend — suis "Papar ikut") → T10
🔴 Baki: cap `kumpulan_id` **boleh dipalsukan** (master pilih kerja berasingan) · pagar tak semak
`murid_id` tergolong dalam kelas/kumpulan guru.

---

## 🎯 ~~TITIK SAMBUNG~~ ✅ DISAMBUNG — mypwa-v2: **T5 SIAP** (dikemas 2026-08-09 **10:1x**)
*Sesi pagi sambungan. Satu task (T5) ditutup + baki T4 `markah: []` → 500.*

**Repo `mypwa-v2` branch `test` @ `5a27148` == `origin/test`** · working tree bersih ·
`main` @ `8542f96` **tidak disentuh**. 🆕 **BACA `mypwa-v2/MEMORY.md` DULU.**

### 📊 KEADAAN 10 TASK — **7 siap**
`T1` ✅ · `T2` ✅ · `T3` ✅ · `T4` ✅ · **`T5` ✅ `5a27148`** · `T6` ✅ · `T7` ✅ · ⏳ `T8` `T9` `T10`

### 🟢 T5 — LALUAN KUMPULAN (`/jadual`, `/murid`, cap COALESCE pada `/bulk`)
🔑 **Pelan T5 akan bagi hijau palsu — bukan sebab assertion lemah, sebab TIADA DATA.**
Kesemua 16 `ujian_item` staging ada `guna_kumpulan = 0`; Step 5 pelan ("laluan kelas tak
regresi") tidak menyentuh cabang baharu walau sekali. Bila satu cabang hanya hidup di bawah
keadaan yang tiada dalam data, fixture mesti **dicipta**, bukan dicari.
🔑 **Pelan rujuk `laluan` yang tidak wujud** selepas T4 — dan `node --check` (Step 4 pelan)
**tidak menangkapnya**: rujukan pengenal tak-terisytihar ialah sintaks **sah**, meletup sebagai
500 pada runtime sahaja. ➡️ `node --check` sahkan fail boleh **dihurai**, bukan **dijalankan**.
🔑 **Pembersihan dijerat oleh larangan KITA SENDIRI:** `DELETE /kumpulan/:id` = 409 selagi ada
markah bercap (T3) ⇒ `afterAll` mesti padam **ujian dahulu** (cascade buang cap), baru kumpulan.
🟢 Dua mutasi lawan kod sebenar, menggigit **berasingan** — perlu, sebab semasa merah ujian cap
tersekat di hulu oleh `400` `/murid`, jadi ia tak pernah dilihat gagal atas sebabnya sendiri.
**Verify:** merah 5/5 sebab masing-masing → hijau 5/5 kod sama · unit 99/99 · suite penuh
**43/0/2** (2.6m, garis dasar paling bersih setakat ini) · staging kembali kosong selepas 8
larian · disahkan semula atas binaan **Actions** `ecd206bb` (8/8).

### 🔴 AMARAN OPERASI — JANGAN HIDUPKAN SUIS SEBELUM T8
`ujian.html:181` kunci dropdown ikut `kelas_id`. Baris kumpulan (`kelas_id` NULL) ⇒ semua
kumpulan runtuh jadi **satu** pilihan, dan ia hantar `kelas_id=null` ⇒ senarai murid **KOSONG
tanpa satu ralat pun**. Bukan regresi (0 item `guna_kumpulan=1` hari ini) — tetapi suis itu
kini **butang yang memecahkan skrin guru secara senyap** sampai T8 mendarat.

### ⏭️ SAMBUNG: **T8** (`public/ujian.html` — dropdown sedar kumpulan; ia juga tutup amaran di atas) → T9 → T10
🔴 Baki: `kumpulan_id` yang dicap **boleh dipalsukan** (pagar semak "ada mana-mana kumpulan untuk
subjek+tahun+sesi", bukan "kumpulan_id INI milik awak"). **Master pilih kerjakan berasingan.**

---

## 🎯 ~~TITIK SAMBUNG~~ ✅ DISAMBUNG — mypwa-v2: T4 SIAP + ACTIONS PULIH (dikemas 2026-08-09 **08:5x**)
*Sesi pagi 07:33–08:5x. Satu task (T4) + satu blocker infrastruktur ditutup.*

**Repo `mypwa-v2` branch `test` @ `fc6cbb8` == `origin/test`** · working tree bersih ·
`main` @ `8542f96` **tidak disentuh**.
🆕 **BACA `mypwa-v2/MEMORY.md` DULU** — lebih terperinci. Ledger:
`.superpowers/sdd/2026-08-08-kumpulan-intervensi/progress.md` (gitignored).

### 📊 KEADAAN 10 TASK — 6 siap
`T1` ✅ · `T2` ✅ · `T3` ✅ · `T6` ✅ · `T7` ✅ · **`T4` ✅ `fb2b570`** · ⏳ `T5` `T8` `T9` `T10`

### 🔓 T4 — PAGAR KEIZINAN `POST /api/ujian-markah/bulk` (`fb2b570`)
Route ini dahulu **tidak menyemak apa-apa selain "ada token"**. Diperhatikan berlaku di staging,
bukan dibaca dari kod: `PELAWAT` (baca-sahaja) dan `GURU luar hak` kedua-duanya dapat
`200 {"ok":true,"message":"1 rekod markah disimpan."}` — dan baris itu **memang masuk DB**.
Selepas fix: `403`, dan bacaan semula sahkan **tiada baris ditulis**.
**Verify:** merah **2/3** → hijau **3/3** atas kod ujian **sama persis** · unit 99/99 · suite penuh
36 lulus (`skala-gred:253` 6/6 lulus berasingan = persekitaran).

🔴 **PELAN T4 AKAN BAGI HIJAU PALSU — tercetus pada data sebenar.** `POST /bulk` sudah pulangkan
`403` untuk ujian **DITUTUP**. Pelan minta assert `toBe(403)` sahaja, dan **ketiga-tiga** ujian
staging tertutup secara berkesan (`1`+`50` status tutup; `2` buka tapi `tarikh_tutup` 2026-06-17
lepas) ⇒ dua ujian penolakan **LULUS atas sistem tanpa pagar**.
➡️ **`403` BUKAN SATU SEBAB — assert MESEJ.** → [[feedback_status_dikongsi_sebab]] (memory BARU)
🔴 Lima lagi kecacatan pelan: ESM lawan CommonJS repo · `process.env.BASE_URL` kosong yang
**mematikan penjaga production pelan sendiri** · `ujian_item_id: 1` tak disahkan wujud · gelung
`1..50` boleh pilih id tak wujud · tiada assertion peranan.
🔴 **`markah: []` → 500** (`DB.batch([])` meletup). Pelan guna muatan kosong ⇒ selepas pagar betul,
guru sah dapat **500** dan pelan suruh salahkan `nama_sesi` dalam SQL. Buru bug yang tak wujud.
🔑 `ujian_item.tahun` **INTEGER** lawan `kelas.tahun`/`kumpulan.tahun` **TEXT** — D1 pulangkan
nombor sebagai REAL ⇒ `'4.0' = '4'` sifar baris ⇒ **setiap guru ditolak**, ADMIN lulus, tanpa ralat.
`String()` di sempadan. Hanya **sisi TERIMA** boleh tangkap ini; semua spek lain login ADMIN.

### ✅ GITHUB ACTIONS PULIH (08:43) — puncanya SECRET TOKEN
Master tetapkan semula secret `CLOUDFLARE_API_TOKEN` (nilai dari `.env.ujian.ps1`).
Push `526b0f2` → deployment **`2dfc0618`** @ `00:43:17Z` **dicipta Actions, bukan tangan kita**.
Gate `kumpulan-pagar` **3/3 hijau** atas binaan itu (Actions=LF vs manual=CRLF ⇒ bait berbeza,
jadi tingkah laku disahkan semula, bukan diandaikan).
➡️ **Blocker merge ke `main` kini TERBUKA** — production hanya boleh lalu Actions.

🔴 **AKU SALAH LABEL 12 JAM: "Actions SENYAP" — sebenarnya TERCETUS dan GAGAL** (run #549–#552
semuanya ❌). Master yang dedahkan, via screenshot inbox GitHub.
🔑 Puncanya: aku tanya sistem deploy **Cloudflare** ("ada deployment baharu?") → "tiada". Tapi
*"tak tercetus"* dan *"tercetus lalu gagal"* bagi kesan hilir yang **SAMA**. Aku sudah pegang
pengajaran *"tanya sistem deploy, bukan endpoint"* — dan tetap tanya sistem yang **SALAH**.
➡️ **Tanya sistem yang MEMILIKI peristiwa itu.** → [[feedback_sifar_palsu]] (dikemas)
🟢 Yang berjalan betul: punca dipersempit **berprinsip** dulu (antara run berjaya terakhir dan
gagal pertama, repo cuma berubah docs + 3 SQL migration; deploy manual atas kod sama BERJAYA ⇒
punca di luar repo), dan korelasi masa ditulis sebagai **calon, bukan punca**.
⚠️ Master pilih **ganti secret terus** tanpa baca log — jimat satu pusingan. Gerbang keputusan =
**deployment baharu yang bukan kita cipta**, bukan senarai run hijau (run boleh hijau tanpa deploy).

### ⏭️ BAKI T4 (deferred, direkod bukan dilupa)
- `markah: []` → 500. Fix = satu baris pulangan awal. Tak dibundle supaya skop T4 jelas.
- Pagar tak semak `murid_id` tergolong dalam kelas guru. `ujian_item` = (ujian × tahun × subjek),
  **dikongsi semua kelas tahun itu** ⇒ guru SAINS T4 sah boleh tulis markah murid T4 **kelas lain**.
  ⚠️ **Bukti STATIK sahaja**, belum diperhatikan berlaku. Bangkitkan bersama T5.

### ⏭️ SAMBUNG
**T5** (laluan kumpulan dalam `ujian-markah.js` — fail SAMA dengan T4, giliran seterusnya) →
T8 → T9 → T10. Baki semakan bebas T7 masih terbuka (lihat blok di bawah).

---

## 🎯 ~~TITIK SAMBUNG~~ ✅ SUDAH DISAMBUNG — mypwa-v2: KUMPULAN INTERVENSI (dikemas 2026-08-08 **21:35**)
*Sesi 12:00–21:35 (~9.5 jam). Brainstorm → spec → pelan 10 task → 6 task siap. Blocker D1 dibuka
20:15; T7 ditutup 21:1x dengan bug Critical ditemui, dibaiki, dan **dibuktikan pada runtime**.*

**Repo `mypwa-v2` branch `test` @ `35559e9` == `origin/test`** · `main` @ `8542f96` **tidak disentuh**.
🆕 **BACA `mypwa-v2/MEMORY.md` DULU** — lebih terperinci. Ledger per-task:
`.superpowers/sdd/2026-08-08-kumpulan-intervensi/progress.md` (gitignored, kekal di disk).

### 📊 KEADAAN 10 TASK

| Task | Status |
|---|---|
| T1 migration + sahkan FK | ✅ `94c6949` — **FK komposit DIKUATKUASAKAN oleh D1** |
| T2 helper fungsi-tulen | ✅ `5dcb30e` |
| T3 8 route + 2 larangan | ✅ `9692c61` (3 fix round) |
| T6 tab admin Kumpulan | ✅ `2bbff65` (2 fix round) |
| *fix envelope* | ✅ `ad7e67f` — **disahkan runtime** |
| **T7** suis `guna_kumpulan` | ✅ `35559e9` — **merah→hijau pada pelayar sebenar** |
| T4 T5 T8 T9 T10 | ⏳ belum |

### ✅ DUA HAL TERBUKA — KEDUA-DUANYA SUDAH DITUTUP 2026-08-09

**1. ~~GitHub Actions senyap~~ ✅ PULIH 08:43 — dan labelnya SALAH.** Ia bukan senyap; ia
**tercetus dan GAGAL** (run #549–#552 semuanya ❌). Punca = secret `CLOUDFLARE_API_TOKEN`;
master tetapkan semula, deployment `2dfc0618` dicipta Actions. Lihat blok TITIK SAMBUNG di atas.
🔑 Ayat asal di bawah ini dikekalkan **sebagai rekod kesilapan**, bukan sebagai fakta.

**2. Catatan sesi lepas menulis tafsiran sebagai fakta.** "T7 tunggu perambatan, bukan bug" — salah.
Probe endpoint berulang **tak boleh** membezakan "belum sampai" dari "takkan sampai"; hanya
`wrangler deployments list` boleh. ➡️ **Tanya sistem deploy, bukan endpoint.**

### 🔴 BUG ENVELOPE KEJADIAN KE-4 — dibawa balik oleh `cherry-pick` SENDIRI
`admin.html:2726` `r.data.data.filter(...)` lawan array telanjang ⇒ TypeError ⇒ menghidupkan
intervensi **mustahil**, gagalnya **senyap** (kotak nampak bertanda, DB kekal 0, tiada toast).
Mematikan berfungsi (melangkau blok itu) ⇒ ujian manual dua-hala nampak separuh betul.
🔑 `de8e0c7` ditulis semasa route berbalut → di-revert **keluar** → `ad7e67f` tukar bentuk route
semasa ia tiada ⇒ imbasan fix **tak boleh melihatnya** → cherry-pick bawa balik utuh. Diff
cherry-pick **bersih**; yang berubah ialah **sekelilingnya**. → [[feedback_cherry_pick_kontrak_basi]]
🔑 **DUA konvensyen envelope wujud** — `laporan-ujian.html` guna `.data.data` dgn BETUL. Penjaga
pukal akan menuduh 3 baris tak bersalah. **Imbas dulu sebelum bina penjaga.**

### 🟢 UJIAN TINGKAH LAKU MENANGKAP APA YANG 3 SEMAKAN STATIK TERLEPAS
`tests/kumpulan-suis.spec.js` — sahkan dari **PELAYAN**, bukan `toBeChecked()` (kotak itulah yang
menipu; `toBeChecked()` akan **LULUS** atas kod berbug). MERAH atas kod berbug hidup dgn mesej
sebenar `"Cannot read properties of undefined (reading 'filter')"` → HIJAU atas kod ujian sama.
🔑 `page.on('dialog', d => d.accept())` **wajib** — Playwright tolak dialog secara lalai ⇒ tanpa ia
laluan BETUL gagal atas sebab palsu.

### ⏭️ BAKI TERBUKA (semakan bebas T7 — belum dikerjakan, di luar skop)
`kumpulan.js:79` abaikan `kelas.tahun_sesi` (**laten** — betul hari ini secara kebetulan) · tiada
pagar pelayan untuk hidupkan suis tanpa kumpulan · `DELETE /item/:id` **cascade padam markah tanpa
pengesahan mahupun audit** · `skala-gred.spec.js:39` masih ada fallback `ADMIN_USER`.

### 🔓 CARA GUNA D1 SEKARANG (blocker sudah selesai)
Token Cloudflare dengan kebenaran **D1: Edit** hidup dalam `.env.ujian.ps1` (gitignored).

```powershell
. .\.env.ujian.ps1; npx wrangler d1 execute mypwa-v2-staging-db --remote --command "..."
```

🔑 **Dot-source WAJIB pada SETIAP arahan** — env var tidak kekal antara panggilan tool.
🔑 Template *"Edit Cloudflare Workers"* **TIDAK** termasuk D1. Gejala mengelirukan: auth
**BERJAYA** (wrangler kenal akaun+emel) tapi `/d1/database` bagi `Authentication error [10000]`.
Sunting kebenaran token — **jangan Roll**, itu tukar nilai token.
🔴 `mypwa-v2-db` = **sekolah sebenar**. Semua kerja pada `mypwa-v2-staging-db` sahaja.

### 🟢 BUKTI RUNTIME PERTAMA (staging, baca-sahaja)
- `GET /api/kumpulan` → **`Object[]`** array telanjang ⇒ fix envelope disahkan **hidup**
- `GET /api/kumpulan/murid?tahun=4` → **93 murid** merentas **4 DELIMA · 4 ZAMRUD · 4 TOPAZ**
  ⇒ senario idea doc wujud dalam data sebenar
- Tahun 4 = tepat 4 subjek: BM · ENGLISH · MATEMATIK · SAINS
- Ujian: `1` DIAGNOSTIK TAHUN 4 (4 item) · `2` LATIHAN PERTENGAHAN SESI (12 item) · `50` Modul (0)

### ❌ ~~HAL SEMASA — T7 tunggu perambatan deploy~~ — DAKWAAN INI TERBUKTI SALAH
Ayat asal: *"Kod betul. Ini perambatan, bukan bug."* — itu **tafsiran ditulis sebagai fakta**.
Sebenarnya Actions gagal. Dikekalkan sebagai rekod kesilapan sahaja. T7 sudah ✅ `35559e9`.

⏭️ **SAMBUNG (dikemas 2026-08-09):** ~~(1) sahkan `bdafe9d`~~ ✅ · ~~(2) semakan T7~~ ✅ ·
(3) sahkan **10 perkara** dalam senarai runtime ledger — termasuk tiga bug yang kita **tahu**
wujud tapi tak pernah lihat berlaku · ~~(4) T4~~ ✅ `fb2b570` · **(5) T5 ← SETERUSNYA** ·
(6) T8 · (7) T9 · (8) T10.

### 🔴 REKA BENTUK: JANGAN PINDAHKAN `murid.id_kelas`
`ujian_markah` **langsung tidak menyimpan kelas** — ia di-`JOIN` masa **BACA**
(`ujian-markah.js:90`, `:152`, `:237`). Memindahkan murid memindahkan markah **MODUL 1** dia
sekali, **tanpa ralat**. MODUL 1 itulah garis dasar "sebelum intervensi".

**Keputusan master terkunci:** sejarah dikunci pada **UJIAN** bukan tarikh · cap auto
`ujian_markah.kumpulan_id` dilindungi `COALESCE` · suis `guna_kumpulan` pada **`ujian_item`**
(ujian × tahun × subjek) · **admin** tetapkan guru · laporan ikut **cap**, trend ikut
**keahlian semasa** · dua larangan (padam kumpulan bercap `409`, ahli silap tahun `400`).

### 🔑 EMPAT PENGAJARAN SESI INI

**1. `GET /:id/guru` digugurkan dengan sebab yang DIPINJAM.** Mengecil 12 route → 7 atas nama
KISS: `GET /:id/ahli` digugurkan **betul** (superset wujud), kemudian `GET /:id/guru` digugurkan
dengan **alasan yang sama** tanpa menyemak ia masih terpakai. Ia tidak.
Kesan: `PUT` ganti-semua + tiada baca ⇒ panel mula **kosong** ⇒ Simpan memadam penetapan guru
sedia ada **tanpa admin pernah melihatnya**. ➡️ **Ganti-semua MESTI berpasangan dengan
baca-dahulu**; bacaan gagal MESTI menghalang Simpan. → [[feedback_salin_corak_bukan_sebab]]

**2. Aku menulis arahan tentang fail yang aku tidak baca — tiga kali.** Rangka HTML guna kelas
**Tailwind** sedangkan `admin.html` tidak memuat Tailwind · `r?.error` sedangkan mesej ada di
`r?.data?.error` (larangan `409` akan gagal **senyap**) · kiraan "14 ujian" sedangkan kod aku
sendiri ada **13**.

**3. Gelung fix boleh mewarisi kecacatan yang sama.** Penyemak namakan **3** tapak `String()`;
pelaksana baiki ketiga-tiganya; re-review jumpa **tapak keempat**. Punca: senarai fix dibina dari
**senarai penemuan penyemak**, bukan dari **tapak dalam fail**. ➡️ Arahan diubah kepada *"imbas
fail SENDIRI, lapor jumlah sebenar"* → **3 tapak, 0 tertinggal**, dan dua route **ditolak** dengan
sebab boleh diperiksa. → [[feedback_inventori_perlindungan_sedia_ada]]

**4. Route melaporkan NIAT, bukan HASIL.** `diubah: murid_id.length` bukan `meta.changes` ⇒
`DELETE` padan 0 baris, murid **kekal** dalam kumpulan, skrin papar "berjaya". Dibetulkan pada
cawangan DELETE; INSERT dibiarkan **dengan komen KENAPA** (upsert sentiasa menulis ⇒ kiraan tak
boleh bercapah). Komen itu wajib supaya penyemak seterusnya tidak "membetulkan" sesuatu yang
memang sengaja.

🟢 **Subagent yang MEMBANTAH = subagent berguna, disahkan lagi.** Ketiga-tiga kesilapan aku
ditangkap oleh pelaksana/penyemak, bukan oleh aku. Penyemak juga buat **imbasan sink sendiri**
(16 sink, padan tepat dengan pelaksana) dan bukan menerima senarai bulat-bulat.

---


---

## 📌 TITIK SAMBUNG PROJEK LAIN (tidak disentuh sesi ini — jangan kambus)

### eRPH RENDAH — ISI RPT SAINS 5 🔄 SEPARUH JALAN
Branch **`rpt-sains5`** (Google Apps Script + clasp). Task 1–3 kod SIAP, suite **20/20**.
⏭️ **Sambung: re-review `eab609c..ea0fd95`** — fix Critical jadual bersarang belum disemak semula.
Task 8 digantung (Chrome extension tak bersambung).
🔑 Parsing naif boleh hilang **10 minggu** sambil output nampak LENGKAP. → [[project_erph]]

### celiksains — HARDENING ANTI-TIPU 🔴 SPEC + PLAN SIAP, BELUM KOD
Fasa 1a live staging (unit 13/13, E2E 17/17). Satu pemilik content, **BUKAN** multi-tenant.
⏭️ Sambung: laksana pelan hardening. → [[project_celiksains]]

### eRPH MENENGAH — ✅ TIADA KERJA TERTUNGGAK
Repo `erph-menengah-v2`. Semua bug selesai, disahkan master 2026-07-23.
🔑 Geometri borang: baris **7**, jarak **48** (bukan 31).
🔴 Baki tunggal: menu `🛠️ Baikpulih Tapak` masih guna jarak **31** DAN ia **MENULIS** — **JANGAN KLIK.**
→ [[project_erph_menengah]]

### sistem-olahraga · ADNI · idme-pajsk-ext
Olahraga & ADNI ✅ live production, tiada tertunggak. `idme-pajsk-ext` lihat ITEM TERBUKA di bawah.

---

## Compacted History
*Diringkas dari 11 rekod sesi (2026-07-15 → 2026-08-08) pada **2026-08-09**, di atas ringkasan
terdahulu (~35 rekod, 2026-06-24 → 2026-07-15, dibuat 2026-07-18). Detail penuh dalam
`compaction/snapshots/current-session-2026-08-09.md` dan `current-session-2026-07-18.md`.
Pengajaran teknikal terperinci **sudah hidup dalam auto-memory** (`feedback_*`, `project_*`,
`reference_*`) — sengaja tidak disalin semula (DRY). Ini pointer kesinambungan sahaja.*

**mypwa-v2 — XSS silang-keistimewaan ✅ LIVE PRODUCTION** (siap 2026-08-08, `main` @ `8542f96`):
- Task 1–7 siap · `escHtml` 39→91 · 22 sink tetapan · gate payload 2-konteks + 2 ujian mutasi kekal
  dalam suite. Verify: suite **34/0/2**, perambatan **per-fail 11/11**, tingkah laku production
  disahkan (bukan timestamp).
- ⚠️ **Baki diakui: gate payload untuk laluan CETAK masih tiada.**
- → [[feedback_inventori_perlindungan_sedia_ada]] · [[feedback_nama_fungsi_jaminan_palsu]] ·
  [[feedback_escape_atribut_baca_balik]] · [[feedback_perambatan_deploy_per_fail]] ·
  [[feedback_tetingkap_global_agregat]] · [[reference_curl_powershell_json]]

**mypwa-v2 — kerja lain semua ✅ live production, tiada tertunggak:** Skala Gred Default
(`8d8bb84`) · Fasa 8 Ujian Dalaman · Fasa 9 Audit Log · Admin Muat Turun Senarai Murid (`a40c638`)
· fix nombor Bil laporan PAJSK. → [[project_erpm_v2]] · [[reference_mypwa_deploy]]

**Sistem memory per-projek** (2026-08-07) — `CLAUDE.md` = **arahan** (kecil, dibaca setiap sesi) ·
`MEMORY.md` = **ingatan** (membesar). Lucy tulis `MEMORY.md` sendiri tanpa izin.
→ [[feedback_memory_per_projek]]

**sistem-olahraga** — throttle brute-force `/api/login` ✅ live (`5468d85`), D1 `login_attempts`
keyed IP+username, fail-closed. Sebelumnya: markah per-hakim (PK 5-lajur, RAW+DERIVED), hardening
multi-tenant PK komposit, cetak laporan. Semua live `atletik.celikguru.my`.
→ [[project_sistem_olahraga]] · [[feedback_pk_komposit_join]] · [[feedback_nilai_terbitan_arkib]]
- 🔴 Backlog security **diterima sedar**: timing side-channel enumeration username · sustained
  per-username lockout DoS.

**eRPH (rendah + menengah)** — bug import *"Berjaya tapi kosong"* ✅ siap kedua-dua repo. Satu
gejala, tiga punca yang sama pada kedua-duanya: markdown `**OBJEKTIF:**` tak padan regex ·
`clearContent()` dipanggil SEBELUM parse (import gagal MEMADAM RPH sedia ada) · `alert("Berjaya")`
tanpa syarat menyembunyikan dua yang pertama. → [[project_erph]] · [[project_erph_menengah]] ·
[[feedback_bukti_saluran_lossy]]

**Lucy skills / memory** — 30 skills aktif. Compaction Option A (RAM + snapshot) **menggantikan**
protokol 500-baris lama; `main/session-format.md` masih wujud tetapi **DINETRALKAN** — jangan
jalankan protokol di dalamnya. → [[project_lucy_skills]]

### 🔴 ITEM TERBUKA (belum buat — JANGAN kambus)
- **`idme-pajsk-ext`** — spec + plan **SIAP** (8 task TDD), repo `Documents/code/idme-pajsk-ext`
  (spec `ca3abcc`, plan `890821f`). Pendekatan A: MV3 Side Panel + blok JSON `#pajsk-export` dalam
  `mypwa-v2/pajsk.html`. Task 1–6 boleh mula; **Task 7 GATED** — master kena bekal selector borang
  `idme.moe.gov.my`. Ujian `node --test` (tiada npm). → [[project_idme_pajsk_ext]]
- **mypwa-v2**: gate payload XSS laluan **cetak** · cap `kumpulan_id` **boleh dipalsukan** (master
  pilih kerja berasingan) · pagar `/bulk` tak semak `murid_id` tergolong dalam kelas/kumpulan guru
  · **`pelawat.html` kekal mod kelas sahaja** (pengguna ke-4 `/analisis`; master pilih skop 3 skrin
  pada T9 — keputusan sedar, bukan terlepas pandang).
- **Cadangan audit kecil Lucy** (belum master putus): `created_at` timezone di tempat lain ·
  teks/border pucat pada page cetak lain (slip ujian, pajsk, RPM). Kekal dalam snapshot.
- **Lalai `Admin@1234` dalam `seed.sql`** — master pilih **biarkan**; relevan semula hanya bila
  pasang instance baharu. Pembetulan sebenar = paksa tukar pada log masuk pertama.
- Backlog projek lain (Lompat Tinggi Fasa 2, DELETE-sebelum-validate, normalisasi markah_penuh,
  ETR boleh-laras, eksport Excel) **tercatat dalam `CLAUDE.md`/`MEMORY.md` projek masing-masing.**

*Tiada kredensial, token, mahupun `JWT_SECRET` dibawa masuk ringkasan ini.*
