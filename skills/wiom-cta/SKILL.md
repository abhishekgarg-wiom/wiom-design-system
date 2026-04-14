---
name: wiom-cta
description: Wiom CTA/Button component system — types, states, pairing rules, layout logic, and token mappings. Use this skill EVERY TIME you create, edit, or review any screen, dialog, bottom sheet, or component that contains a button or CTA for the Wiom Partner or Customer app. Trigger when you see Button, CTA, TextButton, OutlinedButton, FilledButton, onClick, or any interactive action element.
---

# Wiom CTA System

The approved CTA/Button foundation for the Wiom app ecosystem (Android/Kotlin/Compose).
**5 types · 5 states · 12 universal rules**

---

## 1. Token Reference

### Typography (All CTA Types)

| Property | Value | Token |
|----------|-------|-------|
| Font | Noto Sans | — |
| Size | 16sp | `type.label.l` |
| Weight | SemiBold 600 | `type.label.l` |
| Line Height | 24sp | `type.label.l` |
| Kotlin | `WiomType.labelL` | — |

Exception: Acknowledge CTA body text uses 14sp Regular 400 (`type.body.m` / `WiomType.bodyM`).

### Spacing

| Property | Value | Token |
|----------|-------|-------|
| Height | 48px | `space/48` |
| Horizontal Padding | 16px | `space/16` |
| Icon-to-Text Gap | 8px | `space/8` |
| Acknowledge Checkbox-to-Text Gap | 4px | `space/4` |
| Side-by-Side CTA Gap | 12px | `space/12` |
| Stacked CTA Gap | 8px | `space/8` |
| Corner Radius | 16px | `radius/large` (foundation: buttons = radius-large) |
| Border Width (Secondary) | 2px | `stroke-medium` |
| Icon Size | 20px | — |
| Checkbox Frame | 24×24px | — |
| Touch Target | 48×48px min | `space/48` |

### Colors Per Type × State

**Primary**

| State | Fill | Text | Extra |
|-------|------|------|-------|
| Default | `color.brand.primary` #D9008D | `color.text.on-color` #FFF | — |
| Pressed | `color.brand.primary-pressed` #A30070 | `color.text.on-color` #FFF | — |
| Disabled | `color.surface.muted` #E8E4F0 | `color.text.disabled` #A7A1B2 | — |
| Focused | `color.brand.primary` #D9008D | `color.text.on-color` #FFF | Ring: `color.brand.primary-soft` #FFB2E4 |
| Loading | `color.brand.primary` #D9008D | hidden | Spinner: white |

**Secondary**

| State | Fill | Text | Border | Extra |
|-------|------|------|--------|-------|
| Default | transparent | `color.brand.primary` #D9008D | `color.brand.primary` #D9008D | — |
| Pressed | `color.brand.primary-subtle` #FFCCED | `color.brand.primary` #D9008D | `color.brand.primary` | — |
| Disabled | transparent | `color.text.disabled` #A7A1B2 | `color.border.default` #D7D3E0 | — |
| Focused | transparent | `color.brand.primary` #D9008D | `color.brand.primary` | Ring: `color.brand.primary-soft` |
| Loading | transparent | hidden | `color.brand.primary` | Spinner: brand.primary |

**Tertiary**

| State | Fill | Text | Extra |
|-------|------|------|-------|
| Default | transparent | `color.brand.primary` #D9008D | — |
| Pressed | `color.brand.primary-subtle` #FFCCED | `color.brand.primary` #D9008D | — |
| Disabled | transparent | `color.text.disabled` #A7A1B2 | — |
| Focused | `color.brand.primary-tint` #FFE5F6 | `color.brand.primary` #D9008D | Ring: `color.brand.primary-soft` |
| Loading | transparent | hidden | Spinner: brand.primary |

**Destructive**

| State | Fill | Text | Extra |
|-------|------|------|-------|
| Default | `color.negative.primary` #D92130 | `color.text.on-color` #FFF | — |
| Pressed | `color.negative.primary-pressed` #A31824 | `color.text.on-color` #FFF | — |
| Disabled | `color.surface.muted` #E8E4F0 | `color.text.disabled` #A7A1B2 | — |
| Focused | `color.negative.primary` #D92130 | `color.text.on-color` #FFF | Ring: `color.negative.soft` #FFB3B9 |
| Loading | `color.negative.primary` #D92130 | hidden | Spinner: white |

**Acknowledge**

| State | Checkbox | Text Color | Behavior |
|-------|----------|------------|----------|
| Unchecked | `color.border.strong` #665E75 border, no fill | `color.text.primary` #161021 | Primary CTA = disabled |
| Checked | `color.brand.primary` #D9008D fill, white checkmark | `color.text.primary` #161021 | Primary CTA = enabled |
| Disabled | 40% opacity | `color.text.disabled` #A7A1B2 | — |

---

## 2. The 5 CTA Types

| Type | Purpose | Example |
|------|---------|---------|
| **Primary** | Main action on screen | "भुगतान करें", "Recharge Now" |
| **Secondary** | Alternate action alongside Primary | "Cancel", "वापस जाएं" |
| **Tertiary** | Low-emphasis dismiss or soft action | "ठीक है", "Maybe Later" |
| **Destructive** | Irreversible/danger action | "Delete", "Remove Plan" |
| **Acknowledge** | Checkbox + 1st person confirmation before critical action | "मैंने सभी जानकारी सही दी है" |

### Icon Variants (Primary, Secondary, Tertiary, Destructive only)

| Variant | Description |
|---------|-------------|
| None | Text only (default) |
| Leading | 20px icon before text |
| Trailing | 20px icon after text |

Loading state = NO icon, only spinner.
Icon color follows text color for all states.
Icon selection rules in separate skill (Action Icons vs Non-Action Icons).

---

## 3. Decision Logic — Which CTA to Use

```
Is this the main action on the screen?
  → PRIMARY

Is this an alternate/cancel action next to a Primary?
  → SECONDARY

Is this a low-emphasis dismiss or soft action?
  → TERTIARY

Does this action delete, remove, or can't be undone?
  → DESTRUCTIVE

Does the user need to confirm they've verified something before proceeding?
  → ACKNOWLEDGE (checkbox) + PRIMARY (disabled until checked)
```

---

## 4. Layout Rules — Pairing 2 CTAs

### Bottom Sheet

| Scenario | Layout | CTAs |
|----------|--------|------|
| 1 action (confirm/acknowledge) | Full-width | Primary only |
| 2 actions, one dominant | Side by side | Secondary (left) + Primary (right) |
| 2 actions, one dominant, soft dismiss | Vertical stack | Primary (top) + Tertiary (bottom) |
| 2 actions, equal weight, short text | Side by side | Secondary + Secondary |
| 2 actions, equal weight, long text | Vertical stack | Secondary (top) + Secondary (bottom) |
| Destructive confirmation | Side by side | Secondary (left) + Destructive (right) |

### Dialog / Popup

| Scenario | Layout | CTAs |
|----------|--------|------|
| Simple acknowledgment ("ठीक है") | Single centered | Tertiary only |
| Confirmation | Side by side | Secondary (left) + Primary (right) |
| Destructive confirmation | Side by side | Secondary (left) + Destructive (right) |
| Celebration / success | Full-width | Primary only |

### Acknowledge CTA Scenarios

| Scenario | Example Copy (1st Person) | Behavior |
|----------|--------------------------|----------|
| Before submitting verified data | "मैंने सभी जानकारी सही दी है" | Unchecked → Primary disabled. Checked → Primary enabled. |
| Before irreversible action | "मैं इस रिचार्ज को confirm करता/करती हूँ" | Forces active acknowledgment before proceeding. |
| Accepting terms or policy | "मैंने Terms & Conditions पढ़ लिए हैं" | Blocks submission until acknowledged. |
| Confirming task completion | "मैंने यह काम पूरा कर लिया है" | Partner confirms work done before marking complete. |
| Handing over credentials | "मैंने कस्टमर को credentials दे दिए हैं" | Ensures accountability before closing ticket. |

---

## 5. The 12 Universal Rules

1. **Max 2 CTAs** — never 3.
2. **Primary appears only once** per screen / sheet / dialog.
3. **Right = preferred action** in side-by-side layout.
4. **Top = preferred action** in vertical stack layout.
5. **Tertiary only in vertical stacks** — never side by side with Primary.
6. **Destructive never alone** — always paired with a Secondary escape route.
7. **CTA text copy reflects the next screen's context** — not the current screen. "भुगतान करें" (Pay) on plan selection, not "आगे बढ़ें" (Continue).
8. **Acknowledge CTA uses 1st person** — "मैंने..." not "कस्टमर ने...". Used before irreversible actions.
9. **CTA container never has shadow** — always flat with `border-top: 1px solid color.border.default`. No box-shadow, no elevation, no exceptions.
10. **CTA text never wraps, truncates, or shrinks** — if text is too long, write shorter copy. Do not reduce font size. Do not add ellipsis. Do not allow multi-line button text. Rewrite the copy to be shorter while keeping the meaning.
11. **Layout is decided by delta (importance gap) between two CTAs:**
    - **Low/Zero delta** (both CTAs have equal weight) → **Horizontal** side by side. First try to shorten text to fit. Only go vertical if shortening loses critical context.
    - **High delta** (one CTA is clearly dominant, the other is a dismiss) → **Vertical** stack. Dominant on top, dismiss below.
12. **Equal-weight CTAs: always try horizontal first** — shorten text to fit side by side. Only switch to vertical stack (Secondary + Secondary) if Claude determines that shortening the text would lose too much context for the user to make an informed choice.

13. **CTA placement — sticky bottom vs inline:**
    - **Sticky bottom (fixed):** Screen-level primary actions that must always be visible while scrolling — recharge, submit, confirm, payment. The CTA bar sticks to the bottom of the screen above the navigation.
    - **Inline (not fixed):** CTAs inside cards, dialogs, or bottom sheets — they move with the content, not pinned to screen edge.
    - **Rule:** If the CTA is the final action of the screen (the reason the screen exists), it's sticky. If it's one of many actions within a container, it's inline.

---

## 5a. Delta-Based Layout Decision Tree

```
Two CTAs on screen. What's the importance gap (delta)?

STEP 1: Measure the delta
  - Is one action clearly "the thing to do" and the other just "dismiss/escape"?
    → HIGH DELTA
  - Are both actions legitimate choices the user needs to weigh?
    → LOW/ZERO DELTA

STEP 2: Pick layout based on delta

HIGH DELTA (dominant + dismiss):
  Is the dismiss a cancel/back with visual weight?
    → Horizontal: Secondary (left) + Primary (right)
  Is the dismiss truly throwaway ("later", "no thanks")?
    → Vertical: Primary (top) + Tertiary (bottom)
  Is the dominant action destructive?
    → Horizontal: Secondary (left) + Destructive (right)

LOW/ZERO DELTA (equal weight):
  Can both texts fit in ~14 characters each (Hindi) or ~18 chars (English)?
    → Horizontal: Secondary + Secondary (side by side)
  Is the text too long but can be shortened without losing meaning?
    → Shorten text → Horizontal
  Would shortening lose critical context for the user's decision?
    → Vertical: Secondary (top) + Secondary (bottom)
```

### Delta Validation Against All Rules

| Pairing | Delta | Layout | Rule Check |
|---------|-------|--------|------------|
| Secondary + Primary | Medium | Horizontal | Rule 3 ✓ |
| Primary + Tertiary | High | Vertical | Rule 4, 5 ✓ |
| Secondary + Secondary (short) | Zero | Horizontal | Rule 12 ✓ |
| Secondary + Secondary (long, can't shorten) | Zero | Vertical | Rule 11, 12 ✓ |
| Secondary + Destructive | Medium | Horizontal | Rule 6, 3 ✓ |
| Acknowledge + Primary | N/A (sequential, not parallel choice) | Vertical always | Rule 8 ✓ |

---

## 6. Never Do

| Anti-Pattern | Why |
|-------------|-----|
| Primary + Primary | No hierarchy — user doesn't know which to tap |
| Destructive + Primary | Two strong actions compete |
| Three buttons at same level | Too many choices — simplify the flow |
| Tertiary side by side with Primary | Borderless next to filled looks broken horizontally |
| Destructive alone | No escape route — always pair with Secondary |
| "आगे बढ़ें" / "Continue" / "Submit" | Says nothing about what's next — use contextual copy |
| "कस्टमर ने..." in Acknowledge | Must be 1st person — "मैंने..." |
| Raw hex colors in code | Use semantic tokens from color skill |
| Arbitrary spacing values | Use tokens from spacing skill |
| Shadow on CTA container | Never — always flat border-top, no box-shadow |
| Wrapping / truncating / shrinking CTA text | Never — rewrite shorter copy instead |
| Vertical layout without trying horizontal first | For equal-weight pairs, always try shortening text for horizontal first |

---

## 7. Compose Implementation

```kotlin
// Primary CTA
WiomButton(
    text = "भुगतान करें",
    type = WiomButtonType.Primary,
    state = WiomButtonState.Default,
    icon = null,
    onClick = { }
)

// Secondary CTA
WiomButton(
    text = "वापस जाएं",
    type = WiomButtonType.Secondary,
    state = WiomButtonState.Default,
    onClick = { }
)

// Destructive with icon
WiomButton(
    text = "Delete",
    type = WiomButtonType.Destructive,
    state = WiomButtonState.Default,
    icon = WiomButtonIcon.Leading(Icons.Delete),
    onClick = { }
)

// Acknowledge + Primary pair
WiomAcknowledgeRow(
    text = "मैंने सभी जानकारी सही दी है",
    checked = isAcknowledged,
    onCheckedChange = { isAcknowledged = it }
)
WiomButton(
    text = "Submit",
    type = WiomButtonType.Primary,
    state = if (isAcknowledged) WiomButtonState.Default 
            else WiomButtonState.Disabled,
    onClick = { }
)

// Side by side pair in bottom sheet
Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
    WiomButton(
        text = "Cancel",
        type = WiomButtonType.Secondary,
        modifier = Modifier.weight(1f),
        onClick = { }
    )
    WiomButton(
        text = "Confirm",
        type = WiomButtonType.Primary,
        modifier = Modifier.weight(1f),
        onClick = { }
    )
}

// Vertical stack pair
Column(verticalArrangement = Arrangement.spacedBy(8.dp)) {
    WiomButton(
        text = "भुगतान करें",
        type = WiomButtonType.Primary,
        modifier = Modifier.fillMaxWidth(),
        onClick = { }
    )
    WiomButton(
        text = "बाद में",
        type = WiomButtonType.Tertiary,
        modifier = Modifier.fillMaxWidth(),
        onClick = { }
    )
}
```

---

## Strict Rules

### DO
- Use ONLY token colors from the color skill
- Use ONLY spacing values from the spacing skill
- Use `type.label.l` (16sp SemiBold 600) for all CTA text
- Follow the decision tree to pick the right type
- Use contextual copy that tells user where they're going
- Use 1st person for Acknowledge CTAs
- Pair Destructive with Secondary always

### DO NOT
- Do NOT use more than 2 CTAs per screen/sheet/dialog
- Do NOT use Primary more than once per screen
- Do NOT invent new button styles or colors
- Do NOT use generic text ("Continue", "Submit", "Next")
- Do NOT put Tertiary side by side with Primary
- Do NOT leave Destructive without an escape route
- Do NOT hardcode colors or spacing — use tokens

### WHEN IN DOUBT
Ask: "What does the user need to understand before they tap?" — the answer is your CTA text and type.
