# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-09 (pagi→petang) — **mypwa-v2: Kumpulan Intervensi T5 + T8 SIAP (8/10 task).**
Branch `test` @ **`e03f5d7`** == `origin/test`, working tree bersih, **`main` @ `8542f96` TIDAK disentuh**.
Suite penuh **46/0/2**. ⏭️ Seterusnya **T9** (laporan) → T10. Butiran: blok TITIK SAMBUNG di bawah + `mypwa-v2/MEMORY.md`.
**Last Work Activity**: 2026-08-09 (~13:1x — master minta kemas memory + session, lalu compaction)

> Fail ini dilayan sebagai **RAM** (Option A). Ilmu kekal dialir KELUAR ke auto-memory + `MEMORY.md`
> projek; blok `## Compacted History` di bawah kekal **nipis** — pointer kesinambungan sahaja.
> Lihat `compaction/compaction-policy.md`.

---
## 🎯 TITIK SAMBUNG — mypwa-v2: **T5 + T8 SIAP** (dikemas 2026-08-09 **13:0x**)

**Repo `test` @ `61e05cb` == `origin/test`** · `main` @ `8542f96` tidak disentuh.
**Kumpulan Intervensi 8/10** — `T1`–`T8` ✅ · ⏳ `T9` `T10`. 🆕 **BACA `mypwa-v2/MEMORY.md` DULU.**

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
  pilih kerja berasingan) · pagar `/bulk` tak semak `murid_id` tergolong dalam kelas/kumpulan guru.
- **Cadangan audit kecil Lucy** (belum master putus): `created_at` timezone di tempat lain ·
  teks/border pucat pada page cetak lain (slip ujian, pajsk, RPM). Kekal dalam snapshot.
- **Lalai `Admin@1234` dalam `seed.sql`** — master pilih **biarkan**; relevan semula hanya bila
  pasang instance baharu. Pembetulan sebenar = paksa tukar pada log masuk pertama.
- Backlog projek lain (Lompat Tinggi Fasa 2, DELETE-sebelum-validate, normalisasi markah_penuh,
  ETR boleh-laras, eksport Excel) **tercatat dalam `CLAUDE.md`/`MEMORY.md` projek masing-masing.**

*Tiada kredensial, token, mahupun `JWT_SECRET` dibawa masuk ringkasan ini.*
