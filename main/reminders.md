# Reminders — Lucy & Master
*Persistent cross-session. Item TERBUKA sahaja — butiran penuh ada dalam `project_<nama>.md`
auto-memory (`C:\Users\user\.claude\projects\C--Users-user\memory\`), jangan duplicate di sini.*
*Kemas kini: 2026-10-07, disegerak drpd `git log` + MEMORY.md projek (git yang betul).*

---

## Terbuka

- **`opr-insaniah`** — 🟢 `@60` LIVE + digabung `master` (`53ca059`), tiada cabang tertunggak. **BELUM launch
  rasmi ke sekolah** (dibetulkan master 2026-09-01). Calon seterusnya (master putuskan): ikon ✕ pada chip ·
  **uji PPD** (`moe.gov.my` buka pautan PDF ANYONE_WITH_LINK? akaun sebenar belum diuji; iPhone tiada) ·
  KAUNTER reset (padam baris ujian + fail Drive yatim dulu) · Cari kelas + launch rasmi (ditangguh).
  🟡 Tak disahkan dlm MEMORY: I5 (`oauthScopes`, perlu tetingkap cuti sekolah — re-consent semua guru);
  kesesakan pelayan `mulakanSesi()` 3–4 s belum disiasat. C1 SUDAH ditutup (lihat Selesai).
- **`opr-program`** — 🟡 **SAMBUNG: menunggu "ya" master** utk deployment UKUR sementara (bukan URL
  guru). Rangka muat `@30` LIVE 2026-09-28 (suite 617); master lapor di telefon "putih dulu sekejap
  baru rangka" — putih = SEBELUM HTML diurai. HTML ~670 KB, 82% pustaka PDF yang cuma dipakai masa
  Hantar tapi diurai SEBELUM `app.js` (melambatkan `mulakanSesi()`). Belum diukur — nombor tentukan
  pilihan (muat turun / urai / `doGet` dominan). Master sebut PERANTI bila baca nombor.
  🔴 **TERTUNGGAK MASTER:** smoke `@29` (LIVE sejak 2026-09-20). 3 langkah smoke `@29`: (1) pilih Buku Program >10MB →
  mesti ditolak SERTA-MERTA (lencana + butang Hantar mati) sebelum cuba muat naik; (2) hantar laporan
  biasa → status "Menjana PDF…" → "Memuat naik…" → "Menyimpan…"; (3) tutup wifi tengah proses →
  status kosong semula + banner ralat biasa, bukan terperangkap. Buku Program 177MB belum diuji
  langsung di peranti (pagar diuji unit+mutasi sahaja).
  **Backlog:** langkah 3/3 = 3 PDF contoh Edge headless (2×/0.92, 1.5×/0.85, 1.5×/0.75) → master pilih
  (keputusan visual) · Fasa 4 Migrasi 100 rekod AppSheet · Admin edit SEMUA laporan · Panel
  `RujukanService.gs` (ditunda sengaja) · drift Escape/ID-pangkas modal salin ke sini drpd opr-insaniah.
- **`celiksains`** — 🟡 Hardening anti-tipu LIVE **staging** (production BELUM). Sub-projek 2 (auth
  email+OTP): kod SIAP Task 1–8; **Task 9 perlu master** (`wrangler login` skop Email, izin
  `email sending enable`; Email Service = Workers Paid $5). Keputusan master tertunggak: had kadar ·
  enumerasi/PDPA · UI login. Tiada git remote.
- **`mypwa-v2`** — 🟡 **Pelan baiki query `GET /api/ujian` (kuota D1 free tier) SIAP, BELUM diluluskan
  master, BELUM ada kod.** `mypwa-v2-db` ≈ 100% read akaun, 81% = satu query itu. Master perlu luluskan
  **T0** (pelan) + jawab 3 soalan terbuka; **T8** (deploy production) tunggu "ya" jelas. Pelan:
  `docs/superpowers/plans/2026-09-29-mypwa-v2-baiki-query-senarai-ujian-plan.md`.
  Juga: Kumpulan Intervensi mendarat **MATI** (`guna_kumpulan=0` pada 36/36 item, suite 58/0/2) —
  tunggu admin hidupkan bila sedia, sengaja, bukan bug.
- **`digital-hub`** — Tiada tugasan kod terbuka. LIVE prod `a312f79` (2026-09-02). Master: telefon
  Android yang DAH install PWA masih ikon lama → uninstall + Chrome site settings Delete data +
  reinstall (Chrome cache manifest ~24j). ✅ Password admin prod SUDAH ditukar (disahkan master
  2026-09-28). Backlog design-stage: audit log · kategori button (Pengurusan/Kurikulum/HEM/Kokur) ·
  strip pengumuman · WAF rate-limit F5.
- **`idme-pajsk-ext`** — Task 7 gated — tunggu master bekal selector borang sebenar
  `idme.moe.gov.my`.
- **`erph`** — RPT Sains Tahun 5 separuh jalan (branch `rpt-sains5`). Sambung: re-review commit
  `eab609c..ea0fd95`.
- **`BrightMe`** — Pending keputusan master: format modul BAKAT (belum ditetapkan).
- **Keputusan master tertunggak (sistem memory):** folder `coding-projects/active/*.md` (LRU lama)
  — lupuskan atau kekal arkib? · Projek Aktif kini 11 (had 10) — turunkan `idme-pajsk-ext`?
  · repo `memory/` ahead 3 komit drpd origin (belum dipush).

## ✅ Tiada tugasan terbuka

- **`takwim-digital`** — LIVE `@26` (2026-10-04), suite 124/0. Butang "Hantar Sekarang" (admin) + swipe
  kalendar + digest Telegram/Google Chat + reminder email H-1/2/3 + cuti Google dikongsi ke digest.
- **`certificate-generator`** — LIVE (GitHub Pages). ⚠️ Push TAK auto-deploy: lepas setiap push, run
  `gh workflow run static.yml --repo syazwanbmw-dev/Certificate-Generator`.

## ⚠️ Ranjau kekal (bukan tugasan — amaran operasi)

- **`erph-menengah-v2`** — Menu **"🛠️ Baikpulih Tapak"** MENULIS ke sistem sebenar — jangan klik
  semasa uji/demo.

## Selesai baru-baru ini (ringkasan sahaja — baca `project_<nama>.md` untuk butiran penuh)

- ✅ `opr-insaniah` — `@45`→`@60` (2026-10-05..07): C1 tutup pendedahan `google.script.run` (19 fungsi `_`,
  `1fe0089`) · medan KELAS + panel kelas `@49`/`@50` · gambar tambah/buang `@51`/`@52` · input Masa dua
  pemilih `@55` · PDF ANYONE_WITH_LINK `@57` · butang Panduan + butang seragam + ikon SVG `@58`–`@60`.
  Digabung `master` `53ca059`, suite 1013/0.
- ✅ `takwim-digital` — Butang "Hantar Sekarang" di System Settings. LIVE `@26` (2026-10-04).
- ✅ `celiksains` — Sub-projek 2 kod siap (Task 1–8) + re-review fix wave `fbdd72a` lulus (2026-09-29/30).
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
- ✅ `digital-hub` — Import Setting + PWA installable ("Digital SKS") + Open Graph. LIVE prod
  `a312f79`, suite 197u/71e (2026-09-02).
- ✅ `erph-menengah-v2` — 5 bug import + prompt 2 objektif selesai & disahkan (2026-07-23).
- ✅ `sistem-olahraga-sekolah` — Throttle brute-force `/api/login` LIVE production (2026-07-19).

---
*Sejarah lebih lama (takwim-digital Ogos–Sep, digital-hub Ogos–Sep, dll) sudah dibuang dari fail ni*
*(DRY) — rujuk `project_<nama>.md` atau `git log` repo masing-masing.*
