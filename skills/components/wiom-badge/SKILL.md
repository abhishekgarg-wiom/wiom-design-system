---
name: wiom-badge
description: >
  Wiom Badge system — passive status indicators. 3 types (Dot / Count /
  Label), 24 ship variants. Reference for generating Jetpack Compose
  code with WiomBadgeDot / WiomBadgeCount / WiomBadgeLabel and for
  instancing the existing Figma Badge component sets via MCP. Use this
  skill EVERY TIME you create, edit, or review any screen, card, list
  item, tab, or header containing a status indicator, count, tag, or
  label for the Wiom Partner or Customer Android app, or when assembling
  Figma frames with badges. Trigger on: Badge, Tag, Label, Status,
  Count, Dot, Indicator, Chip (status), pill, instance of 1499:953 /
  1499:957 / 1499:962, "5+", "9+", पूरा हुआ, असफल, पक्का.
---

# Wiom Badge System

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), badge
component sets `1499:953` (Dot), `1499:957` (Count), `1499:962` (Label),
and the `wiom-design-foundations` token set.

**3 types · 24 ship variants · element-first tokens.**
| Type | Count | Variants |
|---|---|---|
| Dot | 3 | Color: Brand · Critical · Neutral |
| Count | 3 | Color: Brand · Critical · Neutral |
| Label | 18 | Size (Default / Small) × Color (6) × Style (Filled / Tinted) — Small ships Tinted only |

Badges are **passive** — never tappable, never interactive. If it needs
a tap target, it's a Chip (`wiom-tabs-filters` §3), not a badge.

---

## 1. Figma — instancing the components

Three separate component sets. Strings are **case-sensitive**.

### Badge / Dot — `1499:953`

| Property | Type | Default | Accepted values |
|---|---|---|---|
| `Color` | Variant | `Brand` | `Brand` · `Critical` · `Neutral` |

### Badge / Count — `1499:957`

| Property | Type | Default | Accepted values |
|---|---|---|---|
| `Color` | Variant | `Brand` | `Brand` · `Critical` · `Neutral` |

> The number rendered inside the count is set on the inner Text node
> after instancing — it's not a variant property. Cap the displayed
> value at `9+` for single-digit max or `99+` for double-digit max.

### Badge / Label — `1499:962`

| Property | Type | Default | Accepted values |
|---|---|---|---|
| `Size` | Variant | `Default` | `Default` (28 dp) · `Small` (24 dp) |
| `Color` | Variant | `Brand` | `Brand` · `Positive` · `Warning` · `Critical` · `Info` · `Neutral` |
| `Style` | Variant | `Filled` | `Filled` · `Tinted` — **Small × Filled does not ship** (use Default for Filled) |

Badges hug their content. The component itself does not own surface
margin — its parent (a Row inside a list item, an icon's Box, a card
header) places it.

### Code enum ↔ Figma variant string

| `WiomBadgeColor` (Kotlin) | Figma `Color` value |
|---|---|
| `Brand` | `Brand` |
| `Positive` | `Positive` (Label only) |
| `Warning` | `Warning` (Label only) |
| `Critical` | `Critical` |
| `Info` | `Info` (Label only) |
| `Neutral` | `Neutral` |

| `WiomBadgeSize` (Kotlin, Label only) | Figma `Size` value |
|---|---|
| `Default` | `Default` |
| `Small` | `Small` |

| `WiomBadgeStyle` (Kotlin, Label only) | Figma `Style` value |
|---|---|
| `Filled` | `Filled` (Default only) |
| `Tinted` | `Tinted` (Default + Small) |

---

## 2. Anatomy

### Dot

| Slot | Value | Token |
|---|---|---|
| Container size | 8 × 8 dp | — |
| Container radius | full pill | `radius-full` |
| No content | — | — |

### Count

| Slot | Value | Token |
|---|---|---|
| Container width | Hugs content; 20 dp min, grows when "9+" / "99+" | — |
| Container height | 20 dp | — |
| Container radius | full pill | `radius-full` |
| Horizontal padding | 4 dp | `space-4` |
| Number style | Noto Sans Regular 10 / 16 LH | `type.meta.xs` |
| Inner row | Centered both axes | — |

### Label

| Slot | Default (28 dp) | Small (24 dp) | Token |
|---|---|---|---|
| Container width | Hugs content | Hugs content | — |
| Container height | Derived (4 V + 20 LH + 4 V = 28) | Derived (4 V + 16 LH + 4 V = 24) | — |
| Container radius | 4 dp | 4 dp | `radius-tiny` |
| Horizontal padding | 12 dp | 8 dp | `space-12` / `space-8` |
| Vertical padding | 4 dp | 4 dp | `space-4` |
| Label style | Noto Sans SemiBold 14 / 20 LH | Noto Sans SemiBold 12 / 16 LH | `type.label.md` / `type.label.sm` |
| Inner row | Centered both axes | Centered both axes | — |

**No fixed dimensions** — every container hugs its content. Don't set
`Modifier.width(...)`, `Modifier.height(...)`, `heightIn`, `widthIn`, or
`Modifier.size(...)` on a Label badge. Dot is the one exception (fixed
8 × 8 dp because it has no content to derive from).

---

## 3. The 3 types

| Type | Purpose | Where it sits | Don't use for | Sample copy / value |
|---|---|---|---|---|
| **Dot** | "Something here, no detail needed" — unread, active, new | **Overlay only** — top-right of an icon or avatar (Notification bell, profile picture) | Numeric counts (use Count); explanatory state (use Label); standalone display | (no content) |
| **Count** | Numeric quantity attached to an icon, tab, or list item | **Overlay** on icons (bell with "5"); **inline** on tabs ("टिकट 9+"); never standalone | "0" — hide the badge entirely; non-numeric content (use Label); status / state | "5", "9+", "99+" |
| **Label** | Text status / state / role | **Inline** in list rows, cards, headers, tab labels | Things the user can act on (use Chip / CTA); long copy that wraps; multiple-state stacks | "पूरा हुआ", "असफल", "पक्का", "एडमिन", "Confirmed", "Failed" |

---

## 4. Tokens per type × variant

All tokens are **element-first** (`bg.*` / `text.*`). Always reference
tokens, never hex.

### 4.1 Dot

| Color | Container fill |
|---|---|
| Brand | `bg.brand` |
| Critical | `bg.critical` |
| Neutral | `bg.muted` |

### 4.2 Count

| Color | Container fill | Number color |
|---|---|---|
| Brand | `bg.brand` | `text.onBrand` |
| Critical | `bg.critical` | `text.onCritical` |
| Neutral | `bg.muted` | `text.subtle` |

### 4.3 Label — Filled (Default size only)

| Color | Container fill | Label color |
|---|---|---|
| Brand | `bg.brand` | `text.onBrand` |
| Positive | `bg.positive` | `text.onPositive` |
| Warning | `bg.warning` (gold) | `text.onWarning` (#372902 olive) |
| Critical | `bg.critical` | `text.onCritical` |
| Info | `bg.info` | `text.onInfo` |
| Neutral | `bg.muted` | `text.subtle` |

> **Warning is special.** White-on-gold (`bg.warning` #E2B203) fails
> AA contrast — Warning Filled uses dark olive `text.onWarning`
> (#372902), not white. This is a foundations rule, not a badge rule.

### 4.4 Label — Tinted (Default + Small)

| Color | Container fill | Label color |
|---|---|---|
| Brand | `bg.brand.subtle` | `text.brand` |
| Positive | `bg.positive.subtle` | `text.positive` |
| Warning | `bg.warning.subtle` | `text.onWarning` (same token as Filled — see §4.3) |
| Critical | `bg.critical.subtle` | `text.critical` |
| Info | `bg.info.subtle` | `text.info` |
| Neutral | `bg.subtle` | `text.subtle` |

> Note that Tinted Neutral uses `bg.subtle` (#F1EDF7), not `bg.muted`
> (#E8E4F0) — the lighter of the two greys, so it reads as recessed
> rather than active. Filled Neutral uses `bg.muted`.

---

## 5. Placement

| Type | Position | Anchoring |
|---|---|---|
| Dot | **Overlay** — top-right corner of icon / avatar | `Box` parent, child aligned `TopEnd`. **No offset** — at 8 dp the dot fits naturally in the corner of a 24 dp icon. |
| Count | **Overlay** on icons (bell + "5") OR **inline** on tab labels ("टिकट 5") | Overlay → `align(Alignment.TopEnd)` + `offset(x = 4.dp, y = (-4).dp)` to nudge the 20 dp circle into the icon's corner. Inline → in a `Row` after the tab label, separated by `space-8`. |
| Label | **Inline** in a list row, card, or header | In a `Row` next to other content, gap `space-8` to neighbour text. Hugs content; doesn't fill |

**Single badge per row.** Never stack multiple badges on the same line
item — pick the most informative one. If a row genuinely has two
distinct status dimensions, that's a sign the row is doing too much;
split it into two rows or move one dimension into a sub-line of body
text.

---

## 6. Universal rules

1. **Badges are passive** — never tappable, never wrapped in
   `Modifier.clickable`. If a tap target is needed → use Chip
   (`wiom-tabs-filters` §3).
2. **No icons inside badges.** Color + text is sufficient. If you
   need icon + text + color, that's a Status Indicator pattern (a
   separate component, not in this skill).
3. **No fixed dimensions on Label badges.** Width and height emerge
   from padding + content. Dot is the exception — fixed 8 × 8 dp.
4. **Small ships Tinted only.** If the badge is important enough to
   be Filled, use Default size. There is no `Small × Filled` Figma
   variant.
5. **Hide Count at zero.** A `0` count means "no badge at all."
   Never render "0".
6. **Cap Count display.** `1`–`9` render as the digit; `10+` render
   as `9+`; `100+` render as `99+`. Never show raw 3-digit counts.
7. **Text never wraps.** Single line only — `maxLines = 1`,
   `overflow = TextOverflow.Visible` (the badge hugs content; if it
   doesn't fit on the row, rewrite copy shorter).
8. **One badge per row.** No stacking.
9. **Color matches status family.** Don't put a Positive badge on a
   row whose other signals are Critical. Status families are
   exclusive — pick one.
10. **Tokens only — no raw hex.** Element-first only (`bg.*` /
    `text.*`). No legacy `color.brand.primary` etc.
11. **Dot is overlay-only.** Never inline a Dot in a row. If you need
    a tiny inline indicator with text, use a Small Tinted Label.

---

## 7. Decision logic

```
Step 1 — Type
  Just "something new / unread", no number or text?
    → Dot

  A numeric quantity (notifications, unread count, tab counter)?
    → Count

  A text status / state / role?
    → Label  (continue to Step 2)

Step 2 — Label size
  "If I remove this badge, does the row still serve its purpose?"
  YES — badge supports other content (amount + 'पक्का', name + 'एडमिन', tab + count)
    → Small (24 dp)
  NO — badge IS the information (transaction outcome, slot status, device state)
    → Default (28 dp)

Step 3 — Label style (Default size only)
  "Is this state final?"
  YES — terminal: completed, failed, confirmed, expired, revoked, deployed
    → Filled
  NO — transitional: processing, provisional, pending, active, eligible
    → Tinted
  (Small skips this step — Tinted only.)

Step 4 — Color
  Success / confirmed / complete / eligible    → Positive (green)
  Warning / pending / provisional / attention  → Warning (gold)
  Error / failed / expired / revoked           → Critical (red)
  Active / in-progress / informational         → Info (purple)
  Brand-tied state ("Wiom-managed", brand pre-booking marker) → Brand (pink)
  Default / proposed / inactive / muted        → Neutral (grey)
```

---

## 8. Compose API + code

### Public API

```kotlin
enum class WiomBadgeColor { Brand, Positive, Warning, Critical, Info, Neutral }
enum class WiomBadgeSize  { Default, Small }
enum class WiomBadgeStyle { Filled, Tinted }
// Compile-time guarantee: Small + Filled doesn't ship. WiomBadgeLabel rejects
// (size = Small, style = Filled) at runtime via require(); see resolver.

@Composable
fun WiomBadgeDot(
    color: WiomBadgeColor = WiomBadgeColor.Brand,    // Brand | Critical | Neutral only
    modifier: Modifier = Modifier,
)

@Composable
fun WiomBadgeCount(
    count: Int,                                       // hidden when count == 0
    color: WiomBadgeColor = WiomBadgeColor.Brand,    // Brand | Critical | Neutral only
    maxOneDigit: Boolean = false,                     // true → "9+" cap; false → "99+" cap
    modifier: Modifier = Modifier,
)

@Composable
fun WiomBadgeLabel(
    text: String,
    color: WiomBadgeColor = WiomBadgeColor.Neutral,  // any of 6
    size: WiomBadgeSize = WiomBadgeSize.Default,
    style: WiomBadgeStyle = WiomBadgeStyle.Tinted,   // Small forces Tinted; runtime require()
    modifier: Modifier = Modifier,
)
```

### Token resolver (drives container fill + label colour)

```kotlin
@Composable
private fun rememberLabelBadgeColors(
    color: WiomBadgeColor,
    style: WiomBadgeStyle,
): BadgeColors = when (style) {
    WiomBadgeStyle.Filled -> when (color) {
        WiomBadgeColor.Brand    -> BadgeColors(Wiom.bg.brand,    Wiom.text.onBrand)
        WiomBadgeColor.Positive -> BadgeColors(Wiom.bg.positive, Wiom.text.onPositive)
        WiomBadgeColor.Warning  -> BadgeColors(Wiom.bg.warning,  Wiom.text.onWarning)   // dark olive on gold
        WiomBadgeColor.Critical -> BadgeColors(Wiom.bg.critical, Wiom.text.onCritical)
        WiomBadgeColor.Info     -> BadgeColors(Wiom.bg.info,     Wiom.text.onInfo)
        WiomBadgeColor.Neutral  -> BadgeColors(Wiom.bg.muted,    Wiom.text.subtle)
    }
    WiomBadgeStyle.Tinted -> when (color) {
        WiomBadgeColor.Brand    -> BadgeColors(Wiom.bg.brand.subtle,    Wiom.text.brand)
        WiomBadgeColor.Positive -> BadgeColors(Wiom.bg.positive.subtle, Wiom.text.positive)
        WiomBadgeColor.Warning  -> BadgeColors(Wiom.bg.warning.subtle,  Wiom.text.onWarning) // no text.warning token
        WiomBadgeColor.Critical -> BadgeColors(Wiom.bg.critical.subtle, Wiom.text.critical)
        WiomBadgeColor.Info     -> BadgeColors(Wiom.bg.info.subtle,     Wiom.text.info)
        WiomBadgeColor.Neutral  -> BadgeColors(Wiom.bg.subtle,           Wiom.text.subtle) // bg.subtle, not bg.muted
    }
}

@Composable
private fun rememberDotOrCountFill(color: WiomBadgeColor): Color = when (color) {
    WiomBadgeColor.Brand    -> Wiom.bg.brand
    WiomBadgeColor.Critical -> Wiom.bg.critical
    WiomBadgeColor.Neutral  -> Wiom.bg.muted
    else -> error("Dot / Count support Brand · Critical · Neutral only — not $color")
}

@Composable
private fun rememberCountTextColor(color: WiomBadgeColor): Color = when (color) {
    WiomBadgeColor.Brand    -> Wiom.text.onBrand
    WiomBadgeColor.Critical -> Wiom.text.onCritical
    WiomBadgeColor.Neutral  -> Wiom.text.subtle
    else -> error("Count supports Brand · Critical · Neutral only — not $color")
}

private data class BadgeColors(val container: Color, val label: Color)
```

### Implementation

```kotlin
// ── Dot ──────────────────────────────────────────────────────────
@Composable
fun WiomBadgeDot(color: WiomBadgeColor, modifier: Modifier) {
    Box(
        modifier = modifier
            .size(8.dp)                                           // dot has no content; fixed
            .clip(CircleShape)
            .background(rememberDotOrCountFill(color)),
    )
}

// ── Count ────────────────────────────────────────────────────────
@Composable
fun WiomBadgeCount(
    count: Int,
    color: WiomBadgeColor,
    maxOneDigit: Boolean,
    modifier: Modifier,
) {
    if (count <= 0) return                                        // hide at zero
    val display = when {
        maxOneDigit && count > 9   -> "9+"
        !maxOneDigit && count > 99 -> "99+"
        else                       -> count.toString()
    }
    Box(
        modifier = modifier
            .defaultMinSize(minWidth = 20.dp, minHeight = 20.dp)  // 20×20 circle for 1-digit; grows for "9+"/"99+"
            .clip(RoundedCornerShape(Wiom.radius.full))
            .background(rememberDotOrCountFill(color))
            .padding(horizontal = Wiom.space.s4),
        contentAlignment = Alignment.Center,
    ) {
        Text(
            text = display,
            style = Wiom.type.meta.xs,
            color = rememberCountTextColor(color),
            maxLines = 1,
        )
    }
}

// ── Label ────────────────────────────────────────────────────────
@Composable
fun WiomBadgeLabel(
    text: String,
    color: WiomBadgeColor,
    size: WiomBadgeSize,
    style: WiomBadgeStyle,
    modifier: Modifier,
) {
    require(!(size == WiomBadgeSize.Small && style == WiomBadgeStyle.Filled)) {
        "WiomBadgeLabel: Small + Filled is not a ship variant. Use Default for Filled."
    }
    val colors = rememberLabelBadgeColors(color, style)
    val (hPad, textStyle) = when (size) {
        WiomBadgeSize.Default -> Wiom.space.s12 to Wiom.type.label.md
        WiomBadgeSize.Small   -> Wiom.space.s8  to Wiom.type.label.sm
    }
    Box(
        modifier = modifier
            .clip(RoundedCornerShape(Wiom.radius.tiny))           // 4dp
            .background(colors.container)
            .padding(horizontal = hPad, vertical = Wiom.space.s4),
        contentAlignment = Alignment.Center,
    ) {
        Text(
            text = text,
            style = textStyle,
            color = colors.label,
            maxLines = 1,
        )
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Small + Filled (doesn't ship), raw hex, badge wrapped in clickable
Box(
    modifier = Modifier
        .clickable { /* ... */ }                          // badges are passive
        .background(Color(0xFFB85C00)),                   // hex; Warning is gold not orange now
) {
    WiomBadgeLabel(
        text = "Pending",
        size = WiomBadgeSize.Small,
        style = WiomBadgeStyle.Filled,                    // crashes via require()
        color = WiomBadgeColor.Warning,
    )
}

// ✅ RIGHT — Tinted Small Warning, no clickable, tokens only
WiomBadgeLabel(
    text = "Pending",
    size = WiomBadgeSize.Small,
    style = WiomBadgeStyle.Tinted,                        // Small ships Tinted only
    color = WiomBadgeColor.Warning,
)
```

---

## 9. Screen-level recipes

### Notification bell with Count overlay

```kotlin
Box {
    Icon(
        imageVector = Icons.Rounded.Notifications,
        contentDescription = "सूचनाएं, $count नई",
        tint = Wiom.icon.action,
        modifier = Modifier.size(24.dp),
    )
    WiomBadgeCount(
        count = count,
        color = WiomBadgeColor.Critical,
        maxOneDigit = true,                               // "9+" cap
        modifier = Modifier
            .align(Alignment.TopEnd)
            .offset(x = 4.dp, y = (-4).dp),               // nudge into the icon's corner
    )
}
```

### Transaction list row (Default Filled, badge IS the information)

```kotlin
Row(
    verticalAlignment = Alignment.CenterVertically,
    horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
    modifier = Modifier.fillMaxWidth(),
) {
    Column(modifier = Modifier.weight(1f)) {
        Text("निकासी — ₹2,000 → SBI", style = Wiom.type.label.md, color = Wiom.text.default)
        Text("12 अप्रैल, 2026",       style = Wiom.type.body.sm, color = Wiom.text.subtle)
    }
    WiomBadgeLabel(
        text = "असफल",
        color = WiomBadgeColor.Critical,
        size = WiomBadgeSize.Default,
        style = WiomBadgeStyle.Filled,
    )
}
```

### Wallet line with Small Tinted (badge supports the amount)

```kotlin
Row(
    verticalAlignment = Alignment.CenterVertically,
    horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
) {
    Text("सेटलमेंट",  style = Wiom.type.body.md, color = Wiom.text.subtle)
    Text("₹8,200",   style = Wiom.type.label.md, color = Wiom.text.default)
    WiomBadgeLabel(
        text = "पक्का",
        color = WiomBadgeColor.Positive,
        size = WiomBadgeSize.Small,
        style = WiomBadgeStyle.Tinted,
    )
}
```

---

## 10. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are the few extras
that aren't already a numbered rule.)

- ❌ Generic status copy ("Status", "State", "Active"). Use specific
  Hindi/English: "पूरा हुआ" / "असफल" / "प्रोसेसिंग" / "Confirmed".
- ❌ Multi-line badge text. Single line, `maxLines = 1`. If it doesn't
  fit, rewrite shorter — never wrap.
- ❌ Using `text.warning` — that token doesn't exist. Warning text is
  `text.onWarning` in both Filled and Tinted (per §4.3).
- ❌ Filled Neutral with `bg.subtle` (or Tinted Neutral with `bg.muted`)
  — they're two distinct grey washes, easy to swap by mistake.
- ❌ Counting a `Status Indicator` (icon + text + color) as a badge.
  That's a separate pattern; badges have no icon.

---

## 11. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Badges canvas: `1499:951`. Re-pull
`get_variable_defs` on `1499:952` if foundations token names ever
change — those are the only values that should drift.

| Node | Component / variant |
|---|---|
| `1499:953` | Badge / Dot component set — 3 colors |
| `1499:954` · `1499:955` · `1499:956` | Dot Brand · Critical · Neutral |
| `1499:957` | Badge / Count component set — 3 colors |
| `1499:958` · `1499:960` · `2941:11409` | Count Brand · Critical · Neutral |
| `1499:962` | Badge / Label component set — 18 ship variants |
| `3104:7296` · `1499:963` · `1499:967` · `1499:971` · `1499:975` · `1499:979` | Default Filled — Brand · Positive · Warning · Critical · Info · Neutral |
| `1499:965` · `3104:7298` · `1499:969` · `1499:973` · `1499:977` · `1499:981` | Default Tinted — Brand · Positive · Warning · Critical · Info · Neutral |
| `1499:983` · `3104:7300` · `1499:985` · `1499:987` · `1499:989` · `1499:991` | Small Tinted — Brand · Positive · Warning · Critical · Info · Neutral |

**Description-vs-ship discrepancies** — if a stale Figma description or
old skill says otherwise, trust ship values:

- **24 ship variants total** (3 Dot + 3 Count + 18 Label), not the
  20 that earlier docs implied.
- **Count ships 3 colors**, not 2 (Neutral was added).
- **Count is 20 dp tall with `space-4` horizontal padding** —
  earlier docs claimed 18 dp / 5 px.
- **Label ships 6 colors**, not 5 (Brand was added).
- **`Critical`, not `Negative`** — naming aligned with element-first
  foundations across the file.
- **Warning is gold, not orange.** `bg.warning` is #E2B203 with
  `text.onWarning` #372902 olive. White-on-gold fails AA.
- **No `text.warning` token exists.** Warning Tinted text uses
  `text.onWarning` (the same olive that works on gold also works on
  the subtle wash and on `bg.default`).
- **Filled Neutral fill is `bg.muted`; Tinted Neutral fill is
  `bg.subtle`** — two distinct grey washes.

---

## See also

- `wiom-design-foundations` — token source (`bg.*`, `text.*`,
  `space-*`, `radius-*`, `type.*`). Especially the warning-family
  rule and the element-first contrast pairs.
- `wiom-tabs-filters` — Chip component for interactive (tappable)
  filtering; Count badges on tabs.
- `wiom-list-item` — list-row anatomy that hosts a trailing Label
  badge.
- `wiom-top-bar` — places the bell icon that hosts the Count overlay.
- `wiom-icon-badge` — distinct component (icon inside a colored
  circle); not a badge in the status-indicator sense.
