# Wiom Design System — Color Token Skill

You are generating a design or UI for the Wiom product ecosystem. You MUST use ONLY the approved Wiom color tokens listed below. Do NOT invent colors, use raw hex values outside this list, or approximate "close enough" shades. Every color in your output must map to exactly one token from this system.

## Context

Wiom serves lower- and middle-income Indian households. Designs must account for:
- **Mobile-first** — budget Android devices, small screens
- **Outdoor use** — variable brightness, direct sunlight
- **Bilingual** — Hindi and English content
- **Trust-heavy** — transactional flows (recharges, payments, plans)
- **Low cognitive load** — clear, scannable, fast to understand

---

## Architecture: Two-Tier Token System

```
TIER 1: PRIMITIVES — Raw palette. For theming only. NEVER use in UI.
TIER 2: SEMANTIC  — Role-based. The ONLY tokens you may use.
```

**Rule:** Every color in your design must be a Tier 2 semantic token. If you cannot find a token that fits, flag it as a gap — do not invent a color.

---

## Complete Token Reference — 43 Semantic Tokens

### Brand (7 tokens)

| Token | Hex | Use for | Do NOT use for |
|-------|-----|---------|----------------|
| `color.brand.primary` | `#D9008D` | Primary CTA button fill, checked checkbox/radio fill, selected state indicators, focused input stroke | Body text, large surface fills, decorative use |
| `color.brand.primary-hover` | `#C00080` | Primary CTA hover state, brand interactive hover | Static elements, text |
| `color.brand.primary-pressed` | `#A30070` | Primary CTA pressed/active state, tap feedback on mobile | Static elements, text |
| `color.brand.primary-soft` | `#FFB2E4` | Focus ring (all non-destructive components), brand-accent borders | Text, primary fills |
| `color.brand.primary-subtle` | `#FFCCED` | Secondary button pressed fill, brand-tinted card backgrounds | Primary fills, text |
| `color.brand.primary-tint` | `#FFE5F6` | Secondary/tertiary button hover fill, lightest brand background | Text, borders, strong emphasis |
| `color.brand.secondary` | `#4A1535` | Premium surfaces, deep brand accent, partner headers | Primary interactive fills |

**Hierarchy (strongest to lightest):**
`primary` → `primary-soft` → `primary-subtle` → `primary-tint`

### Text (7 tokens)

| Token | Hex | Use for | Do NOT use for |
|-------|-----|---------|----------------|
| `color.text.primary` | `#161021` | Headings, body text, labels, input values, text on light colored fills (warning.subtle, positive.subtle) | De-emphasized content, disabled text |
| `color.text.secondary` | `#5C5570` | Descriptions, supporting text, placeholders, helpers, metadata, timestamps, default navigation/UI icons | Primary headings, interactive labels |
| `color.text.disabled` | `#A7A1B2` | All disabled text, disabled labels, disabled icons | Active or interactive text |
| `color.text.inverse` | `#FAF9FC` | Text on dark neutral surfaces (surface.inverse, brand.secondary) | Text on light backgrounds |
| `color.text.link` | `#D9008D` | Inline text links (default state) | Non-interactive text, headings |
| `color.text.link-hover` | `#C00080` | Text links on hover/focus | Default link state |
| `color.text.on-color` | `#FFFFFF` | Text on ANY colored fill (brand.primary, positive.primary, negative.primary, info.primary) | Text on light or dark neutral surfaces |

### Surface (6 tokens)

| Token | Hex | Use for | Do NOT use for |
|-------|-----|---------|----------------|
| `color.surface.base` | `#FAF9FC` | Page bg, card bg, input fill, modal bg, menu bg | Emphasis backgrounds, tinted states |
| `color.surface.subtle` | `#F1EDF7` | Hover tint, disabled fill, section cards, option hover row | Full-page backgrounds |
| `color.surface.muted` | `#E8E4F0` | Grouped sections, alternating rows, secondary cards, neutral pressed state | Borders, text, interactive fills |
| `color.surface.strong` | `#D7D3E0` | Stronger background differentiation (reserved) | Primary backgrounds, large surfaces |
| `color.surface.inverse` | `#161021` | Dark sections, inverse hero, preview blocks, footers | Regular product screens |
| `color.surface.selected` | `#FFE5F6` | Selected tab fill, active chip fill, current nav item bg, selected list row | Hover states (use subtle), pressed states |

### Border (5 tokens)

| Token | Hex | Use for | Do NOT use for |
|-------|-----|---------|----------------|
| `color.border.default` | `#D7D3E0` | Input borders, card borders, list dividers, section separators | Small controls (20px) where it's too faint |
| `color.border.subtle` | `#E8E4F0` | Inner-card dividers, light separators | Primary form borders |
| `color.border.strong` | `#665E75` | Checkbox border (unchecked), radio border (unchecked), small controls | Large surface borders |
| `color.border.brand` | `#FFB2E4` | Focus rings, brand-accent borders | Default form borders |
| `color.border.selected` | `#D9008D` | Selected tab indicator, active chip border, selected card outline | Default form borders, hover states |

**Size rule:** Large containers (100px+) → `default`. Small controls (20px) → `strong`.
**Selection rule:** Selected state = `surface.selected` fill + `border.selected` stroke.

### Positive — Success (4 tokens)

| Token | Hex | Use for |
|-------|-----|---------|
| `color.positive.primary` | `#008043` | Success text, icons, status indicators |
| `color.positive.soft` | `#A5E5C6` | Success outline, light positive border |
| `color.positive.subtle` | `#C9F0DD` | Success card background |
| `color.positive.tint` | `#E1FAED` | Success banner fill, lightest positive |

**When:** Payment successful, plan activated, recharge complete, connection active, validation passed.

### Negative — Error / Destructive (6 tokens)

| Token | Hex | Use for |
|-------|-----|---------|
| `color.negative.primary` | `#D92130` | Error text/border, destructive button fill, required asterisk |
| `color.negative.primary-hover` | `#BF1D2A` | Destructive button hover |
| `color.negative.primary-pressed` | `#A31824` | Destructive button pressed |
| `color.negative.soft` | `#FFB3B9` | Destructive focus ring, negative outline |
| `color.negative.subtle` | `#FFCCD0` | Error card background |
| `color.negative.tint` | `#FFE5E7` | Error banner fill, lightest negative |

**When:** Validation error, payment failed, connection lost, required field.

**Destructive button rules:** Red button is ONLY for irreversible, high-consequence actions — account deletion, active plan cancellation, payment method removal. Do NOT use red for "No thanks", "Cancel" (closing a form), "Skip", or routine declines. Those use secondary (brand) button. Wiom presents choices neutrally — red signals danger, not preference.

### Warning (3 tokens)

| Token | Hex | Use for |
|-------|-----|---------|
| `color.warning.primary` | `#B85C00` | Warning text on light backgrounds, warning icons |
| `color.warning.soft` | `#FFDA40` | Warning indicator fills (NOT for icons on white — invisible at 1.30:1) |
| `color.warning.subtle` | `#FFF2BF` | Warning banner fill |

**When:** Low balance, plan expiring, action required before deadline, system maintenance, auto-pay not enabled, any non-error alert.

**Warning icon rule:** Always use `warning.primary` (#B85C00) for warning icons on light backgrounds. NEVER use `warning.soft` (#FFDA40) as an icon color on white — yellow on white is 1.30:1 contrast, completely invisible.

### Info (4 tokens)

| Token | Hex | Use for |
|-------|-----|---------|
| `color.info.primary` | `#6D17CE` | Info icon, badge, status indicator |
| `color.info.soft` | `#D6B2FF` | Info outline, light info border |
| `color.info.subtle` | `#E4CCFF` | Info card background |
| `color.info.tint` | `#F1E5FF` | Info banner fill, lightest info |

**When:** Helpful tips, system information, feature announcements, neutral status.

### Overlay (1 token)

| Token | Hex | Use for |
|-------|-----|---------|
| `color.overlay.default` | `rgba(22,16,33, 0.50)` | Modal backdrop, bottom sheet scrim, blocking overlays |

**Skeleton/loading states:** Use `surface.muted` (#E8E4F0) for placeholder fills + `surface.subtle` (#F1EDF7) for shimmer highlights. No separate skeleton tokens.

---

## Icon Color Rules

Icons map to existing tokens — no separate icon tokens. Default icons use `text.secondary` (not `text.primary`) because filled icons have more visual mass than text at the same color.

| Context | Token | Hex |
|---------|-------|-----|
| Navigation, UI chrome, decorative | `color.text.secondary` | `#5C5570` |
| Actionable / tappable (back, close, menu) | `color.text.secondary` | `#5C5570` |
| Disabled | `color.text.disabled` | `#A7A1B2` |
| Inverse (on dark surface) | `color.text.inverse` | `#FAF9FC` |
| On colored fill | `color.text.on-color` | `#FFFFFF` |
| Success status | `color.positive.primary` | `#008043` |
| Error status | `color.negative.primary` | `#D92130` |
| Warning status | `color.warning.primary` | `#B85C00` |
| Info status | `color.info.primary` | `#6D17CE` |
| Brand emphasis (selected check, focused chevron) | `color.brand.primary` | `#D9008D` |

```
Icon on a colored fill or dark surface?
  → color.text.on-color / color.text.inverse

Disabled icon?
  → color.text.disabled

Status indicator (success, error, warning, info)?
  → Use {family}.primary
  → Warning: use warning.primary (#B85C00), NOT warning.soft (#FFDA40)

Brand emphasis (selected, focused)?
  → color.brand.primary

Default navigation / UI / decorative icon?
  → color.text.secondary (#5C5570)
```

---

## Decision Trees

### Which text color?

```
Text on a colored fill (brand.primary, positive.primary, negative.primary, info.primary)?
  → color.text.on-color (#FFFFFF)

Text on a dark neutral surface (surface.inverse, brand.secondary)?
  → color.text.inverse (#FAF9FC)

Text on a light colored surface (warning.subtle, positive.subtle, negative.subtle)?
  → color.text.primary (#161021)

Component is disabled?
  → color.text.disabled (#A7A1B2)

It's a text link?
  → color.text.link (#D9008D) / hover: color.text.link-hover (#C00080)

Heading, label, input value, or primary content?
  → color.text.primary (#161021)

Description, helper, placeholder, metadata, or timestamp?
  → color.text.secondary (#5C5570)
```

### Which surface color?

```
Standard page, card, input, or modal background?
  → color.surface.base (#FAF9FC)

Hover tint, disabled fill, or section card?
  → color.surface.subtle (#F1EDF7)

Grouped section or alternating row?
  → color.surface.muted (#E8E4F0)

Currently selected/active item (tab, chip, nav, list row)?
  → color.surface.selected (#FFE5F6)

Dark or inverse context?
  → color.surface.inverse (#161021)
```

### Which border color?

```
Large container (input, card, menu: 100px+ wide)?
  → color.border.default (#D7D3E0)

Small control (checkbox, radio: ~20px)?
  → color.border.strong (#665E75)

Light inner separator?
  → color.border.subtle (#E8E4F0)

Brand-accented boundary or focus ring?
  → color.border.brand (#FFB2E4)

Selected/active element indicator?
  → color.border.selected (#D9008D)
```

### Which interaction state?

```
Hovered?
  → Brand CTA: color.brand.primary-hover (#C00080)
  → Destructive: color.negative.primary-hover (#BF1D2A)
  → Secondary/tertiary button: color.brand.primary-tint (#FFE5F6) fill
  → Option row / neutral surface: color.surface.subtle (#F1EDF7)

Pressed/active?
  → Brand CTA: color.brand.primary-pressed (#A30070)
  → Destructive: color.negative.primary-pressed (#A31824)
  → Secondary/tertiary button: color.brand.primary-subtle (#FFCCED) fill
  → Neutral surface (list row, nav item): color.surface.muted (#E8E4F0)

Currently selected (tab, chip, nav)?
  → Surface: color.surface.selected (#FFE5F6)
  → Border/indicator: color.border.selected (#D9008D)

Focused?
  → All non-destructive: color.border.brand / color.brand.primary-soft (#FFB2E4) focus ring
  → Destructive: color.negative.soft (#FFB3B9) focus ring

Disabled?
  → Surface: color.surface.muted (#E8E4F0)
  → Text/icons: color.text.disabled (#A7A1B2)
  → Fills/strokes: base color at 40% opacity

Action inside a status banner (e.g., "Undo" on success toast, "Learn more" on info banner)?
  → Use brand interaction states (brand.primary-hover / brand.primary-pressed)
  → Exception: destructive action inside any context → negative hover/pressed
```

### Which feedback color family?

```
Success, completion, positive outcome?
  → Positive (green): primary/soft/subtle/tint

Error, failure, destructive action, required field?
  → Negative (red): primary/soft/subtle/tint + hover/pressed for buttons

Non-error alert, expiry, low balance, attention needed?
  → Warning (yellow): primary/soft/subtle

Helpful info, tips, announcements, neutral status?
  → Info (purple): primary/soft/subtle/tint
```

### Which shade within a feedback family?

```
Text or icon conveying the status?
  → {family}.primary (darkest, strongest)

Outline or light border around feedback element?
  → {family}.soft

Card or container background for feedback content?
  → {family}.subtle

Full-width banner or lightest background wash?
  → {family}.tint
```

**Status text on status banners:** Use `text.primary` (#161021) for body text on status banners — NOT `{family}.primary`. Several family primaries fail AA contrast on their own tint/subtle backgrounds (warning 4.09:1, positive 4.08:1, negative 3.51–4.18:1). Reserve `{family}.primary` for icons only on these surfaces. Exception: `info.primary` passes on its own backgrounds and can be used for text.

---

## Approved Contrast Pairings

All ratios calculated against final shipped hex values (April 2026).

### Text on neutral surfaces (all pass WCAG AA 4.5:1)

| Text | Surface | Ratio | Use case |
|------|---------|-------|----------|
| text.primary `#161021` | surface.base `#FAF9FC` | 17.71:1 | Default UI |
| text.primary `#161021` | surface.subtle `#F1EDF7` | 16.10:1 | Cards, sections |
| text.primary `#161021` | surface.muted `#E8E4F0` | 14.85:1 | Grouped areas |
| text.secondary `#5C5570` | surface.base `#FAF9FC` | 6.70:1 | Descriptions, metadata |
| text.inverse `#FAF9FC` | surface.inverse `#161021` | 17.71:1 | Dark sections |

### Text on colored fills (all pass WCAG AA 4.5:1)

| Text | Surface | Ratio | Use case |
|------|---------|-------|----------|
| text.on-color `#FFFFFF` | brand.primary `#D9008D` | 4.84:1 | Primary CTA label |
| text.on-color `#FFFFFF` | negative.primary `#D92130` | 4.99:1 | Destructive CTA label |
| text.on-color `#FFFFFF` | positive.primary `#008043` | 5.04:1 | Success icon label |
| text.on-color `#FFFFFF` | info.primary `#6D17CE` | 7.88:1 | Info badge label |

### Status banners — use text.primary for body text

| Foreground | Background | Ratio | Status |
|------------|------------|-------|--------|
| text.primary `#161021` | warning.subtle `#FFF2BF` | 16.55:1 | PASS |
| text.primary `#161021` | positive.tint `#E1FAED` | 16.90:1 | PASS |
| text.primary `#161021` | negative.tint `#FFE5E7` | 15.58:1 | PASS |
| text.primary `#161021` | info.tint `#F1E5FF` | 15.39:1 | PASS |
| info.primary `#6D17CE` | info.tint `#F1E5FF` | 6.53:1 | PASS — info can use family color for text |
| warning.primary `#B85C00` | warning.subtle `#FFF2BF` | 4.09:1 | **FAIL** — use text.primary for text |
| positive.primary `#008043` | positive.subtle `#C9F0DD` | 4.08:1 | **FAIL** — use text.primary for text |
| negative.primary `#D92130` | negative.tint `#FFE5E7` | 4.18:1 | **FAIL** — use text.primary for text |
| negative.primary `#D92130` | negative.subtle `#FFCCD0` | 3.51:1 | **FAIL** — use text.primary for text |

### Pairings to AVOID

| Text | Surface | Ratio | Why |
|------|---------|-------|-----|
| text.disabled | surface.muted | 2.00:1 | Disabled text on tinted surface — unreadable |
| text.inverse | surface.base | 1.00:1 | Invisible — same lightness |
| warning.soft `#FFDA40` | surface.base | 1.30:1 | Yellow on white — use warning.primary for icons |
| brand.primary | brand.primary-tint | 4.10:1 | Fails AA for text — use text.primary on brand surfaces |

### Mobile-specific note

Brand primary (#D9008D) on white is 4.62:1 — passes for non-text and large text but marginal for normal text. For small brand-colored text, use `brand.primary-hover` (#C00080, 5.63:1). Test critical pairings on actual budget devices.

---

## Component State Rules

### Error states
- Border/stroke → `color.negative.primary` (#D92130)
- Error helper text → `color.negative.primary` (#D92130)
- Error icon → `color.negative.primary` (#D92130)
- Labels + input text → NO change (stay `color.text.primary`)
- Checked checkbox/radio in error → fill becomes `color.negative.primary`

### Disabled states
- Text and icons → `color.text.disabled` (#A7A1B2)
- Container fill → `color.surface.subtle` (#F1EDF7)
- Filled/stroked elements → base color at 40% opacity
- No pointer events

### Focus ring
- Non-destructive components → `color.brand.primary-soft` (#FFB2E4)
- Destructive components → `color.negative.soft` (#FFB3B9)

### Selected states
- Surface fill → `color.surface.selected` (#FFE5F6)
- Border/indicator → `color.border.selected` (#D9008D)
- Distinct from hover (surface.subtle) and pressed (brand.primary-subtle)

---

## Strict Rules

### DO
- Use ONLY tokens from this list — never raw hex values
- Use `text.on-color` (#FFFFFF) for text on any colored fill
- Use `text.inverse` (#FAF9FC) for text on dark neutral surfaces
- Use `text.primary` (#161021) for text on light semantic surfaces (warning.subtle, positive.tint, etc.)
- Use `text.primary` (#161021) for body text on status banners — NOT family.primary colors
- Use `text.secondary` (#5C5570) for default navigation/UI icons — NOT text.primary
- Use `warning.primary` (#B85C00) for warning icons — NOT warning.soft (#FFDA40)
- Use the correct border weight by element size (default for large, strong for small)
- Use semantic feedback colors ONLY for their intended meaning
- Use `surface.selected` + `border.selected` for currently active/selected elements
- Use brand interaction states for actions inside status banners
- Follow the interaction state decision tree exactly
- Keep the consistent pattern: primary → soft → subtle → tint (dark to light)

### DO NOT
- Do NOT use `brand.primary` (#D9008D) as body text or large surface fill
- Do NOT use `text.primary` on dark/brand fills — use `text.inverse` or `text.on-color`
- Do NOT use `negative.primary` for decorative red — it means error/destructive only
- Do NOT use red (destructive) buttons for "No thanks", "Skip", "Cancel" (closing a form), or routine declines — use secondary (brand) button. Red is ONLY for irreversible actions (account deletion, plan cancellation, payment method removal)
- Do NOT use `surface.inverse` for regular screens — it's for rare high-contrast moments
- Do NOT use `text.on-color` on light or dark neutral surfaces — it's ONLY for colored fills
- Do NOT use `text.inverse` on light backgrounds — it will be invisible
- Do NOT use `warning.soft` (#FFDA40) as icon color on white backgrounds — invisible (1.30:1)
- Do NOT use `{family}.primary` as text color on status banners (except info) — use text.primary instead
- Do NOT invent new colors, opacities (except 40% for disabled), or gradients
- Do NOT mix feedback families (e.g., warning icon on a negative card)
- Do NOT use primitive tokens in UI — primitives are for theming infrastructure only

### WHEN IN DOUBT
If no token fits your use case, flag it as a gap. Do not approximate. Use the decision trees above to resolve ambiguity.

---

## Quick Hex Lookup

```
BRAND:     #D9008D  #C00080  #A30070  #FFB2E4  #FFCCED  #FFE5F6  #4A1535
TEXT:      #161021  #5C5570  #A7A1B2  #FAF9FC  #D9008D  #C00080  #FFFFFF
SURFACE:   #FAF9FC  #F1EDF7  #E8E4F0  #D7D3E0  #161021  #FFE5F6
BORDER:    #D7D3E0  #E8E4F0  #665E75  #FFB2E4  #D9008D
POSITIVE:  #008043  #A5E5C6  #C9F0DD  #E1FAED
NEGATIVE:  #D92130  #BF1D2A  #A31824  #FFB3B9  #FFCCD0  #FFE5E7
WARNING:   #B85C00  #FFDA40  #FFF2BF
INFO:      #6D17CE  #D6B2FF  #E4CCFF  #F1E5FF
OVERLAY:   rgba(22,16,33,0.50)
```

---

Apply this color system to the user's design request. Use token names in your output (not just hex values) so the design is traceable back to this system. If generating code, use the CSS custom property format: `var(--color-brand-primary)`, `var(--color-text-secondary)`, etc.
