---
name: wiom-input-fields
description: >
  Wiom Input / Textarea / OTP — three text-entry components. 3
  components, 20 ship variants. Reference for generating Jetpack
  Compose code with WiomInput / WiomTextarea / WiomOtp and for
  instancing the Figma component sets (Field/Input `1422:112`,
  Field/Textarea `1426:47`, Field/OTP `1790:740`) via MCP. Use this
  skill EVERY TIME you write, edit, or review any form, OTP entry,
  search bar, dropdown trigger, address field, or any field that
  captures user text in the Wiom Partner or Customer Android app, or
  when assembling Figma frames with text fields. Trigger on: Input,
  TextField, TextInput, OutlinedTextField, Textarea, multiline,
  search bar, OTP, verification code, mobile number, password,
  currency / amount, dropdown trigger, "₹", placeholder, helper
  text, counter, label, validation, error, focused, read-only.
---

# Wiom Input Fields

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
sets `1422:112` (Input), `1426:47` (Textarea), `1790:740` (OTP), and
the `wiom-design-foundations` token set.

**3 components · 20 ship variants · element-first tokens.**

| Component | States | Variants |
|---|---|---|
| Field / Input | 8 | Default · Focused · Filled · Error · Success · Warning · Disabled · Read-only |
| Field / Textarea | 6 (+2) | Default · Focused · Filled · Error · Success · Disabled; **Warning and Read-only also allowed**, same rules as Input (owner decision 27 Nov 2026, same on iOS, D59) |
| Field / OTP | 6 | Default · Focused · Filled · Error · Success · Disabled (no Warning, no Read-only) |

> **Search bar is Input**, not a separate component — it's a
> configuration (leading `search` + trailing `cancel`). **Dropdown
> trigger is Input** in `Read-only` state with a chevron + a
> bottom-sheet on tap. The deprecated `wiom-dropdown` skill is gone;
> its trigger anatomy lives here, its menu UX in `wiom-bottomsheet`.

---

## 1. Figma — instancing the components

Each component set is independently instanceable. Strings are
**case-sensitive**. The 16 / 8 / 6 properties below combine the State
variant with booleans + text + instance-swap to cover every shipping
permutation.

### Field / Input — `1422:112` (16 props)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Default` | `Default` · `Focused` · `Filled` · `Error` · `Success` · `Warning` · `Disabled` · `Read-only` |
| `Title`, `Show Title` | Text, Boolean | `"Label"`, `true` | Field label above the wrap. Hide only when externally labelled by a parent section. |
| `Body` | Text | `"Enter your value"` | Value or placeholder text inside the wrap |
| `Show Leading Icon`, `Leading Icon` | Boolean, Instance swap | `false`, `search` | Reveal + pick a 20 dp Material Symbols Rounded icon |
| `Show Prefix`, `Prefix` | Boolean, Text | `false`, `"₹"` | Affix before the value (e.g. `₹`). Mobile numbers take none (D47). `text.subtle` |
| `Show Suffix`, `Suffix` | Boolean, Text | `false`, `".00"` | Affix after the value (e.g. `.00`, `kg`) — `text.subtle` |
| `Show Trailing Icon`, `Trailing Icon` | Boolean, Instance swap | `false`, `cancel` | Reveal + pick a 20 dp icon. **Hardcoded for status states** — Error/Success/Warning win over user-set trailing icon. |
| `Show Helper`, `Helper` | Boolean, Text | `true`, `"Helper text"` | Helper / validation message under the wrap |
| `Show Counter`, `Counter` | Boolean, Text | `false`, `"0 / 100"` | Right-aligned char count in the helper row |

### Field / Textarea — `1426:47` (8 props)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Default` | 6 values (no Warning, no Read-only) |
| `Title`, `Show Title` | Text, Boolean | `"Label"`, `true` | Same rule as Input |
| `Body` | Text | `"Placeholder text…"` | Multi-line value / placeholder |
| `Show Helper`, `Helper` | Boolean, Text | `true`, `"Helper text"` | Helper row |
| `Show Counter`, `Counter` | Boolean, Text | `false`, `"0 / 200"` | Char count |

No icon slots, no prefix/suffix.

### Field / OTP — `1790:740` (6 props)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Default` | 6 values (no Warning, no Read-only) |
| `6-digit` | Boolean | `false` (4 boxes default) | `true` = 6 boxes (~48 dp each); `false` = 4 boxes (~76 dp each). Container stays 328 dp wide; boxes split via `weight(1f)`. |
| `Title`, `Show Title` | Text, Boolean | `"Enter OTP"`, `true` | Same rule |
| `Show Helper`, `Helper` | Boolean, Text | `true`, e.g. `"Sent to 98765 43210"` | Helper / resend prompt |

> **Focused-state timer is hardcoded.** The Focused variant ships with
> a visible **timer (e.g. `00:24`)** at the right of the helper row.
> You don't toggle or theme it — it's part of the Focused appearance.
> Don't recreate it as a separate counter / chip on top of the field.

### Code enum ↔ Figma variant string

| `FieldState` (Kotlin) | Figma `State` value | Available on |
|---|---|---|
| `Default` | `Default` | Input · Textarea · OTP |
| `Focused` | `Focused` | Input · Textarea · OTP |
| `Filled` | `Filled` | Input · Textarea · OTP |
| `Error` | `Error` | Input · Textarea · OTP |
| `Success` | `Success` | Input · Textarea · OTP |
| `Warning` | `Warning` | Input only |
| `Disabled` | `Disabled` | Input · Textarea · OTP |
| `ReadOnly` | `Read-only` (with hyphen) | Input only |

---

## 2. Anatomy

All three components share the same outer column structure: **Title →
wrap → Helper row**, gap `space-8` (8 dp) between rows.

### Input

```
Title (type.title.sm empty → type.body.lg focused/filled, D50; optional via Show Title)
   ↓ space-8
┌─── wrap (radius-medium, padding 16H/12V, border per state) ────┐
│  ●Lead    Prefix    Body / placeholder   Suffix    ●Trail      │
│  20dp                                                20dp       │
└──── gap between every adjacent slot: space-12 ─────────────────┘
   ↓ space-8
┌─── helper row ────────────────────────────────────────────────┐
│ Helper text (type.body.md)                          0 / 100  │
└────────────── helper ↔ counter gap: space-12 ────────────────┘
```

| Slot | Value | Token |
|---|---|---|
| Container width | Derived — `fillMaxWidth()` (host owns the gutter) | — |
| Container height | Derived — Title 24 LH + space-8 + wrap (12 V + 24 LH + 12 V) + space-8 + helper 20 LH | — |
| Wrap radius | 12 dp | `radius-medium` |
| Wrap horizontal padding | 16 dp | `space-16` |
| Wrap vertical padding | 12 dp | `space-12` |
| Wrap inner gap (slot ↔ slot) | 12 dp | `space-12` |
| Vertical stack gap (Title ↔ wrap ↔ helper) | 8 dp | `space-8` |
| Title style | Empty and not focused: `type.title.sm` (Bold 16/24). Focused, filled or read-only: `type.body.lg` (Regular 16/24). Owner decision 27 Nov 2026, same on iOS (D50) | `type.title.sm` / `type.body.lg` |
| Body / Prefix / Suffix style | Noto Sans Regular 16 / 24 LH | `type.body.lg` |
| Helper / Counter style | Noto Sans Regular 14 / 20 LH | `type.body.md` |
| Icon size (leading / trailing) | 20 dp | `icon.sm` |

### Textarea

Same as Input minus icon / prefix / suffix slots. The wrap accepts
multiple lines; height grows with content. **Inner Row uses
`verticalAlignment = Alignment.Top`** (not `Center`) so the placeholder
sits at the top of the multi-line area, not floating to the vertical
middle of a tall field.

### OTP

```
Title (type.title.sm empty → type.body.lg focused/filled, D50)
   ↓ space-8
┌─── Boxes row (gap space-8 between boxes, fillMaxWidth) ────────┐
│  ┌──┐  ┌──┐  ┌──┐  ┌──┐                  ← 4-digit (default)  │
│  │ 0│  │ 0│  │ 0│  │ 0│                                        │
│  └──┘  └──┘  └──┘  └──┘                                        │
│       OR                                                        │
│  ┌─┐  ┌─┐  ┌─┐  ┌─┐  ┌─┐  ┌─┐            ← 6-digit             │
│  └─┘  └─┘  └─┘  └─┘  └─┘  └─┘                                  │
└─── each box: weight(1f), radius-medium, 12 V/H pad ────────────┘
   ↓ space-8
┌─── helper row ────────────────────────────────────────────────┐
│ Sent to 98765 43210                                  00:24   │  ← timer (Focused only)
└───────────────────────────────────────────────────────────────┘
```

Each box is a square-ish container (height = 12 V + 24 LH + 12 V =
48 dp) that fills width via `weight(1f)`. Boxes don't have fixed
widths — the container's width and the count split it.

| Slot | Value | Token |
|---|---|---|
| Title style | Empty and not focused: `type.title.sm` (Bold 16/24). Focused, filled or read-only: `type.body.lg` (Regular 16/24). Owner decision 27 Nov 2026, same on iOS (D50) | `type.title.sm` / `type.body.lg` |
| **Digit (inside each box)** | **Noto Sans SemiBold 16 / 24 LH** | **`type.label.lg`** |
| Helper / Timer style | Noto Sans Regular 14 / 20 LH | `type.body.md` |
| Box wrap radius | 12 dp | `radius-medium` |
| Box wrap padding | 12 H · 12 V | `space-12` |
| Box-to-box gap | 8 dp | `space-8` |
| Active box border | 2 dp | `stroke.brand.focus` |
| Inactive box border | 1 dp | `stroke.subtle` |

> **Digit typography is `type.label.lg`** — same weight as the Title.
> Don't use `type.body.lg` (Regular weight); the digit needs SemiBold
> presence to read as a discrete entered token, not flowing text.

**No fixed dimensions** on any of the three components. Wrap heights
hug from padding + line-height. Container width = parent surface
margin (`fillMaxWidth()`).

---

## 3. Pick the right component

```
Need to capture user text? Ask:

Single line — text / number / email / password / phone / amount?
   → Input (with optional prefix/suffix/leading/trailing slots)

Search across a list or screen?
   → Input · Show Leading Icon=true · Leading Icon=search
            · Show Trailing Icon=true (only when query present) · Trailing Icon=cancel

Multi-line — address / comment / description / reason / feedback?
   → Textarea (height grows with content)

Verification code (4 or 6 digits)?
   → OTP (boxed digits with auto-advance focus + Focused-state timer)

Single-/Multi-select picker?
   → Input · State=Read-only · Show Trailing Icon=true · Trailing Icon=expand_more
            · onClick opens wiom-bottomsheet picker
```

**Search bar, dropdown trigger, password reveal — all configurations of
Input.** Don't draw custom fields; tokens won't bind.

---

## 4. States

| State | Visual signal | Use when |
|---|---|---|
| **Default** | 1 dp `stroke.subtle` border, Title `text.default` (highlighted) | Empty, unfocused. Pull attention to the Title. |
| **Focused** | 2 dp `stroke.brand.focus` border, Title `text.subtle` | User has tapped in. Title softens — the input bar carries the focus. |
| **Filled** | 1 dp `stroke.subtle` border, Title `text.subtle`, Body `text.default` | Content present, not focused. The value speaks for itself. |
| **Error** | 2 dp `stroke.critical.focus` border, error icon (trailing), helper `text.critical` | Validation failed. Helper **must** explain what's wrong. |
| **Success** | 2 dp `stroke.positive.focus` border, check icon (trailing), helper `text.positive` | Validated OK (e.g., OTP matched, mobile confirmed). Use sparingly. |
| **Warning** (Input only) | 2 dp `stroke.warning` border, warning icon (trailing), helper `text.onWarning` | Value is valid but risky / unusual. **Not** a substitute for Error. |
| **Disabled** | 1 dp `stroke.subtle`, fill `bg.disabled`, all text `text.disabled` | Field is truly inactive — user cannot edit AND cannot copy. |
| **Read-only** (Input only) | 1 dp `stroke.subtle`, fill `bg.subtle`, Body `text.default`, Title `text.subtle` | Value is displayable + copyable but not editable (Customer ID, account number, dropdown trigger value). **Not the same as Disabled.** |

---

## 5. Tokens per state

All tokens are **element-first**. Always reference tokens, never hex.

| State | Wrap fill | Border (width · color) | Title color | Body color | Helper color | Trailing icon |
|---|---|---|---|---|---|---|
| Default | `bg.default` | 1 dp `stroke.subtle` | `text.default` | `text.disabled` (placeholder) | `text.subtle` | user-set |
| Focused | `bg.default` | 2 dp `stroke.brand.focus` | `text.subtle` | `text.default` | `text.subtle` | user-set |
| Filled | `bg.default` | 1 dp `stroke.subtle` | `text.subtle` | `text.default` | `text.subtle` | user-set |
| Error | `bg.default` | 2 dp `stroke.critical.focus` | `text.subtle` | `text.default` | `text.critical` | hardcoded `error` · `icon.critical` |
| Success | `bg.default` | 2 dp `stroke.positive.focus` | `text.subtle` | `text.default` | `text.positive` | hardcoded `check_circle` · `icon.positive` |
| Warning | `bg.default` | 2 dp `stroke.warning` | `text.subtle` | `text.default` | `text.onWarning` | hardcoded `warning` · `icon.action` on a `bg.warning.subtle` circle (gold is ratings-only) |
| Disabled | `bg.disabled` | 1 dp `stroke.subtle` | `text.disabled` | `text.disabled` | `text.disabled` | dimmed |
| Read-only | `bg.subtle` | 1 dp `stroke.subtle` | `text.subtle` | `text.default` | `text.subtle` | typically `expand_more` for dropdown |

> Aligned with the shipped library and iOS skills, 26 Nov 2026: Read-only title `text.subtle` (`WiomInput.kt:226`), leading icon `icon.nonAction` (`:254`), OTP status borders on every resting box (`WiomOtp.kt:95-142`).

> **Warning is special.** White-on-`bg.warning` fails AA, but Warning's
> message text in this skill sits on `bg.default` (not `bg.warning`).
> Helper text uses `text.onWarning` (#372902 olive, **not** a
> nonexistent `text.warning`). Foundations rule: `text.onWarning` is
> the only text token paired with the warning family on every surface
> (`bg.warning`, `bg.warning.subtle`, `bg.default` — all pass AA).

> **Status icons are hardcoded.** When State = Error / Success /
> Warning, the trailing slot renders the matching status icon and
> overrides any user-set `Trailing Icon`. This is a Figma constraint;
> don't fight it.

---

## 6. Surface contract

| Placement | What the surface (parent) owns | What the field does |
|---|---|---|
| Inside screen content | `space-16` start / end gutter via screen padding | `fillMaxWidth()` — width emerges as 328 dp on a 360 dp screen |
| Inside dialog | `space-24` via dialog padding | `fillMaxWidth()` |
| Inside bottom sheet | `space-16` via sheet padding | `fillMaxWidth()` |
| Inside a per-step screen / sheet | `space-16` via step content padding | `fillMaxWidth()` |

**No fixed dimensions** on any field component.

- ❌ `Modifier.width(...)` / `Modifier.height(...)` / `heightIn(...)` /
  `widthIn(...)` / `Modifier.size(...)` on the field container or the
  wrap. Width = `fillMaxWidth()`; height = derived from padding +
  line-height (Textarea grows with content).

---

## 7. Universal rules

1. **Label ramp: weight and colour change together (D50).** Empty and
   not focused: `type.title.sm` (Bold) in `text.default`. Focused,
   filled or read-only: `type.body.lg` (Regular) in `text.subtle`.
   Disabled: same weight rule, `text.disabled`. Don't override
   per-instance. Owner decision 27 Nov 2026, same on iOS (D50).
2. **Error helper must be actionable.** Don't ship `Helper="Helper
   text"` in production — specify what's wrong and how to fix it.
3. **Affixes go in the Prefix / Suffix slot.** Never type `₹` into
   Body: it breaks validation, copy/paste, and semantic meaning.
   **Mobile numbers take no prefix:** only Indian numbers can sign up
   or log in, so the field is 10 digits starting 6 to 9, number keypad,
   no `+91` and no phone icon (owner decision 27 Nov 2026, same on iOS, D47).
4. **Read-only ≠ Disabled.** Read-only displays values the user can
   read and copy (text stays `text.default`). Disabled is for truly
   inactive fields (`text.disabled`, `bg.disabled`).
5. **One trailing icon at a time.** Status icons (Error / Success /
   Warning) take over the trailing slot. Don't stack.
6. **Keep Title visible by default.** Hide only when a parent section
   header already labels the field.
7. **No fixed dimensions.** Width = `fillMaxWidth()`; height = derived
   from padding + line-height. Textarea grows with content.
8. **Textarea alignment is `Alignment.Top`, not `Center`.** Inner Row
   `verticalAlignment = Alignment.Top` so placeholder sits at the top
   of the multi-line area; placeholder/value Box uses
   `Alignment.TopStart`.
9. **Counter only where it aids the user.** Char limits on textareas,
   chat drafts. Don't show on short free-text fields. OTP has its own
   built-in timer — never stack a counter on top.
10. **OTP is its own component.** Don't fake 4/6-digit entry with a
    single Input + spaces in Body — OTP has correct per-digit focus,
    box sizing, and timer.
11. **Status icons are hardcoded.** Don't pass a custom `Trailing
    Icon` for Error / Success / Warning states.
12. **Tokens only — element-first.** No raw hex, no legacy
    `color.brand.primary` / `color.text.primary`.

---

## 8. Compose API + code

### Public API

```kotlin
enum class FieldState {
    Default, Focused, Filled, Error, Success, Warning, Disabled, ReadOnly,
}

@Composable
fun WiomInput(
    value: String,
    onValueChange: (String) -> Unit,
    title: String,
    state: FieldState = FieldState.Default,            // Focused is auto-derived if you also provide a focusRequester; pass explicit only for non-interactive previews
    placeholder: String? = null,
    helper: String? = null,
    counter: String? = null,                           // pass when char limit is meaningful
    leadingIcon: ImageVector? = null,
    trailingIcon: ImageVector? = null,                 // ignored for Error/Success/Warning (hardcoded)
    prefix: String? = null,
    suffix: String? = null,
    keyboardOptions: KeyboardOptions = KeyboardOptions.Default,
    keyboardActions: KeyboardActions = KeyboardActions.Default,
    visualTransformation: VisualTransformation = VisualTransformation.None,  // password etc.
    onClick: (() -> Unit)? = null,                     // set for Read-only dropdown trigger
    modifier: Modifier = Modifier,
)

@Composable
fun WiomTextarea(
    value: String,
    onValueChange: (String) -> Unit,
    title: String,
    state: FieldState = FieldState.Default,            // 6 states (no Warning, no ReadOnly)
    placeholder: String? = null,
    helper: String? = null,
    counter: String? = null,
    minLines: Int = 3,
    maxLines: Int = Int.MAX_VALUE,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomOtp(
    value: String,
    onValueChange: (String) -> Unit,                   // length-clamped to digits
    title: String = "Enter OTP",
    state: FieldState = FieldState.Default,            // 6 states
    sixDigit: Boolean = false,                         // false = 4 boxes; true = 6 boxes
    helper: String? = null,                            // e.g. "Sent to 98765 43210" or resend prompt
    timer: String? = null,                             // e.g. "00:24" — shown right of helper in Focused
    modifier: Modifier = Modifier,
)
```

### Token resolver

```kotlin
@Composable
private fun rememberFieldColors(state: FieldState): FieldColors = when (state) {
    FieldState.Default  -> FieldColors(Wiom.bg.default,  border(1.dp, Wiom.stroke.subtle),         Wiom.text.default,  Wiom.text.disabled,   Wiom.text.subtle)
    FieldState.Focused  -> FieldColors(Wiom.bg.default,  border(2.dp, Wiom.stroke.brand.focus),    Wiom.text.subtle,   Wiom.text.default,    Wiom.text.subtle)
    FieldState.Filled   -> FieldColors(Wiom.bg.default,  border(1.dp, Wiom.stroke.subtle),         Wiom.text.subtle,   Wiom.text.default,    Wiom.text.subtle)
    FieldState.Error    -> FieldColors(Wiom.bg.default,  border(2.dp, Wiom.stroke.critical.focus), Wiom.text.subtle,   Wiom.text.default,    Wiom.text.critical)
    FieldState.Success  -> FieldColors(Wiom.bg.default,  border(2.dp, Wiom.stroke.positive.focus), Wiom.text.subtle,   Wiom.text.default,    Wiom.text.positive)
    FieldState.Warning  -> FieldColors(Wiom.bg.default,  border(2.dp, Wiom.stroke.warning),        Wiom.text.subtle,   Wiom.text.default,    Wiom.text.onWarning)
    FieldState.Disabled -> FieldColors(Wiom.bg.disabled, border(1.dp, Wiom.stroke.subtle),         Wiom.text.disabled, Wiom.text.disabled,   Wiom.text.disabled)
    FieldState.ReadOnly -> FieldColors(Wiom.bg.subtle,   border(1.dp, Wiom.stroke.subtle),         Wiom.text.subtle,   Wiom.text.default,    Wiom.text.subtle)
}

@Composable
private fun rememberStatusIcon(state: FieldState): Pair<ImageVector, Color>? = when (state) {
    FieldState.Error   -> Icons.Rounded.Error       to Wiom.icon.critical
    FieldState.Success -> Icons.Rounded.CheckCircle to Wiom.icon.positive
    FieldState.Warning -> Icons.Rounded.Warning     to Wiom.icon.action   // dark, in an Icon Badge Sm (24 dp bg.warning.subtle circle, 16 dp glyph); never gold
    else -> null
}

private data class FieldColors(
    val container: Color,
    val border: BorderStroke,
    val title: Color,
    val body: Color,
    val helper: Color,
)
```

### `WiomInput` implementation

```kotlin
@Composable
fun WiomInput(
    value: String,
    onValueChange: (String) -> Unit,
    title: String,
    state: FieldState,
    placeholder: String?,
    helper: String?,
    counter: String?,
    leadingIcon: ImageVector?,
    trailingIcon: ImageVector?,
    prefix: String?,
    suffix: String?,
    keyboardOptions: KeyboardOptions,
    keyboardActions: KeyboardActions,
    visualTransformation: VisualTransformation,
    onClick: (() -> Unit)?,
    modifier: Modifier,
) {
    val colors = rememberFieldColors(state)
    val statusIcon = rememberStatusIcon(state)
    val effectiveTrailing = statusIcon?.first ?: trailingIcon
    val effectiveTrailingTint = statusIcon?.second ?: Wiom.icon.action

    Column(
        modifier = modifier.fillMaxWidth(),
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
    ) {
        // D50: Bold when empty and not focused; Regular once focused, filled or read-only.
        val titleStyle = if (value.isEmpty() && state == FieldState.Default) Wiom.type.title.sm else Wiom.type.body.lg
        Text(title, style = titleStyle, color = colors.title)

        // Wrap
        Surface(
            color = colors.container,
            shape = RoundedCornerShape(Wiom.radius.medium),
            border = colors.border,
            modifier = Modifier
                .fillMaxWidth()
                .let { if (state == FieldState.ReadOnly && onClick != null) it.clickable { onClick() } else it },
        ) {
            Row(
                modifier = Modifier.padding(horizontal = Wiom.space.s16, vertical = Wiom.space.s12),
                horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
                verticalAlignment = Alignment.CenterVertically,
            ) {
                leadingIcon?.let {
                    Icon(it, contentDescription = null,
                         tint = if (state == FieldState.Disabled) Wiom.icon.disabled else Wiom.icon.nonAction,   // decorative, as shipped
                         modifier = Modifier.size(20.dp))
                }
                prefix?.let {
                    Text(it, style = Wiom.type.body.lg, color = Wiom.text.subtle, maxLines = 1)
                }
                BasicTextField(
                    value = value,
                    onValueChange = onValueChange,
                    enabled = state != FieldState.Disabled && onClick == null,
                    readOnly = state == FieldState.ReadOnly,
                    singleLine = true,
                    textStyle = Wiom.type.body.lg.copy(color = colors.body),
                    cursorBrush = SolidColor(Wiom.text.default),
                    keyboardOptions = keyboardOptions,
                    keyboardActions = keyboardActions,
                    visualTransformation = visualTransformation,
                    decorationBox = { inner ->
                        if (value.isEmpty() && placeholder != null) {
                            Text(placeholder, style = Wiom.type.body.lg, color = Wiom.text.disabled, maxLines = 1)
                        }
                        inner()
                    },
                    modifier = Modifier.weight(1f),
                )
                suffix?.let {
                    Text(it, style = Wiom.type.body.lg, color = Wiom.text.subtle, maxLines = 1)
                }
                effectiveTrailing?.let {
                    Icon(it, contentDescription = null,
                         tint = effectiveTrailingTint, modifier = Modifier.size(20.dp))
                }
            }
        }

        // Helper row
        if (helper != null || counter != null) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
                verticalAlignment = Alignment.CenterVertically,
            ) {
                helper?.let {
                    Text(it, style = Wiom.type.body.md, color = colors.helper, modifier = Modifier.weight(1f))
                }
                counter?.let {
                    Text(it, style = Wiom.type.body.md, color = Wiom.text.subtle, maxLines = 1)
                }
            }
        }
    }
}
```

`WiomTextarea` is the same shape with `singleLine = false`,
`verticalAlignment = Alignment.Top`, and the placeholder Box using
`contentAlignment = Alignment.TopStart`.

### WiomOtp — single `BasicTextField` + `decorationBox` (canonical)

> **Don't use N separate `BasicTextField`s** with manual focus
> chaining. That's the wrong pattern for boxed OTP — paste fails,
> backspace is unreliable, and IME suggestions break. Use **one**
> hidden `BasicTextField` that owns the digit string, and render N
> visual cells inside its `decorationBox`. Active cell = `text.cursor`
> index inside the value; cell styling is pure layout.

```kotlin
@Composable
fun WiomOtp(
    value: String,
    onValueChange: (String) -> Unit,
    title: String = "Enter OTP",
    state: FieldState = FieldState.Default,
    sixDigit: Boolean = false,
    helper: String? = null,
    timer: String? = null,
    modifier: Modifier = Modifier,
) {
    val count = if (sixDigit) 6 else 4
    val colors = rememberFieldColors(state)
    val focusedIndex = value.length.coerceAtMost(count - 1)
    var isFocused by remember { mutableStateOf(false) }

    Column(modifier = modifier.fillMaxWidth(), verticalArrangement = Arrangement.spacedBy(Wiom.space.s8)) {
        val titleStyle = if (value.isEmpty() && !isFocused) Wiom.type.title.sm else Wiom.type.body.lg   // D50
        Text(title, style = titleStyle, color = colors.title)

        BasicTextField(
            value = value,
            onValueChange = { onValueChange(it.filter(Char::isDigit).take(count)) },
            singleLine = true,
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.NumberPassword),
            modifier = Modifier.fillMaxWidth().onFocusChanged { isFocused = it.isFocused },
            decorationBox = {
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
                ) {
                    repeat(count) { i ->
                        // Active box only while editing; on Error / Success every resting box
                        // takes the status border (as shipped, WiomOtp.kt:95-142).
                        val isActive = isFocused && i == focusedIndex && state != FieldState.Disabled
                        val (restWidth, restColor) = when (state) {
                            FieldState.Error   -> 2.dp to Wiom.stroke.critical.focus
                            FieldState.Success -> 2.dp to Wiom.stroke.positive.focus
                            else               -> 1.dp to Wiom.stroke.subtle
                        }
                        val borderWidth = if (isActive) 2.dp else restWidth
                        val borderColor = if (isActive) Wiom.stroke.brand.focus else restColor

                        Box(
                            modifier = Modifier
                                .weight(1f)
                                .heightIn(min = 48.dp)
                                .border(borderWidth, borderColor, RoundedCornerShape(Wiom.radius.medium))
                                .padding(horizontal = Wiom.space.s12, vertical = Wiom.space.s12),
                            contentAlignment = Alignment.Center,
                        ) {
                            Text(
                                text = value.getOrNull(i)?.toString().orEmpty(),
                                style = Wiom.type.label.lg,                   // SemiBold 16/24 — see §6
                                color = colors.body,
                            )
                        }
                    }
                }
            },
        )

        if (helper != null || timer != null) {
            Row(modifier = Modifier.fillMaxWidth(), horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12)) {
                helper?.let { Text(it, style = Wiom.type.body.md, color = colors.helper, modifier = Modifier.weight(1f)) }
                timer?.let  { Text(it, style = Wiom.type.body.md, color = Wiom.text.subtle) }   // Focused state only
            }
        }
    }
}
```

Key properties:

- **One `BasicTextField`** owns the entire digit string. Paste, IME
  autofill, and backspace work without any per-cell focus management.
- **`decorationBox` renders N visual cells** via `repeat(count)` + a
  `Row` of `weight(1f)` boxes. Cells are pure visual chrome.
- **Active cell = next-position cell, only while the field is focused**,
  i.e. cell at `value.length` (clamped to `count - 1`). Active = 2 dp
  `stroke.brand.focus`. Resting cells = 1 dp `stroke.subtle`, or on
  Error / Success every resting cell takes the 2 dp status border
  (`stroke.critical.focus` / `stroke.positive.focus`), as shipped.
- **Digit text style = `type.label.lg`** (SemiBold 16/24) — see §6.

### Common mistake → fix

```kotlin
// ❌ WRONG — affix typed into the value, fixed height, error without helper, body.sm counter
WiomInput(
    value = "+91 9876543210",                          // prefix typed into the value; Indian numbers need none
    onValueChange = { ... },
    title = "Mobile",
    state = FieldState.Error,
    helper = null,                                     // Error must explain what's wrong
    counter = "10 / 10",
    modifier = Modifier.height(56.dp),                 // never hardcode height
)

// ✅ RIGHT: 10 digits, no prefix (D47), helper explains, height derived
WiomInput(
    value = phoneDigits,
    onValueChange = { phoneDigits = it.filter(Char::isDigit).take(10) },
    title = "Mobile number",
    state = if (isValidIndianMobile(phoneDigits)) FieldState.Filled else FieldState.Error,
    helper = if (isValidIndianMobile(phoneDigits)) "We'll send an OTP" else "Enter 10 digits",
    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Phone),
    modifier = Modifier.fillMaxWidth(),
)
```

---

## 9. Configuration patterns

Quick recipes — all property overrides on the right component.

### Mobile number (Indian only, no prefix, D47)

```kotlin
// 10 digits starting 6 to 9. No +91 prefix, no phone icon.
fun isValidIndianMobile(d: String) = d.length == 10 && d.first() in '6'..'9'

WiomInput(
    value = phone, onValueChange = { phone = it.filter(Char::isDigit).take(10) },
    title = "Mobile number",
    helper = "We'll send an OTP",
    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Phone),
    modifier = Modifier.fillMaxWidth(),
)
```

### OTP entry (use the dedicated component, not Input)

```kotlin
WiomOtp(
    value = otp, onValueChange = { otp = it.filter(Char::isDigit).take(6) },
    title = "Enter OTP",
    sixDigit = true,
    state = FieldState.Focused,
    helper = "Sent to 98765 43210",
    timer = "00:24",                                   // Focused state shows timer at the right
    modifier = Modifier.fillMaxWidth(),
)
```

### Search bar

```kotlin
WiomInput(
    value = query, onValueChange = { query = it },
    title = "",                                        // bare search; Show Title=false in Figma
    placeholder = "Search…",
    leadingIcon = Icons.Rounded.Search,
    trailingIcon = if (query.isNotEmpty()) Icons.Rounded.Close else null,
    helper = null,
    modifier = Modifier.fillMaxWidth(),
)
```

### Password (toggle reveal)

```kotlin
var revealed by remember { mutableStateOf(false) }
WiomInput(
    value = password, onValueChange = { password = it },
    title = "Password",
    placeholder = "••••••••",
    trailingIcon = if (revealed) Icons.Rounded.VisibilityOff else Icons.Rounded.Visibility,
    visualTransformation = if (revealed) VisualTransformation.None else PasswordVisualTransformation(),
    modifier = Modifier.fillMaxWidth(),
)
```

### Currency amount (₹ … .00)

```kotlin
WiomInput(
    value = amount, onValueChange = { amount = it.filter { ch -> ch.isDigit() } },
    title = "Recharge amount",
    prefix = "₹",
    suffix = ".00",
    helper = "Includes 18% GST",
    keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
    modifier = Modifier.fillMaxWidth(),
)
```

### Dropdown trigger (Read-only Input + bottom-sheet picker)

```kotlin
var showSheet by remember { mutableStateOf(false) }
WiomInput(
    value = selectedLanguage?.label ?: "",
    onValueChange = {},                                // Read-only
    title = "Language",
    placeholder = "Select language",
    state = FieldState.ReadOnly,
    trailingIcon = Icons.Rounded.KeyboardArrowDown,
    onClick = { showSheet = true },
    modifier = Modifier.fillMaxWidth(),
)
if (showSheet) {
    WiomBottomSheet(size = BottomSheetSize.Half, onDismiss = { showSheet = false }) {
        // Single-select picker: WiomListItem rows with Type=Radio.
        // For multi-select, sheet has a Primary "Done" CTA that commits the selection
        // and Body shows up to N selected values (e.g., "Push, SMS (+1)").
    }
}
```

### Address (Textarea)

```kotlin
WiomTextarea(
    value = address, onValueChange = { address = it.take(200) },
    title = "Installation address",
    placeholder = "House / floor / landmark…",
    counter = "${address.length} / 200",
    minLines = 3,
    modifier = Modifier.fillMaxWidth(),
)
```

---

## 10. Anti-patterns

(Universal rules in §7 cover the bans. Listed here are extras that
aren't already a numbered rule.)

- ❌ Drawing a custom field with `Box` + `BasicTextField` + manual
  border / padding instead of using the component.
- ❌ Building OTP as an Input with `"4 3 8 1"` spaced digits in
  `Body` — there's a dedicated component with auto-advance focus,
  per-digit boxing, and the Focused-state timer.
- ❌ Redrawing the OTP Focused-state timer on top — it's already
  baked into the variant.
- ❌ Using `CenterStart` / `CenterVertically` alignment on Textarea —
  placeholder drifts to the middle of a multi-line box. Use
  `TopStart` and `Alignment.Top`.
- ❌ Using `text.warning` token — it doesn't exist. Warning helper
  uses `text.onWarning` (the dark olive that doubles as warning text
  on every surface, per foundations).
- ❌ Setting `state = FieldState.Error` without setting `helper` — the
  user is left guessing what's wrong.
- ❌ Recoloring the Title per instance — the state-driven ramp exists
  for a reason.
- ❌ Hiding `Title` to save vertical space — once the user starts
  typing, the placeholder disappears and the field loses its label.

---

## 11. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Input Fields canvas: `3069:5441`.
Re-pull `get_variable_defs` on `3069:5441` if foundations token names
ever change.

| Node | Component / variant |
|---|---|
| `1422:112` | Field / Input — 8 state variants |
| `1422:2` · `1422:15` · `1422:28` | Input Default · Focused · Filled |
| `1422:41` · `1422:56` · `1422:71` | Input Error · Success · Warning |
| `1422:86` · `1422:99` | Input Disabled · Read-only |
| `1426:47` | Field / Textarea — 6 state variants |
| `1426:5` · `1426:12` · `1426:19` · `1426:26` · `1426:33` · `1469:21` | Textarea Default · Focused · Filled · Error · Success · Disabled |
| `1790:740` | Field / OTP — 6 state variants + `6-digit` boolean |
| `1790:637` · `1790:654` · `1790:672` · `1790:689` · `1790:706` · `1790:723` | OTP Default · Focused · Filled · Error · Success · Disabled |

**Description-vs-ship discrepancies** — if a stale doc says otherwise,
trust ship values:

- The Figma **page header** in this canvas reads "2 components · 14
  state variants". The actual ship is **3 components × 20 state
  variants** (Input 8 + Textarea 6 + OTP 6). Page header is outdated;
  the component sets are the source of truth.
- The Read-only state was previously documented as part of "8 states
  total". Confirmed in ship: only Input has `Read-only`; Textarea and
  OTP do not.
- `Counter` typography is `type.body.md` (14 sp Regular, same as
  helper), not `type.body.sm` — earlier docs said `body.sm`. Figma
  ships the whole helper row at one size.
- OTP boxes split width via `weight(1f)`, not fixed 76 / 48 dp. The
  76 / 48 numbers earlier docs cited are derived from `(328 − 3 × 8)
  ÷ 4` and `(328 − 5 × 8) ÷ 6`, but the boxes are flex children — if
  the bar gets wider/narrower, they scale.
- Border colors: all four status / focus borders use the saturated
  `*.focus` / `*.warning` family (`stroke.brand.focus`,
  `stroke.critical.focus`, `stroke.positive.focus`, `stroke.warning`)
  — not their `*.default` siblings.

---

## See also

- `wiom-design-foundations` — token source. Especially the
  warning-family rule (`text.onWarning` works on `bg.warning`,
  `bg.warning.subtle`, AND `bg.default`).
- `wiom-bottomsheet` — the picker sheet that a Read-only Input opens
  for dropdowns / single- and multi-select pickers.
- `wiom-selection-controls` — Radio / Checkbox rows inside the picker
  sheet.
- `wiom-cta` — the Primary "Done" CTA inside multi-select picker
  sheets.
- `wiom-list-item` — list-row anatomy that hosts each picker option.
- `wiom-pagination` — position indicator (Pagination/Bars) for multi-step
  flows that embed Input / OTP across screens.
- `wiom-compose-layout-rules` — Modifier discipline; no fixed heights;
  `Icons.Rounded.*` enforcement.
