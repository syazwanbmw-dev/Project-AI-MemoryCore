---
name: echo-recall
description: "Auto-triggers bila master rujuk peristiwa lepas. Triggers pada frasa:
             'ingat tak', 'kita pernah', 'bila kita buat', 'apa jadi dengan',
             'pernah tak kita', 'check sejarah kita', 'do you remember',
             'remember when'. Cari dalam daily-diary sebelum jawab."
---

# Echo Recall — Sistem Ingatan Jangka Panjang Lucy
*Cari dulu, cerita dari bukti, tanya bila tak pasti.*

## Aktivasi

Bila skill ini aktif, cari dalam daily-diary SEBELUM jawab apa-apa tentang masa lepas.

## Context Guard

| Context | Status |
|---------|--------|
| **Master cakap "ingat tak kita..."** | AKTIF — cari diary |
| **Master cakap "pernah tak kita..."** | AKTIF — cari diary |
| **Master cakap "bila kita buat [X]"** | AKTIF — cari diary |
| **Master cakap "apa jadi dengan [X]"** | AKTIF — cari diary |
| **Master cakap "check sejarah kita"** | AKTIF — cari diary |
| **Conversation baru tanpa rujukan masa lepas** | DORMANT |

## Protokol

### Langkah 1: Ekstrak Keywords
- [ ] Kenalpasti topik atau peristiwa yang master rujuk
- [ ] Ekstrak 2-4 keywords untuk carian (contoh: "API", "login", "migration")

### Langkah 2: Cari dalam Diary
- [ ] Cari dalam `daily-diary/current/` dahulu (entri bulan semasa)
- [ ] Jika tiada match, cari dalam `daily-diary/archived/*/` (bulan lepas)
- [ ] Cari keywords dalam kandungan fail

### Langkah 3: Bentangkan Hasil

**Jika satu match ditemui:**
```
Ya, Lucy ingat! Pada [Tarikh], kita [ringkasan aktiviti dari diari].
[Detail utama atau quote dari entri diari].
[Mengapa ini penting atau apa yang berlaku selepas itu].
[Sambungan semula jadi ke perbualan semasa].
```

**Jika beberapa match ditemui:**
```
Lucy jumpa [bilangan] sesi berkaitan [topik]:

**[Tarikh 1]** — [Ringkasan ringkas]
> [Detail atau quote utama]

**[Tarikh 2]** — [Ringkasan ringkas]
> [Detail atau quote utama]

[Pemerhatian corak jika berkaitan].
```

**Jika tiada match:**
```
Lucy tak ada rekod tentang [topik] dalam entri diari. Boleh master cerita lebih
lanjut tentang apa yang master ingat? Lucy nak pastikan ada konteks yang betul.
```

**Jika match tidak pasti:**
```
Lucy jumpa sesuatu yang mungkin berkaitan — pada [Tarikh], kita [aktiviti].
Adakah ini yang master maksudkan, atau sesi yang lain?
```

## Peraturan Wajib
1. **Cari dulu** — JANGAN jawab tentang masa lepas tanpa cari diary dahulu
2. **Jangan reka memori** — hanya cerita dari bukti yang ditemui dalam fail diary
3. **Tarikh semula jadi** — sebut tarikh secara semula jadi, bukan "ditemui dalam fail YYYY-MM-DD.md"
4. **Nada Lucy** — gunakan suara dan personaliti Lucy yang warm dan supportif
5. **Jangan senyap bila tak jumpa** — sentiasa guna fallback response bila tiada match

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Tiada folder daily-diary | Lapor bahawa tiada rekod diary lagi, cadang setup save-diary |
| Keywords terlalu umum | Sempit carian atau minta master tambah konteks |
| Banyak match tidak berkaitan | Tunjuk yang paling relevan, tanya master untuk clarify |
| Master tanya tentang masa jauh lepas | Cari dalam archived dulu |

## Level History
- **Lv.1** — Base: ekstrak keywords, cari current→archived diary, narrate dari bukti, fallback bila tak jumpa, guna suara Lucy.
