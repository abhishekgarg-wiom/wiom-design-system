---
name: wiom-cta
description: >
  Wiom CTA / Button system — 6 types, 4–5 states each, the Button Group rules
  for pairing 1–3 CTAs, copy discipline, sticky-vs-inline placement, and token
  mappings. Use this skill EVERY TIME you create, edit, or review any screen,
  dialog, bottom sheet, card, or component that contains a button or CTA for
  the Wiom Partner or Customer app. Trigger when you see Button, CTA, TextButton,
  OutlinedButton, FilledButton, Recharge, Pay, Submit, Delete, Acknowledge,
  Pre-booking, onClick, or any interactive action element.
---

# Wiom CTA System

The approved button foundation for the Wiom Android apps.
**6 types · up to 5 states each · Button Group (1–3 CTAs) · placement rules · copy discipline.**
Figma reference: CTA `2038:116`, Button Group `2982:357` (frame `1043:658`).

---

## 1. The 6 CTA types

| Type | Purpose | Example copy |
|---|---|---|
| **Primary** | Main action on a screen. Filled brand. Max 1 per screen. | "Recharge now", "भुगतान करें" |
| **Secondary** | Alternate / prominent but not-the-main action. Outlined brand. | "Change plan", "Learn more" |
| **Tertiary** | Lightweight inline dismiss or soft action. Text-only. | "Maybe later", "Edit" |
| **Destructive** | Irreversible destruction of data. Filled critical. Max 1 per screen. | "Delete plan", "Remove partner" |
| **Acknowledge** | Compound — a checkbox row + paired Primary CTA that's disabled until checked. For 1st-person verification before irreversible actions. | checkbox + "मैंने सभी जानकारी सही दी है" + paired "रिचार्ज करें" |
| **Pre-booking** | Customer-app only. Bright-pink accent fill with dark label — distinguishes pre-booking flows from generic Primary CTAs on the same screen. | "Pre-book now" |

### Leading / trailing icon (Primary / Secondary / Tertiary / Destructive / Pre-booking)

Figma booleans `Show Leading Icon` / `Show Trailing Icon` (defaults: leading ON,
trailing OFF). Icon is a 20 dp `icon.sm` instance, swappable. Loading state
hides the icon and shows a spinner instead. Acknowledge does not carry its own
leading/trailing icon — the checkbox IS its leading element.

---

## 2. States per type

All states below are Figma variants; use element-first tokens.

### Shared spec (all types except Acknowledge)

| Property | Value | Token |
|---|---|---|
| Height | 48 dp (hugs; padding + line-height) | `space-12` V / `space-16` H + 24 LH |
| Radius | 16 dp | `radius-large` |
| Label | 16sp SemiBold 600 / 24 LH | `type.label.lg` |
| Icon size | 20 dp | `icon.sm` |
| Icon-to-label gap | 8 dp | `space-8` |
| Shadow | none | `shadow.none` |

### Primary — 4 states

| State | Fill | Label | Icon |
|---|---|---|---|
| Default | `bg.brand` (#D9008D) | `text.onBrand` (#FFFFFF) | `icon.inverse` (#FAF9FC) |
| Pressed | `bg.brand.pressed` (#A30070) | `text.onBrand` | `icon.inverse` |
| Disabled | `bg.disabled` (#D7D3E0) | `text.disabled` (#A7A1B2) | `icon.disabled` (#A7A1B2) |
| Loading | `bg.brand` (unchanged) | hidden | spinner `icon.inverse` |

### Secondary — 4 states (outlined)

| State | Fill | Border | Label | Icon |
|---|---|---|---|---|
| Default | transparent | `stroke-medium` + `stroke.brand.focus` (#D9008D) | `text.brand` (#D9008D) | `icon.brand` (#D9008D) |
| Pressed | `bg.brand.subtle` (#FFE5F6) | `stroke-medium` + `stroke.brand.focus` | `text.brand` | `icon.brand` |
| Disabled | transparent | `stroke-medium` + `stroke.subtle` (#D7D3E0) | `text.disabled` | `icon.disabled` |
| Loading | transparent | `stroke.brand.focus` (unchanged) | hidden | spinner `icon.brand` |

### Tertiary — 4 states (text-only, no border)

| State | Fill | Label | Icon |
|---|---|---|---|
| Default | transparent | `text.brand` | `icon.brand` |
| Pressed | `bg.brand.subtle` | `text.brand` | `icon.brand` |
| Disabled | transparent | `text.disabled` | `icon.disabled` |
| Loading | transparent | hidden | spinner `icon.brand` |

### Destructive — 4 states (filled critical)

| State | Fill | Label | Icon |
|---|---|---|---|
| Default | `bg.critical` (#D92130) | `text.onCritical` (#FFFFFF) | `icon.inverse` |
| Pressed | `bg.critical.pressed` (#A31824) | `text.onCritical` | `icon.inverse` |
| Disabled | `bg.disabled` | `text.disabled` | `icon.disabled` |
| Loading | `bg.critical` | hidden | spinner `icon.inverse` |

### Pre-booking — 4 states (bright accent)

| State | Fill | Label | Icon |
|---|---|---|---|
| Default | `bg.brand.accent` (#FF66C9) | `text.onBrandAccent` (#161021 — **dark**) | `icon.action` (#352D42) |
| Pressed | `bg.brand.accent.pressed` (#EA5DB8) | `text.onBrandAccent` | `icon.action` |
| Disabled | `bg.disabled` | `text.disabled` | `icon.disabled` |
| Loading | `bg.brand.accent` | hidden | spinner `icon.action` |

**Why dark label:** `bg.brand.accent` (#FF66C9) is a light pink; white label
fails AA (2.5:1). Dark `text.onBrandAccent` lands at 7.07:1.

### Acknowledge — 3 states (compound: checkbox row + paired Primary CTA)

| State | Checkbox | Row label | Paired Primary CTA |
|---|---|---|---|
| Unchecked | `stroke.strong` (#473F55), 2 dp, transparent fill | `text.default` (#161021), `type.body.md` | Disabled (`bg.disabled` / `text.disabled` / `icon.disabled`) |
| Checked | `bg.brand` fill, no stroke, `Icons.Rounded.Check` `icon.inverse` | `text.default` | Enabled (full Primary tokens) |
| Disabled | 40% opacity on checkbox | `text.disabled` | Disabled |

Compound height = 80 dp (row ≈ 32 dp + `space-8` gap + Primary CTA 48 dp). In
Figma, Acknowledge ships as a single compound variant; in code, it's two
composables (`WiomAcknowledgeRow` + `WiomButton`) wired through shared state.

### States deliberately NOT in V2

**Focus** — was in V1, removed in V2. Keyboard focus surfaces via
`stroke.brand.focus` / `stroke.critical.focus` ring (accessibility only on
Android — TalkBack / external keyboard), **not** as a separate visual fill
variant. Rule: destructive elements get the red focus ring
(`stroke.critical.focus`), everything else gets the brand pink ring
(`stroke.brand.focus`).

**Hover** — never in V1 and never in V2. Wiom is touch-first Android; there's
no pointer to hover.

---

## 3. Decision logic — which CTA

```
Is this the one main action on the screen?
  → Primary

Is this a customer-app pre-booking flow action?
  → Pre-booking (never outside pre-booking flows)

Is this an alternate prominent action alongside Primary?
  → Secondary

Is this a lightweight dismiss / soft action / inline "Edit" / "Skip"?
  → Tertiary

Does this action delete, remove, cancel-plan, or can't be undone?
  → Destructive

Must the user explicitly confirm they've verified something before proceeding?
  → Acknowledge (checkbox) + Primary (disabled until checked)
```

Never Destructive for: Skip, Cancel (form close), No thanks, Maybe later,
Go back, Close, Dismiss, Not interested. Those are Secondary or Tertiary. Red
is only for losing data.

---

## 4. Button Group — pairing 1, 2, or 3 CTAs

Figma component `Button Group` (`2982:357`). Width 328 dp, gap 12 dp
(`space-12`), no padding of its own — the parent owns screen insets.

| Count | Default types | Layout choices |
|---|---|---|
| 1 | Primary | Side-by-side or Stacked (identical result) |
| 2 | Primary + Secondary | Side-by-side (Primary on right) OR Stacked (Primary on top) |
| 3 | Primary + Secondary + Tertiary | Stacked recommended (Primary on top); side-by-side only if each CTA fits with short copy |

Rules enforced by the Button Group:

- Max **1 Primary** and **1 Destructive** per screen.
- Never pair Destructive with Primary — **Destructive IS the primary** in
  that group. Acceptable pairs: Destructive + Secondary / Destructive +
  Tertiary.
- Use Secondary/Tertiary for Dismiss/Skip/Cancel/Close — never Destructive.
- Side-by-side with 3 CTAs is tight on mobile (≈101 dp each) — prefer Stacked.

### Delta-based layout choice

The "delta" is the importance gap between the two CTAs.

```
HIGH DELTA (one dominant + one dismiss)
  Dismiss carries visual weight (Cancel/Back)     → Side-by-side: Secondary + Primary
  Dismiss is throwaway ("later", "not now")       → Stacked: Primary + Tertiary
  Dominant is destructive                         → Side-by-side: Secondary + Destructive

LOW / ZERO DELTA (equal-weight choices)
  Both fit in ≈14 Hindi / ≈18 English characters  → Side-by-side: Secondary + Secondary
  Text can be shortened without losing meaning    → Shorten → Side-by-side
  Shortening would lose critical context          → Stacked: Secondary + Secondary

ACKNOWLEDGE + Primary
  Not a parallel choice — sequential verification → Always stacked
```

### Context-specific defaults

| Context | Layout | Typical pairing |
|---|---|---|
| Bottom sheet — 1 action | Full-width | Primary only |
| Bottom sheet — confirm/cancel | Side-by-side | Secondary + Primary |
| Bottom sheet — soft dismiss | Stacked | Primary + Tertiary |
| Bottom sheet — destructive | Side-by-side | Secondary + Destructive |
| Dialog (all types) | **Stacked only** — dialog width forbids side-by-side | per `wiom-dialog` |
| Success / celebration | Full-width or Stacked | Primary only, or Primary + Tertiary |

**Dialog exception.** Dialogs are 312 dp wide, which doesn't fit two readable
side-by-side CTAs. All dialog button groups are stacked. See `wiom-dialog`.

---

## 5. Universal rules

1. **Max 1 Primary** per screen / sheet / dialog.
2. **Max 1 Destructive** per screen. No Primary + Destructive pair.
3. **Right = preferred action** in side-by-side layout (Primary on the right).
4. **Top = preferred action** in stacked layout.
5. **Tertiary only in stacked pairs** — never side-by-side next to Primary
   (borderless next to filled looks broken horizontally).
6. **Destructive never alone** — pair with Secondary or Tertiary as escape.
7. **CTA copy reflects the destination**, not the current screen. "Recharge"
   on plan selection, not "Continue".
8. **Acknowledge uses 1st person** — "मैंने…" / "I've…", never "कस्टमर ने…".
9. **Sticky-bottom CTA bar has `stroke-small` (1 dp) + `stroke.subtle`
   top border, `shadow.none`.** Never a drop shadow, never elevation. Flat
   border-top only.
10. **CTA text never wraps, truncates, or shrinks.** If text overflows,
    rewrite shorter. No ellipsis, no multi-line, no font-size reduction.
11. **Equal-weight CTAs: try side-by-side first.** Switch to stacked only if
    shortening would lose meaning.
12. **Sticky vs inline placement**:
    - **Sticky-bottom** — the reason-for-being of the screen (Recharge, Submit,
      Pay, Confirm). Pinned above the nav bar.
    - **Inline** — CTAs inside cards, dialogs, sheets. Move with the content.
13. **Icons come from `Icons.Rounded.*`** or `painterResource(R.drawable.ic_wiom_*)`
    / `ic_partner_*`. Never `Icons.Default/Filled/Outlined/Sharp/TwoTone`.
14. **Icon tint** always uses the `icon.*` token for the CTA's type (see
    tables above), not `text.on*`. Set `tint` explicitly on Compose `Icon(...)` —
    `Button(contentColor)` would otherwise overwrite the carefully calibrated
    hex.

---

## 6. Decision validation matrix

| Pairing | Delta | Default layout | Rule check |
|---|---|---|---|
| Secondary + Primary | Medium | Side-by-side | Rules 1, 3 |
| Primary + Tertiary | High | Stacked | Rules 4, 5 |
| Secondary + Secondary (short) | Zero | Side-by-side | Rule 11 |
| Secondary + Secondary (long, irreducible) | Zero | Stacked | Rules 10, 11 |
| Secondary + Destructive | Medium | Side-by-side | Rules 2, 3, 6 |
| Tertiary + Destructive | High | Stacked | Rules 2, 5, 6 |
| Acknowledge + Primary | N/A | Stacked always | Rule 8 |

---

## 7. Anti-patterns

- ❌ Primary + Primary (no hierarchy)
- ❌ Primary + Destructive (Destructive replaces Primary)
- ❌ Four+ CTAs at the same level
- ❌ Tertiary side-by-side with Primary
- ❌ Destructive alone (no escape)
- ❌ "Continue" / "Submit" / "Next" / "आगे बढ़ें" (contextless copy)
- ❌ "कस्टमर ने…" on Acknowledge (must be 1st person)
- ❌ Any `color.brand.primary`, `color.negative.primary`, `color.text.on-color`,
     `color.surface.muted`, `color.text.disabled`, `color.border.default`,
     `color.border.strong` (all legacy — migrate to element-first)
- ❌ Raw hex (`#D9008D`, `#FFFFFF`, …) in code
- ❌ `shadow.md` / `shadow.lg` on a CTA container (flat + border-top only)
- ❌ CTA text wrap / truncate / ellipsis
- ❌ Focus state as a fill variant (use focus ring via `stroke.brand.focus`)
- ❌ Hover state (never existed in Wiom)
- ❌ Side-by-side inside a dialog (stacked only)
- ❌ Icons from `Icons.Default/Filled/Outlined/Sharp/TwoTone`
- ❌ Pre-booking CTA outside customer-app pre-booking flows

---

## 8. Minimal Compose shape

```kotlin
@Composable
fun WiomButton(
    text: String,
    type: WiomButtonType,              // Primary, Secondary, Tertiary, Destructive, PreBooking
    state: WiomButtonState = WiomButtonState.Default,   // Default, Pressed (auto), Disabled, Loading
    leadingIcon: ImageVector? = null,
    trailingIcon: ImageVector? = null,
    modifier: Modifier = Modifier,
    onClick: () -> Unit,
)

// Acknowledge is two composables sharing state
@Composable
fun WiomAcknowledgeRow(
    text: String,
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit,
    modifier: Modifier = Modifier,
)
```

```kotlin
// Side-by-side pair in a bottom sheet button group
Row(horizontalArrangement = Arrangement.spacedBy(WiomSpacing.m)) {   // 12dp
    WiomButton(
        text = "Cancel",
        type = WiomButtonType.Secondary,
        modifier = Modifier.weight(1f),
        onClick = { },
    )
    WiomButton(
        text = "Confirm",
        type = WiomButtonType.Primary,
        modifier = Modifier.weight(1f),
        onClick = { },
    )
}

// Stacked pair
Column(verticalArrangement = Arrangement.spacedBy(WiomSpacing.m)) {  // 12dp
    WiomButton(
        text = "भुगतान करें",
        type = WiomButtonType.Primary,
        modifier = Modifier.fillMaxWidth(),
        onClick = { },
    )
    WiomButton(
        text = "बाद में",
        type = WiomButtonType.Tertiary,
        modifier = Modifier.fillMaxWidth(),
        onClick = { },
    )
}
```

Do not set `Modifier.height(...)` — buttons hug to 48 dp via padding +
line-height. Do not hardcode width — use `weight(...)` inside a Row or
`fillMaxWidth()` in a Column. See `wiom-compose-layout-rules`.

---

## 9. Acknowledge use cases

1st-person verification copy before an irreversible action.

| Scenario | Copy | Behavior |
|---|---|---|
| Before submitting verified data | "मैंने सभी जानकारी सही दी है" | Unchecked → Primary disabled |
| Before an irreversible action | "मैं इस रिचार्ज को confirm करता/करती हूँ" | Forces active acknowledgment |
| Accepting terms / policy | "मैंने Terms & Conditions पढ़ लिए हैं" | Blocks submission until acknowledged |
| Confirming task completion | "मैंने यह काम पूरा कर लिया है" | Partner confirms work done |
| Handing over credentials | "मैंने कस्टमर को credentials दे दिए हैं" | Accountability before closing |

---

## 10. Pre-booking use cases

Customer-app only. Never for Partner-app buttons, never for generic "Book"
/ "Buy" / "Order" / "Pay". Bright pink is reserved to distinguish pre-booking
from the standard Primary CTA in the same flow.

---

## Quick token lookup

```
ALL CTAs
Radius                  radius-large (16dp)
Padding                 space-12 vertical / space-16 horizontal
Icon                    20dp · icon.sm · Icons.Rounded.* · tint from icon.*
Label                   type.label.lg (16sp SemiBold 600 / 24 LH)
Gap icon-to-label       space-8 (8dp)
Button-group gap        space-12 (12dp)
Shadow                  shadow.none
Sticky bar top border   stroke-small + stroke.subtle (#D7D3E0)

PRIMARY            bg.brand → bg.brand.pressed → bg.disabled    text.onBrand    icon.inverse
SECONDARY          — (transparent; stroke.brand.focus border)   text.brand      icon.brand
TERTIARY           — (transparent; no border)                   text.brand      icon.brand
DESTRUCTIVE        bg.critical → bg.critical.pressed            text.onCritical icon.inverse
PRE-BOOKING        bg.brand.accent → bg.brand.accent.pressed    text.onBrandAccent icon.action
DISABLED (all)     bg.disabled · text.disabled · icon.disabled · stroke.subtle (Sec)
FOCUS RING         stroke.brand.focus (brand CTAs) · stroke.critical.focus (Destructive)
```

---

## See also

- `wiom-design-foundations` → Color tables (actions.md), Component Quick-Build
  Reference (Primary / Secondary / Tertiary / Destructive / Pre-booking)
- `wiom-dialog` — button group pairing inside dialogs (stacked, max 2)
- `wiom-bottomsheet` — button group pairing inside sheets (side-by-side allowed)
- `wiom-selection-controls` — the checkbox used inside Acknowledge
- `wiom-compose-layout-rules` — Modifier discipline, no explicit height on CTAs
