# Wiom Design System

The official Wiom Design System for the Wiom Partner & Customer Android apps — distributed as **Claude Code skills** so any designer or developer with Claude Code can apply Wiom design rules consistently when generating, reviewing, or refactoring UI.

This repo contains:

- **`skills/`** — 18 installable Claude Code skills (foundations + 16 components + compose layout rules)

> Human-readable reference documents (`docs/`) were removed in the V2 refresh because they had drifted from the skills. They will be regenerated in a follow-up pass.

---

## What's included

### Foundations and cross-cutting rules

| Skill | Covers |
|---|---|
| `wiom-design-foundations` | Color, typography, iconography, spacing, radius, stroke, shadow tokens + cross-foundation rules and component quick-build reference |
| `wiom-compose-layout-rules` | Structural + anti-hallucination rules for Jetpack Compose UI against Wiom foundations (width / height / child sizing / icons / variants / pre-commit checklist) |

### Components (`skills/components/wiom-*`)

| Skill | Component |
|---|---|
| `wiom-cta` | Buttons / CTAs — 6 types (Primary · Secondary · Tertiary · Destructive · Acknowledge · Pre-booking) |
| `wiom-top-bar` | Top Bar — 3 sizes × 4 states |
| `wiom-navigation-bar` | Bottom Navigation Bar — 2–5 tabs |
| `wiom-tabs-filters` | Pill Tabs, Underline Filters, Chips |
| `wiom-input-fields` | Input, Textarea, OTP, Search, Dropdown field (trigger for `wiom-bottomsheet` pickers) |
| `wiom-selection-controls` | Checkbox (with Indeterminate), Radio, Switch (with Pressed) — indicator-only; labels live on `wiom-list-item` |
| `wiom-list-item` | List Item — 5 types (Default · `icon with bg` · Checkbox · Radio · Switch) |
| `wiom-badge` | Dot, Count, and Label badges |
| `wiom-icon-badge` | Icon-inside-container affordance — 3 sizes × 6 tones |
| `wiom-bottomsheet` | Bottom Sheet — 8 size variants + picker / action-menu patterns (replaces the deprecated dropdown menu UX) |
| `wiom-dialog` | Dialog / Modal — 5 types (Alert · Input · Selection · Illustration · Loading) |
| `wiom-toast` | Toast / Snackbar — 4 types × 3 behaviors |
| `wiom-loader` | Loader — Spinner, Status (Wait → Success / Error), Dots, Skeleton, Brand full-screen |
| `wiom-progress-indicator` | Determinate progress — Linear, Completion meter, Milestones, Circular |
| `wiom-stepper` | Stepper — Horizontal and Vertical, shared StepIndicator atom |
| `wiom-pagination` | Pagination — Dots-Simple / Dots-Expanded / Bars, Counter, Scroll Indicator |

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

Copy every skill into your user-level Claude skills folder so they work in any project. The repo layout is:

```
skills/
├── wiom-design-foundations/
├── wiom-compose-layout-rules/
└── components/
    ├── wiom-cta/
    ├── wiom-top-bar/
    └── …
```

Claude Code's skills folder is flat, so flatten as you copy:

**macOS / Linux:**
```bash
cp -r skills/wiom-design-foundations skills/wiom-compose-layout-rules ~/.claude/skills/
cp -r skills/components/* ~/.claude/skills/
```

**Windows (Git Bash):**
```bash
cp -r skills/wiom-design-foundations skills/wiom-compose-layout-rules "$HOME/.claude/skills/"
cp -r skills/components/* "$HOME/.claude/skills/"
```

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse skills\wiom-design-foundations, skills\wiom-compose-layout-rules $HOME\.claude\skills\
Copy-Item -Recurse skills\components\* $HOME\.claude\skills\
```

### Install one skill only

```bash
cp -r skills/components/wiom-cta ~/.claude/skills/
```

### Verify

Open Claude Code and run `/help` — your installed skills should be listed. Each skill auto-triggers based on what you ask Claude to do (e.g. asking for a button → `wiom-cta` activates).

---

## Update

To get the latest version:

```bash
cd wiom-design-system
git pull
cp -r skills/wiom-design-foundations skills/wiom-compose-layout-rules ~/.claude/skills/
cp -r skills/components/* ~/.claude/skills/
```

---

## Structure

```
wiom-design-system/
├── README.md
├── skills/
│   ├── wiom-design-foundations/
│   │   ├── SKILL.md
│   │   └── references/              # Detailed token tables
│   ├── wiom-compose-layout-rules/
│   │   └── SKILL.md
│   └── components/
│       ├── wiom-cta/SKILL.md
│       ├── wiom-top-bar/SKILL.md
│       ├── wiom-navigation-bar/SKILL.md
│       ├── wiom-tabs-filters/SKILL.md
│       ├── wiom-input-fields/SKILL.md
│       ├── wiom-selection-controls/SKILL.md
│       ├── wiom-list-item/SKILL.md
│       ├── wiom-badge/SKILL.md
│       ├── wiom-icon-badge/SKILL.md
│       ├── wiom-bottomsheet/SKILL.md
│       ├── wiom-dialog/SKILL.md
│       ├── wiom-toast/SKILL.md
│       ├── wiom-loader/SKILL.md
│       ├── wiom-progress-indicator/SKILL.md
│       ├── wiom-stepper/SKILL.md
│       └── wiom-pagination/SKILL.md
└── …
```

---

## What changed in V2 (2026-04)

The repo was refreshed to match Figma V2 and the updated foundations skill:

- New unified `wiom-selection-controls` merges the old `wiom-checkbox`, `wiom-radio`, `wiom-switch`. Labels + helper text move to `wiom-list-item`; the control ships indicator-only. Checkbox gains `Indeterminate`; Switch gains `Pressed`; Hover / Focused removed as visual variants (keyboard focus is ring-only now).
- New `wiom-icon-badge` covers the `icon-with-bg` affordance — glyph inside a filled circular container, 3 sizes × 6 tones.
- New `wiom-dialog` covers modal centered surfaces — Alert / Input / Selection / Illustration / Loading.
- `wiom-dropdown` is **deleted**. The dropdown trigger is now an `wiom-input-fields` Input with `State=Read-only` + chevron. The dropdown menu UX is a `wiom-bottomsheet` picker (single-select / multi-select / action menu / search-header patterns). There is no popover / anchored menu in Wiom mobile.
- `wiom-cta` gains `Pre-booking` as a 6th type; `Focus` is removed as a visual state (now surfaces via `stroke.brand.focus` / `stroke.critical.focus` ring).
- `wiom-bottomsheet` gains `Share`, `Illustration Left`, and `Form` size variants (8 total).
- `wiom-list-item` gains `icon with bg` as a 5th Type variant that hosts a `wiom-icon-badge` in the leading slot.
- Every skill migrated to element-first color tokens (`bg.*` / `text.*` / `stroke.*` / `icon.*`). Legacy `color.brand.primary` / `color.warning.primary` (#B85C00 brown) / slash-namespaced tokens removed across the board.
- Tappable chrome icons (top bar, navigation bar, pagination chevrons) now use `icon.action` per foundations rule, not `text.subtle`.
- Warning-family text on warning-tinted surfaces uses `text.onWarning` (#372902 olive), never `text.default`.

---

## How the skills work

Each skill is a Markdown file with YAML frontmatter. Claude Code reads the `description` field and auto-activates the skill when it judges the current task matches. You don't need to invoke them manually — just ask Claude to "build a settings screen" or "review this checkout flow", and the relevant Wiom skills will load automatically.

Skills are scoped to **Wiom Android (Kotlin / Jetpack Compose)**. Token names map 1:1 to `WiomTokens.kt`.

---

## Contributing

This is the canonical source for the Wiom Design System. To propose a change:

1. Open a PR with the updated `SKILL.md`
2. Tag the design lead for review
3. Once merged, all consumers can `git pull && cp -r skills/... ~/.claude/skills/` to update

---

## Contact

For questions, contact the Wiom Design team.
