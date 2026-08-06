# Idea — Kumpulan Intervensi (mypwa-v2)

*Bincang santai dengan master, 2026-08-06 malam (23:45–23:58). BELUM brainstorm penuh, BELUM ada spec/pelan. Sambung selepas kerja XSS selesai.*

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

## Soalan yang BELUM dijawab

1. Semua murid tahun 4 masuk kumpulan, atau sesetengah sahaja kekal dalam kelas asal?
2. Kumpulan terikat pada **sesi** atau pada **ujian**? (Susunan berubah antara MODUL 2 dan MODUL 3?)
3. Siapa urus keahlian kumpulan — admin sahaja, atau guru juga?
4. Perlu laporan/analisis khas ikut kumpulan, atau guna semula paparan sedia ada?

## Status

**Idea sahaja.** Belum brainstorm penuh, belum spec, belum pelan.
**Aturan kerja:** habiskan XSS silang-keistimewaan (Task 3–7) dahulu, kemudian brainstorm penuh untuk ini.
