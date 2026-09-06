# Spec — Digest Aktiviti Mingguan ke Telegram + Google Chat

**Projek:** `takwim-digital` (Google Apps Script + Google Calendar)
**Tarikh:** 2026-09-06
**Status:** Draf reka bentuk — tunggu semakan master sebelum `writing-plans`
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
- Penanda per-aktiviti "Kongsi umum" (checkbox borang aktiviti)
- Enjin digest mingguan berjadual + trigger boleh-set (hari/jam/minit)
- Penghantar Telegram (Bot API) + penghantar Google Chat (incoming webhook)
- Tetapan baharu di System Settings + Setup Wizard
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
| 3 | Penanda **satu checkbox** "Kongsi umum" (parent + murid), lalai **OFF** | Master tolak dua-toggle berasingan — YAGNI |
| 4 | Simpan penanda sebagai baris `Kongsi: 1` dalam `description` event | Corak sama `Reminder:` / `RemindTo:` / `PIC` / `Agensi` — Calendar tiada custom field |
| 5 | **Digest mingguan sahaja**, tetingkap **7 hari akan datang** | Master pilih A (bukan +H-1, bukan preview minggu depan) |
| 6 | Lalai **Ahad 07:45**; hari + jam + minit **boleh admin set** di UI | Corak sama `REMINDER_HOUR` |
| 7 | Kandungan: **tajuk + hari/tarikh + lokasi** sahaja | Tiada PIC/agensi/nota dalaman ke sasaran luar |
| 8 | Minggu tiada aktiviti bertanda "kongsi" → **skip senyap** | Group tak kena spam mesej kosong |
| 9 | **Google Chat disambung dalam v1** (bukan follow-up) | Space "semua murid" memang wujud — audiens sebenar, bukan spekulasi |
| 10 | Enjin **sink-agnostik**: `buildDigestText_()` pulang teks biasa sekali → fan-out ke 2 penghantar nipis | Tambah/buang sink kemudian = kos kecil |

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
        ├─ safeGetEvents_(getPPDCalendar_(), today, today+7h)
        │        .map(eventToObject_)
        │        .filter(e => e.shareBroadcast)      ← penanda "Kongsi: 1"
        ├─ (0 event) → return  (tak set penanda)
        ├─ DGSENT_<isoWeek> wujud? → return
        ├─ text = buildDigestText_(events, cfg)      ← teks biasa, tiada markup
        ├─ sink Telegram:  sendToTelegram_(text, token, chatIdsCsv)
        ├─ sink GChat:     sendToGoogleChat_(text, webhooksCsv)
        ├─ setProperty DGSENT_<isoWeek>
        └─ pruneDigestMarkers_()   ← simpan 8 minggu terakhir
```

**Fungsi baharu (`Code.js`)**

| Fungsi | Jenis | Tugas |
|--------|-------|-------|
| `sendWeeklyDigest_()` | handler trigger | Orkestra aliran di atas |
| `syncDigestTrigger_(day, hour, minute)` | side-effect | Padam trigger `sendWeeklyDigest_` lama, cipta satu baharu. Dipanggil dari `updateSystemSettings` + `installSystem` (macam `syncReminderTrigger_`) |
| `buildDigestText_(events, cfg)` | tulen | Teks biasa (tiada `<b>`, tiada markdown) — dikongsi kedua-dua sink |
| `digestDateLabel_(e)` | tulen | Label tarikh satu-hari vs julat pelbagai-hari (guna `formatDate_`) |
| `sendToTelegram_(text, token, chatIdsCsv)` | side-effect | Loop `chat_id`, POST ke `api.telegram.org/bot<token>/sendMessage`, best-effort |
| `sendToGoogleChat_(text, webhooksCsv)` | side-effect | Loop URL webhook, POST JSON `{text}`, best-effort |
| `sanitizeForGChat_(s)` | tulen | Neutralkan aksara format Chat (`* _ ~ \` < >`) — dipakai HANYA dalam sink Chat |
| `parseCsvList_(raw)` | tulen | Pisah `[\s,;]+`, trim, buang kosong. **Tidak** lowercase (chat_id / URL case-sensitive) |
| `isShareBroadcast_(description)` | tulen | Padan `^Kongsi:\s*1` berlabuh baris |
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
Kongsi: 1
```

Hanya ditulis bila `payload.shareBroadcast === true`. `parseDescriptionMeta_` pulang
`shareBroadcast: /(?:^|\n)Kongsi:\s*1\b/i.test(description)`. `eventToObject_` dedah
medan `shareBroadcast`.

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
4. `events = safeGetEvents_(getPPDCalendar_(), today, rangeEnd).map(eventToObject_)
   .filter(e => e.shareBroadcast)`.
   - Cuti Google Malaysia **tidak** dalam kalendar kerja → auto-exclude.
   - Cuti Sekolah admin (`category==='cuti'`, dalam kalendar kerja) **termasuk** kalau
     ditanda "Kongsi" — pilihan admin.
5. `if (!events.length) return;` — **skip senyap, tak set penanda**.
6. `weekKey = isoWeekKey_(today, cfg.TIMEZONE)`. `if (props.getProperty('DGSENT_'+weekKey)) return;`
7. `text = buildDigestText_(events, cfg)`.
8. **Sink Telegram** (jalan hanya kalau `TG_TOKEN && TG_CHAT_IDS`):
   `sendToTelegram_(text, cfg.BROADCAST_TG_TOKEN, cfg.BROADCAST_TG_CHAT_IDS)`.
9. **Sink Google Chat** (jalan hanya kalau `GCHAT_WEBHOOKS`):
   `sendToGoogleChat_(text, cfg.BROADCAST_GCHAT_WEBHOOKS)`.
10. `props.setProperty('DGSENT_'+weekKey, String(Date.now()))`. `pruneDigestMarkers_()`.
11. Seluruh badan dibalut `try/catch` → `addAudit_('DIGEST_RUN_FAILED', e.message, cfg.ADMIN_EMAIL)`.

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
6. Tanda satu aktiviti akan datang sebagai **"Kongsi umum"** → Run `sendWeeklyDigest_` lagi
   → sahkan mesej masuk group Telegram **dan** Space Chat.

## 10. Ujian

Projek tiada framework — ikut corak `selfTestReminderHelpers_()` (fungsi tulen, `node`
lokal, mutation test setiap logik kritikal).

`selfTestDigestHelpers_()`:

| Semakan | Kes |
|---------|-----|
| `parseCsvList_` | `'-100a, -100b ; '` → `['-100a','-100b']`; **tidak** lowercase |
| `isShareBroadcast_` | `Kongsi: 1` → true; `Kongsi: 0` / tiada → false; `Kongsi` dalam badan teks → false (berlabuh baris) |
| `buildDescription_` round-trip | Baris `Kongsi` ada ⟺ flag set; `PIC`/`Agensi`/`Reminder`/`RemindTo` sedia ada tak terjejas |
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
- `filter(e => e.shareBroadcast)` → `filter(() => true)` → ujian "aktiviti tak bertanda tak bocor" mesti gagal
- Set penanda `DGSENT_` **sebelum** hantar (bukan selepas) → ujian urutan mesti gagal

`node --check Code.js` + `grep -c $'\r' Code.js Index.html` = 0 (tiada CRLF flip) setiap commit.

**Verify hujung-ke-hujung:** langkah 5–6 Seksyen 9 (bot + Space ujian master).

## 11. Fail disentuh

| Fail | Perubahan |
|------|-----------|
| `Code.js` | +~170 baris. `DEFAULT_CONFIG` (+6 kunci); `validateSetupInput_` (+validasi 6 medan); `getSystemSettings` (+bendera write-only); `updateSystemSettings` + `installSystem` (+merge medan, +panggil `syncDigestTrigger_`); `buildDescription_` + `parseDescriptionMeta_` + `eventToObject_` (+`Kongsi`/`shareBroadcast`); 15 fungsi baharu (Seksyen 4) |
| `Index.html` | Checkbox "Kongsi umum (parent + murid)" dalam borang aktiviti (round-trip cipta + edit); 6 medan dalam skrin System Settings (token gaya-password, chat_ids, webhook gaya-password, dropdown hari, jam, minit) |
| `SETUP.md` / `SETUP.html` | Dokumen 6 tetapan + 3 langkah setup bot/webhook + nota skop kebenaran |
| `docs/PANDUAN-GURU.md` / `docs/index.html` | Versi ringkas untuk cikgu |

Tiada fail baharu. `.claspignore` sedia ada hadkan `clasp push` ke 3 fail app — spec ini
(dalam repo memory berasingan) tak terlibat.

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

1. Master semak spec ni (GitHub render, mobile OK).
2. Master approve / minta ubah.
3. `writing-plans` → plan pelaksanaan bertahap (kekal local).
4. Kod ikut pipeline Kata (sederhana-besar): `plan` → kod → `sight-hone` →
   `cross-ai-julius` → `commit-seal` → push → deploy `clasp`.
