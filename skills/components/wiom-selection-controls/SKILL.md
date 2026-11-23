---
name: wiom-selection-controls
description: >
  Wiom Checkbox · Radio · Switch — the three small selection controls,
  unified because they share sizing, token patterns, and the rule that
  label / helper / row chrome live on `wiom-list-item`, never inside
  the control itself. 3 components · 21 ship variants total
  (Checkbox 9 = 3 states × 3 selections; Radio 6 = 3 states × 2
  selections; Switch 6 = 3 states × 2 selections). Reference for
  generating Jetpack Compose code with `WiomCheckbox` / `WiomRadio` /
  `WiomSwitch` and for instancing the Figma component sets via MCP.
  Use this skill EVERY TIME you create, edit, or review a checkbox,
  radio, switch, toggle, multi-select, consent (T&C, auto-debit,
  privacy), single-select (plan, payment method, language,
  notification frequency), or instant-apply binary setting in the
  Wiom Partner or Customer Android app, or when assembling Figma
  frames with one of these three controls. Trigger on: Checkbox,
  RadioButton, Radio, Switch, Toggle, multi-select, consent, T&C,
  opt-in, opt-out, single-select, plan selection, payment method,
  preference, notification toggle, privacy toggle, auto-pay,
  instance of 1223:100 / 1400:2 / 1613:38. Pairs with
  wiom-design-foundations (tokens), wiom-list-item (row anatomy +
  label), wiom-cta (Acknowledge type uses inline checkbox), and
  wiom-compose-layout-rules (Modifier discipline).
---

# Wiom Selection Controls — Checkbox · Radio · Switch

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
sets `1223:100` (Checkbox), `1400:2` (Radio), `1613:38` (Switch), and
the `wiom-design-foundations` token set.

**3 components · 21 ship variants · 24 dp bounding box (Switch 52 × 32 dp track) · indicator-only — no label slot.**

| Component | Variants | Anatomy |
|---|---|---|
| Checkbox | 9 = 3 states (Default / Disabled / Error) × 3 selections (No / Indeterminate / Selected) | 24 dp rounded square (`radius-tiny` 4 dp) |
| Radio | 6 = 3 states (Default / Disabled / Error) × 2 selections (No / Yes) | 24 dp circle (`radius-full`) |
| Switch | 6 = 3 states (Default / Pressed / Disabled) × 2 selections (Off / On) | 52 × 32 dp pill track + 24 dp circular thumb |

These are **inline indicators**. They never ship with a built-in label, helper text, supporting copy, or row chrome — those slots belong to `wiom-list-item` (multi-select rows, language picker, payment-method picker, settings list) or to the Acknowledge CTA from `wiom-cta` (consent inline). The control itself stops at the indicator.

---

## 1. Figma — instancing the components

Three component sets, instanced separately. Strings are **case-sensitive**.

### Checkbox — `1223:100` (9 ship variants)

| `State` ↓ \ `Selection` → | `No` | `Indeterminate` | `Selected` |
|---|---|---|---|
| `Default` | `1221:8` — empty rounded square · `stroke.strong` outline | `3063:10145` — `bg.brand` fill · `Icons.Rounded.Remove` glyph `icon.inverse` | `1223:30` — `bg.brand` fill · `Icons.Rounded.Check` glyph `icon.inverse` |
| `Disabled` | `3063:10143` — empty · `stroke.subtle` outline | `1223:23` — `bg.disabled` fill · dash glyph `icon.disabled` | `1223:51` — `bg.disabled` fill · check glyph `icon.disabled` |
| `Error` | `1223:16` — empty · `stroke.critical.focus` outline | `3063:10147` — `bg.critical` fill · dash glyph `icon.inverse` | `1223:58` — `bg.critical` fill · check glyph `icon.inverse` |

### Radio — `1400:2` (6 ship variants)

| `State` ↓ \ `Selected` → | `No` | `Yes` |
|---|---|---|
| `Default` | `1398:2` — empty circle · `stroke.strong` outline | `1398:32` — `bg.brand` fill · 10 dp dot `icon.inverse` |
| `Disabled` | `1398:20` — empty · `stroke.subtle` outline | `1398:50` — `bg.disabled` fill · dot `icon.disabled` |
| `Error` | `1398:26` — empty · `stroke.critical.focus` outline | `1398:56` — `bg.critical` fill · dot `icon.inverse` |

### Switch — `1613:38` (6 ship variants)

| `State` ↓ \ `Selection` → | `Off` | `On` |
|---|---|---|
| `Default` | `1613:2` — `bg.muted` track · `stroke.strong` outline · thumb left | `1613:20` — `bg.brand` track · no stroke · thumb right |
| `Pressed` | `3063:10352` — `bg.subtle` track · `stroke.strong` · thumb left | `3063:10502` — `bg.brand.pressed` track · no stroke · thumb right |
| `Disabled` | `3063:10487` — `bg.disabled` track · `stroke.subtle` · thumb left | `1613:26` — `bg.disabled` track · no stroke · thumb right |

> **No `Show Label` / `Show Helper` / `Show Description` properties exist on any of the three.** The Figma component is the indicator only. Labels and helpers are owned by the row (`wiom-list-item`) or the inline parent (e.g., the Acknowledge CTA's compound checkbox).

### Code enum ↔ Figma variant string

| Kotlin | Figma value |
|---|---|
| `WiomCheckboxState.Default / Disabled / Error` | `Default` / `Disabled` / `Error` |
| `WiomCheckboxSelection.No / Indeterminate / Selected` | `No` / `Indeterminate` / `Selected` |
| `WiomRadioState.Default / Disabled / Error` | `Default` / `Disabled` / `Error` |
| `WiomSwitchState.Default / Pressed / Disabled` | `Default` / `Pressed` / `Disabled` (Pressed is runtime-driven, not authored) |

---

## 2. Anatomy

### Checkbox — 24 × 24 dp

```
┌─ 24 dp ──────────┐
│                  │
│  ┌────────────┐  │       Outer: 24 × 24 dp · radius-tiny (4 dp)
│  │            │  │       Stroke (No):  stroke-medium (2 dp) · stroke.strong / .subtle / .critical
│  │            │  │       Fill (Selected / Indeterminate): bg.brand / .disabled / .critical
│  │            │  │       Glyph: Icons.Rounded.Check (Selected) · Icons.Rounded.Remove (Indeterminate)
│  └────────────┘  │       Glyph tint: icon.inverse (Default · Error) · icon.disabled (Disabled)
│                  │
└──────────────────┘
```

| Slot | Value |
|---|---|
| Bounding box | 24 × 24 dp (fixed) |
| Radius | `radius-tiny` (4 dp) |
| Stroke (selection = No) | `stroke-medium` (2 dp) — color per state (see §5) |
| Fill (selection ≠ No) | per state (see §5) — no stroke when filled |
| Glyph (Selected) | `Icons.Rounded.Check` — Material Symbols Rounded |
| Glyph (Indeterminate) | `Icons.Rounded.Remove` — horizontal dash |
| Glyph size | inherits the icon container — fills the 24 dp box minus internal padding |
| Glyph tint | `icon.inverse` (Default · Error) · `icon.disabled` (Disabled) |
| Shadow | none — flat inline control |

### Radio — 24 × 24 dp

```
┌─ 24 dp ──────────┐
│                  │
│      ◯           │            Outer: 24 × 24 dp · radius-full
│                  │            Stroke (No): stroke-medium · stroke.strong / .subtle / .critical
│                  │            Fill (Yes): bg.brand / .disabled / .critical · no stroke
│                  │            Inner dot (Yes): 10 × 10 dp · radius-full · icon.inverse / .disabled
│                  │
└──────────────────┘
```

| Slot | Value |
|---|---|
| Bounding box | 24 × 24 dp (fixed) |
| Radius | `radius-full` |
| Stroke (selected = No) | `stroke-medium` (2 dp) — color per state |
| Fill (selected = Yes) | per state — no stroke when filled |
| Inner dot (Yes) | 10 × 10 dp · `radius-full` · `icon.inverse` (Default · Error) · `icon.disabled` (Disabled) |

### Switch — 52 × 32 dp track + 24 dp thumb

```
┌──── 52 dp ────────────────┐
│                            │   Track: 52 × 32 dp · radius-full
│  ●──────────                │   Track fill: per state (see §5)
│                            │   Thumb: 24 × 24 dp · radius-full · #FFFFFF
│                            │   Thumb position: Off → flush left · On → flush right
└──────── 32 dp ─────────────┘   Thumb inset: 4 dp on all sides when at rest

  Off (Default): track bg.muted + stroke.strong, thumb left
  On  (Default): track bg.brand · no stroke, thumb right
```

| Slot | Value |
|---|---|
| Track | 52 × 32 dp · `radius-full` |
| Track stroke (Off · Default / Pressed) | `stroke-medium` · `stroke.strong` |
| Track stroke (Off · Disabled) | `stroke-medium` · `stroke.subtle` |
| Track stroke (On, any state) | none |
| Track fill | per state (see §5) |
| Thumb | 24 × 24 dp · `radius-full` · `#FFFFFF` (always white, all states — physical affordance, not a tokened surface) |
| Thumb inset | 4 dp on all sides (so 32 − 24 − 8 = 0; thumb fits flush within the 4 dp padding) |
| Shadow | none on track and thumb (flat inline control) |

---

## 3. Surface contract — indicator only · no label · no helper · no row chrome

These are the three controls in the Wiom DS that **do not own** a label, helper text, supporting copy, or row padding. Everything around the indicator lives in the parent — typically `wiom-list-item`, occasionally the Acknowledge CTA's inline parent.

### What this means in code

- `WiomCheckbox / WiomRadio / WiomSwitch` accept no `label` or `helper` parameters. The composable renders the indicator and stops.
- The control's bounding box is its visual size: 24 dp (Checkbox / Radio) or 52 × 32 dp (Switch). **Don't** add `Modifier.size` / `Modifier.padding` from the call site to enlarge the indicator — if you need a bigger tap target, the parent row owns that (typically `wiom-list-item` provides ≥ 48 dp tap target via the row's vertical padding).
- The control has **no built-in tap area beyond its bounding box** when used inside a `wiom-list-item` (row tap routes to `onSelect` for the whole row). When used inline, wrap the control in a `Box` with `Modifier.minimumInteractiveComponentSize()` or attach a 48 dp clickable parent.

### Hit-target ownership

| Pattern | Hit target owned by |
|---|---|
| Inside `wiom-list-item` (the common case) | The row — full row click invokes `onSelect`; control itself is decorative within the row's a11y tree |
| Inline form (Acknowledge CTA, single consent line) | The compound parent (CTA, Row) wraps the control in a 48 dp+ tap surface |
| Standalone (rare; e.g., a single privacy toggle without a list row) | Wrap with `Modifier.minimumInteractiveComponentSize().clickable {}` at the call site |

### What's exception-allowed

- The Acknowledge CTA from `wiom-cta` ships an inline checkbox where the parent CTA owns the label. The selection-control still renders just the indicator; the CTA owns the rest.

---

## 4. States

The full state matrices (token per cell) live in §1's variant tables alongside the Figma node IDs. Two cross-control rules to call out separately:

- **Selected + Error swap.** When a Checkbox is `Selected` (or Radio is `Yes`) AND the form is in error, the fill swaps from `bg.brand` → `bg.critical`. The glyph / dot stays `icon.inverse`. Switch has no Error state.
- **Switch Pressed steps the track one level.** Off: `bg.muted` → `bg.subtle`. On: `bg.brand` → `bg.brand.pressed`. The thumb does **not** move on press — only on selection change.

### States deliberately NOT shipped

- **Hover** — not a touch primitive on Android. Not authored; don't fabricate.
- **Focused** (as a distinct fill / stroke variant) — keyboard focus is communicated via the `stroke.brand.focus` ring (2 dp, +2 dp outside the indicator), **not** by changing the indicator's own colors. The ring is hard-edged (no blur).
- **Switch — Error** — switches are instant-apply settings; there's no form submission to validate. If you find yourself wanting Switch+Error, the right move is usually a Checkbox or a confirmation dialog.
- **Checkbox / Radio — Pressed** — Compose ripple covers it; no separate authored state.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Shared

| Slot | Token |
|---|---|
| Stroke width (all) | `stroke-medium` (2 dp) |
| Indicator stroke — unselected · Default | `stroke.strong` (#473F55) |
| Indicator stroke — unselected · Disabled | `stroke.subtle` (#D7D3E0) |
| Indicator stroke — unselected · Error | `stroke.critical.focus` (#D92130) |
| Selected fill — Default | `bg.brand` (#D9008D) |
| Selected fill — Disabled | `bg.disabled` (#D7D3E0) |
| Selected fill — Error | `bg.critical` (#D92130) |
| Selected glyph / dot — Default · Error | `icon.inverse` (#FAF9FC) |
| Selected glyph / dot — Disabled | `icon.disabled` (#A7A1B2) |
| Shadow (all) | none — flat inline control |
| Focus ring (a11y) | `stroke.brand.focus` (#D9008D) · `stroke-medium` · 2 dp outside indicator · radius = indicator radius + 2 dp · hard edge (no blur) |

### Checkbox

| Slot | Value |
|---|---|
| Outer | 24 × 24 dp · `radius-tiny` (4 dp) |
| Selected glyph | `Icons.Rounded.Check` |
| Indeterminate glyph | `Icons.Rounded.Remove` |

### Radio

| Slot | Value |
|---|---|
| Outer | 24 × 24 dp · `radius-full` |
| Inner dot (Yes) | 10 × 10 dp · `radius-full` |

### Switch

| Slot | Value |
|---|---|
| Track | 52 × 32 dp · `radius-full` |
| Thumb | 24 × 24 dp · `#FFFFFF` (raw hex — physical thumb, not a tokened surface) |
| Thumb inset | 4 dp all sides |
| Off track fill — Default | `bg.muted` (#E8E4F0) |
| Off track fill — Pressed | `bg.subtle` (#F1EDF7) |
| Off track fill — Disabled | `bg.disabled` (#D7D3E0) |
| On track fill — Default | `bg.brand` (#D9008D) |
| On track fill — Pressed | `bg.brand.pressed` |
| On track fill — Disabled | `bg.disabled` (#D7D3E0) |

> **Switch thumb is the only place raw `#FFFFFF` ships in this skill.** The thumb reads as a physical affordance (a real switch nub), not a tokened surface. Everywhere else use element-first tokens.

---

## 6. Universal rules

1. **No label / helper / row chrome inside the control.** Labels, supporting text, padding, ripple are owned by `wiom-list-item` (rows) or the inline parent (Acknowledge CTA). The selection control composable renders the indicator and stops.
2. **One size per control.** Don't expose `size` parameters or accept `Modifier.size(…)` overrides. The 24 dp / 52 × 32 dp dimensions are the contract.
3. **`stroke.strong` on unselected default.** Small controls need the heavier weight to be readable on a 360 dp screen at arm's length. Don't drop to `stroke.default` or `stroke.subtle` for the unselected default.
4. **`stroke-medium` (2 dp), not `stroke-small` (1 dp).** The indicator stroke is intentionally heavier than card / divider strokes elsewhere in the system — it's a tap-target affordance.
5. **Filled = no stroke.** When the control is in any filled state (Selected, Indeterminate, Switch On), drop the stroke entirely. Foundations cross-rule: shadow XOR border, and filled controls own contrast through fill.
6. **Selected + Error swaps `bg.brand` → `bg.critical`.** The glyph stays `icon.inverse`. Applies to Checkbox and Radio. Switch has no Error state.
7. **No Hover / Focused / Error variants beyond the matrix.** Keyboard focus uses `stroke.brand.focus` (a ring, not a recolour). Switch has no Error. Don't invent missing states.
8. **Switch thumb is white, always — and it doesn't move on press.** Thumb position changes on selection (Off ↔ On), not on press. Pressed state changes the track fill only.
9. **Pre-checking consent is banned.** Consent (T&C, auto-debit, marketing) checkboxes start `Selection = No`. The user must actively opt in.
10. **Standalone Radio is not a valid pattern.** Always part of a group of 2+ mutually-exclusive options.
11. **Switch is for instant-apply only.** If the change requires a "Save" tap to take effect, use a Checkbox. (Switches that wait for submission are a pattern bug.)
12. **Selection eases; the value doesn't wait.** Fill, stroke, glyph and switch track colours change with `WiomMotion.selection()` (200 ms, `FastOutSlowInEasing`, foundations § Motion) in both directions. The thumb slides on the same 200 ms spec. Pressed colours snap. The callback fires on tap, never after the animation.
13. **Tokens only — element-first.** No raw hex except the Switch thumb's `#FFFFFF`. No legacy `color.brand.primary`.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomCheckboxState     { Default, Disabled, Error }
enum class WiomCheckboxSelection { No, Indeterminate, Selected }

enum class WiomRadioState        { Default, Disabled, Error }

enum class WiomSwitchState       { Default, Disabled }   // Pressed is runtime, not exposed

@Composable
fun WiomCheckbox(
    selection: WiomCheckboxSelection,
    state: WiomCheckboxState = WiomCheckboxState.Default,
    onToggle: (() -> Unit)? = null,                       // null when state == Disabled or row-owned
    modifier: Modifier = Modifier,                        // alignment / padding from parent only — no size overrides
)

@Composable
fun WiomRadio(
    selected: Boolean,
    state: WiomRadioState = WiomRadioState.Default,
    onSelect: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomSwitch(
    checked: Boolean,
    state: WiomSwitchState = WiomSwitchState.Default,
    onCheckedChange: ((Boolean) -> Unit)? = null,
    modifier: Modifier = Modifier,
)
```

> **Size is not exposed.** Each control has one size. The `modifier` parameter accepts alignment / padding from the parent only; don't apply `Modifier.size(…)` or `Modifier.height/width(…)` from a call site — those would distort the indicator.

### `WiomCheckbox` impl

```kotlin
@Composable
fun WiomCheckbox(
    selection: WiomCheckboxSelection,
    state: WiomCheckboxState = WiomCheckboxState.Default,
    onToggle: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val interactionSource = remember { MutableInteractionSource() }
    val isFocused by interactionSource.collectIsFocusedAsState()
    val colors = rememberCheckboxColors(state, selection)
    val shape = RoundedCornerShape(4.dp)                              // radius-tiny
    // §6 rule 12: fill and stroke cross-fade; filled = stroke fades to Transparent (rule 5).
    val fill by animateColorAsState(colors.fill, WiomMotion.selection(), label = "CheckboxFill")
    val stroke by animateColorAsState(colors.stroke, WiomMotion.selection(), label = "CheckboxStroke")

    Box(
        modifier = modifier
            .size(24.dp)
            .focusRing(focused = isFocused, shape = shape)            // §6 rule 7 — see helper below
            .background(fill, shape)
            .border(BorderStroke(2.dp, stroke), shape)
            .then(
                if (state != WiomCheckboxState.Disabled && onToggle != null)
                    Modifier.clickable(
                        interactionSource = interactionSource,
                        indication = LocalIndication.current,
                        onClick = onToggle,
                    )
                else Modifier
            ),
        contentAlignment = Alignment.Center,
    ) {
        // Material `Icons.Rounded.Check / Remove` ship intrinsic ~2 dp padding inside their
        // 24 dp container, so a 24 dp glyph centred in a 24 dp Box renders at the right visual
        // density (~20 dp visible) without manual sizing.
        // Glyph fades in / out with the fill (no draw-on, no scale).
        Crossfade(targetState = selection, animationSpec = WiomMotion.selection(), label = "CheckboxGlyph") { sel ->
            when (sel) {
                WiomCheckboxSelection.No -> Unit
                WiomCheckboxSelection.Indeterminate -> Icon(
                    imageVector = Icons.Rounded.Remove,
                    contentDescription = null,
                    tint = rememberCheckboxColors(state, sel).glyph!!,
                )
                WiomCheckboxSelection.Selected -> Icon(
                    imageVector = Icons.Rounded.Check,
                    contentDescription = null,
                    tint = rememberCheckboxColors(state, sel).glyph!!,
                )
            }
        }
    }
}

@Composable
private fun rememberCheckboxColors(
    state: WiomCheckboxState,
    selection: WiomCheckboxSelection,
): CheckboxColors {
    val isFilled = selection != WiomCheckboxSelection.No
    return when (state) {
        WiomCheckboxState.Default  -> if (isFilled)
            CheckboxColors(fill = Wiom.bg.brand,    stroke = Color.Transparent, glyph = Wiom.icon.inverse)
        else
            CheckboxColors(fill = Color.Transparent, stroke = Wiom.stroke.strong, glyph = null)
        WiomCheckboxState.Disabled -> if (isFilled)
            CheckboxColors(fill = Wiom.bg.disabled,  stroke = Color.Transparent, glyph = Wiom.icon.disabled)
        else
            CheckboxColors(fill = Color.Transparent, stroke = Wiom.stroke.subtle, glyph = null)
        WiomCheckboxState.Error    -> if (isFilled)
            CheckboxColors(fill = Wiom.bg.critical,  stroke = Color.Transparent, glyph = Wiom.icon.inverse)
        else
            CheckboxColors(fill = Color.Transparent, stroke = Wiom.stroke.critical.focus, glyph = null)
    }
}

private data class CheckboxColors(val fill: Color, val stroke: Color, val glyph: Color?)
```

### `WiomRadio` impl

```kotlin
@Composable
fun WiomRadio(
    selected: Boolean,
    state: WiomRadioState = WiomRadioState.Default,
    onSelect: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val interactionSource = remember { MutableInteractionSource() }
    val isFocused by interactionSource.collectIsFocusedAsState()
    val colors = rememberRadioColors(state, selected)
    // §6 rule 12: fill and ring cross-fade in place.
    val fill by animateColorAsState(colors.fill, WiomMotion.selection(), label = "RadioFill")
    val stroke by animateColorAsState(colors.stroke, WiomMotion.selection(), label = "RadioStroke")

    Box(
        modifier = modifier
            .size(24.dp)
            .focusRing(focused = isFocused, shape = CircleShape)
            .background(fill, CircleShape)
            .border(BorderStroke(2.dp, stroke), CircleShape)
            .then(
                if (state != WiomRadioState.Disabled && onSelect != null)
                    Modifier.clickable(
                        interactionSource = interactionSource,
                        indication = LocalIndication.current,
                        onClick = onSelect,
                    )
                else Modifier
            ),
        contentAlignment = Alignment.Center,
    ) {
        // Dot fades in / out (no scale).
        AnimatedVisibility(
            visible = selected,
            enter = fadeIn(WiomMotion.selection()),
            exit = fadeOut(WiomMotion.selection()),
        ) {
            Box(
                modifier = Modifier
                    .size(10.dp)                                      // inner dot
                    .background(rememberRadioColors(state, true).dot!!, CircleShape),
            )
        }
    }
}

@Composable
private fun rememberRadioColors(state: WiomRadioState, selected: Boolean): RadioColors = when (state) {
    WiomRadioState.Default  -> if (selected)
        RadioColors(fill = Wiom.bg.brand,    stroke = Color.Transparent, dot = Wiom.icon.inverse)
    else
        RadioColors(fill = Color.Transparent, stroke = Wiom.stroke.strong, dot = null)
    WiomRadioState.Disabled -> if (selected)
        RadioColors(fill = Wiom.bg.disabled,  stroke = Color.Transparent, dot = Wiom.icon.disabled)
    else
        RadioColors(fill = Color.Transparent, stroke = Wiom.stroke.subtle, dot = null)
    WiomRadioState.Error    -> if (selected)
        RadioColors(fill = Wiom.bg.critical,  stroke = Color.Transparent, dot = Wiom.icon.inverse)
    else
        RadioColors(fill = Color.Transparent, stroke = Wiom.stroke.critical.focus, dot = null)
}

private data class RadioColors(val fill: Color, val stroke: Color, val dot: Color?)
```

### `WiomSwitch` impl

```kotlin
@Composable
fun WiomSwitch(
    checked: Boolean,
    state: WiomSwitchState = WiomSwitchState.Default,
    onCheckedChange: ((Boolean) -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val interactionSource = remember { MutableInteractionSource() }
    val pressed by interactionSource.collectIsPressedAsState()
    val isFocused by interactionSource.collectIsFocusedAsState()
    val colors = rememberSwitchColors(state, checked, pressed)
    // §6 rule 12: track colour eases on selection; pressed colour snaps (instant feedback).
    val trackSpec: AnimationSpec<Color> = if (pressed) snap() else WiomMotion.selection()
    val trackFill by animateColorAsState(colors.trackFill, trackSpec, label = "SwitchTrackFill")
    val trackStroke by animateColorAsState(colors.trackStroke, trackSpec, label = "SwitchTrackStroke")

    // Track inner travel = 52 − 24 (thumb) − 4 × 2 (insets) = 20 dp.
    // Thumb x-offset: 0 dp when Off, 20 dp when On. 200 ms glide, same spec as every selection change.
    val thumbOffsetX by animateDpAsState(
        targetValue = if (checked) 20.dp else 0.dp,
        animationSpec = WiomMotion.selection(),
        label = "WiomSwitchThumbOffset",
    )

    Box(
        modifier = modifier
            .size(width = 52.dp, height = 32.dp)
            .focusRing(focused = isFocused, shape = CircleShape)
            .background(trackFill, CircleShape)
            .border(BorderStroke(2.dp, trackStroke), CircleShape)       // Transparent when On (rule 5)
            .then(
                if (state != WiomSwitchState.Disabled && onCheckedChange != null)
                    Modifier.clickable(
                        interactionSource = interactionSource,
                        indication = null,                            // no ripple — switches are instant-apply
                        onClick = { onCheckedChange(!checked) },
                    )
                else Modifier
            )
            .padding(4.dp),                                            // 4 dp inset all sides
        contentAlignment = Alignment.CenterStart,
    ) {
        Box(
            modifier = Modifier
                .offset(x = thumbOffsetX)
                .size(24.dp)
                .background(Color.White, CircleShape),                 // physical thumb — raw white by design
        )
    }
}

@Composable
private fun rememberSwitchColors(
    state: WiomSwitchState,
    checked: Boolean,
    pressed: Boolean,
): SwitchColors = when (state) {
    WiomSwitchState.Default  -> when {
        checked && pressed  -> SwitchColors(trackFill = Wiom.bg.brand.pressed, trackStroke = Color.Transparent)
        checked             -> SwitchColors(trackFill = Wiom.bg.brand,         trackStroke = Color.Transparent)
        !checked && pressed -> SwitchColors(trackFill = Wiom.bg.subtle,        trackStroke = Wiom.stroke.strong)
        else                -> SwitchColors(trackFill = Wiom.bg.muted,         trackStroke = Wiom.stroke.strong)
    }
    WiomSwitchState.Disabled -> if (checked)
        SwitchColors(trackFill = Wiom.bg.disabled, trackStroke = Color.Transparent)
    else
        SwitchColors(trackFill = Wiom.bg.disabled, trackStroke = Wiom.stroke.subtle)
}

private data class SwitchColors(val trackFill: Color, val trackStroke: Color)

// Shared focus-ring modifier — used by all three controls.
// Renders a 2 dp `stroke.brand.focus` ring 2 dp outside the indicator (radius = indicator
// radius + 2 dp) only while focused. Hard edge — no blur. Used for keyboard a11y.
@Composable
private fun Modifier.focusRing(focused: Boolean, shape: Shape): Modifier {
    if (!focused) return this
    val ringColor = Wiom.stroke.brand.focus
    return this
        .padding(2.dp)                                                // 2 dp gap between ring and indicator
        .border(BorderStroke(2.dp, ringColor), shape)
        .padding(2.dp)                                                // restore the 2 dp inset for the indicator
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material Checkbox (not Wiom-tokened), label baked into the control,
//             custom size override
Checkbox(
    checked = isChecked,
    onCheckedChange = setChecked,
    modifier = Modifier.size(20.dp),                                  // never override; size is the contract
)
Text("I agree to terms")                                              // label outside the row contract

// ✅ RIGHT — WiomCheckbox renders only the indicator; row + label come from wiom-list-item
WiomListItem(
    type = WiomListItemType.Checkbox,
    headline = "I agree to terms",
    supportingText = "You can review them anytime in settings.",
    leadingContent = {
        WiomCheckbox(
            selection = if (isChecked) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
            onToggle = { setChecked(!isChecked) },
        )
    },
    onClick = { setChecked(!isChecked) },                              // row owns the tap
)
```

---

## 8. Recipes

### Consent (Acknowledge CTA inline)

```kotlin
// The Acknowledge CTA from wiom-cta wraps WiomCheckbox inline. The CTA owns the
// label ("I agree to terms"); the selection control renders only the indicator.
WiomAcknowledgeCta(
    label = "I agree to the Terms & Privacy Policy",
    checked = agreed,
    onCheckedChange = { agreed = it },
)
```

### Multi-select list (5 plans)

```kotlin
val plans = remember { listOf(/* Plan(...) × 5 */) }
val selected = remember { mutableStateMapOf<String, Boolean>() }

LazyColumn {
    items(plans) { plan ->
        WiomListItem(
            type = WiomListItemType.Checkbox,
            headline = plan.name,
            supportingText = "₹${plan.price} / month",
            leadingContent = {
                WiomCheckbox(
                    selection = if (selected[plan.id] == true)
                        WiomCheckboxSelection.Selected
                    else WiomCheckboxSelection.No,
                    onToggle = { selected[plan.id] = selected[plan.id] != true },
                )
            },
            onClick = { selected[plan.id] = selected[plan.id] != true },
        )
    }
}
```

### Tri-state parent (hierarchical multi-select)

```kotlin
val childSelection = /* derived from children */
val parentSelection = when {
    childSelection.all { it }  -> WiomCheckboxSelection.Selected
    childSelection.none { it } -> WiomCheckboxSelection.No
    else                        -> WiomCheckboxSelection.Indeterminate
}

WiomListItem(
    type = WiomListItemType.Checkbox,
    headline = "All plans",
    leadingContent = {
        WiomCheckbox(
            selection = parentSelection,
            onToggle = { toggleAllChildren() },                       // selects all if any unchecked
        )
    },
)
```

### Plan selection (Radio group)

```kotlin
val options = listOf("Monthly", "Quarterly", "Annual")
var picked by remember { mutableStateOf(0) }

Column {
    options.forEachIndexed { i, label ->
        WiomListItem(
            type = WiomListItemType.Radio,
            headline = label,
            leadingContent = {
                WiomRadio(
                    selected = i == picked,
                    onSelect = { picked = i },
                )
            },
            onClick = { picked = i },
        )
    }
}
```

### Instant-apply setting (Switch)

```kotlin
var pushEnabled by remember { mutableStateOf(true) }

WiomListItem(
    type = WiomListItemType.Switch,
    headline = "Push notifications",
    supportingText = "Get notified about plan renewals and offers.",
    trailingContent = {
        WiomSwitch(
            checked = pushEnabled,
            onCheckedChange = { pushEnabled = it },                   // applies instantly — no Save needed
        )
    },
    onClick = { pushEnabled = !pushEnabled },                          // row tap also toggles
)
```

### Form validation (consent error)

```kotlin
var agreed by remember { mutableStateOf(false) }
var submitted by remember { mutableStateOf(false) }
val checkboxState = if (submitted && !agreed)
    WiomCheckboxState.Error
else WiomCheckboxState.Default

WiomListItem(
    type = WiomListItemType.Checkbox,
    headline = "I agree to the Terms",
    supportingText = if (checkboxState == WiomCheckboxState.Error)
        "You must agree to continue."
    else null,
    leadingContent = {
        WiomCheckbox(
            selection = if (agreed) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
            state = checkboxState,
            onToggle = { agreed = !agreed },
        )
    },
)
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Instant colour swap on selection, or a bounce / scale / draw-on tick. Colours ease on `WiomMotion.selection()` (§6 rule 12).
- ❌ Delaying `onToggle` / `onCheckedChange` until the animation ends. The value commits on tap.
- ❌ Material `Checkbox` / `RadioButton` / `Switch` directly — wrong tokens, wrong sizing, wrong stroke weight.
- ❌ Pre-checking consent on first render — must start `No`; user actively opts in.
- ❌ Standalone Radio without a peer group — wrong control; use Checkbox or a confirmation dialog.
- ❌ Switch in a form that requires a Save tap — switches are instant-apply only; use Checkbox.
- ❌ Re-implementing the indicator with raw `Box` + `Text("✓")` — always use `WiomCheckbox` / `WiomRadio` / `WiomSwitch`.
- ❌ Custom thumb fill on Switch (anything other than `#FFFFFF`) — the thumb is a physical affordance.
- ❌ Animating the thumb on press (it should only move on selection change). Compose ripple is also banned on the switch — disable indication via `indication = null` per the impl.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Selection Controls canvas: `3069:5843`.
Re-pull `get_variable_defs` on each component set if foundations token names ever change.

| Node | Component / variant |
|---|---|
| `1223:100` | Checkbox component set — 9 ship variants |
| `1221:8` · `1223:30` · `3063:10145` | Checkbox · `Default × No` · `Default × Selected` · `Default × Indeterminate` |
| `1223:16` · `1223:58` · `3063:10147` | Checkbox · Error × No / Selected / Indeterminate |
| `3063:10143` · `1223:51` · `1223:23` | Checkbox · Disabled × No / Selected / Indeterminate |
| `1400:2` | Radio component set — 6 ship variants |
| `1398:2` · `1398:32` · `1398:20` · `1398:50` · `1398:26` · `1398:56` | Radio variants in row order (Default · Disabled · Error) × (No · Yes) |
| `1613:38` | Switch component set — 6 ship variants |
| `1613:2` · `3063:10352` · `3063:10487` | Switch · `Off` × Default / Pressed / Disabled |
| `1613:20` · `3063:10502` · `1613:26` | Switch · `On` × Default / Pressed / Disabled |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The Figma component description for **Checkbox** says *"Supports Unchecked and Checked selection states"* — but the ship has **three selection states**: `No`, `Indeterminate`, `Selected`. The description is outdated; trust the variants. Indeterminate is a V2 addition for hierarchical multi-select trees.
- The Figma component description for **Radio** says *"V1: Medium size only"* — V2 ships **one size only** (24 dp); there is no size variant. The description is leftover V1 language.
- The **bounding box is 24 × 24 dp** for Checkbox and Radio. Older skill drafts mentioned "20 × 20 dp" — that referred to the visible glyph inside a 24 dp container. Trust the 24 dp ship dimension as the layout-impacting size; the indicator's visual figure renders within it.
- **Switch ships a Pressed state** (V2 addition). It's runtime-driven from `InteractionSource`, not authored at the call site. The track fill steps one level on press (`bg.muted` → `bg.subtle` for Off; `bg.brand` → `bg.brand.pressed` for On); the thumb does **not** move on press — only on selection change.
- **Switch has no Error state.** Switches are instant-apply settings; no form submission to validate against. Don't fabricate one.
- **The `Selected + Error` fill swap** (`bg.brand` → `bg.critical`) applies to Checkbox and Radio. Glyph stays `icon.inverse` regardless. The unselected error path uses `stroke.critical.focus` (#D92130) on the outline — foundations does not expose a non-`.focus` `stroke.critical` token; the `.focus` variant is the canonical error-border token.
- **The thumb is `#FFFFFF`** — not a tokened surface. It reads as a physical switch nub, and ships raw white on every track tone (including `bg.disabled` greys). This is the only place this skill ships raw hex.
- The on-canvas frame `3063:10031` (`icon_with_bg`) is **not a selection control** — it's a stray instance from `wiom-icon-badge`. Ignore it for this skill.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.brand` / `bg.brand.pressed` / `bg.critical` / `bg.disabled` / `bg.muted` / `bg.subtle`, `stroke.strong` / `stroke.subtle` / `stroke.critical.focus` / `stroke.brand.focus`, `icon.inverse` / `icon.disabled`, `radius-tiny` / `radius-full`.
- `wiom-list-item` — the row component that owns the label, supporting text, and tap target around these controls. Multi-select rows, settings rows, language pickers, payment-method pickers all use this pattern.
- `wiom-cta` — the **Acknowledge** CTA inlines a `WiomCheckbox` for consent flows; the CTA owns the label, the control owns the indicator.
- `wiom-input-fields` — when you need an input + selection together (e.g., "Other" radio + free-text field), pair this skill with input-fields rather than re-implementing inside the control.
- `wiom-tabs-filters` — for chip / filter UIs, **not** these. Tabs and filters have their own anatomy.
- `wiom-bottomsheet` — for picker-style single-select with > 5 options (longer than a dialog can hold), use a bottom-sheet picker instead of a long Radio group.
- `wiom-compose-layout-rules` — Modifier discipline; no explicit `size(...)` on these controls; `Icons.Rounded.*` enforcement.
