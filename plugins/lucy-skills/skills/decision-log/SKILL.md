---
name: decision-log
description: "Auto-triggers bila keputusan bukan-obvious dibuat dalam conversation.
             Juga triggers bila master cakap 'log keputusan', 'kenapa kita pilih',
             'apa trade-off', 'patut guna A atau B' (selepas resolved), atau tanya
             tentang sebab di sebalik pilihan lepas."
---

# Decision Log — Log Keputusan Arkitek & Teknikal
*Setiap "kenapa" yang penting disimpan, setiap trade-off diingati.*

## Aktivasi

Bila skill ini aktif, baca `main/decisions.md` secara senyap.

- Jika master nak log keputusan: tangkap dan confirm
- Jika master cari keputusan lepas: cari dan bentangkan
- Jika moment keputusan dikesan: tawaran untuk log

## Context Guard

| Context | Status |
|---------|--------|
| **Master cakap "log keputusan"** | AKTIF — tangkap keputusan |
| **Master tanya "kenapa kita pilih [X]?"** | AKTIF — cari decision log |
| **Keputusan bukan-obvious dibuat** | AKTIF — tawaran log |
| **"Patut guna A atau B?" (selepas resolved)** | AKTIF — log pilihan |
| **Hujung sesi ada keputusan tak dilog** | AKTIF — prompt untuk log |
| **Conversation biasa (tiada konteks keputusan)** | DORMANT |

## Protokol

### Bila Keputusan Dikesan
- [ ] Kenalpasti keputusan: apa yang dipilih dan apa yang ditolak
- [ ] Tentukan sama ada bukan-obvious (skip pilihan remeh/obvious)
- [ ] Jika bukan-obvious: tawaran log atau log terus jika master cakap "log keputusan"

### Bila Log Keputusan
- [ ] Dapatkan tarikh semasa (YYYY-MM-DD)
- [ ] Tulis tajuk pendek yang deskriptif
- [ ] Compose entry dalam format:
  ```markdown
  ## YYYY-MM-DD — Tajuk Pendek
  **Konteks**: Situasi apa yang membawa kepada keputusan ini
  **Keputusan**: Apa yang dipilih (dan apa yang ditolak)
  **Sebab**: Kenapa — trade-off, kekangan, bukti
  ```
- [ ] APPEND ke `main/decisions.md` (selepas entry terakhir)
- [ ] Confirm: "Logged: [tajuk]"

### Bila Cari Keputusan
- [ ] Baca `main/decisions.md`
- [ ] Cari keyword yang match dengan soalan master
- [ ] Bentangkan keputusan yang match dengan konteks penuh
- [ ] Jika tiada match: maklumkan master dan tawaran log entry baru

### Bila Balik Keputusan
- [ ] JANGAN edit entry asal
- [ ] APPEND entry baru yang rujuk entry asal:
  ```markdown
  ## YYYY-MM-DD — Balik: [tajuk asal]
  **Konteks**: Kenapa keputusan asal dipertimbangkan semula
  **Keputusan**: Pilihan baru, merujuk yang lama
  **Sebab**: Apa yang berubah — maklumat baru, kekangan, atau keutamaan
  ```

## Peraturan Wajib
1. **Append-only** — JANGAN edit atau delete entry lepas
2. **Konteks + Keputusan + Sebab** — tiga field wajib untuk setiap entry
3. **Sertakan alternatif yang ditolak** — dokumentasi apa yang TIDAK dipilih dan kenapa
4. **Bukan-obvious sahaja** — skip pilihan remeh (guna git, format preferences)
5. **Pengesanan semula jadi** — kesan moment keputusan tanpa perlu arahan explicit

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Tiada decisions.md | Buat fail baru dengan header standard |
| Tiada keputusan yang match | Maklumkan master, tawarian log entry baru |
| Keputusan kabur ("pilih yang lebih baik") | Minta master jelaskan alternatif dan sebab |
| Beberapa keputusan berkaitan | Log setiap satu sebagai entry berasingan dengan cross-reference |
| Balik keputusan lama | Append entry baru dengan prefix "Balik:", jangan edit asal |

## Level History
- **Lv.1** — Base: pengesanan keputusan, append-only logging, format Konteks+Keputusan+Sebab, carian, tracking pembalikan.
