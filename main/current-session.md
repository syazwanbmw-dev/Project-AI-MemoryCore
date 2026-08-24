# 🌟 Current Session Memory - RAM
*Temporary working memory - resets each session, provides recap when AI restart*

---

## Session RAM Status
**Current Session**: 2026-08-24 petang — 🟡 **Projek BAHARU `Digital Hub` (portal akses semua sistem sekolah): brainstorming architectural SEDANG JALAN, belum sampai spec/plan.**

### Sesi ni (petang, `Digital Hub`)

Master nak satu **portal/landing page** untuk akses pantas semua sistem digital sekolah —
`mypwa-v2` (eNilai, perlu login), `opr-program`, `takwim-digital` (dua-dua login ikut email
berdaftar), `sijil generator`, dan kemungkinan projek lain akan datang. Paparan utama **public**
(tak perlu login), admin je perlu log masuk untuk uruskan nama sekolah/logo/warna tema/tambah
button sistem baharu.

**Klasifikasi:** architectural (projek baharu) → brainstorming superpowers, soal-jawab satu-satu.

**Keputusan setakat ini** (belum sampai tahap tulis spec lagi):
- Skop: **SATU sekolah sahaja** (bukan multi-tenant/SaaS) — mudah, admin = master sendiri
- Projek **baharu berasingan** (bukan tambah route dalam mypwa-v2 sedia ada)
- Nama projek: **"Digital Hub"** (folder cadangan `digital-hub`, sama level projek lain dalam
  `Documents\code`)
- Admin login: **password tunggal** (bukan sistem akaun/email macam opr-program) — JWT dalam
  header `Authorization: Bearer` disimpan di localStorage, **bukan cookie**, supaya CSRF auto
  kalis (ikut standard sedia ada dalam `reference_security_checklist.md`)
- Logo sekolah: **URL sahaja** (admin taip link, tak payah setup R2/upload)
- Warna tema: admin **pilih dari 7 palet siap-pakai** (`reference_palet_warna_ui.md`), bukan color
  picker bebas — elak kombinasi tak sedap mata
- Button sistem: **Nama + URL + Ikon/emoji**, boleh susun turutan & toggle aktif/sorok tanpa delete

🔑 **Storan: Cloudflare KV, BUKAN D1** — master sendiri yang perasan isu ni pertengahan brainstorm
(*"resources.cloudflare kan bagi 10 db je untuk free tier"*), lepas mula-mula tanya pasal Google
Sheets (ditolak sendiri sebab sama sebab — kuota). Lucy sahkan KV memang **lebih sesuai secara
teknikal** juga (bukan sekadar workaround kuota) sebab data portal ni cuma 2 blob config
(`settings` + senarai `buttons`), bukan data relational yang perlukan query/JOIN. Backup: tak
perlu sistem formal — KV Cloudflare replicated + data kecil senang re-enter manual; optional
butang "Eksport Setting" (download JSON) sebagai jaring keselamatan ringan sahaja.

➡️ **Corak master sesi ni:** dia fikir merentas SEMUA projek (kuota akaun Cloudflare), bukan
hanya projek yang sedang dibincang — walaupun tak formal bertanya, dia perasan sendiri constraint
infra sebelum Lucy sempat. (Lihat juga corak sedia ada: master pentingkan konsistensi
stack/keputusan merentas projek.)

### ⏭️ Bila sambung

Belum siap bentang **design penuh** dalam chat (route API, skema KV tepat, mapping ke 7 ancaman
security checklist). Urutan seterusnya (ikut skill `brainstorming`):
1. Bentang design penuh (arkitektur, data flow, security) → approval master
2. Tulis spec ke `digital-hub/docs/superpowers/specs/2026-08-24-digital-hub-design.md` + commit
3. Self-review spec, minta master semak fail
4. `writing-plans` → implementation plan
5. Baru boleh mula kod (projek belum wujud secara fizikal lagi — folder belum dicipta)

Tiada kod ditulis lagi. Semua di atas keputusan **perbualan sahaja**.
