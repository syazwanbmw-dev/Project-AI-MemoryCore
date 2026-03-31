# Daily Diary Protocol
*Format rujukan untuk entri diary Lucy — rujukan tetap, jangan ubah*

---

## Struktur Entri

Setiap entri diary mengikut format ini:

```markdown
## [Tarikh] ([Masa] - [Pagi/Petang/Malam]) - [Tajuk Sesi]

### Ringkasan Sesi
- **Tarikh:** [YYYY-MM-DD]
- **Masa:** [HH:MM AM/PM]
- **Jenis Sesi:** [Kerja Kod / Belajar / Perbincangan / Debugging]
- **Projek:** [Nama projek atau "Umum"]

### Topik Utama
- [Topik 1 — apa yang dibincangkan atau dibuat]
- [Topik 2]
- [Topik 3]

### Pencapaian & Pembelajaran
- [Apa yang berjaya disiapkan]
- [Apa yang dipelajari]
- [Masalah yang diselesaikan]

### Keputusan Penting
- [Keputusan teknikal atau reka bentuk yang dibuat]

### Momen Berkesan
- [Breakthrough, idea baru, atau kerjasama yang berkesan]

### Langkah Seterusnya
- [ ] [Task seterusnya]
- [ ] [Apa yang perlu diselesaikan]

---
```

---

## Peraturan Penulisan

1. **APPEND sahaja** — jangan overwrite entri yang sedia ada
2. **Satu fail per hari** — berbilang entri dipisahkan oleh `---`
3. **Bukti sebenar** — dokumenkan kandungan sesi yang nyata, bukan ringkasan generik
4. **Timestamp sebenar** — sentiasa dapatkan masa semasa via system command
5. **Archive dulu** — semak monthly archive sebelum setiap tulis baru

## Kitaran Archive Bulanan

```
Bila "save diary" dicall:
  1. Scan daily-diary/current/ untuk fail dari bulan lepas
  2. Kalau ada → cipta daily-diary/archived/YYYY-MM/
  3. Pindahkan fail bulan lepas ke archived/YYYY-MM/
  4. Teruskan tulis entri baru dalam current/
```

## Penamaan Fail

- Format: `YYYY-MM-DD.md`
- Contoh: `2026-03-26.md`
- Satu fail per hari, semua entri hari tu dalam fail yang sama

---

*Daily Diary Protocol v1.0 — Lucy Memory System*
