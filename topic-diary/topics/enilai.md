# eNilai (mypwa-v2)
*Created: 2026-06-10*

---

## [19:07, Rabu, 10 Jun 2026] — Auto-Tutup Ujian by Date + Bug Fix Kedudukan Slip

**Context:** Projek mypwa-v2 (eNilai) — sistem pengurusan penilaian murid sekolah rendah, Cloudflare Workers + D1. Sesi 2026-06-10 petang.

**Discovery:**

### 1. Bug Fix — Kedudukan Keseluruhan Slip Keputusan
- **Bug:** `kiraRankingSlip` guna `purata` (average) untuk sort, tapi table KED guna `jumlah_A DESC + jumlah_markah DESC` → rank tidak konsisten
- **Bug 2:** `rankKesel` kira semua murid semua tahun → sepatutnya rank dalam tahun yang sama sahaja
- **Fix backend:** Tambah `k.tahun AS tahun_kelas` dalam SELECT laporan, expose dalam response
- **Fix frontend:** Sort by `jumlah_A DESC + jumlah_markah DESC`, group `rankKesel` by `tahun`

### 2. Feature — Auto-Tutup Ujian by Date
- **Konsep:** Lazy check on-the-fly, tiada cron job. Semakan berlaku setiap request
- **Migration 024:** `ALTER TABLE ujian ADD COLUMN tarikh_tutup TEXT`
- **Aturan tarikh:** `today > tarikh_tutup` = BLOCK (403). Hari sama = DIBENARKAN
- **GET filter guru:** `WHERE u.status = 'buka' AND (u.tarikh_tutup IS NULL OR u.tarikh_tutup >= '${today}')`
- **PUT atomicity:** `db.batch()` dengan dua statement — pattern `'tarikh_tutup' in body` untuk detect clear-ke-NULL vs tidak-update
- **Frontend badge:** `effectiveStatus(u)` — 4 state: Ditutup / Ditutup (Auto) / Buka·tarikh / Buka

**Details:**

```javascript
// Pattern untuk detect clear vs skip dalam PUT
if ('tarikh_tutup' in body) {
  stmts.push(
    c.env.DB.prepare('UPDATE ujian SET tarikh_tutup = ? WHERE id = ?')
      .bind(body.tarikh_tutup ?? null, id)
  );
}

// Frontend effectiveStatus
function effectiveStatus(u) {
  const today = new Date().toLocaleDateString('en-CA', { timeZone: 'Asia/Kuala_Lumpur' });
  if (u.status === 'tutup') return { label: 'Ditutup', cls: 'badge-gray' };
  if (u.tarikh_tutup && today > u.tarikh_tutup) return { label: 'Ditutup (Auto)', cls: 'badge-amber' };
  if (u.tarikh_tutup) return { label: `Buka · ${u.tarikh_tutup}`, cls: 'badge-green' };
  return { label: 'Buka', cls: 'badge-green' };
}
```

**Pitfalls:**
- Migration mesti applied ke DB SEBELUM kod baru deploy — elak window "Ralat server" bila kolum belum ada
- `'tarikh_tutup' in body` (bukan `body.tarikh_tutup`) — satu-satunya cara detect explicit `null` vs field tidak dihantar
- Staging DB boleh jadi tidak sync dengan session notes — sentiasa verify via `PRAGMA table_info(table)` sebelum test
- `wrangler d1 execute` memerlukan `--remote` flag untuk hit remote DB (bukan local)
- Tarikh Malaysia: guna `toLocaleDateString('en-CA', { timeZone: 'Asia/Kuala_Lumpur' })` untuk dapat `YYYY-MM-DD` dalam timezone betul

**Keywords:** enilai, mypwa-v2, cloudflare-d1, auto-tutup, lazy-check, db-batch, migration, slip-keputusan, ranking

---
