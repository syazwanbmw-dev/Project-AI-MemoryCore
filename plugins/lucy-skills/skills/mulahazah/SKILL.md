---
name: mulahazah
description: "MESTI guna bila master cakap 'continuous-improvement', 'instinct status',
             'apa yang kamu pelajari', 'tunjuk rules', 'mulahazah status',
             'pattern apa yang kamu perasan', 'behavioral learning', atau tanya
             tentang pembelajaran sesi, rules terkumpul, atau pattern yang Lucy perasan."
---

# Mulahazah — Sistem Pembelajaran Tingkah Laku
*Lucy yang ingat cara kamu bekerja.*

## Aktivasi

Bila skill ini aktif, output:

`Mulahazah aktif — membaca rules yang dipelajari...`

Kemudian execute protokol di bawah.

## Gambaran Keseluruhan

Mulahazah adalah lapisan pemerhatian pasif dalam Lucy. Ia tangkap corak kerja master, analisa tingkah laku, dan tulis rules yang boleh diambil tindakan ke `~/.claude/mulahazah/rules.md`. Rules tersebut kekal merentasi sesi — secara senyap membentuk cara Lucy bekerja dengan master, tanpa perlu diingatkan setiap kali.

## Context Guard

| Context | Status |
|---------|--------|
| **Master tanya tentang rules atau pattern yang dipelajari** | AKTIF — protokol penuh |
| **Master taip `/continuous-improvement`** | AKTIF — protokol penuh |
| **Master cakap "apa yang Lucy pelajari"** | AKTIF — protokol penuh |
| **Master nak tengok ringkasan rules** | AKTIF — protokol penuh |
| **Conversation coding biasa** | DORMANT — ikut rules.md secara senyap |
| **Master tanya pasal Forge skills** | DORMANT — guna Forge skill sebaliknya |

## Protokol

### Langkah 1: Baca Rules yang Dipelajari

- [ ] Semak `~/.claude/mulahazah/rules.md` — di sini rules yang dipelajari disimpan
- [ ] Jika fail tidak wujud, lapor "Tiada rules lagi. Run `/continuous-improvement` selepas sesi kerja untuk generate rules."
- [ ] Kira dan papar jumlah rules

### Langkah 2: Refleksi Sesi

Generate refleksi untuk sesi ini:

```
## Refleksi — [Tarikh]
- Apa yang berjaya:
- Apa yang gagal:
- Apa yang akan Lucy buat berbeza:
- Rule untuk ditambah:
```

Jika ada "Rule untuk ditambah", semak `~/.claude/mulahazah/rules.md` sama ada sudah wujud. Jika tidak, append.

### Langkah 3: Analisa Pemerhatian (jika diminta atau pada /continuous-improvement)

- [ ] Semak `~/.claude/mulahazah/projects/*/observations.jsonl`
- [ ] Jika 5+ pemerhatian wujud: lapor pattern yang ditemui
- [ ] Lapor apa yang ditemui: rules baru diekstrak, atau "tiada pattern baru lagi"
- [ ] Jika tiada pemerhatian: nota bahawa hooks mungkin tidak dipasang

### Langkah 4: Lapor Status Observer

- [ ] Semak sama ada `~/.claude/mulahazah/observer.pid` wujud
- [ ] Jika berjalan: tunjuk PID dan confirm analisa latar belakang aktif
- [ ] Jika tidak berjalan: nyatakan ia optional

### Langkah 5: Papar Status Rules Penuh

Baca `~/.claude/mulahazah/rules.md` dan papar:
- Jumlah rules
- Rules dikumpul mengikut tarikh ditambah
- Mana-mana rules yang nampak lapuk atau bertentangan (flag untuk semakan master)

## Peraturan Wajib

1. Jangan reka-cipta rules. Hanya lapor rules yang wujud dalam `~/.claude/mulahazah/rules.md`.
2. Jangan reka-cipta pemerhatian — hanya lapor apa yang wujud dalam fail observations.
3. Jika `~/.claude/mulahazah/` tidak wujud, output prompt pemasangan dan berhenti.
4. Jangan auto-apply rules tanpa pengetahuan master — surface dalam langkah refleksi.
5. Bila rules.md wujud dan ada rules, ikut rules tersebut dalam kerja sesi ini.

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| **Tiada rules.md lagi** | Lapor "Tiada rules lagi. Run `/continuous-improvement` selepas sesi pertama." |
| **Mulahazah tidak dipasang** | Output: "Mulahazah belum dipasang. Lihat Feature/Mulahazah-System/install-mulahazah.md untuk mula." |
| **Observer tidak berjalan** | Nota ia optional. Jangan halang pelaksanaan skill. |
| **Kurang dari 5 pemerhatian** | Nota lebih banyak tool calls diperlukan sebelum pattern boleh diekstrak. |

## Sinergi dengan Features Lain

| Feature | Cara Mulahazah Berinteraksi |
|---------|-----------------------------|
| **Forge** | Cluster rules dari rules.md dengan pattern berulang menjadi cadangan Forge skill. |
| **Save Diary** | Rules aktif dari rules.md diringkaskan dalam diari sesi. |
| **Decision Log** | Rules yang dipakai semasa sesi dilog sebagai keputusan tingkah laku. |

## Level History

- **Lv.1** — Base: kekekalan rules.md, refleksi sesi, `/continuous-improvement` command, integrasi Forge + Diary + Decision Log.
