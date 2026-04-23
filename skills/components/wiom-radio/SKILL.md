---
name: wiom-radio
description: Wiom Radio component spec — anatomy, state variants, token assignments, and usage rules. Use this skill EVERY TIME you create, edit, or review any screen, form, dialog, or list that contains a radio button for the Wiom Partner or Customer app. Trigger when you see Radio, RadioButton, single-select, plan selection, payment method, exclusive choice, or any single-choice selection pattern. Pairs with wiom-design-foundations for token values.
---

# Wiom Design System — Radio Component Skill

You are generating a design or UI that includes a radio button for the Wiom product ecosystem. You MUST use ONLY the approved Wiom radio specification below. Do NOT deviate from the defined anatomy, token assignments, state rules, or sizing. Every radio in your output must conform exactly to this system.

## Context

Wiom serves lower- and middle-income Indian households. Radio decisions must account for:
- **Mobile-first** — budget Android devices, outdoor use, small screens
- **Trust-heavy** — plan selection, payment method choice are critical path for recharges and payments
- **Bilingual** — Hindi and English labels, Devanagari characters are taller/wider at same size
- **Low cognitive load** — clear selection state, scannable, no ambiguity about which option is chosen
- **Form-heavy** — plan selection, payment methods, onboarding choices, language preference

---

## Component Overview

```
PURPOSE:  Single-choice selection control — choose exactly one option from a mutually exclusive set.
SIZE:     Medium only (V1). No small/large variants.
STATES:   2 selection × 3 interaction = 6 variants.
SHADOW:   shadow.none — radio is a flat inline form control.
NOTE:     Hover and Focused states are excluded from V1 — mobile-first product.
```

---

## Anatomy

```
┌─ Radio ─────────────────────────────────────────┐
│                                                   │
│  ┌────────────┐                                   │
│  │  Indicator  │   Label text                     │
│  │   ( ● )    │   Helper text (optional)          │
│  └────────────┘                                   │
│                                                   │
└───────────────────────────────────────────────────┘
```

| Element | Required | Spec |
|---------|----------|------|
| **Indicator** | Yes | 20×20px fixed frame, `radius-full` (9999px), `stroke-medium` (2px) inside |
| **Inner dot** | Conditional | 10×10px circle centered in indicator, `color.text.inverse` (#FAF9FC), visible only when Selected |
| **Label** | Yes | Always required. No radio without visible label text. |
| **Helper text** | Optional | Below label, toggled via boolean property |

**Critical rule:** The entire row (indicator + label) is the click/tap target. A radio without a visible label is never allowed. Radios must always appear in a group of 2 or more — a single radio is not a valid pattern.

---

## Selection States

| State | Indicator | Inner element |
|-------|-----------|---------------|
| **Unselected** | Empty container with `stroke-medium` border | None |
| **Selected** | Filled with `color.brand.primary`, no border | White dot (`color.text.inverse`) centered |

**Filled state rule:** When the radio becomes selected, the stroke is removed. Fill IS the boundary. This follows cross-foundation rule #2.

**Key difference from Checkbox:** Radio uses `radius-full` (9999px, circular) instead of `radius-tiny` (4px, rounded square). This is the primary visual distinction. The inner element is a dot, not a checkmark.

---

## Interaction States

| State | Description | Visual change |
|-------|-------------|---------------|
| **Default** | Resting state | Base appearance per selection state |
| **Disabled** | Not interactive | Reduced opacity, `text.disabled` labels, no pointer events |
| **Error** | Validation failed | Red border/fill, error text replaces helper text |

**Excluded from V1:** Hover and Focused states are not built. Wiom is a mobile-first product — touch interactions don't produce hover, and keyboard focus states are deferred to a later accessibility phase.

---

## Complete Token Reference

### Indicator tokens

| Property | Token | Value |
|----------|-------|-------|
| Size | — | 20×20px |
| Radius | `radius-full` | 9999px |
| Inner dot size | — | 10×10px |
| Inner dot radius | `radius-full` | 9999px |
| Stroke width (unselected) | `stroke-medium` | 2px, inside |
| Stroke position | — | Always INSIDE |
| Shadow | `shadow.none` | none |

### Color tokens — Unselected indicator

| Element | Default | Disabled | Error |
|---------|---------|----------|-------|
| Fill | Transparent | `color.surface.subtle` | Transparent |
| Stroke | `color.border.strong` | `color.border.strong` @ 40% paint opacity | `color.negative.primary` |

### Color tokens — Selected indicator

| Element | Default | Disabled | Error |
|---------|---------|----------|-------|
| Fill | `color.brand.primary` | `color.brand.primary` (0.4 node opacity) | `color.negative.primary` |
| Stroke | None | None | None |
| Inner dot | `color.text.inverse` | `color.text.inverse` (inherits 0.4 node opacity) | `color.text.inverse` |

### Text color tokens

| Element | Default | Disabled | Error |
|---------|---------|----------|-------|
| Label | `color.text.primary` | `color.text.disabled` | `color.text.primary` |
| Helper text | `color.text.secondary` | `color.text.disabled` | — |
| Error text | — | — | `color.negative.primary` |

### Typography tokens

| Element | Token | Font | Size | Line height | Weight |
|---------|-------|------|------|-------------|--------|
| Label | `type.body.md` | Noto Sans | 14sp | 20sp | Regular 400 |
| Helper text | `type.body.sm` | Noto Sans | 12sp | 16sp | Regular 400 |
| Error text | `type.body.sm` | Noto Sans | 12sp | 16sp | Regular 400 |

**Why `body.md`, not `label.md`:** Radio labels describe choices and content ("Monthly — Rs 299/month", "Pay via UPI", "English"). This is reading text, not compact UI control text. Regular weight is more readable for potentially longer content. Consistent with Checkbox.

### Spacing tokens

| Property | Token | Value |
|----------|-------|-------|
| Indicator to label gap | `space/8` | 8px |
| Label to helper text gap | `space/4` | 4px |
| Between stacked radios (external) | `space/12` | 12px |
| Group to surrounding content (external) | `space/16` | 16px |
| Group label to first radio (external) | `space/8` | 8px |

---

## Decision Trees

### When to use Radio

```
User must pick EXACTLY ONE from a visible set of 2–5 options?
  → Radio

User selects a plan, payment method, or preference from a short list?
  → Radio

User answers a single-choice question in onboarding/forms?
  → Radio
```

### When NOT to use Radio

```
User can select MULTIPLE options?
  → Checkbox (not Radio)

User needs a binary on/off toggle?
  → Toggle / Switch (not Radio)

User picks from a long list (6+ items)?
  → Select / Dropdown (not Radio)

User selects tags or filter chips?
  → Chip (not Radio)

User triggers an action or navigates?
  → Button (not Radio)

User needs to confirm or agree to a statement?
  → Checkbox (not Radio)
```

### Which label typography?

```
Radio label describing a choice or option?
  → type.body.md (Regular 400) — always

Radio label is a compact control header?
  → Still type.body.md — radio labels are always body text
```

### Which border color for unselected indicator?

```
Unselected, default state?
  → color.border.strong (#665E75) — small 20px control needs strong contrast

Unselected, disabled?
  → color.border.strong at 40% paint opacity — base color faded per foundation rule #10

Unselected, error?
  → color.negative.primary (#D92130) — error state overrides border color
```

### Which fill for selected indicator?

```
Selected, default?
  → color.brand.primary (#D9008D) — solid fill, no stroke

Selected, disabled?
  → color.brand.primary with 0.4 node opacity — entire indicator faded

Selected, error?
  → color.negative.primary (#D92130) — error replaces brand fill
```

---

## Wiom Use Cases

### Plan selection (critical path)

```
Screen: Plan comparison, Recharge screen, Plan upgrade
Label:  "Plan A: Rs 299/month — 1.5GB/day"
        "Plan B: Rs 449/month — 2GB/day + Hotstar"
        "Plan C: Rs 699/month — 3GB/day + Disney+"
State:  One pre-selected by default (cheapest or recommended).
Helper: "Billed monthly. Cancel anytime."
Layout: Stack with space/12 gap between items.
Rule:   Exactly one option must be selected at all times.
```

### Payment method selection

```
Screen: Recharge checkout, Payment screen
Label:  "Pay via UPI"
        "Debit / Credit Card"
        "Cash on delivery"
Helper: "Instant transfer from your bank"
State:  One pre-selected (most popular method).
```

### Language preference

```
Screen: Onboarding, Settings
Label:  "English"
        "हिन्दी"
State:  Current language pre-selected.
Rule:   Must work at the same component size for both scripts.
```

### Notification frequency

```
Screen: Settings, Notification preferences
Label:  "Immediately"
        "Daily digest"
        "Weekly summary"
        "Never"
State:  Current setting pre-selected.
```

### Filter / sort options

```
Screen: Browse plans, Search results
Label:  "Price: Low to High"
        "Price: High to Low"
        "Most Popular"
        "Newest First"
Layout: Stack within a filter panel.
Rule:   Exactly one sort option active at a time.
```

---

## Figma Component Structure

```
Radio                           ← root, auto-layout horizontal, gap: space/8
├── Indicator                   ← 20×20 fixed, radius-full (9999px)
│   └── Dot                     ← 10×10 ellipse, centered, hidden when Unselected
└── Content                     ← auto-layout vertical, gap: space/4
    ├── Label                   ← type/body/md, color/text/primary
    └── HelperText              ← boolean toggle, type/body/sm
```

**Dot rule:** The inner dot is a simple ellipse, not a component instance. Fill color is `color.text.inverse` (#FAF9FC). Visible only in Selected states.

### Variant properties (Figma)

| Property | Values | Type | Default |
|----------|--------|------|---------|
| Selection | Unselected, Selected | Variant | Unselected |
| State | Default, Disabled, Error | Variant | Default |
| Show Helper Text | true, false | Boolean | false |
| Label | (editable) | Text | "Label" |
| Helper Text | (editable) | Text | "Helper text" |

**Total variants:** 2 selection × 3 interaction = 6

---

## Disabled State Pattern

Follows cross-foundation rule #10:

| Element | Treatment |
|---------|-----------|
| Label text | `color.text.disabled` (#A7A1B2) |
| Helper text | `color.text.disabled` (#A7A1B2) |
| Inner dot | Inherits 0.4 node opacity from parent indicator |
| Unselected indicator fill | `color.surface.subtle` (#F1EDF7) |
| Unselected indicator stroke | `color.border.strong` at 40% paint opacity |
| Selected indicator fill | `color.brand.primary` at 0.4 node opacity |
| Pointer events | None — not interactive |

---

## Error State Pattern

| Element | Treatment |
|---------|-----------|
| Unselected indicator stroke | `color.negative.primary` (#D92130), `stroke-medium` (2px) |
| Selected indicator fill | `color.negative.primary` (#D92130) — replaces brand |
| Dot on selected error | `color.text.inverse` (#FAF9FC) — unchanged |
| Label text | `color.text.primary` (#161021) — NO change |
| Error text | `color.negative.primary` (#D92130), `type.body.sm` |
| Error text position | Replaces helper text, same position below label |

**Wiom-specific:** Error on radio group almost always means "required group with no selection." The most common error message is "Please select an option to continue." Error typically appears below the entire group, not below individual items.

---

## Touch Target Guidance

The radio component hugs content (20px row height). For mobile touch compliance:

| Context | Recommended approach |
|---------|---------------------|
| Standalone radio group | Parent adds `space/12` vertical padding → 44px total (12 + 20 + 12) |
| Stacked radios | `space/12` gap between items → 32px per row (still below 44px) |
| Critical forms (payment) | Add explicit padding at layout level for 48px touch target |

**Rule:** Touch target sizing is a layout concern, not a component concern. The component hugs content for flexibility.

---

## Layout Patterns

### Stacked radio group (most common)

```
Group Label                         ← type.label.md, color.text.primary
  ↕ space/8
(●) Option A                        ← Radio instance, Selected
  ↕ space/12
( ) Option B                        ← Radio instance, Unselected
  ↕ space/12
( ) Option C                        ← Radio instance, Unselected
  ↕ space/16
[Next button or surrounding content]
```

### With helper text

```
Group Label                         ← type.label.md, color.text.primary
  ↕ space/8
(●) Pay via UPI                     ← Radio, Selected, Show Helper Text=true
    Instant transfer from your bank ← Helper text
  ↕ space/12
( ) Debit / Credit Card             ← Radio, Unselected, Show Helper Text=true
    Visa, Mastercard, RuPay accepted
  ↕ space/16
[Pay button]
```

### With error state

```
Group Label                         ← type.label.md, color.text.primary
  ↕ space/8
( ) Option A                        ← Radio, State=Error
  ↕ space/12
( ) Option B                        ← Radio, State=Error
  ↕ space/8
Please select an option to continue ← Error text below group, negative/primary
  ↕ space/16
[Submit button]
```

---

## Radio vs Checkbox — Key Differences

| Property | Radio | Checkbox |
|----------|-------|----------|
| Selection model | Exactly one from a set | Zero, one, or many |
| Indicator shape | Circle (`radius-full` 9999px) | Rounded square (`radius-tiny` 4px) |
| Inner element | 10×10px dot | Checkmark icon instance |
| Group behavior | One selected at all times | Independent toggles |
| Indeterminate state | None | Supported (dash icon) |
| Pre-selection | Typically one pre-selected | Typically all unchecked |
| Common Wiom use | Plan selection, payment method | Consent, multi-select, filters |

---

## Strict Rules

### DO
- Use ONLY the tokens listed in this skill — no hardcoded values
- Always include a visible label — no radio without text
- Always use radios in groups of 2 or more — a single radio is invalid
- Use `type.body.md` for labels — not `type.label.md`
- Use `color.border.strong` for unselected indicator — not `border.default`
- Use `stroke-medium` (2px) for indicator border — not `stroke-small`
- Use `radius-full` (9999px) for indicator — circular, not rounded square
- Remove stroke when selected — filled state = no border (cross-rule #2)
- Use `color.text.inverse` for the inner dot on selected states
- Use `shadow.none` — radio is a flat form control
- Use `space/12` gap between stacked radios
- Use `space/8` gap from indicator to label
- Keep labels sentence case ("Monthly billing", not "MONTHLY BILLING")
- Test with Hindi labels — Devanagari wraps differently
- Pre-select one option by default in most radio groups
- Show group-level error when no option is selected in a required group

### DO NOT
- Do NOT use radio for multiple selections — use Checkbox
- Do NOT use radio for binary on/off toggles — use Toggle/Switch
- Do NOT use a single radio alone — always 2+ in a group
- Do NOT use `color.border.default` for the unselected indicator — too faint at 20×20px
- Do NOT use `stroke-small` (1px) for the indicator border — too thin at 20×20px
- Do NOT use `radius-tiny` (4px) for indicator — that's Checkbox, not Radio
- Do NOT add shadow to radio — it's a flat inline control
- Do NOT add hover or focus states — V1 is mobile-first, these are deferred
- Do NOT size the indicator differently — always 20×20px
- Do NOT use radio without a visible label (accessibility violation)
- Do NOT use radio cards or rich-content variants — deferred to future

### WHEN IN DOUBT
- Label should be in `type.body.md` (Regular) — radio labels are content, not controls
- Border should be `color.border.strong` — small elements need strong contrast
- Stroke should be `stroke-medium` (2px) — small elements need visible weight
- Gap between stacked radios is `space/12` — dense but not cramped
- One option should be pre-selected unless there's a product reason not to
- No hover/focus states in V1 — mobile-first, defer to future accessibility phase

---

## Anti-Pattern Checklist

Before submitting any design or code with radio buttons, verify NONE of these are present:

- [ ] Hardcoded hex values instead of semantic tokens
- [ ] Missing visible label text
- [ ] Single radio used alone (not in a group of 2+)
- [ ] Border still present on selected indicator (filled = no border)
- [ ] `border.default` on unselected indicator (should be `border.strong`)
- [ ] `stroke-small` (1px) on indicator (should be `stroke-medium` 2px)
- [ ] `radius-tiny` (4px) on indicator (should be `radius-full` 9999px — that's Checkbox)
- [ ] Shadow on radio (should be `shadow.none`)
- [ ] Hover or Focused state added (not in V1 — mobile-first)
- [ ] `type.label.md` for radio label (should be `type.body.md`)
- [ ] Radio used for multiple-selection (use Checkbox)
- [ ] Radio used as instant toggle (use Toggle/Switch)
- [ ] Dot using `brand.primary` fill instead of `text.inverse` on selected state
- [ ] Missing error state on required radio groups
- [ ] Indicator size other than 20×20px
- [ ] Inner dot size other than 10×10px
- [ ] Spacing not on the 4px grid
- [ ] Label in ALL CAPS (use sentence case)
- [ ] No pre-selected default in a radio group (usually one should be selected)

---

## Quick Token Lookup

```
INDICATOR:   20×20px, radius-full (9999px), stroke-medium (2px)
DOT:         10×10px, radius-full (9999px), text.inverse (#FAF9FC)
FILL:        brand.primary (#D9008D) → error: negative.primary (#D92130)
STROKE:      border.strong (#665E75) → disabled: border.strong @ 40% → error: negative.primary (#D92130)
DISABLED:    surface.subtle fill, border.strong @40% (unselected) / brand.primary @0.4 node opacity (selected)
FOCUS:       Not in V1 (mobile-first, deferred to accessibility phase)
LABEL:       type.body.md (14sp/20sp/Regular), text.primary (#161021)
HELPER:      type.body.sm (12sp/16sp/Regular), text.secondary (#5C5570)
ERROR:       type.body.sm, negative.primary (#D92130)
GAP:         space/8 (indicator→label), space/4 (label→helper)
STACK:       space/12 between radios, space/16 group→content
SHADOW:      shadow.none — always
```

---

Apply this radio specification to the user's design request. Use token names in your output (not just values) so the design is traceable back to the Wiom Design System. Reference variant properties (Selection, State, Show Helper Text) when describing radio instances.
