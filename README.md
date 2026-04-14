# Wiom Design System

The official Wiom Design System for the Wiom Partner & Customer Android apps — distributed as **Claude Code skills** so any designer or developer with Claude Code can apply Wiom design rules consistently when generating, reviewing, or refactoring UI.

This repo contains:

- **`skills/`** — 10 installable Claude Code skills (foundations + components)
- **`docs/`** — PDF and HTML reference documents (component anatomies, foundation usage guides) for human reading

---

## What's included

### Foundations

| Skill | Covers |
|---|---|
| `wiom-design-foundations` | Color, typography, iconography, spacing, radius, stroke, shadow tokens for Jetpack Compose |
| `wiom-visual-skill` | Visual hierarchy, screen states, accessibility, form patterns (Universal Principles of Design) |

### Components

| Skill | Component |
|---|---|
| `wiom-cta` | Buttons / CTAs (5 types × 5 states) |
| `wiom-top-bar` | TopBar (3 sizes × 4 states) |
| `wiom-input-fields` | Input, Textarea, Search bar |
| `wiom-checkbox` | Checkbox (10 variants) |
| `wiom-radio` | Radio button |
| `wiom-list-item` | List Item (4 type variants) |
| `wiom-tabs-filters` | Tabs, Filter chips, Segmented controls |
| `wiom-badge` | Dot, Count, and Label badges |

---

## Install

You need [Claude Code](https://docs.claude.com/en/docs/claude-code) installed.

### One-time setup

Clone this repo somewhere:

```bash
git clone https://github.com/abhishekgarg-wiom/wiom-design-system.git
cd wiom-design-system
```

### Install all skills (recommended)

Copy every skill into your user-level Claude skills folder so they work in any project:

**macOS / Linux:**
```bash
cp -r skills/* ~/.claude/skills/
```

**Windows (Git Bash):**
```bash
cp -r skills/* "$HOME/.claude/skills/"
```

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse skills\* $HOME\.claude\skills\
```

### Install one skill only

```bash
cp -r skills/wiom-cta ~/.claude/skills/
```

### Verify

Open Claude Code and run `/help` — your installed skills should be listed. Each skill auto-triggers based on what you ask Claude to do (e.g. asking for a button → `wiom-cta` activates).

---

## Update

To get the latest version:

```bash
cd wiom-design-system
git pull
cp -r skills/* ~/.claude/skills/
```

---

## Structure

```
wiom-design-system/
├── README.md
├── skills/                          # 10 Claude Code skills
│   ├── wiom-design-foundations/
│   │   ├── SKILL.md
│   │   └── references/              # Detailed token tables
│   ├── wiom-visual-skill/SKILL.md
│   ├── wiom-cta/SKILL.md
│   ├── wiom-top-bar/SKILL.md
│   ├── wiom-input-fields/SKILL.md
│   ├── wiom-checkbox/SKILL.md
│   ├── wiom-radio/SKILL.md
│   ├── wiom-list-item/SKILL.md
│   ├── wiom-tabs-filters/SKILL.md
│   └── wiom-badge/SKILL.md
└── docs/                            # Human-readable references
    ├── foundations/                 # color, typography, iconography (HTML), radius, shadow, spacing, stroke (PDF)
    └── components/                  # Anatomy + documentation PDFs for each component
```

---

## How the skills work

Each skill is a Markdown file with YAML frontmatter. Claude Code reads the `description` field and auto-activates the skill when it judges the current task matches. You don't need to invoke them manually — just ask Claude to "build a settings screen" or "review this checkout flow", and the relevant Wiom skills will load automatically.

Skills are scoped to **Wiom Android (Kotlin/Compose)**. Token names map 1:1 to `WiomTokens.kt`.

---

## Contributing

This is the canonical source for the Wiom Design System. To propose a change:

1. Open a PR with the updated `SKILL.md`
2. Tag the design lead for review
3. Once merged, all consumers can `git pull && cp -r skills/* ~/.claude/skills/` to update

---

## Contact

For questions, contact the Wiom Design team.
