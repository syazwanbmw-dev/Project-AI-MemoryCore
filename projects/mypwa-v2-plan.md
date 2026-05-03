# mypwa-v2 — Plan Pembangunan
*Dibuat: 2026-04-21 | Remake my-pwa, migrate Supabase → D1*

---

## Ringkasan Projek

Rebuild sistem eRPM (Rekod Perkembangan Murid) dari Supabase ke stack baru.
Sistem untuk **satu sekolah**, beberapa guru berkongsi sistem yang sama.

**Nama folder:** `mypwa-v2`
**Stack:** Hono.js · Cloudflare Workers · D1 (SQLite) · JWT · Vanilla HTML/JS · Tailwind CSS · SheetJS (XLSX)

---

## Roles

| Role | Siapa | Akses |
|------|-------|-------|
| `ADMIN` | Admin sekolah | Semua data — manage guru, kelas, murid, subjek, kurikulum, tetapan |
| `GURU` | Cikgu | Rekod sendiri sahaja, boleh lihat kelas & murid (shared) |

---

## Data Isolation

| Table | Isolation | Nota |
|-------|-----------|------|
| `pengguna` | Soft delete | Jangan padam — set status `BERHENTI` sahaja |
| `sesi` | Shared | Admin manage |
| `kelas` | Shared | Admin manage |
| `murid` | Shared | Admin manage |
| `subjek` | Shared | Admin manage |
| `kurikulum` | Shared | Admin manage |
| `tetapan` | Shared | Admin manage |
| `jadual_guru` | Per guru | Guru declare sendiri kelas + subjek yang diajar |
| `rekod` | **Per jadual_guru** | Papar rekod berdasarkan kelas+subjek dalam jadual_guru guru |

---

## Schema D1

```sql
-- 1. PENGGUNA
CREATE TABLE IF NOT EXISTS pengguna (
  id             INTEGER PRIMARY KEY AUTOINCREMENT,
  nama           TEXT NOT NULL,
  username       TEXT UNIQUE NOT NULL,
  password_hash  TEXT NOT NULL,          -- SHA-256, bukan plaintext
  role           TEXT DEFAULT 'GURU',    -- 'ADMIN' | 'GURU'
  status         TEXT DEFAULT 'AKTIF',   -- 'AKTIF' | 'SUSPEND' | 'BERHENTI'
  created_at     TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at     TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 2. SESI (Tahun akademik)
CREATE TABLE IF NOT EXISTS sesi (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  nama_sesi  TEXT NOT NULL UNIQUE,       -- contoh: '2025', '2026'
  aktif      INTEGER DEFAULT 0,          -- 1 = sesi semasa
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 3. KELAS (Shared, admin manage)
CREATE TABLE IF NOT EXISTS kelas (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  nama_kelas TEXT NOT NULL,              -- contoh: '3 Arif'
  tahun      TEXT NOT NULL,             -- contoh: '3' (tahun persekolahan)
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(nama_kelas)
);

-- 4. MURID (Shared, admin manage)
CREATE TABLE IF NOT EXISTS murid (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  id_kelas   INTEGER NOT NULL,
  nama       TEXT NOT NULL,
  no_kad     TEXT,
  jantina    TEXT,                       -- 'L' | 'P'
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY(id_kelas) REFERENCES kelas(id) ON DELETE CASCADE
);

-- 5. SUBJEK (Shared, admin manage)
CREATE TABLE IF NOT EXISTS subjek (
  id           INTEGER PRIMARY KEY AUTOINCREMENT,
  nama_subjek  TEXT NOT NULL,
  kod_subjek   TEXT,
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 6. KURIKULUM (Shared, admin manage — tajuk/SK/SP per subjek)
CREATE TABLE IF NOT EXISTS kurikulum (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  subjek_id  INTEGER NOT NULL,
  tahun      TEXT NOT NULL,             -- tahun persekolahan: '1'-'6'
  tajuk      TEXT,
  sk         TEXT NOT NULL,
  sp         TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY(subjek_id) REFERENCES subjek(id) ON DELETE CASCADE
);

-- 7. REKOD PRESTASI (Per guru — isolation via pengguna_id)
CREATE TABLE IF NOT EXISTS rekod (
  id                INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id       INTEGER NOT NULL,   -- WAJIB dari JWT token
  murid_id          INTEGER NOT NULL,
  subjek_id         INTEGER,
  nama_sesi         TEXT NOT NULL,      -- contoh: '2026'
  tajuk             TEXT NOT NULL,
  sk                TEXT NOT NULL DEFAULT 'UMUM',
  sp                TEXT DEFAULT '',
  tahap_penguasaan  INTEGER,            -- 1-6
  catatan           TEXT,
  tarikh            TEXT NOT NULL,      -- 'YYYY-MM-DD', default hari ini
  created_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(pengguna_id, murid_id, nama_sesi, subjek_id, tajuk, sk),
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id),   -- TIADA CASCADE — rekod kekal walaupun guru berhenti
  FOREIGN KEY(murid_id) REFERENCES murid(id) ON DELETE CASCADE,
  FOREIGN KEY(subjek_id) REFERENCES subjek(id) ON DELETE SET NULL
);

-- 8. JADUAL GURU (Guru declare sendiri — ajar subjek apa, kelas apa, sesi apa)
CREATE TABLE IF NOT EXISTS jadual_guru (
  id          INTEGER PRIMARY KEY AUTOINCREMENT,
  pengguna_id INTEGER NOT NULL,
  kelas_id    INTEGER NOT NULL,
  subjek_id   INTEGER NOT NULL,
  nama_sesi   TEXT NOT NULL,
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(pengguna_id, kelas_id, subjek_id, nama_sesi),
  FOREIGN KEY(pengguna_id) REFERENCES pengguna(id),
  FOREIGN KEY(kelas_id) REFERENCES kelas(id) ON DELETE CASCADE,
  FOREIGN KEY(subjek_id) REFERENCES subjek(id) ON DELETE CASCADE
);

-- 9. TETAPAN (School-wide settings)
CREATE TABLE IF NOT EXISTS tetapan (
  id         INTEGER PRIMARY KEY AUTOINCREMENT,
  kunci      TEXT NOT NULL UNIQUE,
  nilai      TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Seed tetapan untuk landing page (masukkan dalam seed.sql):
-- nama_sekolah    → nama sekolah (contoh: "SK Salor")
-- tagline         → slogan sistem (contoh: "Sistem Rekod Perkembangan Murid")
-- teks_sambutan   → teks hero (contoh: "Selamat datang, guru-guru...")
-- hero_image      → URL gambar hero (paste URL — boleh upgrade ke R2 kemudian)
-- warna_tema      → tema warna: 'navy'|'hijau'|'merah'|'langit'|'ungu'|'teal'
-- status_sistem   → 'BUKA' | 'TUTUP'
-- mesej_status    → mesej maintenance bila status TUTUP
--
-- NOTA UPGRADE R2: Bila ready, tukar nilai hero_image dari URL luar
-- ke R2 URL (https://pub-xxx.r2.dev/...). Tiada perubahan schema diperlukan.
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

`pengguna_id` **WAJIB** dari token — tidak boleh dari request body/params.

---

## Struktur Folder

```
mypwa-v2/
├── src/
│   ├── index.js              -- Hono entry + mount routes + serve HTML
│   ├── middleware/
│   │   └── auth.js           -- JWT verify, inject pengguna_id + role ke context
│   ├── routes/
│   │   ├── auth.js           -- POST /api/auth/login, /tukar-password
│   │   ├── pengguna.js       -- CRUD akaun guru (ADMIN sahaja)
│   │   ├── sesi.js           -- CRUD sesi akademik (ADMIN sahaja)
│   │   ├── kelas.js          -- CRUD kelas + auto-generate (ADMIN sahaja)
│   │   ├── murid.js          -- CRUD murid + bulk upload (ADMIN sahaja)
│   │   ├── subjek.js         -- CRUD subjek (ADMIN sahaja)
│   │   ├── kurikulum.js      -- CRUD kurikulum (ADMIN sahaja)
│   │   ├── rekod.js          -- CRUD rekod prestasi (query by jadual_guru)
│   │   ├── jadual-guru.js    -- Guru setup kelas + subjek yang diajar
│   │   └── tetapan.js        -- Read/update tetapan (ADMIN sahaja)
│   └── utils/
│       └── hash.js           -- SHA-256 password hash (Web Crypto API)
├── public/
│   ├── index.html            -- Login page
│   ├── dashboard.html        -- Home guru (greeting + stats rekod)
│   ├── rekod.html            -- Borang input rekod prestasi
│   ├── laporan.html          -- Laporan rekod guru sendiri
│   ├── tetapan.html          -- Guru setup jadual_guru (kelas + subjek yang diajar)
│   └── admin.html            -- Admin panel (semua management)
├── templates/
│   └── template-murid.xlsx   -- Template Excel untuk import murid
├── migrations/
│   └── 001_initial.sql       -- Schema penuh
├── schema.sql                -- Reference
├── seed.sql                  -- ADMIN account default + sesi semasa
├── .gitignore
├── wrangler.toml
└── package.json
```

---

## Pages & UX

### Pages Guru

| Page | Kandungan |
|------|-----------|
| `dashboard.html` | Greeting + analytics per subjek per sesi (purata TP, % murid TP≥4) + overall per subjek |
| `rekod.html` | Borang input rekod — pilih kelas → murid → subjek → tajuk/SK → TP |
| `laporan.html` | Detail rekod — filter kelas + subjek + sesi → table murid vs tajuk/SK |
| `tetapan.html` | Setup jadual_guru — tambah/padam kelas + subjek yang diajar |

### UX Flow

```
index.html (login)
    ↓
dashboard.html
    → "Hai, Cikgu [Nama]!"
    → Kalau tiada jadual_guru: "Sila setup jadual mengajar anda" + button → tetapan.html
    → Kalau tiada rekod: "Anda belum buat rekod perkembangan murid" + button → rekod.html
    → Kalau ada rekod: analytics cards per subjek per sesi
         - Purata TP keseluruhan
         - % murid capai TP ≥ 4
         - Breakdown per sesi (2024, 2025, 2026...)
    → Sidebar: Dashboard | Input Rekod | Laporan | Tetapan
    ↓
rekod.html
    → Pilih sesi → kelas → murid → subjek → tajuk/SK/SP
    → Tarikh: auto = hari ini, boleh ubah
    → Simpan → toast "Berjaya!" → reset borang → guru terus input
    → Button "Balik Dashboard"
    ↓
laporan.html
    → Filter: sesi + kelas + subjek
    → Table: murid (baris) × tajuk/SK (lajur) — nilai TP dalam setiap sel
    → Boleh print / export Excel
```

### Admin
```
index.html (login)
    ↓
admin.html
    → Sidebar: Dashboard | Pengguna | Kelas & Murid | Subjek & Kurikulum | Tetapan
    → Manage semua data sekolah
```

### Kelas — Auto-generate
```
Admin masuk: Tahun [1,2,3,4,5,6] + Nama kelas [Arif, Bestari, Cemerlang]
→ System generate: 1 Arif, 1 Bestari, 1 Cemerlang, 2 Arif... (18 kelas)
→ Atau manual tambah satu-satu kalau ada nama khas
```

---

## API Endpoints

### Auth (Public)
- `POST /api/auth/login` — { username, password } → { token }
- `POST /api/auth/tukar-password` — tukar password sendiri (auth required)

### Pengguna (ADMIN sahaja)
- `GET /api/pengguna` — senarai semua guru
- `POST /api/pengguna` — tambah guru baru
- `PUT /api/pengguna/:id` — edit guru
- `PUT /api/pengguna/:id/status` — tukar status guru: AKTIF / SUSPEND / BERHENTI (tiada hard delete)
- `POST /api/pengguna/:id/reset-password` — reset password guru

### Sesi (ADMIN sahaja)
- `GET /api/sesi` — senarai sesi
- `POST /api/sesi` — tambah sesi baru
- `PUT /api/sesi/:id` — set aktif / edit
- `DELETE /api/sesi/:id` — padam sesi

### Kelas (ADMIN sahaja)
- `GET /api/kelas` — senarai kelas
- `POST /api/kelas` — tambah kelas (manual)
- `POST /api/kelas/generate` — auto-generate dari pattern
- `PUT /api/kelas/:id` — edit kelas
- `DELETE /api/kelas/:id` — padam kelas (cascade murid)

### Murid (ADMIN sahaja)
- `GET /api/murid?kelas_id=` — senarai murid
- `POST /api/murid` — tambah murid (satu)
- `POST /api/murid/bulk` — upload batch dari XLSX (JSON array)
- `PUT /api/murid/:id` — edit murid
- `DELETE /api/murid/:id` — padam murid

### Subjek (ADMIN sahaja)
- `GET /api/subjek` — senarai subjek
- `POST /api/subjek` — tambah subjek
- `PUT /api/subjek/:id` — edit subjek
- `DELETE /api/subjek/:id` — padam subjek

### Kurikulum (ADMIN sahaja)
- `GET /api/kurikulum?subjek_id=&tahun=` — senarai kurikulum
- `POST /api/kurikulum` — tambah tajuk/SK/SP
- `PUT /api/kurikulum/:id` — edit
- `DELETE /api/kurikulum/:id` — padam

### Rekod (Auth required)
- `GET /api/rekod?kelas_id=&subjek_id=&nama_sesi=` — GURU: rekod dalam jadual_guru dia | ADMIN: semua
- `POST /api/rekod` — simpan/upsert (pengguna_id dari JWT)
- `DELETE /api/rekod/:id` — padam (GURU: own sahaja | ADMIN: semua)

### Jadual Guru (Auth: GURU)
- `GET /api/jadual-guru` — senarai jadual guru sendiri
- `POST /api/jadual-guru` — tambah (pilih kelas + subjek + sesi)
- `DELETE /api/jadual-guru/:id` — padam jadual

### Tetapan (ADMIN sahaja)
- `GET /api/tetapan` — baca semua tetapan
- `PUT /api/tetapan/:kunci` — update nilai

---

## Data Integrity Rules

### Cascade DELETE — Rantaian Padam

| Padam | Kesan Automatik |
|-------|----------------|
| `kelas` | Semua `murid` dalam kelas tu padam → semua `rekod` murid tu padam |
| `murid` | Semua `rekod` murid tu padam |
| `pengguna` (guru) | **TIADA CASCADE** — rekod kekal, guru set `status = 'BERHENTI'` |
| `subjek` | `kurikulum` berkaitan padam, `rekod.subjek_id` jadi NULL (data selamat) |

### Peraturan Backend — Wajib Sebelum DELETE

Sebelum padam mana-mana shared data, backend **WAJIB** check dan pulangkan warning:

```
DELETE /api/kelas/:id
→ Semak: berapa murid dalam kelas ni?
→ Semak: berapa rekod akan affected?
→ Pulangkan: { murid_count: 12, rekod_count: 84, warning: "..." }
→ Frontend tunjuk confirmation dialog dengan info ni
```

Sama untuk:
- `DELETE /api/murid/:id` → check berapa rekod akan padam
- `DELETE /api/pengguna/:id` → check berapa rekod guru akan padam
- `DELETE /api/subjek/:id` → check berapa kurikulum + rekod affected

### Frontend — Confirmation Dialog Wajib

Setiap butang DELETE pada shared data **WAJIB** tunjuk confirmation dengan maklumat jelas:

```
⚠ Padam Kelas 3 Arif?

Tindakan ini akan memadam:
• 25 murid dalam kelas ini
• 312 rekod prestasi berkaitan

Tindakan ini TIDAK BOLEH dibatalkan.

[Batal]  [Ya, Padam]
```

Butang "Ya, Padam" hanya aktif selepas user taip semula nama kelas (untuk kelas dengan rekod > 50).

---

## Security Checklist

- [ ] Tiada hardcoded credentials dalam code
- [ ] `.env` dan `.dev.vars` dalam `.gitignore`
- [ ] Password di-hash (SHA-256 via Web Crypto API)
- [ ] JWT secret dalam environment variable (bukan hardcode)
- [ ] Semua routes protected kecuali `/api/auth/login`
- [ ] `pengguna_id` SENTIASA dari JWT token — tidak boleh dari request body
- [ ] ADMIN routes check `role === 'ADMIN'` — return 403 kalau bukan
- [ ] GURU hanya boleh akses rekod sendiri (`WHERE pengguna_id = token.pengguna_id`)
- [ ] Unauthorized request → redirect ke `index.html`
- [ ] Token expired → redirect ke login
- [ ] CASCADE DELETE pada semua FK — tiada orphan data
- [ ] Backend check count sebelum DELETE shared data — pulangkan warning ke frontend
- [ ] Frontend tunjuk confirmation dialog dengan bilangan rekod affected
- [ ] D1 foreign key enforcement aktif (`PRAGMA foreign_keys = ON` dalam migration)
- [ ] `pengguna` TIDAK BOLEH dipadam dari DB — admin hanya boleh set `status = 'BERHENTI'`
- [ ] Rekod guru `BERHENTI` kekal dalam DB — admin boleh rujuk, guru baru boleh tengok via admin panel

---

## Data Migration Map (Supabase → D1)

| Supabase | D1 (mypwa-v2) | Perubahan |
|----------|---------------|-----------|
| `data_guru` | `pengguna` | rename, hash password (plaintext → SHA-256) |
| `data_sesi` | `sesi` | rename |
| `data_kelas` | `kelas` | rename, remove pengguna_id (jadi shared) |
| `senarai_murid` | `murid` | rename, semak FK ke kelas |
| `data_subjek` | `subjek` | rename, remove pengguna_id (jadi shared) |
| `data_kurikulum` | `kurikulum` | rename, remove pengguna_id (jadi shared) |
| `rekod` | `rekod` | tambah tarikh field, semak pengguna_id betul |
| `tetapan` | `tetapan` | rename, ubah struktur key-value |

**Langkah migration:**
1. Export semua data dari Supabase (JSON/CSV)
2. Transform: rename fields, hash semua password
3. Semak FK integrity (kelas_id dalam murid betul ke?)
4. Import ke D1 via wrangler
5. Verify data — count rows, semak sample rekod

---

## Packages Diperlukan

| Package | Tujuan |
|---------|--------|
| `hono` | Web framework |
| `jose` | JWT sign + verify |
| `sheetjs` (xlsx) | Parse XLSX dalam browser (CDN, zero server cost) |

---

## Fasa Pelaksanaan

### Fasa 1 — Setup Foundation

#### 1a. Git & GitHub
- [ ] Init git dalam folder `mypwa-v2`
- [ ] Buat `.gitignore` (cover `.env`, `.dev.vars`, wrangler secrets)
- [ ] Buat GitHub repo baru: `mypwa-v2` (private)
- [ ] Push initial commit ke `main`
- [ ] Buat `test` branch dari `main`

#### 1b. GitHub Actions (Auto Deploy)
- [ ] Buat `.github/workflows/deploy.yml`
  - Push ke `test` → deploy ke Cloudflare Workers (environment: test)
  - Push ke `main` → deploy ke Cloudflare Workers (environment: production)
- [ ] Tambah secrets dalam GitHub repo:
  - `CLOUDFLARE_API_TOKEN`
  - `CLOUDFLARE_ACCOUNT_ID`

#### 1c. Cloudflare Workers Setup
- [ ] Setup `package.json` (hono, jose, wrangler)
- [ ] Buat `wrangler.toml` (nama: `mypwa-v2`, binding D1)
- [ ] Create D1 database: `wrangler d1 create mypwa-v2-db`
- [ ] Buat `migrations/001_initial.sql` (schema penuh)
- [ ] Run migration: `wrangler d1 execute mypwa-v2-db --file migrations/001_initial.sql --remote`
- [ ] Buat `seed.sql` (ADMIN account default + sesi semasa + tetapan landing page)
- [ ] Run seed: `wrangler d1 execute mypwa-v2-db --file seed.sql --remote`
- [ ] Test deploy: `wrangler deploy` → dapat URL `mypwa-v2.workers.dev`
- [ ] Commit: `init: setup mypwa-v2 project`

### Fasa 2 — Backend Core
- [ ] `src/utils/hash.js` — SHA-256 hash
- [ ] `src/middleware/auth.js` — JWT verify, inject user ke context
- [ ] `src/routes/auth.js` — login + tukar-password
- [ ] `src/index.js` — Hono app, mount routes, serve HTML
- [ ] Test login dengan Hoppscotch
- [ ] Commit: `feat: auth system`

### Fasa 3 — Admin Routes
- [ ] `routes/pengguna.js`
- [ ] `routes/sesi.js`
- [ ] `routes/kelas.js` (termasuk auto-generate)
- [ ] `routes/murid.js` (termasuk bulk upload)
- [ ] `routes/subjek.js`
- [ ] `routes/kurikulum.js`
- [ ] `routes/tetapan.js`
- [ ] Commit: `feat: admin routes`

### Fasa 4 — Guru Routes
- [ ] `routes/rekod.js` (upsert, query by jadual_guru)
- [ ] `routes/jadual-guru.js` (guru setup kelas + subjek)
- [ ] Commit: `feat: rekod routes`

### Fasa 5 — Frontend
- [ ] `public/index.html` — login page
- [ ] `public/dashboard.html` — home guru (sidebar + greeting + stats)
- [ ] `public/rekod.html` — borang rekod (tarikh auto today, reset after save)
- [ ] `public/laporan.html` — laporan rekod
- [ ] `public/tetapan.html` — guru setup jadual_guru (kelas + subjek yang diajar)
- [ ] `public/admin.html` — admin panel (sidebar + semua management)
- [ ] `templates/template-murid.xlsx` — template import
- [ ] Commit: `feat: frontend complete`

### Fasa 6 — Data Migration
- [ ] Export data dari Supabase (JSON)
- [ ] Tulis migration script — transform + hash password
- [ ] Import ke D1 (`wrangler d1 execute --remote`)
- [ ] Verify data integrity
- [ ] Commit: `data: migrate dari Supabase`

### Fasa 7 — Deploy & Cutover

#### 7a. Final Review
- [ ] `sight-hone` review
- [ ] `commit-seal`
- [ ] Push ke `test` branch → verify semua fungsi di `mypwa-v2.workers.dev`

#### 7b. Domain Cutover (Zero Downtime)
- [ ] Verify data migration betul dalam production D1
- [ ] Cloudflare Pages dashboard → `erpm-sksalor.pages.dev` → **buang custom domain** `erpm-sksalor.celikguru.my`
- [ ] Cloudflare Workers dashboard → `mypwa-v2` → **tambah custom domain** `erpm-sksalor.celikguru.my`
- [ ] Tunggu DNS propagate (< 1 minit dalam Cloudflare)
- [ ] Verify `https://erpm-sksalor.celikguru.my` → sistem baru
- [ ] Merge `test` → `main` → production deploy otomatik

---

## Deployment Info

| | Semasa Develop | Selepas Cutover |
|---|---|---|
| **URL test** | `mypwa-v2.workers.dev` | - |
| **URL live** | `erpm-sksalor.celikguru.my` (sistem lama) | `erpm-sksalor.celikguru.my` (sistem baru) |
| **Hosting lama** | Cloudflare Pages (`erpm-sksalor.pages.dev`) | Tidak aktif (fail masih ada) |
| **Hosting baru** | Cloudflare Workers | Cloudflare Workers |

## Catatan

- Projek my-pwa **masih live** semasa development — jangan sentuh Supabase data
- Folder `mypwa-v2` baru — tiada kaitan dengan `my-pwa` yang sedia ada
- Semasa develop: guna `mypwa-v2.workers.dev` untuk test
- Domain cutover hanya dilakukan setelah **data migration selesai dan verified**
- `wrangler d1 execute --remote` untuk test dengan real D1 database
