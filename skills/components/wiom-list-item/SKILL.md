---
name: wiom-list-item
description: >
  Wiom List Item — the unified row component for every list pattern
  in the Wiom apps: navigation, settings, selection (multi /
  single), toggle, and information rows. 1 component · 5 types
  (Default · icon with bg · Checkbox · Radio · Switch) × 2 states
  (Default · Disabled) = **10 ship variants**, plus 5 booleans
  (Pressed, Selected, Has Leading, Has Trailing Icon, Has Secondary,
  Has Trailing Meta) and 3 text props (Primary, Secondary, Trailing
  Meta). Hosts `wiom-icon-badge` (icon-with-bg type), `wiom-selection-
  controls` (Checkbox / Radio / Switch types). Reference for
  generating Jetpack Compose code with `WiomListItem` and its 4 type-
  specific helpers, and for instancing the Figma component (set
  `1655:487`) via MCP. Use this skill EVERY TIME you place a list row,
  settings entry, multi-select / single-select picker row, toggle
  row, contact / profile row, or information row in the Wiom Partner
  or Customer Android app, or when assembling Figma frames with
  stacked rows. Trigger on: list item, list row, settings list,
  selection list, toggle list, list cell, navigation row, picker row,
  contact row, instance of 1655:487. Pairs with wiom-design-foundations
  (tokens), wiom-icon-badge (icon-with-bg leading), wiom-selection-
  controls (Checkbox / Radio / Switch leading or trailing), wiom-cta
  (don't put a CTA in a row — route via row tap), and wiom-compose-
  layout-rules (Modifier discipline).
---

# Wiom List Item

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
set `1655:487`, and the `wiom-design-foundations` token set. Hosts
`wiom-icon-badge` (icon-with-bg type) and `wiom-selection-controls`
(Checkbox / Radio / Switch types) as nested instances.

**1 component · 5 types · 2 states · 10 ship variants · 360 dp width · 48 dp min-height.**

| Type | Leading | Trailing | Use for |
|---|---|---|---|
| `Default` | 24 dp icon (`icon.nonAction`) | 20 dp `chevron_right` (`icon.brand`) | Navigation · settings · info rows |
| `icon with bg` | 48 dp `WiomIconBadge` (Md, tone-swappable) | 20 dp `chevron_right` (`icon.brand`) | Prominent settings sections · feature entry · status summary |
| `Checkbox` | 24 dp `WiomCheckbox` (selection × state) | (none) | Multi-select picker row |
| `Radio` | 24 dp `WiomRadio` (selected × state) | (none) | Single-select picker row |
| `Switch` | 24 dp icon (`icon.nonAction`) | 52 × 32 dp `WiomSwitch` (off/on × state) | Settings toggles — instant-apply binary settings |

The list item **owns the row**: padding, tap target, ripple, Pressed / Selected / Disabled overlays. Selection controls and the icon badge **own their indicators**. The skill split is: row layout & states here; indicator anatomy in `wiom-selection-controls` / `wiom-icon-badge`.

---

## 1. Figma — instancing the component

Instance the component set `1655:487` and set `Type` + `State`. Strings are **case-sensitive**.

### Variants — 10 ship combinations (5 Type × 2 State)

| `Type` | `State = Default` | `State = Disabled` |
|---|---|---|
| `Default` | `1655:421` | `3106:1101` |
| `icon with bg` | `3064:7588` | `3106:1114` |
| `Checkbox` | `1655:435` | `3106:1128` |
| `Radio` | `1655:453` | `3106:1140` |
| `Switch` | `1655:469` | `3106:1152` |

> **`State = Disabled` is a Figma ship variant** (foundations Pattern A — fill / text / icon all swap to the disabled token family), **not an opacity overlay**. Older skill drafts implemented Disabled as `alpha(0.38f)` — that fails AAA contrast and disagrees with foundations. Use the disabled tokens directly.

### Properties (apply per instance)

| Property | Type | Default | Applies to | Purpose |
|---|---|---|---|---|
| `Pressed` | Boolean | `false` | All | Reveals `bg.subtle` (#F1EDF7) overlay across the row — runtime tap feedback |
| `Selected` | Boolean | `false` | Default · icon with bg · Checkbox · Radio | Reveals `bg.selected` (#FFCCED) overlay across the row — current / chosen menu item. Skipped on Switch |
| `Has Leading` | Boolean | `true` | Default · Switch | Hide the leading icon slot (rarely useful — most rows have a leading) |
| `Has Trailing Icon` | Boolean | `true` | Default · icon with bg | Hide the trailing chevron — used for menu rows (no nav implied) and status rows (info display) |
| `Has Secondary` | Boolean | `false` | All | Reveal the supporting-text line below the primary |
| `Has Trailing Meta` | Boolean | `false` | Default · icon with bg | Reveal the trailing meta text (e.g. "English", "12.4 GB", "v3.4.1") between content and chevron |
| `Primary Text` | Text | `"Primary text"` | All | The primary (clickable headline) — `type.body.lg` |
| `Secondary Text` | Text | `"Supporting text"` | All | The supporting line — `type.body.sm` |
| `Trailing Meta Text` | Text | `"Meta"` | Default · icon with bg | The meta value — `type.body.sm` |

### Bubbled-up nested-instance properties

For Type=`Checkbox` / `Radio` / `Switch` / `icon with bg`, the nested control / badge exposes its own variants on the list-item instance properties panel:

| Type | Bubbled props |
|---|---|
| `icon with bg` | Leading `WiomIconBadge` glyph (instance swap to any 24 dp icon). **Tone is not bubbled in Figma** — it ships Neutral; in code the helper accepts a `tone` parameter |
| `Checkbox` | `Selection` (No · Indeterminate · Selected) · `State` (Default · Disabled · Error) |
| `Radio` | `Selection` (No · Yes) · `State` (Default · Disabled · Error) |
| `Switch` | `Selection` (Off · On) · `State` (Default · Pressed · Disabled). Pressed is runtime-driven, not authored |

> The list-item's Primary Text **is** the label for Checkbox / Radio / Switch types — the selection controls themselves are indicator-only (see `wiom-selection-controls` §3). Do NOT add a label inside the control instance; the row owns it.

### Code enum ↔ Figma variant string

| Kotlin | Figma `Type` value |
|---|---|
| `WiomListItemType.Default` | `Default` |
| `WiomListItemType.IconBg` | `icon with bg` *(case-sensitive — lowercase, with spaces)* |
| `WiomListItemType.Checkbox` | `Checkbox` |
| `WiomListItemType.Radio` | `Radio` |
| `WiomListItemType.Switch` | `Switch` |

| Kotlin | Figma `State` value |
|---|---|
| `WiomListItemState.Default` | `Default` |
| `WiomListItemState.Disabled` | `Disabled` |

---

## 2. Anatomy

```
┌──────────────────────── 360 dp wide · 48 dp min-height ───────────────────────────┐
│                                                                                    │
│ [ space-16 padding all sides ]                                                     │
│                                                                                    │
│  ●Lead    Primary text (type.body.lg, text.default)         Meta  ●Trail          │
│  24 dp    ↓ space-4 ↓                                       12 sp 20 dp           │
│  (or      Secondary text (type.body.sm, text.subtle)                              │
│   48 dp                                                                            │
│   for                                                                              │
│   icon                                                                             │
│   with                                                                             │
│   bg)                                                                              │
│                                                                                    │
└────────────────────────────────────────────────────────────────────────────────────┘
   ↑                                                                            ↑
  space-16 L                                          space-8 (meta↔chevron)  space-16 R
   ↑
  space-12 (leading→content)
```

| Slot | Value |
|---|---|
| Width | 360 dp (edge-to-edge in screen body, or `fillMaxWidth()` in code) |
| Min-height | 48 dp (action touch target — WCAG ≥ 48) |
| Outer padding | `space-16` all sides |
| Leading → content gap | `space-12` |
| Content → trailing gap | `space-12` |
| Primary → secondary gap | `space-4` (only when `Has Secondary = true`) |
| Trailing meta → trailing icon gap | `space-8` |
| Leading | 24 dp icon (Default · Switch) · 48 dp `WiomIconBadge` (icon with bg) · 24 dp control (Checkbox · Radio) |
| Content (Column, fills remaining width) | Primary `type.body.lg` (16 / 24) · Secondary `type.body.sm` (12 / 16) when shown |
| Trailing | `chevron_right` 20 dp (Default · icon with bg) · `WiomSwitch` 52 × 32 dp (Switch) · (none) for Checkbox · Radio |

> **Switch row computed height ≈ 64 dp** — the 32 dp switch track + 16 × 2 padding = 64. The 48 dp `min-height` is satisfied by the larger trailing. All other types compute to 56 dp (24 dp content + 16 × 2). Don't fix heights — let them hug intrinsically.

---

## 3. Surface contract — the row is the surface

The list item **owns the row** as a self-contained surface: full-width fill, 16 dp inner gutter, 48 dp tap target, Pressed / Selected / Disabled overlays. It hosts but does not add to the indicator components — `wiom-icon-badge` and `wiom-selection-controls` render their atoms; the row supplies the rest.

### What this means in code

- `Modifier.fillMaxWidth()` on the row (`360 dp` in Figma, but in code the screen body or card width drives it).
- `heightIn(min = 48.dp)` — never `height(56.dp)` or any other fixed value. Switch type computes to ~64 dp because of the trailing track height; let it hug.
- The row owns `Modifier.clickable { onClick() }` — even on Checkbox / Radio / Switch types, the **row tap** flips the control's value. The control itself stays decorative inside the row's a11y semantics (per `wiom-selection-controls` §3).
- Pressed and Selected ship as **`bg.*` overlays under the content**, not as recolours of the content. Pressed = `bg.subtle`. Selected = `bg.selected`. Disabled state swaps the content tokens directly.

### Stacking lists

- Stack rows directly with **0 gap** — the 16 dp vertical padding on each row creates natural separation. Don't add explicit dividers. **Exception:** inside a `wiom-dialog` Selection body, rows sit `space-8` apart (Figma ship `2151:113`).
- Place the stack inside a card with `radius-large` (16 dp) corners and `bg.default` or `bg.subtle` fill. **Card padding = 0 all sides** — rows own their padding.
- Inside a bottom-sheet picker, the stack hosts ≤ 4 rows for quick pickers (or unlimited rows in scrollable pickers); the sheet handle + handle padding is handled by `wiom-bottomsheet`.

### Disabled = foundations Pattern A · not opacity

`state = Disabled` swaps every content token to its `*.disabled` variant — text → `text.disabled`, icons → `icon.disabled`, icon-badge fill → `bg.disabled`; nested checkbox / radio / switch flip to their own Disabled state. Row tap is suppressed. **Never `Modifier.alpha(0.38f)`** — opacity fails AAA contrast and disagrees with foundations. Per-slot tokens in §5.

---

## 4. Pressed · Selected · Disabled — runtime states

| State | Visual signal | When |
|---|---|---|
| `Pressed` (boolean) | `bg.subtle` overlay across the row | Compose `InteractionSource` press — runtime tap feedback. Don't author this in Figma except when illustrating the press visual |
| `Selected` (boolean) | `bg.selected` overlay across the row | Currently-chosen menu item · current page in a side-nav · selected option in a confirmation dialog. Skipped on Switch (the switch position IS the selection signal) |
| `Disabled` (variant) | All content tokens swap to `*.disabled` family | Row is non-interactive — feature gated, prerequisite missing, or context-disabled. Tap is suppressed |

**Pressed XOR Selected** — they don't combine. If a row is the current selection AND being pressed, prefer Selected (overlay continues to read).

**Disabled overrides Pressed / Selected** — a disabled row never shows press feedback or selection.

**Motion:** the `bg.selected` overlay fades in and out on `WiomMotion.selection()` (200 ms, foundations § Motion), together with the hosted checkbox / radio. Pressed `bg.subtle` snaps (instant feedback). The tap commits immediately; nothing waits for the fade.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Layout

| Slot | Token |
|---|---|
| Outer padding | `space-16` |
| Leading → content gap | `space-12` |
| Primary → secondary gap | `space-4` |
| Trailing meta → trailing icon gap | `space-8` |
| Min-height | 48 dp (action touch target) |
| Width | `fillMaxWidth()` (360 dp in Figma) |

### Default state

| Slot | Token |
|---|---|
| Row background | transparent (adopts parent surface) |
| Pressed overlay | `bg.subtle` (#F1EDF7) |
| Selected overlay | `bg.selected` (#FFCCED) |
| Primary text | `type.body.lg` + `text.default` (#161021) |
| Secondary text | `type.body.sm` + `text.subtle` (#5C5570) |
| Trailing meta text | `type.body.sm` + `text.subtle` |
| Leading icon (Default · Switch) | 24 dp · `icon.nonAction` (#898296) — decorative-in-tappable-row |
| Trailing chevron | 20 dp · **`icon.brand` (#D9008D)** — pink, signalling tap affordance |

> **Trailing chevron is `icon.brand`, not `icon.nonAction` / `icon.action`.** Per Figma ship: the chevron is the row's affordance signal and ships in brand pink to call out the tap. Decorative leading icons stay neutral; the chevron pulls the eye.

### Disabled state (foundations Pattern A)

| Slot | Token |
|---|---|
| Primary text | `type.body.lg` + `text.disabled` (#A7A1B2) |
| Secondary text | `type.body.sm` + `text.disabled` |
| Trailing meta text | `type.body.sm` + `text.disabled` |
| Leading icon (Default · Switch) | 24 dp · `icon.disabled` (#A7A1B2) |
| Leading icon-with-bg | `bg.disabled` (#D7D3E0) fill + `icon.disabled` glyph |
| Leading Checkbox · Radio | per `wiom-selection-controls` Disabled state |
| Trailing chevron | 20 dp · `icon.disabled` |
| Trailing Switch | per `wiom-selection-controls` Disabled state |
| Row tap | suppressed |

No shadow on the row. No stroke. No tonal elevation. Pressed / Selected / Disabled are all token swaps, never opacity.

---

## 6. Universal rules

1. **Min-height 48 dp · let it hug above that.** Don't fix `height(56.dp)`. Switch type computes to ~64 dp; that's correct.
2. **Row owns the tap.** Even on Checkbox / Radio / Switch types, `Modifier.clickable` is on the row, not on the nested control. The control's `onToggle` / `onSelect` / `onCheckedChange` is wired to the **same** lambda as the row's `onClick` (tapping anywhere flips the state).
3. **Selection controls and icon-badge are nested instances — never re-implemented.** Use `WiomCheckbox` · `WiomRadio` · `WiomSwitch` from `wiom-selection-controls`. Use `WiomIconBadge` (Md size) from `wiom-icon-badge`. The list-item supplies the row; those skills supply the indicator.
4. **Primary Text is the label for Checkbox / Radio / Switch types.** The selection controls ship indicator-only — never put a label inside the control instance.
5. **Disabled is Pattern A, not opacity.** Swap content tokens directly to the `*.disabled` family. Never `Modifier.alpha`.
6. **Trailing chevron is `icon.brand` (pink).** It's the tap-affordance signal — leading icons stay `icon.nonAction` (neutral), trailing chevron pulls the eye.
7. **Pressed XOR Selected.** Don't render both overlays together. Selected wins when the row is the current item.
8. **Stack lists with 0 gap and no dividers** (one exception: `space-8` between rows in a `wiom-dialog` Selection body). The 16 dp vertical padding on each row provides separation. Cards hosting the stack have 0 internal padding.
9. **Don't put a CTA in a row.** If the row leads to an action that needs a button, route via row tap; the action page below shows the CTA. Rows are navigation / selection / display, not action surfaces.
10. **Tokens only — element-first.** No raw hex. No legacy `WiomColor.surfaceSubtle` / `WiomColor.textPrimary` namespaces; use `Wiom.bg.subtle` / `Wiom.text.default`.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomListItemType  { Default, IconBg, Checkbox, Radio, Switch }
enum class WiomListItemState { Default, Disabled }
```

The base `WiomListItem` is slot-based; four type-specific helpers wire the right leading / trailing automatically.

```kotlin
// Base — slot-based. Use the type helpers below for typical rows.
@Composable
fun WiomListItem(
    primary: String,
    secondary: String? = null,
    leading: @Composable (() -> Unit)? = null,           // 24 dp icon · 48 dp icon-badge · 24 dp selection-control
    trailing: @Composable (() -> Unit)? = null,          // 20 dp chevron · 52×32 dp switch · null for Checkbox / Radio
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
)

// Default type — leading icon + trailing chevron.
@Composable
fun WiomListItem(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int? = R.drawable.ic_settings_rounded,
    showTrailingChevron: Boolean = true,
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
)

// icon-with-bg type — leading 48 dp WiomIconBadge + trailing chevron.
@Composable
fun WiomListItemIconBg(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int,
    leadingTone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    showTrailingChevron: Boolean = true,
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
)

// Checkbox type — leading WiomCheckbox; row tap flips selection.
@Composable
fun WiomListItemCheckbox(
    primary: String,
    secondary: String? = null,
    selection: WiomCheckboxSelection,                    // No · Indeterminate · Selected
    onSelectionChange: (WiomCheckboxSelection) -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    selected: Boolean = false,                            // overlay — separate from the checkbox value
    modifier: Modifier = Modifier,
)

// Radio type — leading WiomRadio; row tap selects this option.
@Composable
fun WiomListItemRadio(
    primary: String,
    secondary: String? = null,
    radioSelected: Boolean,                               // the radio's Yes/No state (named to disambiguate from row.selected)
    onRadioSelect: () -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    selected: Boolean = false,
    modifier: Modifier = Modifier,
)

// Switch type — leading icon + trailing WiomSwitch; row tap toggles.
@Composable
fun WiomListItemSwitch(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int? = null,             // optional — `Has Leading` boolean equivalent
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    modifier: Modifier = Modifier,
)
```

### Base `WiomListItem` impl

```kotlin
@Composable
fun WiomListItem(
    primary: String,
    secondary: String? = null,
    leading: @Composable (() -> Unit)? = null,
    trailing: @Composable (() -> Unit)? = null,
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val isDisabled = state == WiomListItemState.Disabled
    val primaryColor   = if (isDisabled) Wiom.text.disabled else Wiom.text.default
    val secondaryColor = if (isDisabled) Wiom.text.disabled else Wiom.text.subtle
    val metaColor      = if (isDisabled) Wiom.text.disabled else Wiom.text.subtle

    // Pressed = runtime via InteractionSource. Selected = caller-driven overlay.
    val interactionSource = remember { MutableInteractionSource() }
    val pressed by interactionSource.collectIsPressedAsState()

    // Two layers: Pressed snaps (instant feedback); Selected fades on WiomMotion.selection.
    // Selected is drawn on top, so it wins when both apply (§4). Disabled shows neither.
    val pressedFill = if (pressed && !isDisabled) Wiom.bg.subtle else Color.Transparent
    val selectedFill by animateColorAsState(
        targetValue = if (selected && !isDisabled) Wiom.bg.selected else Wiom.bg.selected.clear(),  // not Color.Transparent
        animationSpec = WiomMotion.selection(),
        label = "ListItemSelected",
    )

    Box(
        modifier = modifier
            .fillMaxWidth()
            .heightIn(min = 48.dp)
            .background(pressedFill)
            .background(selectedFill)
            .then(
                if (!isDisabled && onClick != null)
                    Modifier.clickable(
                        interactionSource = interactionSource,
                        indication = LocalIndication.current,
                        onClick = onClick,
                    )
                else Modifier
            ),
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(Wiom.space.s16),
            verticalAlignment = Alignment.CenterVertically,
            horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
        ) {
            leading?.invoke()
            Column(
                modifier = Modifier.weight(1f),
                verticalArrangement = Arrangement.spacedBy(Wiom.space.s4),
            ) {
                Text(primary, style = Wiom.type.body.lg, color = primaryColor, maxLines = 1)
                if (secondary != null) {
                    Text(secondary, style = Wiom.type.body.sm, color = secondaryColor, maxLines = 1)
                }
            }
            if (trailing != null || trailingMeta != null) {
                Row(
                    verticalAlignment = Alignment.CenterVertically,
                    horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
                ) {
                    if (trailingMeta != null) {
                        Text(trailingMeta, style = Wiom.type.body.sm, color = metaColor, maxLines = 1)
                    }
                    trailing?.invoke()
                }
            }
        }
    }
}
```

### Default-type helper impl

```kotlin
@Composable
fun WiomListItem(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int? = R.drawable.ic_settings_rounded,
    showTrailingChevron: Boolean = true,
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val isDisabled = state == WiomListItemState.Disabled
    WiomListItem(
        primary = primary,
        secondary = secondary,
        leading = leadingIconRes?.let { res ->
            {
                Icon(
                    painter = painterResource(res),
                    contentDescription = null,
                    tint = if (isDisabled) Wiom.icon.disabled else Wiom.icon.nonAction,
                    modifier = Modifier.size(24.dp),
                )
            }
        },
        trailing = if (showTrailingChevron) {
            {
                Icon(
                    painter = painterResource(R.drawable.ic_chevron_right_rounded),
                    contentDescription = null,
                    tint = if (isDisabled) Wiom.icon.disabled else Wiom.icon.brand,
                    modifier = Modifier.size(20.dp),
                )
            }
        } else null,
        trailingMeta = trailingMeta,
        selected = selected,
        state = state,
        onClick = onClick,
        modifier = modifier,
    )
}
```

### `WiomListItemIconBg` impl

```kotlin
@Composable
fun WiomListItemIconBg(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int,
    leadingTone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    showTrailingChevron: Boolean = true,
    trailingMeta: String? = null,
    selected: Boolean = false,
    state: WiomListItemState = WiomListItemState.Default,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val isDisabled = state == WiomListItemState.Disabled
    WiomListItem(
        primary = primary,
        secondary = secondary,
        leading = {
            // Disabled list-item swaps the icon-badge tokens to bg.disabled + icon.disabled
            // (foundations Pattern A) — synthesised here since icon-badge has no Disabled tone.
            if (isDisabled) {
                Box(
                    modifier = Modifier
                        .size(48.dp)
                        .background(Wiom.bg.disabled, CircleShape),
                    contentAlignment = Alignment.Center,
                ) {
                    Icon(
                        painter = painterResource(leadingIconRes),
                        contentDescription = null,
                        tint = Wiom.icon.disabled,
                        modifier = Modifier.size(24.dp),
                    )
                }
            } else {
                WiomIconBadge(
                    iconRes = leadingIconRes,
                    size = WiomIconBadgeSize.Md,
                    tone = leadingTone,
                )
            }
        },
        trailing = if (showTrailingChevron) {
            {
                Icon(
                    painter = painterResource(R.drawable.ic_chevron_right_rounded),
                    contentDescription = null,
                    tint = if (isDisabled) Wiom.icon.disabled else Wiom.icon.brand,
                    modifier = Modifier.size(20.dp),
                )
            }
        } else null,
        trailingMeta = trailingMeta,
        selected = selected,
        state = state,
        onClick = onClick,
        modifier = modifier,
    )
}
```

### `WiomListItemCheckbox` / `WiomListItemRadio` / `WiomListItemSwitch` impl

```kotlin
@Composable
fun WiomListItemCheckbox(
    primary: String,
    secondary: String? = null,
    selection: WiomCheckboxSelection,
    onSelectionChange: (WiomCheckboxSelection) -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    selected: Boolean = false,
    modifier: Modifier = Modifier,
) {
    val cbState = if (state == WiomListItemState.Disabled)
        WiomCheckboxState.Disabled else WiomCheckboxState.Default
    val toggled: () -> Unit = {
        val next = when (selection) {
            WiomCheckboxSelection.No, WiomCheckboxSelection.Indeterminate -> WiomCheckboxSelection.Selected
            WiomCheckboxSelection.Selected -> WiomCheckboxSelection.No
        }
        onSelectionChange(next)
    }
    WiomListItem(
        primary = primary,
        secondary = secondary,
        leading = {
            // Control is decorative in the row's a11y tree — onToggle = null; row tap flips it.
            WiomCheckbox(selection = selection, state = cbState, onToggle = null)
        },
        trailing = null,
        selected = selected,
        state = state,
        onClick = toggled,
        modifier = modifier,
    )
}

@Composable
fun WiomListItemRadio(
    primary: String,
    secondary: String? = null,
    radioSelected: Boolean,
    onRadioSelect: () -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    selected: Boolean = false,
    modifier: Modifier = Modifier,
) {
    val rState = if (state == WiomListItemState.Disabled)
        WiomRadioState.Disabled else WiomRadioState.Default
    WiomListItem(
        primary = primary,
        secondary = secondary,
        leading = { WiomRadio(selected = radioSelected, state = rState, onSelect = null) },
        trailing = null,
        selected = selected,
        state = state,
        onClick = onRadioSelect,
        modifier = modifier,
    )
}

@Composable
fun WiomListItemSwitch(
    primary: String,
    secondary: String? = null,
    @DrawableRes leadingIconRes: Int? = null,
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit,
    state: WiomListItemState = WiomListItemState.Default,
    modifier: Modifier = Modifier,
) {
    val isDisabled = state == WiomListItemState.Disabled
    val swState = if (isDisabled) WiomSwitchState.Disabled else WiomSwitchState.Default
    WiomListItem(
        primary = primary,
        secondary = secondary,
        leading = leadingIconRes?.let { res ->
            {
                Icon(
                    painter = painterResource(res),
                    contentDescription = null,
                    tint = if (isDisabled) Wiom.icon.disabled else Wiom.icon.nonAction,
                    modifier = Modifier.size(24.dp),
                )
            }
        },
        trailing = {
            // Switch indicator is decorative in the row's a11y tree — onCheckedChange = null.
            WiomSwitch(checked = checked, state = swState, onCheckedChange = null)
        },
        selected = false,                                  // Switch type doesn't ship Selected
        state = state,
        onClick = { onCheckedChange(!checked) },
        modifier = modifier,
    )
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — fixed height, alpha-based disabled, label baked into the checkbox,
//             nested control owns the click (dual tap targets, broken a11y).
Row(modifier = Modifier.fillMaxWidth().height(56.dp).alpha(if (disabled) 0.38f else 1f)) {
    Checkbox(checked = checked, onCheckedChange = setChecked)         // Material, not Wiom
    Text("I agree to terms")                                          // label inside row layout, ad-hoc
}

// ✅ RIGHT — heightIn min 48 dp, foundations Pattern A for disabled,
//             primary text is the label, row tap flips the checkbox.
WiomListItemCheckbox(
    primary = "I agree to the Terms",
    selection = if (agreed) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
    onSelectionChange = { next -> agreed = next == WiomCheckboxSelection.Selected },
    state = if (formDisabled) WiomListItemState.Disabled else WiomListItemState.Default,
)
```

---

## 8. Recipes

### Settings list (Default rows in a card)

```kotlin
Card(
    shape = RoundedCornerShape(16.dp),                    // radius-large
    colors = CardDefaults.cardColors(containerColor = Wiom.bg.default),
    modifier = Modifier.fillMaxWidth().padding(horizontal = 16.dp),
) {
    Column {                                               // 0 gap, no dividers
        WiomListItem(
            primary = "Account",
            leadingIconRes = R.drawable.ic_person_rounded,
            onClick = onAccount,
        )
        WiomListItem(
            primary = "Notifications",
            leadingIconRes = R.drawable.ic_notifications_rounded,
            onClick = onNotifications,
        )
        WiomListItem(
            primary = "Language",
            trailingMeta = "English",
            leadingIconRes = R.drawable.ic_language_rounded,
            onClick = onLanguage,
        )
        WiomListItem(
            primary = "Privacy",
            leadingIconRes = R.drawable.ic_lock_rounded,
            onClick = onPrivacy,
        )
    }
}
```

### Status list (no chevron — info display)

```kotlin
Column {
    WiomListItem(
        primary = "Storage",
        trailingMeta = "12.4 GB",
        showTrailingChevron = false,
        leadingIconRes = R.drawable.ic_sd_card_rounded,
        onClick = null,                                    // not tappable
    )
    WiomListItem(
        primary = "Plan",
        trailingMeta = "28 days",
        showTrailingChevron = false,
        leadingIconRes = R.drawable.ic_card_membership_rounded,
        onClick = null,
    )
    WiomListItem(
        primary = "Build",
        trailingMeta = "v3.4.1",
        showTrailingChevron = false,
        leadingIconRes = R.drawable.ic_info_rounded,
        onClick = null,
    )
}
```

### Prominent settings sections (icon-with-bg)

```kotlin
Column {
    WiomListItemIconBg(
        primary = "Wallet",
        secondary = "Recharges, refunds, pending payments",
        leadingIconRes = R.drawable.ic_account_balance_wallet_rounded,
        leadingTone = WiomIconBadgeTone.Brand,
        onClick = onWallet,
    )
    WiomListItemIconBg(
        primary = "Plan health",
        secondary = "All systems normal",
        leadingIconRes = R.drawable.ic_check_circle_rounded,
        leadingTone = WiomIconBadgeTone.Positive,
        onClick = onHealth,
    )
    WiomListItemIconBg(
        primary = "Action required",
        secondary = "KYC verification pending",
        leadingIconRes = R.drawable.ic_warning_rounded,
        leadingTone = WiomIconBadgeTone.Warning,
        onClick = onKyc,
    )
}
```

### Multi-select picker (Checkbox)

```kotlin
val plans = remember { listOf("Monthly", "Quarterly", "Annual") }
val picked = remember { mutableStateMapOf<String, Boolean>() }

Column {
    plans.forEach { plan ->
        WiomListItemCheckbox(
            primary = plan,
            selection = if (picked[plan] == true)
                WiomCheckboxSelection.Selected
            else WiomCheckboxSelection.No,
            onSelectionChange = { next ->
                picked[plan] = (next == WiomCheckboxSelection.Selected)
            },
        )
    }
}
```

### Single-select picker (Radio) — language picker as the canonical case

```kotlin
val languages = remember { listOf("English", "हिन्दी", "मराठी") }
var pickedIndex by remember { mutableStateOf(0) }

Column {
    languages.forEachIndexed { i, lang ->
        WiomListItemRadio(
            primary = lang,
            radioSelected = i == pickedIndex,
            onRadioSelect = { pickedIndex = i },
        )
    }
}
```

### Toggle settings (Switch — the primary instant-apply pattern)

```kotlin
var pushEnabled by remember { mutableStateOf(true) }
var darkMode by remember { mutableStateOf(false) }
var autoPay by remember { mutableStateOf(false) }

Column {
    WiomListItemSwitch(
        primary = "Push notifications",
        secondary = "Plan renewals and offers",
        leadingIconRes = R.drawable.ic_notifications_rounded,
        checked = pushEnabled,
        onCheckedChange = { pushEnabled = it },
    )
    WiomListItemSwitch(
        primary = "Dark mode",
        leadingIconRes = R.drawable.ic_dark_mode_rounded,
        checked = darkMode,
        onCheckedChange = { darkMode = it },
    )
    WiomListItemSwitch(
        primary = "Auto-pay",
        secondary = "Renew automatically every month",
        leadingIconRes = R.drawable.ic_autorenew_rounded,
        checked = autoPay,
        onCheckedChange = { autoPay = it },
    )
}
```

### Bottom-sheet picker (Default rows with `Selected` overlay)

```kotlin
val options = remember { listOf("Recommended", "Cheapest", "Newest") }
var picked by remember { mutableStateOf(options.first()) }

Column {                                                   // inside a wiom-bottomsheet body
    options.forEach { opt ->
        WiomListItem(
            primary = opt,
            leadingIconRes = null,                         // no leading icon for sheet picker
            showTrailingChevron = false,                   // sheet picker is select-and-dismiss
            selected = opt == picked,                      // bg.selected overlay
            onClick = {
                picked = opt
                onSelected(opt)                            // sheet dismisses on its own
            },
        )
    }
}
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Selected overlay snapping on while the hosted radio / checkbox fades. The row and its control ease together (§4 Motion); only Pressed snaps.
- ❌ Putting a CTA inside a row — rows route navigation / selection / display. CTAs go on the destination page.
- ❌ Adding a `Divider()` between stacked rows — the 16 dp vertical padding handles separation. Dividers are noise.
- ❌ `Modifier.alpha(0.38f)` for Disabled — use the `*.disabled` token family directly (Pattern A). Opacity fails AAA contrast.
- ❌ Wrapping the entire row in `Modifier.alpha(…)` for any reason — alpha kills affordance contrast. Use tokens.
- ❌ Putting both Pressed and Selected overlays on the same row — they don't combine. Selected wins.
- ❌ Implementing a row with raw `Row` + `Text` + `Icon` from scratch — always use `WiomListItem` (or one of the 4 type helpers).
- ❌ Adding a label inside `WiomCheckbox` / `WiomRadio` / `WiomSwitch` — those are indicator-only. The row's primary text is the label.
- ❌ Nested-control `onToggle` / `onSelect` / `onCheckedChange` set to a non-null lambda inside a row — the row owns the click. Pass `null` to the control and wire the lambda to the row.
- ❌ Using `WiomColor.surfaceSubtle` / `WiomColor.textPrimary` legacy namespaces — current foundations is `Wiom.bg.subtle` / `Wiom.text.default`.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. List Item canvas: `3069:6035`. Component set: `1655:487`.
Re-pull `get_variable_defs` on `1655:487` if foundations token names ever change.

| Node | Variant |
|---|---|
| `1655:487` | List Item component set — 10 ship variants (5 Type × 2 State) |
| `1655:421` | Type=Default · State=Default |
| `3106:1101` | Type=Default · State=Disabled |
| `3064:7588` | Type=icon with bg · State=Default |
| `3106:1114` | Type=icon with bg · State=Disabled |
| `1655:435` | Type=Checkbox · State=Default |
| `3106:1128` | Type=Checkbox · State=Disabled |
| `1655:453` | Type=Radio · State=Default |
| `3106:1140` | Type=Radio · State=Disabled |
| `1655:469` | Type=Switch · State=Default |
| `3106:1152` | Type=Switch · State=Disabled |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The component set ships **10 variants** (5 Type × 2 State), not 5. Older skill drafts described `Disabled` as a boolean — the Figma ship promotes it to a State variant alongside `Default`. The Compose API surfaces this as `state: WiomListItemState`, not a boolean.
- **Trailing chevron is `icon.brand` (#D9008D pink)**, not `icon.nonAction` / `icon.action`. The chevron is the row's affordance signal — it ships in brand pink to call out the tap. Decorative leading icons stay `icon.nonAction`. Older skill drafts said the chevron uses neutral; the V2 ship corrects this.
- **Disabled uses foundations Pattern A** — content tokens swap to the `*.disabled` family (`text.disabled` / `icon.disabled` / `bg.disabled` for icon-badge fill). Older drafts implemented disabled as `alpha(0.38f)`; that's a foundations violation and is corrected here.
- **`Has Leading` applies to `Default` / `Switch`; `Has Trailing Icon` applies to `Default` / `icon with bg`** — not all booleans apply to all types. Checkbox / Radio's leading IS the control (can't be hidden); Switch's trailing IS the switch.
- **Selected boolean does NOT apply to Switch** — the switch position is the selection signal. Adding a `bg.selected` overlay on top would double-encode. Other types (Default · icon with bg · Checkbox · Radio) all support Selected.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.subtle` (Pressed) / `bg.selected` (Selected) / `bg.disabled`, `text.default` / `text.subtle` / `text.disabled`, `icon.nonAction` / `icon.brand` / `icon.disabled`, `space-4` / `space-8` / `space-12` / `space-16`, `type.body.lg` / `type.body.sm`, `radius-large` (16 dp) for hosting cards. Disabled = Pattern A (token swap, not opacity).
- `wiom-selection-controls` — `WiomCheckbox` / `WiomRadio` / `WiomSwitch` — embedded in the Checkbox / Radio / Switch types. Indicator-only; label is the row's primary text. Row owns the tap.
- `wiom-icon-badge` — `WiomIconBadge` Md — embedded in the `icon with bg` type. Tone-swappable via the helper's `leadingTone` parameter.
- `wiom-bottomsheet` — primary host for picker rows (Checkbox / Radio for short lists, Default with `selected` overlay for longer single-select pickers).
- `wiom-cta` — **don't put a CTA inside a row.** The row routes; the destination page or sheet hosts the CTA.
- `wiom-compose-layout-rules` — Modifier discipline; `heightIn(min = 48.dp)` not fixed heights; `Arrangement.spacedBy` for stacked lists; no dividers between rows.
