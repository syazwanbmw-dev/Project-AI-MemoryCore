# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-25 petang — 🟢 **Projek `opr-insaniah`: siri fix PDF + Senarai Laporan, LIVE production `@44`.**

### Sesi ni (petang, `opr-insaniah`)

Master lapor 4 isu berturut-turut di peranti sebenar (iPhone Chrome + iPad 11"), semua
disiasat guna `systematic-debugging` + `AskUserQuestion` untuk kumpul bukti sebelum fix
(bukan teka). Setiap fix: TDD + mutation-test (dibuktikan gigit, dipulih guna `cp`) +
`clasp push` + `create-deployment` ke ID sedia ada + disahkan `list-deployments` DUA KALI.

1. **PDF beza ikut peranti** — punca: `font-family:system-ui` (iPhone dpt San Francisco,
   Android dpt Roboto, html2canvas rakam SKRIN sebenar). Fix: kunci fon **'Inter'** (Google
   Fonts), master pilih dari artifak perbandingan 4 fon. Tambah `document.fonts.ready` gate
   sebelum `html2canvas()` (helper `tangkapNodA4()`, dipakai 2 laluan).
2. **Buka PDF di iPhone → tab `about:blank`** — 3 pusingan (R1→R2→R3):
   - R1: pautan manual "Buka ↗" SENTIASA papar (bukan hanya bila `tab===null`)
   - R2: try/catch pd `tab.location.href` + jam batas 20s (butang tersekat "Membuka…")
   - R3: `PELAYAR_TAB_TANGGUH_GAGAL` (UA CriOS/FxiOS/EdgiOS/OPiOS) skip `window.open()`
     terus pada pelayar iOS pihak ketiga -- tiada lagi tab kosong yatim
   - **Punca sebenar:** Chrome iOS (WKWebView wrapper Apple) tak sokong navigasi
     lewat-tangguh pada tab yg dibuka awal. Pautan manual berjaya sebab iOS pintas
     `drive.google.com` jadi Universal Link → buka app Drive NATIF, bukan Chrome.
3. **Jadual Senarai Laporan terpicit di iPad MENEGAK** — 3 pusingan (R1→R2→R3), DUA
   percubaan CSS GAGAL sebelum betul:
   - R1 (`.jadual-gulir` overflow-x:auto sahaja) — GAGAL, `<table>` blok biasa regang
     ikut bekas secara lalai walau bekas boleh gulir
   - R2 (+ `width:max-content` + sorok lajur ID) — BERFUNGSI teknikal, tapi master minta
     pendekatan lain selepas MELIHAT
   - R3 (keputusan akhir): **lanjutkan mod kad telefon** ke tablet MENEGAK guna
     `@media (max-width:640px), (max-width:1100px) and (orientation:portrait)` — landscape
     kekal jadual biasa. R1+R2 dibuang SEPENUHNYA (bukan kod mati)
4. **Header + label kad kelabu susah baca** — `th`/`td::before` guna `var(--lembut)` →
   `var(--teks)`. Master perasan SERTA-MERTA bila satu sahaja dibetulkan (dua rupa, satu
   peranan) — corak `feedback_bentangan_separa` lagi sekali, dipuji "kelakuan dan ceria".

**Disiplin dikuatkuasakan konsisten:** setiap fix commit BERASINGAN + docs(memory) commit
berasingan, `AskUserQuestion` sebelum setiap commit+deploy (kecuali bila master dah explicit
"proceed"), mutation-test SETIAP ujian baharu sebelum dianggap selesai.

🔴 **Silap Lucy sendiri, direkod telus:** (a) sekali cuba pulih mutasi guna `git checkout`
bukan `cp` — hampir hilang kerja R3 belum-commit (CLAUDE.md dah amaran tepat pasal ini,
ditangkap+dipulih serta-merta); (b) draf komen CSS tersilap tulis pattern `.a4 { ... }`
literal dalam komen prosa, terpadan silap oleh ujian scan-sumber sedia ada — suite SENDIRI
tangkap sebelum commit.

### ⏭️ Bila sambung

Master belum sahkan R3 (mod kad iPad) + fix warna label kad di peranti sebenar. Tiada task
terbuka selain itu. Fasa 2b (Edit+Padam laporan, disebut dlm project_opr_insaniah.md) masih
belum dirancang -- bukan fokus sesi ini.

Deploy penuh sesi ini: `@37 → @38 → @39 → @40 → @41 → @42 → @43 → @44`. Semua ke ID
deployment SEDIA ADA (`AKfycbxss9BfkhHTi4fCEvf_6U4GV9iK4I6sXa-Yt8iqQA8WsyyjoH_04bz4i5y879jQQn1m5Q`).
Commit terakhir: `b92f167`.
