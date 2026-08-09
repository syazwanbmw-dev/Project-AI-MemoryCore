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

---

## Catatan Penting

- **Konsistensi kolum jadual (terutama cetak):** master pentingkan kolum jadual kekal konsisten. Label decorative (cth "Tidak Menguasai") yang muncul hanya pada sesetengah baris buat kolum jadi tak sejajar/tak konsisten — master prefer **BUANG label itu dari paparan cetak** daripada cuba dandan style. Paparan skrin/desktop boleh kekal ada label. (Origin: feature Trend Markah ETR, 2026-07-04)
