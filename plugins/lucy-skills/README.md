# 🔌 Lucy Skills Plugin
*Sistem skill auto-trigger untuk AI companion Lucy*

## Tentang Plugin Ini

Plugin ini membolehkan Lucy mempelajari kemahiran baru melalui fail `.md` mudah.
Setiap skill auto-aktif berdasarkan konteks perbualan — tiada trigger manual diperlukan.

## Struktur

```
lucy-skills/
├── .claude-plugin/
│   └── plugin.json          # Plugin identity
├── skills/
│   └── save-memory/
│       └── SKILL.md         # Skill pertama (simpan memory)
├── skill-format.md          # Template rujukan untuk skill baru
└── README.md                # Dokumentasi ini
```

## Cara Tambah Skill Baru

1. Cipta folder: `skills/[nama-skill]/`
2. Cipta `SKILL.md` dengan format dari `skill-format.md`
3. Skill terus aktif — tiada config diperlukan

## Skills Aktif

| Skill | Trigger | Fungsi |
|-------|---------|--------|
| `save-memory` | "save", "Lucy simpan" | Simpan maklumat ke fail memory |
| `library` | "save library", "do we have" | Pengetahuan teknikal merentas projek |
| `save-diary` | "save diary", "write diary" | Dokumentasi sesi harian |
| `auto-commit` | "commit", "git commit", post-task | Structured git commits |
| `work-plan` | "copy plan", "resume plan", "append plan" | Plan execution + recovery |

## Built-in (dalam main-memory.md)

| Sistem | Trigger | Fungsi |
|--------|---------|--------|
| Echo Memory Recall | "do you remember", "ingat tak" | Cari balik perbualan lama dalam diary |
| LRU Projects | "load project", "new coding project" | Manage 4 projek aktif |
| Time Intelligence | Auto (setiap sesi) | Greeting & behavior mengikut masa |

---

**Version**: 1.2.0
**Installed**: 2026-03-26
**Skills**: 5 active + 3 built-in
