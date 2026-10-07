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
- `opr-program` (migrate AppSheet OPR SK Salor → Apps Script. **2026-09-28: LIVE guru `@30`**
  — rangka muat (skeleton) ganti skrin putih, suite 617/617. Master di telefon: masih "putih dulu
  sekejap baru rangka" — putih itu SEBELUM HTML diurai (HTML ~670 KB, 82% pustaka PDF yang cuma
  dipakai masa Hantar). SAMBUNG: menunggu "ya" utk deployment UKUR sementara. Smoke `@29`
  tertunggak. Backlog: 3 PDF contoh kualiti. Butiran: `opr-program/MEMORY.md`, `current-session.md`)
- `opr-insaniah` (**2026-10-07 12:4x: kod `@61` DISEDIAKAN (cabang `nilai-sebenar` @`765fae1`): 14 nilai sebenar + ikon SVG `✕` + `BersihService.gs` (padam semua laporan); deploy MENUNGGU master di editor (migrasiNilai ×2 → kiraBersihLaporan → bersihkanSemuaLaporan); guru kekal `@60`.** Sebelum itu: **2026-10-07 10:0x: guru `@58` LIVE — butang 📖 Panduan (pautan Drive); cabang `pautan-panduan` belum digabung; ujian peranti menunggu master.** Sebelum itu: **2026-10-06 23:5x: guru `@55` LIVE — input Masa = dua pemilih jam (Mula+Tamat), Tamat boleh kosong, Tamat<=Mula ditolak; suite 958/0; dipush origin. Ujian `@54` lulus iPad; ujian `@55` "dibuat di iPad" — keputusan TIDAK dinyatakan master. Butiran: `opr-insaniah/MEMORY.md`.** Sebelum itu: **2026-10-05 22:3x: KELAS branch `medan-kelas` @`3b04b4c`, 689/0 — butang "Papar (N)"+modal ganti cip jadual (keputusan master); belum deploy, T9 menunggu "ya"**; sebelum itu: guru `@48` — pagar jenis+saiz fail + kecilkanImej tukar format; ujian peranti menunggu master. Langkah 3 lazy-load DITUTUP selepas ukur. Butiran: `opr-insaniah/MEMORY.md`.) Catatan lama: (OPR Pembangunan Karakter Insaniah — **Google Apps Script** terikat pada Sheet,
  bukan Hono/Workers. **BELUM launch rasmi ke sekolah** (dibetulkan master 2026-09-01 — catatan
  lama "sekolah sudah guna aktif harian" SALAH). **2026-08-25:** siri 4 fix guna sistem sebenar
  (fon PDF tak konsisten, tab blank iPhone Chrome, jadual Senarai Laporan terpicit iPad menegak,
  warna label kelabu) — deploy `@37→@44`, suite 526/526. Fasa 2b Edit+Padam laporan masih belum
  dirancang. Butiran penuh: `opr-insaniah/MEMORY.md`)
- `takwim-digital` (Apps Script + Google Calendar, akaun DELIMa. **2026-09-20: LIVE production
  `@26`** (2026-10-04) — butang "Hantar Sekarang" di System Settings (admin sahaja) utk hantar digest
  manual bila terlupa tanda Kongsi; sebelum itu `@25` swipe kalendar + animasi. Google Chat
  disahkan berjaya. Suite 124/124. Butiran: `takwim-digital/MEMORY.md`)
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

- **Master minta drift dokumen dibetulkan DULU, serta-merta — dan Lucy patut BAWA isu drift itu
  sendiri** (2026-09-28). Brief sesi menyebut `reminders.md`/`project-list.md` lapuk (opr-program
  `@17` padahal `@29`); master jawab satu baris *"Betulkan yang lapuk tu dulu"*, kemudian *"Ya commit
  push"* — tiada minta plan. Sokong corak 2026-08-10 (dokumen dibundle, murah). Semakan git sebelum
  menulis menemui LAGI tiga kesilapan yang master tak tunjuk (takwim `@22`→`@25`, digital-hub
  "brainstorming"→LIVE, baris insaniah "C1 DIHOLD sama" yang sudah dibetulkan 2026-09-18).
  ➡️ Bila brief menemui satu drift, **audit fail sejenis lain sekali** lawan `git log`, bukan hanya
  yang disebut. Dan ia terpakai pada auto-memory index juga, bukan hanya fail Lucy.

- **Master memilih DEPLOY dan LIHAT di telefon, bukan ukur dulu — dan hasilnya memang berguna**
  (2026-09-28). Ditawar dua jalan (deploy `@30` sekarang / ukur dulu), master jawab *"Ok deploy 30"*.
  Hasil satu ayat: *"keluar skrin putih dulu sekejap baru ada kad kelabu"* — mengesahkan bahagian
  yang Lucy sudah amaran (skeleton tak boleh sentuh masa sebelum HTML diurai) dan memberi satu
  fakta yang tak boleh diperoleh dari kod: putih itu BETUL-BETUL wujud pada peranti master.
  ➡️ Sambungan [[feedback_soalan_reka_bentuk_contoh]] / keputusan visual dari MELIHAT: untuk
  perubahan berisiko rendah dan boleh diundur, deploy dahulu ialah cara termurah mendapat
  kebenaran dari telefon. Master lapor **tanpa sebut peranti** — Lucy patut minta peranti
  SEKALI masa menyerahkan tugasan smoke (sudah dibuat), bukan tunggu.

- **Master betulkan HIPOTESIS Lucy dengan fakta dunia-sebenar yang kod tak simpan** (2026-10-04).
  Lucy cadang butang manual dan sebut penanda `DGSENT_` mungkin terbakar (kerana 13 Sept). Master
  jawab satu ayat: *"bukan digest tak berfungsi, cuma aku lupa kongsikan aktiviti minggu ini"* —
  punca sebenar ialah tiada aktiviti bertanda, bukan penanda. Gejala (Executions ~4s, tiada mesej)
  SAMA untuk dua punca berbeza. ➡️ Sebelum menyiasat kod, tanya fakta tindakan master
  ("ada tanda Kongsi?"). Sambungan [[feedback_tanya_pernah_berfungsi]].
- **Master lapor RUPA dengan soalan pendek, dan Lucy patut baca CSS — bukan agak** (2026-10-04).
  *"warna button tu memang macam tu ke?"* lalu *"takde pun garis biru"*. Dua pusingan sebab Lucy:
  (1) guna `class="btn"` tanpa semak `.btn` tiada warna; (2) menerangkan `secondary` sebagai "putih
  bergaris" tanpa sebut garisnya (#e5ebf3) hampir tak nampak. Jawapan terbaik ialah *"silap saya,
  sebabnya X"* + 3 pilihan bernama, kemudian master pilih dengan satu angka ("1"). Ujian sumber
  tak nampak skrin — tuntut kelas/warna dalam ujian. → [[feedback_ujian_buta_skrin]]
- **Master "Deploy production" selepas uji di `@HEAD` dan puas hati** — dua perkataan eksplisit,
  tiada soalan lanjut (2026-10-04). Corak sama [[feedback_deploy_confirmation]]: Lucy tunggu frasa itu.
- **Master deploy SEBELUM uji peranti — bila TIADA pengguna sebenar, dan dia yang nyatakan sebabnya** (2026-10-05).
  Lucy tawar uji `@HEAD` dahulu; master jawab *"Deploy production dulu pun ok kan. Tiada user lagi"*. Itu izin
  eksplisit (bukan "Ok" ringkas), dan sebabnya fakta dunia-sebenar yg kod tak simpan (projek BELUM launch rasmi).
  Selepas deploy master kembali dgn *"1234 ok"* — empat semakan peranti dilaporkan sebagai satu baris.
  ➡️ Peraturan "tunggu frasa deploy" kekal. Tetapi bila belum ada pengguna, risiko hanya pada master → ia boleh
  memendekkan urutan. Lucy tetap beri **senarai ujian bernombor** supaya "1234 ok" bermakna sesuatu, dan sediakan
  jalan balik (`@44`). Jangan anggap izin ini berpindah ke projek yg sudah ada guru.
- **Master mahu pepijat yang Lucy temui DIBAIKI — bila ditawar sebagai commit berasingan** (2026-10-05).
  Lucy jumpa `.sorok` kalah `display:` lain semasa kerja lain, TIDAK bundle, tawar berasingan; master: *"Teruskan baiki
  pepijat"*. Corak 2026-08-09/10 (satu commit satu perkara) bertahan. Imbas KELAS masalah, bukan satu kes: yang kedua
  (`#jadualSenarai` dlm @media, telefon/iPad sahaja) lebih serius drpd yang nampak dlm screenshot.

- **Master luluskan dengan "ikut syor" — dan itu sah bila syor Lucy sudah dinyatakan berserta sebab** (2026-10-05).
  Tiga kali dalam satu sesi: pilihan ukuran, plan pagar fail (dua keputusan: logo PNG/JPEG + had 15/5/2 MB), deploy `@48`. Ditawar sebagai
  *"syor saya X kerana Y"* + soalan produk dipisahkan jelas. Master juga gabungkan jawapan (*"Ya dan proceed a"* = padam deployment + mula plan A).
  ➡️ Susun mesej: syor ditulis eksplisit supaya "ikut syor" tidak ambigu; soalan produk (logo, had saiz) lain daripada soalan proses (urutan deploy).
  Tetapi bila mesej ada BANYAK soalan, tafsir "ya/ikut syor" kepada yang berkaitan SAHAJA dan nyatakan tafsiran itu (dibuat sesi ini: "langkah 3 = senarai semak sahaja").
- **Master memberi data ukuran dengan cepat dan tepat — sertakan PERANTI bila diminta** (2026-10-05). Telefon (Honor 50) dua larian + laptop dua larian
  dalam ~10 minit. Lucy terlupa minta peranti pada kali pertama (data tanpa peranti tak boleh direkod) — minta SEKALI, di dalam arahan ukuran.
- **Master tidak menolak penemuan security yang Lucy bawa sendiri — ia terus dijadikan plan** (2026-10-05). Isu `mime` client dipercayai ditemui semasa
  menulis senarai semak launch (bukan diminta) dan master terus pilih baiki sebelum guru pertama. Corak: bawa isu + keparahan JUJUR (rendah–sederhana) +
  cadangan; jangan bengkakkan atau sembunyikan.

- **Master UBAH jawapan sendiri sebaik melihat pilihan konkrit, dan rujuk fitur projek LAIN dengan nama tempatan** (2026-10-05 malam). Medan KELAS:
  jawab "satu kelas sahaja", dua soalan kemudian "guru boleh pilih lebih daripada 1" selepas Lucy tunjuk 18 pilihan dikumpul ikut tahun. Kemudian
  *"kalau buat macam checkbox anjuran?"* — nama itu TIADA dalam projek yang sedang dikerjakan; ia dari `opr-program`. Lucy grep (0 padanan), TANYA
  dan bukan teka; jawapan: *"medan anjuran tu di projek opr program"* → modal+chip sedia ada.
  ➡️ Jangan kunci jawapan awal sebagai spec; bentangkan implikasi (jadual "satu vs berbilang") bila ia berubah. Bila master rujuk nama yang tak
  dikenali, grep projek semasa dahulu, lepas itu tanya — jangan reka makna. Master memilih UI yang dia SUDAH BIASA lihat (konsisten antara projek).
  Master juga minta "Save memory dan session dulu" di tengah brainstorming — titik berhenti semula jadi sebelum spec.
- **Master menilai UI daripada TANGKAPAN SKRIN, dan menolak reka bentuk yang lulus semua ujian** (2026-10-05 malam, KELAS). Suite 676/0 + final review bersih,
  tetapi lihat jadual desktop dengan 18 cip → *"memang tak ok"*, terus beri penggantinya (butang "Papar" + modal Tutup). Gerbang "master tengok skrin"
  BERGUNA — ia menangkap apa yang tiada ujian boleh. Master memberi **arahan konkrit** (letak butang di kolum Kelas, modal + butang Tutup), bukan
  sekadar "tak ok" ⇒ bentang pelan ringkas (jadual sebelum/selepas) + 2 soalan kecil bersyor lalai, tunggu "Proceed". Master juga minta **ukur semula**
  (telefon 390px) bukannya percaya tangkapan lama — Lucy jumpa tangkapan lama artifak alat (Edge ≥500px) dan sahkan dgn iframe 390px sebenar.
  → [[feedback_ujian_buta_skrin]]

- **Master tanya KEPUTUSAN rupa dengan soalan pendek, dan nombor yang Lucy janji akan DISEMAK terhadap tangkapan** (2026-10-06 petang, panel Kelas).
  *"panel telefon 390 tu tak jadi panjang sangat ke?"* → Lucy baca tangkapan sendiri (bukan agak). Lucy janji baris ~55px, tangkapan baharu tunjuk ~80px
  — dilapor JUJUR sebelum master memutuskan; master terima ("1. cuma fix sikit susunan"). Soalan kuantiti ("26 kelas lagi panjang?") dijawab dgn jadual
  anggaran + syor, bukan pilihan kosong; master ambil "1+2 ikut syor". Master tambah permintaan kecil produk (susun tahun 1→6) tanpa ditanya — Lucy
  tafsir "semua paparan" dan NYATAKAN tafsiran; master sahkan ("Ya, susun semua paparan"). ➡️ Jangan janji nombor piksel/ukuran sebelum ukur; sebut tafsiran
  bila arahan kabur, jangan senyap.
  🔑 Master sahkan Sheet ("benih 18 kelas lulus") dan beri "proceed t9" dlm satu mesej — gerbang deploy jelas, sama corak "ya deploy".

- **Master kerap bekerja daripada TELEFON — arahan `!` shell tidak boleh dijalankan** (2026-10-06 petang). Selepas "ya deploy" Lucy minta master
  `! clasp push`; master menaip baris itu tetapi ia sampai sebagai TEKS BIASA (tiada output) dan kemudian bertanya *"Aku guna phone, tk boleh ke?"*.
  ➡️ **Tanya/ingat peranti master SEBELUM memberi langkah yang memerlukan terminal.** Kalau telefon, Lucy yang jalankan (cuba sekali secara biasa;
  jangan pintas jika ditolak) — dan **SAHKAN tindakan benar-benar berlaku** (pull ke folder sementara, grep penanda) sebelum langkah seterusnya.
  Kesunyian selepas arahan ≠ kejayaan. Master bertanya soalan "tak boleh ke?" sebagai soalan ingin tahu, bukan aduan.
- **Master menerima bundle permintaan dalam SATU mesej dan menetapkan tertib:** *"Looks good. Ya deploy. Baiki baki tu dalam commit berasingan. Save memory dan session"*
  (2026-10-06). Corak "satu commit satu perkara" bertahan: baki yang Lucy tawar sebagai pilihan berasingan diambil, tetapi **deploy kekal bagi
  fitur yang master LIHAT** — baki menunggu "ya deploy" sendiri (`@52`). Lucy tidak menyelit baki ke dalam deploy yang diluluskan.
- **Master bertanya soalan pengesahan fakta dalam bahasa ringkas — jawab dengan KOD, bukan ingatan** (2026-10-06): *"Max gambar bagi opr insaniah 2 je ya?"*
  → Lucy grep `PERATURAN_GAMBAR` dan petik `Validate.gs:31`, serta nyatakan client baca dari `SESI.peraturanGambar`. Sambungan [[feedback_dakwaan_melebihi_bukti]].

- **Master menemui apa yang 906 ujian tak nampak — dengan MENGGUNA di telefon — dan melaporkannya jujur termasuk "lama baru perasan"** (2026-10-06 malam).
  Penolakan gambar ke-3 betul tetapi mesejnya di bawah butang Hantar; master menyangka ia ditolak senyap. Dia terus memberi **penyelesaian konkrit**
  (toast/kotak tengah skrin, tutup bila OK) dan **izin deploy `@52` dalam mesej yang sama** ("Fix, then deploy semula. Baru deploy @52") — jadi Lucy
  tidak bertanya semula. ➡️ Bila arahan sudah memuat "fix" + "deploy" + nombor versi, itu izin eksplisit; tetap lakukan pra-terbang + sahkan push.
  🔑 Master menilai mesej UI dari SUDUT PENGGUNA ("user tak nampak"): sebelum menulis mesej penolakan, tanya di mana mata pengguna ketika itu.
  Sambungan [[feedback_ujian_buta_skrin]].
- **Master kini melaporkan PERANTI tanpa diminta, dan "Proceed" satu perkataan = syor terakhir Lucy** (2026-10-06 malam). *"Uji @52 lulus di ipad dan phone"* —
  kedua-dua peranti disebut (tabiat yang Lucy tuntut sejak [[feedback_laporan_manual_peranti]]; kini sendiri). *"Proceed"* selepas Lucy menyenaraikan dua
  tertunggak ⇒ Lucy tafsir sebagai gabung+push (tindakan Lucy, risiko rendah, tidak menyentuh production) dan NYATAKAN tafsiran itu. Betul. Tetapi
  deploy/menyentuh sekolah tetap tunggu frasa jelas — "Proceed" tidak cukup untuk itu.

- **Master membetulkan hipotesis subagent dengan data PERANTI, dan menolak bila kos pilihan dibentang — tetapi menimbulkan soalan domain sendiri** (2026-10-07 malam, PDF iOS).
  Subagent opus (read-only) syor "luaskan regex ke Safari"; master: Chrome pun kena (*"chrome di ipad tiada tab kosong tapi masih kena klik 2 kali"*) ⇒ Lucy baca kod sendiri sebelum plan,
  bentang DUA peringkat (percuma lawan ubah #29), master "1 dulu". Ujian: Chrome satu klik tetapi perlu enable pop-up; Safari/web app dua klik — hasil dilapor TEPAT dengan peranti+pelayar tanpa diminta.
  Master tanya sendiri *"kalau dikongsi anyone masalah selesai kan?"* → Lucy jelaskan yang menyelesaikan = pautan siap SEBELUM klik, bukan "anyone" (gambar murid!); master kemudian nampak sendiri
  *"domain with link cikgu je boleh tengok, PPD guna moe.gov"* — had domain yang Lucy tak sebut. ➡️ Lucy jelaskan ia SUDAH berlaku sekarang & ASING daripada masalah klik; JANGAN campur dua masalah; tanya fakta dunia-sebenar
  (PPD terima OPR macam mana?) sebelum reka. Master berhenti dgn *"Save memory dan session dulu"* ~00:40 — titik berhenti semula jadi; kerja berbaki ditandakan menunggu keputusan A/B.
  🔑 Bila master tanya "kalau X, masalah selesai kan?" — soalan reka bentuk disamar: jawab mekanisme sebenar yang menyelesaikan, kemudian harga X secara bernama (di sini: keselamatan gambar murid).

- **Master memutuskan keputusan PRODUK/KESELAMATAN sendiri dan Lucy MENGIKUT — selepas harga disebut SEKALI** (2026-10-07 01:0x, ANYONE_WITH_LINK).
  Master: *"Ambil keputusan kongsi pautan pdf tu anyone with link. So masalah selesai"* — mengatasi keputusan #29 (DOMAIN) yang Lucy sendiri tulis sebagai syarat keselamatan.
  Lucy tak membantah; sebut harga SEKALI (gambar murid, pautan terlepas), tambah langkah 0 (uji DELIMa) dan teruskan. Dua perkara betul:
  (1) *"So masalah selesai"* separuh benar — ANYONE selesaikan PPD, BUKAN dua klik iOS; Lucy jelaskan mekanisme (pautan mesti sedia SEBELUM klik) dengan membaca kod, bukan mengangguk;
  (2) master luluskan **plan lebih besar daripada frasa**: *"pasang perkongsian semasa cipta"* — Lucy tafsir sebagai kod+ujian (bukan deploy) dan NYATAKAN tafsiran itu.
  ➡️ Tafsir kelulusan pendek **secara sempit** pada tindakan; gerbang destruktif/luar kekal frasa jelas. Di sini master sendiri beri *"Deploy and push"* (jelas) — tetapi Lucy tetap TAHAN create-deployment kerana
  gerbang teknikal (DELIMa benarkan ANYONE? PDF lama masih peribadi) belum lulus; master jalankan migrasi dari editor (angka dilapor tepat: `dikongsi:5`) ⇒ deploy. **Izin deploy tidak menghapus gerbang fakta.**
  🔑 Master melapor ujian dengan peranti+pelayar tanpa diminta (*"ipad safari chrome dan android"*) — tabiat tetap; PPD tidak disebut ⇒ catat sebagai BELUM disahkan, jangan andai.
  🟡 Master bekerja malam (00:45–01:30) dalam kerja pendek-pendek; Lucy tawar titik berhenti tiap fasa. Master tak ambil tawaran berhenti — teruskan ke deploy sebelum tidur.

- **Master menolak penyelesaian yang "rumit" — dan Lucy patut BENTANG YANG TERMUDAH DAHULU** (2026-10-07 pagi, butang Panduan). Lucy bentang varian A (kotak pautan dalam Tetapan admin) sebagai syor, dgn satu soalan; master: *"Macam rumit je. So sekolah yang make a copy tiada panduan la kan"*. Lucy tukar syor kepada B (satu pemalar, KISS) dan master terus *"Proceed B"*. 🔴 Andaian master TERBALIK (dgn B sekolah yang copy DAPAT panduan, dgn A mereka TIADA) — dibetulkan dgn perbandingan A lawan B dalam SATU mesej pendek, bukan hujah panjang. ➡️ Bila ada dua cara dan satu jelas lebih kecil, syorkan yang kecil DAHULU dan sebut harga yang dibayar (tukar pautan = deploy; jangan padam fail). Soalan "kenapa rumit?" = isyarat KISS, sambungan [[feedback_kiss_dry]]. Master juga memilih mekanisme berdasarkan KOS PENGGUNA masa depan (*"hantar PDF boleh hilang atau risiko susah nak rujuk semula"*), bukan kos pembangunan — pautan dalam app > WhatsApp.
- **Master BEKERJA REMOTE (laptop di rumah) dan tak boleh muat naik fail dari komputer** (2026-10-07 pagi): *"Aku tak boleh muat naik pdf, aku bekerja remote, laptop kat rumah. Kat mana kau boleh send pdf tu"*. Lucy tak tahu — fail PDF hanya wujud di laptop. ➡️ `SendUserFile` (alat sesi) menghantar fail sebagai kad muat turun; master simpan lalu muat naik ke Drive dari telefon dan balas pautan dalam satu mesej. **Tanya/ingat LOKASI master (rumah/remote) bila tugasan perlukan fail dari mesin Lucy**; sediakan juga laluan GitHub (repo private, master boleh buka sendiri) sebagai sandaran. Master juga tanya *"Pilihan 1 tu repo github. Io ke"* — istilah teknikal ("simpan sumber dalam repo") perlu dijelaskan dgn akibat guru: repo private ⇒ guru tak nampak ⇒ perlu laluan lain.
- **Master jawab fakta dunia-sebenar dgn pendek dan beri keputusan bernombor** (2026-10-07 08:4x): *"1. Biasa share link je atau upload semula ke folder drive yang dishare. 2. Aku uji nanti, tiada iphone. Laptop ok. 3.proceed 4.biar dulu takpe. 5, panduan tu nak letak dimana. 6 ok tangguh. 7 tangguh dulu"*. Enam item dijawab satu-satu ikut nombor Lucy; satu soalan balas (#5) ialah soalan DOMAIN yang Lucy patut terangkan, bukan jawab "ya". ➡️ Nomborkan senarai tertunggak dgn jelas — master menjawab ikut nombor. Fakta PPD (*share link / folder Drive dikongsi*) mengesahkan ANYONE_WITH_LINK; akaun PPD sebenar tetap belum diuji.
- **Master luluskan dua gerbang dalam dua mesej ringkas: *"Proceed B"* (kod) dan *"Ya deploy, save memory dan session"*** (2026-10-07). Lucy mulakan kod+ujian sahaja pada "Proceed B" (tafsiran sempit; panduan & pautan menyusul), dan deploy hanya pada "Ya deploy" jelas. Master juga luluskan pembetulan docs (CLAUDE.md drift) dgn *"Ya betulkan"* — Lucy bawa isu drift itu sendiri; sambungan corak 2026-09-28 (bawa drift sendiri).
- **Master hantar RUJUKAN luaran (carousel) + screenshot dgn arahan pendek, dan "Ok" = ambil syor Lucy** (2026-10-07 pagi, butang). "Nk fix button ni, nampak tak konsisten. Rujuk gambar" (1 screenshot app + 9 slaid UI Buttons). Lucy baca kod DAHULU, bentang jadual "nampak di skrin ↔ punca dalam kod" + plan + 2 soalan BERSYOR (skop sempit, emoji kekal); master "Ok" ⇒ kedua-dua syor. Kemudian di tengah kerja: *"Simpan rujukan carousel untuk rujukan merentas projek"* ⇒ disimpan sebagai memory rujukan (bukan hanya projek ini). Lepas tangkapan 390px dihantar (`SendUserFile`, master remote): *"Ya deploy. Save memory dan session"* — deploy+simpan dlm satu mesej.
  ➡️ Rujukan yang master share = aset merentas projek, tulis di auto-memory (reference_*), bukan MEMORY projek sahaja. Skop butang: sempit dahulu (konsisten dgn "satu commit satu perkara"). Lucy pilih "tunjuk tangkapan SEBELUM tanya deploy" — master terus luluskan.

- **Master tulis TEKS amaran SENDIRI bila Lucy beri draf, dan "jangan lagi" = TAHAN — kemudian lepaskan dengan jelas** (2026-10-07). Lucy beri draf amaran; master ganti dengan versi lebih rasmi (*"pegawai/pihak berautoriti… orang awam/pihak luar"*) — teks yang melibatkan pihak luar/keselamatan ialah MILIK master, bentang draf sebagai cadangan sahaja. Soalan bernombor dijawab ikut nombor ("1. Looks good 2. jangan lagi 3. aku tak jumpa…"); "jangan lagi" selepas soalan deploy ⇒ TAHAN (Lucy tafsir dan NYATAKAN tafsiran), dan ~12 minit kemudian *"Deploy dan save memory dan session"* ⇒ izin jelas. "Simpan penjana ikut syor" meluluskan folder BAHARU `tools/` (pantang larang restructure) — syor yang dinyatakan eksplisit sah sebagai kelulusan. 🔴 "Tak jumpa kat mana" (Manage versions) ialah soalan prasyarat/laluan: beri dua laluan konkrit; jawapan sebenar master = browser berjaya, **app Drive TAK boleh** (peranti penting — catat). ➡️ Jangan andai master boleh buat langkah Drive di app; sebut "guna pelayar" dari awal.

- **Master beri satu mesej tiga keputusan bernombor, dan menerima tawaran kerja kecil Lucy dengan "buat kerja svg lucy cakap td"** (2026-10-07 tengah hari, NILAI sebenar). Selepas Lucy bentang plan A/B/C (tiga komit berasingan) + 4 soalan bernombor, master jawab ikut nombor: *"1. Padam semua laporan, bersihkan juga gambar yatim dan fail yatim. 2 buat kerja svg lucy cakap td. 3. Ni senarai nilai sebenar"*. Item 2 ialah tawaran Lucy sendiri sebelum itu (`✕` teks pada cip) — master mengambilnya kerana Lucy yang membawa isu itu ⇒ sambungan corak "bawa isu + syor, master ambil".
  ➡️ **Senarai fakta DOMAIN yang master taip sendiri (nilai) mengandungi kesilapan ejaan** ("Barterima", "Intergriti", "fan") — Lucy TUNJUK pembetulan dan minta persetujuan, bukan betulkan senyap; master "Proceed ikut syor" (menerima pembetulan). Teks yang master sumbangkan = miliknya (sama corak teks amaran 2026-10-07); bentang pembetulan sebagai cadangan.
- **Master tanya "A tu selamat tak?" — soalan KESELAMATAN pendek sebelum izin tindakan; jawab dgn BUKTI dari kod, bukan jaminan am** (2026-10-07). Lucy grep `Validate.gs:43` (NILAI senarai bebas, bukan tertutup) + jelaskan laporan simpan TEKS ⇒ "ya, selamat, risiko rendah" + dua risiko tinggal bernama. Master terus "Proceed ikut syor". ➡️ Soalan "selamat tak?" = mahu SEBAB boleh diperiksa; jawapan "ya" tanpa rujukan kod tidak cukup.
- **Master betulkan Lucy dgn soalan separa-tepat: "butang gambar bukan dah ada icon x tu?"** (2026-10-07). Separa betul: ikon GAMBAR di kiri memang SVG, tetapi `✕` di kanan masih aksara teks. Lucy BACA `app.js.html:2582,2594` dahulu, jawab "betul sebahagian" + tunjuk baris. ➡️ Bila master "kata bukan dah ada X?" — dia mengingat rupa, bukan kod; semak KOD sebelum bersetuju atau menyangkal, dan nyatakan bahagian mana betul.
- **Master "Deploy a+c dulu dan terus b" — izin deploy eksplisit DAN arahan susunan** (2026-10-07). Lucy tafsir sempit: deploy A+C (izin jelas), B bermula selepas. Gerbang fakta tetap terpakai: `migrasiNilai` hanya pemilik boleh jalankan di editor ⇒ `create-deployment` menunggu log master. Kod B ditulis semasa menunggu (tiada kesan guru). Soalan terbuka satu ("KAUNTER reset?") dijawab *"Reset"* satu perkataan = syor Lucy (syor dinyatakan eksplisit sebelumnya). ➡️ Bila ada dua langkah bergantung (A+C deploy menunggu master di editor, B tulis sendiri), kerjakan bahagian yang BOLEH tanpa menunggu dan laporkan jelas apa yang master perlu buat — dgn jangkaan log supaya "ok" bermakna sesuatu.

- **Master tanya soalan keputusan-release dalam bentuk PENGESAHAN ("Dah boleh release untuk cikgu guna kan?") — Lucy tidak boleh sekadar mengangguk** (2026-10-07 petang). Soalan disusun supaya jawapan "ya" mudah, tetapi keputusan launch ada syarat yang master sendiri tulis (CLAUDE.md "Bersyarat: timbang semula risiko sebelum launch rasmi"). Lucy BACA `docs/launch-checklist.md` + audit item ⬜ yang tinggal (XSS senarai) SEBELUM menjawab, kemudian bentang: apa yang sudah ✅ (bukti), dan 4 syarat yang MASTER putuskan (harga ANYONE, USERS, ujian akaun biasa, guru pertama). ➡️ Soalan "dah boleh X kan?" = mahu pintu dibuka; jawab dengan senarai bukti + baki bernombor, dan buat sendiri kerja baki yang BOLEH (audit/kemas dokumen) supaya baki master sekecil mungkin. Jangan beri "ya" kosong pada keputusan yang membabitkan data murid. Master menjawab senarai bernombor satu-satu ikut nombor ("1 dah bersih. 2 ok. 3 ok nnti aku test") dan melaporkan peranti sendiri tanpa diminta (laptop Chrome + Android Chrome) — tabiat tetap. Master juga hantar tangkapan skrin Sheet (3 tab) sebagai BUKTI apabila ditanya "apa dah editor 1–3?" — bukti dari sumber (Sheet) mengalahkan ingatan; Lucy baca baris kiraan (37 = 1+4+18+14) bukan sekadar "nampak betul".

- **Master KEKALKAN keputusan berisiko selepas harga dinyatakan, dengan satu perkataan — dan itu keputusan PRODUK/KESELAMATAN yang Lucy ikut** (2026-10-07 19:57). Soalan "dah boleh release?" dijawab Lucy dengan 4 syarat; master: *"1 kekal"* (sahkan `ANYONE_WITH_LINK` selepas membaca harganya), *"2 nanti aku buat"*, *"3 ok"*. Sambungan corak 2026-10-07 01:0x (master memutuskan keselamatan sendiri, Lucy sebut harga SEKALI dan patuh). ➡️ "3 ok" TIDAK jelas: tawaran/permintaan, atau persetujuan akan buat? Lucy tafsir "master akan uji" dan NYATAKAN tafsiran + catat sebagai belum dilapor — bukan "lulus". Syarat ke-4 (guru pertama/saluran sokongan) tidak dijawab — jangan andai dijawab; tanya sekali lagi bila master kembali dengan USERS.

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

- **Master tak jumpa sesuatu ⇒ biasanya LUCY belum buat langkah prasyarat, atau laluan tersembunyi** (2026-10-06).
  Dua kali dalam satu sesi: master "tak jumpa tangkapan" (Lucy beri laluan `%TEMP%` dalam `AppData` yang
  tersembunyi) dan "tak jumpa `migrasiKelas` dalam editor" (Lucy belum `clasp push` — tersilap tertib).
  ➡️ **Sebelum arahkan master ke sesuatu: pastikan ia WUJUD di tempat itu, dan beri laluan yang boleh DIBUKA
  (buka Explorer sendiri, bukan taip laluan tersembunyi).** Soalan "tak jumpa" ≠ master silap.
  🔑 Tabiat baik master yang dikekalkan: beri "ya deploy"/"ya gabung" **jelas** sebelum setiap tindakan keluar;
  jawapan ujian peranti ringkas ("semua lulus") tanpa sebut peranti — tanya peranti bila perlu catatan.
  🟡 Classifier auto-mode menolak `clasp push` daripada Lucy; master jalankan sendiri dgn `!` (Git Bash —
  elak `cd C:\...`). Lucy tak pintas penolakan itu; jelaskan sebab + beri arahan tepat sahaja.

- **Master terima "ukur dulu" dan jawab ringkas ikut syor; sebut peranti secara spontan bila ia mengubah tafsiran** (2026-10-07/08, audit kelajuan Hantar).
  Lucy bentang audit + plan; master "Setuju ikut syor" (dua kali, termasuk "A: dua sampel lagi dulu"), kemudian menyampuk "Aku guna phone" apabila angka perlu dilabel PERANTI, dan minta "audit bahagian tu guna opus" (pandangan kedua oleh subagent) bila syor Lucy ("tak berbaloi") perlu disemak.
  ➡️ **Bila angka ukuran masuk, label peranti dalam catatan (telefon ≠ laptop). Bila master minta subagent opus mengaudit, itu semakan atas syor Lucy sendiri — bentang hasilnya jujur walaupun ia menyokong ATAU menolak syor asal.**
  🔑 "Ya deploy @NN" ditulis jelas setiap kali; "Save memory dan session" datang SEBELUM master menjawab soalan keputusan yang tertunggak (A/B/C) — catat keputusan itu sebagai TERTUNGGAK, jangan andai pilihan.

---

## Catatan Penting

- **Konsistensi kolum jadual (terutama cetak):** master pentingkan kolum jadual kekal konsisten. Label decorative (cth "Tidak Menguasai") yang muncul hanya pada sesetengah baris buat kolum jadi tak sejajar/tak konsisten — master prefer **BUANG label itu dari paparan cetak** daripada cuba dandan style. Paparan skrin/desktop boleh kekal ada label. (Origin: feature Trend Markah ETR, 2026-07-04)
