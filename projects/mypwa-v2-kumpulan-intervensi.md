# Idea — Kumpulan Intervensi (mypwa-v2)

> ⚠️ **FAIL INI SUDAH DIGANTI — 2026-08-08.** Brainstorm penuh selesai; spec diluluskan
> ada dalam repo: **`mypwa-v2/docs/superpowers/specs/2026-08-08-kumpulan-intervensi-design.md`**
> (commit `a8d3fd2`, branch `test`).
>
> Fail ini disimpan sebagai **rekod idea asal sahaja**. Senarai *"Soalan yang BELUM dijawab"*
> di bawah **sudah dijawab semua** — jangan baca ia sebagai kerja tertunggak.

*Bincang santai dengan master, 2026-08-06 malam (23:45–23:58).*

---

## Masalah sebenar dari lapangan

**MODUL LATIHAN 1:** murid tahun 4 duduk ikut kelas asal — 4 DELIMA, 4 TOPAZ, 4 ZAMRUD.

**MODUL LATIHAN 2 dan seterusnya:** sekolah jalankan program intervensi. Murid tahun 4 **disusun semula ikut tahap**, merentas kelas. Kumpulan baharu diberi nama ikut bilik: *Intervensi 4 DELIMA*, *Intervensi 4 TOPAZ*, *Intervensi 4 ZAMRUD*.

**Skop:** 4 subjek peperiksaan matriks pembelajaran tahun 4 — **BM, BI, Sains, Matematik**. Keempat-empatnya guna pengumpulan yang **sama**.

**Kesakitan cikgu:** untuk isi markah satu kumpulan intervensi, cikgu terpaksa buka tiga kelas asal satu-satu dan cari nama. Master nak satu skrin sahaja.

**Disahkan master:** pengumpulan itu **BERCAMPUR** (opsyen A) — satu kumpulan ada murid dari ketiga-tiga kelas asal.

---

## 🔴 BAHAYA — kenapa "tukar kelas murid" adalah jawapan yang SALAH

Cara paling semula jadi ialah cipta kelas baharu dan pindahkan murid ke situ. **Jangan.**

`murid.id_kelas` ialah **keadaan semasa**, satu nilai — bukan sejarah. Laporan tidak menyimpan "murid ini dalam kelas mana semasa ujian itu"; ia **tanya semula, sekarang**. Jadi sebaik murid dipindahkan:

- Markah **MODUL LATIHAN 1** dia berpindah sekali ke kelas baharu
- Purata kelas asal berubah · ranking berubah · taburan gred berubah
- **Tiada ralat, tiada amaran** — laporan lama senyap-senyap menulis semula sejarah

Dan ironinya: MODUL 1 itulah **garis dasar "sebelum intervensi"**. Merosakkannya bermakna hilang satu-satunya ukuran untuk tahu program itu berkesan.

Sekeluarga dengan [[feedback_nilai_terbitan_arkib]] — nilai yang sepatutnya beku tetapi dikira semula setiap kali dipandang.

---

## Idea: dua konsep berbeza, jangan paksa masuk satu medan

| | Maksud | Berubah? |
|---|---|---|
| **Kelas** | Identiti murid — rekod sekolah, slip, PAJSK | Setahun sekali |
| **Kumpulan** | Siapa belajar bersama untuk tujuan tertentu | Ikut keperluan |

Intervensi **bukan** tukar kelas. Ia kumpulan belajar. Ahmad kekal murid 4 DELIMA selama-lamanya; dia *juga* ahli Intervensi 4 TOPAZ.

Bila dua konsep dipaksa berkongsi satu medan, kita terpaksa pilih antara **merosakkan sejarah** atau **menyusahkan cikgu**. Itu sebabnya masalah ini terasa tiada jalan keluar bersih.

### Bentuk kasar

`murid.id_kelas` **tidak pernah berubah**. Tambah konsep Kumpulan di sebelahnya (many-to-many: satu murid, satu kumpulan aktif; satu kumpulan, ramai murid dari pelbagai kelas).

Aliran cikgu — satu dropdown tambahan di tempat yang sama:

```
ujian.html → pilih Ujian
           → [ 4 DELIMA ▾ ]  atau  [ ✦ Intervensi 4 TOPAZ ▾ ]
           → Cari → murid terus keluar, dari mana-mana kelas
```

Cikgu tidak perlu belajar konsep baharu. Slip, PAJSK, senarai murid semua kekal guna kelas asal.

---

## ⚠️ Bahagian yang senang terlepas pandang

`jadual_guru` (tab **Kelas Saya**) ialah cara sistem tentukan cikgu nampak murid mana — cikgu declare *kelas + subjek*.

Kalau murid bercampur, `jadual_guru` **mesti boleh terima kumpulan juga**. Kalau terlepas, gejalanya: *"cikgu tekan Cari, senarai kosong"* — dan puncanya tidak jelas langsung kepada pengguna.

Perlu juga fikir: murid yang **tiada** dalam mana-mana kumpulan mesti tetap boleh diisi markah melalui kelas asal (fallback).

---

## 🎁 Nilai bonus yang lebih besar daripada masalah asal

Sebab kumpulan disusun **ikut tahap**, dan tahap datang dari markah, sistem sudah ada semua yang perlu untuk menjawab:

> **Program intervensi ini berkesan tak?**

Banding purata kumpulan yang **sama** antara MODUL 1 (sebelum disusun) dan MODUL 3 (selepas). Ukuran sebenar, bukan perasaan.

🔑 Ini **hanya mungkin** kalau markah MODUL 1 kekal utuh — iaitu tepat perkara yang musnah kalau murid dipindahkan antara kelas. **Dua sebab berbeza, satu keputusan yang sama.**

Idea lanjut (belum dibincang): sebab markah MODUL 1 sudah ada dalam DB, sistem boleh **cadangkan** pengumpulan awal secara automatik ikut tahap.

---

## Soalan yang dahulu belum dijawab — SEMUA SUDAH DIJAWAB (2026-08-08)

| Soalan asal | Jawapan master |
|---|---|
| 1. Semua murid masuk kumpulan? | Ya bila intervensi aktif — kelas asal **disekat** untuk subjek itu. Murid tertinggal jadi tak boleh diisi, jadi admin dapat amaran "Belum Berkumpulan" |
| 2. Terikat pada sesi atau ujian? | **Ujian.** Suis `guna_kumpulan` pada `ujian_item` (ujian × tahun × subjek) |
| 3. Siapa urus keahlian? | **Admin sahaja** — termasuk menetapkan guru pada kumpulan |
| 4. Laporan khas? | **Ya** — suis "Papar ikut Kelas Asal / Kumpulan" pada laporan-ujian, dashboard, trend |

**Keputusan tambahan yang tidak pernah ada dalam senarai asal:**
- Sejarah dikunci pada **ujian**, bukan tarikh — `ujian` tiada tarikh tunggal yang boleh dipakai
- Cap `ujian_markah.kumpulan_id` ditulis auto masa Simpan, dilindungi `COALESCE`
- **Trend** guna keahlian **semasa** (bukan cap) supaya banding MODUL 1 lawan MODUL 3 berfungsi
- Lubang kebenaran `POST /api/ujian-markah/bulk` ditutup sekali dalam kerja ini

## Status

**Spec siap & diluluskan master, belum ada pelan pelaksanaan.**
Langkah seterusnya: `writing-plans` → pelan → TDD.
