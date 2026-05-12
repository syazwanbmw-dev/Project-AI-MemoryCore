---
name: pengacara-majlis
description: "MUST use when user says 'teks pengacara', 'skrip pengacara', 'pengacara majlis',
             'buat teks MC', 'generate pengacara', 'skrip MC', or describes a majlis and
             needs an emcee script generated."
---

# Pengacara Majlis — Penjana Teks MC
*Jana teks pengacara majlis yang lengkap berdasarkan maklumat yang diberi.*

## Activation

When this skill activates, output:

`"Baik, mari sediakan teks pengacara majlis anda."`

Then proceed to Stage 1.

## Context Guard

| Context | Status |
|---------|--------|
| **User minta teks / skrip pengacara** | ACTIVE — protokol penuh |
| **User describe majlis dan perlukan MC script** | ACTIVE — protokol penuh |
| **Soalan umum tentang majlis (bukan minta teks)** | DORMANT |

## Protocol: 3-Stage Flow

### Stage 1 — Parse Input

Baca semua maklumat yang user bagi. Kenal pasti status setiap info:

| Info | Status |
|------|--------|
| Nama / jenis majlis | Wajib |
| Tarikh & masa majlis | Wajib |
| Atur cara (item + masa) | Wajib — minta kalau tiada |
| Nada (formal / santai) | Wajib — tanya kalau tak disebut |
| Nama pengacara | Optional |
| Nama & jawatan tetamu VIP | Wajib kalau ada VIP |
| Tema majlis | Wajib kalau user minta pantun |

Selepas parse, tentukan:
- Maklumat yang **lengkap** → proceed ke Stage 2 / 3
- Maklumat yang **missing** → tanya satu-satu di Stage 2

### Stage 2 — Clarify (Hanya yang Missing)

- Tanya **satu soalan per mesej** untuk info yang kritikal dan tiada
- **Atur cara adalah paling kritikal** — wajib ada sebelum boleh generate
- Kalau semua maklumat lengkap → skip Stage 2, terus ke Stage 3
- Contoh soalan clarify:
  - "Boleh kongsikan atur cara majlis? (item + masa)"
  - "Formal atau santai?"
  - "Ada tetamu kehormat? Kalau ada, nama dan jawatan?"
  - "Ada tema majlis untuk saya masukkan pantun?"

### Stage 3 — Generate

Jana teks pengacara penuh dalam susunan ini:

---

**1. Ucapan Pembukaan**
- Salam (Assalamualaikum w.b.t. / Selamat pagi/petang/malam)
- Basmallah atau doa pembuka ringkas
- Ucapan selamat datang kepada hadirin

**2. Jemputan Ambil Tempat + Notis Telefon Senyap**
- Minta hadirin mengambil tempat duduk masing-masing
- Minta telefon bimbit diletakkan dalam mod senyap

**3. Umumkan Ketibaan VIP** *(skip kalau tiada VIP)*
- Umumkan ketibaan tetamu kehormat dengan gelaran penuh
- Hadirin diminta berdiri / memberi hormat (ikut protokol)

**4. Atur Cara Item Demi Item**
- Jana teks peralihan untuk setiap item atur cara
- Sertakan masa kalau dibagi oleh user
- Teks peralihan mestilah natural dan tidak berulang

**5. Pantun Tema** *(hanya kalau tema disebut atau diminta)*
- Satu rangkap pantun 4 kerat yang relevan dengan tema majlis
- Letakkan di bahagian yang sesuai (biasanya selepas ucapan utama)

**6. Ucapan Penutup**
- Ucapan terima kasih kepada semua pihak
- Doa selamat / doa penutup
- Penutup rasmi majlis

---

## Mandatory Rules

1. **Jangan reka maklumat** — nama VIP, tempat, tarikh mestilah dari user; guna placeholder kalau tiada
2. **Pantun hanya bila ada tema** — jangan reka atau assume tema sendiri
3. **Satu clarify per mesej** — jangan tanya banyak soalan sekaligus
4. **Output satu blok penuh** — generate keseluruhan teks sekaligus, bukan bahagian-bahagian
5. **Nada konsisten** — formal kekal formal, santai kekal santai dari mula hingga akhir
6. **Placeholder untuk info optional yang tiada** — guna `[Nama Pengacara]`, `[Nama VIP]`, `[Jawatan]`

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Atur cara tak dibagi | Tanya balik — ini kritikal, tak boleh generate tanpa ia |
| Tiada VIP / tetamu kehormat | Skip bahagian ketibaan VIP sepenuhnya |
| Tiada tema majlis | Skip pantun — jangan reka tema |
| Nada tak disebut | Tanya: "Formal atau santai?" sebelum generate |
| Majlis keagamaan | Pastikan doa pembuka & penutup ada, guna bahasa yang sesuai |
| Nama pengacara tak dibagi | Guna `[Nama Pengacara]` sebagai placeholder |
| Masa atur cara tak lengkap | Tanya sama ada nak masa dimasukkan atau teruskan tanpa masa |
| User minta ubah bahagian tertentu | Edit bahagian tersebut sahaja, kekalkan yang lain |

## Contoh Input Minimum

```
Majlis: Hari Sukan Sekolah
Tarikh: 15 Mei 2026, 8.00 pagi
Atur cara:
  8.00 - Ketibaan tetamu
  8.30 - Ucapan pengetua
  9.00 - Acara sukan bermula
  12.00 - Majlis tamat
Nada: Formal
VIP: YBhg. Dato' Hj. Ahmad, Pegawai Pelajaran Daerah
```

## Level History

- **Lv.1** — Base: Parse input majlis, 3-stage flow (Parse→Clarify→Generate), 6-bahagian output, 4 jenis majlis, pantun optional, nada pilihan user. (Origin: Brainstorming session, 2026-05-12)
