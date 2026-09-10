# Reminders — Lucy & Master
*Persistent cross-session. Item TERBUKA sahaja — butiran penuh ada dalam `project_<nama>.md`
auto-memory (`C:\Users\user\.claude\projects\C--Users-user\memory\`), jangan duplicate di sini.*
*Kemas kini: 2026-08-29, disegerak drpd MEMORY.md sebenar.*

---

## Terbuka

- **`takwim-digital`** — ✅ Tiada tugasan terbuka. LIVE `@24` (2026-09-08: admin boleh pilih
  cuti Google mana nak dikongsi ke digest Telegram/Google Chat via checklist System Settings —
  master confirm test production "dah test dan ok"). Digest Mingguan Telegram+Google Chat
  (`@23`, 2026-09-07) + reminder aktiviti email H-1/H-2/H-3 (`@22`) kekal LIVE.
- **`opr-program`** — 🟢 Fix prestasi hantar/kemas kini laporan (cache folder Drive per-hantaran +
  batch write) — **6 task kod SIAP + review akhir Opus bersih** (suite 391→425, commit
  `bf0f6da`..`ac6c00a`, `master` @ `4ae1b5f` 9 commit depan origin). Deployment UJIAN `@17` dibuat
  (guru KEKAL `@16`). **SAMBUNG: master smoke 9 langkah** pada URL ujian `@17` (Task 7 Step 8 pelan).
  Kalau PASS → deploy guru + padam `@17` + push origin. TIADA deploy tanpa izin. (2026-09-10)
  Fasa 2 + C1/I5 SIAP lama (suite 128/128), deployed `@5` — smoke `@5` DIHOLD berasingan.
- **`opr-insaniah`** — 🔒 C1 (Critical) SAMA seperti opr-program: fungsi global tanpa `_` boleh
  dipanggil terus via `google.script.run` → pintas auth. Deployed `@44`, tapi **BELUM launch
  rasmi ke sekolah** (dibetulkan master 2026-09-01 — catatan lama "LIVE harian" SALAH, sudah
  dibetulkan merentas memory). C1-rename murah (tiada re-consent) → buat bila sambung semula.
  I5 (`oauthScopes`) di sana perlu tetingkap cuti sekolah (re-consent semua guru). (dari
  keputusan skop master 2026-08-31, Pilihan A) — **DIHOLD 2026-09-01**, sambung `digital-hub`.
- **`digital-hub`** — Tiada tugasan kod terbuka (5 perubahan LIVE prod `a312f79` 2026-09-02).
  🔑 Master: di telefon Android yang DAH install PWA, ikon masih lama — **uninstall + Chrome
  site settings → Delete data + reinstall** untuk dapat ikon jata sekolah (Chrome cache
  manifest ~24j). Backlog design-stage (belum brainstorm): audit log · kategori button
  (Pengurusan/Kurikulum/HEM/Kokur) · strip pengumuman.
- **`celiksains`** — Hardening anti-tipu: spec + plan SIAP, BELUM mula kod.
- **`idme-pajsk-ext`** — Task 7 gated — tunggu master bekal selector borang sebenar
  `idme.moe.gov.my`.
- **`erph`** — RPT Sains Tahun 5 separuh jalan. Sambung: re-review commit `eab609c..ea0fd95`.
- **`mypwa-v2`** — Feature Kumpulan Intervensi mendarat **MATI** (`guna_kumpulan=0` pada 36/36
  item, suite 58/0/2). Tunggu admin hidupkan bila sedia — sengaja, bukan bug.
- **`BrightMe`** — Pending keputusan master: format modul BAKAT (belum ditetapkan).

## ⚠️ Ranjau kekal (bukan tugasan — amaran operasi)

- **`erph-menengah-v2`** — Menu **"🛠️ Baikpulih Tapak"** MENULIS ke sistem sebenar — jangan klik
  semasa uji/demo.

## Selesai baru-baru ini (ringkasan sahaja — baca `project_<nama>.md` untuk butiran penuh)

- ✅ `takwim-digital` — Cuti Google dikongsi ke digest (checklist System Settings, padanan
  EXACT nama dari Google, bukan cikgu taip) + fix teks lapuk SETUP.md/.html §8.4 OAuth. LIVE
  `@24`, commit `b967e22`+`89fb4c0`, suite 100/0 (2026-09-08).
- ✅ `digital-hub` — Import Setting (POST /api/admin/import, ganti semua, atomik) + PWA
  installable: nama app "Digital SKS", ikon PNG same-origin 192/512, service worker minimal
  network-first, `GET /api/public/icon` redirect ke logo. LIVE prod `a312f79`, suite
  197u/71e (2026-09-02).
- ✅ `takwim-digital` — Logout skrin putih FIXED (`location.reload` iframe → pane + flag) +
  cetak laporan bg putih + ikon custom via URL (https-only) + label dashboard lebih gelap.
  LIVE `@19`, commit `91f371c`+`ed8aff0` (2026-09-01 malam).
- ✅ `takwim-digital` — Responsive iPad: kalendar dashboard + topbar tak lagi terhimpit (grid3
  runtuh 2-kolom `≤1200px` / 1-kolom `≤900px`, topbar flex-wrap `≤960px`). LIVE `@18`, commit
  `6367008` (2026-09-01).
- ✅ `digital-hub` — Slogan portal custom + headline 2-baris (nama_baris2) + fix visual
  (kontras slogan, outline button, label Headline 1/2). LIVE production `46aa5c8`, suite
  169u/56e hijau (2026-09-01).
- ✅ `digital-hub` — Icon URL button + banner jadi hero band LIVE prod+test (`3b978cc`,
  suite 153u/51e hijau) (2026-08-29).
- ✅ `takwim-digital` — Auto-login + pendaftaran satu-langkah (buang OTP) + Button Delete User.
  LIVE `@17` (2026-08-27).
- ✅ `opr-insaniah` — Siri 4 fix guna sistem sebenar (fon PDF, tab iPhone, jadual iPad, warna
  label). Suite 526/526, LIVE `@44` (2026-08-25).
- ✅ `erph-menengah-v2` — 5 bug import + prompt 2 objektif selesai & disahkan (2026-07-23).
- ✅ `sistem-olahraga-sekolah` — Throttle brute-force `/api/login` LIVE production, tiada kerja
  tertunggak (2026-07-19).

---
*Sejarah penuh opr-insaniah Fasa 1/2/2b/2c dan lain-lain item lama yang dulu tersenarai di sini*
*sudah SEMUA selesai dan dibuang dari fail ni (DRY) — rujuk `project_opr_insaniah.md` kalau perlu.*
