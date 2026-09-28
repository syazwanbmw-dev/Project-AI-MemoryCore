# Reminders — Lucy & Master
*Persistent cross-session. Item TERBUKA sahaja — butiran penuh ada dalam `project_<nama>.md`
auto-memory (`C:\Users\user\.claude\projects\C--Users-user\memory\`), jangan duplicate di sini.*
*Kemas kini: 2026-09-28, disegerak drpd `git log` + MEMORY.md projek (git yang betul).*

---

## Terbuka

- **`opr-program`** — 🔴 **TELEFON tertunggak (master):** (a) rangka muat `@30` (LIVE 2026-09-28, suite 617,
  `origin/master` @ `f506e46`) — muncul cukup AWAL atau masih putih dulu? Jawapan tentukan sama ada perlu
  tangguh pustaka PDF (UKUR dulu); (b) smoke `@29` (LIVE sejak 2026-09-20). 3 langkah smoke `@29`: (1) pilih Buku Program >10MB →
  mesti ditolak SERTA-MERTA (lencana + butang Hantar mati) sebelum cuba muat naik; (2) hantar laporan
  biasa → status "Menjana PDF…" → "Memuat naik…" → "Menyimpan…"; (3) tutup wifi tengah proses →
  status kosong semula + banner ralat biasa, bukan terperangkap. Buku Program 177MB belum diuji
  langsung di peranti (pagar diuji unit+mutasi sahaja).
  **Backlog:** langkah 3/3 = 3 PDF contoh Edge headless (2×/0.92, 1.5×/0.85, 1.5×/0.75) → master pilih
  (keputusan visual) · Fasa 4 Migrasi 100 rekod AppSheet · Admin edit SEMUA laporan · Panel
  `RujukanService.gs` (ditunda sengaja).
- **`opr-insaniah`** — 🔒 C1 (Critical): fungsi global tanpa `_` boleh dipanggil terus via
  `google.script.run` → pintas auth. Deployed `@44`, **BELUM launch rasmi ke sekolah** (dibetulkan
  master 2026-09-01). opr-program dah siap versi sendiri (2026-08-31); versi opr-insaniah masih
  DIHOLD — langkah pertama grep semua pemanggil. I5 (`oauthScopes`) perlu tetingkap cuti sekolah
  (re-consent semua guru). Onload lambat disiasat 2026-09-10 (2 round-trip + baca sheet berulang),
  BELUM dibaiki. Mod kad jadual iPad potret belum disahkan di peranti (`@44`).
- **`digital-hub`** — Tiada tugasan kod terbuka. LIVE prod `a312f79` (2026-09-02). Master: telefon
  Android yang DAH install PWA masih ikon lama → uninstall + Chrome site settings Delete data +
  reinstall (Chrome cache manifest ~24j). ✅ Password admin prod SUDAH ditukar (disahkan master
  2026-09-28). Backlog design-stage: audit log · kategori button (Pengurusan/Kurikulum/HEM/Kokur) ·
  strip pengumuman · WAF rate-limit F5.
- **`celiksains`** — Hardening anti-tipu: spec + plan SIAP, BELUM mula kod.
- **`idme-pajsk-ext`** — Task 7 gated — tunggu master bekal selector borang sebenar
  `idme.moe.gov.my`.
- **`erph`** — RPT Sains Tahun 5 separuh jalan (branch `rpt-sains5`). Sambung: re-review commit
  `eab609c..ea0fd95`.
- **`mypwa-v2`** — Feature Kumpulan Intervensi mendarat **MATI** (`guna_kumpulan=0` pada 36/36
  item, suite 58/0/2). Tunggu admin hidupkan bila sedia — sengaja, bukan bug.
- **`BrightMe`** — Pending keputusan master: format modul BAKAT (belum ditetapkan).
- **Keputusan master tertunggak (sistem memory):** folder `coding-projects/active/*.md` (LRU lama)
  — lupuskan atau kekal arkib? · Projek Aktif kini 11 (had 10) — turunkan `idme-pajsk-ext`?

## ✅ Tiada tugasan terbuka

- **`takwim-digital`** — LIVE `@25` (2026-09-20), suite 100/0. Swipe kalendar + digest Telegram/Google
  Chat + reminder email H-1/2/3 + cuti Google dikongsi ke digest, semua LIVE.
- **`certificate-generator`** — LIVE (GitHub Pages). ⚠️ Push TAK auto-deploy: lepas setiap push, run
  `gh workflow run static.yml --repo syazwanbmw-dev/Certificate-Generator`.

## ⚠️ Ranjau kekal (bukan tugasan — amaran operasi)

- **`erph-menengah-v2`** — Menu **"🛠️ Baikpulih Tapak"** MENULIS ke sistem sebenar — jangan klik
  semasa uji/demo.

## Selesai baru-baru ini (ringkasan sahaja — baca `project_<nama>.md` untuk butiran penuh)

- ✅ `certificate-generator` — Hosting standalone GitHub Pages + redesign UI "Ruang Kerja Bersih"
  (putih+emerald, Inter+Manrope) LIVE, commit `00a4692` (2026-09-27/28).
- ✅ `opr-program` — Rangka muat (skeleton) ganti skrin putih. LIVE `@30`, suite 617 (2026-09-28).
- ✅ `opr-program` — Pagar saiz client Buku Program (elak crash telefon 177MB) + mesej peringkat
  hantar. LIVE `@29`, suite 610 (2026-09-20).
- ✅ `takwim-digital` — Swipe kalendar tukar bulan + animasi slide + isyarat loading. LIVE `@25`,
  commit `b3beb5a`+`6153dee` (2026-09-20).
- ✅ `opr-program` — Prestasi hantar laporan (pra-muat senarai + cache rujukan 2 min) LIVE `@28`;
  bug Edit padam Jawatan + gambar herot + tarikh DD/MM/YYYY LIVE `@25`; medan Jawatan `@21`;
  Fasa 3b Panel Admin Pengguna `@20`; cache folder Drive `@18` (2026-09-17..19).
- ✅ `takwim-digital` — Cuti Google dikongsi ke digest (checklist System Settings) LIVE `@24`;
  Digest Mingguan Telegram+Google Chat `@23`; reminder aktiviti email `@22` (2026-09-06..08).
- ✅ `digital-hub` — Import Setting + PWA installable ("Digital SKS") + Open Graph. LIVE prod
  `a312f79`, suite 197u/71e (2026-09-02).
- ✅ `opr-insaniah` — Siri 4 fix guna sistem sebenar (fon PDF, tab iPhone, jadual iPad, warna
  label). Suite 526/526, LIVE `@44` (2026-08-25).
- ✅ `erph-menengah-v2` — 5 bug import + prompt 2 objektif selesai & disahkan (2026-07-23).
- ✅ `sistem-olahraga-sekolah` — Throttle brute-force `/api/login` LIVE production (2026-07-19).

---
*Sejarah lebih lama (takwim-digital Ogos–Sep, digital-hub Ogos–Sep, dll) sudah dibuang dari fail ni*
*(DRY) — rujuk `project_<nama>.md` atau `git log` repo masing-masing.*
