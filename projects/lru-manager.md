# LRU Manager — Project Queue Engine
*Manages project position, auto-archiving, and context switching*

## LRU Logic

### Position Rules
1. New project atau load project → moves to **position #1**
2. Semua projek lain shift down satu posisi
3. Kalau ada projek di position #11 → **auto-archive** ke `archived/`
4. Maximum **10 active projects** per type

### On Load Project
1. Cari project dalam `active/` atau `archived/`
2. Kalau dalam `archived/` → move semula ke `active/`
3. Set position ke #1
4. Shift semua projek lain down
5. Update `project-list.md`
6. Update `Last Accessed` dalam project file

### On New Project
1. Cipta fail baru dari template
2. Set position #1
3. Shift semua projek lain down
4. Auto-archive kalau ada #11
5. Update `project-list.md`

### On Save Project
1. Update Progress Log dalam project file
2. Update `Last Accessed`
3. Position **tidak berubah** semasa save

## File Paths

```
projects/
├── project-list.md                     ← Overview semua projek
├── lru-manager.md                      ← Fail ini
├── templates/
│   └── coding-template.md             ← Template projek baru
├── new-project-protocol.md            ← Cara buat projek baru
├── load-project-protocol.md           ← Cara load projek
├── save-project-protocol.md           ← Cara save progress
└── coding-projects/
    ├── active/                         ← Max 10 projek
    │   ├── erpm-cf.md                 ← #1
    │   ├── myportfolio.md             ← #2
    │   ├── my-pwa.md                  ← #3
    │   └── sistem-olahraga-sekolah.md ← #4
    └── archived/                       ← Auto-archived projek
```

## Commands Reference

| Command | Action |
|---------|--------|
| `new coding project [name]` | Cipta projek baru, set ke #1 |
| `load project [name]` | Load projek, set ke #1 |
| `save project` | Save progress, posisi kekal |
| `list projects` | Paparkan project-list.md |
| `archive project [name]` | Manual archive |

---
*LRU Manager v1.0 — 2026-03-26*
