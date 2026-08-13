# Relationship Memory — Lucy & Master

> Fail ini menyimpan maklumat tentang master: cara kerja, preferens, goals, dan gaya belajar.
> Lucy akan update fail ini secara manual (bila master minta) atau cadangkan auto-save bila detect maklumat penting.

---

## Profil Master

- **Background:** Vibe coder — tiada formal coding background
- **Cara Belajar:** Belajar sambil bina projek sebenar menggunakan AI
- **Komunikasi:** Bahasa Melayu

---

## Tech Stack

- **Backend:** Hono.js on Cloudflare Workers
- **Database:** Cloudflare D1 (SQLite)
- **Frontend:** Vanilla HTML/JS + Tailwind CSS
- **Version Control:** Git → push ke `test` branch → auto deploy ke Cloudflare

---

## Cara Kerja Master

- Suka tengok plan dulu sebelum sebarang perubahan dibuat
- Minta tanya dulu sebelum buat perubahan besar
- Setiap perubahan kod → commit + push ke test branch
- Test DB guna `--remote`
- Run Playwright tests sebelum setiap push

---

## Pantang Larang

- Jangan suggest TypeScript
- Jangan restructure folder tanpa tanya dulu
- Jangan install packages baru tanpa bagitahu dulu
- Jangan delete fail tanpa confirm dulu

---

## Projek Aktif
*(dikemas 2026-08-07 — senarai lama lapuk: `my-pwa` sudah DIPADAM, `erpm-cf`/`myportfolio` kurang aktif)*

- 🆕 `opr-insaniah` (OPR Pembangunan Karakter Insaniah — **Google Apps Script** terikat pada Sheet,
  bukan Hono/Workers. **Fasa 1 Hirisan 1 KOD SIAP 2026-08-13** — branch `fasa1/hirisan-1`, 38/38,
  deploy `@7`, skrin utama hidup. Seterusnya: ujian penerimaan **langkah 4–15**)
- `mypwa-v2` (eNilai — per-SEKOLAH, live production)
- `erph` (sekolah RENDAH) · `erph-menengah-v2`
- `celiksains`
- `sistem-olahraga-sekolah`
- `idme-pajsk-ext`
- Kurang aktif: `adni`, `balapan`, `sprint`, `erpm-v2`, `myportfolio`, `erpm-cf`

---

## Goals & Aspirations

_(akan diisi bila master share)_

---

## Keputusan Master (kekal sehingga ditarik balik)

- **Password awal guru KEKAL** (2026-08-07) — menukarnya bermakna memaklum semua guru; itu keputusan **operasi sekolah**, bukan teknikal. Nilai sebenar hidup dalam `.env.ujian.ps1` (gitignored), **jangan tulis dalam mana-mana fail dijejak git**.
- **Password admin** akan ditukar sendiri oleh master melalui UI — Lucy tidak menukarnya.
- **Izin KEKAL:** Lucy tulis ke `MEMORY.md` setiap projek sendiri, tanpa tanya.
- **Sejarah git sengaja TIDAK ditulis semula** untuk rahsia lama — sebaik password ditukar, nilai lama tidak bernilai; risiko `force-push` lebih besar daripada faedahnya.
- **Password admin SUDAH ditukar** (disahkan master 2026-08-08). Lalai `Admin@1234` dalam `seed.sql` **tidak** dibetulkan sekarang — master pilih biarkan. Ia cuma relevan semula **bila pasang instance baharu**; pembetulan sebenar = paksa tukar pada log masuk pertama, bukan padam baris dokumen.
- **Master beri izin merge production bila bukti mencukupi** — pada 2026-08-08 master pilih **jalankan suite penuh dahulu** sebelum merge, walaupun kesimpulan "persekitaran" sudah terbukti. ➡️ Corak: master mahu **garis dasar hijau penuh** sebelum menyentuh sekolah sebenar, bukan sekadar hujah yang meyakinkan.
- **Izin cipta DATA UJIAN pada staging DB** (2026-08-09) — bila satu cabang kod hanya hidup di bawah keadaan yang **tiada dalam data staging**, master luluskan fixture dicipta (melalui API, dibersihkan semula). ⚠️ `mypwa-v2-staging-db` sahaja; `mypwa-v2-db` = sekolah sebenar, **jangan sentuh**.
- **Skop task kekal KETAT — jangan bundle baiki bersebelahan** (2026-08-09) — ditawarkan dua baki keselamatan untuk digabungkan ke dalam T5; master ambil **yang kecil sahaja** (`markah: []` → 500) dan tolak yang besar (sahkan `kumpulan_id` milik guru), walaupun task itulah yang **memperkenalkan** medan berkenaan. ➡️ Corak: master lebih suka satu commit = satu perkara yang boleh diperiksa, daripada commit besar yang "sekali harung". Lucy patut **tawar sebagai pilihan berasingan**, bukan selitkan diam-diam, dan **rekod baki dalam `MEMORY.md`** supaya ia tidak hilang.

- **KOD dibaiki minimum, DOKUMEN dibaiki menyeluruh** (2026-08-10) — dalam satu sesi yang sama:
  daripada **5** penemuan `sight-hone` pada kod, master ambil **satu sahaja** (pembetulan komen,
  kos hampir sifar) dan tolak empat yang lain walaupun kesemuanya kecil. Tetapi daripada **6**
  drift dokumen `sight-aksara`, master ambil **kesemuanya sekali gus**.
  ➡️ Corak: risiko yang master timbang ialah **menyentuh kod**, bukan jumlah kerja. Perubahan
  dokumen tidak boleh memecahkan apa-apa, jadi ia murah tanpa mengira saiz; perubahan kod
  membawa risiko regresi, jadi ia dinilai satu per satu. **Lucy patut tawar fix kod sebagai
  pilihan berasingan (satu commit satu perkara), tetapi fix dokumen boleh dibundle jadi satu.**
- **Master beri "Ok" ringkas sebagai persetujuan kepada cadangan terakhir Lucy** (2026-08-10).
  Bila ada dua pilihan ditawarkan, "Ok" bermakna pilihan yang Lucy **syorkan**. Kalau taruhannya
  tinggi (sentuh production/DB sekolah), **jangan** tafsir "Ok" — tanya semula secara spesifik.

- **Master KUATKUASAKAN langkah `plan` — dan dia yang perasan, bukan Lucy** (2026-08-12).
  Lucy melompat terus daripada keputusan reka bentuk kepada langkah kod, walaupun `MEMORY.md`
  projek tertulis jelas *"Sambung: tulis pelan pelaksanaan"* dan pipeline Kata untuk projek baru
  bermula dengan `plan`. Master tegur: *"terus bina ke? bukan implementation plan belum ada ke?"*
  ➡️ **Master membaca catatan projek dan mengingatinya.** "Tunjuk plan dulu" bukan formaliti yang
  boleh dilangkau bila kerja nampak kecil — ia gerbang sebenar yang master **semak**.

- **Master lebih suka pelan yang SEMPIT bila langkah seterusnya belum terbukti** (2026-08-12).
  Ditawarkan pelan `Task 0 + Fasa 1` (seperti spec asal) atau `Task 0` sahaja; master ambil yang
  **sempit**. Sebabnya sama dengan corak "satu commit satu perkara": jangan bayar untuk kerja yang
  mungkin dibuang. Merancang di atas tanah yang belum terbukti = kerja dibayar dua kali.

- **"Ukur dahulu" TERBUKTI berbaloi — bukan sekadar berhati-hati** (2026-08-13). Master pilih ukur
  `gambarB64` sebelum menulis pelan Fasa 1, dan bukan sebaliknya. Hasilnya **mengubah reka bentuk**:
  laluan simpan kini ada langkah **resize di client** yang tidak wujud dalam **mana-mana** lakaran
  sebelum itu. Kalau pelan ditulis dahulu, bahagian itu kena buang dan tulis semula.
  ➡️ Ujinya bukan *"berapa besar risikonya?"* tetapi **"tanah yang belum terbukti itu di TEPI
  kawasan yang nak dirancang, atau di DALAMnya?"** Kalau di dalam — ukur dahulu, sentiasa.

- **Master ingatkan KISS & DRY di TENGAH kerja, bukan selepas** (2026-08-13). Sebaik Lucy siap
  menulis pelan 7 task, master hantar *"Ok. Jangan lupa KISS & DRY"* — sebelum melihat pelan itu.
  Semakan sendiri yang tercetus daripadanya membuang **lima** perkara: pemuat ujian yang disalin
  4 kali, `esc()` yang menduakan diri, dua fungsi tanpa pemanggil, dan amaran 18 baris yang takkan
  dibaca.
  ➡️ **Master menyangka Lucy akan lebihkan barang, dan master betul.** Jalankan semakan KISS/DRY
  ke atas kerja sendiri **sebelum** membentangkannya — jangan tunggu diingatkan.
  🔑 Tetapi jangan buang kod hanya kerana ia belum berjalan: pemapar senarai Hirisan 1 **dikekalkan**
  dan ujian penerimaan diubah supaya ia **diperhatikan berjalan**. Kod mati dibaiki dengan
  **menghidupkannya**, bukan sentiasa dengan membuangnya.

- **Master MEMBENARKAN pelan diubah — bila ditunjuk senario kegagalan BERNAMA** (2026-08-13 malam).
  Ini melengkapkan, bukan membatalkan, catatan 2026-08-12 *"master kuatkuasakan langkah `plan`"*.
  Dalam satu sesi pelaksanaan, **dua** semakan mendedahkan pelan yang master sendiri luluskan
  bercanggah dengan dirinya. Kedua-duanya dibentang sebagai **cerita konkrit** — *"Cikgu Zaki
  diturunkan pangkat dengan menambah baris, bukan menyunting; baris ADMIN lama menang; master fikir
  dah turun, sebenarnya tidak"* — bukan sebagai istilah (*"first-match resolution is
  order-dependent"*). Master luluskan penyimpangan **kedua-duanya, serta-merta**.
  ➡️ **Gerbang `plan` itu bukan tentang mematuhi teks pelan; ia tentang master yang memutuskan.**
  Menyelit fix diam-diam melanggarnya; menunjuk kegagalan bernama dan bertanya **tidak**.
  🔑 Yang menukar jawapan ialah **hujah konsistensi dalaman**: *"`binaPetaHeader()` sudah campak
  untuk kolum berganda atas sebab yang sama persis — ini masalah sama bentuk pada baris."* Master
  bergerak paling laju bila ditunjuk keputusan yang **dia sudah buat**, dipakai semula.
  Sambungan [[feedback_soalan_reka_bentuk_contoh]].

- **Master berhenti bila ditawarkan berhenti — jangan tunggu dia minta** (2026-08-13, 22:44).
  Selepas ~6 jam, master pilih *"berhenti — simpan semua dulu"* daripada meneruskan 12 langkah ujian
  penerimaan yang berbaki. Isyarat awal ada dan Lucy **terlepas** pada mulanya: dua mesej bertaip
  rawak (`33333333333333+`, `222999999\/9.`) sekitar 18:00.
  ➡️ Bila kerja berbaki melibatkan **menyunting data sebenar dan mengembalikannya semula**, dan jam
  sudah lewat, **tawarkan titik berhenti dengan jaminan tiada kerja hilang** — jangan sekadar
  serahkan senarai seterusnya. Master ambil tawaran itu.

  🔑 **PENAMBAHAN 23:1x sesi yang sama — master kembali 11 minit kemudian dengan *"sambung project
  opr"*.** Jadi "berhenti" bukan bermakna sesi tamat; ia bermakna **beban** itu yang ditolak, bukan
  kerja. Yang berkesan: **pecahkan baki ikut RISIKO, bukan ikut bilangan.** 12 langkah berbaki
  dibelah kepada 6 *"melihat sahaja, tiada apa perlu dipulihkan"* lawan 6 *"sunting-lalu-pulihkan"*.
  Master ambil yang selamat, siapkan **kesemuanya** dalam ~10 minit, dan projek bergerak 9/14.
  ➡️ **Jangan tawarkan hanya `teruskan` lawan `berhenti`.** Cari belahan semula jadi dalam kerja
  itu sendiri — selalunya *"yang mana boleh silap, dan silapnya susah dipulihkan?"* — dan tawarkan
  bahagian selamat sebagai pilihan ketiga. Bahagian itu selalunya siap penuh, bukan separuh.

## Kekuatan master yang Lucy patut manfaatkan

- **Keputusan VISUAL master datang daripada MELIHAT, bukan daripada berbincang** (2026-08-13).
  Spec projek `opr-insaniah` tercatat *"format OPR sebenar cuma 1 keping gambar"* — ditulis
  daripada perbualan reka bentuk. Dalam masa beberapa minit selepas membuka PDF **sebenar** yang
  dijana spike, master menetapkan **maksimum 2**, dengan susun atur (*"1 gambar center, 2 gambar
  50-50"*). Pusingan berikutnya master kata *"gambar terlalu besar"* — sesuatu yang tidak pernah
  timbul dalam mana-mana perbincangan, dan tidak mungkin timbul, kerana ia tentang **rupa**.
  ➡️ **Untuk apa-apa yang master akan LIHAT (susun atur cetak, saiz, jarak, warna), jangan kunci
  spec daripada perbualan.** Jana satu contoh sebenar, tunjuk, biar master bercakap. Perbincangan
  menghasilkan spec yang kedengaran munasabah dan **salah**; satu PDF menghasilkan keputusan tepat
  dalam satu ayat. Ini sambungan langsung kepada [[feedback_verify_cetak_visual]] — bezanya di sana
  ia tentang **mengesahkan** kerja siap, di sini ia tentang **memutuskan** apa yang hendak dibina.
  🔑 Bila master kata sesuatu "terlalu besar/kecil", cari **nombor** yang master sudah luluskan
  secara tersirat dan berlabuh padanya. Hari ini: 16:9 (197px) diterima, 4:3 (263px) ditolak ⇒
  had 208px, sedikit **di atas** yang diluluskan, supaya yang master sudah setuju **tidak berubah**.

- **Master nampak pengalaman PENGGUNA, Lucy nampak struktur sistem** (2026-08-12).
  Lucy bentang dua jalan pengedaran dan syorkan yang salah — kerana Lucy membandingkan kedua-duanya
  pada langkah yang **sama** (deploy, had platform yang tak boleh dielak) sambil terlepas langkah
  yang **berbeza**: dalam satu jalan cikgu terpaksa membuka editor kod, dalam satu lagi dia cuma
  menyalin Google Sheet. Master terus nampak.
  ➡️ **Bila menimbang pilihan, tanya master "apa yang orang itu nampak?" — bukan cuma bentangkan
  perbandingan teknikal.** Dan bila dua pilihan berkongsi langkah yang paling susah, langkah itu
  **bukan** pembeza; cari beza di tempat lain.

---

## Catatan Penting

- **Konsistensi kolum jadual (terutama cetak):** master pentingkan kolum jadual kekal konsisten. Label decorative (cth "Tidak Menguasai") yang muncul hanya pada sesetengah baris buat kolum jadi tak sejajar/tak konsisten — master prefer **BUANG label itu dari paparan cetak** daripada cuba dandan style. Paparan skrin/desktop boleh kekal ada label. (Origin: feature Trend Markah ETR, 2026-07-04)
