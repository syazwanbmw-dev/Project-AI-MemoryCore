# Fasa 8 — Ujian Dalaman Implementation Plan

> **For agentic workers:** Use superpowers:executing-plans to implement task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Tambah modul Ujian Dalaman — admin setup ujian + skala gred + item (tahun+subjek), guru input markah murid, dashboard school-wide analisis taburan gred + cetak, laporan per kelas boleh cetak.

**Architecture:** 4 table baru (ujian, ujian_gred, ujian_item, ujian_markah). 2 route files baru. Admin UI dalam admin.html (tab baru). Guru input dalam ujian.html (page baru). Dashboard.html tambah tab "eRPM" (existing) + "Ujian Dalaman" (school-wide analisis + cetak). Laporan per kelas dalam laporan-ujian.html (page baru) — pivot table murid × subjek dengan ranking. Guru boleh tengok semua subjek dalam dashboard tapi hanya boleh input markah untuk kelas sendiri (via jadual_guru).

**Tech Stack:** Hono.js (Cloudflare Workers), Cloudflare D1 (SQLite), Vanilla HTML/JS, app.css (custom CSS)

---

## File Map

| Fail | Tindakan | Tanggungjawab |
|------|----------|---------------|
| `migrations/009_ujian.sql` | Create | Table ujian |
| `migrations/010_ujian_gred.sql` | Create | Skala gred per ujian |
| `migrations/011_ujian_item.sql` | Create | Tahun + subjek per ujian |
| `migrations/012_ujian_markah.sql` | Create | Markah murid per ujian_item |
| `src/routes/ujian.js` | Create | Admin CRUD ujian + gred + item |
| `src/routes/ujian-markah.js` | Create | Guru input markah + analisis + laporan endpoint |
| `src/index.js` | Modify | Register 2 route baru |
| `public/admin.html` | Modify | Tambah tab "Ujian Dalaman" |
| `public/ujian.html` | Create | Guru input markah murid |
| `public/laporan-ujian.html` | Create | Laporan per kelas — pivot murid × subjek + ranking + cetak |
| `public/app.js` | Modify | Tambah sidebar links + appKiraGred() utility |
| `public/dashboard.html` | Modify | Tambah 2 tab: eRPM + Ujian Dalaman (analisis + cetak) |

---

## Task 1: Migrations — 4 Table Baru

**Files:**
- Create: `migrations/009_ujian.sql`
- Create: `migrations/010_ujian_gred.sql`
- Create: `migrations/011_ujian_item.sql`
- Create: `migrations/012_ujian_markah.sql`

- [x] **Step 1: Buat 009_ujian.sql**

```sql
-- migrations/009_ujian.sql
CREATE TABLE IF NOT EXISTS ujian (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  nama_ujian   TEXT    NOT NULL,
  nama_sesi    TEXT    NOT NULL,
  markah_penuh INTEGER NOT NULL DEFAULT 100,
  status       TEXT    NOT NULL DEFAULT 'tutup',
  created_at   TEXT    DEFAULT (datetime('now'))
);
```

- [x] **Step 2: Buat 010_ujian_gred.sql**

```sql
-- migrations/010_ujian_gred.sql
CREATE TABLE IF NOT EXISTS ujian_gred (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  ujian_id    INTEGER NOT NULL REFERENCES ujian(id) ON DELETE CASCADE,
  markah_min  INTEGER NOT NULL,
  gred        TEXT    NOT NULL
);
```

- [x] **Step 3: Buat 011_ujian_item.sql**

```sql
-- migrations/011_ujian_item.sql
CREATE TABLE IF NOT EXISTS ujian_item (
  id        INTEGER PRIMARY KEY AUTOINCREMENT,
  ujian_id  INTEGER NOT NULL REFERENCES ujian(id) ON DELETE CASCADE,
  tahun     INTEGER NOT NULL,
  subjek_id INTEGER NOT NULL REFERENCES subjek(id) ON DELETE CASCADE,
  UNIQUE(ujian_id, tahun, subjek_id)
);
```

- [x] **Step 4: Buat 012_ujian_markah.sql**

```sql
-- migrations/012_ujian_markah.sql
CREATE TABLE IF NOT EXISTS ujian_markah (
  id            INTEGER PRIMARY KEY AUTOINCREMENT,
  ujian_item_id INTEGER NOT NULL REFERENCES ujian_item(id)  ON DELETE CASCADE,
  murid_id      INTEGER NOT NULL REFERENCES murid(id)       ON DELETE CASCADE,
  markah        INTEGER,
  pengguna_id   INTEGER NOT NULL REFERENCES pengguna(id),
  tarikh        TEXT    NOT NULL,
  UNIQUE(ujian_item_id, murid_id)
);
```

- [x] **Step 5: Apply migrations ke staging DB**

```bash
npx wrangler d1 migrations apply mypwa-v2-db --remote --env staging
```

Expected output: `4 migrations applied successfully`

- [x] **Step 6: Commit migrations**

```bash
git add migrations/009_ujian.sql migrations/010_ujian_gred.sql migrations/011_ujian_item.sql migrations/012_ujian_markah.sql
git commit -m "feat: migrations 009-012 — ujian dalaman tables"
```

---

## Task 2: Route — ujian.js (Admin CRUD)

**Files:**
- Create: `src/routes/ujian.js`

- [ ] **Step 1: Buat src/routes/ujian.js**

```js
import { Hono } from 'hono';
import { authMiddleware, adminOnly } from '../middleware/auth.js';

const ujian = new Hono();
ujian.use('*', authMiddleware);

// GET /api/ujian — list semua ujian
// ADMIN: semua status | GURU: buka sahaja
ujian.get('/', async (c) => {
  const user = c.get('user');
  const whereStatus = user.role === 'GURU' ? `WHERE u.status = 'buka'` : '';

  const { results } = await c.env.DB.prepare(`
    SELECT u.*,
      (SELECT COUNT(*) FROM ujian_gred WHERE ujian_id = u.id) AS jumlah_gred,
      (SELECT COUNT(*) FROM ujian_item WHERE ujian_id = u.id) AS jumlah_item
    FROM ujian u
    ${whereStatus}
    ORDER BY u.created_at DESC
  `).all();

  return c.json(results);
});

// GET /api/ujian/:id — detail ujian + gred + items
ujian.get('/:id', async (c) => {
  const id = c.req.param('id');
  const u = await c.env.DB.prepare('SELECT * FROM ujian WHERE id = ?').bind(id).first();
  if (!u) return c.json({ error: 'Ujian tidak dijumpai' }, 404);

  const [{ results: gred }, { results: items }] = await Promise.all([
    c.env.DB.prepare(
      'SELECT id, markah_min, gred FROM ujian_gred WHERE ujian_id = ? ORDER BY markah_min ASC'
    ).bind(id).all(),
    c.env.DB.prepare(`
      SELECT ui.id, ui.tahun, ui.subjek_id, s.nama_subjek
      FROM ujian_item ui
      JOIN subjek s ON ui.subjek_id = s.id
      WHERE ui.ujian_id = ?
      ORDER BY ui.tahun, s.nama_subjek
    `).bind(id).all()
  ]);

  return c.json({ ...u, gred, items });
});

// POST /api/ujian — cipta ujian (admin)
ujian.post('/', adminOnly, async (c) => {
  const { nama_ujian, nama_sesi, markah_penuh = 100 } = await c.req.json();
  if (!nama_ujian || !nama_sesi) return c.json({ error: 'nama_ujian dan nama_sesi diperlukan' }, 400);

  const { meta } = await c.env.DB.prepare(
    'INSERT INTO ujian (nama_ujian, nama_sesi, markah_penuh) VALUES (?, ?, ?)'
  ).bind(nama_ujian.trim(), nama_sesi.trim(), markah_penuh).run();

  return c.json({ ok: true, id: meta.last_row_id });
});

// PUT /api/ujian/:id — kemaskini ujian (admin)
ujian.put('/:id', adminOnly, async (c) => {
  const id = c.req.param('id');
  const { nama_ujian, nama_sesi, markah_penuh, status } = await c.req.json();

  await c.env.DB.prepare(`
    UPDATE ujian SET
      nama_ujian   = COALESCE(?, nama_ujian),
      nama_sesi    = COALESCE(?, nama_sesi),
      markah_penuh = COALESCE(?, markah_penuh),
      status       = COALESCE(?, status)
    WHERE id = ?
  `).bind(nama_ujian || null, nama_sesi || null, markah_penuh || null, status || null, id).run();

  return c.json({ ok: true });
});

// DELETE /api/ujian/:id — padam ujian + cascade (admin)
ujian.delete('/:id', adminOnly, async (c) => {
  const id = c.req.param('id');
  await c.env.DB.prepare('DELETE FROM ujian WHERE id = ?').bind(id).run();
  return c.json({ ok: true });
});

// POST /api/ujian/:id/gred — gantikan skala gred (admin)
// Body: { grads: [{markah_min, gred}, ...] }
ujian.post('/:id/gred', adminOnly, async (c) => {
  const id = c.req.param('id');
  const { grads } = await c.req.json();
  if (!Array.isArray(grads) || !grads.length) return c.json({ error: 'grads array diperlukan' }, 400);

  const stmts = [
    c.env.DB.prepare('DELETE FROM ujian_gred WHERE ujian_id = ?').bind(id),
    ...grads.map(g =>
      c.env.DB.prepare('INSERT INTO ujian_gred (ujian_id, markah_min, gred) VALUES (?, ?, ?)')
        .bind(id, g.markah_min, g.gred)
    )
  ];

  await c.env.DB.batch(stmts);
  return c.json({ ok: true });
});

// POST /api/ujian/:id/item — tambah item tahun+subjek (admin)
// Body: { tahun, subjek_id }
ujian.post('/:id/item', adminOnly, async (c) => {
  const id = c.req.param('id');
  const { tahun, subjek_id } = await c.req.json();
  if (!tahun || !subjek_id) return c.json({ error: 'tahun dan subjek_id diperlukan' }, 400);

  try {
    const { meta } = await c.env.DB.prepare(
      'INSERT INTO ujian_item (ujian_id, tahun, subjek_id) VALUES (?, ?, ?)'
    ).bind(id, tahun, subjek_id).run();
    return c.json({ ok: true, id: meta.last_row_id });
  } catch (e) {
    if (e.message?.includes('UNIQUE')) return c.json({ error: 'Kombinasi tahun + subjek sudah wujud' }, 409);
    throw e;
  }
});

// DELETE /api/ujian/item/:item_id — padam satu item (admin)
ujian.delete('/item/:item_id', adminOnly, async (c) => {
  const item_id = c.req.param('item_id');
  await c.env.DB.prepare('DELETE FROM ujian_item WHERE id = ?').bind(item_id).run();
  return c.json({ ok: true });
});

export default ujian;
```

- [x] **Step 2: Commit**

```bash
git add src/routes/ujian.js
git commit -m "feat: route ujian.js — admin CRUD ujian + gred + item"
```

---

## Task 3: Route — ujian-markah.js (Guru + Analisis)

**Files:**
- Create: `src/routes/ujian-markah.js`

- [x] **Step 1: Buat src/routes/ujian-markah.js**

```js
import { Hono } from 'hono';
import { authMiddleware } from '../middleware/auth.js';

const ujianMarkah = new Hono();
ujianMarkah.use('*', authMiddleware);

// GET /api/ujian-markah/jadual?ujian_id=
// Kelas + subjek yang guru boleh isi untuk ujian ini (intersect jadual_guru + ujian_item)
ujianMarkah.get('/jadual', async (c) => {
  const user     = c.get('user');
  const ujian_id = c.req.query('ujian_id');
  if (!ujian_id) return c.json({ error: 'ujian_id diperlukan' }, 400);

  const { results } = await c.env.DB.prepare(`
    SELECT DISTINCT
      jg.kelas_id, k.nama_kelas, k.tahun,
      ui.subjek_id, s.nama_subjek,
      ui.id AS ujian_item_id
    FROM jadual_guru jg
    JOIN kelas k       ON jg.kelas_id = k.id
    JOIN ujian u       ON u.id = ?
    JOIN ujian_item ui ON ui.ujian_id = ? AND ui.tahun = k.tahun AND ui.subjek_id = jg.subjek_id
    JOIN subjek s      ON ui.subjek_id = s.id
    WHERE jg.pengguna_id = ?
      AND jg.nama_sesi   = u.nama_sesi
    ORDER BY k.tahun, k.nama_kelas, s.nama_subjek
  `).bind(ujian_id, ujian_id, user.id).all();

  return c.json(results);
});

// GET /api/ujian-markah/murid?ujian_item_id=&kelas_id=
// Senarai murid dalam kelas + markah sedia ada untuk ujian_item ini
ujianMarkah.get('/murid', async (c) => {
  const ujian_item_id = c.req.query('ujian_item_id');
  const kelas_id      = c.req.query('kelas_id');
  if (!ujian_item_id || !kelas_id) return c.json({ error: 'ujian_item_id dan kelas_id diperlukan' }, 400);

  const { results } = await c.env.DB.prepare(`
    SELECT m.id, m.nama, um.markah, um.tarikh
    FROM murid m
    LEFT JOIN ujian_markah um ON um.murid_id = m.id AND um.ujian_item_id = ?
    WHERE m.id_kelas = ?
    ORDER BY m.nama
  `).bind(ujian_item_id, kelas_id).all();

  return c.json(results);
});

// POST /api/ujian-markah/bulk — simpan markah batch
// Body: { ujian_item_id, tarikh, markah: [{murid_id, markah}, ...] }
ujianMarkah.post('/bulk', async (c) => {
  const user = c.get('user');
  const { ujian_item_id, tarikh, markah } = await c.req.json();
  if (!ujian_item_id || !Array.isArray(markah)) return c.json({ error: 'Data tidak lengkap' }, 400);

  const t = tarikh || new Date().toLocaleDateString('en-CA', { timeZone: 'Asia/Kuala_Lumpur' });

  const stmts = markah.map(r =>
    c.env.DB.prepare(`
      INSERT INTO ujian_markah (ujian_item_id, murid_id, markah, pengguna_id, tarikh)
      VALUES (?, ?, ?, ?, ?)
      ON CONFLICT (ujian_item_id, murid_id)
      DO UPDATE SET markah = excluded.markah, tarikh = excluded.tarikh, pengguna_id = excluded.pengguna_id
    `).bind(ujian_item_id, r.murid_id, r.markah ?? null, user.id, t)
  );

  await c.env.DB.batch(stmts);
  return c.json({ ok: true, message: `${markah.length} rekod markah disimpan.` });
});

// GET /api/ujian-markah/analisis?ujian_id=&subjek_id=&tahun=&kelas_id=
// School-wide taburan gred — semua user login boleh akses (untuk dashboard)
// Filter: ujian_id + subjek_id (wajib), tahun + kelas_id (pilihan)
ujianMarkah.get('/analisis', async (c) => {
  const ujian_id  = c.req.query('ujian_id');
  const subjek_id = c.req.query('subjek_id');
  const tahun     = c.req.query('tahun');
  const kelas_id  = c.req.query('kelas_id');

  if (!ujian_id || !subjek_id) return c.json({ error: 'ujian_id dan subjek_id diperlukan' }, 400);

  const [ujianRow, { results: gredScale }] = await Promise.all([
    c.env.DB.prepare('SELECT markah_penuh FROM ujian WHERE id = ?').bind(ujian_id).first(),
    c.env.DB.prepare(
      'SELECT markah_min, gred FROM ujian_gred WHERE ujian_id = ? ORDER BY markah_min ASC'
    ).bind(ujian_id).all()
  ]);

  let query = `
    SELECT um.markah
    FROM ujian_markah um
    JOIN ujian_item ui ON um.ujian_item_id = ui.id
    JOIN murid m       ON um.murid_id = m.id
    JOIN kelas k       ON m.id_kelas = k.id
    WHERE ui.ujian_id = ? AND ui.subjek_id = ?
  `;
  const bindings = [ujian_id, subjek_id];

  if (tahun)    { query += ' AND k.tahun = ?';    bindings.push(tahun); }
  if (kelas_id) { query += ' AND m.id_kelas = ?'; bindings.push(kelas_id); }

  const { results: markahList } = await c.env.DB.prepare(query).bind(...bindings).all();

  // Kira gred setiap murid menggunakan skala ujian
  const sortedGred = [...gredScale].sort((a, b) => b.markah_min - a.markah_min);
  function kiraGredBackend(markah) {
    if (markah === null || markah === undefined) return '-';
    const found = sortedGred.find(g => markah >= g.markah_min);
    return found ? found.gred : '-';
  }

  const counts = {};
  gredScale.forEach(g => { counts[g.gred] = 0; });

  markahList.forEach(r => {
    const g = kiraGredBackend(r.markah);
    counts[g] = (counts[g] || 0) + 1;
  });

  return c.json({
    ok: true,
    data: {
      counts,
      gredScale,
      markah_penuh: ujianRow?.markah_penuh || 100,
      total: markahList.length
    }
  });
});

export default ujianMarkah;
```

- [x] **Step 2: Commit**

```bash
git add src/routes/ujian-markah.js
git commit -m "feat: route ujian-markah.js — guru input + school-wide analisis"
```

---

## Task 4: Register Routes dalam index.js

**Files:**
- Modify: `src/index.js`

- [x] **Step 1: Tambah imports dan route registration**

Tambah selepas baris `import jadualGuruRoutes`:
```js
import ujianRoutes      from './routes/ujian.js';
import ujianMarkahRoutes from './routes/ujian-markah.js';
```

Tambah selepas baris `app.route('/api/rekod', rekodRoutes)`:
```js
app.route('/api/ujian',        ujianRoutes);
app.route('/api/ujian-markah', ujianMarkahRoutes);
```

- [x] **Step 2: Push + test endpoints**

```bash
git add src/index.js
git commit -m "feat: register route ujian + ujian-markah dalam index.js"
git push origin test
```

Test manual dengan curl atau browser devtools:
- `GET /api/ujian` → expect `[]` (kosong, table baru)
- `GET /api/ujian-markah/analisis?ujian_id=1&subjek_id=1` → expect `400` (belum ada data)

---

## Task 5: Admin Panel — Tab "Ujian Dalaman"

**Files:**
- Modify: `public/admin.html`

- [x] **Step 1: Tambah tab button**

Cari baris tab buttons (baris ~48-52). Tambah selepas button "Tetapan":
```html
<button class="tab-btn" data-tab="ujian" onclick="switchTab('ujian')">Ujian Dalaman</button>
```

- [x] **Step 2: Tambah tab panel**

Tambah selepas `</div>` penutup `panel-tetapan`, sebelum `</main>`:

```html
<!-- ── TAB: Ujian Dalaman ─────────────────────── -->
<div id="panel-ujian" class="tab-panel">

  <!-- Toolbar -->
  <div class="toolbar">
    <div class="toolbar-left">
      <h2 style="font-size:0.9rem;font-weight:700;color:var(--primary);margin:0">Senarai Ujian</h2>
    </div>
    <div class="toolbar-right">
      <button class="btn btn-primary btn-sm" onclick="openTambahUjian()">+ Tambah Ujian</button>
    </div>
  </div>

  <!-- Jadual ujian -->
  <div class="table-wrap">
    <table class="data-table" id="ujianTable">
      <thead>
        <tr>
          <th>Nama Ujian</th>
          <th>Sesi</th>
          <th style="text-align:center">Markah Penuh</th>
          <th style="text-align:center">Gred</th>
          <th style="text-align:center">Item</th>
          <th style="text-align:center">Status</th>
          <th style="text-align:center">Tindakan</th>
        </tr>
      </thead>
      <tbody id="ujianBody"></tbody>
    </table>
  </div>

</div>
```

- [x] **Step 3: Tambah Modal — Tambah/Edit Ujian** *(digabung dalam shared modal sistem sedia ada)*

Tambah sebelum `</body>`:

```html
<!-- Modal: Tambah/Edit Ujian -->
<div class="modal-overlay" id="ujianModal" onclick="if(event.target===this)closeUjianModal()">
  <div class="modal">
    <div class="modal-header">
      <span class="modal-title" id="ujianModalTitle">Tambah Ujian</span>
      <button class="modal-close" onclick="closeUjianModal()">✕</button>
    </div>
    <form id="ujianForm" onsubmit="simpanUjian(event)">
      <div class="form-group">
        <label class="form-label">Nama Ujian</label>
        <input class="form-input" id="ujianNama" placeholder="cth: Ujian Diagnostik" required>
      </div>
      <div class="form-group">
        <label class="form-label">Sesi</label>
        <select class="form-select" id="ujianSesi" required>
          <option value="">— Pilih Sesi —</option>
        </select>
      </div>
      <div class="form-group">
        <label class="form-label">Markah Penuh</label>
        <input class="form-input" id="ujianMarkahPenuh" type="number" min="1" max="1000" value="100" required>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-ghost" onclick="closeUjianModal()">Batal</button>
        <button type="submit" class="btn btn-primary">Simpan</button>
      </div>
    </form>
  </div>
</div>

<!-- Modal: Urus Gred Scale -->
<div class="modal-overlay" id="gredModal" onclick="if(event.target===this)closeGredModal()">
  <div class="modal" style="max-width:520px">
    <div class="modal-header">
      <span class="modal-title" id="gredModalTitle">Skala Gred</span>
      <button class="modal-close" onclick="closeGredModal()">✕</button>
    </div>
    <p style="font-size:0.8rem;color:#64748b;margin-bottom:0.75rem">
      Masukkan markah minimum untuk setiap gred. Gred ditentukan dengan cari markah_min tertinggi yang ≤ markah murid.
    </p>
    <div id="gredRows" style="margin-bottom:0.75rem"></div>
    <div style="display:flex;gap:0.5rem;margin-bottom:1rem">
      <input class="form-input" id="newGredMin" type="number" placeholder="Markah Min" style="width:120px">
      <input class="form-input" id="newGredNama" placeholder="Gred (cth: A)" style="width:100px">
      <button class="btn btn-ghost btn-sm" onclick="tambahGredRow()">+ Tambah</button>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closeGredModal()">Batal</button>
      <button class="btn btn-primary" onclick="simpanGred()">Simpan Skala</button>
    </div>
  </div>
</div>

<!-- Modal: Urus Item (Tahun + Subjek) -->
<div class="modal-overlay" id="itemModal" onclick="if(event.target===this)closeItemModal()">
  <div class="modal" style="max-width:520px">
    <div class="modal-header">
      <span class="modal-title" id="itemModalTitle">Tahun &amp; Subjek</span>
      <button class="modal-close" onclick="closeItemModal()">✕</button>
    </div>
    <div style="display:flex;gap:0.5rem;margin-bottom:0.75rem;align-items:flex-end">
      <div class="form-group" style="margin:0;flex:1">
        <label class="form-label">Tahun</label>
        <select class="form-select" id="itemTahun">
          <option value="">— Tahun —</option>
          <option value="1">Tahun 1</option><option value="2">Tahun 2</option>
          <option value="3">Tahun 3</option><option value="4">Tahun 4</option>
          <option value="5">Tahun 5</option><option value="6">Tahun 6</option>
        </select>
      </div>
      <div class="form-group" style="margin:0;flex:2">
        <label class="form-label">Subjek</label>
        <select class="form-select" id="itemSubjek">
          <option value="">— Subjek —</option>
        </select>
      </div>
      <button class="btn btn-primary btn-sm" onclick="tambahItem()" style="margin-bottom:0">Tambah</button>
    </div>
    <div class="table-wrap">
      <table class="data-table" style="font-size:0.8rem">
        <thead><tr><th>Tahun</th><th>Subjek</th><th style="width:50px"></th></tr></thead>
        <tbody id="itemBody"></tbody>
      </table>
    </div>
    <div class="modal-footer">
      <button class="btn btn-ghost" onclick="closeItemModal()">Tutup</button>
    </div>
  </div>
</div>
```

- [x] **Step 4: Tambah JS fungsi untuk tab Ujian Dalaman**

Cari bahagian `<script>` dalam admin.html. Tambah fungsi-fungsi berikut selepas fungsi `switchTab`:

```js
// ── UJIAN DALAMAN ─────────────────────────────────
let _ujianEditId = null;
let _gredUjianId = null;
let _itemUjianId = null;
let _gredRows    = [];  // [{markah_min, gred}, ...]
let _subjekList  = [];  // untuk modal item

async function loadUjian() {
  const r = await api('GET', '/ujian');
  const list = r?.data || r || [];
  const body = document.getElementById('ujianBody');
  if (!list.length) {
    body.innerHTML = `<tr><td colspan="7" style="text-align:center;color:#94a3b8">Tiada ujian</td></tr>`;
    return;
  }
  body.innerHTML = list.map(u => `
    <tr>
      <td style="font-weight:600">${u.nama_ujian}</td>
      <td>${u.nama_sesi}</td>
      <td style="text-align:center">${u.markah_penuh}</td>
      <td style="text-align:center">
        <span class="badge ${u.jumlah_gred ? 'badge-green' : 'badge-gray'}">${u.jumlah_gred} baris</span>
      </td>
      <td style="text-align:center">
        <span class="badge ${u.jumlah_item ? 'badge-green' : 'badge-gray'}">${u.jumlah_item} item</span>
      </td>
      <td style="text-align:center">
        <span class="badge ${u.status === 'buka' ? 'badge-green' : 'badge-gray'}">${u.status}</span>
      </td>
      <td style="text-align:center">
        <div style="display:flex;gap:0.35rem;justify-content:center;flex-wrap:wrap">
          <button class="btn btn-ghost btn-sm" onclick="openEditUjian(${u.id},'${esc(u.nama_ujian)}','${esc(u.nama_sesi)}',${u.markah_penuh})">Edit</button>
          <button class="btn btn-ghost btn-sm" onclick="openGredModal(${u.id},'${esc(u.nama_ujian)}')">Gred</button>
          <button class="btn btn-ghost btn-sm" onclick="openItemModal(${u.id},'${esc(u.nama_ujian)}')">Item</button>
          <button class="btn btn-ghost btn-sm" onclick="toggleStatusUjian(${u.id},'${u.status}')">
            ${u.status === 'buka' ? 'Tutup' : 'Buka'}
          </button>
          <button class="btn btn-danger btn-sm" onclick="padamUjian(${u.id})">Padam</button>
        </div>
      </td>
    </tr>
  `).join('');
}

function openTambahUjian() {
  _ujianEditId = null;
  document.getElementById('ujianModalTitle').textContent = 'Tambah Ujian';
  document.getElementById('ujianNama').value = '';
  document.getElementById('ujianMarkahPenuh').value = 100;
  document.getElementById('ujianSesi').value = '';
  document.getElementById('ujianModal').classList.add('open');
}

function openEditUjian(id, nama, sesi, markahPenuh) {
  _ujianEditId = id;
  document.getElementById('ujianModalTitle').textContent = 'Edit Ujian';
  document.getElementById('ujianNama').value = nama;
  document.getElementById('ujianSesi').value = sesi;
  document.getElementById('ujianMarkahPenuh').value = markahPenuh;
  document.getElementById('ujianModal').classList.add('open');
}

function closeUjianModal() { document.getElementById('ujianModal').classList.remove('open'); }

async function simpanUjian(e) {
  e.preventDefault();
  const body = {
    nama_ujian:   document.getElementById('ujianNama').value.trim(),
    nama_sesi:    document.getElementById('ujianSesi').value,
    markah_penuh: +document.getElementById('ujianMarkahPenuh').value
  };
  const r = _ujianEditId
    ? await api('PUT', `/ujian/${_ujianEditId}`, body)
    : await api('POST', '/ujian', body);
  if (!r?.ok) { toast(r?.data?.error || 'Gagal simpan.', 'err'); return; }
  closeUjianModal();
  toast(_ujianEditId ? 'Ujian dikemaskini.' : 'Ujian ditambah.');
  loadUjian();
}

async function padamUjian(id) {
  if (!confirm('Padam ujian ini? Semua gred, item dan markah murid akan turut dipadam.')) return;
  const r = await api('DELETE', `/ujian/${id}`);
  if (!r?.ok) { toast('Gagal padam.', 'err'); return; }
  toast('Ujian dipadam.');
  loadUjian();
}

async function toggleStatusUjian(id, statusSemasa) {
  const statusBaru = statusSemasa === 'buka' ? 'tutup' : 'buka';
  const r = await api('PUT', `/ujian/${id}`, { status: statusBaru });
  if (!r?.ok) { toast('Gagal kemaskini status.', 'err'); return; }
  toast(`Ujian kini ${statusBaru}.`);
  loadUjian();
}

// ── Gred Modal ────────────────────────────────────
async function openGredModal(id, nama) {
  _gredUjianId = id;
  document.getElementById('gredModalTitle').textContent = `Skala Gred — ${nama}`;
  const r = await api('GET', `/ujian/${id}`);
  _gredRows = (r?.gred || []).map(g => ({ markah_min: g.markah_min, gred: g.gred }));
  renderGredRows();
  document.getElementById('newGredMin').value  = '';
  document.getElementById('newGredNama').value = '';
  document.getElementById('gredModal').classList.add('open');
}

function closeGredModal() { document.getElementById('gredModal').classList.remove('open'); }

function renderGredRows() {
  const sorted = [..._gredRows].sort((a, b) => a.markah_min - b.markah_min);
  document.getElementById('gredRows').innerHTML = sorted.length
    ? `<table class="data-table" style="font-size:0.82rem;margin-bottom:0.25rem">
        <thead><tr><th>Markah Min</th><th>Gred</th><th style="width:40px"></th></tr></thead>
        <tbody>${sorted.map((g, i) => `
          <tr>
            <td>${g.markah_min}</td>
            <td>${g.gred}</td>
            <td><button class="btn btn-danger btn-sm" onclick="buangGredRow(${i})">×</button></td>
          </tr>
        `).join('')}</tbody>
      </table>`
    : '<p style="font-size:0.8rem;color:#94a3b8">Belum ada skala gred.</p>';
}

function tambahGredRow() {
  const min  = document.getElementById('newGredMin').value;
  const nama = document.getElementById('newGredNama').value.trim();
  if (min === '' || !nama) { toast('Isi markah min dan gred.', 'err'); return; }
  _gredRows.push({ markah_min: +min, gred: nama });
  renderGredRows();
  document.getElementById('newGredMin').value  = '';
  document.getElementById('newGredNama').value = '';
}

function buangGredRow(i) {
  const sorted = [..._gredRows].sort((a, b) => a.markah_min - b.markah_min);
  const target = sorted[i];
  _gredRows = _gredRows.filter(g => !(g.markah_min === target.markah_min && g.gred === target.gred));
  renderGredRows();
}

async function simpanGred() {
  if (!_gredRows.length) { toast('Tambah sekurang-kurangnya satu baris gred.', 'err'); return; }
  const r = await api('POST', `/ujian/${_gredUjianId}/gred`, { grads: _gredRows });
  if (!r?.ok) { toast('Gagal simpan skala gred.', 'err'); return; }
  toast('Skala gred disimpan.');
  closeGredModal();
  loadUjian();
}

// ── Item Modal ────────────────────────────────────
async function openItemModal(id, nama) {
  _itemUjianId = id;
  document.getElementById('itemModalTitle').textContent = `Tahun & Subjek — ${nama}`;
  if (!_subjekList.length) {
    const rs = await api('GET', '/subjek');
    _subjekList = rs?.data || [];
    const selSubjek = document.getElementById('itemSubjek');
    selSubjek.innerHTML = '<option value="">— Subjek —</option>' +
      _subjekList.map(s => `<option value="${s.id}">${s.nama_subjek}</option>`).join('');
  }
  await muatItemBody(id);
  document.getElementById('itemModal').classList.add('open');
}

function closeItemModal() { document.getElementById('itemModal').classList.remove('open'); }

async function muatItemBody(id) {
  const r = await api('GET', `/ujian/${id}`);
  const items = r?.items || [];
  document.getElementById('itemBody').innerHTML = items.length
    ? items.map(item => `
        <tr>
          <td>Tahun ${item.tahun}</td>
          <td>${item.nama_subjek}</td>
          <td><button class="btn btn-danger btn-sm" onclick="padamItem(${item.id})">×</button></td>
        </tr>
      `).join('')
    : '<tr><td colspan="3" style="color:#94a3b8;text-align:center">Tiada item</td></tr>';
}

async function tambahItem() {
  const tahun    = document.getElementById('itemTahun').value;
  const subjek_id = document.getElementById('itemSubjek').value;
  if (!tahun || !subjek_id) { toast('Pilih tahun dan subjek.', 'err'); return; }
  const r = await api('POST', `/ujian/${_itemUjianId}/item`, { tahun: +tahun, subjek_id: +subjek_id });
  if (!r?.ok) { toast(r?.data?.error || 'Gagal tambah item.', 'err'); return; }
  toast('Item ditambah.');
  muatItemBody(_itemUjianId);
  loadUjian();
}

async function padamItem(item_id) {
  const r = await api('DELETE', `/ujian/item/${item_id}`);
  if (!r?.ok) { toast('Gagal padam item.', 'err'); return; }
  toast('Item dipadam.');
  muatItemBody(_itemUjianId);
  loadUjian();
}
```

- [x] **Step 5: Tambah loadUjian() dan populateSesi dalam switchTab**

Dalam fungsi `switchTab`, tambah case untuk tab ujian:

```js
// Dalam fungsi switchTab(id), tambah:
if (id === 'ujian') loadUjian();
```

Dalam fungsi `init()` atau mana-mana tempat yang load sesi untuk admin dropdowns, populate `#ujianSesi`:

```js
// Selepas load sesi list (sesiList ada dalam init admin):
const selUjianSesi = document.getElementById('ujianSesi');
if (selUjianSesi) {
  sesiList.forEach(s => {
    selUjianSesi.innerHTML += `<option value="${s.nama_sesi}">${s.nama_sesi}</option>`;
  });
}
```

- [x] **Step 6: Commit + push + test**

```bash
git add public/admin.html
git commit -m "feat: admin panel — tab Ujian Dalaman (CRUD ujian, gred, item)"
git push origin test
```

Test manual di staging:
- Buka admin panel → klik tab "Ujian Dalaman"
- Tambah ujian
- Urus gred scale
- Urus item (tambah tahun + subjek)
- Toggle status buka/tutup

---

## Task 6: Guru Input — ujian.html

**Files:**
- Create: `public/ujian.html`

- [ ] **Step 1: Buat public/ujian.html**

```html
<!DOCTYPE html>
<html lang="ms">
<head>
  <meta charset="UTF-8">
  <link rel="icon" type="image/svg+xml" href="/favicon.svg">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Ujian Dalaman — eRPM</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="/app.css">
</head>
<body>
  <div class="mobile-header">
    <button class="hamburger" onclick="openSidebar()">☰</button>
    <span>Ujian Dalaman</span>
  </div>
  <div class="sidebar-overlay" id="sidebarOverlay" onclick="closeSidebar()"></div>
  <div id="sidebar"></div>

  <main class="main-content">
    <div class="page-header">
      <h1>Input Markah Ujian Dalaman</h1>
      <p>Pilih ujian, kelas dan subjek — kemudian klik Cari untuk paparkan senarai murid.</p>
    </div>

    <!-- Filter bar -->
    <div class="card" style="margin-bottom:1.25rem">
      <div class="filter-grid">
        <div class="form-group" style="margin:0">
          <label class="form-label">Ujian</label>
          <select class="form-select" id="selUjian" onchange="onUjianChange()">
            <option value="">— Pilih Ujian —</option>
          </select>
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label">Tarikh</label>
          <input class="form-input" id="inpTarikh" type="date">
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label">Kelas</label>
          <select class="form-select" id="selKelas" disabled onchange="onKelasChange()">
            <option value="">— Kelas —</option>
          </select>
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label">Subjek</label>
          <select class="form-select" id="selSubjek" disabled>
            <option value="">— Subjek —</option>
          </select>
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label" style="visibility:hidden">Cari</label>
          <button class="btn btn-primary" id="btnCari" onclick="cariMurid()" disabled style="width:100%">Cari</button>
        </div>
      </div>
    </div>

    <!-- Hasil -->
    <div id="hasilSection" style="display:none">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.75rem;flex-wrap:wrap;gap:0.5rem">
        <div>
          <span id="hasilLabel" style="font-size:0.88rem;font-weight:700;color:var(--primary)"></span>
          <span id="hasilInfo" style="font-size:0.8rem;color:#64748b;margin-left:0.5rem"></span>
        </div>
        <button class="btn btn-primary btn-sm" onclick="simpanSemua()" id="btnSimpan">Simpan Semua</button>
      </div>

      <div style="overflow-x:auto">
        <table class="data-table" id="muridTable">
          <thead>
            <tr>
              <th style="width:2.2rem;text-align:center">#</th>
              <th>Nama Murid</th>
              <th style="width:100px;text-align:center">Markah</th>
              <th style="width:80px;text-align:center">Gred</th>
            </tr>
          </thead>
          <tbody id="muridBody"></tbody>
        </table>
      </div>

      <div style="margin-top:0.75rem">
        <button class="btn btn-primary" onclick="simpanSemua()">Simpan Semua</button>
      </div>
    </div>
  </main>

  <script src="/app.js"></script>
  <script>
    requireAuth();
    renderSidebar('/ujian.html');
    loadTema();

    let jadualCache = [];   // [{kelas_id, nama_kelas, tahun, subjek_id, nama_subjek, ujian_item_id}]
    let muridList   = [];
    let markahMap   = {};   // murid_id → markah (number|null)
    let gredScale   = [];   // [{markah_min, gred}]
    let markahPenuh = 100;
    let ujianItemId = null;

    async function init() {
      document.getElementById('inpTarikh').value = today();
      const r = await api('GET', '/ujian');
      const ujianList = Array.isArray(r) ? r : (r?.data || []);
      const sel = document.getElementById('selUjian');
      ujianList.forEach(u => {
        sel.innerHTML += `<option value="${u.id}" data-markah="${u.markah_penuh}">${u.nama_ujian} (${u.nama_sesi})</option>`;
      });
    }

    async function onUjianChange() {
      const ujian_id = document.getElementById('selUjian').value;
      const opt      = document.getElementById('selUjian').options[document.getElementById('selUjian').selectedIndex];
      markahPenuh    = +(opt?.dataset?.markah || 100);

      const selKelas = document.getElementById('selKelas');
      selKelas.innerHTML = '<option value="">— Kelas —</option>';
      selKelas.disabled  = true;
      document.getElementById('selSubjek').innerHTML = '<option value="">— Subjek —</option>';
      document.getElementById('selSubjek').disabled  = true;
      document.getElementById('btnCari').disabled    = true;
      document.getElementById('hasilSection').style.display = 'none';

      if (!ujian_id) return;

      const r = await api('GET', `/ujian-markah/jadual?ujian_id=${ujian_id}`);
      jadualCache = r?.data || r || [];

      // Build kelas unique list
      const kelasMap = {};
      jadualCache.forEach(j => { if (!kelasMap[j.kelas_id]) kelasMap[j.kelas_id] = j; });
      selKelas.innerHTML = '<option value="">— Kelas —</option>' +
        Object.values(kelasMap).map(j => `<option value="${j.kelas_id}">${j.nama_kelas} (Tahun ${j.tahun})</option>`).join('');
      selKelas.disabled = !jadualCache.length;

      // Load gred scale
      const rGred = await api('GET', `/ujian/${ujian_id}`);
      gredScale = rGred?.gred || [];
    }

    function onKelasChange() {
      const kelas_id  = document.getElementById('selKelas').value;
      const selSubjek = document.getElementById('selSubjek');
      const senarai   = jadualCache.filter(j => j.kelas_id == kelas_id);
      selSubjek.innerHTML = '<option value="">— Subjek —</option>' +
        senarai.map(j => `<option value="${j.ujian_item_id}">${j.nama_subjek}</option>`).join('');
      selSubjek.disabled = !kelas_id;
      document.getElementById('btnCari').disabled = true;
      document.getElementById('hasilSection').style.display = 'none';
      selSubjek.onchange = () => {
        document.getElementById('btnCari').disabled = !selSubjek.value;
      };
    }

    async function cariMurid() {
      ujianItemId     = document.getElementById('selSubjek').value;
      const kelas_id  = document.getElementById('selKelas').value;
      if (!ujianItemId || !kelas_id) return;

      const btn = document.getElementById('btnCari');
      setLoading(btn, true);
      const r = await api('GET', `/ujian-markah/murid?ujian_item_id=${ujianItemId}&kelas_id=${kelas_id}`);
      setLoading(btn, false);

      muridList = r?.data || r || [];
      markahMap = {};
      muridList.forEach(m => { markahMap[m.id] = m.markah ?? ''; });

      buatJadual();

      const namaKelas  = document.getElementById('selKelas').selectedOptions[0]?.text || '';
      const namaSubjek = document.getElementById('selSubjek').selectedOptions[0]?.text || '';
      document.getElementById('hasilLabel').textContent = `${namaKelas} — ${namaSubjek}`;
      document.getElementById('hasilInfo').textContent  = `Markah penuh: ${markahPenuh}`;
      document.getElementById('hasilSection').style.display = 'block';
    }

    function kiraGred(markah) {
      return appKiraGred(markah, gredScale);
    }

    function buatJadual() {
      document.getElementById('muridBody').innerHTML = muridList.map((m, i) => {
        const val  = markahMap[m.id];
        const gred = val !== '' && val !== null && val !== undefined ? kiraGred(+val) : '—';
        return `<tr>
          <td style="text-align:center;color:#94a3b8;font-size:0.78rem">${i + 1}</td>
          <td style="font-size:0.93rem;font-weight:500">${m.nama}</td>
          <td style="text-align:center">
            <input type="number" min="0" max="${markahPenuh}" value="${val}"
              style="width:70px;text-align:center;border:1px solid #e2e8f0;border-radius:6px;padding:0.3rem;font-size:0.9rem"
              oninput="onMarkahInput(${m.id}, this)"
              class="form-input" style="padding:0.3rem 0.5rem">
          </td>
          <td style="text-align:center;font-weight:700;font-size:0.9rem" id="gred-${m.id}">${gred}</td>
        </tr>`;
      }).join('');
    }

    function onMarkahInput(muridId, el) {
      let val = el.value.trim();
      if (val === '') { markahMap[muridId] = null; document.getElementById(`gred-${muridId}`).textContent = '—'; return; }
      const num = Math.min(markahPenuh, Math.max(0, +val));
      markahMap[muridId] = num;
      document.getElementById(`gred-${muridId}`).textContent = kiraGred(num);
    }

    async function simpanSemua() {
      const toSave = muridList.map(m => ({ murid_id: m.id, markah: markahMap[m.id] ?? null }));
      const btn = document.getElementById('btnSimpan');
      setLoading(btn, true);
      const r = await api('POST', '/ujian-markah/bulk', {
        ujian_item_id: +ujianItemId,
        tarikh: document.getElementById('inpTarikh').value || today(),
        markah: toSave
      });
      setLoading(btn, false);
      if (!r?.ok) { toast(r?.data?.error || 'Gagal simpan.', 'err'); return; }
      toast(r.data.message || 'Markah disimpan.');
    }

    init();
  </script>
</body>
</html>
```

- [ ] **Step 2: Commit + push + test**

```bash
git add public/ujian.html
git commit -m "feat: ujian.html — guru input markah ujian dalaman"
git push origin test
```

Test manual di staging sebagai guru:
- Buka `/ujian.html`
- Pilih ujian → kelas → subjek → Cari
- Input markah → gred auto-kira
- Simpan Semua

---

## Task 7: app.js — Sidebar Link + Shared Utility

**Files:**
- Modify: `public/app.js`

- [ ] **Step 1: Tambah sidebar link "Ujian Dalaman"**

Cari `guruLinks` array dalam `renderSidebar()`. Tambah link baru selepas `'Laporan'`:

```js
{ href: '/ujian.html', label: 'Ujian Dalaman' },
```

- [ ] **Step 2: Tambah fungsi kiraGred sebagai shared utility**

Tambah selepas fungsi `setLoading`:

```js
// Kira gred murid berdasarkan skala ujian
// gredScale: [{markah_min, gred}], markah: number|null
function appKiraGred(markah, gredScale) {
  if (markah === null || markah === undefined || markah === '') return '—';
  const sorted = [...gredScale].sort((a, b) => b.markah_min - a.markah_min);
  const found  = sorted.find(g => +markah >= g.markah_min);
  return found ? found.gred : '—';
}
```

- [ ] **Step 3: Commit + push**

```bash
git add public/app.js
git commit -m "feat: app.js — sidebar link Ujian Dalaman + appKiraGred utility"
git push origin test
```

---

## Task 8: Dashboard — Tab eRPM + Tab Ujian Dalaman

**Files:**
- Modify: `public/dashboard.html`

- [ ] **Step 1: Wrap existing content dalam tab eRPM**

Tambah tab navigation selepas `<div class="page-header">...</div>`:

```html
<!-- Dashboard Tabs -->
<div class="tabs" style="margin-bottom:1.5rem">
  <button class="tab-btn active" data-tab="erpm"  onclick="switchDashTab('erpm')">eRPM</button>
  <button class="tab-btn"        data-tab="ujian"  onclick="switchDashTab('ujian')">Ujian Dalaman</button>
</div>
```

Wrap semua content sedia ada (`#noJadual`, `#noRekod`, `#analytics`) dalam:

```html
<div id="dash-erpm" class="tab-panel active">
  <!-- (semua content sedia ada di sini) -->
</div>
```

Tambah panel kosong untuk Ujian Dalaman selepasnya:

```html
<div id="dash-ujian" class="tab-panel">

  <!-- Filter Ujian Dalaman -->
  <div class="card" style="margin-bottom:1.25rem">
    <div class="filter-grid">
      <div class="form-group" style="margin:0">
        <label class="form-label">Ujian</label>
        <select class="form-select" id="dSelUjian" onchange="onDUjianChange()">
          <option value="">— Pilih Ujian —</option>
        </select>
      </div>
      <div class="form-group" style="margin:0">
        <label class="form-label">Subjek</label>
        <select class="form-select" id="dSelSubjek" disabled onchange="muatAnalisis()">
          <option value="">— Pilih Subjek —</option>
        </select>
      </div>
      <div class="form-group" style="margin:0">
        <label class="form-label">Tahun (pilihan)</label>
        <select class="form-select" id="dSelTahun" onchange="onDTahunChange()">
          <option value="">Semua Tahun</option>
          <option value="1">Tahun 1</option><option value="2">Tahun 2</option>
          <option value="3">Tahun 3</option><option value="4">Tahun 4</option>
          <option value="5">Tahun 5</option><option value="6">Tahun 6</option>
        </select>
      </div>
      <div class="form-group" style="margin:0">
        <label class="form-label">Kelas (pilihan)</label>
        <select class="form-select" id="dSelKelas" onchange="muatAnalisis()">
          <option value="">Semua Kelas</option>
        </select>
      </div>
    </div>
  </div>

  <!-- Carta analisis -->
  <div id="dAnalisisWrap" style="display:none">
    <div class="card">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1rem">
        <div>
          <div id="dAnalisisLabel" style="font-size:0.9rem;font-weight:700;color:var(--primary)"></div>
          <div id="dAnalisisTotal" style="font-size:0.78rem;color:#64748b"></div>
        </div>
      </div>
      <div id="dGredChart"></div>
    </div>
  </div>

  <div id="dNoData" style="display:none;text-align:center;padding:3rem 1rem;color:#94a3b8;font-size:0.85rem">
    Pilih ujian dan subjek untuk papar analisis.
  </div>

</div>
```

- [ ] **Step 2: Tambah JS fungsi untuk tab Ujian Dalaman**

Dalam `<script>` dashboard.html, tambah fungsi berikut:

```js
// ── Dashboard Tab ─────────────────────────────────
function switchDashTab(id) {
  document.querySelectorAll('#dash-erpm, #dash-ujian').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn[data-tab]').forEach(b => b.classList.remove('active'));
  document.getElementById('dash-' + id).classList.add('active');
  document.querySelector(`.tab-btn[data-tab="${id}"]`).classList.add('active');
  if (id === 'ujian' && !document.getElementById('dSelUjian').options.length > 1) initUjianDash();
}

let _dUjianItems = []; // [{ujian_item_id, tahun, subjek_id, nama_subjek}] untuk ujian terpilih
let _dKelasAll   = []; // [{id, nama_kelas, tahun}]

async function initUjianDash() {
  const [rUjian, rKelas] = await Promise.all([
    api('GET', '/ujian'),
    api('GET', '/kelas')
  ]);

  const ujianList = Array.isArray(rUjian) ? rUjian : (rUjian?.data || []);
  _dKelasAll = rKelas?.data || [];

  const sel = document.getElementById('dSelUjian');
  sel.innerHTML = '<option value="">— Pilih Ujian —</option>' +
    ujianList.map(u => `<option value="${u.id}">${u.nama_ujian} (${u.nama_sesi})</option>`).join('');
}

async function onDUjianChange() {
  const ujian_id = document.getElementById('dSelUjian').value;
  const selSubjek = document.getElementById('dSelSubjek');
  selSubjek.innerHTML = '<option value="">— Pilih Subjek —</option>';
  selSubjek.disabled  = true;
  document.getElementById('dAnalisisWrap').style.display = 'none';
  document.getElementById('dNoData').style.display = 'block';

  if (!ujian_id) return;

  const r = await api('GET', `/ujian/${ujian_id}`);
  _dUjianItems = r?.items || [];

  // Build unique subjek list dari items
  const subjekMap = {};
  _dUjianItems.forEach(item => { subjekMap[item.subjek_id] = item.nama_subjek; });
  selSubjek.innerHTML = '<option value="">— Pilih Subjek —</option>' +
    Object.entries(subjekMap).map(([id, nama]) => `<option value="${id}">${nama}</option>`).join('');
  selSubjek.disabled = false;
}

function onDTahunChange() {
  const tahun = document.getElementById('dSelTahun').value;
  const selKelas = document.getElementById('dSelKelas');
  const filtered = tahun ? _dKelasAll.filter(k => k.tahun == tahun) : [];
  selKelas.innerHTML = '<option value="">Semua Kelas</option>' +
    filtered.map(k => `<option value="${k.id}">${k.nama_kelas}</option>`).join('');
  muatAnalisis();
}

async function muatAnalisis() {
  const ujian_id  = document.getElementById('dSelUjian').value;
  const subjek_id = document.getElementById('dSelSubjek').value;
  if (!ujian_id || !subjek_id) return;

  const tahun    = document.getElementById('dSelTahun').value;
  const kelas_id = document.getElementById('dSelKelas').value;

  let url = `/ujian-markah/analisis?ujian_id=${ujian_id}&subjek_id=${subjek_id}`;
  if (tahun)    url += `&tahun=${tahun}`;
  if (kelas_id) url += `&kelas_id=${kelas_id}`;

  const r = await api('GET', url);
  if (!r?.data) return;

  const { counts, gredScale, total } = r.data;

  // Label
  const namaSubjek = document.getElementById('dSelSubjek').selectedOptions[0]?.text || '';
  const namaUjian  = document.getElementById('dSelUjian').selectedOptions[0]?.text  || '';
  const tahunLabel = document.getElementById('dSelTahun').selectedOptions[0]?.text  || 'Semua Tahun';
  const kelasLabel = document.getElementById('dSelKelas').selectedOptions[0]?.text  || 'Semua Kelas';

  document.getElementById('dAnalisisLabel').textContent = `${namaSubjek} — ${namaUjian}`;
  document.getElementById('dAnalisisTotal').textContent = `${total} murid · ${tahunLabel} · ${kelasLabel}`;

  // Render chart
  renderGredChart(counts, gredScale, total);

  document.getElementById('dAnalisisWrap').style.display = 'block';
  document.getElementById('dNoData').style.display = 'none';
}

function renderGredChart(counts, gredScale, total) {
  // Sort gred: A (tertinggi) ke bawah
  const sorted = [...gredScale].sort((a, b) => b.markah_min - a.markah_min);
  const max    = Math.max(...sorted.map(g => counts[g.gred] || 0), 1);

  // Warna per gred (A-F/TD)
  const warna = { A: '#22c55e', B: '#84cc16', C: '#3b82f6', D: '#f59e0b', E: '#f97316', F: '#ef4444', TD: '#94a3b8' };

  document.getElementById('dGredChart').innerHTML = sorted.map(g => {
    const count = counts[g.gred] || 0;
    const pct   = total ? Math.round(count / total * 100) : 0;
    const w     = max ? Math.round(count / max * 100) : 0;
    const color = warna[g.gred] || '#64748b';
    return `
      <div style="display:flex;align-items:center;gap:0.75rem;margin-bottom:0.6rem">
        <div style="width:32px;text-align:right;font-size:0.78rem;font-weight:700;color:#1e293b;flex-shrink:0">${g.gred}</div>
        <div style="flex:1;background:#f1f5f9;border-radius:6px;height:28px;overflow:hidden">
          <div style="width:${w}%;background:${color};height:100%;border-radius:6px;transition:width 0.3s;min-width:${count?'4px':'0'}"></div>
        </div>
        <div style="width:70px;font-size:0.78rem;color:#475569;flex-shrink:0">${count} murid (${pct}%)</div>
      </div>
    `;
  }).join('');
}
```

- [ ] **Step 3: Panggil initUjianDash bila tab ujian aktif pertama kali**

Pastikan `switchDashTab('ujian')` panggil `initUjianDash()`. Update:

```js
function switchDashTab(id) {
  document.querySelectorAll('#dash-erpm, #dash-ujian').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.tab-btn[data-tab]').forEach(b => b.classList.remove('active'));
  document.getElementById('dash-' + id).classList.add('active');
  document.querySelector(`.tab-btn[data-tab="${id}"]`).classList.add('active');
  if (id === 'ujian') initUjianDash();
}
```

- [ ] **Step 4: Tambah button Cetak dalam panel analisis + fungsi cetakAnalisis()**

Dalam HTML `#dAnalisisWrap`, tukar header section kepada:

```html
<div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1rem">
  <div>
    <div id="dAnalisisLabel" style="font-size:0.9rem;font-weight:700;color:var(--primary)"></div>
    <div id="dAnalisisTotal" style="font-size:0.78rem;color:#64748b"></div>
  </div>
  <button class="btn btn-ghost btn-sm" onclick="cetakAnalisis()">Cetak</button>
</div>
```

Tambah fungsi `cetakAnalisis()` dalam JS:

```js
function cetakAnalisis() {
  const label  = document.getElementById('dAnalisisLabel').textContent;
  const total  = document.getElementById('dAnalisisTotal').textContent;
  const chart  = document.getElementById('dGredChart').innerHTML;

  const win = window.open('', '_blank');
  win.document.write(`<!DOCTYPE html><html><head>
    <meta charset="UTF-8">
    <title>Analisis — ${label}</title>
    <style>
      body { font-family: 'Plus Jakarta Sans', sans-serif; padding: 2rem; color: #1e293b; }
      h2   { font-size: 1rem; font-weight: 700; margin-bottom: 0.25rem; }
      p    { font-size: 0.8rem; color: #64748b; margin-bottom: 1.5rem; }
      @media print { body { padding: 1rem; } }
    </style>
  </head><body>
    <h2>${label}</h2>
    <p>${total}</p>
    ${chart}
    <script>window.onload = () => { window.print(); }<\/script>
  </body></html>`);
  win.document.close();
}
```

- [ ] **Step 5: Commit + push + test**

```bash
git add public/dashboard.html
git commit -m "feat: dashboard — tab eRPM + tab Ujian Dalaman (analisis + cetak)"
git push origin test
```

Test manual di staging:
- Tab eRPM → carta sedia ada papar normal
- Tab Ujian Dalaman → pilih ujian → pilih subjek → carta gred muncul
- Filter tahun → kelas → carta update
- Klik Cetak → window baru buka, print dialog muncul

---

## Task 10: API Endpoint — Laporan Per Kelas

**Files:**
- Modify: `src/routes/ujian-markah.js` (tambah endpoint baru)

- [x] **Step 1: Tambah endpoint `/api/ujian-markah/laporan`** *(sudah siap dalam Task 3)*

Tambah dalam ujian-markah.js sebelum `export default`:

```js
// GET /api/ujian-markah/laporan?ujian_id=&kelas_id=
// Return pivot data: semua murid dalam kelas × semua subjek untuk ujian+tahun ini
ujianMarkah.get('/laporan', async (c) => {
  const ujian_id = c.req.query('ujian_id');
  const kelas_id = c.req.query('kelas_id');
  if (!ujian_id || !kelas_id) return c.json({ error: 'ujian_id dan kelas_id diperlukan' }, 400);

  // Fetch semua data serentak
  const [ujianRow, kelasRow, { results: gredScale }, { results: rows }] = await Promise.all([
    c.env.DB.prepare('SELECT id, nama_ujian, nama_sesi, markah_penuh FROM ujian WHERE id = ?').bind(ujian_id).first(),
    c.env.DB.prepare('SELECT id, nama_kelas, tahun FROM kelas WHERE id = ?').bind(kelas_id).first(),
    c.env.DB.prepare('SELECT markah_min, gred FROM ujian_gred WHERE ujian_id = ? ORDER BY markah_min ASC').bind(ujian_id).all(),
    // Pivot query: CROSS JOIN murid × ujian_item supaya setiap murid ada semua subjek
    c.env.DB.prepare(`
      SELECT
        m.id   AS murid_id,
        m.nama AS nama_murid,
        ui.id  AS ujian_item_id,
        ui.subjek_id,
        s.nama_subjek,
        um.markah
      FROM murid m
      JOIN kelas k        ON m.id_kelas = k.id
      JOIN ujian_item ui  ON ui.ujian_id = ? AND ui.tahun = k.tahun
      JOIN subjek s       ON ui.subjek_id = s.id
      LEFT JOIN ujian_markah um ON um.murid_id = m.id AND um.ujian_item_id = ui.id
      WHERE m.id_kelas = ?
      ORDER BY m.nama, s.nama_subjek
    `).bind(ujian_id, kelas_id).all()
  ]);

  if (!ujianRow || !kelasRow) return c.json({ error: 'Ujian atau kelas tidak dijumpai' }, 404);

  // Transform rows → murid pivot map
  const muridMap = {};
  const subjekMap = {};

  rows.forEach(r => {
    if (!muridMap[r.murid_id]) {
      muridMap[r.murid_id] = { id: r.murid_id, nama: r.nama_murid, markah: {} };
    }
    muridMap[r.murid_id].markah[r.subjek_id] = r.markah ?? null;

    if (!subjekMap[r.subjek_id]) {
      subjekMap[r.subjek_id] = { id: r.subjek_id, nama_subjek: r.nama_subjek, ujian_item_id: r.ujian_item_id };
    }
  });

  return c.json({
    ok: true,
    data: {
      ujian:       ujianRow,
      kelas:       kelasRow,
      subjek_list: Object.values(subjekMap),
      gredScale,
      murid:       Object.values(muridMap)
    }
  });
});
```

- [x] **Step 2: Commit** *(disertakan dalam commit Task 3)*

```bash
git add src/routes/ujian-markah.js
git commit -m "feat: ujian-markah — endpoint laporan pivot murid x subjek"
```

---

## Task 11: Laporan Ujian Dalaman — laporan-ujian.html

**Files:**
- Create: `public/laporan-ujian.html`
- Modify: `public/app.js` (tambah sidebar link)

**Logik Ranking:**
1. Kira `jumlah_A` per murid (bilangan subjek dapat gred A)
2. Kira `jumlah_markah` per murid (sum markah, skip null/TD)
3. Sort: `jumlah_A DESC`, tiebreaker `jumlah_markah DESC`
4. Assign KED 1, 2, 3... mengikut urutan

**KEPUTUSAN IKUT GRED:** `3A 0B 0C 1D 0E 0F 5TD`
— Kira berapa subjek dapat setiap gred. Susun ikut markah_min DESC (A dulu, TD akhir).

**Warna markah:**
- Gred E atau F → teks merah `#ef4444`
- Gred TD → teks kelabu `#94a3b8`
- Lain → teks hitam

- [ ] **Step 1: Buat public/laporan-ujian.html**

```html
<!DOCTYPE html>
<html lang="ms">
<head>
  <meta charset="UTF-8">
  <link rel="icon" type="image/svg+xml" href="/favicon.svg">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Laporan Ujian — eRPM</title>
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="/app.css">
  <style>
    .lap-wrap { overflow-x: auto; }
    .lap-table { border-collapse: collapse; font-size: 0.75rem; min-width: 100%; }
    .lap-table th, .lap-table td {
      border: 1px solid #cbd5e1; padding: 0.3rem 0.4rem;
      text-align: center; white-space: nowrap;
    }
    .lap-table .col-nama { text-align: left; min-width: 160px; font-weight: 500; }
    .lap-table thead tr:first-child th { background: #1e3a5f; color: #fff; font-weight: 700; }
    .lap-table thead tr:last-child  th { background: #2563eb; color: #fff; font-size: 0.7rem; }
    .lap-table tbody tr:nth-child(even) td { background: #f8fafc; }
    .td-merah  { color: #ef4444; font-weight: 700; }
    .td-kelabu { color: #94a3b8; }
    .td-keputusan { font-size: 0.68rem; text-align: left; min-width: 140px; }
    @media print {
      .no-print { display: none !important; }
      body { padding: 0; }
      .main-content { margin: 0; padding: 0.5rem; }
      #sidebar, .mobile-header { display: none; }
    }
  </style>
</head>
<body>
  <div class="mobile-header">
    <button class="hamburger" onclick="openSidebar()">☰</button>
    <span>Laporan Ujian</span>
  </div>
  <div class="sidebar-overlay" id="sidebarOverlay" onclick="closeSidebar()"></div>
  <div id="sidebar"></div>

  <main class="main-content">
    <div class="page-header">
      <h1>Laporan Ujian Dalaman</h1>
      <p>Laporan markah dan gred setiap murid mengikut kelas.</p>
    </div>

    <!-- Filter bar -->
    <div class="card no-print" style="margin-bottom:1.25rem">
      <div class="filter-grid">
        <div class="form-group" style="margin:0">
          <label class="form-label">Ujian</label>
          <select class="form-select" id="selUjian" onchange="onUjianChange()">
            <option value="">— Pilih Ujian —</option>
          </select>
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label">Kelas</label>
          <select class="form-select" id="selKelas" disabled>
            <option value="">— Pilih Kelas —</option>
          </select>
        </div>
        <div class="form-group" style="margin:0">
          <label class="form-label" style="visibility:hidden">Jana</label>
          <button class="btn btn-primary" id="btnJana" onclick="janaLaporan()" disabled style="width:100%">Jana Laporan</button>
        </div>
      </div>
    </div>

    <!-- Laporan section -->
    <div id="laporanSection" style="display:none">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.75rem;flex-wrap:wrap;gap:0.5rem" class="no-print">
        <div id="laporanLabel" style="font-size:0.88rem;font-weight:700;color:var(--primary)"></div>
        <button class="btn btn-ghost btn-sm" onclick="cetakLaporan()">Cetak</button>
      </div>

      <div class="lap-wrap">
        <table class="lap-table" id="lapTable">
          <thead id="lapHead"></thead>
          <tbody id="lapBody"></tbody>
        </table>
      </div>
    </div>
  </main>

  <script src="/app.js"></script>
  <script>
    requireAuth();
    renderSidebar('/laporan-ujian.html');
    loadTema();

    let _ujianData  = null;  // { ujian, kelas, subjek_list, gredScale, murid }
    let _allKelas   = [];

    async function init() {
      const [rUjian, rKelas] = await Promise.all([
        api('GET', '/ujian'),
        api('GET', '/kelas')
      ]);

      const ujianList = Array.isArray(rUjian) ? rUjian : (rUjian?.data || []);
      _allKelas = rKelas?.data || [];

      const selUjian = document.getElementById('selUjian');
      ujianList.forEach(u => {
        selUjian.innerHTML += `<option value="${u.id}">${u.nama_ujian} (${u.nama_sesi})</option>`;
      });
    }

    async function onUjianChange() {
      const ujian_id = document.getElementById('selUjian').value;
      const selKelas = document.getElementById('selKelas');
      selKelas.innerHTML = '<option value="">— Pilih Kelas —</option>';
      selKelas.disabled  = true;
      document.getElementById('btnJana').disabled = true;
      document.getElementById('laporanSection').style.display = 'none';

      if (!ujian_id) return;

      // Load detail ujian untuk dapat tahun-tahun yang terlibat
      const r = await api('GET', `/ujian/${ujian_id}`);
      const tahunList = [...new Set((r?.items || []).map(i => i.tahun))];

      // Filter kelas yang tahunnya ada dalam ujian ini
      const kelasBerkaitan = _allKelas.filter(k => tahunList.includes(k.tahun));
      selKelas.innerHTML = '<option value="">— Pilih Kelas —</option>' +
        kelasBerkaitan.map(k => `<option value="${k.id}">${k.nama_kelas} (Tahun ${k.tahun})</option>`).join('');
      selKelas.disabled = !kelasBerkaitan.length;
      selKelas.onchange = () => {
        document.getElementById('btnJana').disabled = !selKelas.value;
      };
    }

    async function janaLaporan() {
      const ujian_id = document.getElementById('selUjian').value;
      const kelas_id = document.getElementById('selKelas').value;
      if (!ujian_id || !kelas_id) return;

      const btn = document.getElementById('btnJana');
      setLoading(btn, true);
      const r = await api('GET', `/ujian-markah/laporan?ujian_id=${ujian_id}&kelas_id=${kelas_id}`);
      setLoading(btn, false);

      if (!r?.ok || !r?.data) { toast('Gagal jana laporan.', 'err'); return; }

      _ujianData = r.data;
      renderLaporan(_ujianData);

      const namaUjian = _ujianData.ujian.nama_ujian;
      const namaKelas = _ujianData.kelas.nama_kelas;
      document.getElementById('laporanLabel').textContent = `${namaUjian} — ${namaKelas}`;
      document.getElementById('laporanSection').style.display = 'block';
    }

    function renderLaporan({ ujian, kelas, subjek_list, gredScale, murid }) {
      const sortedGred = [...gredScale].sort((a, b) => b.markah_min - a.markah_min);
      const allGredLabel = sortedGred.map(g => g.gred); // ['A','B','C','D','E','F','TD']

      function kiraGredMurid(markah) {
        return appKiraGred(markah, gredScale);
      }

      // Kira stats per murid
      const muridStats = murid.map(m => {
        let jumlah_markah = 0;
        let jumlah_A = 0;
        const gredCount = {};
        allGredLabel.forEach(g => { gredCount[g] = 0; });

        subjek_list.forEach(s => {
          const markah = m.markah[s.id] ?? null;
          const gred   = kiraGredMurid(markah);
          if (markah !== null) jumlah_markah += markah;
          if (gred !== '—') {
            jumlah_A += (gred === 'A') ? 1 : 0;
            if (gredCount[gred] !== undefined) gredCount[gred]++;
          }
        });

        return { ...m, jumlah_markah, jumlah_A, gredCount };
      });

      // Ranking: jumlah_A DESC, jumlah_markah DESC
      muridStats.sort((a, b) => {
        if (b.jumlah_A !== a.jumlah_A) return b.jumlah_A - a.jumlah_A;
        return b.jumlah_markah - a.jumlah_markah;
      });
      muridStats.forEach((m, i) => { m.ked = i + 1; });

      // Render header
      const subjekHeader = subjek_list.map(s =>
        `<th colspan="2" style="background:#1e3a5f;color:#fff">${s.nama_subjek.toUpperCase()}</th>`
      ).join('');

      const subjekSubHeader = subjek_list.map(() =>
        `<th style="width:36px">M</th><th style="width:32px">G</th>`
      ).join('');

      document.getElementById('lapHead').innerHTML = `
        <tr>
          <th rowspan="2">BIL</th>
          <th rowspan="2" class="col-nama" style="text-align:left">NAMA MURID</th>
          ${subjekHeader}
          <th rowspan="2" style="min-width:60px">JUMLAH<br>MARKAH</th>
          <th rowspan="2" style="width:36px">KED</th>
          <th rowspan="2" class="td-keputusan" style="background:#f59e0b;color:#fff;min-width:140px">KEPUTUSAN IKUT GRED</th>
        </tr>
        <tr>${subjekSubHeader}</tr>
      `;

      // Render body
      document.getElementById('lapBody').innerHTML = muridStats.map((m, i) => {
        const selCells = subjek_list.map(s => {
          const markah = m.markah[s.id] ?? null;
          const gred   = kiraGredMurid(markah);
          const isGagal = gred === 'E' || gred === 'F';
          const isTD    = gred === 'TD' || markah === null;
          const markahCls = isGagal ? 'td-merah' : isTD ? 'td-kelabu' : '';
          const gredCls   = isGagal ? 'td-merah' : isTD ? 'td-kelabu' : '';
          const markahTxt = markah !== null ? markah : '';
          const gredTxt   = isTD ? 'TD' : gred === '—' ? '' : gred;
          return `<td class="${markahCls}">${markahTxt}</td><td class="${gredCls}">${gredTxt}</td>`;
        }).join('');

        const keputusan = sortedGred.map(g =>
          `${m.gredCount[g.gred] || 0}${g.gred}`
        ).join(' ');

        return `<tr>
          <td>${m.ked}</td>
          <td class="col-nama">${m.nama}</td>
          ${selCells}
          <td style="font-weight:700">${m.jumlah_markah}</td>
          <td style="font-weight:700;color:var(--primary)">${m.ked}</td>
          <td class="td-keputusan" style="font-size:0.68rem">${keputusan}</td>
        </tr>`;
      }).join('');
    }

    function cetakLaporan() {
      if (!_ujianData) return;
      const { ujian, kelas } = _ujianData;
      const tableHtml = document.getElementById('lapTable').outerHTML;

      const win = window.open('', '_blank');
      win.document.write(`<!DOCTYPE html><html><head>
        <meta charset="UTF-8">
        <title>Laporan ${ujian.nama_ujian} — ${kelas.nama_kelas}</title>
        <style>
          * { box-sizing: border-box; margin: 0; padding: 0; }
          body { font-family: Arial, sans-serif; padding: 1rem; font-size: 0.72rem; }
          h3   { font-size: 0.85rem; font-weight: 700; margin-bottom: 0.2rem; }
          p    { font-size: 0.7rem; color: #475569; margin-bottom: 0.75rem; }
          .lap-table { border-collapse: collapse; width: 100%; font-size: 0.68rem; }
          .lap-table th, .lap-table td {
            border: 1px solid #cbd5e1; padding: 0.2rem 0.3rem;
            text-align: center; white-space: nowrap;
          }
          .lap-table .col-nama { text-align: left; }
          .lap-table thead tr:first-child th { background: #1e3a5f !important; color: #fff; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
          .lap-table thead tr:last-child  th { background: #2563eb !important; color: #fff; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
          .td-merah  { color: #ef4444; font-weight: 700; }
          .td-kelabu { color: #94a3b8; }
          .td-keputusan { font-size: 0.6rem; text-align: left; }
          @page { size: landscape; margin: 1cm; }
        </style>
      </head><body>
        <h3>${ujian.nama_ujian} — ${kelas.nama_kelas} (${ujian.nama_sesi})</h3>
        <p>Markah Penuh: ${ujian.markah_penuh} | Dicetak: ${new Date().toLocaleDateString('ms-MY')}</p>
        ${tableHtml}
        <script>window.onload = () => { window.print(); }<\/script>
      </body></html>`);
      win.document.close();
    }

    init();
  </script>
</body>
</html>
```

- [ ] **Step 2: Tambah sidebar link dalam app.js**

Dalam `guruLinks` array, tambah selepas `'/ujian.html'`:

```js
{ href: '/laporan-ujian.html', label: 'Laporan Ujian' },
```

- [ ] **Step 3: Commit + push + test**

```bash
git add public/laporan-ujian.html public/app.js
git commit -m "feat: laporan-ujian.html — pivot markah × subjek, ranking A + jumlah, cetak"
git push origin test
```

Test manual di staging:
- Buka `/laporan-ujian.html`
- Pilih ujian → kelas dengan data → klik Jana Laporan
- Verify: kolum subjek betul, markah merah untuk E/F, TD untuk null
- Verify ranking: murid dengan lebih A dapat KED lebih rendah (lebih baik)
- Verify KEPUTUSAN IKUT GRED format: `3A 0B 0C 1D 0E 0F 5TD`
- Klik Cetak → print dialog muncul, landscape orientation

---

## Task 9: Apply Migrations Production + Merge Main

- [ ] **Step 1: Confirm staging berfungsi penuh**

Semak semua flows:
- Admin: tambah ujian, setup gred, tambah item, buka ujian
- Guru: pilih ujian → input markah → simpan
- Dashboard: analisis school-wide dengan pelbagai filter

- [ ] **Step 2: Apply migrations ke production DB**

```bash
npx wrangler d1 migrations apply mypwa-v2-db --remote --env production
```

- [ ] **Step 3: Merge ke main**

```bash
git checkout main
git merge test
git push origin main
git checkout test
```

- [ ] **Step 4: Update CLAUDE.md**

Tambah dokumentasi Fasa 8 dalam CLAUDE.md:
- Table progress Fasa 8: ✅ Done
- Section "Ujian Dalaman" dengan semua endpoints dan design decisions

---

## Nota Penting

| Perkara | Keputusan |
|---------|-----------|
| Skala gred | Sama untuk semua tahun dalam satu ujian |
| Dashboard akses | School-wide — semua guru boleh tengok semua subjek |
| Input markah | Terhad kepada `jadual_guru` guru → kelas+subjek intersection dengan `ujian_item` |
| Null markah | Dibenarkan — `markah = NULL` bermakna belum diisi (bukan sifar) |
| Cascade delete | Padam ujian → padam gred + item + markah secara automatik |
| Gred `TD` | Ditentukan oleh admin (contoh: markah_min=0, gred='TD') |
| `appKiraGred()` | Fungsi shared dalam app.js — digunakan oleh ujian.html, laporan-ujian.html dan dashboard.html |
| **Logik Ranking** | Sort 1: `jumlah_A DESC` (lebih banyak A = lebih tinggi). Sort 2: `jumlah_markah DESC` (tiebreaker) |
| Warna markah laporan | Gred E/F → merah, TD/null → kelabu, lain → hitam |
| Cetak laporan | Landscape `@page`, header biru, inline CSS untuk print color-adjust |
| Cetak analisis | Window baru, carta palang sama, auto print onload |
