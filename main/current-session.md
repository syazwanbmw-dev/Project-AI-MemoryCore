# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: master "sambung opr program" → bersih ukuran → pra-muat + cache → deploy `@28` → ukur hantar laporan (2026-09-19, ~10:39–13:15)
**Current Project**: `opr-program`
**Status**: 🟢 Guru LIVE `@28`, suite 594/594, tree bersih, `master`==`origin/master`. 🟡 Pagar saiz client Buku Program BELUM dibina — menunggu "ya" master.

## Working Memory

### Sesi ni (2026-09-19 tengah hari)
1. **A siap:** kod ukuran dipulih, `Ukur.gs` dipadam, HEAD bersih, deployment ujian `@27` dipadam & disahkan 2× (gotcha `list-deployments` basi).
2. **B(2) `f1024c5`:** `praMuatSenarai()` hantar `senaraiLaporan()` serentak dgn `mulakanSesi()`; `SENARAI_PRA` dimakan SEKALI; hanya `ok:true` digunakan (gagal senyap → panggilan segar). +3 ujian, 3 mutasi ditangkap.
3. **B(3) TETAPAN 2× — SKIP** (0.1–0.2 s, sentuh gerbang setup).
4. **(4) cache rujukan `857dc18`:** `bacaSenaraiRujukanSesi_()` `CacheService` TTL **120 s** (master pilih A + 2 min). Gerbang simpan `ReportService` ×2 KEKAL sheet segar. +9 ujian (ambil TEKS fungsi + `new Function` dgn CacheService palsu), 4 mutasi ditangkap.
5. **Deploy `@28`** atas "Deploy" master (ID guru sama). Master: "rasa lebih laju".
6. **Ukur hantar laporan** (Apps Script Executions, percuma): 2 gambar = 16–17 s, pelayan `ciptaLaporan` ~3 s ⇒ ~11–12 s telefon+muat naik. 4 gambar + Buku 177 MB ⇒ telefon **CRASH**.
7. **Punca crash:** had 10 MB hanya di pelayan; client tak pernah guna `hadBukuProgramBait` (dihantar `Kod.gs:94`); `readAsDataURL` 177 MB→~236 MB memori.
8. Memory ditulis: `opr-program/MEMORY.md` (commit `5804f5a`), `project_opr_program.md`, `feedback_ukur_sebelum_optimum.md` (+Executions), **baharu** `feedback_nilai_dihantar_tak_dipakai.md`, indeks.

### 🔴 SAMBUNG (menunggu "ya" master — pelan sudah dibentang)
1. Pagar saiz client Buku Program (commit sendiri; deploy cepat disyor — crash boleh kena guru sebenar).
2. Mesej peringkat hantar ("Menjana PDF…"→"Memuat naik…"→"Menyimpan…").
3. 3 PDF contoh Edge headless (2×/0.92 kini, 1.5×/0.85, 1.5×/0.75) → lapor saiz+rupa → master pilih (harga: kurang tajam dizum/cetak).
Smoke telefon `@28` penuh (Cuba Semula/padam/edit segar, dropdown ≤2 min, kategori dibuang ditolak) belum disahkan lengkap.

### Nota teknikal
- `app.js.html` + `*.gs` **CRLF**; `tests/*.js` LF. Edit tool boleh campur LF → normalkan `replace(/\r?\n/g,"\r\n")`, sahih `git diff --stat` cuma tambahan. `grep -c $'\r'` TAK boleh dipercayai — semak dgn node `includes("\r\n")`.
- `git`/`clasp` pesan commit: guna `-F fail` (scratchpad), bukan here-string PowerShell.
- Ujian fungsi sentuh API Google: ambil TEKS fungsi + `new Function` dgn kebergantungan palsu.
- Master pilih penyelesaian VISUAL (kualiti PDF) daripada MELIHAT contoh, bukan perbincangan.

## Session Recap (For AI Restart)
- Tunggu jawapan master atas pelan 3 langkah. Jangan mula kod tanpa "ya" (gerbang `plan` master kuatkuasakan).
- Master mengesahkan dgn **ukuran & guna sistem**; soal ketepatan kaedah ukur ("tepat ke jam randik?") — jawab jujur, sebut had.

---
*Session updated: 2026-09-19 ~13:15 (opr-program hantar laporan diukur)*
