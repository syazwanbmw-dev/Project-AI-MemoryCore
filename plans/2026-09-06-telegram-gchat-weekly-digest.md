# Spec — Digest Aktiviti Mingguan ke Telegram + Google Chat

**Projek:** `takwim-digital` (Google Apps Script + Google Calendar)
**Tarikh:** 2026-09-06
**Status:** Diluluskan master 2026-09-06. **Rev. 1 (2026-09-06 malam):** keputusan #3 diubah —
satu toggle → dua checkbox bebas per saluran (master perlu hantar ke satu saluran sahaja
kadang-kala). Plan sedang direvise.
**Klasifikasi:** Architectural (subsistem baharu — saluran keluar ke audiens luar domain)

---

## 1. Latar & matlamat

Sistem sedia ada hantar peringatan aktiviti secara **emel** kepada **guru** (`sendActivityReminders_`,
LIVE `@22`). Tiada saluran ke **ibu bapa** atau **murid**.

Master mahu: sekali seminggu, hantar senarai aktiviti sekolah yang akan datang ke —

- **Group Telegram sedia ada** ibu bapa (satu atau lebih)
- **Google Chat Space sedia ada** "info rasmi murid sk salor" (semua murid, akaun DELIMa)

Satu digest, kandungan sama ke semua sasaran. Satu hala (broadcast) — bukan langganan
per-orang.

## 2. Skop

**Dalam skop**
- Penanda per-aktiviti **per-saluran**: dua checkbox bebas ("Kongsi ke Telegram" +
  "Kongsi ke Google Chat") dalam borang aktiviti
- Enjin digest mingguan berjadual + trigger boleh-set (hari/jam/minit)
- Penghantar Telegram (Bot API) + penghantar Google Chat (incoming webhook)
- Tetapan baharu di **System Settings sahaja** (bukan Setup Wizard — konsisten dengan
  `REMINDER_HOUR` sedia ada)
- Kemas kini dokumentasi (SETUP + PANDUAN-GURU)
- Ujian helper tulen + mutation test

**Luar skop (YAGNI)**
- Langganan / opt-out per-orang, tangkap `chat_id` automatik, endpoint `doPost`
- Kandungan berbeza per group (peta aktiviti → kelas)
- WhatsApp (perlu Business API berbayar + kelulusan Meta)
- Peringatan H-1 per aktiviti ke parent/murid (digest mingguan sahaja)
- Kemas kini susulan bila aktiviti bertanda "kongsi" diubah/dibatal selepas digest dihantar
  (digest = snapshot mingguan)

## 3. Keputusan reka bentuk (dari sesi Q&A 2026-09-06)

| # | Keputusan | Sebab |
|---|-----------|-------|
| 1 | Telegram: bot dalam **group sedia ada**, bukan channel baharu | Paling ringkas; tiada pangkalan data parent, tiada deployment kedua |
| 2 | **Banyak group Telegram**, digest **sama** ke semua | Senarai `chat_id` dipisah koma, loop best-effort |
| 3 | **Dua checkbox bebas** per aktiviti: "Kongsi ke Telegram" + "Kongsi ke Google Chat", lalai kedua-dua **OFF**. Label ringkas — tiada "(ibu bapa)"/"(murid)" | **Rev.1 2026-09-06 malam** — ganti "satu toggle" asal. Master perlu kawalan berasingan: ada masa nak hantar ke satu saluran sahaja |
| 4 | Simpan sebagai **satu baris `Kongsi: tg,gchat`** (senarai saluran) dalam `description` event | Corak sama `Reminder:` / `RemindTo:` — Calendar tiada custom field. Senarai (bukan dua baris berasingan) supaya boleh tambah saluran nanti tanpa ubah format |
| 5 | **Digest mingguan sahaja**, tetingkap **7 hari akan datang** | Master pilih A (bukan +H-1, bukan preview minggu depan) |
| 6 | Lalai **Ahad 07:45**; hari + jam + minit **boleh admin set** di UI | Corak sama `REMINDER_HOUR` |
| 7 | Kandungan: **tajuk + hari/tarikh + lokasi** sahaja | Tiada PIC/agensi/nota dalaman ke sasaran luar |
| 8 | Minggu tiada aktiviti bertanda "kongsi" → **skip senyap** | Group tak kena spam mesej kosong |
| 9 | **Google Chat disambung dalam v1** (bukan follow-up) | Space "semua murid" memang wujud — audiens sebenar, bukan spekulasi |
| 10 | Enjin **sink-agnostik**: setiap sink **tapis senarai aktiviti sendiri** (`shareChannels`), `buildDigestText_()` dipanggil sekali per sink dengan senarai tersendiri | Kandungan boleh beza antara saluran; tambah/buang sink kemudian = kos kecil |

## 4. Seni bina & komponen

Semua dalam `Code.js` + `Index.html`. **Tiada fail baharu**, tiada library baharu.
`UrlFetchApp` kali pertama dipakai dalam projek ni.

```
Trigger masa (mingguan, onWeekDay.atHour.nearMinute)
        │
        ▼
sendWeeklyDigest_()                     ← handler trigger
        │
        ├─ getConfig_()                 ← guard: ada token+chatIds ATAU webhooks?
        ├─ events = safeGetEvents_(getPPDCalendar_(), today, today+7h).map(eventToObject_)
        │           └─ e.shareChannels = parseShareChannels_(desc)  → ['tg'] / ['gchat'] / ['tg','gchat'] / []
        ├─ tgEvents    = events.filter(e => e.shareChannels.indexOf('tg')    !== -1)
        ├─ gchatEvents = events.filter(e => e.shareChannels.indexOf('gchat') !== -1)
        ├─ (tgEvents kosong DAN gchatEvents kosong) → return  (tak set penanda)
        ├─ DGSENT_<isoWeek> wujud? → return
        ├─ sink Telegram (kalau token+chatIds & tgEvents.length):
        │        sendToTelegram_(buildDigestText_(tgEvents, cfg), token, chatIdsCsv)
        ├─ sink GChat (kalau webhooks & gchatEvents.length):
        │        sendToGoogleChat_(buildDigestText_(gchatEvents, cfg), webhooksCsv)
        ├─ setProperty DGSENT_<isoWeek>
        └─ pruneDigestMarkers_()   ← simpan 8 minggu terakhir
```

**Fungsi baharu (`Code.js`)**

| Fungsi | Jenis | Tugas |
|--------|-------|-------|
| `sendWeeklyDigest_()` | handler trigger | Orkestra aliran di atas |
| `syncDigestTrigger_(day, hour, minute)` | side-effect | Padam trigger `sendWeeklyDigest_` lama, cipta satu baharu. Dipanggil dari `updateSystemSettings` + `installSystem` (macam `syncReminderTrigger_`) |
| `buildDigestText_(events, cfg)` | tulen | Teks biasa (tiada `<b>`, tiada markdown). **Dipanggil sekali per sink** dengan senarai aktiviti tersendiri |
| `digestDateLabel_(e)` | tulen | Label tarikh satu-hari vs julat pelbagai-hari (guna `formatDate_`) |
| `sendToTelegram_(text, token, chatIdsCsv)` | side-effect | Loop `chat_id`, POST ke `api.telegram.org/bot<token>/sendMessage`, best-effort |
| `sendToGoogleChat_(text, webhooksCsv)` | side-effect | Loop URL webhook, POST JSON `{text}`, best-effort |
| `sanitizeForGChat_(s)` | tulen | Neutralkan aksara format Chat (`* _ ~ \` < >`) — dipakai HANYA dalam sink Chat |
| `parseCsvList_(raw)` | tulen | Pisah `[\s,;]+`, trim, buang kosong. **Tidak** lowercase (chat_id / URL case-sensitive) |
| `parseShareChannels_(description)` | tulen | Hurai baris `Kongsi:` berlabuh baris → array saluran dikenali (`['tg']` / `['gchat']` / `['tg','gchat']` / `[]`); buang token tak dikenali |
| `isoWeekKey_(date, tz)` | tulen | `"2026-W37"` — stabil sempadan tahun |
| `weekDayEnum_(n)` | tulen | `0..6` → `ScriptApp.WeekDay.SUNDAY..SATURDAY`; luar julat → Ahad |
| `clampDigestDay_(v)` / `clampMinute_(v)` | tulen | Pagar nilai client; rosak → lalai |
| `pruneDigestMarkers_()` | side-effect | Kekal 8 kunci `DGSENT_` terkini, buang lama |
| `selfTestDigestHelpers_()` | ujian | Self-test fungsi tulen (jalan dari editor / node) |

**Guna semula (sedia ada, tak diubah tingkah laku):** `getConfig_`, `safeGetEvents_`,
`getPPDCalendar_`, `eventToObject_`, `formatDate_`, `addAudit_`, `requireSession_`.
Jam digest: guna semula `clampReminderHour_` **jika** ia sudah pagar 0–23 (semak masa kod);
jika hanya pagar bawah, tambah pagar atas dalam `clampMinute_`-gaya helper.

## 5. Model data

### 5.1 Baris `description` event

`buildDescription_(payload, category)` tambah **selepas** `RemindTo:`, **sebelum**
`[PPD_CATEGORY:...]`:

```
Kongsi: tg,gchat
```

- Ditulis hanya bila sekurang-kurangnya satu saluran ditanda. Nilai = senarai saluran
  yang ditanda, susunan tetap (`tg` dahulu, `gchat` kemudian), dipisah koma.
- `payload.shareTg` / `payload.shareGchat` (boolean dari borang) → `['tg','gchat']` ditapis.
- `parseDescriptionMeta_` pulang `shareChannels: parseShareChannels_(description)` (array).
  Tiada baris `Kongsi:` → `[]` → tidak dikongsi.
- `eventToObject_` dedah medan `shareChannels`.
- **`cleanDescription_` MESTI buang baris `Kongsi:`** (macam ia buang `Reminder:` /
  `RemindTo:`). Kalau tidak, penanda bocor ke kotak Keterangan bila guru buka borang edit
  → ditulis semula sebagai teks biasa → aktiviti kekal "dikongsi" walau checkbox dibuang.

### 5.2 Kunci `DEFAULT_CONFIG` baharu

| Kunci | Jenis | Lalai | Nota |
|-------|-------|-------|------|
| `BROADCAST_TG_TOKEN` | string | `''` | Token bot @BotFather. **Write-only di UI** |
| `BROADCAST_TG_CHAT_IDS` | string | `''` | `chat_id` group dipisah koma (cth `-1001234567890,-1009876543210`) |
| `BROADCAST_GCHAT_WEBHOOKS` | string | `''` | URL incoming webhook Space dipisah koma. **Write-only di UI** |
| `DIGEST_DAY` | int 0–6 | `0` (Ahad) | Hari trigger |
| `DIGEST_HOUR` | int 0–23 | `7` | Jam trigger |
| `DIGEST_MINUTE` | int 0–59 | `45` | Minit trigger (`nearMinute`) |

### 5.3 Script Properties runtime

- `DGSENT_<isoWeekKey>` — penanda "digest minggu ni sudah cuba dihantar". Nilai = `Date.now()`.
- Di-prune ke 8 terkini oleh `pruneDigestMarkers_()`.

## 6. Aliran `sendWeeklyDigest_()`

1. `cfg = getConfig_()`.
2. **Guard konfigurasi:** kalau `(!TG_TOKEN || !TG_CHAT_IDS)` **dan** `!GCHAT_WEBHOOKS`
   → `return` (tiada apa dikonfig).
3. `today` = 00:00 hari ini (TZ sistem). `rangeEnd` = `today + 7 hari`.
4. `events = safeGetEvents_(getPPDCalendar_(), today, rangeEnd).map(eventToObject_)`.
   - Cuti Google Malaysia **tidak** dalam kalendar kerja → auto-exclude.
   - Cuti Sekolah admin (`category==='cuti'`, dalam kalendar kerja) **termasuk** kalau
     ditanda salah satu saluran — pilihan admin.
5. `tgEvents = events.filter(e => e.shareChannels.indexOf('tg') !== -1)`;
   `gchatEvents = events.filter(e => e.shareChannels.indexOf('gchat') !== -1)`.
6. `if (!tgEvents.length && !gchatEvents.length) return;` — **skip senyap, tak set penanda**.
7. `weekKey = isoWeekKey_(today)`. `if (props.getProperty('DGSENT_'+weekKey)) return;`
8. **Sink Telegram** (jalan hanya kalau `TG_TOKEN && TG_CHAT_IDS && tgEvents.length`):
   `sendToTelegram_(buildDigestText_(tgEvents, cfg), cfg.BROADCAST_TG_TOKEN, cfg.BROADCAST_TG_CHAT_IDS)`.
9. **Sink Google Chat** (jalan hanya kalau `GCHAT_WEBHOOKS && gchatEvents.length`):
   `sendToGoogleChat_(buildDigestText_(gchatEvents, cfg), cfg.BROADCAST_GCHAT_WEBHOOKS)`.
10. `props.setProperty('DGSENT_'+weekKey, String(Date.now()))`. `pruneDigestMarkers_()`.
11. Seluruh badan dibalut `try/catch` → `addAudit_('DIGEST_RUN_FAILED', e.message, cfg.ADMIN_EMAIL)`.

Nota: penanda `DGSENT_` **satu per minggu untuk seluruh run** (bukan per saluran). Kalau
Telegram berjaya tapi Chat gagal, minggu itu tetap ditanda — konsisten dengan "minggu
tertinggal = tertinggal". Aktiviti yang ditanda saluran **selepas** run Ahad tunggu minggu
depan.

Penanda diset **selepas cuba** semua sink, tanpa mengira kegagalan separa — digest ialah
snapshot mingguan; cuba semula berisiko hantar dua kali ke sink yang berjaya. Minggu
tertinggal = tertinggal (boleh diterima).

## 7. Format mesej (`buildDigestText_`)

Teks biasa — diterima Telegram (tanpa `parse_mode`) **dan** Google Chat webhook:

```
📅 Aktiviti Sekolah — Minggu Ini
Sekolah Kebangsaan Salor

• Hari Sukan Sekolah
  Isnin, 8 September 2026 · Padang Sekolah

• Minggu Bahasa
  Rabu, 10 September 2026 – Jumaat, 12 September 2026 · Dewan Sekolah

• Gotong-royong Perdana
  Sabtu, 13 September 2026 · —

—
Mesej automatik daripada sistem takwim sekolah. Sila jangan balas.
```

- Baris tajuk pakai `cfg.OFFICE_NAME`.
- `digestDateLabel_(e)`: satu hari → `EEEE, d MMMM yyyy`; pelbagai hari → `mula – tamat`
  (guna `displayEndDate` logik sedia ada untuk all-day supaya tak tersasar 1 hari).
- Lokasi kosong → `—`.
- Susun ikut `start` menaik.
- **Tiada** PIC, agensi, badan description, atau senarai penerima.
- Sink Telegram hantar `text` mentah. Sink Chat hantar `sanitizeForGChat_(text)` supaya
  `*_~\`<>` dalam tajuk/lokasi tak jadi format tak sengaja.

## 8. Pengendalian ralat & keselamatan

### 8.1 Rahsia
- `BROADCAST_TG_TOKEN` + `BROADCAST_GCHAT_WEBHOOKS` — Script Properties **sahaja**,
  tidak pernah dalam kod atau git. Repo `takwim-digital` **PUBLIC**.
- Medan UI untuk kedua-duanya **write-only** (corak baharu untuk projek ni):
  `getSystemSettings` pulang bendera boolean (`broadcastTgTokenSet`, `broadcastGchatSet`)
  bukan nilai mentah; `updateSystemSettings` kekalkan nilai semasa bila input
  kosong/undefined, tulis nilai baharu hanya bila input tak kosong.

### 8.2 Sasaran
- `chat_id` / URL webhook — input **admin sahaja** (`updateSystemSettings` dah
  `requireSession_(token, 'canManageUsers')`).
- Bot Telegram hanya boleh hantar ke group yang **bot itu sendiri ahli**. Webhook hanya
  post ke **Space pemiliknya**. Tiada risiko hantar ke sasaran sewenang-wenang.

### 8.3 Best-effort per sasaran
- `UrlFetchApp.fetch(url, { muteHttpExceptions: true, ... })` — 4xx/5xx tak `throw`.
- Semak `getResponseCode()` ∈ [200,299]. Telegram: hurai JSON `{ok:false, description}`
  untuk sebab sebenar.
- Telegram `migrate_to_chat_id` dalam `parameters` (group → supergroup) → audit
  `DIGEST_CHATID_MIGRATED` dengan ID baharu supaya master boleh kemas tetapan.
- Gagal satu sasaran → `addAudit_('DIGEST_SEND_FAILED', sink+' | '+id+' | '+sebab,
  cfg.ADMIN_EMAIL)` + **teruskan** sasaran lain. Corak sama `REMINDER_EMAIL_FAILED`.

### 8.4 Kandungan
- Hanya tajuk / tarikh / lokasi. `buildDigestText_` tidak sesekali sertakan medan dalaman.

### 8.5 Skop OAuth (`UrlFetchApp`)
- `UrlFetchApp` perlu `https://www.googleapis.com/auth/script.external_request`.
- `appsscript.json` = `executeAs: USER_DEPLOYING` → kod jalan sebagai **pengguna
  deploy (master)**. Hanya **master** re-authorize, **sekali**. Guru **tidak** terjejas.
- Trigger `sendWeeklyDigest_` fire sebagai pemilik trigger (master) — kalau master belum
  grant skop, eksekusi trigger gagal senyap. **Prasyarat:** master jalankan
  `sendWeeklyDigest_` sekali dari Apps Script Editor selepas deploy → skrin kebenaran →
  Allow. (Bentuk sama `installReminderTrigger_`.)

### 8.6 Had kadar
- Telegram: ~20 mesej/min per group, ~30/saat global. Beberapa group seminggu sekali —
  jauh di bawah had.
- Google Chat webhook: ~1 mesej/saat per space. Cukup.

## 9. Prasyarat manual master (sekali sahaja — macam trigger reminder)

1. **@BotFather** → `/newbot` → salin token. Tambah bot ke setiap group Telegram parent.
2. Dapatkan `chat_id` setiap group: hantar mesej dalam group, buka
   `https://api.telegram.org/bot<TOKEN>/getUpdates`, salin `result[].message.chat.id`
   (nombor **negatif**).
   ⚠️ Kalau privacy mode bot ON (lalai), `getUpdates` mungkin tak tunjuk mesej biasa —
   hantar `/start@namabot` dalam group, atau guna `@RawDataBot` sementara.
3. Google Chat Space "info rasmi murid sk salor" → **Manage webhooks** → tambah → salin URL.
   (Master sahkan 2026-09-06: destinasi ini memang **Space**, bukan group chat biasa.)
4. System Settings → isi token + `chat_id` (koma) + URL webhook + hari/jam/minit → **Simpan**.
   Trigger tercipta automatik (`syncDigestTrigger_` dipanggil oleh `updateSystemSettings`).
5. Apps Script Editor → **Run `sendWeeklyDigest_`** sekali → **Allow** kebenaran baharu
   (permintaan luar).
6. Tanda satu aktiviti akan datang — **Kongsi ke Telegram** + **Kongsi ke Google Chat**
   → Run `sendWeeklyDigest_` lagi → sahkan mesej masuk group Telegram **dan** Space Chat.
   Ulang dengan satu saluran sahaja ditanda → sahkan mesej pergi ke saluran itu **sahaja**.

## 10. Ujian

Projek tiada framework — ikut corak `selfTestReminderHelpers_()` (fungsi tulen, `node`
lokal, mutation test setiap logik kritikal).

`selfTestDigestHelpers_()`:

| Semakan | Kes |
|---------|-----|
| `parseCsvList_` | `'-100a, -100b ; '` → `['-100a','-100b']`; **tidak** lowercase |
| `parseShareChannels_` | `Kongsi: tg` → `['tg']`; `Kongsi: tg,gchat` → `['tg','gchat']`; `Kongsi: gchat` → `['gchat']`; tiada → `[]`; `Kongsi: xyz` → `[]` (buang tak dikenali); `Kongsi` dalam badan teks → `[]` (berlabuh baris) |
| `buildDescription_` round-trip | Baris `Kongsi` = senarai saluran yang ditanda (susunan `tg,gchat`); tiada checkbox ditanda → tiada baris; `PIC`/`Agensi`/`Reminder`/`RemindTo` sedia ada tak terjejas |
| `cleanDescription_` | Baris `Kongsi:` dibuang (macam `Reminder:` / `RemindTo:`) — tak bocor ke kotak Keterangan |
| `buildDigestText_` | Snapshot; satu-hari vs julat; lokasi kosong → `—`; **tiada** kebocoran PIC/agensi |
| `digestDateLabel_` | All-day pelbagai hari tak tersasar +1 hari |
| `sanitizeForGChat_` | `* _ ~ \` < >` dineutralkan |
| `isoWeekKey_` | 1 Jan, 31 Dis, tahun ada W53 — kunci stabil & unik |
| `weekDayEnum_` | `0→SUNDAY` … `6→SATURDAY`; `7`/`-1` → SUNDAY |
| `clampDigestDay_` / `clampMinute_` | Sempadan + sampah → lalai |
| `pruneDigestMarkers_` | 10 kunci → 8 terbaharu tinggal |

**Mutation test (WAJIB gagal):**
- Buang guard token dalam `sendToTelegram_` → ujian "tiada token → tiada fetch" mesti gagal
- `&&` → `||` dalam guard konfigurasi langkah 2
- Penapis Telegram `indexOf('tg') !== -1` → `filter(() => true)` → ujian "aktiviti tak-tg tak masuk digest Telegram" mesti gagal
- Silang saluran: `'tg'` → `'gchat'` dalam penapis sink Telegram → ujian "aktiviti gchat-sahaja tak pergi Telegram" mesti gagal
- Buang pembuangan baris `Kongsi:` dari `cleanDescription_` → ujian "penanda tak bocor ke Keterangan" mesti gagal
- Set penanda `DGSENT_` **sebelum** hantar (bukan selepas) → ujian urutan mesti gagal

`node --check Code.js` + `grep -c $'\r' Code.js Index.html` = 0 (tiada CRLF flip) setiap commit.

**Verify hujung-ke-hujung:** langkah 5–6 Seksyen 9 (bot + Space ujian master).

## 11. Fail disentuh

| Fail | Perubahan |
|------|-----------|
| `Code.js` | +~180 baris. `DEFAULT_CONFIG` (+6 kunci); `validateSetupInput_` (+validasi 6 medan — **ingat ia PENAPIS memusnah**: kunci tertinggal → hilang setiap Simpan); `getSystemSettings` (+bendera write-only); `updateSystemSettings` + `installSystem` (+merge medan, +panggil `syncDigestTrigger_`); `buildDescription_` + `parseDescriptionMeta_` + `eventToObject_` + **`cleanDescription_`** (+`Kongsi`/`shareChannels`); 15 fungsi baharu (Seksyen 4) |
| `Index.html` | **Dua checkbox** "Kongsi ke Telegram" + "Kongsi ke Google Chat" dalam borang aktiviti (round-trip cipta + edit; **reset eksplisit** bila borang dikosongkan — `resetEventForm` hanya kosongkan `.value`, tak sentuh checkbox); badge 📣 dalam modal detail (TG / Chat / TG+Chat); 6 medan dalam skrin System Settings (token gaya-password + checkbox `clearTgToken`, chat_ids, webhook gaya-password + checkbox `clearGchatWebhooks`, dropdown hari, jam, minit) |
| `selftest-node.js` **(BARU — perlu OK master)** | Harness `node` di root repo: muat `Code.js` dengan global GAS palsu, jalankan `selfTest*Helpers_`. Disekat dari `clasp push` oleh `.claspignore`. Tanpa ini mutation test kena buat manual dalam editor GAS |
| `SETUP.md` / `SETUP.html` | Dokumen 6 tetapan + 3 langkah setup bot/webhook + nota skop kebenaran |
| `docs/PANDUAN-GURU.md` / `docs/index.html` | Versi ringkas untuk cikgu |

`.claspignore` sedia ada hadkan `clasp push` ke 3 fail app — `selftest-node.js` + spec ini
tak masuk projek Apps Script.

## 12. Risiko & andaian

| Perkara | Status |
|---------|--------|
| `nearMinute` = tetingkap hantar **±15 minit** dari jam:minit yang diset | Andaian: boleh diterima untuk digest (master sahkan bila semak spec) |
| Privacy mode bot sembunyikan mesej dari `getUpdates` | Dinota dalam langkah setup (guna `/start@bot` atau `@RawDataBot`) |
| Group → supergroup tukar `chat_id` | Ditangani: audit `DIGEST_CHATID_MIGRATED` bawa ID baharu |
| Ahli Space murid berubah hujung tahun | Tiada kesan — webhook tetap post, tak mudarat |
| Repo PUBLIC — spec ini akan public di repo memory (juga PUBLIC) | Tiada rahsia dalam spec — OK |

---

## Aliran seterusnya

1. ✅ Spec diluluskan master 2026-09-06. Rev.1 (dua checkbox saluran) diluluskan malam sama.
2. ✅ **Keputusan terbuka DISELESAIKAN 2026-09-06 malam:** (a) fail baharu `selftest-node.js`
   **DILULUSKAN**; (b) tetapan digest **System Settings sahaja** (bukan Setup Wizard) — sah.
3. 🔄 Plan pelaksanaan (`PLAN-telegram-gchat-digest.md`, local, ~14 task) direvise untuk Rev.1.
4. Kod ikut pipeline Kata (sederhana-besar): kod → `sight-hone` → `cross-ai-julius` →
   `commit-seal` → push → `clasp push -f` ke `@HEAD` → smoke master → **deploy production
   atas arahan jelas master sahaja**.
