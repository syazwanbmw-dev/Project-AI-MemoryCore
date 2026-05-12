# Design Spec: Skill `pengacara-majlis`
*Tarikh: 2026-05-12 | Status: Approved*

## Tujuan

Skill untuk menjana teks pengacara majlis yang lengkap berdasarkan maklumat yang diberi oleh user. Menggantikan proses manual menulis skrip MC dari kosong.

## Skop

Meliputi 4 jenis majlis:
- Majlis Rasmi Sekolah (Perhimpunan, Hari Anugerah, Perasmian)
- Majlis Sukan / Olahraga (Hari Sukan, Pertandingan, Kejohanan)
- Majlis Keagamaan (Maulid, Khatam Al-Quran, Doa Selamat)
- Majlis Sosial / Umum (Kenduri, Perkahwinan, Persaraan)

## Trigger Phrases

- `teks pengacara`, `skrip pengacara`, `pengacara majlis`
- `buat teks MC`, `generate pengacara`, `skrip MC`

## Protocol: 3-Stage Flow

### Stage 1 — Parse Input

AI baca semua maklumat yang user bagi dan kenal pasti:

| Info | Status |
|------|--------|
| Nama / jenis majlis | Wajib |
| Tarikh & masa | Wajib |
| Atur cara (item + masa) | Wajib — minta kalau tiada |
| Nada (formal / santai) | Wajib — tanya kalau tak disebut |
| Nama pengacara | Optional |
| Nama & jawatan tetamu VIP | Wajib kalau ada VIP |
| Tema majlis | Wajib kalau nak pantun |

### Stage 2 — Clarify

- Tanya **satu soalan per mesej** untuk maklumat yang missing
- Kalau semua maklumat lengkap → skip terus ke Stage 3
- Atur cara adalah maklumat paling kritikal — wajib ada sebelum generate

### Stage 3 — Generate

Jana teks penuh dalam susunan ini:

1. **Ucapan pembukaan** — salam, Basmallah/doa pembuka
2. **Jemputan ambil tempat + notis telefon senyap**
3. **Umumkan ketibaan VIP** (skip kalau tiada VIP)
4. **Atur cara item demi item** dengan teks peralihan antara setiap item
5. **Pantun tema** (hanya kalau tema disebut atau diminta)
6. **Ucapan penutup** — terima kasih, doa selamat, penutup rasmi

## Rules Mandatori

1. **Jangan reka maklumat** — nama VIP, tempat, tarikh mestilah dari user
2. **Pantun hanya bila ada tema** — jangan reka tema sendiri
3. **Satu clarify per mesej** — jangan tanya banyak soalan sekaligus
4. **Output satu blok penuh** — generate keseluruhan teks, bukan bahagian-bahagian
5. **Nada konsisten** — formal kekal formal dari mula hingga akhir majlis
6. **Placeholder untuk info missing** — kalau nama pengacara tak dibagi, guna `[Nama Pengacara]`

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Atur cara tak dibagi | Tanya balik — kritikal, tak boleh generate tanpa ia |
| Tiada VIP | Skip bahagian ketibaan VIP sepenuhnya |
| Tiada tema majlis | Skip pantun |
| Nada tak disebut | Tanya: "Formal atau santai?" sebelum generate |
| Majlis keagamaan | Pastikan doa pembuka & penutup ada |
| Nama pengacara tak dibagi | Guna `[Nama Pengacara]` sebagai placeholder |
| Masa atur cara tak lengkap | Tanya sama ada nak masa dimasukkan atau tidak |

## Lokasi Skill

`C:\Users\user\Documents\code\memory\plugins\lucy-skills\skills\pengacara-majlis\SKILL.md`
