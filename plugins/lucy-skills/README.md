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
| `library` | "save library", "do we have", "install item" | Pengetahuan teknikal merentas projek |
| `save-diary` | "save diary", "write diary" | Dokumentasi sesi harian |
| `auto-commit` | "commit", "git commit", post-task | Structured git commits |
| `work-plan` | "copy plan", "resume plan", "append plan" | Plan execution + recovery |
| `session-briefing` | Auto session start, "brief" | Recap sesi lepas + projek aktif |
| `check-reminders` | Auto session start, "ingatkan aku" | Persistent reminder cross-session |
| `decision-log` | Auto (keputusan bukan-obvious), "log keputusan" | Log keputusan arkitek/teknikal |
| `echo-recall` | "ingat tak", "pernah tak kita", "bila kita buat" | Cari diary sebelum jawab soalan masa lepas |
| `mulahazah` | "/continuous-improvement", "apa yang Lucy pelajari" | Behavioral learning dari pattern master |
| `forge` | Auto (pattern 3+ kali), "create skill", "level up" | Self-improvement + skill creation |
| `post-mortem` | Auto (kegagalan), "post-mortem" | Log kesilapan dan pembelajaran |
| `refine` | Auto sebelum commit (task kecil) | Review kod yang berubah |
| `safi` | "safi", "balance check" | Clean enough? Necessary? |
| `sight-eagle` | "eagle", "quick scan" | Scan kod cepat |
| `sight-hone` | "hone", "review feature" | Review feature dalam konteks |
| `sight-elemental` | "elemental", "deep scan" | Deep scan projek baru |
| `sight-omnipotent` | "omnipotent", "full audit" | Audit penuh (sparingly) |
| `sight-hunt` | "hunt", "latent bugs" | Hunt bugs tersembunyi |
| `sight-aksara` | "aksara", "verify canonical" | Verify kod match plan/docs |
| `convergence` | "convergence", "ready deploy?" | Synthesize review results |
| `cross-ai-julius` | "julius", "gemini review" | Cross-AI second opinion |
| `surai` | Always active | Pressure sensor: fatigue + pipeline stress |
| `axo` | "Axo" | Axo companion mode |
| `commit-seal` | Sebelum push | Final seal sebelum push |
| `work-plan` | "copy plan", "resume plan" | Plan lifecycle management |
| `save-diary` | "save diary" | Session diary documentation |

## Built-in (dalam identity-core.md)

| Sistem | Trigger | Fungsi |
|--------|---------|--------|
| Time Intelligence | Auto setiap sesi | Greeting & behavior ikut masa (pagi/petang/malam) |
| LRU Projects | "load project", "new coding project" | Manage projek aktif |

---

**Version**: 2.0.0
**Updated**: 2026-05-03
**Skills**: 26 active + 2 built-in
