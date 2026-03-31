---
name: save-memory
description: "MUST use when user says 'save', 'save memory', 'save progress',
             'Lucy simpan', 'update memory', 'simpan dalam memory', or when
             important information needs to be preserved to memory files."
---

# Save Memory — Lucy Memory Preservation
*Simpan maklumat penting ke dalam fail memory Lucy*

## Activation

When this skill activates, output:

`💾 Menyimpan memory...`

Then execute the protocol below.

## Protocol

### Step 1: Kenal Pasti Apa Yang Perlu Disimpan
- [ ] Semak perbualan semasa untuk maklumat penting
- [ ] Kenal pasti preferens, keputusan, atau konteks yang bernilai
- [ ] Tentukan fail memory mana yang perlu dikemaskini

### Step 2: Kemaskini Fail Memory
- [ ] Update `main/relationship-memory.md` dengan preferens atau maklumat baru tentang master
- [ ] Update `main/current-session.md` dengan konteks kerja semasa
- [ ] Tambah nota growth jika ada perkembangan hubungan yang significant

### Step 3: Sahkan
- [ ] Paparkan ringkasan apa yang disimpan
- [ ] Sahkan semua fail berjaya dikemaskini

### Step 4: Auto-Commit & Push ke GitHub
- [ ] `cd` ke direktori memory (`C:\Users\user\Documents\code\memory`)
- [ ] Run `git add` untuk fail yang dikemaskini sahaja (bukan `git add -A`)
- [ ] Commit dengan message ringkas: `Memory update: [nama fail yang diubah]`
- [ ] Push ke `master` branch: `git push origin master`
- [ ] Confirm push berjaya

## Mandatory Rules

1. Hanya simpan maklumat yang benar-benar penting — bukan setiap detail perbualan
2. Jaga kandungan sedia ada — tambah atau kemaskini, jangan overwrite tanpa sebab
3. Beritahu master apa yang disimpan selepas selesai
4. Setiap kali memory disimpan, WAJIB auto-commit dan push ke GitHub — tiada memory update tanpa backup

## Edge Cases

| Situasi | Tindakan |
|---------|----------|
| **Tiada maklumat baru** | Beritahu master tiada yang perlu disimpan |
| **Master minta simpan sesuatu yang spesifik** | Simpan terus tanpa soal |

## Level History

- **Lv.1** — Base: Simpan maklumat perbualan ke fail memory atas arahan. (Origin: Fasa 1 install, 2026-03-26)
