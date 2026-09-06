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
*(dikemas 2026-08-24 — senarai lama lapuk: `my-pwa` sudah DIPADAM, `erpm-cf`/`myportfolio` kurang aktif)*

- 🆕 `Digital Hub` (portal akses semua sistem sekolah — brainstorming architectural JALAN,
  belum sampai spec/plan, belum wujud secara fizikal. Butiran: `current-session.md`)
- `opr-program` (migrate AppSheet OPR SK Salor → Apps Script. **2026-09-06: Anjuran/Tempat
  checkbox berbilang LIVE `@11`, fix visual (letterhead full-width + checkbox mobile) LIVE
  `@13`** — fix pertama `@12` ada 2 SILAP (master tangkap kedua-duanya lepas smoke telefon),
  dibetulkan selepas semak git history sebenar + minta screenshot. Suite 365/365. Fasa 3a Tetapan
  & Branding (letterhead upload) LIVE sejak `@10`. **Fasa 3 dipecah 3a/3b/3c** — 3a SIAP, 3b
  (Panel Pengguna & Rujukan) + 3c (Migrasi) belum mula. Smoke manual master utk `@13` TERTUNGGAK.
  Butiran: `opr-program/MEMORY.md`)
- `opr-insaniah` (OPR Pembangunan Karakter Insaniah — **Google Apps Script** terikat pada Sheet,
  bukan Hono/Workers. **BELUM launch rasmi ke sekolah** (dibetulkan master 2026-09-01 — catatan
  lama "sekolah sudah guna aktif harian" SALAH). **2026-08-25:** siri 4 fix guna sistem sebenar
  (fon PDF tak konsisten, tab blank iPhone Chrome, jadual Senarai Laporan terpicit iPad menegak,
  warna label kelabu) — deploy `@37→@44`, suite 526/526. Fasa 2b Edit+Padam laporan masih belum
  dirancang. Butiran penuh: `opr-insaniah/MEMORY.md`)
- `takwim-digital` (Apps Script + Google Calendar, akaun DELIMa. LIVE production `@8` 2026-08-23)
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

- **Master ambil KESEMUA 5 penemuan review — dan itu BUKAN percanggahan dengan corak 2026-08-10**
  (2026-08-16). Pada 10 Ogos master ambil **1 daripada 5** penemuan kod dan tolak empat. Hari ini
  master ambil **4/4** fix kod tanpa teragak-agak. Bezanya bukan mood: setiap penemuan hari ini
  dibentang dengan **cerita kegagalan BERNAMA** — *"`getFileById` berjaya untuk fail dalam sampah,
  jadi guru dapat skrin Google 'item in trash' dan bukan mesej kita, dan kunci `PDF_HILANG_DRIVE`
  tidak pernah menyala"* — bukan sebagai *"pembaikan kecil"*.
  ➡️ **Yang master timbang ialah AKIBAT, bukan saiz diff.** Penemuan tanpa cerita kegagalan
  dibaca sebagai kemasan dan ditolak; penemuan dengan cerita dibaca sebagai risiko dan diambil.
  Sambungan langsung [[feedback_soalan_reka_bentuk_contoh]].

- **Master minta *"update?"* di TENGAH kerja panjang — dan itu isyarat, bukan sekadar soalan**
  (2026-08-16 petang). Lucy membaca seluruh kod projek lalu menulis pelan 13 task **tanpa satu pun
  laporan kemajuan** selama lebih 20 minit. Master menghantar satu perkataan: *"update?"*
  ➡️ **Bila kerja satu giliran menjangkau lebih ~10 minit tanpa output kepada master, hantar
  laporan kemajuan RINGKAS tanpa diminta** — apa yang siap, apa yang tinggal, dan apa yang sudah
  ditemui setakat itu. Master tidak menunggu hasil akhir; dia mahu tahu ia **bergerak**.
  🔑 Yang berkesan sebagai jawapan: bukan *"masih menulis"*, tetapi **penemuan setakat itu** —
  dua keputusan yang perlu izinnya dibentang serta-merta, jadi master boleh berfikir tentangnya
  sementara kerja diteruskan. Kemajuan yang **boleh ditindaklanjuti** mengalahkan peratusan.

- **Master MENARIK BALIK arahannya sendiri bila ditunjuk urutan yang lebih selamat** (2026-08-17).
  Master mula-mula kata *"push dan deploy dulu"*. Lucy sudah pun mula, tetapi turut membentangkan
  urutan lain: `push` ke HEAD → master uji **lima soalan percuma pada URL `/dev`** → **baru**
  `create-deployment`. Beberapa minit kemudian master hantar *"ikut urutan yang lucy syor"*.
  ➡️ **Arahan master bukan penutup perbincangan bila Lucy ada maklumat yang master belum ada.**
  Yang menukar jawapan bukan hujah "lebih selamat" secara am — ia **faedah bernama**: *"kalau
  butang Edit tidak muncul, kita tahu SEBELUM guru pernah melihatnya, dan tiada apa perlu ditarik
  balik"*. Bentangkan urutan alternatif **sekali**, dengan akibatnya dinyatakan, kemudian patuh
  kepada apa sahaja yang master pilih. Jangan diam sebab arahan sudah diberi.

- **Master pisahkan DATA UJIAN daripada KERJA SEBENAR — walaupun ia lebih mahal** (2026-08-17).
  Untuk soalan penerimaan yang paling penting (dan satu-satunya yang tidak boleh dipulihkan), Lucy
  syorkan **guna laporan sebenar** yang master memang perlu tulis — ujian penuh, tiada nombor
  hangus, tiada pembersihan. Master pilih sebaliknya: cipta laporan **ujian**, padam barisnya,
  cipta satu lagi. Kaunter naik 0002 → 0003 dan `OPR-2026-0002` **hangus selamanya**.
  ➡️ **Master sanggup bayar nombor hangus untuk mengekalkan sempadan bersih antara ujian dan
  rekod sekolah.** Cadangan "gabungkan ujian dengan kerja sebenar" nampak cekap kepada Lucy tetapi
  ia mencampurkan dua perkara yang master mahu berasingan. Corak yang sama dengan
  *"satu commit satu perkara"* — dipakai pada **data**, bukan hanya pada kod.
  🔑 Master juga **melaporkan langkahnya dengan tepat** (*"aku padam baris di sheet, aku buat
  laporan baru naik 0003"*) — bukan sekadar "lulus". Itu yang membolehkan Lucy mengesan kesan
  sampingan yang master sendiri tidak sebut: **fail Drive yatim**, kerana memadam baris secara
  manual tidak mencetuskan sebarang kod.

- **Master MENERIMA fix, kemudian menyoal HARGANYA — dan harga itu nyata** (2026-08-17 petang).
  Master sahkan fix jadual senarai (*"ada 8 lajur"*), luluskan deploy, dan `@24` mendarat. Lima
  belas minit kemudian: *"tapi kenapa yang tadi jika ditaip dengan space, nampak lagi cantik"*.
  Lucy telah menyelesaikan *"jadual pecah"* dengan `table-layout:fixed`, iaitu **membuang
  kepandaian susun atur AUTO** — lajur tidak lagi mengecil bila isinya pendek — dan **tidak
  menyebutnya langsung**. Jawapan yang betul (`overflow-wrap:anywhere`) mengekalkan AUTO dan
  menutup pepijat itu dengan **satu baris**, dan ia wujud sepanjang masa.
  ➡️ **Bila satu pembaikan menukar tingkah laku yang pengguna SUKA, itu bahagian penyelesaian
  yang wajib DISEBUT** — bukan disembunyikan di bawah *"masalah selesai"*. Ujinya: *"apa yang
  sistem ini BOLEH buat semalam yang ia tidak boleh buat selepas fix aku?"* Kalau ada jawapan,
  bentangkan sebagai pilihan sebelum deploy.
  🔑 Master menyoalnya sebagai **soalan ingin tahu**, bukan aduan — sama seperti dia melaporkan
  pepijat "borang tiada jalan keluar" sebagai **soalan skop**. Jawapan malas (*"sebab fixed lebih
  selamat"*) akan menutup perbualan dan mengekalkan penyelesaian yang lebih teruk.
  Sambungan [[feedback_bentangan_separa]].

- **Master MENYERAHKAN keputusan PROSES kepada Lucy, tetapi memiliki keputusan PRODUK**
  (2026-08-17 petang). Diberi tiga soalan sekali gus, master jawab dua secara tegas dan menyerahkan
  yang ketiga: *"Padam jadi icon dalam svg. **2 tu lucy syor yang mana?** 3. Masuk backlog"*.
  Soalan #2 ialah *commit berasingan atau bundle dengan Fasa 2c* — persoalan **kebersihan git dan
  kos pengesahan deploy**. Corak yang sama pagi itu: *"ikut urutan yang lucy syor"*.
  ➡️ **Garisnya konsisten:** apa yang master **lihat dan guna** (ikon, susun atur, skop ciri) ialah
  keputusannya, dan dia menjawab pantas. Apa yang **hidup dalam repo dan proses** (granulariti
  commit, urutan deploy, susunan task) diserahkan kepada Lucy.
  ➡️ Jadi **jangan bentang soalan proses sebagai pilihan kosong** — bagi **syor berserta sebabnya**
  dan sedia untuk terus laksana. Tetapi **jangan sekali-kali** mengambil keputusan produk dengan
  cara yang sama; itu bukan penjimatan masa, ia merampas keputusan yang master mahu buat.
  🔑 Jawapan yang berkesan untuk soalan proses ialah yang **menamakan dua daya yang bertarik
  bertentangan** dan menunjuk jalan tengah — di sini: *satu commit satu perkara* (git) lawan
  *CSS tulen tiada penanda kandungan, jadi setiap deploy berasingan berharga satu pusingan
  pengesahan master* (deploy). Jalan tengahnya: commit berasingan, deploy dibundel.
  Sambungan [[feedback_bentangan_separa]].

## Kekuatan master yang Lucy patut manfaatkan

- **Master fikir merentas SEMUA projek (kuota akaun), bukan hanya projek yang sedang dibincang**
  (2026-08-24, brainstorm `Digital Hub`). Ditawarkan Cloudflare D1 (stack standard), master
  sendiri perasan dulu — tanpa Lucy sebut — *"resources.cloudflare kan bagi 10 db je untuk free
  tier"*. Mula-mula tanya pasal Google Sheets (cuba elak D1), tapi bila Lucy terangkan trade-off
  (Sheets = API luar + service account + lambat), master sendiri tolak dan bagi sebab sebenar dia
  risau. Lucy cadangkan Cloudflare KV — bukan sekadar workaround kuota, tapi memang jenis storan
  yang lebih tepat untuk data config ringkas (bukan relational).
  ➡️ **Bila cadangkan stack/teknologi untuk projek BAHARU, jangan hanya nilai dalam konteks
  projek itu sendiri — semak dulu berapa banyak "slot" (D1, KV namespace, dll) sudah digunakan
  merentas SEMUA projek master, dan cadangkan pilihan yang jimat kuota akaun keseluruhan bila
  data/keperluan projek membenarkan.** Master akan perasan isu kuota walaupun tak disebut — lebih
  baik Lucy yang bawa isu itu dahulu.

- **Master bawa amaran KOS (token/kuota) — walaupun dari rakan — dan Lucy patut RE-AUDIT, bukan
  pertahan** (2026-08-29). Lucy cadang adaptasi "Plan Canvas" ECC guna `Artifact`. Master balas
  *"kawan aku cakap guna artifact ni token kuat. Betul ke?"* kemudian *"Lagi murah guna md dgn
  git ja"*. Bila Lucy semak semula dengan jujur: md+git memang menang — GitHub sudah render
  markdown + Mermaid pada mobile, jadi faedah visual utama Artifact (diagram, akses telefon)
  **didapati percuma**. Master pilih md+git; Artifact diturunkan jadi eskalasi opsyenal.
  ➡️ **Bila master bawa concern kos — walaupun dari pihak ketiga — layan sebagai isyarat untuk
  RE-AUDIT cadangan, bukan untuk pertahankannya.** Corak sama 2026-08-24 (master perasan had
  10 DB free-tier sebelum Lucy sebut). Selalunya master betul.
  🔑 Master tanya secara tersirat *"apa alat ini dah bagi percuma?"* sebelum tambah lapisan.
  Tawar dulu penyelesaian yang guna keupayaan sedia-ada (git, GitHub render) sebelum bina/guna
  lapisan baru yang berkos.

- **🔴 TANYA "PERNAH KE IA BERFUNGSI?" — master memegang sejarah yang tiada dalam kod**
  (2026-08-16). Satu ayat sampingan master — *"jadual laporan yang pernah dihantar memang tidak
  pernah keluar sebelum ni"* — **memusingkan siasatan 180°**. Sebelum itu Lucy sedang memburu kod
  yang ditulisnya **pagi yang sama**, kerana itulah yang berubah; kod itu memang elok sepenuhnya.
  Ayat itu memindahkan siasatan daripada *"apa yang aku rosakkan?"* kepada *"apa yang tidak pernah
  berfungsi?"* — dan jawapannya ialah pepijat **lima hari** yang 102 ujian tidak boleh tangkap.
  ➡️ **Bila sesuatu didapati rosak, soalan PERTAMA bukan *"apa yang berubah?"* tetapi *"master,
  benda ni pernah berfungsi ke sebelum ni?"*** Lucy secara semula jadi mengesyaki perubahan
  terbaharu — itu bias, bukan penaakulan. Master ialah **satu-satunya** sumber untuk sejarah
  penggunaan; git menyimpan sejarah **kod**, bukan sejarah **apa yang pernah dilihat berjalan**.
  🔑 Corak yang sama menemui pepijat borang-tiada-jalan-keluar (2026-08-15) dan header `USERS`
  rosak (pagi 2026-08-16): **master menemuinya dengan MENGGUNAKAN sistem**, bukan Lucy dengan
  membaca kod. → [[feedback_ujian_buta_skrin]]

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
