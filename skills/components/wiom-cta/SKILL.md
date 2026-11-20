---
name: wiom-cta
description: >
  Wiom CTA / Button system — 6 types, 23 variants. Reference for
  generating Jetpack Compose code with WiomButton and for instancing
  the existing Figma CTA component (set 2038:116) via MCP. Use this
  skill EVERY TIME you write, edit, or review any screen / dialog /
  sheet / card containing a button or CTA in the Wiom Partner or
  Customer Android app, or when assembling Figma frames that include
  CTAs. Trigger on: Button, CTA, WiomButton, TextButton,
  OutlinedButton, FilledButton, Recharge, Pay, Submit, Delete,
  Acknowledge, Pre-booking, onClick, instance of 2038:116, Button
  Group, sticky bottom bar.
---

# Wiom CTA System

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
set `2038:116`, and the `wiom-design-foundations` token set.
**6 types · 23 variants · element-first tokens.**

The 5 standard types ship 4 states each (Default / Pressed / Disabled /
Loading). Acknowledge has its own 3-state model (Unchecked / Checked /
Disabled). Focus is a stroke ring, not a fill variant.

---

## 1. Figma — instancing the component

When assembling a Figma frame via MCP, instance the component set
`2038:116` and set the variant properties below. Strings are
**case-sensitive** (note the hyphen in `Pre-booking`).

| Property | Type | Default | Accepted values |
|---|---|---|---|
| `Type` | Variant | `Primary` | `Primary` · `Secondary` · `Tertiary` · `Destructive` · `Pre-booking` · `Acknowledge` |
| `State` | Variant | `Default` | Standard 5 types: `Default` · `Pressed` · `Disabled` · `Loading`. Acknowledge: `Unchecked` · `Checked` · `Disabled` |
| `Show Leading Icon` | Boolean | `true` | Hides leading icon when `false`. N/A for Acknowledge / Loading. |
| `Show Trailing Icon` | Boolean | `false` | N/A for Acknowledge / Loading. |
| `Leading Icon` | Instance swap | default chevron | Any icon from `Foundation/icons` (renders at 20 dp) |
| `Trailing Icon` | Instance swap | default chevron | Same library, 20 dp |

Each instance is `w-full` / fills its container. Figma renders it at
328 wide because the preview frame is 360 wide with 16 dp surface
margin. **The surface owns the gutter, not the CTA.**

Button Group is a layout helper (`2982:357`), not a separate component
— it places 1–3 instances of `2038:116` in a row or column with gap
12 dp, padding 0.

### Code enum ↔ Figma variant string

When generating both code and Figma in the same task, map them like so:

| `WiomButtonType` (Kotlin) | Figma `Type` value |
|---|---|
| `Primary` | `Primary` |
| `Secondary` | `Secondary` |
| `Tertiary` | `Tertiary` |
| `Destructive` | `Destructive` |
| `PreBooking` | `Pre-booking` (with hyphen) |
| `Acknowledge` | `Acknowledge` (compound — usually built in code as `WiomAcknowledgeRow` + `WiomButton`) |

| `WiomButtonState` (Kotlin) | Figma `State` value |
|---|---|
| `Default` | `Default` |
| `Disabled` | `Disabled` |
| `Loading` | `Loading` |
| (auto via `interactionSource`) | `Pressed` |

---

## 2. Anatomy

| Slot | Value | Token |
|---|---|---|
| Container width | Derived — `fillMaxWidth()` or `weight(1f)` | — |
| Container height | Derived — top pad + content + bottom pad (lands at 48 dp) | — |
| Container radius | 16 dp | `radius-large` |
| Container shadow | none | `shadow.none` |
| Horizontal padding | 16 dp | `space-16` |
| Vertical padding | 12 dp | `space-12` |
| Inner row | Centered both axes, 8 dp gap | `space-8` |
| Icon (leading / trailing) | 20 dp | `icon.sm` |
| Label | Noto Sans SemiBold 16 / 24 LH | `type.label.lg` |
| Loading spinner | 24 dp (replaces icon + label) | — |

**No fixed dimensions on the CTA — ever.** No `width(...)`,
`height(...)`, `heightIn`, `widthIn`, `size(...)`. The CTA defines
padding and content; dimensions emerge.

---

## 3. The 6 types

| Type | Purpose | Where it sits | Don't use for | Sample copy |
|---|---|---|---|---|
| **Primary** | Main action on a screen. Filled brand. Max 1 per screen. | Sticky bottom bar (most common); inline in cards / sheets / dialogs when it's the dominant action. | Cancel / Skip / Dismiss / Maybe later / generic "Continue". | "Recharge now", "भुगतान करें", "Verify OTP" |
| **Secondary** | Alternate prominent action alongside Primary. Outlined brand. | Side-by-side with Primary in bottom sheets; stacked under Primary in dialogs (side-by-side when both labels are short); rarely standalone. | Destructive actions; throwaway dismiss; the screen's main action. | "Cancel", "Change plan", "वापस जाएं" |
| **Tertiary** | Lightweight inline dismiss / soft action. Text-only. | Stacked under Primary; centered alone in dialogs for plain "OK"; inline in cards / list rows. | Destructive actions; the screen's main action; side-by-side next to Primary. | "बाद में", "Maybe later", "Skip", "Edit", "ठीक है" |
| **Destructive** | Irreversible loss of data. Filled critical. **Never alone.** | Confirmation bottom sheets, confirmation dialogs — always with an escape (Secondary or Tertiary). | "Cancel" (form close), "Skip", "Go back" — those don't destroy data. Red is for losing data. | "Delete plan", "Remove partner", "Deactivate account" |
| **Pre-booking** | Customer app only. The Primary of the **pre-booking journey** — every forward-action CTA from app-land through payment-success. Multiple per journey expected. | Sticky bar / sheets / dialogs / inline cards on every customer-app pre-booking screen until payment-success. **Auth screens reached via the journey still use standard Primary, not Pre-booking** — auth is a horizontal concern. | Login / signup / OTP. Post-payment-success screens. Partner app. Any non-pre-booking customer flow. | "Pre-book now", "Continue to plan", "Pay ₹X to pre-book" |
| **Acknowledge** | Compound — checkbox row + paired Primary CTA, disabled until checked. 1st-person verification before irreversible actions. | Sticky bottom or inline above the gated Primary action, just before submission. | Generic confirmations (use Primary); multi-checkbox forms; 3rd-person narration. | "मैंने सभी जानकारी सही दी है", "मैंने T&C पढ़ लिए हैं" |

---

## 4. Tokens per type × state

All tokens are **element-first** — `bg.*` / `text.*` / `icon.*` /
`stroke.*`. Always reference tokens, never hex.

### 4.1 Primary

| State | Container | Border | Label | Icon |
|---|---|---|---|---|
| Default | `bg.brand` | none | `text.onBrand` | `icon.inverse` |
| Pressed | `bg.brand.pressed` | none | `text.onBrand` | `icon.inverse` |
| Disabled | `bg.disabled` | none | `text.disabled` | `icon.disabled` |
| Loading | `bg.brand` | none | hidden | spinner — `icon.inverse` 24 dp |

### 4.2 Secondary (2 dp border in all states)

| State | Container | Border | Label | Icon |
|---|---|---|---|---|
| Default | transparent | `stroke.brand.focus` | `text.brand` | `icon.brand` |
| Pressed | `bg.brand.subtle` | `stroke.brand.focus` | `text.brand` | `icon.brand` |
| Disabled | transparent | `stroke.subtle` | `text.disabled` | `icon.disabled` |
| Loading | transparent | `stroke.brand.focus` | hidden | spinner — `icon.brand` 24 dp |

### 4.3 Tertiary

| State | Container | Label | Icon |
|---|---|---|---|
| Default | transparent | `text.brand` | `icon.brand` |
| Pressed | `bg.brand.subtle` | `text.brand` | `icon.brand` |
| Disabled | transparent | `text.disabled` | `icon.disabled` |
| Loading | transparent | hidden | spinner — `icon.brand` 24 dp |

### 4.4 Destructive

| State | Container | Label | Icon |
|---|---|---|---|
| Default | `bg.critical` | `text.onCritical` | `icon.inverse` |
| Pressed | `bg.critical.pressed` | `text.onCritical` | `icon.inverse` |
| Disabled | `bg.disabled` | `text.disabled` | `icon.disabled` |
| Loading | `bg.critical` | hidden | spinner — `icon.inverse` 24 dp |

### 4.5 Pre-booking (dark label — `bg.brand.accent` is light pink, white-on-pink fails AA)

| State | Container | Label | Icon |
|---|---|---|---|
| Default | `bg.brand.accent` | `text.onBrandAccent` | `icon.action` |
| Pressed | `bg.brand.accent.pressed` | `text.onBrandAccent` | `icon.action` |
| Disabled | `bg.disabled` | `text.disabled` | `icon.disabled` |
| Loading | `bg.brand.accent` | hidden | spinner — `icon.action` 24 dp |

### 4.6 Acknowledge (compound — checkbox row + paired Primary CTA)

`space-16` between checkbox row and CTA. `space-12` between checkbox
glyph and label (Figma drift: 10 px → normalize to 12 dp for the
4 dp grid). Compound height = 24 + 16 + 48 = **88 dp**.

| State | Checkbox | Row label | Inner CTA |
|---|---|---|---|
| Unchecked | Outlined 24 dp, `stroke.brand.focus` 2 dp | `text.default` | Primary Disabled tokens |
| Checked | Filled `bg.brand`, white check (`icon.inverse`) | `text.default` | Primary Default tokens |
| Disabled | Outlined 24 dp, `stroke.subtle` 2 dp | `text.default` | Primary Disabled tokens |

Acknowledge copy is **always 1st person** ("मैंने…" / "I have…"),
never 3rd person ("कस्टमर ने…").

### Focus ring (all CTAs)

`stroke.brand.focus` on most types, `stroke.critical.focus` on
Destructive. Applied on keyboard / TalkBack focus only — not a fill
variant.

---

## 5. Surface contract

| Surface | H margin (surface owns) | V margin (surface owns) | CTA does |
|---|---|---|---|
| Inline screen content | `space-16` via screen padding | own | `fillMaxWidth()` |
| Sticky bottom bar | `space-16` start / end | `space-16` top, `space-12` bottom + safe-area inset | `fillMaxWidth()` |
| Dialog | `space-24` via dialog padding | own | `fillMaxWidth()` |
| Bottom sheet | `space-16` via sheet padding | own | `fillMaxWidth()` |

**Sticky bar fill:** `bg.default`, 1 dp top border in `stroke.subtle`,
`shadow.none`.

**Sticky vs inline placement:** if removing the CTA would mean the user
has no way to complete the screen's purpose → sticky-bottom. Otherwise
inline.

---

## 6. Pairing (Button Group)

Master table — pick the row matching intent, read across.

| Surface | Scenario | Layout | CTAs |
|---|---|---|---|
| **Bottom sheet** | 1 dominant action | full-width | Primary |
| | Dominant + weighty dismiss | side-by-side | Secondary (left) + Primary (right) |
| | Dominant + soft dismiss | stacked | Primary (top) + Tertiary |
| | Equal weight, short copy¹ | side-by-side | Secondary + Secondary |
| | Equal weight, copy can't shorten | stacked | Secondary + Secondary |
| | Destructive confirmation | side-by-side | Secondary (left) + Destructive (right) |
| | 3 actions | stacked | Primary + Secondary + Tertiary |
| **Dialog** (stacked by default; side-by-side only when both labels are short — 264 dp content width) | Simple acknowledgment | single | Tertiary ("ठीक है") |
| | Confirmation | stacked | Primary + Secondary |
| | Destructive confirmation | stacked | Destructive + Secondary |
| | Celebration / success | full-width | Primary |
| **Sticky bottom bar** | Reason-for-being action | full-width | Primary |
| | Acknowledge gate | stacked compound | Acknowledge + Primary |
| | Two-action terminal step (rare) | side-by-side | Secondary (left) + Primary (right) |
| **Inline (cards / list rows)** | Single inline action | hugs, end-aligned | Tertiary |
| | Card primary action | full-width inside card | Primary |
| | Edit / Delete on a row | side-by-side | Tertiary + Tertiary |

¹ ≤14 Hindi chars or ≤18 English chars per slot.

**Right side = preferred** (side-by-side). **Top = preferred** (stacked).

---

## 7. Universal rules

1. **No fixed dimensions on the CTA.** Width = `fillMaxWidth()` or
   `weight(1f)`. Height emerges from padding + content. No
   `width(...)`, `height(...)`, `heightIn`, `widthIn`, `size(...)`.
2. **Max 1 Primary, max 1 Destructive** per screen / sheet / dialog.
   Never pair Primary + Destructive — Destructive replaces Primary.
3. **Tertiary never side-by-side with Primary** — stacked only.
4. **Destructive never alone** — always paired with Secondary or
   Tertiary as escape.
5. **Pre-booking is the Primary of the pre-booking journey.** Inside
   the journey (customer-app, app-land through payment-success), every
   forward-action CTA is Pre-booking. Outside (auth, post-payment,
   Partner app, other flows) → standard Primary.
6. **CTA copy reflects the destination** — "Recharge", not "Continue".
   Acknowledge copy is always 1st person.
7. **CTA text never wraps, truncates, or shrinks.** Rewrite shorter.
8. **Surfaces own the gutter; CTA fills.**
9. **Icons** come from `Icons.Rounded.*` or
   `painterResource(R.drawable.ic_wiom_* / ic_partner_*)`. Never
   `Icons.Default / Filled / Outlined / Sharp / TwoTone`. Set
   `Icon(tint = …)` explicitly with the `icon.*` token for the type.
   Use `contentDescription = "<action verb>"` for icons that convey
   meaning beyond the label; `null` when the label fully describes the
   action.
10. **Inner Row centered both axes** —
    `Arrangement.spacedBy(8.dp, Alignment.CenterHorizontally)` +
    `Alignment.CenterVertically`. Never `Arrangement.Start` /
    `SpaceBetween`. **The horizontal centering only takes effect when
    the Row is wider than its content** — wrap the Row in
    `Box(modifier.fillMaxWidth(), contentAlignment = Alignment.Center)`
    (or apply `Modifier.fillMaxWidth()` to the Row itself). Without
    this, short labels hug-content and render visually off-center
    inside a fillMaxWidth() button — a real bug surfaced during the
    Android v2 rebuild.
11. **Tokens only — no raw hex, no role-first legacy tokens** like
    `color.brand.primary`. Element-first only (`bg.*`, `text.*`,
    `stroke.*`, `icon.*`).
12. **Flat surfaces** — `shadow.none` on CTA and sticky bar.

---

## 8. Decision logic

### Type

```
One main action on this screen?                     → Primary
Customer-app pre-booking flow?                      → Pre-booking
Alternate prominent action alongside Primary?       → Secondary
Lightweight dismiss / soft action / Edit / Skip?    → Tertiary
Destroys / removes / cancels-plan?                  → Destructive
Must explicitly confirm before proceeding?          → Acknowledge + Primary
```

### Layout (delta = importance gap between two CTAs)

```
Sequential gate (Acknowledge before action)?
  → stacked compound

HIGH DELTA (one dominant + one dismiss)
  Weighty dismiss (Cancel / Back)
    → side-by-side · Secondary (left) + Primary (right)
  Throwaway dismiss ("बाद में" / "Later")
    → stacked · Primary (top) + Tertiary
  Dominant is destructive
    → side-by-side · Secondary (left) + Destructive (right)

LOW / ZERO DELTA (equal weight)
  Both fit ≤14 Hindi / ≤18 English chars per slot
    → side-by-side · Secondary + Secondary
  Can be shortened
    → SHORTEN → side-by-side
  Shortening loses meaning
    → stacked · Secondary + Secondary
```

---

## 9. Compose API + code

### Public API

```kotlin
enum class WiomButtonType { Primary, Secondary, Tertiary, Destructive, PreBooking }
enum class WiomButtonState { Default, Disabled, Loading }
// Pressed is not a value — handled automatically via interactionSource.

@Composable
fun WiomButton(
    text: String,
    type: WiomButtonType,
    state: WiomButtonState = WiomButtonState.Default,
    leadingIcon: ImageVector? = null,
    trailingIcon: ImageVector? = null,
    iconContentDescription: String? = null,        // null when label describes the action
    modifier: Modifier = Modifier,
    onClick: () -> Unit,
)

@Composable
fun WiomAcknowledgeRow(
    text: String,
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit,
    enabled: Boolean = true,
    modifier: Modifier = Modifier,
)

// Lives in wiom-selection-controls; signature shown for code-gen context.
@Composable
fun WiomCheckbox(
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit,
    enabled: Boolean = true,
    modifier: Modifier = Modifier,
)
```

### Token resolver (drives container / label / icon / border per state)

```kotlin
@Composable
private fun rememberCtaColors(
    type: WiomButtonType,
    state: WiomButtonState,
    pressed: Boolean,
): CtaColors {
    // "effective" state collapses Pressed-from-interaction into the state machine
    val effective = when {
        state == WiomButtonState.Disabled -> Effective.Disabled
        state == WiomButtonState.Loading -> Effective.Loading
        pressed -> Effective.Pressed
        else -> Effective.Default
    }
    return when (type) {
        WiomButtonType.Primary -> when (effective) {
            Effective.Default  -> CtaColors(Wiom.bg.brand,         null,                       Wiom.text.onBrand,   Wiom.icon.inverse)
            Effective.Pressed  -> CtaColors(Wiom.bg.brand.pressed, null,                       Wiom.text.onBrand,   Wiom.icon.inverse)
            Effective.Disabled -> CtaColors(Wiom.bg.disabled,      null,                       Wiom.text.disabled,  Wiom.icon.disabled)
            Effective.Loading  -> CtaColors(Wiom.bg.brand,         null,                       Color.Transparent,   Wiom.icon.inverse)
        }
        WiomButtonType.Secondary -> when (effective) {
            Effective.Default  -> CtaColors(Color.Transparent,            Wiom.stroke.brand.focus,    Wiom.text.brand,         Wiom.icon.brand)
            Effective.Pressed  -> CtaColors(Wiom.bg.brand.subtle,         Wiom.stroke.brand.focus,    Wiom.text.brand,         Wiom.icon.brand)
            Effective.Disabled -> CtaColors(Color.Transparent,            Wiom.stroke.subtle,         Wiom.text.disabled,      Wiom.icon.disabled)
            Effective.Loading  -> CtaColors(Color.Transparent,            Wiom.stroke.brand.focus,    Color.Transparent,       Wiom.icon.brand)
        }
        WiomButtonType.Tertiary -> when (effective) {
            Effective.Default  -> CtaColors(Color.Transparent,            null,                       Wiom.text.brand,         Wiom.icon.brand)
            Effective.Pressed  -> CtaColors(Wiom.bg.brand.subtle,         null,                       Wiom.text.brand,         Wiom.icon.brand)
            Effective.Disabled -> CtaColors(Color.Transparent,            null,                       Wiom.text.disabled,      Wiom.icon.disabled)
            Effective.Loading  -> CtaColors(Color.Transparent,            null,                       Color.Transparent,       Wiom.icon.brand)
        }
        WiomButtonType.Destructive -> when (effective) {
            Effective.Default  -> CtaColors(Wiom.bg.critical,             null,                       Wiom.text.onCritical,    Wiom.icon.inverse)
            Effective.Pressed  -> CtaColors(Wiom.bg.critical.pressed,     null,                       Wiom.text.onCritical,    Wiom.icon.inverse)
            Effective.Disabled -> CtaColors(Wiom.bg.disabled,             null,                       Wiom.text.disabled,      Wiom.icon.disabled)
            Effective.Loading  -> CtaColors(Wiom.bg.critical,             null,                       Color.Transparent,       Wiom.icon.inverse)
        }
        WiomButtonType.PreBooking -> when (effective) {
            Effective.Default  -> CtaColors(Wiom.bg.brand.accent,         null,                       Wiom.text.onBrandAccent, Wiom.icon.action)
            Effective.Pressed  -> CtaColors(Wiom.bg.brand.accent.pressed, null,                       Wiom.text.onBrandAccent, Wiom.icon.action)
            Effective.Disabled -> CtaColors(Wiom.bg.disabled,             null,                       Wiom.text.disabled,      Wiom.icon.disabled)
            Effective.Loading  -> CtaColors(Wiom.bg.brand.accent,         null,                       Color.Transparent,       Wiom.icon.action)
        }
    }
}

private enum class Effective { Default, Pressed, Disabled, Loading }

private data class CtaColors(
    val container: Color,
    val border: Color?,             // 2dp when non-null (Secondary)
    val label: Color,
    val icon: Color,                // also used as spinner tint
)
```

### Inner row (decides label alignment; uses the resolver above)

```kotlin
val interactionSource = remember { MutableInteractionSource() }
val pressed by interactionSource.collectIsPressedAsState()
val colors = rememberCtaColors(type, state, pressed)

Surface(
    color = colors.container,
    shape = RoundedCornerShape(Wiom.radius.large),     // 16dp
    border = colors.border?.let { BorderStroke(2.dp, it) },
    modifier = modifier                                // no height, no width
        .clip(RoundedCornerShape(Wiom.radius.large))
        .clickable(
            enabled = state == WiomButtonState.Default,
            interactionSource = interactionSource,
            indication = LocalIndication.current,
            onClick = onClick,
        ),
) {
    // Box(fillMaxWidth + contentAlignment.Center) lets short labels render
    // centered inside a CTA that the caller has stretched with
    // fillMaxWidth() / weight(1f). Without it, the inner Row hugs its
    // content and the label drifts left of true centre.
    Box(
        modifier = Modifier.fillMaxWidth(),
        contentAlignment = Alignment.Center,
    ) {
        Row(
            modifier = Modifier.padding(
                horizontal = Wiom.space.s16,
                vertical = Wiom.space.s12,
            ),
            horizontalArrangement = Arrangement.spacedBy(
                Wiom.space.s8,
                Alignment.CenterHorizontally,
            ),
            verticalAlignment = Alignment.CenterVertically,
        ) {
        if (state == WiomButtonState.Loading) {
            CircularProgressIndicator(
                color = colors.icon,
                strokeWidth = 2.dp,
                modifier = Modifier.size(24.dp),
            )
        } else {
            leadingIcon?.let {
                Icon(it, contentDescription = iconContentDescription,
                    tint = colors.icon, modifier = Modifier.size(20.dp))
            }
            Text(
                text = text,
                style = Wiom.type.label.lg,
                color = colors.label,
                maxLines = 1,
                textAlign = TextAlign.Center,
            )
            trailingIcon?.let {
                Icon(it, contentDescription = null,
                    tint = colors.icon, modifier = Modifier.size(20.dp))
            }
        }
        }
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — fixed height, hardcoded width, hex color, Material default icon
WiomButton(
    text = "Recharge",
    type = WiomButtonType.Primary,
    leadingIcon = Icons.Default.Bolt,                  // wrong icon family
    modifier = Modifier
        .width(328.dp)                                 // never hardcode width
        .height(48.dp),                                // never hardcode height
    onClick = { },
)

// ✅ RIGHT — surface owns gutter, CTA fills, height emerges from padding
Box(modifier = Modifier.padding(horizontal = Wiom.space.s16)) {  // surface owns gutter
    WiomButton(
        text = "Recharge now",                                   // contextual copy
        type = WiomButtonType.Primary,
        leadingIcon = Icons.Rounded.Bolt,                        // Rounded family only
        modifier = Modifier.fillMaxWidth(),
        onClick = { },
    )
}
```

### Button Group

```kotlin
// Side by side
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
) {
    WiomButton("Cancel",  type = WiomButtonType.Secondary, modifier = Modifier.weight(1f), onClick = { })
    WiomButton("Confirm", type = WiomButtonType.Primary,   modifier = Modifier.weight(1f), onClick = { })
}

// Stacked — both horizontalAlignment AND fillMaxWidth on every child;
// without fillMaxWidth, short labels hug and render visually off-center.
Column(
    modifier = Modifier.fillMaxWidth(),
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(Wiom.space.s12),
) {
    WiomButton("भुगतान करें", type = WiomButtonType.Primary,  modifier = Modifier.fillMaxWidth(), onClick = { })
    WiomButton("बाद में",    type = WiomButtonType.Tertiary, modifier = Modifier.fillMaxWidth(), onClick = { })
}
```

### Acknowledge

```kotlin
Column(
    modifier = Modifier.fillMaxWidth(),
    verticalArrangement = Arrangement.spacedBy(Wiom.space.s16),
) {
    Row(
        modifier = Modifier.fillMaxWidth(),
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
        verticalAlignment = Alignment.Top,
    ) {
        WiomCheckbox(
            checked = checked,
            onCheckedChange = onCheckedChange,
            enabled = !disabled,
        )
        Text(
            text = ackCopy,
            style = Wiom.type.label.lg,
            color = Wiom.text.default,
            modifier = Modifier.weight(1f),
        )
    }
    WiomButton(
        text = primaryCopy,
        type = WiomButtonType.Primary,
        state = if (!checked || disabled) WiomButtonState.Disabled
                else WiomButtonState.Default,
        modifier = Modifier.fillMaxWidth(),
        onClick = onConfirm,
    )
}
```

### Sticky bottom bar — `StickyCtaBar` (canonical helper)

Define once and reuse across screen recipes. The bar **is** the surface
— it owns the gutter, top border, and safe-area inset. Children fill
end-to-end.

```kotlin
@Composable
fun StickyCtaBar(
    modifier: Modifier = Modifier,
    content: @Composable ColumnScope.() -> Unit,
) {
    Surface(
        color = Wiom.bg.default,
        modifier = modifier
            .fillMaxWidth()
            .drawBehind {                               // 1dp top border, no custom helper
                drawLine(
                    color = Wiom.stroke.subtle,
                    start = Offset(0f, 0f),
                    end = Offset(size.width, 0f),
                    strokeWidth = 1.dp.toPx(),
                )
            },
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = Wiom.space.s16)   // surface owns gutter
                .padding(top = Wiom.space.s16, bottom = Wiom.space.s12)
                .navigationBarsPadding(),               // safe-area inset
            horizontalAlignment = Alignment.CenterHorizontally,
            content = content,
        )
    }
}

// Usage:
StickyCtaBar {
    WiomButton(
        text = "Recharge now",
        type = WiomButtonType.Primary,
        modifier = Modifier.fillMaxWidth(),
        onClick = { },
    )
}
```

---

## 10. Screen-level recipes

End-to-end assembly examples. Use these as templates — pick the
matching one for the user's intent, then swap copy and tokens.

### Recharge screen (sticky-bottom Primary, the most common pattern)

```kotlin
Scaffold(
    bottomBar = {
        // Sticky bottom bar from §9
        StickyCtaBar {
            WiomButton(
                text = "Recharge now",
                type = WiomButtonType.Primary,
                modifier = Modifier.fillMaxWidth(),
                onClick = onRecharge,
            )
        }
    }
) { padding ->
    // Screen content scrolls underneath; bar stays put.
    Column(modifier = Modifier.padding(padding).padding(horizontal = Wiom.space.s16)) { /* ... */ }
}
```

### Delete confirmation dialog (Destructive + Secondary, stacked)

```kotlin
// Inside the dialog body — dialog itself owns space-24 padding
Column(
    modifier = Modifier.fillMaxWidth(),
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(Wiom.space.s12),
) {
    WiomButton(
        text = "Delete plan",
        type = WiomButtonType.Destructive,
        modifier = Modifier.fillMaxWidth(),
        onClick = onDelete,
    )
    WiomButton(
        text = "Cancel",
        type = WiomButtonType.Secondary,
        modifier = Modifier.fillMaxWidth(),
        onClick = onDismiss,
    )
}
```

### Pre-booking step (sticky-bottom Pre-booking)

```kotlin
// Same shape as Recharge — only the type changes.
StickyCtaBar {
    WiomButton(
        text = "Continue to plan",
        type = WiomButtonType.PreBooking,        // Pre-booking, not Primary
        modifier = Modifier.fillMaxWidth(),
        onClick = onContinue,
    )
}
```

### T&C acknowledgment before submission (Acknowledge + Primary, sticky)

```kotlin
StickyCtaBar {
    var checked by remember { mutableStateOf(false) }
    Column(
        modifier = Modifier.fillMaxWidth(),
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s16),
    ) {
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
            verticalAlignment = Alignment.Top,
        ) {
            WiomCheckbox(checked = checked, onCheckedChange = { checked = it })
            Text(
                "मैंने सभी जानकारी सही दी है",
                style = Wiom.type.label.lg,
                color = Wiom.text.default,
                modifier = Modifier.weight(1f),
            )
        }
        WiomButton(
            text = "जमा करें",
            type = WiomButtonType.Primary,
            state = if (checked) WiomButtonState.Default else WiomButtonState.Disabled,
            modifier = Modifier.fillMaxWidth(),
            onClick = onSubmit,
        )
    }
}
```

### Bottom sheet with two actions (Secondary + Primary, side-by-side)

```kotlin
ModalBottomSheet(onDismissRequest = onDismiss) {
    Column(modifier = Modifier.padding(horizontal = Wiom.space.s16)) {
        // ...sheet content...
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
        ) {
            WiomButton("Change plan", type = WiomButtonType.Secondary,
                modifier = Modifier.weight(1f), onClick = onChange)
            WiomButton("Confirm",     type = WiomButtonType.Primary,
                modifier = Modifier.weight(1f), onClick = onConfirm)
        }
    }
}
```

---

## 11. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. CTAs section: `1043:658`. Re-pull
`get_variable_defs` on `2038:116` if foundations token names ever
change — those are the only values that should drift.

| Node | Component / variant |
|---|---|
| `2038:116` | CTA component set — 23 ship variants |
| `2038:2` · `2038:14` · `2038:26` | Primary Default · Disabled · Loading |
| `2038:28` | Secondary Default (2 dp `stroke.brand.focus`) |
| `2804:310` | Pre-booking Default (`bg.brand.accent` + `text.onBrandAccent`) |
| `2144:2` · `2144:8` · `2144:15` | Acknowledge Unchecked · Checked · Disabled (88 dp compound) |
| `2982:357` · `2982:305` | Button Group set · Count=2 Stacked (each slot `w-full`) |

**Description-vs-ship discrepancies** — Figma's published component
description claims 28 variants and 5 states (incl. `Focused`); the
component **ships 23 variants and 4 states**. Focus is a stroke ring,
not a fill variant. Trust ship values.

---

## See also

- `wiom-design-foundations` — token source (`bg.*`, `text.*`, `icon.*`,
  `stroke.*`, `space-*`, `radius-*`, `type.*`).
- `wiom-compose-layout-rules` — Modifier discipline; no explicit height
  on hugging components; `Icons.Rounded` enforcement.
- `wiom-dialog` — button group inside dialogs (stacked only, max 2).
- `wiom-bottomsheet` — button group inside sheets (side-by-side allowed).
- `wiom-selection-controls` — `WiomCheckbox` used inside Acknowledge.
