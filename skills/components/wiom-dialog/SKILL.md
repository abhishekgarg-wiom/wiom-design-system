---
name: wiom-dialog
description: >
  Wiom Dialog — the modal, blocking, centered surface for confirmation,
  capture, choice, celebration, and foreground loading. 1 component,
  5 ship variants (Type = Alert · Input · Selection · Illustration ·
  Loading), default `Actions = Single` button group (Stacked by
  default; Side-by-side supported via the button-group instance's
  Layout switch when both CTAs are short). Width 312 dp · radius 24 ·
  `shadow.xl` · centered with scrim. Use this skill EVERY TIME you create, edit, or review
  any Dialog, AlertDialog, modal popup, confirmation popup, single-
  field input modal, ≤4-item picker, success/celebration moment, or
  blocking loading screen for the Wiom Partner or Customer Android
  app, or when assembling Figma frames with a centered modal. Trigger
  on: Dialog, AlertDialog, Modal, Popup, confirm, cancel, celebratory,
  delete confirmation, success dialog, processing payment, OTP input
  modal, picker modal, instance of 2151:348. Pairs with wiom-cta
  (button group), wiom-input-fields (Input type body), wiom-list-item
  + wiom-selection-controls (Selection type body), wiom-loader
  (Loading type spinner), and wiom-icon-badge (Alert type header).
---

# Wiom Dialog

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
set `2151:348` (5 type variants), and the `wiom-design-foundations`
token set.

**1 component · 5 ship variants · Stacked button group by default; Side-by-side supported.**

| Type | Header | Text alignment | Body content | Node |
|---|---|---|---|---|
| `Alert` | 48 dp icon-with-bg | left | Title + body | `2151:7` |
| `Input` | (none) | left | Title + body + `wiom-input-fields` row | `2151:44` |
| `Selection` | (none) | left | Title + body + ≤ 4 `wiom-list-item` rows (`Type=Radio`) | `2151:113` |
| `Illustration` | 144 dp illustration container | center | Title + body | `2151:290` |
| `Loading` | 40 dp `wiom-loader` Spinner | center | Title + body | `2151:336` |

The dialog is the **modal blocking surface** — always centered, always
behind a scrim, always dismissed by an explicit CTA (or by the system
back gesture, never by tapping the scrim — see §3). For transient,
swipeable, or multi-item surfaces, use `wiom-bottomsheet` instead.

---

## 1. Figma — instancing the component

Instance the component set `2151:348` and set the `Type` variant.
Strings are **case-sensitive**. The dialog component ships only `Actions = Single` as a top-level variant; the embedded button group instance independently exposes its own `Layout = Stacked / Side by side` switch (Stacked is the default — see callout below §1).

### Variants — 5 ship combinations

| `Type` | `Actions` | Node | Use for |
|---|---|---|---|
| `Alert` | `Single` | `2151:7` | Single-thought confirmation — payment success, delete confirm, status update |
| `Input` | `Single` | `2151:44` | Capture **one** input value inline — mobile number, referral code, display name |
| `Selection` | `Single` | `2151:113` | Pick one item from a **short** list (≤ 4) — language, payment method retry |
| `Illustration` | `Single` | `2151:290` | Celebratory / emphasized moment — savings milestone, achievement, completed onboarding |
| `Loading` | `Single` | `2151:336` | Blocking foreground loading — processing payment, syncing, submitting |

### Properties (apply per instance)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Title` | Text | `"Title"` | Dialog title — `type.heading.md` |
| `Body` | Text | `"Body"` | Optional supporting copy — `type.body.md` |
| `Show Link Button` | Boolean | `true` (Loading only) | **Set `false`.** Loading has no button and is never dismissable (as shipped, `WiomDialog.kt:366-381`). Figma's link button is unused |
| `Header swap` (Alert) | Instance swap | `wiom-icon-badge` `size = md` `tone = neutral` | Swap to a status-coded tone when the alert itself is status-typed |
| `Header swap` (Illustration) | Instance swap | 48 dp icon inside `bg.brand.subtle` 144 dp container | Swap to any 48 dp glyph or to a proper illustration drawable |

### Code enum ↔ Figma variant string

| `WiomDialogType` (Kotlin) | Figma `Type` value |
|---|---|
| `Alert` | `Alert` |
| `Input` | `Input` |
| `Selection` | `Selection` |
| `Illustration` | `Illustration` |
| `Loading` | `Loading` |

> **Button-group layout is `Stacked` by default; `Side-by-side` is also supported.** The dialog component ships only `Actions = Single` as a top-level variant, but the embedded button group instance exposes its own `Layout = Stacked / Side by side` switch (see `wiom-cta` button-group spec). At 312 dp dialog width with `space-24` outer padding, content width is 264 dp — side-by-side fits two short, symmetric CTAs (≈ 126 dp each after the 12 dp internal gap), e.g. "Save" / "Cancel". Use **stacked** when labels are longer, when hierarchy matters (Primary above Tertiary), or whenever you're unsure — it's the safer default. Reach for **side-by-side** only when both CTAs are short, parallel actions and stacked feels heavy.

---

## 2. Anatomy

All 5 types share a single envelope:

```
┌────────── 312 dp ───────────────────────┐
│                                          │
│  [ Padding 24 dp on all sides ]          │
│                                          │
│  ┌──────────────────────────────────┐    │
│  │  Header (Alert / Illustration /  │    │
│  │  Loading) — type-specific        │    │
│  └──────────────────────────────────┘    │
│       ↓ space-16 ↓                       │
│  ┌──────────────────────────────────┐    │
│  │  Title    (type.heading.md)      │    │
│  │       ↓ space-8 ↓                │    │
│  │  Body     (type.body.md)         │    │
│  └──────────────────────────────────┘    │
│       ↓ space-16 (Input / Selection) ↓   │
│  ┌──────────────────────────────────┐    │
│  │  Body content                    │    │
│  │  (Input field / Radio list)      │    │
│  └──────────────────────────────────┘    │
│                                          │
│       ↓ space-48 ↓                       │
│  ┌──────── Button Group ────────┐        │
│  │  [ Confirm CTA — fills row ] │        │   Stacked: confirm on top
│  │       ↓ 12 dp ↓              │        │   SideBySide: confirm on right
│  │  [ Dismiss CTA — fills row ] │        │   (CTA types per wiom-cta)
│  └──────────────────────────────┘        │
│                                          │
└──────────────────────────────────────────┘
              [ shadow.xl drop ]
              [ scrim behind: overlay.scrim 50% ]
```

| Slot | Value |
|---|---|
| Width | **312 dp (fixed)** — dialog has one width. Don't override |
| Padding | `space-24` all sides |
| Radius | `radius-xlarge` (24 dp) |
| Surface fill | `bg.default` (#FAF9FC) |
| Surface elevation | `shadow.xl` (drop · color `#00000026` · offset (0, 8) · radius 24 · spread 0) |
| Scrim behind | `overlay.scrim` (#161021 @ 50%) — covers entire screen behind the dialog |
| Stroke | **none** — shadow XOR border; dialog is shadow-only |
| Header → text gap | `space-16` |
| Title → body gap | `space-8` |
| Text → body content gap (Input · Selection) | `space-16` (continues the 16 dp main-internal rhythm) |
| Main content → button group gap | **`space-48` for all types** (48 dp — confirmed in the Figma ship for Alert, Selection, Illustration, Loading; the legacy "Alert only is 48, others 24" claim is incorrect) |
| Button group internal gap | 12 dp (Stacked or Side-by-side) |
| Title typography | `type.heading.md` (Bold 20 / 28 LH) |
| Title color | `text.default` (#161021) |
| Body typography | `type.body.md` (Regular 14 / 20 LH) |
| Body color | `text.subtle` (#5C5570) |

### Per-type header

| Type | Header anatomy |
|---|---|
| `Alert` | 48 dp circular `wiom-icon-badge` · `size = md` · `tone = neutral` (default `bg.subtle` #F1EDF7 fill, `icon.action` glyph) — swap tone for status-coded alerts |
| `Input` | (none — title sits at the top) |
| `Selection` | (none — title sits at the top) |
| `Illustration` | Full-width × **144 dp** container · `radius-large` (16 dp) · `bg.brand.subtle` (#FFE5F6) by default — holds a 48 dp icon or an illustration drawable. **Brand subtle only** (as shipped, `WiomDialog.kt:318-333`); no other tones |
| `Loading` | 40 dp container with a **20 dp** `wiom-loader` Spinner centered (`size = sm`, `color = brand`) |

### Per-type text alignment

- **Left** — Alert · Input · Selection (form-row reading rhythm)
- **Center** — Illustration · Loading (the visual top draws the eye to the centre)

### Per-type body content slot

| Type | Body content |
|---|---|
| `Alert` | (none — Title + Body only) |
| `Input` | One **`wiom-input-fields`** row — Default state, fills width. **Single field only** — multi-field flows belong on a screen or bottom-sheet form |
| `Selection` | A vertical list of **`wiom-list-item`** rows with `Type = Radio` · row-padding `space-16` all sides (owned by the row) · gap between rows `space-8`. **Maximum 4 rows** — longer lists belong in a bottom-sheet picker |
| `Illustration` | (none — Title + Body only) |
| `Loading` | (none — spinner header carries the visual; Title + Body convey progress copy) |

---

## 3. Surface contract — modal, centered, scrim, system-back-only dismiss

The dialog is a **modal surface** stacked above the screen content. It is **not edge-to-edge** — the scrim covers the screen, the dialog sits centered with breathing room. Standard 16 dp screen gutters do **not** apply: the dialog has a fixed 312 dp width and self-centers.

### What this means in code

- **Wrap with the platform `Dialog`** (Compose Material) so the system handles focus, accessibility, and back-press dismissal.
- **`Modifier.width(312.dp)`** on the inner `Surface`. Don't `fillMaxWidth()` — the dialog is *intentionally* narrower than the screen.
- **Scrim** comes free with `Dialog(...)` — the system paints the standard scrim. Its colour must resolve to foundations' `overlay.scrim` (#161021 @ 50%) — not the platform's neutral black. Don't paint a manual `Box` scrim on top.
- **Dismiss via explicit CTA + system back, not scrim tap.** Set `DialogProperties(dismissOnClickOutside = false, dismissOnBackPress = true)` so users can't accidentally tap-out and lose state mid-Input or mid-Selection. System back is always allowed (and should route to the dismiss action's `onClick` for parity, regardless of which CTA type the dismiss slot uses).

### Stacking with other surfaces

- A dialog over a **bottom sheet** is allowed (sheet fades behind the scrim).
- A dialog over **another dialog** is **not** allowed — single dialog at a time. If two confirmations are needed in sequence, dismiss the first before showing the second.
- A **toast** displayed while a dialog is open lands above the dialog (toast is a foreground notification surface). Avoid this pairing where possible — the user already has one focal action.

---

## 4. Decision logic — Dialog vs Bottom Sheet vs other

```
What is the surface for?

A blocking, centered, single-thought action
   that requires explicit dismiss?
   → Dialog

A transient, bottom-anchored, swipe-to-dismiss,
   multi-item or longer content surface?
   → wiom-bottomsheet

A picker / chooser?
   ≤ 4 items                 → Dialog (Selection)
   > 4 items                 → wiom-bottomsheet (picker pattern)

Capture user input?
   1 field, simple validation → Dialog (Input)
   2+ fields, or wizard       → dedicated screen, or wiom-bottomsheet (form)

Celebrate / acknowledge a moment?
   → Dialog (Illustration)

Confirm a destructive action?
   → Dialog (Alert) — pick CTA types per `wiom-cta`

Foreground loading that blocks the UI?
   → Dialog (Loading)  — must always have an error fallback path

Inline / non-blocking activity?
   → wiom-loader (not a Dialog)
```

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

| Slot | Token (light theme) |
|---|---|
| Surface fill | `bg.default` (#FAF9FC) |
| Surface radius | `radius-xlarge` (24 dp) |
| Surface shadow | `shadow.xl` (offset 0, 8 · radius 24 · color #00000026) |
| Scrim | `overlay.scrim` (#161021 @ 50%) — provided by the platform `Dialog` |
| Padding | `space-24` |
| Header → text gap | `space-16` |
| Title → body gap | `space-8` |
| Text → body-content gap (Input / Selection) | `space-16` |
| Main → button-group gap | `space-48` (all 5 types) |
| Button-group internal gap | 12 dp (Stacked or Side-by-side) |
| Title typography | `type.heading.md` (Bold 20 / 28 LH) |
| Title color | `text.default` (#161021) |
| Body typography | `type.body.md` (Regular 14 / 20 LH) |
| Body color | `text.subtle` (#5C5570) |
| Alert header (icon-with-bg) — fill | `bg.subtle` (#F1EDF7) by default; status-coded tone subtles otherwise |
| Alert header — icon tint | `icon.action` (#352D42) by default; status-coded tone otherwise |
| Illustration container — fill | `bg.brand.subtle` (#FFE5F6) by default |
| Illustration container — radius | `radius-large` (16 dp) |
| Illustration container — height | 144 dp |
| Loader container | 40 dp · transparent · centered Spinner inside |
| Loader spinner | 20 dp · `wiom-loader` Type=Spinner · `size = sm` · `color = brand` |

No stroke. No tonal elevation tint. Element-first only — don't deepen colours to "match a screen".

---

## 6. Universal rules

1. **Width is 312 dp, fixed.** Don't `fillMaxWidth()`, don't `widthIn`. Dialog has one width across every type.
2. **Shadow XOR border.** Dialog ships with `shadow.xl` and **no stroke**. Never add a border.
3. **Scrim is owned by the platform Dialog.** Don't paint your own scrim Box; configure dismiss behaviour via `DialogProperties` instead.
4. **Don't dismiss on scrim tap.** Set `dismissOnClickOutside = false`. Users always exit via an explicit CTA (or system back, which should mirror the dismiss action's `onClick` regardless of which CTA type fills that slot).
5. **One dialog at a time.** Sequential confirmations dismiss the first before showing the second. Never stack dialogs.
6. **Max 2 CTAs · Loading none · Stacked is the default; Side-by-side allowed when both labels are short.** Alert / Input / Selection / Illustration use 1 or 2 CTAs; Loading uses 0 (no button; never dismissable, as shipped). The button group's `Layout` is **Stacked** by default — switch to **Side-by-side** only when both CTAs are short, parallel actions ("Save" / "Cancel", "Yes" / "No"); otherwise the ~126 dp per-button width truncates labels. For CTA-vs-CTA pairing rules (Primary vs Destructive, when to use Tertiary for Cancel / Dismiss / Skip, copy discipline) see `wiom-cta` — those are owned by the CTA skill, not duplicated here.
7. **Loading dialog must have an error fallback.** It has no button and ignores back-press and scrim tap, so every long-running operation must route errors and timeouts to a follow-up Alert dialog so the user can recover. Aligned with the shipped library and iOS skills, 26 Nov 2026.
8. **Body components are skill-instances, not re-implementations.** Input row → `wiom-input-fields`. Selection row → `wiom-list-item` (Type=Radio). Spinner → `wiom-loader`. Header badge → `wiom-icon-badge`. Don't re-paint these inside the dialog.
9. **Selection caps at 4 items.** A scrollable list inside a dialog is an anti-pattern; that pattern belongs in a bottom-sheet picker.
10. **Input caps at 1 field.** Two-or-more-field flows belong on a dedicated screen or in a bottom-sheet form.
11. **Tokens only — element-first.** No raw hex; no `Modifier.background(Color(0xFFFAF9FC))`. Use `Wiom.bg.default`, `Wiom.shadow.xl`, etc.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomDialogType { Alert, Input, Selection, Illustration, Loading }
enum class WiomDialogButtonsLayout { Stacked, SideBySide }

@Composable
fun WiomDialog(
    type: WiomDialogType,
    title: String,
    onDismiss: () -> Unit,
    body: String? = null,                                // optional supporting copy
    header: @Composable (() -> Unit)? = null,            // Alert / Illustration / Loading content
    content: @Composable (ColumnScope.() -> Unit)? = null, // Input field / Selection list
    buttons: @Composable (() -> Unit)? = null,           // 0..2 CTAs — laid out by buttonsLayout
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
    backDismissible: Boolean = true,                     // false = blocking (non-cancellable Loading)
    modifier: Modifier = Modifier,
)
```

The single `WiomDialog` is slot-based. The `buttons` slot is laid out
by `buttonsLayout` — Stacked (default, vertical 12 dp gap) or
SideBySide (horizontal 12 dp gap, each CTA `weight(1f)`). Convenience
helpers (`WiomAlertDialog`, `WiomLoadingDialog`, etc.) compose around
this with the right `header` / `content` / `buttons` slots wired up
and forward `buttonsLayout` where it makes sense.

### Implementation

```kotlin
@Composable
fun WiomDialog(
    type: WiomDialogType,
    title: String,
    onDismiss: () -> Unit,
    body: String? = null,
    header: @Composable (() -> Unit)? = null,
    content: @Composable (ColumnScope.() -> Unit)? = null,
    buttons: @Composable (() -> Unit)? = null,
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
    backDismissible: Boolean = true,                // false = blocking (non-cancellable Loading)
    modifier: Modifier = Modifier,
) {
    val centerContent = type == WiomDialogType.Illustration || type == WiomDialogType.Loading
    val horizontalAlignment =
        if (centerContent) Alignment.CenterHorizontally else Alignment.Start
    val textAlign =
        if (centerContent) TextAlign.Center else TextAlign.Start

    Dialog(
        onDismissRequest = onDismiss,
        properties = DialogProperties(
            dismissOnClickOutside = false,          // §3: scrim tap doesn't dismiss
            dismissOnBackPress = backDismissible,   // non-cancellable Loading sets false
            usePlatformDefaultWidth = false,         // we set 312 dp ourselves
        ),
    ) {
        // NOTE on shadow.xl: foundations spec is offset (0, 8) · radius 24 · #00000026.
        // Compose `Surface.shadowElevation` honours elevation but flattens offset; the
        // visual is close enough at 8 dp. If foundations later exposes a `shadow.xl.elevation: Dp`
        // accessor, swap to it.
        Surface(
            color = Wiom.bg.default,
            shape = RoundedCornerShape(24.dp),
            tonalElevation = 0.dp,                  // foundations: no tint
            shadowElevation = 8.dp,
            modifier = modifier.width(312.dp),
        ) {
            Column(
                modifier = Modifier.padding(Wiom.space.s24),
                horizontalAlignment = horizontalAlignment,
                verticalArrangement = Arrangement.spacedBy(Wiom.space.s48),
            ) {
                // Main block — header (optional) + title + body + content (optional)
                Column(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalAlignment = horizontalAlignment,
                    verticalArrangement = Arrangement.spacedBy(Wiom.space.s16),
                ) {
                    header?.invoke()
                    Column(
                        modifier = Modifier.fillMaxWidth(),
                        horizontalAlignment = horizontalAlignment,
                        verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
                    ) {
                        Text(
                            text = title,
                            style = Wiom.type.heading.md,
                            color = Wiom.text.default,
                            textAlign = textAlign,
                            modifier = Modifier.fillMaxWidth(),
                        )
                        if (body != null) {
                            Text(
                                text = body,
                                style = Wiom.type.body.md,
                                color = Wiom.text.subtle,
                                textAlign = textAlign,
                                modifier = Modifier.fillMaxWidth(),
                            )
                        }
                    }
                    if (content != null) {
                        Column(
                            modifier = Modifier.fillMaxWidth(),
                            verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
                            content = content,
                        )
                    }
                }
                // Button group — Stacked (default) or SideBySide; 12 dp gap either way.
                // The caller passes 0..2 CTAs each with `Modifier.fillMaxWidth()`. In SideBySide,
                // each CTA must additionally apply `Modifier.weight(1f)` (see helpers below) so the
                // 264 dp content width is split evenly with a 12 dp gap.
                if (buttons != null) {
                    when (buttonsLayout) {
                        WiomDialogButtonsLayout.Stacked -> Column(
                            modifier = Modifier.fillMaxWidth(),
                            horizontalAlignment = Alignment.CenterHorizontally,
                            verticalArrangement = Arrangement.spacedBy(12.dp),
                        ) { buttons() }
                        WiomDialogButtonsLayout.SideBySide -> Row(
                            modifier = Modifier.fillMaxWidth(),
                            horizontalArrangement = Arrangement.spacedBy(12.dp),
                            verticalAlignment = Alignment.CenterVertically,
                        ) { buttons() }
                    }
                }
            }
        }
    }
}
```

### Convenience helpers — one per type

```kotlin
@Composable
fun WiomAlertDialog(
    title: String,
    body: String?,
    onDismiss: () -> Unit,
    icon: @DrawableRes Int = R.drawable.ic_settings_rounded,    // swap per use case
    iconBgTone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    confirm: WiomCtaSpec? = null,                                 // prominent slot — type per wiom-cta
    dismiss: WiomCtaSpec? = null,                                 // secondary slot — type per wiom-cta
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
) {
    WiomDialog(
        type = WiomDialogType.Alert,
        title = title,
        body = body,
        onDismiss = onDismiss,
        buttonsLayout = buttonsLayout,
        header = { WiomIconBadge(iconRes = icon, size = WiomIconBadgeSize.Md, tone = iconBgTone) },
        buttons = if (confirm != null || dismiss != null) {
            { CtaPair(confirm, dismiss, buttonsLayout) }
        } else null,
    )
}

@Composable
fun WiomInputDialog(
    title: String,
    body: String?,
    field: @Composable () -> Unit,                              // a WiomInput instance
    onDismiss: () -> Unit,
    confirm: WiomCtaSpec,
    dismiss: WiomCtaSpec? = null,
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
) {
    WiomDialog(
        type = WiomDialogType.Input,
        title = title,
        body = body,
        onDismiss = onDismiss,
        content = { field() },
        buttonsLayout = buttonsLayout,
        buttons = { CtaPair(confirm, dismiss, buttonsLayout) },
    )
}

@Composable
fun WiomSelectionDialog(
    title: String,
    body: String?,
    options: List<String>,                                       // ≤ 4
    selectedIndex: Int,
    onSelect: (Int) -> Unit,
    onDismiss: () -> Unit,
    confirm: WiomCtaSpec,
    dismiss: WiomCtaSpec? = null,
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
) {
    require(options.size in 1..4) {
        "Selection dialog supports 1..4 options; for longer lists use a bottom-sheet picker"
    }
    WiomDialog(
        type = WiomDialogType.Selection,
        title = title,
        body = body,
        onDismiss = onDismiss,
        buttonsLayout = buttonsLayout,
        content = {
            options.forEachIndexed { i, label ->
                WiomListItem(
                    type = WiomListItemType.Radio,
                    label = label,
                    selected = i == selectedIndex,
                    onClick = { onSelect(i) },
                )
            }
        },
        buttons = { CtaPair(confirm, dismiss, buttonsLayout) },
    )
}

@Composable
fun WiomIllustrationDialog(
    title: String,
    body: String?,
    @DrawableRes illustrationRes: Int = R.drawable.ic_celebration_rounded,
    onDismiss: () -> Unit,
    confirm: WiomCtaSpec,
    dismiss: WiomCtaSpec? = null,
    buttonsLayout: WiomDialogButtonsLayout = WiomDialogButtonsLayout.Stacked,
) {
    WiomDialog(
        type = WiomDialogType.Illustration,
        title = title,
        body = body,
        onDismiss = onDismiss,
        buttonsLayout = buttonsLayout,
        header = {
            Box(
                modifier = Modifier
                    .fillMaxWidth()
                    .height(144.dp)
                    .clip(RoundedCornerShape(16.dp))               // radius-large
                    .background(Wiom.bg.brand.subtle),           // brand subtle only
                contentAlignment = Alignment.Center,
            ) {
                Icon(
                    painter = painterResource(illustrationRes),
                    contentDescription = null,
                    tint = Wiom.icon.brand,
                    modifier = Modifier.size(48.dp),
                )
            }
        },
        buttons = { CtaPair(confirm, dismiss, buttonsLayout) },
    )
}

@Composable
fun WiomLoadingDialog(
    title: String,
    body: String?,
) {
    // No button, never dismissable: back-press and scrim tap do nothing (as shipped).
    WiomDialog(
        type = WiomDialogType.Loading,
        title = title,
        body = body,
        onDismiss = { /* no-op */ },
        backDismissible = false
        header = {
            Box(modifier = Modifier.size(40.dp), contentAlignment = Alignment.Center) {
                WiomLoader(type = WiomLoaderType.Spinner, size = WiomLoaderSize.Sm)
            }
        },
        buttons = null,
    )
}

// Internal helper — renders 0..2 CTAs with 12 dp gap. Each spec's `type` is honoured
// verbatim; this helper does NOT impose a default like "second CTA must be Tertiary".
// Pairing rules (which type belongs in which slot, when Destructive replaces Primary,
// when Cancel uses Secondary vs Tertiary, copy discipline) live in `wiom-cta`.
//
//   Stacked    → confirm on top, dismiss below; each CTA `fillMaxWidth()`.
//   SideBySide → confirm on the right, dismiss on the left (Wiom button-group convention);
//                each CTA uses `Modifier.weight(1f)` so the 264 dp content width splits
//                evenly with the 12 dp gap (≈126 dp per cell).
@Composable
private fun CtaPair(
    confirm: WiomCtaSpec?,            // prominent slot — typically Primary or Destructive
    dismiss: WiomCtaSpec?,            // secondary slot: Secondary when paired (D51); Tertiary only when alone
    layout: WiomDialogButtonsLayout,
) {
    when (layout) {
        WiomDialogButtonsLayout.Stacked -> {
            confirm?.let { spec ->
                WiomCta(
                    type = spec.type,                              // honoured verbatim — see wiom-cta
                    label = spec.label,
                    onClick = spec.onClick,
                    modifier = Modifier.fillMaxWidth(),
                )
            }
            dismiss?.let { spec ->
                WiomCta(
                    type = spec.type,                              // honoured verbatim — see wiom-cta
                    label = spec.label,
                    onClick = spec.onClick,
                    modifier = Modifier.fillMaxWidth(),
                )
            }
        }
        WiomDialogButtonsLayout.SideBySide -> {
            // Order: dismiss on the left, confirm on the right (Wiom button-group convention)
            dismiss?.let { spec ->
                WiomCta(
                    type = spec.type,
                    label = spec.label,
                    onClick = spec.onClick,
                    modifier = Modifier.weight(1f),                // works because parent is a Row
                )
            }
            confirm?.let { spec ->
                WiomCta(
                    type = spec.type,
                    label = spec.label,
                    onClick = spec.onClick,
                    modifier = Modifier.weight(1f),
                )
            }
        }
    }
}

data class WiomCtaSpec(
    val label: String,
    val onClick: () -> Unit,
    val type: WiomCtaType,                                        // any type from wiom-cta — caller decides
)
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material AlertDialog (wrong width / radius / shadow), scrim-tap dismiss
//             allowed mid-Input, side-by-side buttons crammed at dialog width
AlertDialog(
    onDismissRequest = { /* scrim tap closes the dialog! */ },    // dismissOnClickOutside default true
    title = { Text("Enter mobile") },
    text = { TextField(value = number, onValueChange = setNumber) }, // Material TextField, not WiomInput
    confirmButton = { Button(onClick = onConfirm) { Text("OK") } }, // side-by-side
    dismissButton = { Button(onClick = onCancel) { Text("Cancel") } },
)

// ✅ RIGHT — WiomInputDialog wraps the platform Dialog; field is a WiomInput instance;
//             scrim tap is consumed; CTA pairing types come from wiom-cta (any combo valid).
WiomInputDialog(
    title = "Enter mobile number",
    body = "We'll send a verification code to verify it's you.",
    field = {
        WiomInput(
            label = "Mobile",
            value = number,
            onValueChange = setNumber,
            keyboardType = KeyboardType.Phone,
        )
    },
    onDismiss = onCancel,
    confirm = WiomCtaSpec("Send OTP", onConfirm, type = WiomCtaType.Primary),
    dismiss = WiomCtaSpec("Cancel", onCancel, type = WiomCtaType.Secondary),
)
```

---

## 8. Screen-level recipes

> **Two-CTA pairings: Primary + Secondary, or Destructive + Secondary** (owner decision 27 Nov 2026, same on iOS, D51). A single acknowledgement is a Tertiary alone ("ठीक है"). Defer to `wiom-cta` for everything else.

### Alert — Payment success (Primary + Secondary, stacked)

```kotlin
WiomAlertDialog(
    title = "Payment successful",
    body = "Your recharge of ₹499 has been credited to your account.",
    icon = R.drawable.ic_check_circle_rounded,
    iconBgTone = WiomIconBadgeTone.Positive,
    onDismiss = onClose,
    confirm = WiomCtaSpec("View receipt", onViewReceipt, type = WiomCtaType.Primary),
    dismiss = WiomCtaSpec("Dismiss", onClose, type = WiomCtaType.Secondary),
)
```

### Alert — Destructive confirmation (Destructive + Secondary, stacked)

```kotlin
WiomAlertDialog(
    title = "Delete this plan?",
    body = "Your saved schedule and preferences will be removed. This can't be undone.",
    icon = R.drawable.ic_delete_rounded,
    iconBgTone = WiomIconBadgeTone.Critical,
    onDismiss = onCancel,
    confirm = WiomCtaSpec("Delete", onDelete, type = WiomCtaType.Destructive),
    dismiss = WiomCtaSpec("Cancel", onCancel, type = WiomCtaType.Secondary),
)
```

### Alert — Side-by-side button group (short, parallel labels — Destructive + Secondary)

```kotlin
// Use SideBySide only when both labels are short ("Save" / "Discard", "Yes" / "No").
// Each cell becomes ~126 dp; longer labels truncate. Pairing types are flexible — here
// the dismiss slot uses Secondary (outlined) for stronger visual presence side-by-side.
WiomAlertDialog(
    title = "Discard changes?",
    body = "Your unsaved edits will be lost.",
    icon = R.drawable.ic_warning_rounded,
    iconBgTone = WiomIconBadgeTone.Warning,
    onDismiss = onCancel,
    confirm = WiomCtaSpec("Discard", onDiscard, type = WiomCtaType.Destructive),
    dismiss = WiomCtaSpec("Cancel", onCancel, type = WiomCtaType.Secondary),
    buttonsLayout = WiomDialogButtonsLayout.SideBySide,
)
```

### Selection — Choose language (Primary + Secondary)

```kotlin
val languages = listOf("English", "हिन्दी", "मराठी")
var selected by remember { mutableStateOf(0) }

WiomSelectionDialog(
    title = "Choose your language",
    body = "You can change this anytime from settings.",
    options = languages,
    selectedIndex = selected,
    onSelect = { selected = it },
    onDismiss = onCancel,
    confirm = WiomCtaSpec("Confirm", { onConfirm(languages[selected]) }, type = WiomCtaType.Primary),
    dismiss = WiomCtaSpec("Cancel", onCancel, type = WiomCtaType.Secondary),
)
```

### Illustration — Savings milestone (Primary only — no dismiss CTA)

```kotlin
// Single CTA is also valid — leave `dismiss` null. System back / scrim still close
// the dialog (subject to `dismissOnClickOutside = false` for input-bearing dialogs).
WiomIllustrationDialog(
    title = "You saved ₹150 this month!",
    body = "Keep recharging on time to unlock more savings.",
    illustrationRes = R.drawable.ic_celebration_rounded,
    onDismiss = onClose,
    confirm = WiomCtaSpec("View savings", onViewSavings, type = WiomCtaType.Primary),
)
```

### Loading: processing (no button, not dismissable)

```kotlin
WiomLoadingDialog(
    title = "Processing payment",
    body = "Please wait while we confirm your transaction.",
)

// Always pair with an error / timeout fallback Alert dialog.
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Using Material `AlertDialog` directly — wrong width, radius, shadow, and dismiss behaviour.
- ❌ Painting a manual scrim `Box` over the screen — the platform `Dialog` provides it; configure dismiss via `DialogProperties` instead.
- ❌ Using `type.heading.lg` (24 / 32 LH) for the title — dialog uses `type.heading.md` (20 / 28).
- ❌ A Cancel button on a Loading dialog, or letting back-press / scrim tap dismiss it. Loading is never dismissable.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Dialog canvas: `3069:6004`. Component set: `2151:348`.
Re-pull `get_variable_defs` on `2151:348` if foundations token names ever change.

| Node | Variant |
|---|---|
| `2151:348` | Dialog component set — 5 ship variants |
| `2151:7` | Type=Alert · Actions=Single |
| `2151:44` | Type=Input · Actions=Single |
| `2151:113` | Type=Selection · Actions=Single |
| `2151:290` | Type=Illustration · Actions=Single |
| `2151:336` | Type=Loading · Actions=Single |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The **main → button-group gap is `space-48` for ALL 5 types** (Alert, Input, Selection, Illustration, Loading). Older skill drafts said "Alert is 48, all others are 24" — that is incorrect; the Figma ship sets `gap-48` on every type's outer column.
- **The dialog component ships only `Actions = Single` as a top-level variant**, but the embedded button group instance independently exposes its own `Layout = Stacked / Side by side` switch. Stacked is the default and the safer choice; Side-by-side is supported when both CTAs are short, parallel actions ("Save" / "Cancel", "Yes" / "No") so each ≈ 126 dp cell stays readable. In Compose, set `buttonsLayout = WiomDialogButtonsLayout.SideBySide`. When in doubt, leave it Stacked.
- **Loading has no button in code.** Figma ships `Show Link Button = true` by default; set it `false`. The library's Loading dialog has no Cancel and is never dismissable.
- **Dialog has no Disabled / Pressed / Focused state** at the surface level — it isn't interactive. The CTAs inside carry their own states.
- **Surface uses `shadow.xl`, no stroke.** The legacy stroke-and-shadow combination is banned; foundations rule is shadow XOR border, and dialog is shadow.
- **Selection list-item rows already include `space-16` all-sides padding and `min-height = 48 dp`** — that's owned by `wiom-list-item`. Don't re-pad inside the dialog body.

---

## See also

- `wiom-design-foundations` — token source. Especially `bg.default` / `radius.xlarge` / `shadow.xl` / `overlay.scrim` / `bg.brand.subtle` / `space.s48`.
- `wiom-cta` — the button group embedded at the bottom; stacked rules and the Primary + Destructive ban.
- `wiom-input-fields` — Input dialog's inline field (instance, don't re-implement).
- `wiom-list-item` + `wiom-selection-controls` — Selection dialog rows (`Type = Radio`).
- `wiom-loader` — Loading dialog spinner (Type=Spinner, size=sm, color=brand).
- `wiom-icon-badge` — Alert dialog header at `size = md`, status-coded tone.
- `wiom-bottomsheet` — the non-modal counterpart for picker lists, multi-field input, and transient confirmations.
- `wiom-compose-layout-rules` — Modifier discipline; `Icons.Rounded.*` enforcement; `Arrangement.spacedBy` for gaps; stacked layout.
