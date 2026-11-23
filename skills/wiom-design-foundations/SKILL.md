---
name: wiom-design-foundations
description: >
  Unified Wiom (CA) Design System foundation tokens and rules for Android (Jetpack Compose).
  Covers ALL visual foundations: color, typography, iconography, spacing, radius, stroke/border, and shadow.
  Use EVERY TIME you write, edit, review, or generate any UI for the Wiom app — Kotlin, Compose, XML, Figma.
  Trigger on: hex values, element-first color tokens (bg.*, text.*, stroke.*, icon.*), brand pink #D9008D,
  TextStyle, Typography, font sizes, weights, sp/dp values, Icon(), painterResource, padding, margin, gap,
  border, stroke, radius, shadow, elevation, RoundedCornerShape, Modifier.border/shadow/padding,
  Arrangement.spacedBy, touch targets, dividers, or legacy tokens like T1_32_Bold, B2_14_Regular,
  radius-main-card, stroke-small, color.brand.primary, color.warning.primary. Also trigger when creating
  new screens, components, layouts, or reviewing PRs. If writing ANY visual UI for Wiom, consult this skill first.
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
| Any core UI chrome (bg, text, stroke, icon, banners) | `references/color/core-tokens.md` |
| Any CTA / button (Primary, Secondary, Tertiary, Pre-booking, Destructive) | `references/color/actions.md` |
| Status bar, badges, empty states, tooltips, ratings, avatars, overlays | `references/color/patterns.md` |
| Chat screens (bubbles, reply, system messages) | `references/color/chat.md` |
| Contrast pairs / WCAG verification | `references/color/contrast.md` |
| Primitive palette | `references/color/primitives.md` |
| Color overview / routing | `references/color.md` |
| Text styling, font size, weight, line-height | `references/typography.md` |
| Icons, icon sizes, fill states, touch targets | `references/iconography.md` |
| Padding, margin, gap, layout spacing, touch targets | `references/spacing.md` |
| Corner radius, rounded corners, nested containers | `references/radius.md` |
| Borders, strokes, dividers, focus rings | `references/stroke.md` |
| Shadows, elevation, depth, overlays | `references/shadow.md` |
| Full component (e.g., a card) | Read ALL relevant files — a card involves color + typography + spacing + radius + stroke/shadow |

---

## Quick Token Reference

### Color — Element-First Semantic Tokens

**Four namespaces matched to Compose slots.** Never use raw hex in UI code. Legacy role-first tokens (`color.brand.primary`, `color.warning.primary`, etc.) are DEPRECATED.

| Compose slot | Token namespace |
|---|---|
| `Box(background=…)`, `Surface(color=…)`, `Modifier.background(…)` | `bg.*` |
| `Text(color=…)` | `text.*` |
| `BorderStroke(color=…)`, `Divider(color=…)`, `Modifier.border(…)` | `stroke.*` |
| `Icon(tint=…)`, `painterResource + tint` | `icon.*` |

**Primitive families (67 total, `internal` only — never import directly in feature code):**

| Family | Range | Purpose / Anchor |
|---|---|---|
| **Neutral** | 50–950 | Warm mauve undertone UI chrome |
| **Gray** | 50–950 | Pure grayscale for chat domain; Gray_50 = #FFFFFF |
| **Brand** | 100–900 | Brand_600 = **#D9008D** (non-negotiable) |
| **Positive** | 100–900 | Success green |
| **Critical** | 100–900 | Error red / destructive |
| **Warning** | 100–900 | Yellow ONLY (never orange); Warning_300 = #E2B203 gold |
| **Info** | 100–900 | Purple; Info_600 = #6D17CE |

**Critical rules:**
- Body text on most light surfaces: `text.default` (#161021). **Exception: warning surfaces use `text.onWarning` (#372902)** — the one status family with its own body-text token (see warning rule below).
- Error banner: `bg.critical.subtle` + `text.default` body + `text.critical` for heading/icon. Never white-on-red (that's destructive CTA only).
- Warning-family text token: `text.onWarning` (#372902 ← Warning_900). **One token, three valid surfaces:** `bg.warning` (7.16:1 AA), `bg.warning.subtle` (8.76:1 AA), AND `bg.default` (14.18:1 AAA) for inline warning-tinted text on white. Warning is the only family where the `text.on*` token doubles as the family's default-surface text — its bg is light (yellow/gold), so its `text.on*` is dark (olive), which works on white too. Critical/positive/info can't do this (their `text.on*` tokens are white). `text.warning` as a separate token is intentionally not defined. White on warning fills fails AA (1.98:1 / ~1.75:1).
- Primary CTA: `bg.brand` (#D9008D). Max **one** per screen.
- `bg.critical` as CTA fill: destructive actions ONLY (Delete, Cancel-plan, Remove). Never for "Dismiss"/"Skip"/"Close".
- CTA icons use `icon.*` tokens, not `text.on*`. Primary/Destructive → `icon.inverse`; Secondary/Tertiary → `icon.brand`; Pre-booking → `icon.action`.
- Selected states (tab bg, active chip, current row): `bg.selected` (#FFCCED, Brand_200) + `text.selected` / `stroke.selected` (#D9008D, Brand_600).
- Tappable chrome icons (back, close, menu, overflow, filter): `icon.action` (#352D42, Neutral_900). Decorative / non-tappable icons: `icon.nonAction` (#898296, Neutral_500) — dark enough to pass graphics AA (3.48:1 on `bg.default`).
- Premium / partner header surface (non-action): `bg.brand.bold` (#443152, Brand_900) + `text.inverse`.
- Overlay scrim (modal backdrop, bottom-sheet scrim): `overlay.scrim` (rgba(22,16,33,0.50)) — the only overlay token.
- **`text.placeholder` doesn't exist — use `text.disabled`.** Same hex #A7A1B2 serves both "text in a visually disabled component" and "placeholder copy in an empty form field." An empty input isn't truly disabled (it's awaiting input) but its placeholder uses the same muted gray by design. Sub-AA (2.39:1) is intentional for placeholders — they're ephemeral.
- **Dark mode: NOT yet defined.** If a dark-mode screen is requested, flag to the design system team — do not implement ad-hoc.
- **Flag a gap, don't invent.** If no token fits, leave a `// GAP: needed X for Y, no token exists` comment and ask. Do not eyedrop from Figma or approximate with the closest token. Only `overlay.scrim` and the 40% disabled-opacity fallback are system-provided exceptions.

→ Full details: sub-references in `references/color/` — **read `core-tokens.md` + `actions.md` before composing any new screen**.

### Typography — 13 Semantic Tokens

Font: **Noto Sans**. All values in sp. Never use sizes outside the scale.

| Token | Size | Weight | LH | Use |
|-------|------|--------|----|-----|
| `type.display.lg` | 48sp | Bold 700 | 64sp | Hero / splash / marketing |
| `type.heading.xl` | 32sp | Bold 700 | 44sp | Section hero |
| `type.heading.lg` | 24sp | Bold 700 | 32sp | Page title |
| `type.heading.md` | 20sp | Bold 700 | 28sp | Card group / sub-section |
| `type.title.lg` | 20sp | Medium 500 | 28sp | Nav / toolbar title |
| `type.title.sm` | 16sp | Bold 700 | 24sp | Card title / list header |
| `type.body.lg` | 16sp | Regular 400 | 24sp | ★ **Default reading text** — paragraph, description, toast body, input value, list primary, empty-state |
| `type.body.md` | 14sp | Regular 400 | 20sp | Card description, chat bubble, helper, banner body, inline error, list secondary |
| `type.body.sm` | 12sp | Regular 400 | 16sp | Caption, chat reply preview, activity timestamp |
| `type.label.lg` | 16sp | SemiBold 600 | 24sp | ★ **Default interactive** — CTA (all types), input label, radio/checkbox/switch, toast action |
| `type.label.md` | 14sp | SemiBold 600 | 20sp | Chip, filter, tab, badge label, inline secondary btn ("Edit", "See all") |
| `type.label.sm` | 12sp | SemiBold 600 | 16sp | Chat reply sender, bottom nav |
| `type.meta.xs` | 10sp | Regular 400 | 16sp | ⚠ Chat-bubble timestamp / badge count ONLY |

**Default: 16sp.** `body.lg` for reading text, `label.lg` for interactive text. Page headers default to `heading.lg` (24 Bold); dialog and bottom-sheet headers are `heading.md` (20 Bold). Chat bubble body is an **intentional exception** at `body.md` (14) — density matters in dense-message domain. Floor rule: sub-14sp only for captions/chat-reply/timestamps; `meta.xs` 10sp only for chat-bubble timestamps and badge counts. Never for Hindi body text.

**Pairing rule (title → description):** Title 24/20 → description 16. Title/label 16 → description 14.

→ Full details: `references/typography.md`

### Iconography — 4 Size Tokens

Library: **Material 3 Icons Rounded** (`Icons.Rounded.*` from `androidx.compose.material:material-icons-extended`). Filled is default. Outlined only for toggle-off/nav-unselected. Brand/partner icons (`ic_wiom_*`, `ic_partner_*`) are the ONLY icons that use `painterResource()`.

| Token | Glyph | Container | Touch Target | Use |
|-------|-------|-----------|-------------|-----|
| `icon.xs` | 16dp | 24dp | 40dp | ⚠ Restricted: inline indicators, input trailing only |
| `icon.sm` | 20dp | 32dp | 40dp | Button icons, compact controls |
| `icon.md` | 24dp | 40dp | 48dp | **Default.** Toolbar, nav, list, chips, cards |
| `icon.lg` | 48dp | 56dp | 56dp | Empty states, onboarding, hero |

**Never:** `Icons.Default/Filled/Outlined/Sharp/TwoTone`. Never create custom SVGs — ask for drawables.

→ Full details: `references/iconography.md`

### Spacing — 4px Grid · 13 primitives · 11 semantic tokens

Every spacing/sizing/padding/margin value must be a **multiple of 4px**. Primitives `space/1` and `space/2` are utility-only (hairlines, optical adjustments) — they have no semantic alias.

| Semantic tokens (11) | 4px | 8px | 12px | 16px | 24px | 32px | 40px | 48px | 64px | 72px | 84px |
|-------|-----|-----|------|------|------|------|------|------|------|------|------|

**Hierarchy:** Tight (8–12px) = one unit. Medium (16–24px) = same section. Loose (32–40px+) = separate sections.
**Margins:** Never. Parents own gap between children (Arrangement.spacedBy). Children own internal padding.
**Screen margins:** 16px default, 12px chat screens. Between cards: 16px. Card padding: 16px.
**Dialog padding:** 24px all sides. **Bottom sheet padding:** 16px all sides.

→ Full details: `references/spacing.md`

### Radius — 7 Tokens on 4px Grid

| Token | Value | Use |
|-------|-------|-----|
| `radius-none` | 0px | Table cells, app bar, square thumbnails |
| `radius-tiny` | 4px | Badges, tags, tiny chips, lozenges, checkbox |
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

**Focus-ring color tokens** (saturated, pulled from `stroke.*.focus` in `references/color/core-tokens.md` — cite these directly from component skills):

| Token | Hex | Use |
|---|---|---|
| `stroke.brand.focus` | #D9008D | Keyboard focus ring · Secondary CTA rest border · active input |
| `stroke.critical.focus` | #D92130 | Invalid-input border · destructive-element focus ring |
| `stroke.positive.focus` | #008043 | Focus ring on positive elements |
| `stroke.info.focus` | #6D17CE | Focus ring on info elements |

**Position:** Always inside. Focus rings outside (a11y only), offset 2px, radius = component radius + 2px.
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

**Rule:** Default to `shadow.md`. Max 1–2 `shadow.lg` per screen. `shadow.xl` is always paired with `overlay.scrim` (the modal backdrop — the only overlay token).

→ Full details: `references/shadow.md`

---

### Motion: selection transitions · 1 token

| Token | Value | Compose | Use |
|---|---|---|---|
| `motion.micro` + `ease.standard` | 200 ms · cubic (0.4, 0, 0.2, 1) | `tween(200, easing = FastOutSlowInEasing)` | Selection colour changes: checkbox, radio, switch (track + thumb), pill tab, underline filter, chip, selected list row |

```kotlin
object WiomMotion {
    const val MICRO_MS = 200
    /** Selection / state colour change. Same curve as iOS `motion/micro` + `ease/standard`. */
    fun <T> selection(): FiniteAnimationSpec<T> = tween(MICRO_MS, easing = FastOutSlowInEasing)
}

/** Clear version of a colour: fade to / from this, never Color.Transparent. */
fun Color.clear(): Color = copy(alpha = 0f)
```

- **The value commits on tap; only the look eases.** Callbacks, state and TalkBack fire immediately. Never wait for the animation.
- **Animate colours in place** with `animateColorAsState(target, WiomMotion.selection())` (and `animateDpAsState` for the switch thumb). Selection and deselection use the same spec.
- **Fade to `color.clear()`, never `Color.Transparent`.** `Color.Transparent` is transparent *black*, so pink → Transparent passes through a muddy dark mid-frame. A fill that appears or disappears animates between `bg.x` and `bg.x.clear()`.
- **Pressed stays instant.** Press feedback (`bg.subtle`, `bg.brand.pressed`) snaps; use `snap()` while pressed.
- **Nothing moves between controls.** No sliding pill or underline from one item to another, no bounce, no scale, no width animation.
- **Keep control identity stable** (no `key()` change on toggle) so a quick second tap reverses from the current colour.
- **System "Remove animations"** is honoured automatically by Compose. Don't override it.
- Same rule on iOS, so both apps feel the same. Owner decision, 23 Nov 2026.

## Cross-Foundation Rules

These rules span multiple foundations. Violations are common — check these first.

### 1. Shadow vs Border — Never Both
A component uses EITHER a border OR a shadow, not both. Flat (0dp) → 1px border. Elevated (shadow) → no border.
**Exception:** On critical interactive surfaces (payment cards), pair `shadow.md` + `stroke-small` (1px) + `stroke.subtle` (#D7D3E0) for budget device robustness.

### 2. Filled State = No Border
Checked checkbox, toggled switch, selected chip, filled button — fill IS the boundary. Remove the stroke.

### 3. Radius Matches Stroke
Border radius must match the component's radius token. Focus ring radius = component radius + 2px offset.

### 4. Spacing Owns Layout, Not Components
Parents set gap between children. Children set their own padding. No margins — ever.

### 5. Touch Targets Are Non-Negotiable
Owned by `spacing.md` §6. Three-tier system by action importance. Tiers are **minimums** — a component may set a larger target (list rows and tabs are 48 dp):
- **Action** 48×48dp (`space-48`) — high-consequence interactive icons (delete, edit, share, close, menu, back)
- **Navigation** 32×32dp (`space-32`) — medium-priority frequent-use icons (expand, sort, arrows, pagination, filters)
- **Info** 24×24dp (`space-24`) — low-stakes informational icons (help, tooltip triggers, status indicators)

Buttons reach ≥48dp height automatically via `space-12` vertical padding + 24sp line-height — no explicit touch-target rule needed. `icon.xs` (16dp glyph) is restricted — never use it for a standalone tappable action; add invisible hit-area padding so any tappable icon still reaches its tier's minimum.

### 6. Token-Only Values
Every color, size, spacing, radius, stroke, and shadow value must come from the token system. No hardcoded values. No "close enough" approximations.

### 7. Status Color Families Don't Mix
A success icon never appears on a warning surface. A critical border never wraps a positive card. Each status family (**positive / critical / warning / info** — "critical", not "negative"; "negative" is a legacy term) is self-contained.

### 8. Text on Status Banners
Body text on status banners is family-specific:
- **Critical & Positive:** `text.default` (#161021). Reserve `text.critical` / `text.positive` for headings/emphasis (their subtle-bg contrast is AA-large only for critical, marginal for positive).
- **Warning (badge + banner + inline on default surface):** `text.onWarning` (#372902) on all three — `bg.warning` (7.16:1 AA), `bg.warning.subtle` (8.76:1 AA), and `bg.default` (14.18:1 AAA). Warning is the only family whose `text.on*` doubles as the family's default-surface text, because warning bgs are light so `text.on*` is dark. Do not use `text.default` on warning surfaces. No separate `text.warning` token — `text.onWarning` covers the whole family.
- **Info:** either `text.default` or `text.info` (#6D17CE). Both AA (6.53:1 for text.info on `bg.info.subtle`).

### 9. Dividers Inside Cards Respect Padding
Dividers inside a card are inset to match the card's content padding (typically 16px from both edges). Never full-bleed inside a card.

### 10. Disabled State Pattern
A disabled component uses all three disabled tokens together:

- **Surface / fill** → `bg.disabled` (#D7D3E0, Neutral_300)
- **Text** → `text.disabled` (#A7A1B2, Neutral_400)
- **Icon** → `icon.disabled` (#A7A1B2, Neutral_400 — same hex as `text.disabled`; keeps the icon visible on `bg.disabled`). Distinct from `icon.nonAction` (Neutral_500 #898296), which is darker because decorative icons need graphics AA on light surfaces.
- **Outline variants** (Secondary CTA, inputs) additionally change stroke from the rest-state token to `stroke.subtle` (#D7D3E0)

Triple-dim lands at ~1.64:1 (text/icon on disabled surface) — **intentionally sub-AA.** Disabled signals "not available," not "readable." The Neutral_400 / Neutral_300 hex split guarantees content stays visible above the disabled surface (unlike an older pattern where `icon.disabled` shared hex with `bg.disabled` and vanished).

**Per-CTA specifics** live in `color/actions.md` — in short: Primary/Destructive/Pre-booking swap the fill to `bg.disabled`; Secondary additionally lightens the stroke; Tertiary changes only text + icon. All CTAs share the same text.disabled + icon.disabled tokens.

**For non-Neutral base colors** (Brand/Positive/Critical/Warning/Info) with no dedicated disabled token, apply **40% opacity** to the base color. Never apply opacity to Neutral-family tokens. No pointer events (`cursor: not-allowed`, `onClick = null`).

### 11. Surface Gutter Contract — Per Surface

Components are end-to-end (`fillMaxWidth()`); the **host surface** owns the horizontal gutter, and the gutter is **not universal 16 dp** — it varies by surface:

| Surface | Horizontal gutter |
|---|---|
| Top bar · Nav bar · Bottom sheet | **edge-to-edge** (0 dp from screen edges; the surface fills the screen width) |
| Toast | **8 dp** left + 8 dp right (344 dp wide on a 360 screen; the toast floats wider than the 16 dp surface gutter; see `wiom-toast`) |
| Dialog | **24 dp** left + 24 dp right (content area = 264 dp on a 360 screen) |
| List item | **edge-to-edge** when on a screen with no surface background; **16 dp** standard padding when inside a card / list-group with a background colour |
| Screen content · Sticky CTA bar · Filter bar | **16 dp** left + 16 dp right (default screen-body gutter) |

A component's **own internal padding** (CTA's 16 dp pill pad, list-item's 16 dp inner gutter, dialog's 24 dp content padding, bottom-sheet Header's 16 dp horizontal pad) is component chrome and stays as defined in each component skill — distinct from the surface gutter above. Each component skill's §3 Surface contract should match this table for its host surface.

---

## Component Quick-Build Reference

When building a common component, here's which tokens from each foundation apply:

### Card (interactive, flat)
| Foundation | Token |
|-----------|-------|
| Surface | `bg.default` |
| Border | `stroke-small` (1px) + `stroke.subtle` (#D7D3E0 — card outlines) |
| Radius | `radius-large` (16px) |
| Shadow | `shadow.none` (flat) OR `shadow.md` (elevated — then no border) |
| Padding | `space-16` all sides |
| Title | `type.title.sm` + `text.default` |
| Body | `type.body.md` + `text.subtle` |
| Internal divider | `stroke-small` (1px) + `stroke.default` (#E8E4F0 — lighter, inner only) — inset to card padding |
| Internal gap | `space-16` between sections, `space-8` title→subtitle, `space-32` content→CTA |

### Primary Button
| Foundation | Token |
|-----------|-------|
| Fill | `bg.brand` → hover: `bg.brand.hover` → pressed: `bg.brand.pressed` |
| Text | `type.label.lg` + `text.onBrand` |
| Radius | `radius-large` (16px) |
| Shadow | `shadow.none` |
| Stroke | None (filled = no border) |
| Icon (if any) | `icon.sm` (20dp), filled + `icon.inverse` |
| Min height | 48px (via `space-12` vertical padding + 24sp line-height) |

### Secondary Button (outlined)
| Foundation | Token |
|-----------|-------|
| Fill (rest) | transparent (no background) |
| Fill (pressed) | `bg.brand.subtle` (#FFE5F6) |
| Hover | **No hover token — text color shift only** (`text.brand` → `text.brand.hover`) |
| Border | `stroke-medium` (2px) + `stroke.brand.focus` (#D9008D — saturated) |
| Text | `type.label.lg` + `text.brand` |
| Icon (if any) | `icon.sm` (20dp) + `icon.brand` |
| Radius | `radius-large` (16px) |

### Tertiary Button (text-only)
| Foundation | Token |
|-----------|-------|
| Fill (rest) | transparent · no border |
| Fill (pressed) | `bg.brand.subtle` (#FFE5F6) — shared with Secondary |
| Text | `type.label.lg` + `text.brand` (hover → `text.brand.hover`) |
| Icon (if any) | `icon.sm` (20dp) + `icon.brand` |

### Pre-booking CTA (customer-app only)
| Foundation | Token |
|-----------|-------|
| Fill | `bg.brand.accent` (#FF66C9, Brand_400) → pressed: `bg.brand.accent.pressed` (#EA5DB8) |
| Text | `type.label.lg` + `text.onBrandAccent` (#161021 — **dark** label, not white; white fails AA on bright pink) |
| Icon (if any) | `icon.sm` (20dp) + `icon.action` (#352D42) |
| Radius | `radius-large` (16px) |
| Use | Customer-app pre-booking flows **only**. Never for generic "Book"/"Buy"/transactional CTAs. |

### Destructive Button
| Foundation | Token |
|-----------|-------|
| Fill | `bg.critical` → hover: `bg.critical.hover` → pressed: `bg.critical.pressed` |
| Text | `type.label.lg` + `text.onCritical` (#FFFFFF) |
| Icon (if any) | `icon.sm` (20dp) + `icon.inverse` |
| Focus ring | `stroke.critical.focus` (#D92130) — destructive elements get red focus, not brand pink |
| Use | Irreversible destruction only (Delete, Cancel-plan, Remove). Max 1 per screen. NEVER "Dismiss"/"Skip"/"Cancel"/"Close" — those are Secondary/Tertiary. |

### Input Field
| Foundation | Token |
|-----------|-------|
| Border (rest) | `stroke-small` (1px) + `stroke.subtle` (#D7D3E0) |
| Border (active / keyboard focus) | `stroke-medium` (2px) + `stroke.brand.focus` (#D9008D — saturated) |
| Border (error, invalid input) | `stroke-medium` (2px) + `stroke.critical.focus` (#D92130 — saturated, not the soft #FFB3B9) |
| Radius | `radius-medium` (12px) |
| Label | `type.label.lg` (16sp SemiBold) + `text.default` |
| Value | `type.body.lg` (16sp Regular) + `text.default` |
| Helper | `type.body.md` (14sp) + `text.subtle` — 12sp `body.sm` acceptable if space-constrained |
| Helper (warning state) | `type.body.md` (14sp) + `text.onWarning` (#372902) — for warning-tinted helper copy on the default input background (14.18:1 AAA) |
| Error | `type.body.md` (14sp) + `text.critical` (#D92130) |
| Required asterisk | `text.critical` |
| Padding | `space-16` horizontal, `space-12` vertical |
| Shadow | `shadow.none` |

### Bottom Sheet
| Foundation | Token |
|-----------|-------|
| Surface | `bg.default` |
| Radius | `radius-xlarge` (24px) top corners only |
| Shadow | `shadow.md` only — never `shadow.xl`, never a border (see `wiom-bottomsheet`) |
| Padding | `space-16` all sides |
| Header | `type.heading.md` (20sp Bold) + `text.default` |
| Stroke | None (shadow provides depth) |

### Checkbox / Radio
| State | Stroke | Fill |
|---|---|---|
| Unchecked (rest) | `stroke-medium` (2px) + `stroke.strong` (#473F55) | transparent |
| Checked | none (filled = no border) | `bg.brand` (#D9008D) + checkmark `icon.inverse` |
| Focused (keyboard) | `stroke.brand.focus` (#D9008D) ring **outside** | (unchanged) |
| Error (unchecked) | `stroke-medium` + `stroke.critical.focus` (#D92130) | transparent |
| **Checked + in error** | none | **`bg.critical`** (#D92130) + checkmark `text.onCritical` — fill swaps from brand to critical |
| Disabled | `stroke-medium` + `stroke.subtle` (#D7D3E0) | transparent (unchecked) · `bg.disabled` + checkmark `icon.disabled` (checked) |

Radius: checkbox `radius-tiny` (4px); radio `radius-full`. Label next to control stays `text.default` in all states — the control carries the error signal, not the label.

### Icon Badge (icon with bg)

A glyph inside a filled, circular container. Used as a leading affordance in list rows, empty-state hero, inline status indicators, and settings sections. Container is always filled + circular; glyph is always a single `Icons.Rounded.*`.

**Three sizes — glyph + padding scale proportionally with container:**

| Size | Container | Glyph | Padding | Radius |
|---|---|---|---|---|
| `sm` | 24×24 | `icon.xs` (16dp) | `space-4` (4dp) | `radius-full` |
| `md` | 48×48 | `icon.md` (24dp) | `space-12` (12dp) | `radius-full` |
| `lg` | 96×96 | `icon.lg` (48dp) | `space-24` (24dp) | `radius-full` |

**Six tones — pair container bg with matching glyph tint; never mix across tones:**

| Tone | Container bg | Glyph tint | When to use |
|---|---|---|---|
| `neutral` | `bg.subtle` (#F1EDF7) | `icon.nonAction` (#898296) | Default · decorative leading slot in list rows (row itself carries the tap target) |
| `brand` | `bg.brand.subtle` (#FFE5F6) | `icon.brand` (#D9008D) | Featured / promoted rows · brand-flagged settings entry |
| `positive` | `bg.positive.subtle` (#E1FAED) | `icon.positive` (#008043) | Success summary · confirmed / completed rows |
| `warning` | `bg.warning.subtle` (#FFE9A1) | `icon.action` (#352D42) | Warning summary · attention rows (dark glyph, 10.85:1; gold `icon.warning` is ratings-only) |
| `critical` | `bg.critical.subtle` (#FFE5E7) | `icon.critical` (#D92130) | Error summary · attention-required rows |
| `info` | `bg.info.subtle` (#F1E5FF) | `icon.info` (#6D17CE) | Informational rows · tips · helper cards |

**Rules:**
- No border, no shadow — container fill is the only visual weight (Shadow vs Border rule).
- Icon Badge is a **display element**. Tap lives on the parent row or CTA, never on the badge itself.
- Warning icons: dark `icon.action` on `bg.warning.subtle` (10.85:1) or `bg.warning` (6.60:1); gold `icon.warning` is for rating stars only (owner decision, 23 Nov 2026; same on iOS).
- Neutral tone uses `icon.nonAction` by default (decorative-inside-tappable-row). Use `icon.action` (#352D42) instead only when the badge is genuinely the sole affordance — a rare case; prefer restructuring.

---

## Anti-Pattern Checklist

Before submitting any Wiom UI code, verify NONE of these are present:

- [ ] Hardcoded hex color values (use semantic tokens)
- [ ] Hardcoded dp/sp/px values for spacing, radius, stroke, or type (use tokens)
- [ ] Border AND shadow on the same component (pick one)
- [ ] Border on a filled/checked/toggled component (filled = no border)
- [ ] Same radius on nested containers (use inner = outer − padding)
- [ ] Full-bleed divider inside a padded card (inset to match padding)
- [ ] Legacy role-first tokens (`color.brand.primary`, `color.warning.primary` #B85C00 brown, `warning.soft` #FFDA40 — all deprecated; use element-first `bg.*`/`text.*`/`stroke.*`/`icon.*`)
- [ ] `text.default` used on a warning surface (`bg.warning` or `bg.warning.subtle`) — warning body + label text is `text.onWarning` (#372902), which is also valid for warning-tinted inline text on `bg.default` (14.18:1 AAA).
- [ ] Inventing a `text.warning` token — use `text.onWarning` instead. Its dark-olive value passes AAA on white, so a separate default-surface token would be redundant. Warning is deliberately a one-token family.
- [ ] Inventing a `text.placeholder` token — use `text.disabled` (same hex #A7A1B2 covers both disabled-text and empty-input placeholders by design).
- [ ] Using stale `icon.disabled` hex #D7D3E0 — the token now resolves to `#A7A1B2` (Neutral_400, same hex as `text.disabled`). The old #D7D3E0 value was same-hex as `bg.disabled` and vanished on disabled surfaces.
- [ ] Using stale `icon.nonAction` hex #A7A1B2 — the token now resolves to `#898296` (Neutral_500). The old Neutral_400 value failed graphics AA on `bg.default` (2.39:1); Neutral_500 clears 3:1 (3.48:1).
- [ ] `text.onBrand` / `text.onCritical` used as an `Icon(tint=)` value (icons use `icon.*` tokens, even inside CTAs)
- [ ] Namespace/slot mismatch (e.g., `Text(color = bgCriticalSubtle)` — `bg.*` never valid on text)
- [ ] `Icons.Default` / `Icons.Filled` / `Icons.Outlined` / `Icons.Sharp` / `Icons.TwoTone` (use `Icons.Rounded.*` from Material 3 Icons Rounded)
- [ ] `painterResource(R.drawable.rounded_*)` for a standard Material icon (`painterResource` is only for `ic_wiom_*` and `ic_partner_*`)
- [ ] Icon sizes not in {16, 20, 24, 48}dp
- [ ] Spacing values not on the 4px grid
- [ ] Font sizes not in the 13-token type scale
- [ ] Margins on child components (use parent gap)
- [ ] Red button for non-destructive actions ("Cancel", "Skip", "No thanks")
- [ ] `meta.xs` (10sp) used for anything other than timestamps/badges
- [ ] Missing `contentDescription` on interactive icons
- [ ] Focus ring with same radius as component (should be component radius + 2px)

---

*Wiom Design System Foundations — Unified Skill.*
*References (source of truth): color.md + color/{core-tokens, actions, patterns, chat, primitives, contrast}.md · typography.md · iconography.md · spacing.md · radius.md · stroke.md · shadow.md*
*When SKILL.md and a reference file disagree, the reference file wins — SKILL.md is a quick summary, not the authoritative spec.*
