# Log Keputusan — Lucy & Master
*Append-only. Setiap keputusan arkitek dan teknikal yang penting disimpan di sini.*

---

## 2026-05-03 — Adapt upstream sebagai Feature/ reference, bukan merge
**Konteks**: Upstream Kiyoraka ada "unrelated histories" — merge terus akan overwrite fail Lucy yang dah custom.
**Keputusan**: Checkout Feature/ folders dari upstream sahaja. Fail Lucy (main/, plugins/, docs/) tidak disentuh.
**Sebab**: Lebih selamat — kita dapat semua feature baru tanpa risiko hilang customization Lucy. Feature/ berfungsi sebagai dokumentasi dan rujukan install.

---

## 2026-08-22 — Opus wajib untuk fasa /plan sahaja, bukan sesi utama
**Konteks**: Master syak Kata pipeline guna Opus berlebihan (kos mahal). Audit ledger
`subagent-driven-development` merentas 8 projek (`opr-insaniah`, `mypwa-v2`, `celiksains`,
`sistem-olahraga-sekolah`, `erph`) tunjuk Opus dah pun jarang — ~1 dispatch per cawangan
siap (semakan akhir sebelum merge), berbanding puluhan dispatch Sonnet/Haiku untuk kerja
implementasi. Semakan `~/.claude.json` (cache CLI) pula dedah rekod pertukaran model
sonnet↔opus berselang-seli dalam sesi UTAMA beberapa hari kebelakangan — konsisten dengan
`/fast` di-toggle untuk seluruh sesi, walhal `settings.json` default kekal Sonnet.
**Keputusan**: Kekalkan pipeline Kata sedia ada (Sonnet sesi utama + Sonnet/Haiku untuk
implementasi + Opus untuk semakan akhir/omnipotent — tak berubah). TAMBAH SATU peraturan
baharu: fasa `/plan` — khusus bahagian PENULISAN reka bentuk/spec (bukan soal-jawab kumpul
konteks, itu kekal interaktif dalam sesi Sonnet) — WAJIB dispatch subagent `Plan` dengan
`model: opus` secara eksplisit. Ditolak: tukar seluruh sesi utama ke Opus (`/fast` sepanjang
sesi) — walaupun itu tafsiran awal cadangan master "Opus jadi orchestrator".
**Sebab**: Punca kos sebenar bukan pipeline Kata (data ledger bukti ia dah jimat) — ia sesi
utama yang di-toggle Opus secara manual, kena bayar kadar Opus untuk kerja MEKANIKAL
(baca fail, grep, edit kecil) sepanjang sesi. Fasa /plan pula ialah titik keputusan
arkitektur yang paling mahal untuk salah (kod ditulis atas reka bentuk silap) — sepadan
dengan prinsip sedia ada `subagent-driven-development`: *"Architecture and design tasks:
use the most capable model, not session default."* Dilaksana: `kata` SKILL.md Lv.6 +
`identity-core.md` Geass.

---

## 2026-08-29 — Plan review visual: md+git primary, Artifact HTML eskalasi opsyenal
**Konteks**: Adaptasi konsep "Plan Canvas" ECC ke `work-plan` Lv.3 (semakan plan sebelum
kod). Pilihan awal: render plan `.md` → HTML → `Artifact` publish + comment thread. Kawan
master bagi amaran Artifact "token kuat". Anggaran kos disemak: kitaran review via Artifact
~10–20K token (render + republish tiap pusingan + baca comments + risiko load skill
`artifact-design`), berbanding review terminal ~0.
**Keputusan**: Jadikan **md + git** laluan UTAMA Lv.3 — `cp` plan `.md` ke
`memory/plans/YYYY-MM-DD-<topik>.plan.md`, commit + push, master baca RENDERED di github.com
(mobile), feedback dalam chat, Lucy Edit `.md` + recommit. Kos ~200–500 token/kitaran.
Artifact HTML (`work-plan/references/plan-artifact-template.html`) DIKEKALKAN tetapi
diturunkan jadi **eskalasi opsyenal** — hanya bila master mahu tunjuk-dan-klik pada plan
berdiagram berat. Ditolak: (a) Artifact sebagai laluan lalai; (b) buang template HTML terus.
**Sebab**: GitHub render markdown + Mermaid natively pada mobile — jadi faedah visual utama
Artifact (diagram bertema, akses telefon) dah didapati percuma via git. Feedback master
selama ini konkrit-berteks ("section 3 pecah dua"), bukan tunjuk-dan-klik — memory catat
corak ni berulang. md+git ≈ kos terminal tapi tambah sejarah git + render + akses telefon.
Template HTML disimpan sebagai pintu keluar (dah siap, tak menyusahkan) selaras prinsip
"kod mati dibaiki dengan menghidupkannya bila perlu, bukan dibuang". Dilaksana: `work-plan`
SKILL.md Lv.3 (revisi) + `kata` SKILL.md nota + `reference_ecc_plan_canvas.md`.

---
