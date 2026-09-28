# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session Context
**Session Type**: kemas memory (drift) → bounded (skeleton loader) → deploy → siasatan awal (2026-09-28 pagi–tengah hari)
**Current Project**: `opr-program` (Google Apps Script, ganti AppSheet OPR SK Salor)
**Status**: 🟢 `@30` LIVE guru (rangka muat). 🟡 Master lapor masih ada putih sekejap SEBELUM rangka — Lucy sudah analisis kod, MENUNGGU "ya" master utk deployment UKUR sementara.

## Working Memory

### Sesi ni (urutan)
1. **Betulkan fail lapuk** — `project-list.md` + `reminders.md` (2026-08-27/29) disegerakkan lawan `git log` semua repo. Ditemui LAGI 3 drift yang master tak tunjuk: takwim `@22`→`@25`, digital-hub "brainstorming"→LIVE prod `a312f79`, baris insaniah "C1 DIHOLD sama". `certificate-generator` ditambah. Auto-memory index (`MEMORY.md`) turut dibetulkan. Commit `c731d68` (memory repo). Master sahkan password admin digital-hub SUDAH ditukar.
2. **Skeleton loader opr-program** (master: "onload lama, letak skeleton bukan skrin putih + label"). Laluan **bounded**, reka bentuk dibentang dalam chat, master "Ok". TDD: ujian dulu (RED 7/7 sebab betul) → kod → GREEN 617/617 (610+7). **7 mutasi DIGIGIT semua** (skrip mutasi pertama tipu: 2 dilangkau sebab CRLF tapi lapor "semua mati" — dibetulkan, kira `dilangkau`). Verify visual Edge headless: ukuran pertama SALAH (dua lajur sempit paksa butang membalut) → banding pada LEBAR SAMA: tab 36=36, tapis 70=70, kad 104=104px.
3. **Terjumpa sebelum kod:** `muatSenarai()` tak sorok kotak "Gagal memuat" bila Cuba Semula ⇒ rangka akan bertindan mesej lama. Dibetulkan (sorok 3 kotak).
4. Commit `3775d3f` (ciri) + `f506e46` (docs). Master "Ok deploy 30" → `clasp push --force` + `create-deployment --deploymentId …08Zi5LPY` → **`@30`**, disahkan `list-versions` + `list-deployments` ×2. Commit `ea623e4`.
5. **Hasil telefon master:** *"keluar skrin putih dulu sekejap baru ada kad kelabu"* (peranti TIDAK disebut). Skeleton berfungsi tapi tak boleh sentuh masa SEBELUM HTML diurai.
6. **Analisis (baca kod, belum diukur):** HTML ~670 KB satu fail; `LibJspdf` 356 + `LibHtml2canvas` 194 = 550 KB (82%). Pustaka cuma dipakai masa Hantar (`app.js.html:1528`) tapi diurai SEBELUM `app.js` (`index.html:83-84`) ⇒ `mulakanSesi()` dilambatkan BETUL-BETUL. `doGet` ringan tapi cold start belum diukur.
7. Master minta "simpan dulu" → save memory + session (ini).

### Pattern berkesan (rujukan sesi depan)
- **Audit fail sejenis, bukan yang disebut sahaja** — satu drift ditunjuk, tiga lagi ditemui lawan git.
- **Baca `lukisSenarai()`/pemanggil SEBELUM tulis ujian** — terjumpa isu bertindan yang ujian pertama tak akan tangkap.
- **Skrip mutasi mesti lapor "dilangkau" berasingan** — "semua mati" boleh menipu bila mutasi tak sempat jalan.
- **Banding visual/ukuran pada LEBAR SAMA** — dua lajur sempit beri jurang palsu (kad 145 vs 101).
- **Deploy risiko-rendah dulu, biar master lihat di telefon** — beri fakta yang tak boleh diperoleh dari kod.

## Session Recap (For AI Restart)
- `opr-program` `@30` LIVE (rangka muat, suite 617). `origin/master` @ `ea623e4` + commit memory terkini. Kerja bersih.
- 🔴 **SAMBUNG:** tanya master "ya"/"tidak" untuk deployment **UKUR sementara** (ID berasingan, BUKAN URL guru — corak `@27`). Pemasa di bawah skrin: responseEnd (muat turun) · rangka diurai · rangka dicat (rAF) · pustaka siap · `mulakanSesi` dihantar · sesi siap · senarai siap. Master baca nombor + SEBUT PERANTI. Padam deployment + buang kod ukuran lepas guna.
- Keputusan ikut nombor: **muat turun dominan** → buang 550 KB dari HTML awal, ambil di latar (besar) · **urai dominan** → hantar `mulakanSesi()` SEBELUM pustaka diurai (kecil) · **`doGet` dominan** → kedua-duanya tak banyak membantu.
- Tertunggak master: smoke `@29` 3 langkah (Buku Program >10MB ditolak serta-merta; status 3 peringkat; wifi putus).
- Backlog lain opr-program: 3 PDF contoh kualiti (langkah 3/3) · sifar palsu sedia ada bila menaip semasa memuat · Fasa 4 Migrasi.
- Keputusan tertunggak sistem memory: projek Aktif kini 11 (had 10) — turunkan `idme-pajsk-ext`? · lupuskan `coding-projects/active/`?

---
*Session updated: 2026-09-28 ~12:20 (opr-program skeleton `@30` LIVE, menunggu keputusan ukur)*
