# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

## Session RAM Status
**Current Session**: Updated
**Last Activity**: 2026-07-14 (petang ~16:04–17:25)

### 🆕 Sesi 2026-07-14 (petang ~16:04–17:25): sistem-olahraga — 🔴 BUG KEHILANGAN DATA: HAKIM SALING MENIMPA MARKAH ⏳ SPEC + PLAN SIAP, BELUM EXECUTE

**Master report 2 bug; PUNCANYA SATU.** (1) Hakim pilih semula rumah → markah tak dipapar semula. (2) Laporan tunjuk PERBARISAN 96/97 sedangkan 5 hakim × 5 kriteria patut jauh lebih tinggi.

**🔴 PUNCA — `markah_penilaian` TIADA DIMENSI HAKIM:**
- PK = `(id_sekolah, id_kategori_penilaian, id_rumah)` (`schema.sql:183`) — **tiada `id_hakim`**. DB hanya benarkan SATU baris per rumah.
- `POST /api/hakim/markah` (`index.js:1512`) guna `ON CONFLICT ... DO UPDATE SET markah = excluded.markah` → **setiap hakim MENIMPA hakim sebelumnya**.
- `hakim.js:302` MEMANG hantar `id_hakim`, tapi backend (`index.js:1482`) cuma destructure 3 medan → **`id_hakim` dibuang terus, tak pernah disimpan**.
- **BUKTI PRODUCTION (read-only, izin master):** `PERBARISAN | BIRU 96 | HIJAU 97 | KUNING 98 | MERAH 96` — **`bil_baris=1` SETIAP rumah** walaupun 5 hakim (sks1–sks4, pibg1) hantar. 96–98 = skor SEORANG hakim (5×20=maks 100). **Markah 4 hakim HILANG KEKAL — tak boleh dipulihkan.**
- **Penemuan sampingan:** jadual sama simpan DUA jenis data — PERBARISAN (hakim, ada kriteria) DAN MERENTAS DESA 697–805 (sub_admin taip manual, tiada kriteria, tiada had). `hakim.js:130` tapis pertandingan tanpa kriteria. **Apa-apa fix TAK BOLEH rosakkan laluan manual ini.**

**KEPUTUSAN MASTER:**
| Perkara | Pilihan |
|---|---|
| Markah/kriteria | **10** (turun dari 20) → 1 hakim = 50, 5 hakim = 250 |
| Agregat | **JUMLAH (SUM)**, bukan purata — sentiasa integer, tiada perpuluhan |
| Hakim edit markah sendiri | **Ya** — papar semula, laras, hantar semula |
| Data lama (96–98) | **Biar** — tiada migration data |

Lucy mula syorkan PURATA (adil bila hakim tercicir), master pilih JUMLAH + turunkan skala ke 10 untuk elak perpuluhan. **Risiko diterima:** hakim tercicir = rumah rugi 50 markah. **Mitigasi (Task 6):** `GET /api/markah-penilaian` pulangkan `bil_hakim` supaya sub_admin NAMPAK "4/5 hakim" dan boleh betulkan sebelum umum keputusan.

**REKA BENTUK — corak RAW + DERIVED:** jadual baru `markah_hakim` (PK 5-lajur, **granulariti PER-KRITERIA**), `markah_penilaian` kekal sebagai nilai TERBITAN (jumlah). → `laporan.js` **sifar perubahan** (ia cuma dapat nombor betul), laluan manual sub_admin **sifar perubahan**, **tiada rebuild jadual** production (SQLite tak boleh ALTER PK).

**🔴 LUBANG YANG DITANGKAP SPEC SELF-REVIEW (Lucy hampir hantar spec yang tak selesaikan Bug 1!):** draf pertama simpan **jumlah** setiap hakim (45) sahaja. Frontend **TAK BOLEH** pulihkan 5 slider dari nombor 45. **Granulariti data mesti sepadan dengan apa yang UI perlu papar semula.**

**🔴 2 LUBANG LAGI DITANGKAP OLEH SOALAN MASTER (bukan oleh Lucy):**
1. *"Backend ada validate id_hakim tak?"* → Lucy trace → **`DELETE /api/pengguna/hakim/:id` akan tinggalkan `markah_hakim` ORPHAN.** Dan padam sahaja TAK CUKUP: `markah_penilaian` ialah nilai TERBITAN — mesti **KIRA SEMULA**, kalau tidak markah rumah kekal mengandungi sumbangan hakim yang dah tiada. **Peraturan am: setiap laluan yang padam data mentah MESTI kira semula nilai terbitannya.**
2. *"Hakim sekolah1 tak tertukar dengan sekolah2 kan?"* → **TIDAK.** `pengguna.id_pengguna` = **PK SATU LAJUR** → username unik GLOBAL → `sks1` mustahil wujud di 2 sekolah (DB tolak). **Kontras `kategori` (PK KOMPOSIT)** yang bagi bug 84-vs-42. **TAPI `markah_hakim` yang kita cipta ialah PK KOMPOSIT** → tertakluk peraturan kunci penuh → **Guard rule #4 ditambah**.

**SECURITY:**
- **`id_hakim` MESTI dari JWT** (`c.get('id_pengguna')`, `index.js:149`), **BUKAN dari body**. `hakim.js:292` sekarang ambil dari **localStorage** → sesiapa boleh ubah & menyamar jadi hakim lain. **Prinsip: jangan validate input yang kau tak sepatutnya terima langsung.**
- **`JWT_SECRET` disahkan SUDAH DISET** dalam production DAN test (`wrangler secret list`). Fallback `'sila-tukar-secret-ini'` (`index.js:2387`) tak pernah aktif. **Backlog:** ia patut CAMPAK ERROR, bukan senyap guna secret yang tertulis dalam repo.

**KENAPA USERNAME MESTI UNIK GLOBAL (master tanya):** `POST /api/login` hanya terima `{username, password}` — **tiada konteks sekolah**. Carian = `WHERE id_pengguna = ?` (`index.js:2363`); `id_sekolah` datang **DARI baris yang dijumpai**, kemudian masuk JWT. **Keunikan diperlukan SEBELUM token wujud.** JWT bukan penyelesaian — JWT ialah HASIL. Untuk benarkan username berulang → `pengguna` jadi PK komposit = kelas bug `id_kategori` semula. **Status: kekal unik global** (master tiada preferens, Lucy pilih risiko terendah); konvensyen disyor `sks1-nba3003`. Dicatat CLAUDE.md projek.

**PLAN 6 TASK** (`docs/superpowers/plans/2026-07-14-markah-hakim-per-hakim.md`):
T1 migration `markah_hakim` → T2 **guard rule #4 + MUTATION TEST** (buktikan boleh GAGAL dulu) → T3 backend POST/GET (identiti JWT) → T4 cascade 5 laluan + **kira semula** → T5 `hakim.js` (skala 10, papar semula markah, "Kemas Kini Markah") → T6 `bil_hakim`.
**Plan self-review tangkap 3 lubang:** (1) ujian bergantung akaun hakim yang mungkin TIADA — dan ujian padam-hakim akan **MEMUSNAHKAN akaun sebenar test DB** → kini cipta akaun pakai-buang `ujiA-<timestamp>` via API + `afterEach` padam; (2) tukar **pertandingan** (bukan rumah) sedangkan rumah terpilih → markah tak dimuat = Bug 1 masih hidup dalam laluan sempit; (3) ujian spec #4/#10/#14 tiada task.

**COMMIT (semua di `test`, DOKUMEN SAHAJA — 0 kod implementasi):** `b8d159f` `5088204` `7df2493` (spec) → `710e3ef` (plan) → `d89c86d` (backlog SaaS + JWT_SECRET). **main masih `b6aff36`.**

**⏭️ NEXT:** master pilih cara laksana — **Subagent-Driven (disyor)** atau Inline → execute 6 task → verify staging → **master confirm** → migration production `olahraga-db` → merge main `--no-ff`.
⚠️ **JANGAN deploy production tanpa confirm master** (walaupun tertulis dalam plan).

--- (rekod sesi lepas) ---
### Sesi 2026-07-14 (petang ~14:56–15:44): sistem-olahraga — VERIFY HARDENING + MERGE ✅ LIVE PRODUCTION (main `b6aff36`, doc `94d7149`)
**Master tanya: "Aku perlu test apa?"** → Lucy jawab dengan UJIAN, bukan senarai. **main == test == `94d7149`. TIADA kerja tertunggak.**

**🔑 RANGKA FIKIR SESI NI: jangan test FIX itu — test apa yang fix itu mungkin ROSAKKAN.**
Fix hardening sifatnya *menambah sekatan*. Risiko sebenar bukan "sekatan tak jalan", tapi **"sekatan terlalu ketat sampai halang kerja yang betul"**. Kegagalan jenis ni lumpuhkan sistem sepenuhnya TAPI suite ujian kekal HIJAU.

**2 LUBANG DITUTUP** (`tests/verify-hardening-staging.spec.js`, 5 ujian):
1. **Guard lama cuma buktikan `id_acara` palsu DITOLAK (404). TIADA siapa buktikan id SAH masih DITERIMA.** Kalau validasi tersilap → **hakim tak boleh simpan keputusan langsung**, dan 40/40 ujian tetap lulus. → Ujian baru: cipta acara → POST keputusan → **200** → baca balik dari DB, 3 baris betul-betul ada.
2. **URUTAN PAPAN SKOR** — regresi yang hampir lolos pagi tadi, tiada ujian menyentuhnya. → Simpan keputusan baru → sahkan ia naik ke **PUNCAK** `GET /api/dashboard/pemenang` + `GET /api/public/:id/dashboard`. Buktikan id format LAMA (`kep-<ms>-<4char>`) & BARU (`kep-<ms>-<uuid>`) bercampur ikut KRONOLOGI. **Disahkan VISUAL (screenshot), bukan angka semata.**

**🔧 CORAK BARU SANGAT BERGUNA — uji TULISAN DB pada staging tanpa mencemarkannya:**
cipta acara ujian → simpan keputusan → semak → `DELETE /api/acara/:id`. Endpoint itu **tolak balik markah rumah** (`src/index.js` ~690, bukan sekadar cascade padam). **Bukti reconcile:** semasa ujian HIJAU=357/MERAH=341 → selepas cleanup **HIJAU=339/MERAH=335/KUNING=337** = betul-betul asal. Kira balik kena: HIJAU Johan+Naib (10+8=18), MERAH Ketiga (6), KUNING 0. Ujian #5 assert `markahSelepas == markahSebelum`.

**SEAL ✅:** Playwright **45/45** chromium lawan staging, wrangler dry-run CLEAN (179.39 KiB), `npm run guard` HIJAU (3 peraturan), 0 secret (7 fail diff).
**MERGE:** `git merge test --no-ff` → main `b6aff36` → push → GitHub Actions deploy.
**VERIFY PRODUCTION ✅ (bukan HTTP 200 kosong):** probe `POST /api/keputusan` dengan `id_acara` palsu → **404 "Acara tidak dijumpai"** = penanda KOD BARU (kod lama pulangkan 500). Probe SELAMAT — validasi berhenti sebelum sebarang DELETE/INSERT. Login production `admin_dba1097`/`1234`.
Doc commit `94d7149` (CLAUDE.md tanda LIVE) → `test` di-fast-forward ke main, dua branch selaras.

**⚠️ NOTA BUKAN-REGRESI (jangan panik sesi depan):** dalam acara YANG SAMA, susunan Johan/Naib/Ketiga adalah **RAWAK** (ketiga-tiga disimpan dalam ms yang sama → suffix UUID tentukan urutan). **Format id LAMA pun sama** (`kep-<ms>-<4 aksara rawak>`). Kalau master mahu Johan sentiasa di atas: tambah `kedudukan ASC` pada `ORDER BY` (2 tempat: `:2094`, `:2287`).

**⏭️ NEXT (backlog teratas): RATE LIMITING `/api/login`** — isu security tertinggi. Sedia ada cuma delay 1s, **tiada had cubaan** → brute-force terbuka. Mula dengan `brainstorming`. Keputusan perlu: per-IP vs per-username; Cloudflare WAF Rate Limiting (tiada kod, level edge) vs Durable Object/KV counter (lebih halus, boleh kunci akaun). Nota: multi-tenant, SATU endpoint `/api/login` untuk SEMUA sekolah — per-IP sahaja mungkin tak cukup. Boleh sekalikan: `POST /api/login` pulangkan 500 bila medan body hilang (patut 400).

--- (rekod sesi pagi — kini SUDAH LIVE) ---
### Sesi 2026-07-14 (pagi ~07:36–11:30): sistem-olahraga — HUNT AUDIT MULTI-TENANT + 4 FIX HARDENING [✅ SUDAH MERGE & LIVE — lihat entri petang di atas]
**Master tanya: "bug kategori bertindan NBA3003/WBA2005 tu berlaku lagi tak kat mana-mana?"** → hunt audit → 4 fix → push test. **main masih `7531b04`, BELUM merge.**

**JAWAPAN AUDIT: TIDAK. Bug itu tidak berulang di mana-mana.** Dibuktikan secara STRUKTUR, bukan sekadar baca JOIN:
- Bug hanya mungkin bila **id sama boleh sah wujud di 2 sekolah** → hanya pada jadual PK KOMPOSIT. Cuma ADA DUA: `kategori` (dah fix) dan `markah_penilaian` (selamat — komponennya `crypto.randomUUID()`, unik global).
- Semua jadual lain PK SATU LAJUR → id berganda merentas sekolah **MUSTAHIL** (DB sendiri tolak). Itu sebab `JOIN acara ON ka.id_acara = a.id_acara` tanpa id_sekolah tetap selamat.
- **Pengajaran: audit corak macam ni MESTI bermula dari SCHEMA, bukan dari senarai JOIN.** Grep JOIN sahaja takkan beritahu mana yang betul-betul berisiko.

**BUKTI DB (production + test, read-only):** 0 orphan, **0 rujukan silang-sekolah** pada SEMUA laluan (keputusan↔acara, pendaftaran↔acara, keputusan↔murid, kriteria↔pertandingan, markah↔rumah). Dan bukti fix lama berfungsi: JOIN separuh kunci = **419 baris**, kunci penuh = **239**, kiraan sebenar = **239** ✅.
**⚠️ `D_id_kategori_dikongsi = 4` dalam KEDUA-DUA DB** — id berlanggar (`kat-l10/l12/p10/p12`) MASIH dalam data (sengaja tiada migration). Satu-satunya benda yang menahan bug hidup semula = skop `id_sekolah` dalam JOIN. **Guard yang pegang picunya.**

**4 FIX (subagent-driven, 7 commit `7531b04..caa4963`):**
1. **Guard jadi gerbang CI SEBENAR** (`scripts/guard-tenant.mjs`, `npm run guard`) — guard lama hidup dalam `tests/` yang **GITIGNORED**, tak pernah masuk git, `deploy.yml` tiada langkah test langsung. Ia melindungi APA-APA PUN TIDAK. Kini skrip Node biasa (tiada browser), di-commit, **gagalkan deploy** dalam KEDUA-DUA job.
2. **Orphan `markah_penilaian`** — satu-satunya jadual **tanpa FOREIGN KEY langsung**. `DELETE /api/rumah/:id` dulu cuma padam `rumah_sukan`. Kini cascade manual `db.batch`. (0 orphan setakat ini — ditutup mumpung kosong.)
3. **`POST /api/keputusan` sahkan `id_acara`** — dulu `acaraInfo` null → **teruskan sahaja**. Kini 404, SEBELUM sebarang DELETE/INSERT (kedua-dua laluan heat+final).
4. **`id_keputusan` hibrid** `` `kep-${Date.now()}-${crypto.randomUUID()}` ``.

**🔴 REGRESI HAMPIR LOLOS (ditangkap REVIEW, bukan ujian — TIADA ujian menyentuhnya):**
Plan asal kata guna `crypto.randomUUID()` TULEN. Itu akan **memecahkan `ORDER BY ka.id_keputusan DESC LIMIT 20`** dalam `GET /api/dashboard/pemenang` + `GET /api/public/:id/dashboard` → "Pemenang Terkini" (dashboard.js) dan **PAPAN SKOR AWAM** (papan.js) papar 20 pemenang **RAWAK**. Id lama `kep-<Date.now()>-<rand>` = awalan ms **13-digit lebar tetap** (sampai 2286) → susunan leksikografi == kronologi. **Kebetulan bermanfaat yang telah jadi KEBERGANTUNGAN SENYAP.** Guard hijau, 40/40 Playwright lulus, grid Statistik betul — semua "hijau" sambil papan skor awam akan hancur.
→ Master pilih HIBRID: kekal susunan + jamin unik. BONUS: id lama & baru kongsi format awalan → sort bercampur betul.

**🔴 GUARD BUTA (ditangkap RE-REVIEW):** rule #3 asal cuma semak `randomUUID` ADA. Reviewer **buktikan secara EMPIRIK** — patah balik ke UUID tulen (regresi sama persis) → **guard LULUS BERSIH**. Fix: rule #3 kini tuntut `Date.now()` DAN `crypto.randomUUID()`. Ketiga-tiga keadaan dibuktikan (hijau / tangkap UUID hilang / tangkap awalan hilang).

**🔑 PENGAJARAN BESAR SESI INI:**
1. **Mutation testing ialah SATU-SATUNYA cara sahkan guard.** Tiga kali sesi ni: setiap guard yang kita bina ada titik buta, dan hanya *sengaja rosakkan kod → tengok guard bangun tak* yang mendedahkannya. Ujian yang cuma sahkan "keadaan baik masih baik" TAK PERNAH jumpa apa-apa. Guard v1 buta pada susunan; guard v2 buta pada refactor.
2. **"Semua hijau" ≠ betul.** Regresi papan skor lolos SEMUA ujian sebab tiada ujian menyentuh susunan. Yang tangkap ialah soalan: *"apa LAGI yang bergantung pada bentuk benda yang aku ubah ni?"*
3. **Guard dalam `tests/` yang gitignored = guard PALSU.** Ia tak pernah jalan dalam CI, hilang kalau folder dibersih. Guard sebenar mesti di-commit + jadi gerbang deploy.
4. **curl dalam Git Bash TAK BOLEH capai network** walaupun sandbox-disabled (000 untuk production juga). **Guna `node -e "fetch(...)"`** — itu corak yang berfungsi.
5. **wrangler 4.79 default port 8787**, bukan 8788 → `wrangler dev --env test --remote --port 8788` (spec repo jangka 8788).
6. `gh` CLI TIDAK dipasang. Verify deploy dengan jalankan spec terhadap staging URL (404 vs 500 = penanda kod baru/lama).

**SEAL ✅:** Playwright **40/40** (termasuk e2e yang biasanya flaky, isolasi-kategori 4/4, hardening 2/2), wrangler dry-run CLEAN (179.39 KiB), guard HIJAU, 0 secret. Push `7531b04..caa4963 test->test`.
**VERIFY STAGING ✅:** `hardening.spec.js` **2/2 LULUS terhadap staging yang di-deploy** — 404 (bukan 500) buktikan kod BARU live, dan deploy berjaya buktikan **langkah guard dalam CI memang jalan & hijau**.

**⏭️ NEXT:** master verify staging → **merge `main --no-ff`** → production. Backlog dari final review (master pilih tangguh) dicatat dalam CLAUDE.md projek — yang PALING penting: **guard mesti scan `src/**/*.js` SEBELUM sebarang "Code DRY extraction"**, kalau tidak route yang dipindah terus tak berpengawal.
**Plan:** `docs/superpowers/plans/2026-07-14-hardening-multi-tenant.md`. **Ledger:** `.superpowers/sdd/progress.md`.

--- (rekod sesi lepas) ---
### Sesi 2026-07-13 (pagi–tengah hari ~07:31–13:10): sistem-olahraga — Senarai Pingat + BUG MULTI-TENANT ✅ SEMUA LIVE PRODUCTION (main `7531b04`)
**main == test == `7531b04`. TIADA kerja tertunggak.** Dua kerja: satu yang master minta, satu yang Lucy TERJUMPA semasa verify.

**Kerja 1 — Senarai pingat: pemegang pingat sahaja (main `138e9f5`):**
- **Master minta:** murid yang tiada emas/perak/gangsa tak perlu masuk senarai pingat.
- **Master pilih:** tempat ke-4 TIDAK layak; kolum "Ke-4" KEKAL dipapar (untuk pemegang pingat, ke-4 dia masih dikira dalam Jumlah). Laporan penuh ikut sama.
- **Fix:** `GET /api/pingat` + arkib → `HAVING SUM(CASE WHEN kedudukan IN (1,2,3) ...) > 0`. Tapis di **BACKEND** → page Senarai Pingat DAN seksyen pingat "Download Semua Laporan" konsisten dengan SATU perubahan (DRY). Arkib dulu kira tempat ke-4 sebagai layak — kini takrif sama. Label "X peserta" → "X pemegang pingat".
- **Bukti test DB NBA3003:** 121 murid → 107, **14 dibuang**, kesemua 14 disahkan tiada kedudukan 1/2/3. Playwright 3/3 + screenshot visual disahkan.

**Kerja 2 — 🔴 BUG MULTI-TENANT (main `4aec59e`) — Lucy TERJUMPA, bukan master report:**
- **Cara terjumpa:** query diagnostik Lucy bagi 27 murid dibuang, senarai nama bagi 14. **Lucy TAK abaikan percanggahan** → siasat → jumpa punca.
- **PUNCA:** `id_kategori` dijana tanpa id sekolah (`admin.js:454`: `'kat-' + nama` → `kat-l10`). Schema guna **PK KOMPOSIT `(id_kategori, id_sekolah)`** — jadi data SAH, tapi **13 JOIN hanya padankan `id_kategori`** = guna SEPARUH kunci → padanan silang sekolah → **baris berganda**.
- **PRODUCTION MEMANG TERJEJAS:** NBA3003 & WBA2005 kongsi `kat-l10/l12/p10/p12`. Laporan keputusan NBA3003 kategori L10 papar **84 baris, sebenar 42**. Master sahkan sendiri di staging: "betul..memang berbeza".
- **DBA1097 terselamat KEBETULAN sahaja** — kategorinya bernama "L1"/"L2" → `kat-l1`, bukan `kat-l10`. Kalau master pernah namakan kategori "L10", laporan SK Salor pun dah berganda.
- **FIX:** 13 JOIN → `AND k.id_sekolah = a.id_sekolah`. **Guna perbandingan LAJUR, BUKAN bind param (`= ?`)** — bind param perlu tambah argumen `.bind()`, dan silap urutan bind GAGAL SENYAP. 3 query arkib guna `ka.id_sekolah` (acara di-LEFT JOIN, boleh NULL). + `admin.js` jana id berskop (`'kat-' + TENANT_ID + '-' + nama`).
- **⚠️ PERANGKAP yang hampir lolos:** tukar penjanaan id SAHAJA → sekolah yang sudah ada `kat-l10` akan dapat id baru → upsert `ON CONFLICT(id_kategori,...)` TAK PADAN → **cipta kategori PENDUA**. Fix: `POST /api/kategori` cari kategori sedia ada ikut **NAMA** dulu (nama = kunci logikal, id = butiran dalaman). Corak SAMA dengan fungsi arkib (`:2160`) — konvensyen sedia ada, bukan reka baru.
- **TIADA migration** — kategori lama (`kat-l10`) & baru (`kat-DBA1097-l10`) wujud serentak dengan selamat (PK komposit + semua JOIN berskop). Migration data DITOLAK: risiko tinggi, sifar faedah tambahan.
- **GUARD:** `tests/guard-join-kategori.spec.js` — baca `src/index.js`, GAGAL kalau jumpa `JOIN kategori` tanpa `id_sekolah`. **Dibuktikan boleh gagal** (suntik JOIN runggak → merah + tunjuk baris tepat). Guard yang tak pernah diuji gagal = guard palsu.
- **Verify:** Playwright isolasi 4/4 (lokal kod baru + staging), guard 1/1, pingat 3/3, e2e 18/18. Production DBA1097 disahkan **0 pendua, 56 acara** (tiada regresi).
- **Spec:** `docs/superpowers/specs/2026-07-13-isolasi-kategori-sekolah-design.md`.

**⏭️ NEXT (master arah sambung sesi depan): RATE LIMITING `/api/login`**
- Isu security tertinggi dalam backlog. Sedia ada: cuma `await new Promise(r => setTimeout(r, 1000))` (delay 1s setiap request) — **tiada had cubaan**, brute-force masih terbuka.
- Mula dengan `brainstorming` (belum dibincang langsung). Keputusan yang perlu: per-IP vs per-username (atau kedua-dua); Cloudflare **WAF Rate Limiting Rules** (tiada kod, level edge) vs **Durable Object / KV counter** dalam Worker (lebih halus, boleh kunci akaun).
- Nota: sistem multi-tenant, satu endpoint `/api/login` untuk SEMUA sekolah — per-IP sahaja mungkin tak cukup.
- Backlog berkaitan yang boleh disekalikan: `POST /api/login` pulangkan **500** bila medan body hilang (patut 400).

**🔑 PENGAJARAN SESI INI:**
1. **Percanggahan angka = JANGAN abaikan.** 27 vs 14 nampak macam bug kecil dalam query Lucy — sebenarnya ia menyingkap bug multi-tenant LIVE dalam production. Kalau Lucy tolak ansur, bug tu kekal sampai sekolah ketiga daftar.
2. **Uji kod BARU, bukan kod LAMA.** Perubahan backend belum di-deploy → ujian terhadap staging cuma uji kod lama (lulus tanpa makna). Guna **`wrangler dev --env test --remote`** (worker LOKAL kod baru + test DB SEBENAR) + Playwright `BASE_URL=http://localhost:8788`. Ini corak baru yang SANGAT berguna.
3. **Guard mesti dibuktikan boleh GAGAL.** Cubaan pertama Lucy suntik JOIN runggak guna `\n` — fail guna CRLF → suntikan tak menjadi → guard "lulus" secara palsu. Kalau Lucy tak semak, guard tu tak berguna langsung.
4. **`wrangler dev --remote` tak larat Playwright selari** → guna `--workers=1`. Kegagalan sporadic = lag, bukan regresi (sahkan dengan jalankan berasingan).
5. **Suite `e2e.spec.js` memang flaky** bawah beban (guna `waitForTimeout` tetap) — kegagalan berpindah-pindah antara larian (05, kemudian 08a). Sahkan berasingan sebelum panik.
6. **Kredensial:** production `admin_dba1097` password = **`1234`** (BUKAN `123456` macam staging). `POST /api/login` jangka `{ username, password }` — BUKAN `id_pengguna`/`kata_laluan` (itu id elemen HTML). Hantar medan salah → **500** (patut 400 — dicatat backlog).

--- (rekod sesi lepas) ---
### Sesi 2026-07-13 (malam ~00:08–01:50): sistem-olahraga — 2 Fix Cetak Laporan ✅ SEMUA LIVE PRODUCTION (main `b18056e`)
**Master test cetak staging → jumpa bug header. Dua fix, dua merge, semua live. main == test == `b18056e`. TIADA kerja tertunggak.**

**Kerja 1 — Header cetak jadi FOOTER (main `80eb96e` → doc `22162ac`):**
- **Bug master:** cetakan biasa (Cari → Cetak), header berulang mendarat di BAWAH setiap muka surat, jadi footer — dan BERTINDIH baris terakhir jadual (lebih teruk dari yang master perasan).
- **PUNCA SEBENAR (systematic-debugging + bukti PDF):** `#running-header` guna `position: fixed; top: -17mm` (Lucy sendiri tulis semalam). **Enjin cetak Chrome MENGULANG elemen fixed tiap muka surat (betul) TAPI MENGABAIKAN `top` dan meletakkannya di hujung BAWAH kawasan kandungan.** CSS-nya sah — diagnostik tunjuk position aktif, top dikira betul (-64px), tiada ancestor transform/filter/contain yang rampas containing block. Enjin cetak yang tak melayan.
- **FIX:** kandungan laporan dibungkus shell `<table id="print-shell">`, header duduk dalam `<thead>`. **Chrome mengulang baris `<thead>` tiap muka surat secara automatik — ini SATU-SATUNYA mekanisme header-berulang yang boleh dipercayai dalam Chrome.** Hipotesis diuji dulu (bungkus DOM staging semula via page.evaluate, jana PDF, tengok) sebelum tulis fix sebenar.
- **⚠️ BUG LUCY YANG HAMPIR LOLOS:** selektor shell mula-mula guna DESCENDANT (`#print-shell td`) → kena SEMUA sel jadual acara yang BERSARANG dalam shell → paparan SKRIN hancur total (semua sel jadi baris block bertindan). **SEMUA assertion masih LULUS** (lebar container OK, bilangan .acara-block OK) — hanya SCREENSHOT yang dedahkan. Fix: rantaian child (`#print-shell > tbody > tr > td`).
- Named page `laporan-normal` (margin atas 24mm) DIBUANG — header kini dalam aliran kandungan, margin biasa 12mm memadai → satu lagi acara muat page 1 (4, dulu 3). 32 acara → 10 muka surat, kekal.
- Merge ini turut bawa **guard backend payload kosong** yang tertunggak (commit `beec1b8` semalam) → backlog "BACKEND punca sebenar bug C2" kini DITUTUP.

**Kerja 2 — Tandatangan Download Semua dapat muka surat kosong sendiri (main `87b507c` → doc `b18056e`):**
- **Penemuan (contact sheet audit):** laluan Download Semua = 20 muka surat, page 20 = tandatangan "Disahkan oleh" SAHAJA (satu helai terbuang).
- **Punca:** blok tandatangan di hujung dokumen, selepas `#seksyen-statistik-extra` yang guna named page `landscape-stat`. **Tukar orientasi @page MEMAKSA pemisah muka surat** → tandatangan terpelanting keluar. (Ini isu "I3" yang dicatat 2026-07-12 tapi tak pernah diverify visual — akhirnya disahkan.)
- **Master pilih:** tandatangan duduk BAWAH jadual "Laporan Markah Akhir Rumah Sukan" (page 1, ada banyak ruang kosong) — itulah keputusan rasmi yang perlu disahkan.
- **Fix:** blok yang SAMA dipakai cetakan biasa (di hujung laporan), jadi JS alihkan ke `#seksyen-markah-extra` dalam laluan Download Semua sahaja, pulangkan ke hujung `#laporan-main` semasa reset — elak salin markup jadi dua (DRY). 20 → 19 muka surat.
- **Master pilih KEKAL (bukan bug):** `.kategori-section { page-break-after: always }` → setiap kategori mula page baru, 60-70% ruang kosong setiap satu (page 6-17). Opsyen "kategori mengalir bebas" boleh jimat 5-6 helai lagi — terbuka kalau master ubah fikiran.

**🔑 PENGAJARAN BESAR (2 sesi berturut bug cetak lolos ke tangan master):**
1. **Kira angka ≠ verify.** Spec cetak asal cuma KIRA bilangan muka surat → bug header lolos. Assertion Lucy untuk shell pun semua LULUS sambil paparan skrin hancur. **Untuk kerja cetak/visual: WAJIB jana PDF sebenar, render jadi imej, TENGOK.**
2. **Cara render PDF jadi imej tanpa poppler (Windows):** Playwright `page.pdf({preferCSSPageSize: true})` → base64 → `page.addScriptTag` pdf.js CDN → render ke canvas → PNG → Lucy `Read` imej. Contact sheet (grid semua page) untuk imbas dokumen panjang sekali pandang.
3. **`page.pdf({format:'A4'})` ABAIKAN margin `@page` CSS** — mesti guna `preferCSSPageSize: true` supaya setia dengan Ctrl+P sebenar master.
4. **Corak ujian pra-push (SANGAT BERGUNA):** `page.route('**/laporan.html', r => r.fulfill({path:'public/laporan.html'}))` → sajikan fail LOKAL terhadap API staging sebenar. Uji perubahan frontend dengan data hidup SEBELUM push. Inilah yang tangkap bug selektor descendant sebelum sampai staging.
5. Playwright auto-dismiss `confirm()` — handler `btn-download-semua` bermula dengan confirm, kena `page.on('dialog', d => d.accept())`.
6. Stub `window.print = () => { throw }` → handler berhenti sebelum setTimeout reset → keadaan cetak kekal untuk jana PDF.

**Spec cetak (tests/, gitignored):** `cetak-laporan.spec.js` (uji staging + GUARD punca sebenar: header mesti dalam `<thead>`, mesti BUKAN `fixed`), `cetak-verify-lokal.spec.js` (PDF pra-push + render PNG), `cetak-regresi-lokal.spec.js` (guard skrin + laluan DL Semua), `dl-semua-audit.spec.js` (contact sheet).
**SEAL ✅ ×2:** syntax OK, wrangler dry-run CLEAN (178.63 KiB), Playwright 30/30, secrets clean. Production `atletik.celikguru.my` disahkan live (node fetch) selepas kedua-dua merge.

--- (rekod sesi lepas) ---
### Sesi 2026-07-12 (petang ~20:00–20:37): sistem-olahraga — Fix Backend + Cetak Laporan [✅ SEMUA DAH LIVE — lihat sesi 2026-07-13 di atas]
**Sambung terus lepas 3 fix pasca-kejohanan live production.** Master arah: "fix backlog backend tu sekarang" + ubah cetak laporan. **HEAD test = `bbcb0a3` (pushed, staging live). main masih `b802c05` — BELUM merge.**
- **Kerja 1 — Punca sebenar bug C2 DITUTUP** (`src/index.js`, `POST /api/keputusan` ~1751): guard `!Array.isArray(keputusan) || keputusan.length === 0 → 400` diletak di PALING ATAS handler, sebelum sebarang DELETE. Lindungi kedua-dua laluan (heat + final). Guard frontend (3 tapak POST) DIKEKALKAN = 2 lapis.
  - **BUKTI HIDUP:** POST payload kosong ke `acr-NBA3003-kat-p12-4x100m` (12 keputusan tersimpan) → 400 "Tiada keputusan dihantar" → **kira semula: 12 baris KEKAL UTUH**. Sebelum fix, 12 baris tu terpadam + markah rumah tertolak.
  - Nota: cek lama `if (statements.length > 0)` (~1917) kini dead code tapi inert — biarkan.
- **Kerja 2 — Cetak laporan muat maksimum** (`public/laporan.{js,html}`): master minta "muat seberapa banyak jadual dalam satu page tapi jangan pecahkan jadual, tak muat letak di page baru".
  - `renderLaporan` tak lagi paksa 2 acara/page (`.page-wrapper` + `page-break-after:always` DIBUANG). Jadual mengalir bebas; `.acara-block { page-break-inside: avoid }` (sedia ada) pastikan tak pecah.
  - **Header berulang:** `#running-header` `position:fixed` dalam margin atas `@page laporan-normal { margin: 24mm 10mm 12mm }`, `top:-17mm` → Chrome ulang tiap muka surat. Ganti `.page-repeat-header` suntikan per-wrapper (tak boleh lagi sebab JS tak tahu di mana page pecah). `#print-header` (header penuh page-1) disorok dlm cetakan biasa — maklumat sama dipadatkan ke running header (kejohanan / sekolah · tajuk · tarikh).
  - `rh-title`/`rh-sub` **nowrap + ellipsis** (dari review): nama kejohanan panjang kalau wrap → header melimpah margin → bertindih jadual pertama.
  - **BUKTI PDF SEBENAR (Playwright `page.pdf()` dari staging):** 32 acara → **10 muka surat** (3.2 acara/page). Dulu paksa 2/page = 16 muka surat. **Jimat 37% kertas.** Tinggi header 52px < bajet 64px.
- **⚠️ BUG LUCY YANG DITANGKAP UJIAN (pengajaran penting):** Lucy mula-mula skop CSS cetak baru pada `body:not(.laporan-lengkap)`, ingat `laporan-lengkap` = penanda "Download Semua". **SALAH** — kelas itu juga masuk pada Cari BIASA yang kebetulan lengkap (ia penanda gating tandatangan). Akibat: cetakan biasa yang lengkap terlepas layout baru sepenuhnya. Fix commit `bbcb0a3`: guna penanda BERASINGAN `cetak-semua` (ditambah HANYA dalam handler btn-download-semua). **Kalau tak jana PDF sebenar, bug ni sampai ke tangan master.**
- **SEAL ✅:** syntax OK, wrangler dry-run CLEAN (178.6 KiB), Playwright **26/26** chromium (termasuk spec cetak baru). Push `394212f..bbcb0a3 test->test`. Staging verified fresh.
- **⏳ NEXT (master test dulu):** master buka staging → Laporan → Cari → **Cetak** → semak print preview: (1) jadual muat maksimum satu page? (2) ada jadual terpotong tengah? (patut TIADA) (3) header page 2/3 elok, tak bertindih jadual? **Master confirm → merge main `--no-ff` → production.**
- **Spec ujian lokal (gitignored):** `tests/cetak-laporan.spec.js` (jana PDF + kira muka surat), `tests/dq-undo.spec.js` (undo DQ + gating + WYSIWYG). Guna sekolah **NBA3003 (nba3003/1234)** sebab DBA1097 dah diarkib penuh dalam test DB.

### 🆕 Sesi 2026-07-12 (tengah hari–petang ~17:03–19:50): sistem-olahraga — 3 Fix Pasca-Kejohanan ✅ LIVE PRODUCTION (main `b802c05`)
**Sambung dari checkpoint pagi.** Subagent-Driven. **main = `b802c05` (merge --no-ff), test = `394212f` (doc). Production `atletik.celikguru.my` disahkan live (node fetch, cf-cache MISS, semua penanda ADA). TIADA kerja tertunggak.** Ledger: `.superpowers/sdd/progress.md`.
- **⚠️ SATU-SATUNYA BENDA BELUM DIVERIFY VISUAL (I3):** kedudukan blok tandatangan dalam **"Download Semua Laporan"** — ia dipindah keluar dari `#seksyen-markah-extra` jadi sibling selepas `#extra-sections`. Seksyen statistik guna named page `landscape-stat` → tukar named page PAKSA page break → tandatangan mungkin mendarat di page sendiri. Master arah merge tanpa semak. Kalau nampak pelik → kerja CSS kecil.
- **PENGAJARAN BESAR:** review per-task semua "clean", tapi **final whole-branch review (opus) jumpa 2 CRITICAL** yang cuma nampak bila silang task + backend. Jangan skip final review walaupun setiap task dah lulus.
- **Task 3 fix** `9e6db08` (WYSIWYG sentinel `__none__` + label "Tiada acara dipilih") → review clean → T3 complete.
- **Task 4** `1b1eea0` (Semua Kategori + gating tandatangan via kelas `laporan-lengkap`) → review clean. Deviasi diluluskan: `semuaAcara` guna `!has('__none__') && size===length` (elak false-positive bila hanya 1 acara).
- **⚠️ FINAL WHOLE-BRANCH REVIEW (opus) = READY:NO — 2 CRITICAL jumpa.** Lucy verify sendiri dalam kod, kedua-dua BETUL:
  - **C1:** `setRowDQ(tr,false)` (undo DQ) blank span → save handler langkau baris `'-'` → backend **DELETE heat=0 SEBELUM insert** (index.js:1873) → **keputusan atlet TERPADAM + markah rumah ditolak**. Mod manual tiada recovery (dropdown beku).
  - **C2:** payload kosong → backend tetap jalankan DELETE batch (:1876) sebelum semak `statements.length>0` (:1910) → **seluruh acara terpadam**, hakim nampak "Tiada keputusan dihantar" sahaja.
  - **I1:** `kemasStatusLengkap` baca dropdown LIVE → tukar kategori tanpa klik Cari + toggle checkbox → tandatangan keluar atas laporan SEPARA.
  - **I2:** unlock helper (3 tapak) buka semula input baris DQ.
- **Fix `a208db8`** (satu fix subagent, frontend sahaja). **Master pilih:** (1) undo DQ = **PULIHKAN kedudukan automatik** (manual: repaint dari `<select>` + syncManualRanks/Lock; masa/padang: trigger semula `btn-kira*`); (2) guard C2 = **frontend sahaja** (3 tapak POST), bug backend → backlog.
- **Re-review (opus) = READY:YES** — C1/C2/I1/I2 semua PASS, quality Approved, 0 Critical/Important baru.
- **SEAL ✅:** syntax OK, wrangler dry-run CLEAN (178KiB), secrets clean, **Playwright 66/66 PASS** (chromium+firefox+webkit, izin sandbox master). Push `8c75fc4..a208db8 test->test`. Staging verified fresh (node fetch, `cf-cache: MISS`, semua penanda baru ADA).
- **VERIFY STAGING E2E ✅ (Playwright, data hidup):** 3/3 PASS — (1) **C1: kedudukan "4" → klik DQ → klik undo → kembali "4"** (bukan '-'), bukti klik sebenar; (2) I1: tukar kategori tanpa klik Cari + toggle checkbox → `laporan-lengkap=false` (tandatangan TAK keluar atas laporan separa); (3) WYSIWYG: untick semua acara → 16 blok jadi 0, label "Tiada acara dipilih". **TIDAK klik Simpan** → 0 tulisan DB. Spec lokal `tests/dq-undo.spec.js` (gitignored).
- **🔑 PENEMUAN TEST DB:** sekolah master **DBA1097 dalam `olahraga-test` DAH DIARKIB PENUH** (12 kategori `is_arkib=1`, **0 acara hidup**) → dropdown kategori KOSONG di staging, **bukan bug** (endpoint `/api/kategori` tapis `is_arkib`). Guna sekolah demo yang ada data hidup: **NBA3003 (nba3003/1234, 32 acara)** atau WBA2005 (53 acara). Master bagi kredensial NBA3003.
- **Nota UI:** acara yang dah disimpan = TERKUNCI (butang DQ disabled) sampai klik **Edit** (`.btn-edit-balapan`/`.btn-edit-padang`) — reka bentuk sedia ada, elak tersilap ubah keputusan muktamad.
- **MERGE:** master arah "merge" → `git merge test --no-ff` → main `b802c05` → push → GitHub Actions deploy → production verified live. CLAUDE.md projek dikemas (main commit + entri Selesai + backlog).
- **BACKLOG dicatat (dalam CLAUDE.md projek):** backend DELETE-sebelum-validate (punca sebenar C2, `src/index.js` ~1876 + simpanHeat ~1756) — **guard frontend cuma tutup laluan, punca masih terbuka**; padang semua 0.00m → semua dapat kedudukan 1 (pre-existing); asimetri DQ-on tak re-rank tapi undo-DQ re-rank; dua mekanisme DQ (butang vs taip "DQ").
- **Cara execute:** Subagent-Driven — implementer haiku/sonnet, reviewer sonnet, final review + re-review opus. Scripts `task-brief`/`review-package`.

--- (rekod sesi lepas: SPEC + PLAN SIAP) ---
### 🆕 Sesi 2026-07-09 (tengah hari ~11:28–13:37): sistem-olahraga — 3 Fix Pasca-Kejohanan ⏳ SPEC + PLAN SIAP, BELUM EXECUTE
**Konteks:** Kejohanan sebenar SK SALOR dah selesai (semak production `olahraga-db`: semua 56 acara lengkap keputusan, keputusan==daftar). Master kenal pasti 3 penambahbaikan. Brainstorm penuh → spec → writing-plans (belum execute).
- **3 keputusan reka bentuk (semua master sahkan):**
  1. **DQ butang setiap baris** — balapan (masa+manual+heat) & padang (Lompat Jauh/Lontar Peluru). BUKAN Lompat Tinggi (kekal NH). Relay 4x100m = DQ seluruh pasukan rumah (satu baris). Padang: 0.0m cubaan gagal KEKAL (prestasi), DQ = override berasingan (batal peserta). Tiada kotak sebab. **Penemuan penting:** DQ separa dah wujud — balapan mod masa boleh taip "DQ" (parseMasa), simpanHeat + save final dah kendali badge 'DQ' via pushKeputusan. Backend penuh sokong (kedudukan=0→markah 0). Gap: butang jelas + skip ranking + laluan padang (kira/save/populate) + mod manual.
  2. **Dropdown checkbox pilih acara** (laporan) — WYSIWYG (tapis skrin + cetak), default semua ter-tick.
  3. **Tandatangan "Disahkan oleh"** — muncul HANYA laporan lengkap (Semua Kategori + Semua jenis + semua acara ter-tick, ATAU butang Download Semua Laporan). **Punca bug:** CSS cetak paksa `#extra-sections` + `#print-tandatangan-semua` `display:block !important` tanpa syarat → keluar setiap cetakan. Fix: buang paksaan, gate guna kelas `laporan-lengkap`. + tambah "Semua Kategori" ke dropdown kategori.
- **Skop:** frontend SAHAJA, 0 migration, 0 package, 0 backend. Fail: `public/keputusan.{html,js}`, `public/laporan.{html,js}`.
- **SPEC:** `docs/superpowers/specs/2026-07-09-fix-dq-laporan-design.md` (commit `3a58a71`, test). **PLAN:** `docs/superpowers/plans/2026-07-09-fix-dq-laporan.md` (4 task, commit `8e8e7c0`, test). Self-review kedua-dua ✅. Belum push (Geass — commit-seal dulu).
- **Butang DQ diletak DALAM sel Kedudukan sedia ada** (bukan kolum baru) — jaga konsistensi jadual. Guna satu penanda `tr.dataset.dq` + helper `setRowDQ(tr,on)` + event delegation global.
- **4 task plan:** T1 butang DQ+toggle (keputusan.js) → T2 skip ranking + save/populate padang (keputusan.js) → T3 checkbox acara WYSIWYG (laporan) → T4 Semua Kategori + gating tandatangan (laporan). Titik sentuh + kod lengkap dalam plan.
- **⚠️ NEXT (sambung):** master pilih cara execute (Subagent-Driven disyor / Inline) → laksana 4 task → sight-hone → commit-seal (Playwright izin sandbox) → push test → verify staging (butang DQ + cetak laporan visual) → tunggu confirm master → merge main `--no-ff` → production `atletik.celikguru.my`. Risiko diketahui: `repopulateHeatKeputusan` tak dibaca penuh (T2 Step 6 nota mirror setRowDQ jika perlu).
- HEAD test = `8e8e7c0` (spec+plan sahaja, belum ada kod implementasi).

### 🆕 Sesi 2026-07-07 (petang ~19:03–19:26): sistem-olahraga — Markah Berkumpulan ✅ DEPLOY MAIN — LIVE PRODUCTION (master sahkan visual OK)
**Sambung dari pagi.** Master sahkan staging OK → arah deploy production.
- **Migration production `olahraga-db`** (izin master + sandbox-disabled): `ALTER TABLE tetapan_markah ADD COLUMN jenis TEXT NOT NULL DEFAULT 'individu'`. Pre-check schema (belum ada jenis) → jalankan → verify: kolum masuk, **24 rekod → 'individu', 0 hilang** (padan test DB). Nota: 1 glitch 7403 transient cubaan pertama, retry berjaya (test DB read OK bukti token elok). DB `olahraga-db` id `5dd90565...`, ada dalam akaun sama edc032a2 (bukan isu akaun).
- **Seal:** node --check src/index.js OK, wrangler dry-run CLEAN (178KiB).
- **Merge `test`→`main` `--no-ff` = `ec9a098`** (12 commit: Spec+Plan+7 feature+doc limitasi+fix arkib+doc fixed). main dulu `5160d5b`. Push `5160d5b..ec9a098 main->main`.
- **Deploy GitHub Actions SIAP** (poll node fetch, cubaan pertama 200): statistik-mata.js live (kiraMataStatistik), admin.js (berkumpulan), statistik.js + arkib-cetak.js origin ada kiraMataStatistik (**cache-buster perlu — edge cache serve versi lama pada URL kanonik; master hard-refresh Ctrl+F5 untuk nampak**).
- **✅ MASTER SAHKAN PRODUCTION OK (~19:26):** `atletik.celikguru.my` visual betul (toggle simpan berasingan, KUTIPAN==JUMLAH, NILAI 2 baris, arkib cetak 2 baris). **FEATURE SELESAI, TIADA KERJA TERTUNGGAK.** Lucy balik branch test.
- **Pengajaran:** (1) urutan deploy DB-schema-additive = migration DULU (masa kod lama live, kolum baru diabaikan = zero-downtime) → baru merge+push kod baru; (2) edge cache Cloudflare serve fail statik lama pada URL kanonik selepas deploy — verify origin guna `?cb=` cache-buster, `cf-cache: MISS` = origin fresh.

--- (rekod pagi asal di bawah, status kini SELESAI) ---
### 🆕 Sesi 2026-07-07 (pagi ~10:24–): sistem-olahraga — Markah Berkumpulan ✅ PUSHED TEST + STAGING LIVE [✅ DEPLOY PRODUCTION SELESAI — lihat entri petang di atas]
**Sambung dari semalam.** Master pilih: (1) final review opus DULU → (2) push feature, fix arkib berasingan.
- **Final review opus (whole-branch 22366a5..0de57fb):** 0 Critical, 1 Important, 3 Minor. Important = arkib relay KUTIPAN≠JUMLAH. **Lucy verify sendiri (receiving-code-review):** BETUL & PRE-EXISTING — arkib JUMLAH `SUM(markah_diperoleh)` tanpa dedup (`index.js:3153,:3166`) tapi relay simpan markah penuh setiap pelari (`:1893-1897`) → melambung ×bil pelari. `rumah_sukan.jumlah_markah` (live) deduped jadi live OK. Branch kita TAK burukkan (cuma 2× buat jurang nampak). Minor semua pre-accepted.
- **Master keputusan:** push feature dulu, fix arkib = follow-up berasingan. Betulkan claim spec.
- **Doc commit `10fd48a`:** nota limitasi arkib §5.3 dalam spec + backlog CLAUDE.md projek (fix: dedup SUM atas DISTINCT id_rumah,id_acara,kedudukan; AMARAN ubah angka arkib lampau).
- **Seal (Geass):** Build CLEAN, Secrets CLEAN, syntax 5 fail OK, unit test 3/3, **Playwright chromium 22/22 PASS** (2× — sebelum & selepas push, sandbox-disabled izin master). SEALED.
- **PUSH:** `22366a5..10fd48a test->test` (8 commit: 7 feature + 1 doc). GitHub Actions deploy staging SIAP (statistik-mata.js 200, penanda kiraMataStatistik ADA di statistik/admin/arkib-cetak). Migration test DB dah dibuat semalam.
- **[KEMASKINI ~11:15] FIX ARKIB SIAP (master arah baiki sekarang, bukan tangguh):** master tanya sahkan "markah 20 = satu kumpulan bukan setiap peserta". Lucy trace kod: LIVE 100% betul (save dedup `rumahMarkahDone` + grid `SELECT DISTINCT` :1975 → 20 sekali per rumah). Cuma ARKIB salah. Master arah fix sekarang. **Fix `4b3d80d`:** 2 query arkib (`markah_rumah` ~3151, `statistik` ~3162) dibungkus subquery `SELECT DISTINCT (id_rumah,id_acara,kedudukan,markah_diperoleh)` sebelum SUM. **Validasi test DB (data langsung, izin sandbox):** BARU==`rumah_sukan.jumlah_markah` beza 0 utk 3 rumah (337/335/339), LAMA melambung ~57% (529/527/531). Doc commit `73c6a9b` (spec §5.3 tanda DIBAIKI + buang dari backlog). Seal ✅ (build clean, unit 3/3, Playwright 22/22 ×lagi). Push `10fd48a..73c6a9b test->test` (cubaan ke-2, timeout intermittent). Nota: test DB `n_arkib=0` → fix arkib TAK visible staging, bukti = validasi DB.
- **HEAD test = `73c6a9b`.** main masih `5160d5b` (belum merge). Total 10 commit test belum merge main (7 feature + 1 doc limitasi + 1 fix arkib + 1 doc fixed).
- **⚠️ NEXT:** master verify VISUAL staging (`sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev`, admin_dba1097/123456): (1) toggle Individu/Berkumpulan simpan berasingan; (2) Statistik KUTIPAN==JUMLAH tiap rumah; (3) NILAI PINGAT 2 baris; (4) arkib cetak 2 baris — TAPI arkib KUTIPAN≠JUMLAH untuk tahun relay = BUG LAMA diketahui, bukan baru. Master confirm → migration production `olahraga-db` (BELUM) → merge main `--no-ff` → push main auto-deploy → sahkan `atletik.celikguru.my`.


### 🆕 Sesi 2026-07-07 (malam ~00:08–01:13): sistem-olahraga — Markah Berkumpulan ✅ 7 TASK EXECUTED (subagent-driven), ⏳ PENDING final review + deploy
**Sambung dari spec+plan.** Master pilih **Subagent-Driven** → execute penuh 7 task. **Semua review bersih (spec ✅ + quality Approved, 0 Critical/0 Important setiap task).**
- **HEAD = `0de57fb` (test branch, LOKAL — 7 commit kod BELUM PUSH).** Plan `22366a5` dah push. Working tree bersih.
- **Commit setiap task:** T1 migration `984f98d` → T2 backend kira/simpan jenis+fallback `3ae654c` → T3 admin toggle `beebe1a` → T4 statistik/arkib +bilangan_peserta+jenis `a73673c` → T5 helper `kiraMataStatistik`+unit test `d7a64b6` → T6 statistik.js 2 baris NILAI+KUTIPAN reconcile `cd660c1` → T7 arkib-cetak.js sama `0de57fb`.
- **Migration test DB SUDAH dijalankan** (`wrangler d1 execute olahraga-test --remote`, izin master): 24 rekod lama → `jenis='individu'`, 0 data hilang. **Production migration (`olahraga-db`) BELUM — tunggu confirm master.**
- **T5 helper (risiko tertinggi) di-hand-trace reviewer:** reconcile 30 (johan ind 10 + relay 20), fallback, zero — semua betul. Unit test `# pass 3` (fail `tests/statistik-mata.unit.js` gitignored, tak commit — hanya `public/statistik-mata.js` di-commit).
- **Ledger:** `.superpowers/sdd/progress.md` (semua task tanda complete + senarai Minor findings). Briefs/reports/diffs dalam `.superpowers/sdd/` (untracked).
- **Minor findings terkumpul (untuk triage final review, tiada yg blocking):** jenis-drift bila bilangan_peserta diubah selepas simpan (pre-accepted, dlm limitasi spec); GET tetapan-markah interleave (frontend tapis, by design); admin.js load-race (tak mungkin) + gaya ??/|| + aria-pressed; schema.sql tak reflect kolum jenis (migration=source); magic number `>=4` bukan shared constant (konvensyen sedia ada).
- **⚠️ NEXT (sambung esok — master arah berhenti sini):**
  1. **Final whole-branch review (opus)** — dispatch DITOLAK master (stop). Diff dah sedia: `.superpowers/sdd/review-22366a5..0de57fb.diff` (7 commit, 35KB). Guna template `requesting-code-review/code-reviewer.md`. Base=`22366a5` Head=`0de57fb`.
  2. Triage Minor → `commit-seal` (Geass) → **push test** (7 commit) → GitHub Actions deploy staging.
  3. Verify staging: KUTIPAN MATA == JUMLAH MATA setiap rumah; admin toggle simpan ind+kump berasingan; 2 baris NILAI PINGAT; arkib cetak sama.
  4. Master confirm staging → **jalankan migration production `olahraga-db`** → merge main `--no-ff` → push main auto-deploy → sahkan `atletik.celikguru.my`.
- **Task SEDERHANA, guna proses penuh brainstorm→spec→plan→subagent-driven. BELUM sentuh production langsung.**


### 🆕 Sesi 2026-07-06 (petang–malam ~21:02–23:34): sistem-olahraga — Susunan + Cetak Senarai Peserta & Senarai Mula ✅ SEMUA (6) LIVE PRODUCTION
**Konteks:** master tambah baik paparan/cetak page guru rumah (pendaftaran) + fix bug + tambah baik cetak senarai mula acara padang (admin). 6 kerja, semua flow test→verify staging→merge main `--no-ff`. **main terkini = `5160d5b`.**

**Kerja 1 — Susunan senarai (backend, `main f678ad7`):**
- **Masalah:** master tanya "nama disusun ikut apa?" → semak backend: endpoint `GET /api/peserta` (`src/index.js` baris 1338) guna `ORDER BY pa.id_pendaftaran DESC` = ikut masa daftar (terbaru dulu), nampak rawak.
- **Fix (1 baris, baris 1361):** tukar ke `ORDER BY k.nama_kategori ASC, a.nama_acara ASC, m.nama ASC` → berkumpul kemas kategori → acara → nama (A→Z). Konsisten dgn cetak + endpoint lain sistem. 0 migration, 0 frontend.
- **Nota:** acara disusun sebagai TEKS (100m sebelum 60m) — perangai sedia ada seluruh sistem, master OK.
- Commit test `fddbed0` → master verify staging → merge main `--no-ff` `f678ad7`.

**Kerja 2 — Cetak 3 kategori satu halaman (frontend, `main 56863f2`):**
- **Masalah:** cetak (`public/pendaftaran.js` fungsi `cetakPendaftaran`) pisah page ikut BILANGAN peserta (≤6→6 blok, ≤20→2 blok, >20→1 blok) → L1 L2 L3 berpecah antara page.
- **Master pilih (AskUserQuestion):** 3 kategori satu page TETAP → page1 L1 L2 L3, page2 P1 P2 P3.
- **Fix (baris ~553-580, -20 baris):** ganti logik bertingkat dgn `for` potong 3-3 (`PER_PAGE=3`, `entries.slice`). Bergantung pada Kerja 1 — data dah tersusun ikut kategori jadi potong 3-3 dapat kumpulan betul. Frontend-only, 0 backend.
- Commit test `2020bb5` → master verify staging → merge main `--no-ff` `56863f2`.

**Kerja 3 — Cetak: table kategori tak terpisah (frontend, `main b2e02fb`):**
- Tambah `page-break-inside: avoid` pada `.blok` (style cetak `pendaftaran.js`) → table satu kategori tak terpotong tengah; kalau tak muat baki page, turun penuh ke page seterusnya. 1 CSS property.
- Merge ni turut bawa commit doc `5f48767` (kemaskini CLAUDE.md projek main commit `90acc60`→`56863f2`). Commit test `b2393ee` → merge main `b2e02fb`.

**Kerja 4 — FIX BUG: senarai mula padang ikut giliran (frontend, `main c15ccaf`):**
- **Bug master:** tab Lorong (sub_admin) → acara padang → jana giliran rawak → simpan → cetak senarai mula, susunan TAK sama macam disimpan.
- **Root cause (systematic-debugging):** aliran betul kecuali 1 langkah — giliran disimpan betul ke `lorong_pelepas` (INTEGER) via PATCH `/api/lorong`; backend `/api/peserta/acara/:id` pulang `ORDER BY lorong_pelepas ASC` (betul). TAPI `admin.js` SUSUN SEMULA padang ikut `nama_murid` (baris 1904 cetak satu acara + 2341 cetak batch) → buang susunan giliran. Komen kod sendiri: "padang & lompat bebas (ikut nama)".
- **Fix:** cabang `else` (padang/lompat) tukar ke `(a.lorong||999)-(b.lorong||999) || nama` → susun ikut giliran, fallback nama utk belum diundi (NULL). 2 tempat, 3 baris. Bukti node: sort lama Ali(g2),Bakar(gnull),Cik(g3),Zaki(g1) ❌ → baru Zaki(g1),Ali(g2),Cik(g3),Bakar(gnull) ✅.
- Commit test `1c2bfd4` → merge main `c15ccaf`.

**Kerja 5 — Cetak senarai mula: 3 acara satu page + jadual tak terpisah (frontend, `main 97e3297`):**
- Master nak cetak senarai mula admin (`cetakYangDipilih` dlm `admin.js`) ikut gaya cetak guru rumah. Unit di sini = ACARA (bukan kategori) — master sahkan "3 acara satu page" via AskUserQuestion.
- **Fix:** buang logik `BIG_THRESHOLD` (max 2/page) → potong 3-3 (`PER_PAGE=3`). Tambah `page-break-inside:avoid` pada `.event-blok`. **Turunkan margin antara acara 30mm→8mm** (Lucy flag: 30mm buat 3 jarang muat) — master lulus. Frontend-only, -5 baris.
- Commit test `d24aa3e` → merge main `97e3297`.

**Kerja 6 — Cetak senarai mula: ulang header setiap page + ketatkan muat 100% (frontend, `main 5160d5b`):**
- Master uji cetak padang: header (nama kejohanan/sekolah) cuma keluar page 1; 3 acara muat hanya bila set skala cetak 90% manual.
- **Fix (`cetakYangDipilih` dlm `admin.js`):** suntik `.doc-header` ke DALAM setiap `.print-page` (dulu sekali je di luar, sebelum semua page) via const `headerHTML`. Ketatkan font 10pt→9pt (body/table/event-tajuk) + kurang jarak (doc-header margin 4mm→2mm, h1 12→11pt, h2 10.5→9.5pt, event-blok margin 8mm→6mm & padding 2→1.5mm, th/td padding 2px→1px) supaya 3 acara + header muat 100% tanpa set skala manual.
- **Nota master:** "muat 100%" bergantung bilangan peserta setiap acara (acara ramai = jadual tinggi) — master OK, boleh laras lagi kalau ada acara tak muat.
- Commit test `b56a147` → merge main `5160d5b`.

**Nota keseluruhan:** semua kerja KISS, tiada tertunggak. Kerja 1 (susunan kategori) jadi asas Kerja 2 (potong-3). **Corak cetak standard sekarang: potong-3 blok/page + `page-break-inside:avoid` pada blok** (dipakai pendaftaran.js `.blok` + admin.js `.event-blok`). CLAUDE.md projek: main commit ditulis `56863f2` (Kerja 3), sebenarnya dah `5160d5b` — master arah **biarkan**, update sekali je bila ada perubahan lain. Untracked files (`.superpowers/`, `Project Resources/`, `docs/mockup/`, beberapa plan/spec, 1 screenshot) belum diputuskan. **Network push GitHub timeout intermittent** beberapa kali sesi ni — retry berjaya (isu diketahui).

### 🆕 Sesi 2026-07-06 (tengah hari + malam ~23:37–23:54): sistem-olahraga — Markah Berbeza Acara Berkumpulan ✅ BRAINSTORM SELESAI + SPEC SIAP (belum execute)
**[KEMASKINI malam 23:54]** Sambung brainstorm dari tengah hari. Bahagian 2 diputuskan + spec penuh ditulis.
- **Bahagian 2 — master pilih BUAT TEPAT (ikut jenis):** KUTIPAN MATA kira ikut jenis setiap acara (guna `bilangan_peserta` dlm grid) → reconcile penuh dgn JUMLAH MATA. NILAI PINGAT jadi 2 baris (Individu + Berkumpulan). BILANGAN PINGAT & JUMLAH MATA kekal.
- **KISS+DRY (master ingatkan):** 2 set sahaja (heuristik `bilangan_peserta>=4`, tiada medan jenis DB baru). Helper baru `public/statistik-mata.js` `kiraMataStatistik()` dikongsi `statistik.js` + `arkib-cetak.js` (elak salin logik renderStatistik yg identik).
- **Fallback penting (dari spec self-review):** kalau set berkumpulan kosong (guru belum set) → guna set INDIVIDU, dipakai DI KIRA (elak relay dapat 0 markah = regresi) DAN DI PAPARAN (markahKump default markahInd, kekal reconcile). Lihat spec §4.4a.
- **Titik sentuh kod disahkan (grep):** tetapan_markah — GET `index.js:249`, POST `:266` (id individu `{sek}-pos-{ked}` kekal, berkumpulan `{sek}-berkumpulan-pos-{ked}`), kira final `:1844` (isBerkumpulan dah wujud `:1747`), statistik ringkasan markah `:1941`, statistik/grid `:1955` (+bilangan_peserta), arkib legend `:3175`, reset DELETE `:2870` (tak ubah). Admin borang `admin.html:371` input m1-m8, `admin.js:955` muat + `:978` submit.
- **SPEC:** `docs/superpowers/specs/2026-07-06-markah-berkumpulan-design.md` (commit `4a06692`, push test). Self-review ✅ (fallback ditambah).
- **NEXT:** master review spec → kalau OK, `writing-plans` → execute (task SEDERHANA). BELUM sentuh kod implementasi.

--- (rekod asal tengah hari di bawah) ---
### 🆕 Sesi 2026-07-06 (tengah hari ~13:09–14:43): sistem-olahraga — Markah Berbeza Acara Berkumpulan ⏳ BRAINSTORM SEPARUH JALAN [✅ SELESAI — lihat kemaskini di atas]
**Permintaan master:** acara berkumpulan (relay) patut ada markah pemenang BERBEZA dari acara individu. Sekarang guna satu set sahaja.
- **Penemuan keadaan semasa (penting):**
  - Konfig markah pemenang disimpan dalam jadual DB `tetapan_markah` (id_tetapan PK=`{id_sekolah}-pos-{kedudukan}`, kolum: id_sekolah, kedudukan, markah). Per sekolah (multi-tenant), `ON DELETE CASCADE`.
  - Admin set via `public/admin.html:371` borang "Konfigurasi Sistem Pemarkahan", input `m1`–`m8`. **Default HTML: 10/8/6/5/4/3/2/1** (Johan→ke-8). `admin.js:955` muatTurun + `:978` submit → `POST /api/tetapan-markah`.
  - API: `GET/POST /api/tetapan-markah` (`src/index.js:248`, POST guard `checkRole('sub_admin')`, upsert `ON CONFLICT`).
  - **"Berkumpulan" TIADA medan jenis sebenar** — dikesan via heuristik `bilangan_peserta >= 4` (`isBerkumpulan`, dipakai seluruh sistem: had pendaftaran, kiraan). `jenis_acara` dalam DB = 'Balapan'/'Padang' (bukan individu/kumpulan).
  - **Pengiraan markah** (`src/index.js` blok final heat=0, ~1842-1898): bina `mapMarkah[kedudukan]=markah` dari tetapan_markah, `markah_diperoleh = mapMarkah[k.kedudukan] || 0`, tambah ke `rumah_sukan.jumlah_markah`. Relay guna `Set rumahMarkahDone` supaya markah dikira SEKALI per rumah+kedudukan (tak double). **TAPI nilai markah sama untuk individu & berkumpulan** — itu jurangnya.
  - Guna markah juga di: `index.js:1844` (kira+tolak-lama), `:1940` (statistik/ringkasan legend), `:3175` (arkib legend). Paparan: `statistik.js` + `arkib-cetak.js renderStatistik` (baris NILAI PINGAT, KUTIPAN MATA guna markahMap).
- **KEPUTUSAN BRAINSTORM setakat ni (master sahkan via AskUserQuestion):**
  1. **Skop = Individu vs Berkumpulan** (2 set sahaja, bukan Balapan/Padang/Relay atau per-acara). KISS.
  2. **UI = satu borang dengan TOGGLE/tab** Individu↔Berkumpulan, kedua-dua kekal 8 kedudukan.
  3. **Default markah berkumpulan = 2× individu → 20/16/12/10/8/6/4/2.**
- **REKA BENTUK Bahagian 1 (master LULUS):**
  - Definisi berkumpulan kekal `bilangan_peserta >= 4` (elak refactor besar, tiada medan jenis baru).
  - Migration: `ALTER TABLE tetapan_markah ADD COLUMN jenis TEXT NOT NULL DEFAULT 'individu';` (semua rekod lama auto 'individu', selamat, tak sentuh PK). ID: individu kekal `{id_sekolah}-pos-{kedudukan}`, berkumpulan `{id_sekolah}-berkumpulan-pos-{kedudukan}`.
  - Kira keputusan: `jenisMarkah = isBerkumpulan ? 'berkumpulan' : 'individu'` → SELECT ... WHERE id_sekolah=? AND jenis=?. Logik tolak-lama guna set sama (konsisten, tak tersilap).
- **Bahagian 2 (BELUM DIPUTUSKAN — master minta clarify, belum jawab):** Jadual Statistik/Arkib ada baris NILAI PINGAT + KUTIPAN MATA yang kira `bilangan × markahMap[kedudukan]`. Pingat dikumpul ikut KEDUDUKAN sahaja (tak tahu jenis) → KUTIPAN MATA jadi TAK TEPAT untuk relay (cth HIJAU Johan relay 20 + Johan individu 10 → papar 2×10=20, sebenar 30). JUMLAH MATA (dari rumah_sukan.jumlah_markah) SENTIASA betul. 3 pilihan dicadang: (A) buat tepat—hantar jenis dalam grid, kira per-acara; (B) terima anggaran KISS + nota; (C) buang baris KUTIPAN. **Master reject soalan, minta clarify dulu — belum bagi input.**
- **NEXT (sambung sesi depan):** clarify + putuskan Bahagian 2 → siapkan reka bentuk penuh → tulis spec `docs/superpowers/specs/2026-07-06-markah-berkumpulan-design.md` → spec self-review → master review → writing-plans → execute. Task SEDERHANA (1 migration + backend kira + admin.html/js toggle + mungkin statistik display). BELUM sentuh kod langsung.

### 🆕 Sesi 2026-07-06 (tengah hari ~12:00–13:06): mypwa-v2 — Ringkasan Analisa ETR (page Trend) ⏳ SPEC + PLAN SIAP, BELUM EXECUTE
**Permintaan master:** buat **analisa untuk ETR** dalam page Trend. Selepas brainstorm penuh, skop dijelaskan: (1) **berapa ramai murid CAPAI ETR** + (2) **taburan gred sasaran** (berapa murid disasarkan ke setiap gred), per subjek.
- **Keputusan brainstorm (semua master sahkan via AskUserQuestion):**
  1. Metrik: bil capai ETR (status='capai' ÷ murid ada ETR) + taburan gred sasaran (petakan nilai ETR → gred).
  2. Petakan gred **guna skala gred ujian sumber TOV**.
  3. Surface = **seksyen dalam `trend.html`** (bukan page/tab baru). Audiens = guru+pelawat+admin (semua yang dah akses trend — tiada kawalan akses baru).
  4. **Filter berubah:** Sesi + **Tahun**(wajib) + **Kelas**(opsyen, default "Semua Kelas"). *(Master mula pilih "kelas dipapar sahaja", lepas tu tukar ke Tahun+Kelas — subteks jadi "Pilih sesi dan tahun...".)*
  5. **Semua Kelas** → Ringkasan ETR agregat seluruh tahun SAHAJA (sorok senarai murid). **Satu kelas** → Ringkasan + senarai murid sedia ada.
  6. Bentuk paparan = **carta palang mendatar per subjek** (guna semula corak `buildChartHtml` dashboard). Cetak = sertakan ringkasan.
- **Nota teknikal penting:** ETR sentiasa ≥ 50 (kiraETR sasar ≥ lulus) → taburan gred sasaran **takkan ada 'Tidak Menguasai'**. Frontend vanilla **tak boleh import `etr.mjs`** (tiada build step) → agregat dikira **server-side** (endpoint pulang array `ringkasan`), frontend render sahaja (DRY + boleh unit test).
- **Reka bentuk backend:** `GET /trend` ubah `kelas_id`→`tahun`+`kelas_id`(opsyen); tambah `tovUid` per subjek; pulang `ringkasan`+`bilTiadaTov`; `kelas`→objek skop `{tahun,kelas_id,nama_kelas}`. **0 migration, 0 package.** Utiliti baru `gredBagi()` + `ringkasanETR()` dalam `src/utils/etr.mjs` (pure, unit-tested).
- **Spec:** `docs/superpowers/specs/2026-07-06-analisa-etr-design.md` (commit `caeb50f`, push test). **Plan:** `docs/superpowers/plans/2026-07-06-analisa-etr.md` (5 task TDD, commit `f087d0a`, push test). Self-review plan ✅ (spec coverage, placeholder, type consistency).
- **NEXT (sambung sesi depan):** master pilih cara execute (Subagent-Driven disyorkan / Inline) → laksana 5 task → sight-hone → commit-seal → push test → verify staging → merge main (tunggu confirm master untuk production). Task SEDERHANA.

### 🆕 Sesi 2026-07-06 (pagi ~10:38–11:55): sistem-olahraga — Banner Amaran Tarikh Tutup Guru Rumah ✅ LIVE PRODUCTION (main `954b281`)
**Masalah master:** bila admin set tarikh tutup masa depan, guru rumah tak nampak apa-apa di page pendaftaran — banner "tutup" sedia ada cuma muncul SELEPAS terlambat.
- **Penemuan:** `pendaftaran.js` dah baca `t.tarikh_tutup_pendaftaran` dari `/sekolah/tetapan` + ada banner merah `#banner-pendaftaran-tutup` (muncul bila status=0 atau tarikh lepas). Jurang = tiada papar bila BUKA + tarikh masa depan. **Frontend sahaja**, data dah ada.
- **Reka (master pilih via AskUserQuestion):** banner amber di slot atas sama (bukan baris kecil / bukan escalation merah). KISS.
- **Perubahan:** `pendaftaran.html` +banner amber `#banner-pendaftaran-buka` (ikon jam, hidden default). `pendaftaran.js` +cabang `else if (t.tarikh_tutup_pendaftaran)` — papar "Pendaftaran akan ditutup pada [tarikh]" guna `toLocaleString('ms-MY',{dateStyle:'medium',timeStyle:'short'})` (sama format banner tutup, DRY). 18 baris, 0 backend/migration.
- **Verify:** Playwright 4/4 PASS (fail lokal `tests/pendaftaran-banner.spec.js`, gitignored) — uji terpencil: seed localStorage lepas guard guru_rumah + mock semua `/api` (page.route), suntik tarikh senario (depan→amber, lepas→merah, tiada→takde, status=0→merah). Tak sentuh DB sebenar. + screenshot visual disahkan (`screenshots/banner-amber-guru-rumah.png`). Guna sandbox-disabled (master arah "run playwright").
- **Deploy:** commit test `e6f3262` → verify staging (node fetch) → Playwright 4/4 → merge main `--no-ff` `954b281` → push main auto-deploy → production `atletik.celikguru.my` disahkan live (node fetch grep penanda). TIADA kerja tertunggak.
- **Pengajaran:** page `pendaftaran.html` guard client-side WAJIB `peranan==='guru_rumah'` (redirect kalau tak) — untuk uji tanpa creds, seed localStorage + mock API (logik frontend terpencil, deterministik, tiada side-effect DB). Corak ni bagus untuk uji perubahan render frontend.

### 🕐 Sesi terdahulu (2026-07-06 awal pagi, ~03:05)
**Session Focus**: ✅ sistem-olahraga (tab Keputusan) — SEMUA LIVE PRODUCTION. Sesi panjang (~22:41 5 Julai → 03:05 6 Julai): (1) fix bug panel balapan tersembul acara padang (main `a7ca618`); (2) footer butang konsisten mobile (main `a449778`); (3) **FEATURE: Kad Responsif tab Keputusan mobile Fasa 1** — key-in Padang+Balapan guna phone tanpa scroll kiri-kanan, jadual→kad menegak ≤639px, guna proses penuh brainstorm→spec→plan→subagent-driven (main `810c8b6`). Lompat Tinggi = Fasa 2 (backlog). TIADA kerja tertunggak.

### 🆕 Sesi 2026-07-06 (awal pagi ~00:22–03:05): sistem-olahraga — Feature Kad Responsif Tab Keputusan Mobile (Fasa 1) ✅ LIVE PRODUCTION (main `810c8b6`)
**Permintaan master:** master suka key-in rekod pemenang guna phone; nak daftar keputusan tanpa scroll kiri-kanan di tab Keputusan.
- **Proses penuh (task sederhana):** brainstorming → spec → writing-plans → subagent-driven-development. Master pilih setiap keputusan via AskUserQuestion.
- **Keputusan brainstorm:** (1) skop = ketiga-tiga acara, TAPI pecah fasa; (2) cara key-in = **senarai kad** (semua peserta, scroll atas-bawah) bukan satu-per-satu; (3) Pendekatan **A = CSS kad responsif** (guna semula corak `papan.html`, breakpoint ≤639px, desktop kekal) — bukan JS render kad (Pendekatan B ditolak, risiko pecah Kira/Simpan); (4) **Fasa 1 = Padang + Balapan** dulu, **Lompat Tinggi = Fasa 2** (matriks 2D peserta×ketinggian, perlu reka kad khas).
- **Spec:** `docs/superpowers/specs/2026-07-05-keputusan-kad-mobile-design.md` (commit `b12308d`). **Plan:** `docs/superpowers/plans/2026-07-05-keputusan-kad-mobile.md` (commit `517978d`).
- **Mekanisme:** tambah kelas kongsi `results-card` pada jadual (#table-padang, #table-balapan, jadual heat suntikan) + atribut `data-label`/`data-cell` pada `<td>` semasa render; satu blok `<style>` media query (≤639px) tukar `tr`→kad, `td`→blok berlabel, sorok thead. **Logik Kira/Simpan tak disentuh** (kelas input `.input-cuba-*`/`.input-masa-*`/`.input-manual-*` kekal).
- **Subagent-driven:** Task 1 (haiku) kad Padang + CSS kongsi → commit `604b4af`, review sonnet Spec✅ Quality✅. Task 2 (haiku) kad Balapan biasa+heat → commit `4995ea8`, review sonnet ✅. Final whole-branch review (sonnet) = MERGE-READY 0 Critical/Important.
- **Fix susulan (master verify phone):** badge Kedudukan `position:absolute` **bertindih nama panjang** di kad padang. Fix: buang absolute → Kedudukan jadi blok berlabel mengalir sebaris (guna rule `td[data-label]`), buang `padding-right` nama. Commit `5ccd2ad`.
- **Deploy:** push test → poll staging (node fetch) → Playwright 18/18 → master verify phone OK → merge main `--no-ff` `810c8b6` → production disahkan live (node fetch grep penanda).
- **Minor diterima master:** kad Balapan — nombor Lorong duduk baris sendiri atas nama (bukan sebaris macam mockup). Master OK. Boleh laras nanti kalau nak (kerja kecil).

**Pengajaran teknikal:**
- **Flexbox blockification:** `<td>` dengan inline `display:table-cell` (mod manual/heat) dalam `tr{display:flex}` → dikira `block` automatik (CSS Display L3), jadi CSS kad tak pecah. `display:none` kekal none. Tak perlu `!important` pada display.
- **`position:absolute` badge atas teks flow = rapuh** bila teks panjang (padding reserve tak cukup). Lebih kukuh guna blok mengalir biasa.
- **Corak kad responsif papan.html** = pattern standard master suka: thead sorok, tr→kad flex-wrap, td→blok, `td[data-label]::before{content:attr(data-label)}` untuk label dinamik.
- Subagent-driven sesuai untuk task transkripsi kod (plan ada kod penuh) — haiku implementer + sonnet reviewer, murah & laju.

### 🆕 Sesi 2026-07-05 (malam ~22:41–23:42): sistem-olahraga — Fix Bug Panel Balapan Padang + Footer Butang Mobile ✅ DUA-DUA LIVE PRODUCTION
**Konteks:** master report bug di tab Keputusan → Padang (screenshot Lontar Peluru).

**Kerja 1 — Bug panel balapan tersembul dalam acara padang (`public/keputusan.js`):**
- **Masalah:** bila pilih acara padang (lompat tinggi/lontar peluru/lompat jauh), kad LORONG (panel-balapan, ada radio Catatan Masa/Manual) tersembul di bawah kad padang yang betul.
- **Punca (systematic-debugging):** `removeHeatPanel()` (baris 348-353) buat `panel-balapan.classList.remove('hidden')` TANPA syarat — direka untuk restore panel balapan lepas keluar mod Heat. Tapi ia dipanggil juga dalam laluan `if (isPadang)` (baris 240) → buka balik panel balapan walau konteks padang. `switchTab('padang')` dah sorok betul, tapi bila master PILIH acara dari dropdown, handler jalankan removeHeatPanel() → panel balapan muncul balik.
- **Fix (+1 baris, di tempat panggilan bukan dalam fungsi):** `if (isPadang) { removeHeatPanel(); panelBalapan.classList.add('hidden'); ... }`. Selamat — laluan balapan tak disentuh (removeHeatPanel kekal buka panel balapan untuk konteks balapan).
- **Deploy:** commit `fa07424` → push test → poll staging (node fetch) → Playwright 18/18 → master verify production OK → merge main `--no-ff` `a7ca618`.

**Kerja 2 — Butang footer Keputusan konsisten di mobile (`public/keputusan.html`, 3 panel):**
- **Masalah:** butang Kira Kedudukan (Auto) / Cetak / Simpan tak konsisten saiz di phone. Punca: padding melintang beza (px-6/px-5/px-8) + `flex justify-between` paksa satu baris sesak.
- **Iterasi (master pandu via AskUserQuestion + feedback):**
  1. Mula: `display:contents` pada wrapper Cetak/Simpan di mobile → semua butang jadi sibling `flex-1` sama lebar. Commit `26ab979`.
  2. Master: "Kira masih besar" — punca `flex:1 1 0` + `min-width:auto` (item flex tak shrink bawah min-content teks panjang). Tambah `min-w-0` + kurang padding mobile `px-2 sm:px-6/5/8`. Commit `3afae4d`.
  3. Master cadang layout lebih kemas: **Kira melintang penuh (w-full) atas, Cetak+Simpan 50/50 bawah**. Footer `flex flex-col sm:flex-row`, Kira `w-full sm:w-auto`, wrapper `flex gap-2 w-full sm:w-auto`, Cetak/Simpan kekal `flex-1 min-w-0`. Commit `9b18957`.
- **Nota:** panel Balapan tiada Cetak → Auto Ranking penuh atas, Simpan penuh bawah.
- **Deploy:** setiap commit push test → poll staging → master verify phone → akhirnya merge main `--no-ff` `a449778`.

**Pengajaran teknikal:**
- `flex-1` sahaja TAK cukup untuk butang sama lebar bila ada teks panjang — WAJIB tambah `min-w-0` (override `min-width:auto`) supaya item boleh shrink bawah min-content.
- `display:contents` (Tailwind `contents`) = cara flatten wrapper div jadi anak-anaknya naik sibling flex parent (guna `contents sm:flex` untuk mobile-only flatten).
- Playwright spec (`tests/e2e.spec.js`) — **fail lokal (gitignored `/tests/`)**, BASE_URL staging. Password sub_admin `admin_dba1097` betul = **`123456`** (spec sempat lapuk `1234` → betulkan lokal, tak masuk git). Superadmin `dragon`/`f4994`.
- Verify guna node fetch (poll staging keputusan.js/html grep penanda baru) — browser extension + curl masih bermasalah, node fetch (TLS sendiri) berjaya. Playwright guna sandbox-disabled (izin master setiap kali).

### 🆕 Sesi 2026-07-05 (malam ~20:49–22:36): sistem-olahraga — Fix Jantina Murid SK Salor + Kad Pemenang Mobile ✅ DUA-DUA LIVE PRODUCTION
**Konteks:** master check jadual pendaftaran SK Salor (`id_sekolah=DBA1097`) → jumpa masalah data + UI.

**Kerja 1 — Baiki jantina 231 murid (data fix D1 production):**
- **Masalah:** master update senarai murid dulu tapi lupa isi jantina → SEMUA 467 murid tertag `L`. Rumah HIJAU pula dah daftar 70 pendaftaran acara (semua murid lelaki).
- **Penemuan kritikal:** endpoint `POST /api/murid/csv` guna **`INSERT OR IGNORE`** (src/index.js ~906) — re-upload CSV **TAK update** murid sedia ada, cuma SKIP. Jadi fix mesti **UPDATE jantina in-place ikut `id_murid`** supaya FK `pendaftaran_acara`→`murid` (ON DELETE CASCADE) kekal utuh (bukan padam-insert yang tukar id_murid → orphan).
- **CSV master:** `Downloads/templat_murid (1).csv`. Semakan silang jantina-CSV vs penanda nama (BIN→L, BINTI→P): mula ada silap (cth BIN tertag P), master betulkan → pusingan kedua **0 silap, 0 nama tanpa penanda**. 468 baris (237 L / 231 P).
- **Reconciliation (nama+tahun+kelas):** 467 murid DB padan penuh CSV; **231 akan tukar L→P**; **0 murid TERDAFTAR yang jantina berubah** (semua 70 pendaftaran HIJAU = murid lelaki kekal L → tiada konflik); 1 murid CSV tiada di DB = **AHMAD FATHAN BIN MOHD LOKMAN** (master arah PADAM sebab dah pindah — dia memang tak wujud di DB, cuma dikeluarkan dari fail CSV).
- **Backup dulu:** 70 pendaftaran HIJAU → `backup/pendaftaran_hijau_backup_20260705-210937.json` + `.sql` (INSERT restore).
- **Apply:** `UPDATE murid SET jantina='P' ... WHERE id_murid IN (231 id)` → **231 rows written**. Sahkan: 236 L / 231 P (jumlah 467) + 70 pendaftaran valid, 0 orphan. Query guna `wrangler d1 execute olahraga-db --remote` (sandbox-disabled, izin master).
- **Memory baru:** `reference_olahraga_csv_upload.md` (gotcha INSERT OR IGNORE + guna UPDATE by id_murid).

**Kerja 2 — Papan skor: jadual Pemenang Terkini jadi kad bertindan di mobile:**
- **Masalah:** di telefon, jadual "Pemenang Terkini" (`papan.html`) perlu scroll kiri-kanan untuk nampak kolum Kedudukan. Punca: `min-width:420px` pada jadual + kolum Ked. paling kanan.
- **Pilihan master (AskUserQuestion):** **kad bertindan** — desktop kekal jadual, mobile (≤639px) tukar baris → kad (acara atas, nama+rumah bawah, badge Ked. sudut kanan atas).
- **Pelaksanaan (CSS sahaja + kemas JS):** `public/papan.html` — tambah `id="winners-table"`, media query `@media (max-width:639px)` (thead disembunyi, tr/td jadi block, badge `position:absolute` kanan atas, benarkan nama panjang wrap elak overflow), buang kolum "Kelas" (header tersembunyi tapi td badan masih render → header/badan tak sejajar, colspan 4→3). `public/papan.js` — buang `<td>` kelas, colspan 4→3.
- **⚠️ Isu ditangani:** sel Pemenang ada `white-space:nowrap` + nama panjang (cth "TUAN MUHAMMAD ADAM BIN...") → dalam kad boleh overflow → tambah `white-space:normal !important` di mobile.
- **Deploy:** commit test `cb80632` → master verify telefon OK → merge main `--no-ff` `07d2491` → push main auto-deploy. **Verify production** guna node-fetch (curl tersekat TLS sesi ni): papan.js (tiada p.kelas, colspan 3) + papan.html (id winners-table, media query, badge absolute, tiada th Kelas, min-width:420px desktop kekal) — semua penanda baru ADA, lama TIADA. **LIVE.**
- **Nota env:** browser extension (claude-in-chrome) **gagal attach** sepanjang sesi (`Cannot attach to this target`) + `file://` disekat → verifikasi visual guna browser tak dapat; ganti dengan node-fetch semak fail production. curl exit 35/code 000 (TLS/sandbox) — guna `node fetch` (TLS sendiri, macam git) berjaya.

### 🆕 Sesi 2026-07-04 (malam ~00:43–01:53): mypwa-v2 — Logik ETR Baru (Berasaskan Gred) ✅ LIVE PRODUCTION (main `5afd2ff`)
**Sambung dari spec 2026-07-03.** Execute penuh guna subagent-driven-development + 2 follow-up UX dari master.
- **Plan:** `docs/superpowers/plans/2026-07-04-etr-logik-baru-trend.md` (commit `5532c90`, pushed test).
- **Deviasi spec (sengaja, testability):** `kiraETR` diekstrak ke `src/utils/etr.mjs` (ESM) bukan inline dalam ujian-markah.js — sebab package.json tiada `"type":"module"`, jadi `.mjs` satu-satunya cara `node --test` fungsi tulen tanpa pecahkan playwright.config.js (CommonJS). Folder src/utils sedia wujud.
- **Task 1 (haiku):** `src/utils/etr.mjs` `kiraETR(tov,scale,cap)` + `tests/etr.unit.mjs` (node --test, .unit.mjs supaya Playwright abaikan). 9 baris proof-table lulus. Commit `ff57331`. Review sonnet ✅ Approved 0C/0I (hand-trace 9 baris).
- **Task 2 (haiku):** endpoint `/trend` guna kiraETR, jejak `tovUid` (ujian sumber TOV) → skala+cap betul, tambah medan `aras`. Commit `375268d`. Review sonnet ✅ Approved.
- **Task 3 (haiku):** trend.html papar "Tidak Menguasai" amber. Commit `59372a4`. Review sonnet ✅ Approved 0 isu.
- **Task 4:** trend.spec.js sedia longgar — tiada kod baru.
- **Final review (opus):** Ready=YES, 0C/0I. Nota confirm-before-ship: pass=50 tetap → kalau ujian sumber markah_penuh≠100 label TM tersilap. **Master sahkan semua ujian /100 → RESOLVED.** Minor test-comment dibaiki → `f1e28ce` (# pass 4).
- **Logik kiraETR:** tov<40→50; 40≤tov<50→min(tov+15,cap); band bukan-tertinggi pisah=lo+round(⅔(hi−lo)), tov<pisah→nextMin else tov+tambahan (jarakDariAtas 1→+10, ≥2→+15); tertinggi tov≤lo+4→+10 else +5; fallback skala kosong→min(tov+15,cap).
- **Follow-up 1 (master):** pecah kolum gabungan "TOV–ETR" → dua kolum berasingan TOV & ETR (desktop+cetak). Commit `7f71ee1`.
- **Follow-up 2→3 (master):** label "Tidak Menguasai" dalam cetak buat kolum TOV tak konsisten (murid lain tiada label). Mula cuba block, master arah **BUANG terus dari cetak** → label KEKAL desktop sahaja. + fix smoke race (waitForFunction tunggu option populate). Commit `90aad7a`→`ef7f8e0`.
- **Playwright smoke:** PASS 3.1s (sandbox-disabled izin master, TEST_USER=test/test123) — bukan skip, 0 ralat JS, 2 kolum terbentuk.
- **Seal:** node --check OK, unit test # pass 4, wrangler dry-run CLEAN, secret bersih.
- **Deploy:** push test → merge main `--no-ff` `5afd2ff` (7 fail, 605+) → push main auto-deploy production. Lucy balik test. **0 migration.**
- **✅ MASTER SAHKAN PRODUCTION OK (2026-07-04 ~01:59):** `erpm-sksalor.celikguru.my/trend` berfungsi live, GitHub Actions deploy hijau. TIADA kerja tertunggak — feature SELESAI.
- **Pengajaran:** (1) test smoke boleh SKIP senyap bila option dropdown async belum populate — waitForFunction tutup race, PASS jadi bukti sebenar; (2) label decorative dalam jadual cetak buat kolum tak konsisten — master prefer BUANG dari cetak daripada dandan style.

### 🆕 Sesi 2026-07-03 (malam ~21:48–22:37): mypwa-v2 — Ubah Logik ETR (Trend Markah) ✅ BRAINSTORM SELESAI + SPEC SIAP [✅ EXECUTED 2026-07-04 — lihat entri di atas]
**Sambung dari brainstorm 2026-07-02.** Master perhalusi logik ETR sepenuhnya → spec ditulis & committed. BELUM sentuh kod.
- **Keputusan brainstorm FINAL (semua disahkan master via AskUserQuestion):**
  1. Baseline = **TOV** (ujian pertama). **Dinamik** ikut skala gred admin (`ujian_gred`). Markah lulus = **50 (tetap)**.
  2. **Corak tunggal** (Lucy jumpa — master setuju): setiap gred, murid di **bawah band** → sasar **naik satu gred** (ETR = markah mula gred atas); di **atas band** → **tambahan tetap** yang mengecil ikut gred.
  3. **Titik pisah bawah/atas = ⅓ atas band** (dinamik): `pisah = lo + round(⅔×(hi−lo))`.
  4. **Tambahan** ikut kedudukan dari gred tertinggi: tertinggi +10/+5, satu bawahnya +10, dua+ bawahnya +15. (Skala A/B/C → C=+15, B=+10, A=+10/+5, padan contoh master.)
  5. **Bawah gred C (tov<50):** berperingkat — `tov<40 → ETR=50`; `40–49 → tov+15`. Semua sasar ≥ gred C.
  6. **Gred tertinggi (A):** `tov ≤ lo+4 → tov+10`; selebihnya `tov+5`. Cap 100.
  7. **Label:** murid bawah lulus = **"Tidak Menguasai"** (istilah PBD, BUKAN "gagal"/"belum lulus"). Gred D/E/F **kekal dipapar** (badge dari skala admin — `appKiraGred` sedia ada, 0 kerja tambahan).
- **Jadual bukti 9 baris** dalam spec — padan tepat semua contoh master (35→50, 45→60, 55→66, 62→77, 70→82, 78→88, 84→94, 92→97, 98→100 cap).
- **Kod sasaran:** `src/routes/ujian-markah.js` endpoint `/trend` — ganti baris **267** (`Math.min(tov+15,100)`) dgn helper baru `kiraETR(tov, scale, cap)`. Jejak `tovUjianId` supaya guna skala + `markah_penuh` ujian **sumber TOV**. Tambah medan `aras='Tidak Menguasai'` bila tov<50. Frontend `trend.html` papar aras + sasaran gred. **0 migration, 0 package.** Task SEDERHANA.
- **Andaian didokumen:** lulus=50 tetap (gred teks bebas, tiada flag lulus — boleh jadi tetapan admin masa depan, YAGNI). Markah skala /markah_penuh. Fallback skala kosong → `min(tov+15,cap)`.
- **Spec:** `docs/superpowers/specs/2026-07-03-etr-logik-baru-trend-design.md` commit `b138c5a` (branch test, **PUSHED** `c1cf928..b138c5a`, origin synced).
- **NEXT (sambung sesi depan):** master review spec → kalau OK, `writing-plans` → execute (task sederhana).

### 🆕 Sesi 2026-07-02 (petang ~11:40–13:44): mypwa-v2 — Ubah Logik ETR (Trend Markah) ⏳ BRAINSTORM BELUM SELESAI (SUPERSEDED oleh entri 2026-07-03 di atas)
**Permintaan master:** ubah logik penetapan ETR dalam feature Trend Markah (endpoint `GET /api/ujian-markah/trend`, `src/routes/ujian-markah.js` baris ~255-275; papar di `public/trend.html`).
- **Logik LAMA (sedia ada, live):** `ETR = min(TOV + 15, 100)`. TOV = markah sah pertama. Status: `terkini >= etr` → ✅ Capai; else jurang = etr − terkini.
- **Logik BARU yang master mahu** (berasaskan gred, bukan +15 tetap):
  - Target minimum = gred C = 50 markah.
  - Murid markah < 50 → ETR = 50 (sasar lulus C).
  - Murid gred C → ETR masuk gred B; gred B → ETR masuk gred A; gred A → ETR = TOV + 10 (maks 100).
- **Keputusan brainstorm setakat ni (master dah sahkan via AskUserQuestion):**
  1. **Baseline = TOV (ujian PERTAMA)** — sasaran tetap sepanjang sesi, konsisten headcount KPM. (bukan markah terkini)
  2. Mula-mula master pilih "nilai ETR = ambang bawah gred atas" + "ikut skala gred sebenar ujian (`ujian_gred`)".
  3. Kes gred A: **ETR = TOV + 10, maks 100** (master tolak idea ETR=100 flat sbg tak logik; tolak juga tengah-julat sbb murid dah 90 takda ruang).
- **⚠️ TWIST TERAKHIR (belum disahkan penuh):** master kata untuk gred C & B, **TAK NAK guna ambang gred** — sebab terlalu senang (murid C dapat 58, ambang B=60 cuma naik 2 markah). Master bagi contoh "murid gred C → ETR di markah permulaan gred B, cth 66".
- **Tafsiran Lucy (PENDING master sahkan):** contoh 66 = murid C dengan TOV 56 → **56 + 10 = 66**. Maksudnya logik jadi SERAGAM: `TOV < 50 → ETR = 50; TOV ≥ 50 → ETR = min(TOV + 10, 100)`. Implikasi: **skala gred TAK diperlukan lagi untuk kira ETR** (cuma untuk papar badge gred). Ini bercanggah sikit dengan keputusan #2 (ikut skala) — perlu jelas dengan master.
- **SOALAN TERBUKA belum dijawab master:**
  1. Sahkan tafsiran "+10 seragam" (66 = TOV 56 + 10)?
  2. Lantai bawah 50: tetap 50, atau bagi TOV+10 juga (cth TOV 45 → 55)?
- **NEXT (sambung sesi depan):** selesaikan 2 soalan terbuka → bentang design final → tulis spec `docs/superpowers/specs/YYYY-MM-DD-etr-logik-baru-design.md` → writing-plans → execute. Task SEDERHANA (1 endpoint backend + mungkin sedikit trend.html). Kemungkinan 0 migration.
- **Nota teknikal:** `appKiraGred(markah, gredScale)` dalam `app.js` — susun skala desc ikut markah_min, cari gred pertama markah≥markah_min. Gred = TEXT bebas (bukan tetap A/B/C). `ujian_list[].gredScale` per ujian dalam response trend.

### 🆕 Sesi 2026-07-02 (pagi ~07:33–08:06): sistem-olahraga — Tambah Acara 50m Templat Balapan Ekspres ✅ LIVE PRODUCTION (main `66b7778`)
**Permintaan master:** tambah acara `50m` dalam senarai template daftar acara ekspres.
- **Lokasi:** `public/admin.js` — constant `PRESET_ACARA.Balapan` (baris ~2685). Ini data preset template sahaja (bukan DB) — admin klik guna template → acara masuk pertandingan. Tiada migration/backend.
- **Perubahan:** tambah `{ nama: '50m', kuota: 2 }` di paling atas (sebelum `60m`) — 50m acara standard tahap 1 sekolah rendah, jadi posisi pertama (paling pendek). 1 baris, 1 insertion.
- **Pipeline (task kecil):** node --check OK → commit `66b7778` → push test (`d6eb3d7..66b7778`) → master confirm → merge main **ff-only** (`90acc60..66b7778`, test linear atas main) → push main auto-deploy production. Lucy balik branch test.
- **Nota:** merge turut bawa `d6eb3d7` (doc tanda slider hakim LIVE) yg dah sedia dalam test → CLAUDE.md status dikemaskini automatik. **PENDING master:** verify `atletik.celikguru.my` → admin → Templat Acara Standard → Balapan → 50m di atas.

### 🆕 Sesi 2026-07-01 (pagi–tengah hari): mypwa-v2 — Trend Markah + ETR ✅ LIVE PRODUCTION (main `f07d059`)
**Sambung dari spec 30 Jun.** Execute penuh guna subagent-driven-development (4 task + review setiap satu + final opus review).
- **Plan:** `docs/superpowers/plans/2026-07-01-trend-markah-etr.md` commit `49b8152`. Kod lengkap dalam plan (endpoint + trend.html + test).
- **Task 1 (backend, haiku):** endpoint `GET /api/ujian-markah/trend` — `3f66dd7`. Review sonnet ✅ Approved 0C/0I. Guna `ui.tahun=k.tahun` affinity, TOV/ETR/status on-the-fly.
- **Task 2 (trend.html, sonnet):** `6e37994`. Review sonnet Approved + 1 Important (logoUrl tak esc dlm cetak) + 1 Minor (is_td order path cetak) → FIX `a3d2b7a`.
- **Task 3 (routing app.js, haiku):** `2cc99c0` — sidebar link "Trend Markah" (guru grup 'ujian' + pelawat) + `/trend` dlm PELAWAT_PAGES. Controller self-review (3-baris mekanikal).
- **Task 4 (test, haiku):** `43e3c55` — `tests/trend.spec.js` smoke resilient (skip guard, assert errors===[]). Self-review.
- **Final review (opus):** WITH FIXES, 0 Critical/0 security. 1 IMPORTANT: `ujian_list` tak ditapis ikut `kelas.tahun` → kolum kosong utk ujian tahun lain (projek namakan ujian per-tahun → AKAN muncul). → FIX `c1cf928` (EXISTS filter `ui.tahun=kelas.tahun` + susun murid `localeCompare`).
- **Seal:** node --check ALL OK, wrangler dry-run CLEAN (192KiB), secrets clean.
- **Playwright:** ✅ 1 passed 9.2s lawan staging (sandbox-disabled izin master, TEST_USER=test/test123) — tak skip, 0 ralat JS, jadual trend terbentuk.
- **Deploy:** push test (`4b091c0..c1cf928`) → merge main `--no-ff` `f07d059` (6 fail, 1129+) → push main auto-deploy production. **0 migration.** Lucy balik branch test.
- **PENDING master:** verify production `erpm-sksalor.celikguru.my/trend` (visual: kolum betul ikut tahun, cetak landscape, PELAWAT reachability) + confirm GitHub Actions deploy hijau.
- **Backlog (review, belum buat):** normalisasi markah_penuh berbeza; ETR boleh-laras; carta garis per murid; eksport Excel.
- **Pengajaran:** report file subagent (haiku) kerap TAK di-overwrite (kekal basi dari feature lama) — controller WAJIB nilai diff sebenar via git, bukan percaya report. Betul semua kali ni (commit subjek + diff disahkan sendiri).

### 🆕 Sesi 2026-06-30 (tengah hari ~13:21–14:08): mypwa-v2 — Trend Markah + ETR ⏳ SPEC SIAP, BELUM EXECUTE
**Idea master:** feature trend markah merentasi beberapa ujian, **per subjek per individu murid**, + masukkan **ETR** (sasaran).
- **Keputusan brainstorm (master pilih):**
  1. **ETR dikira AUTO** (bukan input manual) = **TOV + 15, maks 100**. Tiada jadual DB baru, tiada migration — kira on-the-fly.
  2. **TOV = ujian PERTAMA** (kaedah headcount KPM standard, konsisten sepanjang tahun).
  3. **Urutan ujian ikut `created_at`** (KISS, tiada migration). Trend = semua ujian dalam sesi sama yang kelas terlibat.
  4. **Surface = page BARU** `public/trend.html`, **carian per KELAS** (bukan per murid satu-satu). Pengguna = **GURU + PELAWAT** (+ admin).
  5. **Susun atur = grouped per MURID** (collapse macam tab PAJSK `murid-group-header`): baris = SUBJEK, kolum = TOV–ETR + setiap ujian (markah+gred) + Status (✅ Capai / ❌ -jurang). Master tunjuk screenshot PAJSK + lakaran sebagai rujukan.
  6. **GURU nampak SEMUA subjek** (gaya kad laporan penuh, bukan tapis jadual_guru). ⚠️ Nota security: longgarkan akses GURU melebihi jadual_guru — master terima (markah bukan sulit antara staf, pelawat/dashboard pun dedah). Dokumen dalam spec.
- **Reka bentuk:** 1 endpoint baru `GET /api/ujian-markah/trend?nama_sesi=&kelas_id=` (authMiddleware, baca-sahaja). Guna semula pattern join `ui.tahun = k.tahun` + `appKiraGred()`. Frontend `trend.html` + sidebar link (guru+pelawat) + `/trend` dlm PELAWAT_PAGES. Cetak termasuk v1. Test `tests/trend.spec.js`. **0 migration, 0 package.**
- **Andaian (limitasi):** markah dianggap skala /100. Normalisasi markah_penuh berbeza = backlog.
- **Status data model disahkan:** `ujian`(created_at,nama_sesi), `ujian_item`(tahun INT,subjek_id), `ujian_markah`(markah,is_td,tarikh), `ujian_gred`(per ujian). `kelas`(tahun TEXT, tahun_sesi INT). SQLite numeric affinity buat `k.tahun`='5' match `ui.tahun`=5.
- **Spec:** `docs/superpowers/specs/2026-06-30-trend-markah-etr-design.md` commit `26fb6d4` (base `4b091c0`, branch test).
- **NEXT:** master review spec → writing-plans → execute (subagent-driven/inline). Branch test.

### 🆕 Sesi 2026-06-29 (malam ~00:45–04:57): mypwa-v2 — Cache Drill-down Gred (optimization) ✅ LIVE PRODUCTION (main `4b091c0`)
**Asal soalan master:** "bila klik gred, jika 100 murid, berapa kali request?" → Lucy semak kod `bukaSenaraiGred()` (dashboard.html:756). Jawapan: 1 request/klik (server pulang SEMUA murid sekali gus via `detail=1`, frontend `.filter()` ikut gred). Tiada N+1. Master minta optimize.
- **Masalah halus:** klik gred BERLAINAN pada carta SAMA fetch semula tiap kali walaupun `detail=1` dah pulang semua gred. 6 gred = 6 request data identik.
- **Fix (`public/dashboard.html` `bukaSenaraiGred`, +9/-2):** cache response pada `ctx._cache`. Klik 1 → fetch & simpan; klik gred lain carta sama → guna cached, 0 request. **Auto-invalidate** sebab `_gredCtx={}` reset dalam muatAnalisis/Semua/Trend (baris 497/547/637) — tiada data basi, tiada logik invalidation manual. KISS.
- **Refine CLEAR:** disahkan `ctxId` dikira sekali per carta (dashboard.html:727, luar `.map()`) → semua baris gred carta sama kongsi ctx object sama → kongsi cache. Carta lain (Keseluruhan/Tahun/Kelas/Trend) ctxId asing → cache asing, fetch sekali setiap satu.
- **Behavior-preserving:** klik pertama tetap 1 fetch macam dulu; cuma klik ke-2+ pada carta sama yang berubah. Risiko regresi rendah.
- **Seal:** Build CLEAN (wrangler dry-run, 19 files, D1 OK), Secrets CLEAN, Logic VERIFIED. Playwright Lucy tak run (sandbox tiada network) — feature asal dah disahkan master staging, perubahan ni frontend-only.
- **Commit:** `4b091c0` (base `ed1ea74`). Push `ed1ea74..4b091c0 test->test`.
- **✅ MERGE MAIN (master verify staging OK):** ff-only berjaya kali ni (test linear atas main — `ed1ea74`+`4b091c0` terus dari `ae609e2`). Merge `ae609e2..4b091c0 main->main`, push main → GitHub Actions auto-deploy production. Frontend-only, 0 migration. **Latest main = `4b091c0`.** Lucy balik branch test selepas merge. TIADA kerja tertunggak.

### 🆕 Sesi 2026-06-28 (petang ~13:12–14:05): mypwa-v2 — Drill-down Carta Gred ke Paparan GURU ⏳ PUSH TEST, PENDING TEST STAGING
**Asal soalan master:** "ingat tak kita buat boleh klik carta keluar nama murid?" → echo-recall jumpa feature drill-down dalam **pelawat.html** (dibuat 2026-06-26). Master minta **extend ke paparan guru**.
- **Apa:** port feature klik baris gred (count>0) → modal senarai murid (nama·kelas·markah) dari `pelawat.html` ke `dashboard.html` (tab "Ujian Dalaman"). Backend `/api/ujian-markah/analisis?...&detail=1` SEDIA ADA → **0 backend, 0 migration, 0 package, 1 fail + 1 test.**
- **Fail (`public/dashboard.html`):** +CSS `.gredRowKlik`/`.gred-modal-*`; +modal `#gredModal`; +`esc()`, `bukaSenaraiGred()`, `tutupGredModal()`, state `_gredCtx`/`_gredCtxSeq`; `buildChartHtml()` tambah param ke-4 `ctx`. **5 carta jadi clickable** (Keseluruhan, Tahun, Kelas, Semua Subjek, **Trend** — Trend bonus luar plan, master OK). Reset `_gredCtx={}` di `muatAnalisis`/`muatAnalisisSemua`/`muatTrend`. Salin tepat corak pelawat (KISS/DRY).
- **Test baru:** `tests/dashboard.spec.js` — login guru → tab Ujian Dalaman → pilih ujian+subjek → klik `.gredRowKlik` → sahkan `#gredModal` + table muncul → tutup. Mirror `pelawat.spec.js`.
- **Pipeline Kata:** brainstorm(skip, port jelas) → plan(lulus master) → Code → sight-hone CLEAR → commit-seal (Build dry-run CLEAN, secrets CLEAN, **Tests PENDING staging**) → auto-commit.
- **Commit test:** `ed1ea74` (base `ae609e2`). Push `ae609e2..ed1ea74 test->test`. **BELUM merge main.**
- **⚠️ NEXT (master jalankan sendiri — Pilihan A):** tunggu staging deploy (~1-2 min), run:
  `! cd C:/Users/user/Documents/code/mypwa-v2 && TEST_PASSWORD='test123' PELAWAT_PASSWORD='<pelawat>' npx playwright test tests/dashboard.spec.js tests/pelawat.spec.js`
  Perhati: test "Dashboard guru — klik gred..." mesti PASS; "Klik gred dalam Analisis" (pelawat) PASS (regresi). Skip "Tiada ujian/subjek" = data staging kosong, bukan bug. Kalau hijau → master confirm → merge main.

### 🆕 Sesi 2026-06-28 (tengah hari): mypwa-v2 — Nama Fail PDF Slip Individu Ikut Nama Murid ✅ LIVE PRODUCTION (main `ae609e2`)
**Masalah master:** bila cikgu download slip keputusan individu sebagai PDF, nama fail tak ikut nama murid.
- **Diagnosis:** slip individu BUKAN jana PDF sebenar — guna cetak browser (`window.print()`). Nama fail cadangan "Save as PDF" diambil dari `<title>` dokumen. Title lama (`wrapSlipHTML`) sama untuk semua murid (`Slip Keputusan — {nama ujian}`).
- **Fix (`public/laporan-ujian.html`):** tambah param ke-3 opsyen `tajukFail` pada `wrapSlipHTML(slips, ujian, tajukFail)` → guna sebagai `<title>` kalau ada, else default. `cetakSlipMurid()` hantar nama murid. `cetakSemuaSlip()` KEKAL default (banyak murid).
- **Format akhir (master revise 2x):** awalnya `Slip - {nama} ({kelas})` → master minta tukar prefix ke nama ujian → **`{nama ujian} - {nama murid} ({kelas})`** (cth "Peperiksaan Pertengahan Tahun - Ahmad Ali (5 DELIMA)").
- **Nota penting:** ini "Save as PDF" browser — bukan auto-download. Browser tetap papar dialog cetak; hanya nama fail CADANGAN yang berubah. Nak auto-download nama tepat perlu library jana PDF (jsPDF) = perubahan besar, TAK dibuat.
- **Commits test:** `745e9d2` (ikut nama murid, prefix "Slip") → `ae609e2` (prefix tukar ke nama ujian). Dua-dua MERGE MAIN ff-only (`745e9d2`, `ae609e2`). Push main auto-deploy production. 0 migration.
- **Nota git:** ff-only BERJAYA kali ni (test linear bersambung terus dari main `11c287c`). `git pull` sempat gagal sekali (network timeout intermittent) → retry sandbox biasa berjaya.

### 🆕 Sesi 2026-06-26 (malam ~19:00): mypwa-v2 — Label 'Tutup' + Progress Bar Ikut Kelas Sebenar ✅ LIVE PRODUCTION (main `f44cfd4`)
**Apa:** Dua perubahan pada paparan PELAWAT (sambung kerja drill-down petang tadi).
- **Perubahan 1 — Label butang:** card ujian tab Progress, butang toggle detail 'Sembunyi'→'Tutup' (`pelawat.html` togglePelawatDetail). Komit awal `be1d69d`.
- **Perubahan 2 — Progress bar kira kelas sebenar:** master report bar pening — kira ikut "item" (Tahun×Subjek) & 1 murid je dah dikira siap. Master pilih (via AskUserQuestion) kira ikut **kelas sebenar**, kelas lengkap = SEMUA subjek (item tahun itu) × SEMUA murid kelas tu bermarkah/TD.
  - **PENTING (pengajaran):** mula-mula tersilap kira **slot subjek×kelas** (DIAGNOSTIK=12, padahal kelas cuma 3, item cuma 4). Master perasan "12 tu bukan kelas". Verify data sebenar DB → betulkan ke kelas sebenar. **Bukti > teka.**
  - **SQL (`src/routes/ujian.js` query `/ujian`):** tambah `jumlah_kelas` (kelas ada murid yg tahunnya terlibat) + `kelas_lengkap` (`(bil_item_tahun × bil_murid) <= bil_markah_diisi`). Medan lama `item_diisi/jumlah_item` DIKEKALKAN (backward-compat).
  - **Frontend:** `pelawat.html` + `admin.html` guna `kelas_lengkap/jumlah_kelas`, label "X / Y kelas lengkap", header admin "Progress Subjek"→"Progress Kelas". Detail view TAK diusik (master kata dah betul).
- **Bonus fix test:** `pelawat.spec.js` semua gagal di login — bukan kod, app guna **clean URL `/pelawat`** tapi test tunggu `/pelawat.html`. Betulkan `waitForURL` ke regex `\/pelawat(\.html)?`. User pelawat staging = **PKP** (bukan 'pelawat'), creds via env var (lihat memory reference_mypwa_pelawat_test).
- **Verify:** SQL diuji terus DB staging (6 & 3 kelas) + production (12 & 3, DIAGNOSTIK 100%). Playwright pelawat 5 passed/1 skip (sandbox-disabled, izin master). node --check OK.
- **Commits test:** `be1d69d` (Tutup) → `298849a` (progress kelas + fix test) → `aa8b70b` (true-kelas SQL). **MERGE MAIN `f44cfd4`** (`305b3e9`..`f44cfd4`, --no-ff, 4 fail). Push main auto-deploy production. Tiada migration.

### 🆕 Sesi 2026-06-26 (petang/malam): mypwa-v2 PELAWAT — Drill-down Senarai Murid Mengikut Gred ✅ LIVE PRODUCTION
**Masalah master:** dalam tab Analisis pelawat, carta cuma papar BILANGAN murid per gred — tak tahu SIAPA murid gagal (E/F), kelas mana, markah berapa.
- **Penyelesaian:** setiap baris gred (count>0) boleh klik → popup modal papar nama·kelas·markah murid dalam gred+skop tu. Susun ikut kelas→markah rendah-tinggi (murid lemah/berisiko dulu). Semua gred A-F+TD, kedua-dua mod (satu-subjek carta Keseluruhan/Tahun/Kelas + "Semua Subjek" per kad subjek).
- **Pendekatan A (dipilih):** lanjut `/api/ujian-markah/analisis` dengan param opsyen `&detail=1` → respons tambah `data.murid: [{id,nama,nama_kelas,markah,is_td,gred}]`. Tanpa detail = respons BYTE-IDENTICAL (dashboard.html selamat). Lazy fetch bila gred diklik. Senarai modal guna query+param SAMA dgn carta → dijamin reconcile dgn kiraan (termasuk tahun_sesi). Tolak B (`/laporan` tak sokong tahun_sesi → mismatch) & C (route baru, langgar DRY).
- **Fail (3, tiada migration/route/package):** `src/routes/ujian-markah.js` (handler /analisis, +17 baris), `public/pelawat.html` (carta boleh klik + `_gredCtx` registry + modal + bukaSenaraiGred/tutupGredModal + helper esc()), `tests/pelawat.spec.js` (smoke test resilient, skip guard kalau data jarang).
- **Pipeline:** brainstorm→spec→plan→subagent-driven (Task1 backend haiku, Task2 frontend sonnet, Task3 test haiku). Setiap task lulus review spec+kualiti. Task2 Important (innerHTML modal tak escape) → FIX 812d9d3 helper esc() pada kod modal BARU sahaja (interpolasi unescaped sedia ada seluruh fail = luar skop, corak projek). Review whole-branch opus: ✅ READY MERGE, 0 Critical/0 Important, invarian parity+backward-compat dibukti.
- **Commits test:** 3443c0c (t1) → 1021de2 (t2) → 812d9d3 (fix) → ae2c426 (t3). Base b94960d.
- **Deploy:** SEAL (4/5, Playwright tertangguh — sandbox tiada network) → push test (Lucy guna sandbox-disabled, izin master) → master verify staging OK → **MERGE MAIN `305b3e9`** (af7d978..305b3e9, --no-ff) → push main auto-deploy production. Spec/plan: docs/superpowers/{specs,plans}/2026-06-26-pelawat-senarai-murid-gred*.
- **✅ MASTER SAHKAN PRODUCTION OK (2026-06-26 petang):** drill-down berfungsi live erpm-sksalor.celikguru.my. TIADA kerja tertunggak — feature SELESAI.

### 🆕 Sesi 2026-06-25/26 (malam): mypwa-v2 PELAWAT — 3 fix selepas master uji staging
**Konteks DRIFT (R1):** memo lama kata feature "KOD SIAP, BELUM PUSH" — sebenarnya DAH push ke test (`origin/test = b49ea74`, disahkan git fetch). Belum merge main je. Master uji staging → jumpa bug.
- **Fix #1 — Pelawat sangkut "Memuatkan data"** ✅ master sahkan jadi. Punca: `requireAuth` allowlist `PELAWAT_PAGES=['/pelawat.html','/profil.html']` guna `.html`, TAPI Cloudflare hidang **clean URL** `/pelawat` → `location.pathname` (`/pelawat`) tak match → requireAuth pulang null → `init()` henti baris pertama → sidebar+progress kosong. Fix `app.js`: normalize pathname `.replace(/\.html$/,'')` + `PELAWAT_PAGES=['/pelawat']`. (Page lain selamat — guna `normPath()`; cuma blok PELAWAT baru terlepas.) Commit `535df00`.
- **Fix #2 — Buang "Profil Saya"** (master: pelawat dikawal admin sepenuhnya). `app.js`: pelawatLinks tinggal Pemantauan Ujian; PELAWAT_PAGES=['/pelawat'] (buang /profil → pelawat tak boleh akses profil.html). Admin urus password via reset-password. Commit `f0ae5f4`. ⏳ verify.
- **Fix #3 — Skeleton loader tab Progress.** `pelawat.html`: CSS `.skel` shimmer + helper `skeletonProgress(3)`, papar 3 kad placeholder masa init sebelum fetch, diganti data bila loadProgress siap. Self-contained (tiada sentuh app.css). Commit `5f67b69`. ⏳ verify.
- **Diagnosis:** systematic-debugging — kumpul bukti (sidebar kosong+tab ada → _user null; URL /pelawat tanpa .html → punca). Soalan visual + 1-baris console pecahkan masalah, BUKAN navigate DevTools. → **R3 di-forge** (dahulukan bukti rendah-friction). Bash sandboxed (curl staging HTTP 000) — tak boleh verify sendiri, master hard-refresh staging.
- **Fix #4 — Tab Progress 2 kad sebaris** ✅ master sahkan jadi. `pelawat.html` `<style>`: `#progressList{grid-template-columns:1fr 1fr}` desktop, turun `1fr` pada ≤768px. Skeleton ikut grid sama. Commit `b22df5a`.
- **Fix #5 — Bump app.js v6→v7** (pelawat.html sahaja) — elak browser guna app.js?v=6 cached (lama, tiada fix clean-URL) di production. Query `?v=7` baru = pasti fetch segar. panduan.html kekal v6 (page guru, tak terkesan). Commit `a654107`.
- **Commits test malam ni:** b49ea74 → 535df00 → f0ae5f4 → 5f67b69 → b22df5a → a654107. **MERGE MAIN `74bfb3f`** (4fd9ada→74bfb3f, --no-ff, 14 commit, 10 fail +1544/-13). Push main → GitHub Actions auto-deploy production. Tiada migration.
- **⚠️ NEXT (master):** (1) tunggu Actions deploy siap (~1-2 min); (2) verify production `erpm-sksalor.celikguru.my` — login pelawat OK + sidebar 1 link + skeleton + 2 kad sebaris; (3) admin cipta akaun pelawat di UI production (role Pelawat) untuk GB/GPK/PPD/JPN. Production DB asing — akaun staging tak pindah.
- **⚠️ OUTSTANDING:** master verify #2 (sidebar tinggal 1 link) + #3 (skeleton shimmer) di staging (hard-refresh Ctrl+Shift+R sebab app.js?v=6 cached). Lepas semua OK → merge main (pertimbang bump ?v=7 supaya client dapat app.js baru). Pelawat seed akaun staging: SUDAH (master daftar sendiri via admin).

### 🆕 Sesi 2026-06-25 (tengah hari): mypwa-v2 — Paparan Pemantauan PELAWAT [⚠️ DRIFT: sebenarnya DAH push test b49ea74 — lihat entry malam 25/26 Jun di atas]
**Apa:** Peranan baru `PELAWAT` (baca-sahaja) + page baru `public/pelawat.html` (2 tab: Progress + Analisis) untuk Guru Besar/GPK + pemantau PPD/JPN tengok progress & analisis Ujian Dalaman tanpa boleh ubah data.
- **Brainstorm (master pilih):** akaun login khas (bukan link awam); skop = progress + analisis; papar nama murid OK (pegawai sah); SATU peranan PELAWAT; progress ada drill-down detail; Pendekatan A (page berasingan, page live tak diusik); security GET-endpoint lain = terima dulu (audiens dipercayai).
- **Backend (3 fail, tiada migration):** `src/middleware/auth.js` middleware baru `adminOrPelawat`; `src/routes/ujian.js` `progress-detail` longgar ke adminOrPelawat; `src/routes/pengguna.js` benarkan role PELAWAT. Semua mutation kekal adminOnly → PELAWAT terblock auto. `pengguna.role` TEXT bebas (tiada CHECK constraint).
- **Frontend:** `app.js` (homeFor + requireAuth allowlist `['/pelawat.html','/profil.html']` + renderSidebar pelawatLinks/label), `index.html` (login redirect 3-hala), `admin.html` (opsyen role Pelawat + badge biru). BARU `public/pelawat.html` (Progress: bar + drill-down; Analisis: carta gred Keseluruhan/Tahun/Kelas + cetak — TIADA Trend/Semua-Subjek, YAGNI).
- **Pipeline:** brainstorm→spec→plan(6 task)→subagent-driven. Spec `docs/superpowers/specs/2026-06-25-pelawat-pemantauan-ujian-design.md` (commit 05b87dd). Plan `docs/superpowers/plans/2026-06-25-pelawat-pemantauan-ujian.md` (commit 1542095).
- **Commits feature (branch test, BELUM push):** 5031ab3 (backend) → 316b44c (routing/sidebar) → 93ee736 (admin) → 85d6d1f (Progress) → a44b802 (Analisis) → 945716b (fix window.open guard) → b49ea74 (test pelawat.spec.js). Base 1542095.
- **Review:** setiap task lulus spec+quality review. Final whole-branch (opus) = ✅ READY MERGE, 0 Critical/0 Important. Security disahkan kukuh. Semua Minor diterima.
- **⚠️ OUTSTANDING (master/human):** (1) seed akaun `pelawat`/`pelawat123` DB staging (UI admin atau wrangler d1 execute); (2) run Playwright `tests/pelawat.spec.js` (perlu deploy staging + sandbox-disabled + izin master). Lepas tu: push test → master verify staging → merge main.
- **Backlog dicadang:** dokumen kewujudan PELAWAT + trade-off GET-endpoint dalam memory/CLAUDE.md (supaya kerja endpoint masa depan ingat PELAWAT boleh capai mana-mana GET authMiddleware-only).
- **Ledger:** `.superpowers/sdd/progress.md` (tracking penuh 6 task).

### 🆕 Sesi 2026-06-25 (pagi): mypwa-v2 — Buang Kedudukan dari Slip Cetak Individu ✅ LIVE PRODUCTION
**Apa:** Master minta buang **Kedudukan Kelas** + **Kedudukan Keseluruhan** dari slip cetak keputusan individu murid. Ranking KEKAL dipapar dalam table pivot tab laporan — cuma "hide" dari slip cetak sahaja.
- **Fail:** `public/laporan-ujian.html` — fungsi `buildSlipHTML()`. Buang 2 baris display (`<div>Kedudukan Kelas...`, `<div>Kedudukan Keseluruhan...`) dalam `.stats-section` (tinggal Purata Markah sahaja) + buang 2 pembolehubah yatim `rankKelasStr`/`rankKeselStr`. **1 fail, 4 baris dibuang, 0 backend, 0 migration.**
- **KEKAL tak diusik:** fungsi `kiraRankingSlip()` — sebab ia masih kira `purata`. Objek `rank` masih ada field rankKelas/rankKesel tapi tak dipakai display (KISS, kos buang tinggi). Table pivot tab laporan render di fungsi berasingan → ranking di skrin tak terjejas.
- **Pipeline (task kecil):** Code → refine (grep clean, tiada rujukan yatim) → commit-seal (wrangler dry-run BERSIH, 18 assets) → push test `08bfb08`.
- **Deploy:** master confirm staging OK → merge test→main `--no-ff` (`fe77e63..4fd9ada`) → push main → GitHub Actions auto-deploy production. Frontend sahaja.
- **Nota git:** ff-only gagal sebab main kumpul 22 merge commit "Merge branch 'test'" yang test tiada (test = branch linear, main = integrasi). Guna `--no-ff` ikut corak sejarah. Kandungan sebenar 1 fail/4 baris, merge auto tanpa konflik.

### 🆕 Sesi 2026-06-25 (malam): sistem-olahraga — Slider Penilaian Hakim ⏳ STAGING, PENDING VERIFY
**Apa:** Tukar input markah hakim (page penilaian) dari **5 butang toggle (1–5)** → **slider native 0–20** setiap kriteria (slide/drag, mesra-sentuh telefon). Markah penuh/kriteria 5 → 20 (jumlah max auto = bilangan kriteria × 20).
- **Keputusan brainstorm (master pilih):** (1) granularity = nombor bulat 0–20 (`step=1`); (2) makna 0 = belum nilai, gate butang Hantar KEKAL (semua kriteria mesti > 0); (3) Pendekatan A = `<input type="range">` native (bukan custom drag); (4) tambah validasi julat backend.
- **DRY (master minta):** konstan `MARKAH_PENUH = 20` dalam `hakim.js` = sumber tunggal frontend (skor max + slider markup). Backend `src/index.js` kekal literal `20` dgn komen cermin (tak kongsi merentas tier — kekal KISS).
- **Fail disentuh:** `public/hakim.js` (slider render + event + reset, −31 baris lebih ringkas), `public/hakim.html` (CSS slider, buang CSS radio lama), `src/index.js` (validasi `POST /api/hakim/markah`). **0 migration, 0 package, 0 ubah skema.** Backend simpan total sahaja (`markah_penilaian.markah`).
- **Backend validasi:** kira bilangan kriteria aktif dari DB → max = ×20; tolak markah bukan integer / null / <1 / > max / id_rumah kosong / pertandingan tiada kriteria. Server jadi sumber kebenaran julat (tutup jurang sedia ada).
- **Pipeline:** brainstorm → spec → plan (+revise DRY) → subagent-driven (3 implementer + final whole-branch review). Final review: SPEC ✅ KUALITI Approved, 2 Important (id_rumah + markah<1) dah fix.
- **Spec:** `docs/superpowers/specs/2026-06-25-slider-penilaian-hakim-design.md`. **Plan:** `docs/superpowers/plans/2026-06-25-slider-penilaian-hakim.md`.
- **Commits test (push):** `236abda` (backend) → `474ba4f` (slider js) → `4e66a32` (css) → `7e7c66a` (fix) → `be8ccce` (filled track warna). Base sebelum kerja = `657ff9b` (docs). Branch `test`, BELUM merge main.
- **🔨 FORGE (2026-06-25 pagi):** Rule **R2** ditambah dalam `~/.claude/mulahazah/rules.md` — guna `100dvh` (bukan `100vh`) untuk page full-height pada phone, susunan `height:100vh; 100dvh` (dvh menang). Origin: bug `100vh` muncul 2 kali (iPad sidebar mypwa-v2 + hakim.html butang Hantar). Diluluskan master manual. (rules.md = fail tempatan, bukan git, auto-load via mulahazah.)
- **🆕 Fix#2 (2026-06-25 pagi):** master report butang Hantar tersorok di phone (terlalu ke bawah). Punca: `hakim.html` baris 76 `#main-app style="height:100dvh; height:100vh"` — `100vh` (terakhir) MENANG → container lebih tinggi dari viewport nampak, footer `absolute bottom-0` tersorok di sebalik toolbar browser. Fix: tukar susunan → `100vh; 100dvh` (100dvh menang, fallback 100vh). 1 baris, `public/hakim.html`. Sama pattern iPad bug memory (100vh≠visible). Push test `19ce4c7`. ⏳ master verify phone.
- **🆕 Fix#1 (2026-06-25 pagi):** master report slider tak warnakan ruang 0→nilai. Punca: WebKit (Chrome/Safari/mobile) tiada filled track native (`::-moz-range-progress` Firefox-only). Fix: helper `warnaSlider()` dalam `hakim.js` set `linear-gradient` inline ikut `value/MARKAH_PENUH`, panggil di 4 tempat (render, input, reset-belum-pilih-rumah, resetBorang). 1 fail (`public/hakim.js`), 0 CSS. SEALED (wrangler dry-run CLEAN, node --check OK, Playwright skip=visual) → push test `be8ccce`. ⏳ master verify staging.
- **Staging:** `https://sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev` (login hakim → page penilaian).
- **⚠️ NEXT (esok 2026-06-25):** master test sendiri di staging (slider + hantar markah). Kalau OK → master bagi isyarat → Lucy merge test→main (auto-deploy production `atletik.celikguru.my`). Playwright TAK dijalankan (perlu sandbox-disabled + izin master).

### 🏁 Ringkasan Akhir Sesi (2026-06-24 pagi)
Feature **Cetak Markah dari Page Keputusan** (mypwa-v2) — siap, dipoles, teruji, live production.
- **Apa:** butang Cetak dalam `ujian.html` → helaian A4 markah subjek (header sekolah + nama ujian/kelas/tahun, jadual Bil·Nama·Markah·Gred + ringkasan gred + purata/min/max). Boleh cetak ujian yang dah ditutup (mod baca-sahaja).
- **Kos:** 1 fail frontend (`public/ujian.html`) + 1 test. **0 backend, 0 migration, 0 page baru.**
- **Pengesahan:** self-review → wrangler dry-run → Playwright E2E suite **10/10 PASS** (mod sandbox-disabled, izin master) → master confirm browser + phone.
- **Commits main:** `d5c0dd9` (feature + buang Markah Penuh) → `cdd4061` (UI polish: butang seragam navy + buang garis kelabu mobile).
- **Memory baru:** `feedback_sandbox_mode.md` (mod khas guna izin dulu). Diary: 3 entri dalam `daily-diary/current/2026-06-24.md`.
- **Status:** TIADA kerja tertunggak untuk feature ni. Backlog "Compact mode Ujian Dalaman" ✅ DISELESAIKAN (cara lain — preset zoom, bukan compact mode).

**Tambahan (~11:31):** Backlog "Compact mode Ujian Dalaman" diselesaikan — bukan dengan compact mode, tapi **preset `zoom:0.88` + max 6 kad/halaman** dalam cetak analisis dashboard (`cetakSekyen` + `cetakAnalisis3`, helper `binaKadGrid`). Master test print preview OK → merge main `519bf0a`. LIVE PRODUCTION.

**Tambahan (~11:40):** Cetak analisis dashboard — border card + label `.sub` + tagline `.hdr p` kelabu pucat (#e2e8f0/#64748b) → hitam `#1e293b` (seragam dgn nama sekolah/garis header). Master confirm print preview OK → merge main `fe77e63`. LIVE PRODUCTION. **Latest main = `fe77e63`.**

**💡 Backlog dicadang Lucy (belum buat):** satu pass semakan page cetak lain (laporan-ujian slip, pajsk, laporan RPM) untuk teks/border pucat serupa — master belum putuskan.

---

### 🆕 PROJEK BARU (2026-06-24 petang): Chrome Extension — Import PAJSK → IDMe KPM ⏳ BRAINSTORM (belum execute)

**Idea master:** Chrome extension untuk cikgu pindah data dari Tab PAJSK (eNilai/mypwa-v2) ke laman KPM `https://idme.moe.gov.my/login`. Alat produktiviti sah — cikgu pindah data sendiri (yg wajib hantar KPM) lebih pantas, akaun + data sendiri.

**Keputusan brainstorm setakat ni (via soalan):**
- **Q1 IDMe:** taip satu-satu (borang per murid/aktiviti), TIADA upload pukal → extension auto-isi justified.
- **Q2 Cara isi:** SEPARA-AUTO — extension isi semua medan, CIKGU tekan Hantar sendiri (selamat + lembut ToS).
- **Q3 Sumber data:** baca tab eNilai yg terbuka (tiada backend/token/CORS, guna sesi login sedia ada).
- **Q4 Interaksi:** cikgu KLIK rekod dari panel extension → isi borang IDMe yg terbuka (bukan auto-padan murid).

**Pendekatan dicadang (tunggu master pilih):**
- **A ⭐ (Lucy syor):** MV3 Side Panel + eNilai PAJSK page tambah blok JSON tersembunyi (frontend sahaja) utk extension baca data bersih. 2 content script (baca eNilai + isi IDMe) + service worker. Kebal, kemas.
- **B:** Panel terapung + korek (scrape) table eNilai. Tak sentuh eNilai tapi rapuh.

**⚠️ Kebergantungan utama:** bahagian isi borang IDMe perlu PETA MEDAN (field mapping) — master kena bagi struktur borang IDMe (screenshot + inspect element), sebab Lucy tak boleh log masuk laman KPM. Bina berperingkat (config boleh-ubah).

**✅ BRAINSTORM + SPEC + PLAN SELESAI (2026-06-24 petang):** Master pilih Pendekatan A + edaran 2 fasa (unpacked → Web Store Unlisted). Repo baru `C:\Users\user\Documents\code\idme-pajsk-ext` (git init, branch master). Spec commit `ca3abcc`, Plan commit `890821f`.
- **Spec:** `idme-pajsk-ext/docs/superpowers/specs/2026-06-24-extension-pajsk-idme-design.md`
- **Plan:** `idme-pajsk-ext/docs/superpowers/plans/2026-06-24-extension-pajsk-idme.md` — 8 task TDD.
- **Bentuk data eNilai disahkan** (pajsk.html renderLaporan, `_lLastData.list`): rekod ada `nama_murid, nama_kelas, kategori, nama_aktiviti, peringkat, pencapaian, catatan, drive_link, nama_sesi`. Blok JSON `#pajsk-export` (Task 3) dalam mypwa-v2.
- **Urutan:** Task 1,2,5 boleh mula serta-merta (tulen, tiada external); 3-4 perlu eNilai; 6 perlu 5 (uji lawan mock-idme.html); **Task 7 GATED** — master kena inspect borang IDMe sebenar bekal selector. Task 8 sedia Web Store.
- **Ujian:** Node built-in `node --test` (TIADA npm package — hormat pantang). Fail `lib/*.js` dual-mode (content script + require).
- **NEXT:** master pilih cara execute (subagent-driven / inline / work-plan). Boleh siapkan Task 1-6 tanpa tunggu IDMe.

## 💭 Working Memory (RAM)

### Session Recap (For AI Restart)

- **Sesi 2026-06-24 pagi: mypwa-v2 — Cetak Markah dari Page Keputusan (REVISI penempatan)** ✅ SELESAI & LIVE PRODUCTION (main `cdd4061`), teruji E2E 10/10, master confirm browser + phone

  Master revise plan "Cetak Markah Subjek" semalam. **Tukar penempatan:** buang idea page baru `markah-subjek.html` → letak butang **Cetak terus dalam `ujian.html` (page Keputusan)**.

  - **Keputusan master (via revise):** (1) butang Cetak dalam ujian.html, bukan page baru; (2) skop = satu kelas+subjek dipapar (buang "Semua Kelas Saya"); (3) KENA boleh cetak ujian DITUTUP; (4) kandungan = jadual + ringkasan gred + purata/min/max; (5) header = logo+nama sekolah, nama ujian, kelas, tahun.
  - **Penemuan teknikal:** `GET /ujian` (tanpa `?input=1`) DAH pulangkan semua ujian utk guru termasuk tutup (`src/routes/ujian.js:12-13` — where kosong). Jadi cuma tukar `/ujian?input=1`→`/ujian`. **0 baris backend, tiada migration, tiada page baru.**
  - **Fail disentuh:** `public/ujian.html` (6 suntingan: endpoint, ujianMap, detect tutup, mod baca-sahaja `terapkanModTutup()`, butang #btnCetak, `cetakMarkah()`+`kiraGredCounts()`). BARU `tests/cetak-markah.spec.js`. Spec+plan dikemas (revisi). `.gitignore` tambah `.superpowers/`.
  - **Mod baca-sahaja:** ujian tutup → input markah disable + Simpan disorok + nota 🔒, Cetak kekal aktif (markah tersimpan tetap boleh cetak). Elak guru Simpan ke ujian tutup (backend tolak 403).
  - **Ringkasan gred + purata dikira CLIENT-SIDE** dari markahMap/tdMap guna appKiraGred() — tiada panggil /analisis (DRY, elak ownership-gate).
  - **Pipeline:** Code → sight-hone(self) CLEAR → wrangler dry-run BERSIH → commit-seal → push test. Commit `0bcf380` (test). Push a7b6950→0bcf380.
  - **⚠️ TAK DAPAT run Playwright:** sandbox Bash tiada network keluar (curl github.com pun code=000). Verify E2E TERTANGGUH — master kena uji di browser.
  - **✅ MASTER VERIFY STAGING OK → MERGE MAIN:** master confirm staging OK (+ minta buang 'Markah Penuh: 100' dari baris sub cetak, commit `164dda3`). Merge test→main `d5c0dd9` (8a4cc86→d5c0dd9), push main → GitHub Actions auto-deploy production (frontend sahaja, tiada migration). Commits feature: 0bcf380 (cetak) + 164dda3 (buang markah penuh).
  - **✅ MASTER CONFIRM PRODUCTION OK (2026-06-24 pagi):** ujian.html → Cetak berfungsi live di production. Diary disimpan (daily-diary/current/2026-06-24.md). E2E Playwright suite penuh 10/10 PASS (guna mod sandbox-disabled dgn izin master — lihat feedback_sandbox_mode.md).
  - **✅ UI POLISH (2026-06-24 ~10:19):** master report 2 isu dari phone — (1) butang Cetak vs Simpan tak konsisten saiz/warna; (2) garis kelabu td tak align dalam kad mobile. Fix: Cetak btn-ghost→btn-primary (master pilih dua-dua navy solid saiz sama); mobile media query `#muridTable td{border-top:none}` (punca = global `td{border-top}` app.css:209 bocor ke layout flex card). Commit test `2ad3414` → master confirm phone OK → merge main `cdd4061`. LIVE PRODUCTION. FEATURE + POLISH SELESAI.

- **Sesi 2026-06-23 petang/malam: mypwa-v2 — Cetak Markah Subjek Saya (SPEC + PLAN SIAP, BELUM execute)** ⏳ NEXT: execute esok

  Seorang cikgu minta boleh cetak markah Ujian Dalaman yang direkod — **tapi subjek dia sahaja**. Page sedia ada `laporan-ujian.html` papar pivot SEMUA subjek, jadi cikgu tak boleh cetak helaian subjek dia seorang.

  - **Keputusan reka bentuk (master pilih via brainstorm):**
    1. Jenis = **Markah Ujian Dalaman** (markah nombor + gred), BUKAN RPM/TP.
    2. Skop = boleh pilih **satu kelas** ATAU **semua kelas cikgu ajar**.
    3. Isi helaian = **Bil · Nama · Markah · Gred** + **ringkasan gred** (bilangan per gred + purata/min/max). TIADA ranking, TIADA ruang tandatangan.
    4. Penempatan = **page baru khas** `public/markah-subjek.html` (bukan ubah laporan-ujian.html).
    5. "Semua Kelas Saya" = **page-break per kelas** (setiap kelas helaian berasingan + ringkasan sendiri, Bil mula semula 1).
  - **Penemuan teknikal penting:** data DAH WUJUD lewat endpoint sedia ada — **tiada backend baru, tiada migration**:
    - `/api/ujian-markah/jadual?ujian_id=` → kelas+subjek cikgu (kunci skop "subjek dia", tapis ikut jadual_guru)
    - `/api/ujian-markah/murid?ujian_item_id=&kelas_id=` → murid+markah+is_td
    - `/api/ujian-markah/analisis?ujian_id=&subjek_id=&kelas_id=` → counts per gred + gredScale + markah_penuh
    - `appKiraGred()` (app.js shared) untuk gred; `/api/tetapan` untuk header cetak
    - `GET /api/ujian` (tanpa `?input=1`) → guru nampak SEMUA ujian termasuk yang DITUTUP (boleh cetak markah lepas peperiksaan tamat)
  - **Fail disentuh:** BARU `public/markah-subjek.html` + BARU `tests/markah-subjek.spec.js` + MODIFY `public/app.js` (1 link dalam guruLinks grup 'ujian').
  - **Spec:** `docs/superpowers/specs/2026-06-23-markah-subjek-saya-design.md` (commit f6e6127)
  - **Plan:** `docs/superpowers/plans/2026-06-23-markah-subjek-saya.md` (commit 95c48af) — 5 task bite-sized, kod konkrit penuh.
  - **NEXT (ESOK 2026-06-24):** master pilih cara execute — **subagent-driven disyorkan** atau inline. Branch `test`.
  - **Nota test:** page guru-only → test MESTI login GURU `TEST_USER=test TEST_PASSWORD=test123`. Bahagian render markah data-dependent → smoke test direka resilient (assert struktur + tiada JS error). Page perlu sampai staging dulu sebelum Playwright lawan staging sah (push test auto-deploy, atau guna `wrangler dev`).
  - **Nota security (highlighted):** `/murid` & `/analisis` tiada ownership-gate (pre-existing, sama macam laporan-ujian.html). Page baru lebih ketat di UI. TAK diubah dalam kerja ni.
  - **Commits test setakat ni (spec+plan sahaja, BELUM push):** 50441b7 (spec) → f6e6127 (spec perjelas) → 95c48af (plan). Kod BELUM ditulis.

- **Sesi 2026-06-22 malam: Audit Memory Drift + Forge Rule R1 + Penjelasan erpm-v2** ✅ SELESAI (housekeeping, bukan kerja kod)

  Master perasan session brief tunjuk status basi ("audit log belum execute" sedangkan dah live). Siasat → jumpa baris index MEMORY.md DRIFT dari realiti (kerja siap & merge main tanpa memo dikemaskini).

  - **4 entry basi dibetulkan (semua disahkan via git):** (1) mypwa-v2 Audit Log Fasa 9 ✅ live main; (2) sistem-olahraga Arkib Tahunan ✅ live main (7 fasa, 76 is_arkib + 6 endpoint); (3) ADNI ✅ deploy main (main==test==db35628); (4) eRPM v2 — dijelaskan keliru folder.
  - **Disahkan TEPAT (tak diusik):** BrightMe, celikguru vision, sistem asas olahraga.
  - **Rule R1 di-forge** dalam `~/.claude/mulahazah/rules.md` (folder dicipta baru): verify status "pending/belum execute" dengan git SEBELUM percaya/papar dalam brief. Betulkan memo serta-merta bila jumpa drift.
  - **erpm-v2 vs mypwa-v2 dijelaskan dalam memory:** mypwa-v2 = produk khas per-SEKOLAH (DIKEKALKAN, live, erpm-sksalor.celikguru.my). erpm-v2 = SaaS multi-tenant per-GURU (MASTER+GURU, self-register) — BELUM MULA (folder scaffolding sahaja, 1 commit, 0 fail kod, idle sejak 14 Apr). Plan: memory/projects/erpm-v2-plan.md.
  - **⚠️ Security nota erpm-v2:** plan guna SHA-256 kosong — regresi vs PBKDF2 100k+salt projek lain. Bila execute, ikut PBKDF2.
  - **Pengajaran teras:** memory ≠ realiti. Snapshot masa-lampau; status kerja berubah, memo tak auto-update.

### Session Recap (Lama)

- **Sesi 2026-06-22 tengah hari: mypwa-v2 — Drag Susun Kedudukan Dokumen** ✅ SEALED, PUSHED test, staging live. PENDING master verify → merge main.

  Master nak admin boleh drag-and-drop susun kedudukan dokumen dalam Tab Dokumen (admin sahaja); susunan terpakai untuk paparan guru. Pipeline Kata penuh (brainstorm→spec→plan→subagent-driven→sight-hone/review→commit-seal→push). KISS & DRY.

  - **Konteks penting:** "Tab Dokumen" = table `panduan` (id, nama, url, created_at). Admin urus di `admin.html` tab "Dokumen", guru tengok di `panduan.html`. Fail/route kekal nama `panduan` (bukan `dokumen`).
  - **Keputusan reka bentuk (master pilih):** (1) buang pagination di admin (senarai penuh, drag bebas), (2) drag-and-drop native HTML5 (desktop), (3) guru view kekal pagination — auto ikut susunan baru. Tiada audit log untuk reorder.
  - **Migration 025** (`025_panduan_urutan.sql`): `ALTER TABLE panduan ADD COLUMN urutan INTEGER` + backfill correlated-COUNT ikut `nama ASC` (preserve paparan). ⚠️ Framework `migrations apply` GAGAL pada staging (024 tak tracked, duplicate column tarikh_tutup) → apply terus guna `d1 execute --file`. Sama isu akan berlaku utk production — apply 025 guna `d1 execute` masa merge main.
  - **Backend** (`src/routes/panduan.js`): GET `ORDER BY urutan ASC, id ASC` + sokong `?all=1` (admin-gated, semua rekod tanpa LIMIT); POST set `urutan = MAX+1`; endpoint baru `PUT /api/panduan/urutan` (adminOnly, body `{ids:[]}`, validate Number.isInteger, `db.batch()`) — WAJIB daftar SEBELUM `PUT /:id`.
  - **Frontend admin** (`public/admin.html`): buang pagination (#panduanAdminPg, _panduanPage), `loadPanduanAdmin()` no-arg guna `?all=1`, baris `draggable` + handle ≡, fungsi `initPanduanDrag/renumberPanduan/saveUrutanPanduan`. Guru `panduan.html` tiada perubahan.
  - **Review (Opus) finding Important:** `?all=1` asalnya tak admin-gated → Lucy tambah guard `c.get('user').role !== 'ADMIN' → 403` (commit f9771e4). Minor (drag-luar-table tak persist, test smoke-only, whole-row draggable) — deferred, acceptable.
  - **Test:** `tests/dokumen.spec.js` baru (assert draggable + handle ≡ + #panduanAdminPg count 0). Suite penuh 9/9 PASS lawan staging (a11y flake ERR_NETWORK_CHANGED sekali, re-run hijau).
  - **Credentials staging admin:** `admin` / `fcoy4994` (sama dengan production).
  - **Spec:** `docs/superpowers/specs/2026-06-22-drag-susun-dokumen-design.md`. **Plan:** `docs/superpowers/plans/2026-06-22-drag-susun-dokumen.md`.
  - **Commits test:** `6ad926e`→`f9771e4`. Merge main = `a953d20` (feature) → `8a4cc86` (ci fix).
  - **✅ DEPLOY PRODUCTION (master confirm browser):** migration 025 applied prod DB (`d1 execute`, 9 dokumen backfill 1-9). Merge main + deploy production manual (wrangler, Version b5a8e426). Feature DISAHKAN LIVE oleh master dalam browser (erpm-sksalor.celikguru.my). ⚠️ Mesin master tak boleh curl/Playwright ke worker semasa sesi (HTTP 000/SSL err — rangkaian local flapping); verify via wrangler API + browser master sahaja.
  - **✅ CI FIX (penting — pengajaran):** GitHub Actions auto-deploy PRODUCTION gagal (staging OK). Punca: (1) secret `CLOUDFLARE_API_TOKEN` lama kurang permission **Zone: Workers Routes Edit + Zone Read** untuk `celikguru.my` — production ada custom route `erpm-sksalor.celikguru.my/*`, staging tiada route jadi tak terjejas; (2) `cloudflare/wrangler-action@v3` telan output error. **Fix:** master jana token baru guna template "Edit Cloudflare Workers" + include zone celikguru.my; tukar step production `deploy.yml` dari wrangler-action → `run: npx wrangler deploy --env production` (env CLOUDFLARE_API_TOKEN+ACCOUNT_ID) supaya error nampak penuh. Hasil: run main `8a4cc86` HIJAU. Push main akan datang auto-deploy production OK.

- **Sesi 2026-06-21 petang: sistem-olahraga — EXECUTE Reset Password Sekolah** ✅ KOD SIAP, SEALED, DEPLOY STAGING(-test). PENDING master verify manual → merge main

  Master pilih execute plan reset password sub_admin. Semua 5 task siap:
  - **Backend** (`src/index.js` selepas line 2952): `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password`. Guard 3 lapis (id_pengguna+id_sekolah+peranan='sub_admin') dalam SELECT & UPDATE; validasi min 6 aksara (FE+BE); hash PBKDF2; GET tak dedah hash. Commit `1ccbe72`.
  - **Frontend**: seksyen "Reset Password Sub Admin" dalam modal Urus (`superadmin.html`) + logik `muatSubAdmin`/handler reset+toggle 👁 (`superadmin.js`). ID sebenar: `#urus-pilih-subadmin`, `#urus-password-baharu`, `#btn-toggle-password`, `#btn-reset-password-subadmin`. Commit `3ae75be`.
  - **Ujian**: test 16 ditambah dalam `tests/e2e.spec.js` (gitignored — lokal sahaja, TAK masuk repo). Guna `16` sebab `07` dah dipakai. Assertion utama = kewujudan elemen (deterministik).
  - **Pipeline**: sight-hone CLEAR → commit-seal SEALED (17 ujian sedia ada PASS, wrangler dry-run clean) → push test `3ae75be`.
  - **⚠️ PENEMUAN ENV (penting):** CI deploy push `test` → worker **`sistem-olahraga-sekolah-test`** (db `olahraga-test`). Kod baru DAH live di situ (marker disahkan). TAPI `BASE_URL` ujian + login `dragon`/`f4994` hanya jalan di worker **top-level** `sistem-olahraga-sekolah` (= production, db `olahraga-db`, domain `atletik.celikguru.my`). Db `olahraga-test` nampaknya tiada seed superadmin → ujian automatik & verify tak boleh jalan di `-test`. Ujian 16 'gagal' bukan sebab bug — sebab poll worker salah.
  - **Staging -test URL:** https://sistem-olahraga-sekolah-test.syazwan-skpp82.workers.dev
  - **✅ SEED + VERIFY (2026-06-21 petang lewat):** db `olahraga-test` tiada superadmin (sa_cred=0) → Lucy seed `superadmin_credentials` (id=1, username='dragon', password=hash PBKDF2 'f4994') via `wrangler d1 execute olahraga-test --remote`. Hash dijana guna Node webcrypto (params sama: salt 16B, 100000 iter, SHA-256, format `pbkdf2:salt:hash`). Login `-test` kini BERJAYA (peranan super_admin).
  - **VERIFIKASI PENUH lawan -test:** (1) Test 16 Playwright PASS. (2) E2E API: GET sub-admin→reset→login password baru `success=True peranan=sub_admin` (sekolah aktif NBA3003). (3) Guard 404 disahkan utk id bukan sub_admin. Feature FUNCTIONAL end-to-end.
  - **⚠️ DATA TEST DIUBAH semasa verify:** password 2 sub_admin di olahraga-test kini = `ujian123` → `nba3003` (NBA3003, aktif) & `xba3202` (XBA3203, tak aktif). Master boleh reset balik kalau perlu.
  - **Cadangan Lucy:** BASE_URL ujian patut tuding ke worker `-test` (bukan top-level=production) untuk CI hygiene. Sekarang dikekalkan asal (top-level) — keputusan master. olahraga-test = 7 sekolah, 7 sub_admin.
  - **✅ MERGE MAIN (master confirm):** merge test→main `8c5296c` (4 commit: spec, plan, backend, frontend). Push main → GitHub Actions deploy production. DISAHKAN LIVE: marker `urus-pilih-subadmin` ada di workers.dev + `atletik.celikguru.my`; endpoint GET+PATCH pulang 401 (terdaftar, perlu auth). FEATURE LIVE PRODUCTION.
  - **Main commit baru:** `8c5296c` (lama: `1be6002`).
  - **✅ SECURITY REVIEW + FIX (2026-06-21 petang, main `2ae8785`):** master minta semak credential hardcoded terdedah di frontend.
    - Imbasan `public/`: TIADA API key/JWT/token pihak ketiga. Jumpa 1 isu: `superadmin.html:144` ada `value="123456"` (default password lemah untuk daftar sub_admin, nampak dalam page source).
    - **Fix opsyen 1:** buang `value="123456"` + tambah `required minlength="6"` + placeholder; label "Kata Laluan Lalai"→"Awal" (commit `d295d6e`). Backend `POST /api/superadmin/admin` tambah validasi `kata_laluan.length<6 → 400` (commit `4ed2d97`) — sepadan endpoint reset.
    - **JWT_SECRET semakan:** DISET betul di production DAN env test (wrangler secret list) — bersama SUPERADMIN_USERNAME/PASSWORD. Fallback 'sila-tukar-secret-ini' tak pernah dipakai. SELAMAT.
    - **Disahkan LIVE production:** frontend 123456 hilang + minlength ada; backend password 5-aksara → HTTP 400. Merge main `2ae8785`.
  - **✅ FOLLOW-UP (2026-06-21 petang): 2 housekeeping selesai:**
    1. Password `nba3003` + `xba3202` (olahraga-test) di-standard ke `1234` (asal hashed, tak boleh pulih — guna konvensyen test projek). Login nba3003/1234 disahkan berjaya.
    2. `BASE_URL` ujian tukar ke worker `-test` (`sistem-olahraga-sekolah-test...`) untuk CI hygiene — fail `tests/e2e.spec.js` gitignored (lokal sahaja). Suite penuh 18/18 PASS lawan -test → admin_dba1097/1234, gb/1234 semua wujud di olahraga-test. -test kini staging berfungsi.

- **Sesi 2026-06-21 petang (awal): sistem-olahraga — Plan Reset Password Sekolah** ✅ SPEC + PLAN SIAP (kini DAH EXECUTE, lihat atas)

  Master perasan superadmin TAK BOLEH reset password akaun sekolah (sub_admin) bila admin sekolah lupa password. Gap sebenar SaaS multi-tenant — satu-satunya jalan sekarang = padam sekolah (hilang data) atau edit SQL manual.

  - **Keputusan reka bentuk (master pilih):** (1) superadmin taip password baru sendiri, (2) sub_admin sahaja, (3) dropdown senarai sub_admin (perlu endpoint GET).
  - **Design:** 2 endpoint baru dalam `src/index.js` — `GET /api/superadmin/sekolah/sub-admin` + `PATCH /api/superadmin/admin/reset-password` (guard 3 lapis: id_pengguna + id_sekolah + peranan='sub_admin'; hash PBKDF2; validasi min 6 aksara). UI dalam modal "Urus Sekolah" sedia ada (dropdown + password + toggle 👁).
  - **Prinsip:** KISS & DRY — guna corak sedia ada (`apiFetch`, `hashPassword`, `serverError`, `selectedTenantId`). Tiada migration/package baru.
  - **Spec:** `docs/superpowers/specs/2026-06-21-reset-password-sekolah-design.md` (commit `fa51bd3`)
  - **Plan:** `docs/superpowers/plans/2026-06-21-reset-password-sekolah.md` (commit `dfa545f`) — 5 task bite-sized.
  - **NEXT:** master pilih cara execute (subagent-driven disyorkan / inline). Branch `test`.
  - **Nota repo:** ujian E2E lawan staging (BASE_URL=staging worker, superadmin `dragon`/`f4994`), bukan localhost. Superadmin boleh ada >1 sub_admin per sekolah.

- **Sesi 2026-06-21 tengah hari: Masa Log Audit → format 24-jam** ✅ LIVE PRODUCTION (merge main `0ff490a`)

  Sambungan dari fix timezone semalam. Master nak masa dipapar dalam sistem 24-jam (bukan PG/PTG).

  - **Fix (`admin.html:1484`):** tambah `hour12:false` dalam options `toLocaleString`. Display-only — logik penukaran UTC→MYT (`replace(' ','T') + 'Z'`) kekal tak berubah.
  - **Kesan:** `11:41 PG` → `11:41` · `4:45 PTG` → `16:45` · tengah malam → `00:xx`.
  - **Pengajaran:** `toLocaleString('ms-MY', ...)` default 12-jam (PG/PTG). Untuk 24-jam guna `hour12:false`. Format paparan terasing dari logik pengiraan masa.
  - Pipeline: Code → Playwright 8/8 PASS → commit-seal (wrangler dry-run CLEAN) → push test `e452827` → master confirm staging → merge main `0ff490a`.
  - **Backlog dicadang:** audit tempat lain yang papar `created_at` — mungkin ada bug timezone/format serupa (BELUM buat).

- **Sesi 2026-06-21 malam: Fix Masa Log Audit → MYT** ✅ LIVE PRODUCTION (merge main `230dfb0`)

  Masalah: master perasan masa log masuk dalam Log Audit (admin.html) salah — tersasar 8 jam.

  - **Punca:** SQLite `CURRENT_TIMESTAMP` simpan UTC format `"YYYY-MM-DD HH:MM:SS"` (guna space, tiada `Z`/penanda zon). String non-ISO macam ni ditafsir `new Date()` sebagai waktu TEMPATAN, bukan UTC. Jadi walaupun `toLocaleString` dah ada `timeZone:'Asia/Kuala_Lumpur'`, input ke `new Date()` dah salah sebelum penukaran zon — hasilnya papar nilai UTC mentah.
  - **Fix (`admin.html:1484`):** `new Date(row.created_at.replace(' ', 'T') + 'Z')` — normalize ke ISO UTC dulu (`"2026-06-20T16:22:00Z"`), baru `toLocaleString` tukar +8 ke MYT betul.
  - **Bukti:** 16:22 UTC → sebelum fix papar 04:22 PTG (salah), selepas fix papar 12:22 PG = 00:22 MYT (betul).
  - **Display-only:** DB tak diubah, data audit lama terus betul, tiada migration.
  - **Pengajaran:** `new Date()` pada string SQLite UTC (space-separated, tiada Z) = ditafsir local time. Sentiasa normalize ke ISO+`Z` sebelum tukar zon. Pattern ni mungkin ada di tempat lain yang papar `created_at` — patut audit masa depan.
  - Commits: `acbb3ac` (test) → merge main `230dfb0`. Playwright 8/8 PASS, wrangler dry-run CLEAN.

### Session Recap (Lama)

- **Sesi 2026-06-14/15: OMR Scanner — 3 Bug Fixes** ⏳ TEST BRANCH (belum merge main)

  Konteks: master test OMR scanner, jumpa 2 isu berturutan. Debug guna data sebenar (Playwright + foto master), bukan teka.

  1. **"Anchor tidak ditemui"** (commit `e67b8c1`):
     - Punca: `detectAnchors` lama cari anchor dalam kuadran TETAP di penjuru gambar. Gagal bila borang kecil di tengah frame (anchor tak sampai penjuru).
     - Fix: auto-detect — Otsu adaptive threshold + connected-components (DFS flood fill) + tapis bentuk kotak pejal + pilih 4 penjuru + validasi segi empat.
     - Terbukti kesan 4 anchor tepat pada foto sebenar (19ms).

  2. **Markah tak tepat (baca kosong)** (commit `92f6344`):
     - Punca: threshold MUTLAK `< 110` terlalu ketat. Murid guna PENSEL (grafit kelabu, brightness ~101-132) → hampir semua bubble dibaca kosong. Brightness per soalan terbukti BETUL (geometri OK), cuma ambang salah.
     - Fix: `detectBubbles` tukar ke KONTRAS RELATIF — banding bubble paling gelap vs purata 3 lain dalam soalan sama (margin >= 18). Auto-laras ikut kertas/cahaya. Terbukti baca 10/10 betul.

  3. **Frame terpotong → hasil salah senyap** (commit `92f6344`):
     - Punca: bila anchor atas keluar frame, algoritma pilih dot bubble sebagai anchor palsu → mapping rosak → markah salah tanpa amaran (bahaya untuk guru).
     - Fix: guard saiz seragam dalam `detectAnchors` — tolak kalau 4 anchor ratio saiz > 3.5 (_found = -2). Mesej baru: "Borang tak masuk penuh dalam bingkai."

  4. **Bingkai panduan nisbah A4** (commit `4f5daf9`):
     - Master perasan bingkai putus-putus terlalu panjang utk A4. Punca: `#guideBox` guna 80%x80% saiz video → nisbah ikut kamera.
     - Fix: CSS `aspect-ratio:210/297` + center + max-width 90%. Visual sahaja, detection tetap scan seluruh frame.

  - **Pengajaran:** gejala sama ("markah salah") boleh ada 2 punca berbeza (geometri + threshold). Kumpul data brightness sebenar → buktikan punca, bukan teka.
  - **Status:** SEMUA 3 fix dah merge main `7d1755c` → LIVE PRODUCTION. Anchor tak perlu dalam bingkai putus-putus (scan seluruh frame); bingkai = panduan zon selamat sahaja.
  - **Detail penuh:** mypwa-v2/CLAUDE.md → section "OMR Scanner — Bug Fixes Detection (2026-06-15)".

### Session Recap (Lama)

- **Sesi 2026-06-12 petang/malam: iPad Bug Fixes — LIVE PRODUCTION** ✅

  1. **Dropdown race condition (ujian.html + laporan-ujian.html):**
     - `selUjian` disabled + "Memuatkan..." semasa `init()` fetch API
     - Loading feedback "Memuatkan..." pada dependent dropdowns semasa `onUjianChange()`
     - Root cause: iPad user tap dropdown sebelum `init()` siap load options

  2. **Sidebar logout button tidak kelihatan pada iPad:**
     - Fix 1: `.sidebar` height → `height:100vh; height:100dvh` (iOS 100vh bug)
     - Fix 2: `#sidebar` → `height:100dvh` (outer container masih 100vh)
     - Fix 3: `.sidebar-nav` tambah `min-height:0` (iOS flex child refuse shrink bug)
     - Fix 4: `.sidebar-logout` → `padding-bottom: env(safe-area-inset-bottom)` (iPad no home button)
     - Fix 5: Button visibility naik — opacity 0.6→0.85, border 0.15→0.3, background subtle
     - Root cause chain: iOS 100vh != visible area, flex min-height bug, button faint on touch device

  3. **Confirmed working on iPad by master** ✅
  4. **Playwright test:** 8/8 PASS (TEST_USER=test TEST_PASSWORD=test123)
  4. **Commits test:** `7aab1f3` → `64507f3` → `2dfaca4` → `59015df` → `d1292b2`
  5. **Merge:** test → main `ad1015e`
  6. **Deploy production:** Version `9591deac-6627-484b-8efe-8fe764b7b925`

- **Sesi 2026-06-12 pagi: OMR Scanner — UI Polish, Bug Fixes, Merge Production** ✅ LIVE PRODUCTION
  - Template, UI, bug fixes (anchor dark detect, duplicate const grid, mobile navbar)
  - Merge test→main `83919b7` — LIVE production
  - Playwright 9/9 PASS

- **Sesi 2026-06-11 tengah hari: OMR Scanner — Execute 6 tasks** ✅ LIVE PRODUCTION

- **Sesi 2026-06-10 petang: Feature Auto-Tutup Ujian — LIVE PRODUCTION** ✅
  - Migration 024, commit `dcc1cbe`

### Remaining / Next Steps

| Task | Status | Notes |
|------|--------|-------|
| Compact mode Ujian Dalaman | ✅ SELESAI (2026-06-24) | Diselesaikan cara lain: preset zoom:0.88 + max 6 kad/halaman (page-break) dalam cetak dashboard. Live main 519bf0a |
| LinkedIn setup + dokumentasi | ⏳ BACKLOG | |

### Important Context
- Production DB: `0d2c2d33-0a87-46cc-9aa4-6df32ab4b23f`
- Staging DB: `f87c8bbc-77a5-4d57-88d1-284195de437f`
- Production URL: `erpm-sksalor.celikguru.my`
- Staging URL: `https://mypwa-v2-staging.syazwan-skpp82.workers.dev`
- Latest commit main: `4b091c0` (cache drill-down gred + drill-down carta gred GURU — LIVE production, ff-only merge dari test)
- Latest commit test: `4b091c0` (sama dengan main — synced selepas merge)
- Playwright test credentials (GURU): TEST_USER=test TEST_PASSWORD=test123
- Playwright PELAWAT creds: user PKP (staging), creds via env PELAWAT_USER/PELAWAT_PASSWORD — lihat memory reference_mypwa_pelawat_test
- App guna clean URL: pelawat mendarat `/pelawat` (bukan `/pelawat.html`)
- Migration 024 applied ke staging + production ✅
- AppScript secrets: APPSCRIPT_URL + APPSCRIPT_SECRET (wrangler secrets)
- CSS version: app.css?v=6
- Production version ID: `9591deac-6627-484b-8efe-8fe764b7b925`

### iPad Bug Notes (untuk rujukan masa depan)
- iOS `100vh` ≠ visible viewport — guna `100dvh` untuk fixed elements
- iOS flex children perlu `min-height:0` untuk scroll properly dalam flex column
- iOS touch device tiada `:hover` — jangan design UI yang bergantung pada hover untuk visibility
- `env(safe-area-inset-bottom)` untuk iPad/iPhone tanpa home button
