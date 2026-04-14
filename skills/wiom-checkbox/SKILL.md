---
name: wiom-checkbox
description: Wiom Checkbox component spec — anatomy, 10 state variants (2 selection × 5 interaction), token assignments, and usage rules. Use this skill EVERY TIME you create, edit, or review any screen, form, dialog, or list that contains a checkbox for the Wiom Partner or Customer app. Trigger when you see Checkbox, consent, T&C, multi-select, opt-in, opt-out, preference toggle, or any binary selection pattern. Pairs with wiom-design-foundations for token values.
---

# Wiom Design System — Checkbox Component Skill

You are generating a design or UI that includes a checkbox for the Wiom product ecosystem. You MUST use ONLY the approved Wiom checkbox specification below. Do NOT deviate from the defined anatomy, token assignments, state rules, or sizing. Every checkbox in your output must conform exactly to this system.

## Context

Wiom serves lower- and middle-income Indian households. Checkbox decisions must account for:
- **Mobile-first** — budget Android devices, outdoor use, small screens
- **Trust-heavy** — consent checkboxes (T&C, privacy) are critical path for recharges and payments
- **Bilingual** — Hindi and English labels, Devanagari characters are taller/wider at same size
- **Low cognitive load** — clear selection state, scannable, no ambiguity about what's checked
- **Form-heavy** — plan selection, onboarding checklists, preference settings, filter screens

---

## Component Overview

```
PURPOSE:  Binary selection control — select one or more options, or confirm a statement.
SIZE:     Medium only (V1). No small/large variants.
STATES:   2 selection × 5 interaction = 10 variants.
SHADOW:   shadow.none — checkbox is a flat inline form control.
```

---

## Anatomy

```
┌─ Checkbox ───────────────────────────────────────┐
│                                                    │
│  ┌────────────┐                                    │
│  │  Indicator  │   Label text                      │
│  │    [ ✓ ]    │   Helper text (optional)          │
│  └────────────┘                                    │
│                                                    │
└────────────────────────────────────────────────────┘
```

| Element | Required | Spec |
|---------|----------|------|
| **Indicator** | Yes | 20×20px fixed frame, `radius-tiny` (4px), `stroke-medium` (2px) inside |
| **Check icon** | Conditional | `check` from icon library (Material Symbols Rounded, filled), 20×20 instance matching indicator, glyph ~13×9px (~65% fill ratio), visible only when Checked |
| **Label** | Yes | Always required. No checkbox without visible label text. |
| **Helper text** | Optional | Below label, toggled via boolean property |

**Critical rule:** The entire row (indicator + label) is the click/tap target. A checkbox without a visible label is never allowed.

---

## Selection States

| State | Indicator | Icon |
|-------|-----------|------|
| **Unchecked** | Empty container with `stroke-medium` border | None |
| **Checked** | Filled with `color.brand.primary`, no border | Checkmark in `color.text.inverse` |

**Filled state rule:** When the checkbox becomes checked, the stroke is removed. Fill IS the boundary. This follows cross-foundation rule #2.

---

## Interaction States

| State | Description | Visual change |
|-------|-------------|---------------|
| **Default** | Resting state | Base appearance per selection state |
| **Hover** | Cursor over checkbox (web) | Subtle background fill on unchecked; `brand.primary-hover` on checked |
| **Focused** | Keyboard focus | Focus ring appears around indicator (2px, 0 blur) |
| **Disabled** | Not interactive | Reduced opacity, `text.disabled` labels, no pointer events |
| **Error** | Validation failed | Red border/fill, error text replaces helper text |

---

## Complete Token Reference

### Indicator tokens

| Property | Token | Value |
|----------|-------|-------|
| Size | — | 20×20px |
| Radius | `radius-tiny` | 4px |
| Stroke width (unchecked) | `stroke-medium` | 2px, inside |
| Stroke position | — | Always INSIDE |
| Shadow | `shadow.none` | none |

### Color tokens — Unchecked indicator

| Element | Default | Hover | Focused | Disabled | Error |
|---------|---------|-------|---------|----------|-------|
| Fill | Transparent | `color.surface.subtle` | Transparent | `color.surface.subtle` | Transparent |
| Stroke | `color.border.strong` | `color.border.strong` | `color.border.strong` | `color.border.strong` @ 40% | `color.negative.primary` |
| Focus ring | — | — | `color.brand.primary-soft` | — | — |

### Color tokens — Checked indicator

| Element | Default | Hover | Focused | Disabled | Error |
|---------|---------|-------|---------|----------|-------|
| Fill | `color.brand.primary` | `color.brand.primary-hover` | `color.brand.primary` | `color.brand.primary` @ 40% | `color.negative.primary` |
| Stroke | None | None | None | None | None |
| Icon | `color.text.inverse` | `color.text.inverse` | `color.text.inverse` | `color.text.inverse` @ 40% | `color.text.inverse` |
| Focus ring | — | — | `color.brand.primary-soft` | — | — |

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

**Why `body.md`, not `label.md`:** Checkbox labels describe choices and content ("I agree to terms", "Enable notifications", "Plan A: Rs 299/month"). This is reading text, not compact UI control text. Regular weight is more readable for potentially longer content.

### Spacing tokens

| Property | Token | Value |
|----------|-------|-------|
| Indicator to label gap | `space/8` | 8px |
| Label to helper text gap | `space/4` | 4px |
| Between stacked checkboxes (external) | `space/12` | 12px |
| Group to surrounding content (external) | `space/16` | 16px |
| Group label to first checkbox (external) | `space/8` | 8px |

### Focus ring spec

| Property | Value |
|----------|-------|
| Technique | Drop shadow, 0 blur (not a stroke, not a glow) |
| Applied to | Indicator only (not full row) |
| X/Y offset | 0 / 0 |
| Blur | 0 — must be 0, no glow |
| Spread | 2px |
| Color | `color.brand.primary-soft` (#FFB2E4) |

---

## Decision Trees

### When to use Checkbox

```
User needs to select ONE or MORE options from a set?
  → Checkbox

User needs to confirm or agree to a statement?
  → Checkbox

User needs to enable/disable a preference or setting?
  → Checkbox (or Toggle — use Toggle for instant-apply settings, Checkbox for form-submit settings)
```

### When NOT to use Checkbox

```
User must pick EXACTLY ONE from a set?
  → Radio (not Checkbox)

User needs a binary on/off with instant effect?
  → Toggle / Switch (not Checkbox)

User picks from a dropdown list?
  → Select (not Checkbox)

User selects tags or filter chips?
  → Chip (not Checkbox)

User triggers an action or navigates?
  → Button (not Checkbox)
```

### Which label typography?

```
Checkbox label describing a choice, option, or agreement?
  → type.body.md (Regular 400) — always

Checkbox label is a compact control header?
  → Still type.body.md — checkbox labels are always body text
```

### Which border color for unchecked indicator?

```
Unchecked, any interaction state except disabled/error?
  → color.border.strong (#665E75) — small 20px control needs strong contrast

Unchecked, disabled?
  → color.border.strong at 40% opacity — base color faded per foundation rule #10

Unchecked, error?
  → color.negative.primary (#D92130) — error state overrides border color
```

### Which fill for checked indicator?

```
Checked, default/focused?
  → color.brand.primary (#D9008D)

Checked, hover?
  → color.brand.primary-hover (#C00080)

Checked, disabled?
  → color.brand.primary at 40% opacity

Checked, error?
  → color.negative.primary (#D92130) — error replaces brand fill
```

---

## Wiom Use Cases

### Agreement & consent (critical path)

```
Screen: Recharge confirmation, Plan activation, Account setup
Label:  "I agree to the Terms & Conditions"
        "I agree to the Privacy Policy"
        "I authorize auto-debit of Rs ₹XXX/month"
State:  Unchecked by default. Error state shown if user tries to proceed without checking.
Helper: "You can review the full terms before agreeing"
Error:  "You must agree to continue"
Rule:   NEVER pre-check consent checkboxes. User must actively opt in.
```

### Multi-select lists

```
Screen: Plan comparison, Feature selection, Category filter
Label:  "Plan A: Rs 299/month — 1.5GB/day"
        "Plan B: Rs 449/month — 2GB/day + Hotstar"
State:  Can select multiple. Checked state shows brand.primary fill.
Layout: Stack with space/12 gap between items.
```

### Preference settings

```
Screen: Notification settings, Account preferences
Label:  "Enable notifications"
        "Send me offers and updates via SMS"
        "Auto-renew my plan"
Helper: "We'll send updates about your policy"
State:  Reflects current setting. Can be checked or unchecked.
```

### Partner onboarding checklists

```
Screen: Partner verification, Document upload, Training completion
Label:  "ID proof uploaded"
        "Address verified"
        "Training module completed"
State:  Programmatically checked as steps complete. Some may be disabled (not yet available).
```

### Filter selections

```
Screen: Browse plans, Search results, Category pages
Label:  "4G only"
        "Under Rs 200"
        "OTT included"
Layout: Stack or group with clear group label above.
```

---

## Figma Component Structure

```
Checkbox                        ← root, auto-layout horizontal, gap: space/8
├── Indicator                   ← 20×20 fixed, radius-tiny (4px)
│   └── CheckIcon               ← instance of "check" from icon library, 20×20, absolute positioned, conditional (Checked only)
└── Content                     ← auto-layout vertical, gap: space/4
    ├── Label                   ← type/body/md, color/text/primary
    └── HelperText              ← boolean toggle, type/body/sm
```

**Icon rule:** The check icon MUST be an instance of the `check` component from the icon library (Material Symbols Rounded, filled). Never use a raw vector. Instance is resized to 20×20 to match the indicator, giving a ~65% glyph fill ratio. Fill color is overridden to `color.text.inverse` via variable binding.

### Variant properties (Figma)

| Property | Values | Type | Default |
|----------|--------|------|---------|
| Selection | Unchecked, Checked | Variant | Unchecked |
| State | Default, Hover, Focused, Disabled, Error | Variant | Default |
| Show Helper Text | true, false | Boolean | false |
| Label | (editable) | Text | "Label" |
| Helper Text | (editable) | Text | "Helper text" |

**Total variants:** 2 selection × 5 interaction = 10

---

## Disabled State Pattern

Follows cross-foundation rule #10:

| Element | Treatment |
|---------|-----------|
| Label text | `color.text.disabled` (#A7A1B2) |
| Helper text | `color.text.disabled` (#A7A1B2) |
| Icons | Base color at 40% opacity |
| Unchecked indicator fill | `color.surface.subtle` (#F1EDF7) |
| Unchecked indicator stroke | `color.border.strong` at 40% opacity |
| Checked indicator fill | `color.brand.primary` at 40% opacity |
| Pointer events | None — not interactive |

---

## Error State Pattern

| Element | Treatment |
|---------|-----------|
| Unchecked indicator stroke | `color.negative.primary` (#D92130), `stroke-medium` (2px) |
| Checked indicator fill | `color.negative.primary` (#D92130) — replaces brand |
| Icon on checked error | `color.text.inverse` (#FAF9FC) — unchanged |
| Label text | `color.text.primary` (#161021) — NO change |
| Error text | `color.negative.primary` (#D92130), `type.body.sm` |
| Error text position | Replaces helper text, same position below label |

**Wiom-specific:** Error on checkbox almost always means "required checkbox not checked" in consent flows. The most common error message is "You must agree to continue."

---

## Touch Target Guidance

The checkbox component hugs content (20px row height). For mobile touch compliance:

| Context | Recommended approach |
|---------|---------------------|
| Standalone checkbox | Parent adds `space/12` vertical padding → 44px total (12 + 20 + 12) |
| Stacked checkboxes | `space/12` gap between items → 32px per row (still below 44px) |
| Critical forms (consent) | Add explicit padding at layout level for 48px touch target |

**Rule:** Touch target sizing is a layout concern, not a component concern. The component hugs content for flexibility.

---

## Layout Patterns

### Stacked checkbox group (most common)

```
Group Label                         ← type.label.md, color.text.primary
  ↕ space/8
[✓] Option A                        ← Checkbox instance
  ↕ space/12
[ ] Option B                        ← Checkbox instance
  ↕ space/12
[ ] Option C                        ← Checkbox instance
  ↕ space/16
[Next button or surrounding content]
```

### Inline consent (single checkbox)

```
Form content above
  ↕ space/16
[ ] I agree to the Terms & Conditions    ← Checkbox with helper text
    You can review the full terms before agreeing
  ↕ space/16
[Submit button]
```

### With error state

```
Form content above
  ↕ space/16
[ ] I agree to the Terms & Conditions    ← Checkbox, State=Error
    You must agree to continue            ← Error text in negative/primary
  ↕ space/16
[Submit button]
```

---

## Strict Rules

### DO
- Use ONLY the tokens listed in this skill — no hardcoded values
- Always include a visible label — no checkbox without text
- Use `type.body.md` for labels — not `type.label.md`
- Use `color.border.strong` for unchecked indicator — not `border.default`
- Use `stroke-medium` (2px) for indicator border — not `stroke-small`
- Remove stroke when checked — filled state = no border (cross-rule #2)
- Apply focus ring to indicator only — not the full row
- Use `shadow.none` — checkbox is a flat form control
- Use `space/12` gap between stacked checkboxes
- Use `space/8` gap from indicator to label
- Keep labels sentence case ("Enable notifications", not "ENABLE NOTIFICATIONS")
- Test with Hindi labels — Devanagari wraps differently
- Show error text when validation fails on required checkboxes

### DO NOT
- Do NOT pre-check consent checkboxes — user must actively opt in
- Do NOT use checkbox for mutually exclusive choices — use Radio
- Do NOT use checkbox for instant on/off toggles — use Toggle/Switch
- Do NOT use `color.border.default` for the unchecked indicator — too faint at 20×20px
- Do NOT use `stroke-small` (1px) for the indicator border — too thin at 20×20px
- Do NOT add shadow to checkbox — it's a flat inline control
- Do NOT put the focus ring on the full row — indicator only
- Do NOT use blur on the focus ring — must be 0
- Do NOT change indicator fill/stroke on focus — ring is the sole indicator
- Do NOT size the indicator differently — always 20×20px
- Do NOT use checkbox without a visible label (accessibility violation)
- Do NOT mix selection states within a single checkbox — each checkbox is binary

### WHEN IN DOUBT
- Label should be in `type.body.md` (Regular) — checkbox labels are content, not controls
- Border should be `color.border.strong` — small elements need strong contrast
- Stroke should be `stroke-medium` (2px) — small elements need visible weight
- Focus ring goes on indicator only — follows Button and Input Field pattern
- Gap between stacked checkboxes is `space/12` — dense but not cramped

---

## Anti-Pattern Checklist

Before submitting any design or code with checkboxes, verify NONE of these are present:

- [ ] Hardcoded hex values instead of semantic tokens
- [ ] Missing visible label text
- [ ] Pre-checked consent checkbox
- [ ] Border still present on checked indicator (filled = no border)
- [ ] `border.default` on unchecked indicator (should be `border.strong`)
- [ ] `stroke-small` (1px) on indicator (should be `stroke-medium` 2px)
- [ ] Shadow on checkbox (should be `shadow.none`)
- [ ] Focus ring with blur > 0 (must be 0)
- [ ] Focus ring on full row (should be indicator only)
- [ ] `type.label.md` for checkbox label (should be `type.body.md`)
- [ ] Checkbox used for mutually exclusive choice (use Radio)
- [ ] Checkbox used as instant toggle (use Toggle/Switch)
- [ ] Raw vector used for checkmark instead of icon library instance
- [ ] Missing error state on required checkboxes
- [ ] Indicator size other than 20×20px
- [ ] Spacing not on the 4px grid
- [ ] Label in ALL CAPS (use sentence case)

---

## Quick Token Lookup

```
INDICATOR:   20×20px, radius-tiny (4px), stroke-medium (2px)
FILL:        brand.primary (#D9008D) → hover: primary-hover (#C00080) → error: negative.primary (#D92130)
STROKE:      border.strong (#665E75) → disabled: border.strong @ 40% → error: negative.primary (#D92130)
ICON:        "check" from icon library, 20×20 instance, text.inverse (#FAF9FC) fill override
FOCUS:       brand.primary-soft (#FFB2E4), 2px spread, 0 blur, indicator only
LABEL:       type.body.md (14sp/20sp/Regular), text.primary (#161021)
HELPER:      type.body.sm (12sp/16sp/Regular), text.secondary (#5C5570)
ERROR:       type.body.sm, negative.primary (#D92130)
GAP:         space/8 (indicator→label), space/4 (label→helper)
STACK:       space/12 between checkboxes, space/16 group→content
SHADOW:      shadow.none — always
```

---

Apply this checkbox specification to the user's design request. Use token names in your output (not just values) so the design is traceable back to the Wiom Design System. Reference variant properties (Selection, State, Show Helper Text) when describing checkbox instances.
