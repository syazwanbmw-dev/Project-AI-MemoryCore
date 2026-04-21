# Print Table Row Chunking
*Wrap lajur jadual yang terlalu lebar ke baris ke-2 untuk cetakan A4*

## Overview

### What is Print Table Row Chunking?
Apabila jadual HTML mempunyai terlalu banyak lajur untuk muat dalam satu halaman cetak (A4 landscape), data dibahagikan kepada "chunk" dengan bilangan lajur tetap per baris. Data baris ke-2 diletakkan tepat di bawah baris ke-1 dalam lajur yang sama, menggunakan HTML `rowspan` untuk menyatukan sel Peserta/Terbaik/Kedudukan.

### When to Use
- Jadual cetak dengan lajur dinamik yang boleh melebihi lebar halaman A4
- Data berbentuk "cubaan" atau "percubaan" (C1/C2/C3) per item
- Bilangan lajur tidak tetap (bergantung pada input pengguna)

---

## Architecture Diagrams

### Layout: ≤ MAX_COLS heights (satu baris data)

```
┌──────────┬────────────┬────────────┬────────────┬─────────┬─────────┐
│ Peserta  │   H1       │   H2  ...  │   H10      │Terbaik  │  Ked    │
│ (rs=2)   ├────┬───┬───┼────┬───┬───┼────┬───┬───┤ (rs=2)  │ (rs=2)  │
│          │ C1 │C2 │C3 │ C1 │C2 │C3 │ C1 │C2 │C3 │         │         │
├──────────┼────┼───┼───┼────┼───┼───┼────┼───┼───┼─────────┼─────────┤
│  Ali     │ O  │ · │ · │ X  │ · │ · │ O  │ · │ · │ 1.40m   │    1    │
└──────────┴────┴───┴───┴────┴───┴───┴────┴───┴───┴─────────┴─────────┘
```

### Layout: > MAX_COLS heights (dua baris data, wrap ke baris ke-2)

```
┌──────────┬────────────┬────────────┬────────────┬─────────┬─────────┐
│ Peserta  │  H1        │  H2  ...   │  H10       │Terbaik  │  Ked    │
│ (rs=3)   ├────────────┼────────────┼────────────┤ (rs=3)  │ (rs=3)  │
│          │  H11       │  H12  ...  │  H20 / —   │         │         │
│          ├────┬───┬───┼────┬───┬───┼────┬───┬───┤         │         │
│          │ C1 │C2 │C3 │ C1 │C2 │C3 │ C1 │C2 │C3 │         │         │
├──────────┼────┼───┼───┼────┼───┼───┼────┼───┼───┼─────────┼─────────┤
│  Ali     │ O  │ · │ · │ X  │ · │ · │ O  │ · │ · │ 1.60m   │    1    │
│(rowspan) │ X  │ · │ · │ ·  │ · │ · │    │   │   │(rowspan)│(rowspan)│
└──────────┴────┴───┴───┴────┴───┴───┴────┴───┴───┴─────────┴─────────┘

— = placeholder kosong (height tidak wujud)
```

### Struktur Header (hasChunk2 = true)

```
headRow1: Peserta(rs=3) | H1...H10 (colspan=3 each) | Terbaik(rs=3) | Ked(rs=3)
headRow2:               | H11..H20 (colspan=3, '—' jika tiada)
headRowC:               | C1 C2 C3 × MAX_COLS  ← sekali sahaja, tidak diulang
```

---

## Key Concepts

### 1. Chunk Slicing
```javascript
const MAX_COLS = 10;
const chunk1 = allItems.slice(0, MAX_COLS);
const chunk2 = allItems.slice(MAX_COLS, MAX_COLS * 2);
const hasChunk2 = chunk2.length > 0;
```

### 2. Header dengan Placeholder
```javascript
// H11-H20: tunjuk label, '—' jika tiada item
const headRow2 = hasChunk2
    ? `<tr>${Array.from({length: chunk1.length}, (_, i) => {
        const item = chunk2[i];
        return item !== undefined
            ? `<th colspan="3">${item.label}</th>`
            : `<th colspan="3" style="color:#bbb">—</th>`;
      }).join('')}</tr>`
    : '';
```

### 3. Data Cells dengan Padding (getChunkCells)
```javascript
// padTo: isi dengan sel kosong jika chunk lebih pendek dari MAX_COLS
function getChunkCells(tr, chunkItems, startIdx, padTo) {
    let cells = '';
    for (let i = 0; i < padTo; i++) {
        if (i < chunkItems.length) {
            for (let c = 0; c < 3; c++) {
                const btn = tr.querySelector(`[data-hidx="${startIdx + i}"][data-cidx="${c}"]`);
                const state = btn?.dataset.state || '';
                cells += `<td>${state || '·'}</td>`;
            }
        } else {
            cells += '<td></td><td></td><td></td>';  // padding kosong
        }
    }
    return cells;
}
```

### 4. Body Rows dengan Rowspan
```javascript
const bodyRowspan = hasChunk2 ? 2 : 1;
const headerRowspan = hasChunk2 ? 3 : 2;

// Row 1: chunk1 data + rowspan untuk Peserta/Terbaik/Ked
rowsHTML += `<tr>
    <td rowspan="${bodyRowspan}">${nama}</td>
    ${getChunkCells(tr, chunk1, 0, chunk1.length)}
    <td rowspan="${bodyRowspan}">${terbaik}</td>
    <td rowspan="${bodyRowspan}">${rank}</td>
</tr>`;

// Row 2: chunk2 data (padded) — hanya jika hasChunk2
if (hasChunk2) {
    rowsHTML += `<tr>${getChunkCells(tr, chunk2, MAX_COLS, chunk1.length)}</tr>`;
}
```

---

## Tech Stack Options

| Component | Cara |
|-----------|------|
| Rendering | Vanilla JS — generate HTML string |
| Styling | Inline CSS dalam print window |
| Print output | `window.open()` → `document.write()` → `window.print()` |
| Data source | DOM query (`.querySelector`) semasa runtime |

---

## Benefits

| Benefit | Huraian |
|---------|---------|
| Lebar tetap | Jadual tidak overflow halaman cetak |
| Label jelas | H11-H20 ada label dalam header, bukan baris data tanpa nama |
| Padding kosong | Kolum sentiasa sejajar walaupun chunk2 tidak penuh |
| C1/C2/C3 sekali | Sub-header cubaan tidak diulang, lebih bersih |

## When NOT to Use
- Jika bilangan lajur sentiasa tetap (guna jadual biasa)
- Jika data tidak berbentuk "percubaan" (C1/C2/C3) per item
- Jika lebih dari 2 wrap diperlukan (chunk3+) — pertimbang layout berbeza

---

## Reference Implementation

### sistem-olahraga-sekolah (2026)
- Fungsi: `cetakKeputusanLompat()` dalam `public/keputusan.js`
- MAX_COLS = 10 ketinggian per baris, max 20 ketinggian total
- `data-hidx` dan `data-cidx` sebagai key untuk query DOM buttons
- Hard cap pada 20 — lebih 20 ketinggian dalam lompat tinggi tidak realistik

---
*Documented: 2026-04-09*
*Based on: sistem-olahraga-sekolah — cetakKeputusanLompat()*
