---
name: wiom-dialog
description: >
  Wiom Dialog / Modal — the blocking, centered surface for alerts, input
  capture, single-choice selection, celebratory moments, and foreground loading.
  5 types: Alert, Input, Selection, Illustration, Loading. Use this skill EVERY
  TIME you create, edit, or review any Dialog, AlertDialog, Modal, confirmation
  popup, inline input modal, picker modal, success/celebration dialog, or
  blocking loading screen for the Wiom Partner or Customer app. Trigger on
  Dialog, AlertDialog, ModalBottomSheet used as a full dialog, confirm, cancel,
  celebratory, delete confirmation, OTP input dialog, processing payment modal.
  Pairs with wiom-cta (button group), wiom-input-fields (Input type),
  wiom-list-item + wiom-selection-controls (Selection type), wiom-loader
  (Loading type), and wiom-icon-badge (Alert type).
---

# Wiom Dialog

The modal, blocking, centered surface. Five types cover every confirmation,
capture, and foreground loading moment in the Wiom apps. For transient,
bottom-anchored, or dismissible surfaces use `wiom-bottomsheet` — dialogs are
always modal and always centered.

**Platform:** Android (Jetpack Compose).
**Figma reference:** frame `3069:6004` on the Dialog page (`2151:348`).

---

## The five types

| Type | When to use | Figma node |
|---|---|---|
| **Alert** | Single-thought confirmation with an optional icon-badge — "Payment successful", "Delete this plan?", "Mobile number updated". | `2151:7` |
| **Input** | Capture one input value inline inside the dialog — "Enter mobile number", "Edit display name". Full `wiom-input-fields` row sits in the dialog body. | `2151:44` |
| **Selection** | Pick one item from a short list (≤ 4). Uses `wiom-list-item` with `Type=Radio` for each option. Longer lists → `wiom-bottomsheet`, not Dialog. | `2151:113` |
| **Illustration** | Celebratory or emphasized moment — savings milestone, successful onboarding, achievement. Illustration block replaces icon-badge. | `2151:290` |
| **Loading** | Blocking foreground loading — "Processing payment". Spinner + title + body. Cancel CTA is optional. | `2151:336` |

---

## Shared structure

Every dialog, regardless of type, obeys:

| Property | Value | Token |
|---|---|---|
| Width | 312 dp (fixed) | — |
| Radius | 24 dp | `radius-xlarge` |
| Background | #FAF9FC | `bg.default` |
| Shadow | drop, 0 8 24 0 rgba(0,0,0,0.15) | `shadow.xl` |
| Scrim behind | rgba(22,16,33,0.50) | `overlay.scrim` |
| Padding | 24 dp all sides | `space-24` |
| Main content internal gap | 16 dp | `space-16` |
| Title + body internal gap | 8 dp | `space-8` |
| Main → button-group gap (Alert) | 48 dp | `space-48` |
| Main → button-group gap (all others) | 24 dp | `space-24` |
| Title | Bold 20 / 28 LH | `type.heading.md` + `text.default` |
| Body | Regular 14 / 20 LH | `type.body.md` + `text.subtle` |

**Alert is the only type with 48 dp main-to-buttons spacing.** Every other type
ships 24 dp. This is deliberate — the Alert icon-badge + terse copy leaves more
visual weight at the top and balances with a larger button-group gap.

**Shadow + scrim, no border.** Cross-rule: shadow XOR border. Dialog is always
shadow-xl with scrim; never add a stroke.

---

## Per-type anatomy

### 1. Alert

```
┌─ 312w ─────────────────────────────┐
│  [ Icon Badge · md · neutral ]     │  ← 48 dp icon-with-bg
│                                    │  gap 16
│  Payment successful                │  ← type.heading.md · text.default
│  Your recharge of ₹499 has been    │  gap 8
│  credited to your account.         │  ← type.body.md · text.subtle
│                                    │
│  ──────── 48 dp gap ─────────      │
│                                    │
│  ┌────── Primary CTA ──────┐       │
│  └─────────────────────────┘       │
│  ┌────── Tertiary CTA ─────┐       │
│  └─────────────────────────┘       │
└────────────────────────────────────┘
```

- Icon-badge is `wiom-icon-badge` at `size = md`. Default tone is `neutral`; use
  `positive` / `warning` / `critical` / `info` when the alert itself is
  status-coded (success, warning, error, info).
- Text alignment: **left**.
- Button group: 1 CTA (dismiss) or 2 CTAs (Primary action + Tertiary cancel),
  stacked, full-width. Destructive confirmation uses Destructive CTA as the
  primary (with a Tertiary "Cancel" below); never pair Destructive with
  Primary — Destructive **is** the primary in that group.

### 2. Input

```
┌─ 312w ─────────────────────────────┐
│  Enter mobile number               │  ← type.heading.md
│  We'll send a verification code... │  gap 8 · type.body.md · text.subtle
│                                    │  gap 16
│  Label                             │  ← Field/Input component (wiom-input-fields)
│  ┌─────────────────────────┐       │
│  │ [icon] Enter your value │       │
│  └─────────────────────────┘       │
│  Helper text                       │
│                                    │
│  ──────── 24 dp gap ─────────      │
│                                    │
│  [ Primary CTA ]                   │
│  [ Tertiary CTA ]                  │
└────────────────────────────────────┘
```

- Input field is an **instance of `wiom-input-fields`**. Do not re-implement the
  field anatomy here — cite that skill.
- Text alignment: **left**. Input fill-width.
- Use only for **one** input value. Two or more fields means the flow belongs
  on a dedicated screen or bottom sheet, not a dialog.

### 3. Selection

```
┌─ 312w ─────────────────────────────┐
│  Choose your language              │  ← type.heading.md
│  You can change this anytime...    │  gap 8 · type.body.md · text.subtle
│                                    │  gap 16
│  ( ) English                       │  ← List Item · Type=Radio
│  ( ) हिन्दी                        │  ← List Item · Type=Radio
│  ( ) मराठी                         │  ← List Item · Type=Radio
│                                    │
│  ──────── 24 dp gap ─────────      │
│                                    │
│  [ Primary CTA — Confirm ]         │
│  [ Tertiary CTA — Cancel ]         │
└────────────────────────────────────┘
```

- Each row is an instance of `wiom-list-item` with `Type=Radio`. List gap
  between items is `space-8`. Row padding is owned by List Item.
- Text alignment: **left**.
- **List length cap: 4 items.** Longer lists → use `wiom-bottomsheet` with a
  picker pattern instead. A scrollable area inside a dialog is an anti-pattern.

### 4. Illustration

```
┌─ 312w ─────────────────────────────┐
│  ┌─── 144 dp tall ─────────┐       │  ← illustration block
│  │    bg.brand.subtle      │       │    radius-large (16)
│  │    [ 48dp icon/illus ]  │       │    bg.brand.subtle
│  └─────────────────────────┘       │
│                                    │  gap 16
│         You saved ₹150             │  ← type.heading.md · text.default · CENTER
│         this month!                │
│  Keep recharging on time to...     │  gap 8 · type.body.md · text.subtle · CENTER
│                                    │
│  ──────── 24 dp gap ─────────      │
│                                    │
│  [ Primary CTA ]                   │
│  [ Tertiary CTA ]                  │
└────────────────────────────────────┘
```

- Illustration container: full-width, 144 dp tall, `radius-large` (16 dp),
  `bg.brand.subtle` by default. Other subtle-bgs allowed for tone
  (`bg.positive.subtle` for a savings/achievement, `bg.info.subtle` for
  informational celebrations).
- Inner content can be a 48 dp icon (`Icons.Rounded.Celebration`,
  `Icons.Rounded.EmojiEvents`, etc.) or a proper illustration drawable
  (`ic_wiom_illus_*`). When using a drawable, keep it proportionally sized to
  sit inside the 144 dp container with comfortable padding.
- Text alignment: **center** — this is the distinguishing feature of
  Illustration vs Alert.

### 5. Loading

```
┌─ 312w ─────────────────────────────┐
│          [ spinner 20dp ]          │  ← wiom-loader · Spinner · md · brand
│                                    │  gap 16
│         Processing payment         │  ← type.heading.md · text.default · CENTER
│  Please wait while we confirm...   │  gap 8 · type.body.md · text.subtle · CENTER
│                                    │
│  ──────── 24 dp gap ─────────      │
│                                    │
│  [ Tertiary CTA — Cancel ]  ← optional
└────────────────────────────────────┘
```

- Spinner is an instance of `wiom-loader` (Type=Spinner, size=md, color=brand).
  Container frame is 40 dp with the 20 dp spinner centered.
- Text alignment: **center**.
- Button group: **Tertiary "Cancel" only**, and it is **optional**. If the
  operation cannot be cancelled, omit the button group entirely. Never use
  Primary or Destructive in a Loading dialog — the user hasn't actioned
  anything to confirm.
- Loading dialog must be **dismissible on error** (error routes to a new Alert
  dialog). It must NEVER trap the user; always ship a fallback error path.

---

## Button group rules

Dialogs embed `wiom-cta` via a button-group surface. Rules:

- **Layout:** always **stacked**, never side-by-side. The 312 dp width doesn't
  leave enough room for two readable side-by-side CTAs.
- **Max 2 CTAs.** Primary + Tertiary is the default. Primary + Destructive is
  NOT a valid pair — Destructive replaces Primary as the primary of the group.
- **Button-group gap:** 12 dp between CTAs.
- **Full width** — each CTA fills the dialog's internal width (312 − 2×24 = 264
  dp content width).
- **Loading type is the exception** — 0 or 1 CTA only.

Canonical patterns:

| Dialog intent | Button group |
|---|---|
| Confirm a neutral action ("Save", "Continue", "OK") | Primary only |
| Confirm + offer cancel ("Save" / "Cancel") | Primary + Tertiary |
| Delete / destructive ("Delete plan" / "Cancel") | Destructive + Tertiary |
| Celebratory acknowledge ("View plan" / "Dismiss") | Primary + Tertiary |
| Loading (cancellable) | Tertiary "Cancel" only |
| Loading (non-cancellable) | No buttons |

---

## When to use Dialog vs Bottom Sheet

```
Blocking, single-thought, centered, requires explicit dismiss → Dialog
Transient, bottom-anchored, swipe-to-dismiss, multi-item content → Bottom Sheet
Picker with > 4 items → Bottom Sheet (not Dialog Selection)
Any input capture with > 1 field → new screen or Bottom Sheet Form
Celebratory one-screen moment → Dialog Illustration
Confirmation with irreversible action → Dialog Alert (destructive)
Foreground loading that blocks the screen → Dialog Loading
Inline activity spinner (non-blocking) → wiom-loader (not a Dialog)
```

**Hard rule:** if the surface is swipeable / handle-dragged / dismissible by
tapping outside, it's a bottom sheet. Dialog has a scrim but the scrim is opaque
to dismissal unless the flow allows it — always provide an explicit CTA.

---

## Wiom use cases

**Alert**
- Payment success confirmation (after recharge, plan activation, add-on
  purchase).
- Delete plan / remove partner / cancel auto-debit — destructive confirmation.
- Mobile number / email updated confirmation.
- Session expiry notice ("You've been logged out").
- Error resolution ("Couldn't connect — retry?").

**Input**
- Enter mobile number before OTP screen (single field, dialog-sized capture).
- Edit display name (single field).
- Enter referral code.
- Enter pincode for service availability check.

**Selection**
- Choose language (English / हिन्दी / मराठी + one more).
- Select payment method when retrying a failed transaction (short list: UPI /
  Card / Cash).
- Choose notification frequency (Immediately / Daily / Weekly / Never).
- Quick filter switch (Sort by: Recommended / Cheapest / Newest).

**Illustration**
- Savings milestone ("You saved ₹X this month!").
- First-recharge celebration.
- Achievement unlock (loyalty, streak, milestone).
- Completed onboarding success.

**Loading**
- Processing payment (UPI redirect, card auth).
- Confirming plan change.
- Submitting KYC documents.
- Syncing partner inventory — any multi-second non-interruptible op that needs
  to block the UI.

---

## Figma → Compose mapping

Figma variants on node `2151:348`:

| Figma variant | Compose parameter |
|---|---|
| `Type=Alert` | `type = DialogType.Alert` |
| `Type=Input` | `type = DialogType.Input` |
| `Type=Selection` | `type = DialogType.Selection` |
| `Type=Illustration` | `type = DialogType.Illustration` |
| `Type=Loading` | `type = DialogType.Loading` |
| `Actions=Single` (stacked button group) | Button group owned by the Dialog slot API |

Figma ships `Actions=Single` only; side-by-side button groups are intentionally
excluded at dialog width.

---

## Minimal Compose shape

Slot-based so each type composes the right neighbours without this skill
re-implementing them.

```kotlin
@Composable
fun WiomDialog(
    type: DialogType,
    onDismiss: () -> Unit,
    modifier: Modifier = Modifier,
    header: @Composable (() -> Unit)? = null,     // icon-badge (Alert), illustration (Illus), spinner (Loading)
    title: String,
    body: String? = null,
    content: @Composable (ColumnScope.() -> Unit)? = null,  // input field (Input), list (Selection)
    buttons: @Composable (ColumnScope.() -> Unit)? = null,  // 0–2 CTAs, stacked
)

enum class DialogType { Alert, Input, Selection, Illustration, Loading }
```

Notes:

- Compose surface is a `Dialog` (Compose Material) wrapping a `Surface` with
  `bg.default`, `radius-xlarge`, `shadow.xl`, width 312 dp, padding 24 dp all
  sides.
- `header` slot receives a `wiom-icon-badge` (Alert), illustration container
  (Illustration), or a `wiom-loader` spinner (Loading). For Input and Selection,
  leave `header = null`.
- `content` slot receives a `wiom-input-fields` (Input) or a column of
  `wiom-list-item` rows with `Type=Radio` (Selection). Leave `null` for Alert,
  Illustration, and Loading.
- `buttons` slot receives 0–2 `wiom-cta` instances. Always stacked, gap 12 dp.
- **Alignment per type is handled internally by the dialog**, keyed off `type`
  (left for Alert/Input/Selection; center for Illustration/Loading).

**Do not expose** width, height, radius, shadow, or padding parameters. The
dialog has one width and one radius.

---

## Anti-pattern checklist

- [ ] Dialog width other than 312 dp
- [ ] Border or stroke on the dialog (shadow only)
- [ ] Side-by-side button group inside a dialog (stacked only at dialog width)
- [ ] More than 2 CTAs in the button group
- [ ] Primary + Destructive paired (Destructive replaces Primary)
- [ ] Primary or Destructive in a Loading dialog (Tertiary Cancel or none)
- [ ] Scrollable content inside Dialog Selection (use `wiom-bottomsheet`
      when list > 4)
- [ ] More than one input field in Dialog Input (use a screen or Bottom Sheet
      Form)
- [ ] Re-implementing the input field, list row, spinner, or icon-badge
      instead of instancing `wiom-input-fields` / `wiom-list-item` /
      `wiom-loader` / `wiom-icon-badge`
- [ ] Loading dialog with no error fallback path
- [ ] `type.heading.lg` (24 Bold) for the title (should be `type.heading.md`
      20 Bold — dialog-specific override of the general page-title default)
- [ ] Non-centered alignment for Illustration or Loading (both are center)
- [ ] Non-left alignment for Alert, Input, or Selection
- [ ] Adding Hover or Focused states to the dialog surface itself (it isn't
      interactive — the CTAs inside are)

---

## See also

- `wiom-design-foundations` → Dialog padding / radius / shadow / scrim tokens;
  typography heading.md; Component Quick-Build Reference entries for inner
  components
- `wiom-cta` — button group embedded at the bottom; stacked rules
- `wiom-input-fields` — Input dialog's inline field
- `wiom-list-item` + `wiom-selection-controls` — Selection dialog rows
- `wiom-loader` — Loading dialog spinner (Spinner, md, brand)
- `wiom-icon-badge` — Alert dialog header icon at `size = md`
- `wiom-bottomsheet` — the non-dialog surface for picker lists, multi-field
  input, and transient confirmations
- `wiom-compose-layout-rules` — no explicit widths on content frames,
  Arrangement.spacedBy for gaps, stacked layout
