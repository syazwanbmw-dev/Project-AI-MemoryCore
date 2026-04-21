# eRPM v2 — SaaS Migration Plan
*Dibuat: 2026-04-13 | Produk celikguru.my #2*

---

## Ringkasan Projek

Migrate sistem eRPM dari Supabase (hardcoded credentials, plaintext password) ke stack baru sebagai produk SaaS. Setiap guru adalah tenant sendiri — data isolation per guru, bukan per sekolah.

**Nama folder:** `erpm-v2`
**Stack:** Hono.js · Cloudflare Workers · D1 (SQLite) · JWT · Vanilla HTML/JS · Tailwind CSS · SheetJS (XLSX parse)

---

## Roles

| Role | Siapa | Akses |
|------|-------|-------|
| `MASTER` | Master sendiri | Semua — lulus permohonan, reset credential, archive data |
| `GURU` | Cikgu yang daftar | Data sendiri sahaja |

---

## Data Isolation

**Isolation key: `pengguna_id`** (bukan sekolah_id)

Setiap table (kecuali `pengguna` dan `permohonan`) mesti ada:

```sql
pengguna_id  INTEGER NOT NULL,                    -- tenant isolation
created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
updated_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
created_by   INTEGER                              -- audit trail
```

`pengguna_id` dalam setiap query **WAJIB** diambil dari JWT token — tidak pernah dari request body/params.

---

## Schema D1

```sql
-- 1. PENGGUNA (MASTER + GURU)
CREATE TABLE IF NOT EXISTS pengguna (
  id             INTEGER PRIMARY KEY AUTOINCREMENT,
  nama           TEXT NOT NULL,
  username       TEXT UNIQUE NOT NULL,       -- global unique
  password_hash  TEXT NOT NULL,              -- SHA-256, bukan plaintext
  role           TEXT DEFAULT 'GURU',        -- 'MASTER' | 'GURU'
  nama_sekolah   TEXT,
  kod_sekolah    TEXT,
  email          TEXT,
  telefon        TEXT,
  status         TEXT DEFAULT 'PENDING',     -- 'PENDING'|'AKTIF'|'SUSPEND'
  created_at     TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at     TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. PERMOHONAN (Log kelulusan)
CREATE TABLE IF NOT EXISTS permohonan (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id  INTEGER NOT NULL,
  status       TEXT DEFAULT 'PENDING',       -- 'PENDING'|'LULUS'|'TOLAK'
  catatan      TEXT,                         -- sebab tolak (optional)
  diproses_at  TIMESTAMP,
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id)
);

-- 3. KELAS
CREATE TABLE IF NOT EXISTS kelas (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id INTEGER NOT NULL,
  nama_kelas  TEXT NOT NULL,
  tahun       TEXT NOT NULL,
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  created_by  INTEGER,
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id)
);

-- 4. MURID
CREATE TABLE IF NOT EXISTS murid (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id INTEGER NOT NULL,
  id_kelas    INTEGER NOT NULL,
  nama        TEXT NOT NULL,
  no_kad      TEXT,
  jantina     TEXT,
  tahun       TEXT,
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  created_by  INTEGER,
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id),
  FOREIGN KEY(id_kelas) REFERENCES kelas(id)
);

-- 5. SUBJEK
CREATE TABLE IF NOT EXISTS subjek (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id  INTEGER NOT NULL,
  nama_subjek  TEXT NOT NULL,
  kod_subjek   TEXT,
  tahun        TEXT,
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  created_by   INTEGER,
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id)
);

-- 6. KURIKULUM (Template global + customized per guru)
CREATE TABLE IF NOT EXISTS kurikulum (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id  INTEGER,          -- NULL = template global oleh MASTER
  subjek_id    INTEGER,
  tahun        TEXT NOT NULL,
  tajuk        TEXT,
  sk           TEXT NOT NULL,
  sp           TEXT,
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  created_by   INTEGER,
  FOREIGN KEY(subjek_id) REFERENCES subjek(id)
);

-- 7. REKOD PRESTASI (Core feature)
CREATE TABLE IF NOT EXISTS rekod (
  id                INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id       INTEGER NOT NULL,
  murid_id          INTEGER NOT NULL,
  subjek_id         INTEGER,
  tahun             TEXT NOT NULL,
  tajuk             TEXT NOT NULL,
  sk                TEXT NOT NULL DEFAULT 'UMUM',
  sp                TEXT DEFAULT '',
  tahap_penguasaan  INTEGER,
  catatan           TEXT,
  created_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  created_by        INTEGER,
  UNIQUE(pengguna_id, murid_id, tahun, subjek_id, tajuk, sk),
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id),
  FOREIGN KEY(murid_id) REFERENCES murid(id)
);

-- 8. TETAPAN (Per guru)
CREATE TABLE IF NOT EXISTS tetapan (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id  INTEGER NOT NULL,
  kunci        TEXT NOT NULL,
  nilai        TEXT,
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(pengguna_id, kunci),
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id)
);

-- 9. ARKIB (Data lama yang di-archive oleh MASTER)
CREATE TABLE IF NOT EXISTS arkib (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id  INTEGER NOT NULL,
  tahun        TEXT NOT NULL,
  jenis        TEXT NOT NULL,    -- 'rekod'|'murid'|'kelas'
  data_json    TEXT NOT NULL,    -- snapshot data dalam JSON
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id)
);
```

---

## JWT Token Payload

```json
{
  "pengguna_id": 1,
  "role": "GURU",
  "nama": "Ahmad bin Ali",
  "exp": 1234567890
}
```

`pengguna_id` sentiasa dari token — **tidak boleh dihantar dalam request body**.

---

## Struktur Folder

```
erpm-v2/
├── src/
│   ├── index.js              -- Hono entry + mount routes + serve HTML
│   ├── middleware/
│   │   └── auth.js           -- JWT verify, inject pengguna_id + role ke context
│   ├── routes/
│   │   ├── auth.js           -- POST /api/auth/login, /daftar, /logout
│   │   ├── master.js         -- Endpoints MASTER (permohonan, reset, archive)
│   │   ├── kelas.js          -- CRUD kelas
│   │   ├── murid.js          -- CRUD murid + XLSX upload
│   │   ├── subjek.js         -- CRUD subjek
│   │   ├── kurikulum.js      -- CRUD kurikulum (global + per guru)
│   │   └── rekod.js          -- CRUD rekod prestasi (upsert)
│   └── utils/
│       └── hash.js           -- SHA-256 password hash (Web Crypto API)
├── public/
│   ├── index.html            -- Landing page + borang daftar
│   ├── login.html            -- Login page
│   ├── dashboard.html        -- Input rekod + laporan guru
│   ├── tetapan.html          -- Setup kelas, murid, subjek, kurikulum
│   └── master.html           -- Panel MASTER
├── templates/
│   └── template-murid.xlsx   -- Template Excel untuk guru download
├── migrations/
│   └── 001_initial.sql       -- Schema lengkap
├── schema.sql                -- Reference
├── seed.sql                  -- MASTER account + kurikulum global demo
├── .gitignore                -- Wajib cover credentials
├── wrangler.toml
└── package.json
```

---

## API Endpoints

### Auth (Public)
- `POST /api/auth/daftar` — borang daftar guru (status: PENDING)
- `POST /api/auth/login` — { username, password } → { token }
- `POST /api/auth/tukar-password` — tukar password sendiri

### MASTER (role: MASTER sahaja)
- `GET /api/master/permohonan` — senarai permohonan PENDING
- `PUT /api/master/permohonan/:id` — lulus/tolak
- `POST /api/master/reset-credential/:id` — reset username + password guru
- `POST /api/master/archive` — archive data tahun tertentu
- `GET /api/master/archive/:pengguna_id/:tahun` — download data archive

### Kelas (Auth: GURU)
- `GET /api/kelas?tahun=` — senarai kelas guru
- `POST /api/kelas` — tambah kelas
- `PUT /api/kelas/:id` — edit kelas
- `DELETE /api/kelas/:id` — padam kelas

### Murid (Auth: GURU)
- `GET /api/murid?id_kelas=` — senarai murid
- `POST /api/murid` — tambah murid (satu)
- `POST /api/murid/bulk` — upload batch dari XLSX (JSON array)
- `PUT /api/murid/:id` — edit murid
- `DELETE /api/murid/:id` — padam murid

### Subjek (Auth: GURU)
- `GET /api/subjek?tahun=` — senarai subjek
- `POST /api/subjek` — tambah
- `PUT /api/subjek/:id` — edit
- `DELETE /api/subjek/:id` — padam

### Kurikulum (Auth: GURU)
- `GET /api/kurikulum?tahun=&subjek_id=` — ambil (global + milik guru)
- `POST /api/kurikulum` — tambah SK/SP
- `PUT /api/kurikulum/:id` — edit
- `DELETE /api/kurikulum/:id` — padam (hanya milik sendiri)

### Rekod (Auth: GURU)
- `GET /api/rekod?murid_id=&tahun=` — rekod prestasi
- `POST /api/rekod` — simpan/upsert
- `DELETE /api/rekod/:id` — padam

---

## Workflow Pengguna

### Guru Baru
```
1. Buka landing page → klik "Daftar"
2. Isi: nama, nama sekolah, kod sekolah, email, telefon, username, password
3. Submit → status PENDING
4. MASTER terima notifikasi → semak → LULUS
5. Guru login → setup kelas, upload murid (XLSX), tambah subjek
6. Mula input rekod prestasi
```

### MASTER (Daily)
```
1. Login master.html
2. Semak senarai permohonan PENDING
3. Lulus atau tolak (dengan catatan)
4. Bila guru ada masalah: reset credential
5. Hujung tahun: archive data lama by year
```

### Upload Murid (XLSX)
```
1. Guru download template-murid.xlsx
2. Isi nama murid dalam Excel
3. Upload dalam tetapan.html
4. SheetJS parse dalam browser → JSON
5. POST /api/murid/bulk → simpan D1
```

---

## Fasa Pelaksanaan

### Fasa 1 — Setup Foundation
- [ ] Buat folder `erpm-v2`, init git
- [ ] Setup `package.json` (hono, jose, wrangler)
- [ ] Buat `.gitignore`
- [ ] Buat `wrangler.toml`
- [ ] Buat `migrations/001_initial.sql` (schema penuh)
- [ ] Create D1 database via wrangler
- [ ] Run migration
- [ ] Buat `seed.sql` (MASTER account)
- [ ] Commit: `init: setup erpm-v2 project`

### Fasa 2 — Backend Core
- [ ] Buat `src/utils/hash.js`
- [ ] Buat `src/middleware/auth.js`
- [ ] Buat `src/routes/auth.js` (daftar + login)
- [ ] Buat `src/index.js` (Hono app)
- [ ] Test login dengan Hoppscotch
- [ ] Commit: `feat: auth system`

### Fasa 3 — GURU Routes
- [ ] `routes/kelas.js`
- [ ] `routes/murid.js` (termasuk bulk upload)
- [ ] `routes/subjek.js`
- [ ] `routes/kurikulum.js`
- [ ] `routes/rekod.js`
- [ ] Mount semua dalam `index.js`
- [ ] Commit: `feat: semua CRUD routes`

### Fasa 4 — MASTER Routes
- [ ] `routes/master.js` (permohonan, reset, archive)
- [ ] Commit: `feat: master admin routes`

### Fasa 5 — Frontend
- [ ] `public/index.html` (landing + daftar)
- [ ] `public/login.html`
- [ ] `public/tetapan.html` (setup kelas, murid XLSX upload, subjek, kurikulum)
- [ ] `public/dashboard.html` (input rekod + laporan)
- [ ] `public/master.html` (panel MASTER)
- [ ] Buat `templates/template-murid.xlsx`
- [ ] Commit: `feat: frontend complete`

### Fasa 6 — Data Migration
- [ ] Export data dari Supabase
- [ ] Transform: map table lama → baru, assign pengguna_id
- [ ] Hash semua password lama
- [ ] Import ke D1
- [ ] Verify data
- [ ] Commit: `data: migrate dari Supabase`

### Fasa 7 — Deploy
- [ ] sight-hone review
- [ ] commit-seal
- [ ] Push ke test → verify
- [ ] Merge ke main → production

---

## Security Checklist
- [ ] Tiada hardcoded credentials dalam code
- [ ] Password di-hash (SHA-256 via Web Crypto API)
- [ ] `pengguna_id` SENTIASA dari JWT token
- [ ] Semua routes (kecuali auth) ada middleware JWT
- [ ] MASTER endpoints check `role === 'MASTER'`
- [ ] `.gitignore` cover secrets

---

## Packages Diperlukan
| Package | Tujuan |
|---------|--------|
| `hono` | Web framework |
| `jose` | JWT sign + verify |
| `sheetjs` (xlsx) | Parse XLSX dalam browser (client-side, zero server cost) |

*Nota: SheetJS diload dari CDN dalam HTML — tidak perlu install dalam Worker*

---

## Data Migration Map
| Supabase | D1 (erpm-v2) | Perubahan |
|----------|-------------|-----------|
| `data_guru` | `pengguna` | rename, hash password, assign pengguna_id |
| `data_kelas` | `kelas` | tambah pengguna_id, timestamps |
| `senarai_murid` | `murid` | rename, kelas text → id_kelas FK |
| `data_subjek` | `subjek` | tambah pengguna_id |
| `data_kurikulum` | `kurikulum` | tambah pengguna_id, restructure |
| `rekod` | `rekod` | tambah pengguna_id |
| `tetapan` | `tetapan` | tambah pengguna_id, ubah struktur |
