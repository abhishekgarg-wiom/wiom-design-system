---
name: wiom-design-foundations
description: >
  Unified Wiom (CA) Design System foundation tokens and rules for Android (Jetpack Compose).
  Covers ALL visual foundations: color, typography, iconography, spacing, radius, stroke/border, and shadow.
  Use EVERY TIME you write, edit, review, or generate any UI for the Wiom app — Kotlin, Compose, XML, Figma.
  Trigger on: hex values, color tokens, TextStyle, Typography, font sizes, weights, sp/dp values, Icon(),
  painterResource, padding, margin, gap, border, stroke, radius, shadow, elevation, RoundedCornerShape,
  Modifier.border/shadow/padding, Arrangement.spacedBy, touch targets, dividers, or legacy tokens like
  T1_32_Bold, B2_14_Regular, radius-main-card, stroke-small. Also trigger when creating new screens,
  components, layouts, or reviewing PRs. If writing ANY visual UI for Wiom, consult this skill first.
---

# Wiom Design System — Unified Foundations

**7 foundations · 1 design system · Platform: Android (Jetpack Compose)**
**Context: Mobile-first for lower/middle-income Indian households. Budget Android devices, outdoor use, Hindi + English, trust-heavy transactional flows.**

---

## How to Use This Skill

This skill is the single entry point for all Wiom visual foundation decisions. It contains:

1. **This file (SKILL.md)** — Quick-reference tables, cross-foundation rules, and decision routing
2. **7 reference files** in `references/` — Full token tables, component mappings, implementation code, anti-patterns, and industry benchmarks

### When to read a reference file

Read the relevant reference file(s) BEFORE writing any code or making design decisions. Use the routing table below:

| You're working on... | Read |
|----------------------|------|
| Any color, fill, tint, or background | `references/color.md` |
| Text styling, font size, weight, line-height | `references/typography.md` |
| Icons, icon sizes, fill states, touch targets | `references/iconography.md` |
| Padding, margin, gap, layout spacing, touch targets | `references/spacing.md` |
| Corner radius, rounded corners, nested containers | `references/radius.md` |
| Borders, strokes, dividers, focus rings | `references/stroke.md` |
| Shadows, elevation, depth, overlays | `references/shadow.md` |
| Full component (e.g., a card) | Read ALL relevant files — a card involves color + typography + spacing + radius + stroke/shadow |

---

## Quick Token Reference

### Color — 43 Semantic Tokens

Use ONLY Tier 2 semantic tokens. Never raw hex in UI code.

| Category | Key Tokens | Hex Examples |
|----------|-----------|-------------|
| **Brand** | `color.brand.primary` / `primary-hover` / `primary-pressed` / `primary-soft` / `primary-subtle` / `primary-tint` / `secondary` | `#D9008D` `#C00080` `#A30070` `#FFB2E4` `#FFCCED` `#FFE5F6` `#4A1535` |
| **Text** | `color.text.primary` / `secondary` / `disabled` / `inverse` / `link` / `link-hover` / `on-color` | `#161021` `#5C5570` `#A7A1B2` `#FAF9FC` `#D9008D` `#C00080` `#FFFFFF` |
| **Surface** | `color.surface.base` / `subtle` / `muted` / `strong` / `inverse` / `selected` | `#FAF9FC` `#F1EDF7` `#E8E4F0` `#D7D3E0` `#161021` `#FFE5F6` |
| **Border** | `color.border.default` / `subtle` / `strong` / `brand` / `selected` | `#D7D3E0` `#E8E4F0` `#665E75` `#FFB2E4` `#D9008D` |
| **Positive** | `color.positive.primary` / `soft` / `subtle` / `tint` | `#008043` `#A5E5C6` `#C9F0DD` `#E1FAED` |
| **Negative** | `color.negative.primary` / `primary-hover` / `primary-pressed` / `soft` / `subtle` / `tint` | `#D92130` `#BF1D2A` `#A31824` `#FFB3B9` `#FFCCD0` `#FFE5E7` |
| **Warning** | `color.warning.primary` / `soft` / `subtle` | `#B85C00` `#FFDA40` `#FFF2BF` |
| **Info** | `color.info.primary` / `soft` / `subtle` / `tint` | `#6D17CE` `#D6B2FF` `#E4CCFF` `#F1E5FF` |
| **Overlay** | `color.overlay.default` | `rgba(22,16,33,0.50)` |

**Critical rules:** `text.on-color` (#FFFFFF) on colored fills. `text.inverse` (#FAF9FC) on dark neutrals. `text.primary` (#161021) on light status surfaces. Default icons use `text.secondary` (#5C5570). Warning icons use `warning.primary` (#B85C00), NEVER `warning.soft`. Red buttons ONLY for irreversible destructive actions.

→ Full details: `references/color.md`

### Typography — 13 Semantic Tokens

Font: **Noto Sans**. All values in sp. Never use sizes outside the scale.

| Token | Size | Weight | LH | Use |
|-------|------|--------|----|-----|
| `type.display.lg` | 48sp | Bold 700 | 64sp | Hero / splash / marketing |
| `type.heading.xl` | 32sp | Bold 700 | 44sp | Section hero |
| `type.heading.lg` | 24sp | Bold 700 | 32sp | Page title / dialog header |
| `type.heading.md` | 20sp | Bold 700 | 28sp | Card group / sub-section |
| `type.title.lg` | 20sp | Medium 500 | 28sp | Nav / toolbar title |
| `type.title.sm` | 16sp | Bold 700 | 24sp | Card title / list header |
| `type.body.lg` | 16sp | Regular 400 | 24sp | Long-form / description |
| `type.body.md` | 14sp | Regular 400 | 20sp | Default paragraph / chat |
| `type.body.sm` | 12sp | Regular 400 | 16sp | Caption / helper / footnote |
| `type.label.lg` | 16sp | SemiBold 600 | 24sp | Default button / CTA |
| `type.label.md` | 14sp | SemiBold 600 | 20sp | Input label / nav / small btn |
| `type.label.sm` | 12sp | SemiBold 600 | 16sp | Chip / badge / tag |
| `type.meta.xs` | 10sp | Regular 400 | 16sp | ⚠ Timestamp / badge ONLY |

**Default:** `body.md` for reading text, `label.md` for interactive text, `label.lg` for buttons.

→ Full details: `references/typography.md`

### Iconography — 4 Size Tokens

Library: **Material Symbols Rounded** only. Filled is default. Outlined only for toggle-off/nav-unselected.

| Token | Glyph | Container | Touch Target | Use |
|-------|-------|-----------|-------------|-----|
| `icon.xs` | 16dp | 24dp | 40dp | ⚠ Restricted: inline indicators, input trailing only |
| `icon.sm` | 20dp | 32dp | 40dp | Button icons, compact controls |
| `icon.md` | 24dp | 40dp | 48dp | **Default.** Toolbar, nav, list, chips, cards |
| `icon.lg` | 48dp | 56dp | 56dp | Empty states, onboarding, hero |

**Never:** `Icons.Default/Filled/Outlined/Sharp/TwoTone`. Never create custom SVGs — ask for drawables.

→ Full details: `references/iconography.md`

### Spacing — 4px Grid, 13 Primitives

Every spacing/sizing/padding/margin value must be a **multiple of 4px**.

| Token | 4px | 8px | 12px | 16px | 24px | 32px | 40px | 48px | 64px | 72px | 84px |
|-------|-----|-----|------|------|------|------|------|------|------|------|------|

**Hierarchy:** Tight (8–12px) = one unit. Medium (16–24px) = same section. Loose (32–40px+) = separate sections.
**Margins:** Never. Parents own gap between children (Arrangement.spacedBy). Children own internal padding.
**Screen margins:** 16px default, 12px chat screens. Between cards: 16px. Card padding: 16px.
**Touch targets:** Action 48px, Navigation 32px, Info 24px.

→ Full details: `references/spacing.md`

### Radius — 7 Tokens on 4px Grid

| Token | Value | Use |
|-------|-------|-----|
| `radius-none` | 0px | Table cells, app bar, square thumbnails |
| `radius-tiny` | 4px | Badges, tags, tooltips, checkbox |
| `radius-small` | 8px | Chips, snackbars, small containers |
| `radius-medium` | 12px | Input fields, nested containers |
| `radius-large` | 16px | Buttons, cards, main containers |
| `radius-xlarge` | 24px | Dialogs, modals, bottom sheets |
| `radius-full` | 9999px | Avatars, pills, FABs, toggles, radios |

**Nested formula:** inner-radius = outer-radius − padding. Common: 16px outer + 4px pad → 12px inner.
**Corner type:** Standard circular (not squircle). Figma corner smoothing: 0%.

→ Full details: `references/radius.md`

### Stroke — 2 Width Tokens

| Token | Value | Use |
|-------|-------|-----|
| `stroke-small` | 1px | Dividers, card outlines, input borders (rest) |
| `stroke-medium` | 2px | Outlined buttons, checkbox/radio, selected/active/error states, focus rings |

**Position:** Always inside. Focus rings outside (a11y only).
**Style:** Solid always. Dashed only for empty states / upload zones.
**Filled state rule:** When a component becomes filled (checked checkbox, toggled switch), the border is removed.

→ Full details: `references/stroke.md`

### Shadow — 5 Tokens

| Token | CSS Value | Use |
|-------|-----------|-----|
| `shadow.none` | `none` | Buttons, inputs, chips, flat surfaces |
| `shadow.sm` | `0 1px 3px 0 rgba(0,0,0,0.15)` | Sticky nav (scrolled), tooltips, passive cards |
| `shadow.md` | `0 2px 6px 0 rgba(0,0,0,0.15)` | Interactive cards, bottom sheets, dropdowns, FAB |
| `shadow.lg` | `0 4px 12px 0 rgba(0,0,0,0.15)` | Floating toasts, dropdowns over busy backgrounds |
| `shadow.xl` | `0 8px 24px 0 rgba(0,0,0,0.15)` | Modal dialogs (always with overlay backdrop) |

**Rule:** Default to `shadow.md`. Max 1–2 `shadow.lg` per screen. `shadow.xl` always paired with `color.overlay.default`.

→ Full details: `references/shadow.md`

---

## Cross-Foundation Rules

These rules span multiple foundations. Violations are common — check these first.

### 1. Shadow vs Border — Never Both
A component uses EITHER a border OR a shadow, not both. Flat (0dp) → 1px border. Elevated (shadow) → no border.
**Exception:** On critical interactive surfaces (payment cards), pair `shadow.md` + `stroke-small` + `color.border.default` for budget device robustness.

### 2. Filled State = No Border
Checked checkbox, toggled switch, selected chip, filled button — fill IS the boundary. Remove the stroke.

### 3. Radius Matches Stroke
Border radius must match the component's radius token. Focus ring radius = component radius + 2px offset.

### 4. Spacing Owns Layout, Not Components
Parents set gap between children. Children set their own padding. No margins — ever.

### 5. Touch Targets Are Non-Negotiable
Action icons: 48×48px minimum. Navigation: 32×32px. Info: 24×24px. Buttons: padding naturally achieves 48px height.

### 6. Token-Only Values
Every color, size, spacing, radius, stroke, and shadow value must come from the token system. No hardcoded values. No "close enough" approximations.

### 7. Status Color Families Don't Mix
A success icon never appears on a warning surface. A negative border never wraps a positive card. Each status family (positive/negative/warning/info) is self-contained.

### 8. Text on Status Banners
Use `text.primary` (#161021) for body text on status banners — NOT `{family}.primary`. Exception: `info.primary` passes contrast on its own backgrounds.

### 9. Dividers Inside Cards Respect Padding
Dividers inside a card are inset to match the card's content padding (typically 16px from both edges). Never full-bleed inside a card.

### 10. Disabled State Pattern
Text/icons → `color.text.disabled`. Fill → `color.surface.subtle`. Stroked elements → base color at 40% opacity. Stroke width stays the same (1px). No pointer events.

---

## Component Quick-Build Reference

When building a common component, here's which tokens from each foundation apply:

### Card (interactive, flat)
| Foundation | Token |
|-----------|-------|
| Surface | `color.surface.base` |
| Border | `stroke-small` (1px) + `color.border.default` |
| Radius | `radius-large` (16px) |
| Shadow | `shadow.none` (flat) OR `shadow.md` (elevated — then no border) |
| Padding | `space-16` all sides |
| Title | `type.title.sm` + `color.text.primary` |
| Body | `type.body.md` + `color.text.secondary` |
| Internal gap | `space-16` between sections, `space-8` title→subtitle, `space-32` content→CTA |

### Primary Button
| Foundation | Token |
|-----------|-------|
| Fill | `color.brand.primary` → hover: `primary-hover` → pressed: `primary-pressed` |
| Text | `type.label.lg` + `color.text.on-color` |
| Radius | `radius-large` (16px) |
| Shadow | `shadow.none` |
| Stroke | None (filled = no border) |
| Icon (if any) | `icon.sm` (20dp), filled |
| Min height | 48px (via `space-12` vertical padding + 24sp line-height) |

### Input Field
| Foundation | Token |
|-----------|-------|
| Border (rest) | `stroke-small` (1px) + `color.border.default` |
| Border (active) | `stroke-medium` (2px) + `color.brand.primary` |
| Border (error) | `stroke-medium` (2px) + `color.negative.primary` |
| Radius | `radius-medium` (12px) |
| Label | `type.label.md` + `color.text.primary` |
| Value | `type.body.md` + `color.text.primary` |
| Helper | `type.body.sm` + `color.text.secondary` |
| Padding | `space-16` horizontal, `space-12` vertical |
| Shadow | `shadow.none` |

### Bottom Sheet
| Foundation | Token |
|-----------|-------|
| Surface | `color.surface.base` |
| Radius | `radius-xlarge` (24px) top corners only |
| Shadow | `shadow.md` (standard) or `shadow.xl` (full-screen, with overlay) |
| Padding | `space-16` all sides |
| Header | `type.heading.md` + `color.text.primary` |
| Stroke | None (shadow provides depth) |

---

## Anti-Pattern Checklist

Before submitting any Wiom UI code, verify NONE of these are present:

- [ ] Hardcoded hex color values (use semantic tokens)
- [ ] Hardcoded dp/sp/px values for spacing, radius, stroke, or type (use tokens)
- [ ] Border AND shadow on the same component (pick one)
- [ ] Border on a filled/checked/toggled component (filled = no border)
- [ ] Same radius on nested containers (use inner = outer − padding)
- [ ] Full-bleed divider inside a padded card (inset to match padding)
- [ ] `warning.soft` (#FFDA40) as icon color on white (invisible — use `warning.primary`)
- [ ] `text.on-color` on neutral surfaces (only for colored fills)
- [ ] `Icons.Default` / `Icons.Filled` / `Icons.Outlined` (use Material Symbols Rounded drawables)
- [ ] Icon sizes not in {16, 20, 24, 48}dp
- [ ] Spacing values not on the 4px grid
- [ ] Font sizes not in the 13-token type scale
- [ ] Margins on child components (use parent gap)
- [ ] Red button for non-destructive actions ("Cancel", "Skip", "No thanks")
- [ ] `meta.xs` (10sp) used for anything other than timestamps/badges
- [ ] Missing `contentDescription` on interactive icons
- [ ] Focus ring with same radius as component (should be component radius + 2px)

---

*Wiom Design System Foundations — Unified Skill. April 2026.*
*References: color.md · typography.md · iconography.md · spacing.md · radius.md · stroke.md · shadow.md*
