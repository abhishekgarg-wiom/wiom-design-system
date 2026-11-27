---
name: wiom-compose-layout-rules
description: >
  Structural and anti-hallucination rules for Jetpack Compose UI written against Wiom
  foundations and component skills. Use EVERY TIME you write, edit, review, or generate
  Kotlin / Compose UI code for the Wiom Partner or Customer app. Trigger on Modifier,
  fillMaxWidth, fillMaxHeight, widthIn, heightIn, width(, height(, offset, Column, Row,
  Box, LazyColumn, LazyRow, Arrangement, Alignment, Spacer, padding(, margin,
  Icons.Default, Icons.Filled, Icons.Outlined, Icons.Sharp, Icons.TwoTone, ImageVector,
  painterResource, Canvas, or any layout scaffolding for a Wiom screen, component,
  theme, or widget. Pairs with wiom-design-foundations (tokens) and component skills
  under skills/components/wiom-* (anatomy).
---

# Wiom Compose Layout Rules

Structural and anti-hallucination rules for Jetpack Compose UI that consumes Wiom
foundations. Values and tokens live in foundations; component anatomy lives in
component skills; this file governs **how the Compose scaffolding is shaped**.

---

## Relationship to other skills

| You need… | Go to |
|---|---|
| Token values (color, type, spacing, radius, stroke, shadow) | `wiom-design-foundations` |
| Component anatomy, variants, states, and per-surface gutters | `skills/components/wiom-*` |
| How to lay the code out on the canvas of a screen | **this file** |

Never duplicate token values in this file. Never duplicate anatomy from component
skills. If a rule here conflicts with a component skill, the component skill wins —
tell the user and update this file.

---

## 1. Width

- **Default for content containers:** `Modifier.fillMaxWidth()`.
- The **horizontal inset from the screen edge** is the parent's `.padding(...)` using
  a spacing token — never a raw `dp` literal. Screen-level and nested-inset values
  live in `wiom-design-foundations`.
- Use `Modifier.width(...)` **only** for dimensional assets (icons, avatars, logos,
  illustrations) or a deliberate split (e.g., 50/50 columns) — not for general
  content frames.
- Never use `Modifier.widthIn(min = ..., max = ...)` without a named reason
  (grid parity, marquee cap, etc.). Put the reason in a one-line comment.

## 2. Height

- **Default:** let content drive height. Do not set `Modifier.height(...)` on
  content frames.
- Use `Modifier.heightIn(min = ..., max = ...)` **only** with a clear reason
  (row parity in a grid, scrollable area with a max cap). One-line comment stating
  why.
- Buttons and other tappable chrome reach their required height via token padding
  + line-height — **do not** set an explicit height on a CTA, chip, list row, etc.
- Fixed height is fine on icons, avatars, logos, illustrations. On any frame that
  contains text, a fixed height is a red flag — flag or ask.

## 3. Child sizing in Column / Row / Box

- **Horizontal default for children:** `Modifier.fillMaxWidth()`, or
  `Modifier.weight(...)` for stretching inside a `Row`.
- **Vertical default for children:** no explicit height — let content decide.
- **Wrap** content-sized elements (chips, tags, inline labels) by simply omitting
  the fill modifier.
- **Fixed** is valid for dimensional assets and deliberate splits. When fixed
  isn't obviously justified → stop and ask, don't guess.

## 4. Layout structure

- Use `Column`, `Row`, `Box`, `LazyColumn`, `LazyRow` with
  `Arrangement.spacedBy(tokenGap)` for inter-child gaps.
- **No margins on children.** Parents own the gap; children own their own padding.
  Compose doesn't have a margin concept — don't simulate one by putting leading
  padding on every child.
- **No `Modifier.offset(...)` or absolute positioning for layout.** `offset` is for
  animation and decorative nudges only.
- Prefer `Spacer` only when `Arrangement.spacedBy` cannot express the gap (e.g.,
  one-off divider between two siblings in an otherwise wrapping layout). Don't use
  `Spacer` as the default gap mechanism.
- Constraint-style workarounds (ConstraintLayout) are allowed only when the layout
  genuinely cannot be expressed with nested Column/Row/Box. Default to the linear
  layouts.

### Screen scaffold

Every screen starts from the same frame: `Scaffold` with `wiom-top-bar` on top, one scrolling content column (16 dp gutter, `space-16` between cards, `space-32` between sections), an optional sticky CTA bar (`wiom-cta`) at the bottom, and `wiom-toast`'s host above it. Handle insets once: `enableEdgeToEdge`, `Scaffold` padding, `imePadding()` so inputs and the CTA bar stay above the keyboard. Full-page loading, empty and error use `wiom-page-states`.

## 5. Icons and graphics

- **Production icons:** `painterResource(R.drawable.ic_<name>_rounded)` — the Wiom
  app bundles vector drawables that mirror the Material Symbols **Rounded** set,
  and code references those drawables. Do **not** import
  `androidx.compose.material.icons.*` directly in app code; the Material variants
  (`Icons.Default`, `Icons.Filled`, `Icons.Outlined`, `Icons.Sharp`, `Icons.TwoTone`)
  are banned, and pulling `Icons.Rounded.*` from `material-icons-extended` is
  discouraged because the dependency is heavy.
- **Wiom / partner glyphs:** `painterResource(R.drawable.ic_wiom_*)` or
  `painterResource(R.drawable.ic_partner_*)` — **only**.
- If the required icon is not in the bundled `ic_*_rounded` set nor in the Wiom /
  partner drawables, **stop and flag**. Never substitute a similar-looking icon,
  never inline SVG, never hand-draw with `Canvas`.
- **Icon tints** use `icon.*` tokens — never `text.on*`. (Sizes and touch-target
  rules live in `wiom-design-foundations` → iconography.)

## 6. Tokens only

Every color, typography, spacing, radius, stroke, and shadow value in Compose code
resolves to a Wiom token. No raw hex, no raw `sp` / `dp`, no arbitrary numbers.
Full token sets and namespace-to-slot mapping → `wiom-design-foundations`.

If a value genuinely cannot be expressed as a token, the token is missing — flag
the gap and update foundations. Do not inline the raw value as a workaround.

## 7. Variants in Compose

A **variant** is a parameter on the composable (state, size, emphasis, hasIcon,
etc.).

- **State / color / emphasis parameters must not change the component's
  dimensions.** A pressed button is the same size as a default button. A critical
  chip is the same size as a neutral chip.
- **Only a `size` parameter** (e.g., `Size.Small` / `Size.Medium`) may change
  dimensions — and only by swapping the token set the component reads from, not
  by hardcoding a new value.
- Don't introduce a new variant when an existing one covers the case. If the
  component skill lists five states, your composable exposes those five — no more.

## 8. Pre-publish / pre-commit checklist

Before publishing a component or submitting a PR:

- [ ] No `Modifier.width(...)` or `Modifier.height(...)` without a dimensional-
      asset justification or a named reason.
- [ ] No `widthIn` / `heightIn` without a comment stating why.
- [ ] Every fixed dimension has a clear reason. Unclear cases are flagged or asked.
- [ ] All icons use `Icons.Rounded.*` or `painterResource(R.drawable.ic_wiom_*)` /
      `ic_partner_*` — nothing else.
- [ ] Spot-check: no raw hex, no raw `sp` / `dp`, no magic numbers.
- [ ] Spacing comes from tokens via `Arrangement.spacedBy(...)` and
      `.padding(tokenValue)` — no child margins, no `Spacer` used as a default gap.
- [ ] Variants (parameters) don't change dimensions unless the parameter is `size`.
- [ ] No `Modifier.offset(...)` used for layout.
- [ ] Component behaves correctly with content in Hindi + English (line-height
      and wrap behavior verified for both scripts).

## 9. When unclear, ask

Any of the following → surface them, don't silently assume:

- A fixed `width(...)` or `height(...)` on a content frame.
- A non-standard width (e.g., 87% of parent) that doesn't map to `fillMaxWidth()`,
  `weight`, or a token.
- A `widthIn` / `heightIn` whose rationale you can't state in one sentence.
- A required icon that isn't in `Icons.Rounded.*` or Wiom/partner drawables.
- A value (color, sp, dp, radius, stroke) that doesn't map to a foundations token.
- Two component-skill variants that appear to conflict.
- A new variant, state, or size not already documented in the component skill.

---

## Quick-reference anti-patterns

| ❌ Don't | ✅ Do |
|---|---|
| `Modifier.width(320.dp)` on a card | `Modifier.fillMaxWidth()` with parent padding from a spacing token |
| `Modifier.height(48.dp)` on a button | Let padding + line-height from the CTA skill drive the height |
| `Modifier.padding(start = 16.dp)` on every child of a Column | `Column(verticalArrangement = Arrangement.spacedBy(Wiom.space.s16))` and parent owns horizontal padding |
| `Icons.Default.Check` or `Icons.Rounded.Check` directly | `Icon(painter = painterResource(R.drawable.ic_check_rounded), …)` |
| `color = Color(0xFFD9008D)` | `color = Wiom.bg.brand` |
| `fontSize = 16.sp` | `style = Wiom.type.label.lg` |
| `Modifier.offset(x = 8.dp)` to nudge layout | Re-parent, or use a proper `Row`/`Box` with alignment |
| `Spacer(Modifier.height(16.dp))` as the default gap | `Arrangement.spacedBy(WiomSpacing.m)` on the parent |

---

## Scope

This skill governs **Compose UI code for the Wiom Partner and Customer Android
apps**. It does not cover:

- Non-UI Kotlin (networking, DI, data layer) — out of scope.
- Design-time Figma rules — see `wiom-figma-design-rules`.
- Token values themselves — see `wiom-design-foundations`.
- Component anatomy (what a CTA, list item, or bottom sheet looks like) — see
  `skills/components/wiom-*`.
