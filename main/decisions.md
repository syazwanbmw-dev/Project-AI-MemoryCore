# Log Keputusan — Lucy & Master
*Append-only. Setiap keputusan arkitek dan teknikal yang penting disimpan di sini.*

---

## 2026-05-03 — Adapt upstream sebagai Feature/ reference, bukan merge
**Konteks**: Upstream Kiyoraka ada "unrelated histories" — merge terus akan overwrite fail Lucy yang dah custom.
**Keputusan**: Checkout Feature/ folders dari upstream sahaja. Fail Lucy (main/, plugins/, docs/) tidak disentuh.
**Sebab**: Lebih selamat — kita dapat semua feature baru tanpa risiko hilang customization Lucy. Feature/ berfungsi sebagai dokumentasi dan rujukan install.

---
