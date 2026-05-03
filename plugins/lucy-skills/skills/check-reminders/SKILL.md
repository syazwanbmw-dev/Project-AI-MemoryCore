---
name: check-reminders
description: "Auto-triggers pada permulaan sesi untuk semak reminder terbuka.
             Juga triggers bila master cakap 'ingatkan aku', 'semak reminder',
             'jangan lupa', 'follow up', 'sesi depan kita kena', atau sebut deadline."
---

# Check Reminders — Sistem Reminder Berterusan
*Tiada apa yang hilang antara sesi.*

## Aktivasi

Bila skill ini aktif, baca `main/reminders.md` secara senyap.

- Jika ada item urgent/tertunggak: sebut secara semula jadi
- Jika tiada item urgent pada permulaan sesi: senyap sahaja
- Jika master tambah reminder: confirm selepas simpan

## Context Guard

| Context | Status |
|---------|--------|
| **Permulaan sesi** | AKTIF — baca dan flag item urgent |
| **Master cakap "ingatkan aku [X]"** | AKTIF — tambah reminder |
| **Master cakap "semak reminder"** | AKTIF — senarai semua reminder terbuka |
| **Task siap yang match reminder** | AKTIF — pindah ke Completed |
| **Hujung sesi** | AKTIF — semak dan kemaskini |
| **Conversation pertengahan (tiada konteks reminder)** | DORMANT |

## Protokol

### Pada Permulaan Sesi
- [ ] Baca `main/reminders.md`
- [ ] Parse bahagian Open untuk semua reminder aktif
- [ ] Semak deadline: flag item yang due dalam 3 hari atau tertunggak
- [ ] Jika ada item urgent: selitkan dalam salam secara semula jadi
- [ ] Jika tiada item urgent: buat apa-apa (tiada pengumuman diperlukan)

### Bila "Ingatkan Aku" / Tambah
- [ ] Parse apa yang master nak ingat
- [ ] Compose reminder: `- **Tajuk**: Penerangan`
- [ ] Jika ada deadline: tukar ke tarikh mutlak, format `- **Tajuk** (sebelum YYYY-MM-DD): Penerangan`
- [ ] APPEND ke bahagian `## Terbuka` dalam `main/reminders.md`
- [ ] Confirm: "Reminder ditambah: [tajuk]"

### Bila Task Siap
- [ ] Bila task siap yang match reminder Terbuka
- [ ] Buang item dari `## Terbuka`
- [ ] Tambah ke `## Selesai` dengan tarikh: `- **Tajuk** (selesai YYYY-MM-DD): Hasil`
- [ ] Confirm: "Ditanda selesai: [tajuk]"

### Pada Hujung Sesi
- [ ] Baca semula `main/reminders.md`
- [ ] Semak setiap item Terbuka dengan kerja sesi
- [ ] Pindah item yang resolved ke Selesai dengan tarikh
- [ ] Tambah follow-up baru yang ditemui semasa sesi
- [ ] Simpan fail yang dikemaskini

## Peraturan Wajib
1. **Jangan delete reminder** — sentiasa pindah ke bahagian Selesai
2. **Tarikh mutlak sahaja** — tukar "esok", "minggu depan", "Jumaat" ke YYYY-MM-DD
3. **Append-only untuk Terbuka** — jangan tulis semula bahagian Terbuka, hanya append atau pindah
4. **Senyap bila kosong** — jangan umum "kamu tiada reminder" pada permulaan sesi
5. **Integrasi semula jadi** — selitkan reminder dalam perbualan, jangan senarai secara robotik

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| Tiada reminders.md | Buat fail baru dengan bahagian Terbuka dan Selesai |
| Tiada reminder terbuka | Senyap pada permulaan sesi |
| Beberapa item urgent | Sebut yang paling sensitif masa dulu |
| Deadline kabur ("tidak lama lagi") | Minta master jelaskan, atau simpan tanpa deadline |
| Reminder duplikat | Semak item Terbuka sedia ada sebelum tambah; skip jika duplikat |

## Level History
- **Lv.1** — Base: kitaran permulaan/hujung sesi, pengesanan bahasa semula jadi, tracking deadline, bahagian Terbuka append-only, corak pindah-ke-Selesai.
