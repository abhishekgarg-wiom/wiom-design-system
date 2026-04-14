# CA Design System — Radius Foundation Guidelines

> Corner radius tokens, nesting rules, and component-to-radius mapping for the CA (Wiom) Design System.
> Platform: **Android (Jetpack Compose)** — standard circular corners, not squircle.
> Benchmarked against Material Design 3, Apple HIG, Carbon (IBM), Ant Design, Atlassian & modern SaaS (Stripe/Vercel/Linear).

---

## 1. Radius Tokens

### 7 tokens on the 4px grid

| Token | Value | Use |
|-------|-------|-----|
| `radius-none` | 0px | Sharp edges — table cells, inline badges, square thumbnails |
| `radius-tiny` | 4px | Small elements — badges, tags, tiny chips, lozenges |
| `radius-small` | 8px | Chips, snackbars, small containers |
| `radius-medium` | 12px | Input fields, inner cards, nested containers |
| `radius-large` | 16px | Buttons, cards, sheets, main containers |
| `radius-xlarge` | 24px | Dialogs, modals, bottom sheets, large surfaces |
| `radius-full` | 9999px | Avatars, pill buttons, FABs, status dots, circles |

### Migration from current tokens

| Old Token | New Token | Value | Change |
|-----------|-----------|-------|--------|
| *(none)* | `radius-none` | 0px | New |
| `radius-below-24dp` | `radius-tiny` | 4px | Renamed |
| `radius-below-40dp` | `radius-small` | 8px | Renamed |
| `radius-inner-card` | `radius-medium` | 12px | Renamed |
| `radius-main-card` | `radius-large` | 16px | Renamed (value same) |
| `radius-large` | `radius-xlarge` | 24px | Renamed |
| *(none)* | `radius-full` | 9999px | New |

### Why T-shirt naming?

The old naming (`radius-below-40dp`) described when to use the token based on component height. The new naming (`radius-small`) describes the token's position in the scale — extensible, intuitive, and consistent with M3, Atlassian, and Ant Design.

---

## 2. The 4px Grid Rule

Every radius value is a **multiple of 4px** (except 0 and full).

| Value | On grid? | Notes |
|-------|----------|-------|
| 0px | N/A | Sharp — no rounding |
| 4px | Yes | Minimum visible rounding |
| 8px | Yes | Chips, small containers |
| 12px | Yes | Input fields, nested containers |
| 16px | Yes | Buttons, cards, main containers |
| 24px | Yes | Dialogs, modals, large surfaces |
| 9999px | N/A | Full — always pill/circle regardless of size |

This matches the spacing system — radius tokens alias spacing primitives (`space/4`, `space/8`, `space/12`, `space/16`, `space/24`).

---

## 3. Radius Selection Decision Tree

```
Is the element a circle or pill shape (avatar, FAB, status dot, pill button)?
  → YES: radius-full (9999px)
  → NO: Continue ↓

Is the element sharp (table cell, square thumbnail, inline badge)?
  → YES: radius-none (0px)
  → NO: Continue ↓

Is the element a dialog, modal, or bottom sheet?
  → YES: radius-xlarge (24px)
  → NO: Continue ↓

Is the element a button, card, or main container?
  → YES: radius-large (16px)
  → NO: Continue ↓

Is the element an input field or nested inside a rounded container?
  → INPUT: radius-medium (12px)
  → NESTED: Calculate inner radius (see Section 4)
  → NO: Continue ↓

Is the element a chip, snackbar, or small container?
  → YES: radius-small (8px)
  → NO: Continue ↓

Is the element small (badge, tag, lozenge, tooltip)?
  → YES: radius-tiny (4px)
  → NO: Use radius-small (8px) as default
```

---

## 4. Nested Radius — The Formula

### The rule

> **inner-radius = outer-radius - padding**
> If padding > outer-radius, inner-radius = 0.

This ensures the gap between nested rounded containers looks even at all corners.

### How it works

```
┌─────────────────────────────────┐  ← Outer: radius-large (16px)
│  padding: 4px                   │
│  ┌─────────────────────────┐    │
│  │  Inner: 16 - 4 = 12px  │    │  ← radius-medium (12px)
│  └─────────────────────────┘    │
└─────────────────────────────────┘
```

### Common nesting pairs

| Outer | Padding | Inner (calculated) | Token pair |
|-------|---------|-------------------|------------|
| radius-large (16px) | 4px | 12px | → radius-medium |
| radius-large (16px) | 8px | 8px | → radius-small |
| radius-xlarge (24px) | 8px | 16px | → radius-large |
| radius-xlarge (24px) | 16px | 8px | → radius-small |
| radius-small (8px) | 4px | 4px | → radius-tiny |
| radius-medium (12px) | 4px | 8px | → radius-small |
| radius-medium (12px) | 8px | 4px | → radius-tiny |

All pairs land on existing tokens — no arbitrary values needed.

### Why same radius on nested containers is wrong

```
Wrong:                          Correct:
┌──────────┐                    ┌──────────┐
│ ╭──────╮ │  ← thick gap      │ ╭──────╮ │  ← even gap
│ │      │ │     at corners     │ │      │ │     everywhere
│ │      │ │  ← thin gap       │ │      │ │
│ ╰──────╯ │     at sides      │ ╰──────╯ │
└──────────┘                    └──────────┘
outer: 16    inner: 16          outer: 16    inner: 12
```

Using the same radius for both creates an uneven gap — thick at the corners, thin at the sides. Subtracting the padding produces a visually consistent gap.

### Implementation

```kotlin
// Compose
val outerRadius = 16.dp
val padding = 4.dp
val innerRadius = (outerRadius - padding).coerceAtLeast(0.dp)

Box(
    modifier = Modifier
        .clip(RoundedCornerShape(outerRadius))
        .padding(padding)
) {
    Card(
        shape = RoundedCornerShape(innerRadius)
    ) { /* content */ }
}
```

---

## 5. Component-to-Radius Map

| Component | Token | Value | Notes |
|-----------|-------|-------|-------|
| **Card (main)** | `radius-large` | 16px | Primary container |
| **Card (nested/inner)** | `radius-medium` | 12px | Inside a 16px card with 4px padding |
| **Bottom sheet** | `radius-xlarge` | 24px | Top corners only |
| **Dialog / Modal** | `radius-xlarge` | 24px | — |
| **Button** | `radius-large` | 16px | All button variants |
| **Input field** | `radius-medium` | 12px | Text inputs, search bars, dropdowns |
| **Chip** | `radius-small` | 8px | Default chips |
| **Chip (pill)** | `radius-full` | 9999px | When pill shape is needed |
| **Toggle/Switch track** | `radius-full` | 9999px | Pill-shaped track |
| **Toggle/Switch thumb** | `radius-full` | 9999px | Circle |
| **Checkbox** | `radius-tiny` | 4px | Small control, slight rounding |
| **Radio button** | `radius-full` | 9999px | Always circle |
| **Avatar** | `radius-full` | 9999px | Always circle |
| **Avatar (square)** | `radius-small` | 8px | Group/workspace avatars |
| **Badge** | `radius-tiny` | 4px | Small notification badges |
| **Badge (pill)** | `radius-full` | 9999px | Pill-shaped count badges |
| **Tag / Lozenge** | `radius-tiny` | 4px | Status tags, labels |
| **Snackbar / Toast** | `radius-small` | 8px | — |
| **Tooltip** | `radius-tiny` | 4px | — |
| **Tab indicator** | `radius-full` | 9999px | Pill-shaped active indicator |
| **FAB** | `radius-full` | 9999px | Floating action button |
| **App bar** | `radius-none` | 0px | Edge-to-edge, no rounding |
| **Table cell** | `radius-none` | 0px | Sharp edges |
| **Thumbnail (square)** | `radius-none` | 0px | Sharp edges |
| **Divider** | `radius-none` | 0px | Always sharp |
| **Progress bar track** | `radius-full` | 9999px | Pill-shaped track |
| **Slider track** | `radius-full` | 9999px | Pill-shaped track |
| **Slider thumb** | `radius-full` | 9999px | Circle |

---

## 6. Corner Type — Standard Circular

### The rule

**CA uses standard circular corners (CSS `border-radius` / Compose `RoundedCornerShape`).** Not squircle (continuous corners).

### Why not squircle?

1. **Android default** — Compose `RoundedCornerShape` uses circular arcs. Squircle requires custom `Shape` implementation.
2. **Industry alignment** — M3, Ant, Atlassian, Stripe, Linear all use circular. Only Apple uses squircle.
3. **Figma default** — Figma draws circular corners by default. Squircle requires corner smoothing > 0%.
4. **Simplicity** — One corner type across all platforms. No discrepancy between Figma and code.

### Figma corner smoothing

Keep corner smoothing at **0%** (default) for all components. Do not adjust this property.

---

## 7. Top-Corner-Only Rounding

Some components only round their top corners.

| Component | Top corners | Bottom corners |
|-----------|-------------|---------------|
| **Bottom sheet** | radius-xlarge (24px) | radius-none (0px) |
| **Bottom nav bar** | radius-none (0px) | radius-none (0px) |
| **App bar** | radius-none (0px) | radius-none (0px) |
| **Snackbar** | radius-small (8px) | radius-small (8px) |
| **Card** | All 4 corners | radius-large (16px) |

### Implementation

```kotlin
// Bottom sheet — top corners only
RoundedCornerShape(
    topStart = 24.dp,
    topEnd = 24.dp,
    bottomStart = 0.dp,
    bottomEnd = 0.dp
)
```

---

## 8. Anti-Patterns — What NOT to Do

### 1. Same radius on nested containers
Outer 16px, inner 16px with padding between them. **Use the nested formula: inner = outer - padding.**

### 2. radius-full on non-square elements
Using `9999px` or `50%` on a rectangle produces an oval, not a circle. **Only use radius-full on elements that are square or pill-shaped (height-driven).**

### 3. Mixing sharp and rounded siblings
Card A has `radius-large`, Card B next to it has `radius-none`. **All sibling components at the same level should use the same radius token.**

### 4. Hardcoded radius values
Using raw `12` or `16` in code instead of the token. **Always reference the token — it ensures consistency and enables theming.**

### 5. Inconsistent radius on similar components
Buttons at 8px, but chips at 12px, then another button at 4px. **Semantically similar components must share the same radius token.**

### 6. Radius on edge-to-edge elements
Adding radius to app bars, tab bars, or elements that extend to screen edges. **Edge-to-edge components use radius-none (0px).**

### 7. Corner smoothing in Figma
Setting corner smoothing to 60% on some components and 0% on others. **Keep corner smoothing at 0% everywhere — we use standard circular, not squircle.**

### 8. Not accounting for stroke width
A component with `radius-small` (8px) and 2px stroke — the visible inner radius is effectively 6px. **The radius token defines the outer edge. Stroke sits inside, reducing the visible inner rounding.**

---

## 9. Industry Benchmark

### Token Count

| System | Token count | Values |
|--------|-------------|--------|
| **CA (Wiom)** | **7** | 0, 4, 8, 12, 16, 24, full |
| Material 3 | 7 | 0, 4, 8, 12, 16, 28, full |
| Atlassian | 7 | 2, 3, 4, 8, 12, 16, full |
| Ant Design | 5 | 2, 4, 6, 8, 8 |
| Carbon (IBM) | ~2 | 0, pill (enterprise — mostly sharp) |
| Stripe | 1 | Single global variable |
| Linear | ~3 | 6, 8, 10 (not tokenized) |

### Feature Comparison

| Feature | CA | M3 | Ant | Atlassian | Carbon |
|---------|-----|-----|-----|-----------|--------|
| On 4px grid | Yes | Yes | No (6px) | No (3px) | N/A |
| T-shirt naming | Yes | Yes | Yes | Yes | No |
| Full/pill token | Yes | Yes | No | Yes | No |
| None/sharp token | Yes | Yes | No | No | Default |
| Nested radius formula | Documented | Implicit | No | Focus ring only | No |
| Corner type | Circular | Circular | Circular | Circular | Circular |
| Aliased to spacing | Yes | No | No | No | No |

### Where CA leads

1. **Nested radius fully documented** — most systems leave it to developers
2. **Aliased to spacing primitives** — radius and spacing share the same math
3. **All on 4px grid** — cleaner than Atlassian (3px) or Ant (6px)

---

## 10. Quick Reference Card

```
RADIUS SCALE
none: 0px    tiny: 4px    small: 8px    medium: 12px
large: 16px    xlarge: 24px    full: 9999px

DECISION SHORTCUT
Circle/pill? → full
Sharp/table? → none
Large surface? → xlarge
Dialog/modal? → xlarge
Button/card? → large
Input/nested? → medium
Chip/snackbar? → small
Badge/tag? → tiny

NESTED FORMULA
inner = outer - padding
If padding > outer → inner = 0

COMMON PAIRS
16px outer + 4px pad = 12px inner (large → medium)
16px outer + 8px pad = 8px inner (large → small)
24px outer + 8px pad = 16px inner (xlarge → large)

CORNER TYPE
Standard circular. Not squircle.
Figma corner smoothing: 0%.

TOP-ONLY ROUNDING
Bottom sheets: 24px top, 0px bottom
Everything else: all 4 corners same

THE RULE
Every radius value comes from the token scale.
If it's not in the 7 tokens, don't use it.
```

---

*Generated April 2026 — Benchmarked against Material 3, Apple HIG, Carbon (IBM), Ant Design, Atlassian, Stripe, Vercel & Linear.*
*All radius values on the 4px grid. Aliased to spacing primitives.*
