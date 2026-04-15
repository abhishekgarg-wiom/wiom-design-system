# Wiom Design System — Switch Component Skill

You are generating a design or UI that includes a switch/toggle for the Wiom product ecosystem. You MUST use ONLY the approved Wiom switch specification below. Do NOT deviate from the defined anatomy, token assignments, variant rules, or sizing. Every switch in your output must conform exactly to this system.

## Context

Wiom serves lower- and middle-income Indian households. Switch decisions must account for:
- **Mobile-first** — budget Android devices, outdoor use, small screens
- **Trust-heavy** — privacy toggles, data sharing consent, auto-pay settings are critical path
- **Bilingual** — Hindi and English labels, Devanagari characters are taller/wider at same size
- **Low cognitive load** — clear on/off state, instant visual feedback, no ambiguity
- **Instant-apply** — switches take effect immediately, no submit button needed

---

## Component Overview

```
PURPOSE:  Binary on/off toggle for instant-apply settings and preferences.
SIZE:     Medium only (V1). Track 52×32px, Thumb 24×24px.
STATES:   2 selection × 2 interaction = 4 variants.
SHADOW:   shadow.none — switch is a flat inline form control.
NOTE:     Hover, Focused, and Error states are excluded from V1 — mobile-first product.
```

---

## Anatomy

```
┌─ Switch ────────────────────────────────────────────┐
│                                                       │
│  ┌──────────────────┐                                 │
│  │  Track            │   Label text                   │
│  │  ┌──────┐        │   Helper text (optional)        │
│  │  │Thumb │        │                                 │
│  │  └──────┘        │                                 │
│  └──────────────────┘                                 │
│                                                       │
└───────────────────────────────────────────────────────┘
```

| Element | Required | Spec |
|---------|----------|------|
| **Track** | Yes | 52×32px pill container, `radius-full` (9999px). Background changes per selection state. |
| **Thumb** | Yes | 24×24px circle (ellipse), white (#FFFFFF), absolute positioned 4px inset from track edge. |
| **Label** | Yes | Always required. Describes what the toggle controls. |
| **Helper text** | Optional | Below label, toggled via boolean property |

**Critical rule:** The entire row (track + label) is the click/tap target. A switch without a visible label is never allowed. Thumb position (left=Off, right=On) is the primary selection affordance.

---

## Selection States

| State | Track | Thumb position |
|-------|-------|----------------|
| **Off** | `color.surface.muted` fill + `color.border.strong` stroke (2px) | 4px from left edge |
| **On** | `color.brand.primary` fill, no stroke | 4px from right edge |

**Filled state rule:** When the switch is On, the stroke is removed. Fill IS the boundary. This follows the cross-foundation "filled = no border" rule shared with checkbox and button.

---

## Interaction States

| State | Description | Visual change |
|-------|-------------|---------------|
| **Default** | Resting state | Base appearance per selection state |
| **Disabled** | Not interactive | Reduced opacity, `text.disabled` labels, no pointer events |

**Excluded from V1:** Hover, Focused, and Error states are not built. Wiom is a mobile-first product — touch interactions don't produce hover, keyboard focus states are deferred to accessibility phase, and switches are instant-apply toggles with no form validation.

---

## Complete Token Reference

### Track tokens

| Property | Token | Value |
|----------|-------|-------|
| Width | — | 52px |
| Height | — | 32px |
| Radius | `radius-full` | 9999px |
| Stroke width (Off) | `stroke-medium` | 2px, inside |
| Stroke position | — | Always INSIDE |
| Shadow | `shadow.none` | none |

### Thumb tokens

| Property | Token | Value |
|----------|-------|-------|
| Size | — | 24×24px |
| Shape | — | Ellipse (inherently circular) |
| Fill | — | White #FFFFFF (all states) |
| Inset from track edge | — | 4px (all sides) |

### Color tokens — Off track

| Element | Default | Disabled |
|---------|---------|----------|
| Fill | `color.surface.muted` (#E8E4F0) | `color.surface.subtle` (#F1EDF7) |
| Stroke | `color.border.strong` (#665E75) | `color.border.strong` @ 40% opacity |

### Color tokens — On track

| Element | Default | Disabled |
|---------|---------|----------|
| Fill | `color.brand.primary` (#D9008D) | `color.brand.primary` (0.4 frame opacity) |
| Stroke | None | None |

### Text color tokens

| Element | Default | Disabled |
|---------|---------|----------|
| Label | `color.text.primary` (#161021) | `color.text.disabled` (#A7A1B2) |
| Helper text | `color.text.secondary` (#352D42) | `color.text.disabled` (#A7A1B2) |

### Typography tokens

| Element | Token | Font | Size | Line height | Weight |
|---------|-------|------|------|-------------|--------|
| Label | `type.body.md` | Noto Sans | 14sp | 20sp | Regular 400 |
| Helper text | `type.body.sm` | Noto Sans | 12sp | 16sp | Regular 400 |

**Why `body.md`, not `label.md`:** Switch labels describe what the toggle controls ("Enable notifications", "Auto-pay"). This is reading text, not compact UI control text. Consistent with Checkbox and Radio.

### Spacing tokens

| Property | Token | Value |
|----------|-------|-------|
| Track to content gap | `space/8` | 8px |
| Label to helper text gap | `space/4` | 4px |
| Thumb inset (all sides) | — | 4px (absolute position) |
| Between stacked switches (external) | `space/12` | 12px |
| Section gap (external) | `space/24` | 24px |
| Touch target minimum | — | 48×48px (parent handles) |

---

## Decision Trees

### When to use Switch

```
User needs to turn a setting on or off with immediate effect?
  → Switch

User controls a binary preference (notifications, dark mode, auto-pay)?
  → Switch

User toggles a feature flag or permission?
  → Switch

Setting applies instantly without needing a save/submit button?
  → Switch
```

### When NOT to use Switch

```
User needs to select from a form and submit?
  → Checkbox (not Switch)

User picks one option from a mutually exclusive set?
  → Radio (not Switch)

User selects multiple items from a list?
  → Checkbox (not Switch)

User picks from a dropdown of options?
  → Select (not Switch)

Action requires confirmation before applying?
  → Checkbox with submit (not Switch)
```

### Which track fill?

```
Off state?
  → color.surface.muted (#E8E4F0) — neutral, visible on base surface

On state?
  → color.brand.primary (#D9008D) — branded, no stroke

Disabled Off?
  → color.surface.subtle (#F1EDF7) — lighter neutral

Disabled On?
  → color.brand.primary at 0.4 frame opacity — faded brand
```

---

## Wiom Use Cases

### Notification toggles

```
Screen: Settings, Notification preferences
Label:  "Push notifications"
        "SMS alerts"
        "Email updates"
State:  Each independently toggleable.
Helper: "Receive alerts for payments and plan changes"
Layout: Stack with space/12 gap between items.
```

### Privacy settings

```
Screen: Settings, Privacy
Label:  "Share usage data"
        "Allow location access"
        "Marketing communications"
State:  Off by default for privacy-sensitive settings.
Helper: "Help us improve your experience"
```

### Auto-pay and billing

```
Screen: Plan management, Billing settings
Label:  "Auto-renew plan"
Helper: "Your plan will renew on the 15th of each month"
State:  On by default if user opted in during onboarding.
Type:   Critical — affects billing. Consider confirmation dialog before enabling.
```

### Partner dashboard features

```
Screen: Partner settings, Feature management
Label:  "Enable customer self-service"
        "Show plan comparison"
        "Allow online payments"
State:  Configurable per partner.
```

### Quick settings

```
Screen: Home screen, Quick settings panel
Label:  "Dark mode"
        "Hindi language"
        "Offline mode"
State:  Instant-apply, no navigation needed.
```

---

## Figma Component Structure

```
Switch                          ← root, auto-layout horizontal, gap: space/8, CENTER align
├── Track                       ← 52×32px fixed, radius-full, NO auto-layout
│   └── Thumb                   ← 24×24px ellipse, absolute position, white fill
└── Content                     ← auto-layout vertical, gap: space/4
    ├── Label                   ← type/body/md, color/text/primary
    └── HelperText              ← boolean toggle, type/body/sm
```

**Cross-axis alignment:** CENTER — the 32px track is taller than the 20px text line height, so vertical centering looks natural. This differs from Checkbox/Radio which use MIN (top) because their 20px indicators match the text height.

### Variant properties (Figma)

| Property | Values | Type | Default |
|----------|--------|------|---------|
| Selection | Off, On | Variant | Off |
| State | Default, Disabled | Variant | Default |
| Show Helper Text | true, false | Boolean | false |
| Label | (editable) | Text | "Label" |
| Helper Text | (editable) | Text | "Helper text" |

**Total variants:** 2 selection × 2 interaction = 4

---

## Disabled State Pattern

| Element | Treatment |
|---------|-----------|
| Label text | `color.text.disabled` (#A7A1B2) |
| Helper text | `color.text.disabled` (#A7A1B2) |
| Off track fill | `color.surface.subtle` (#F1EDF7) |
| Off track stroke | `color.border.strong` at 40% paint opacity |
| On track fill | `color.brand.primary` at 0.4 frame opacity |
| On track stroke | None |
| Thumb fill | White #FFFFFF (unchanged) |
| Pointer events | None — not interactive |

---

## Switch vs Checkbox — Key Differences

| Property | Switch | Checkbox |
|----------|--------|----------|
| Apply behavior | Instant — no submit needed | Form field — requires submit |
| Selection model | Binary on/off | Binary check/uncheck (or multi-select) |
| Track shape | 52×32px pill (`radius-full`) | 20×20px square (`radius-tiny` 4px) |
| Moving element | 24×24px thumb slides left/right | Checkmark icon appears/disappears |
| Cross-axis alignment | CENTER (32px track > 20px text) | MIN (20px indicator = 20px text) |
| Error state | None — instant apply, no validation | Supported — form validation |
| Common Wiom use | Settings, preferences, toggles | Consent, multi-select, forms |

---

## Strict Rules

### DO
- Use ONLY the tokens listed in this skill — no hardcoded values
- Always include a visible label — no switch without text
- Use `type.body.md` for labels — not `type.label.md`
- Use `color.surface.muted` for Off track fill — not `surface.subtle` or transparent
- Use `color.border.strong` for Off track stroke — not `border.default`
- Use `stroke-medium` (2px) for Off track border — not `stroke-small`
- Use `radius-full` (9999px) for track and thumb
- Remove stroke when On — filled state = no border
- Use white (#FFFFFF) for the thumb in all states
- Use `shadow.none` — switch is a flat form control
- Use `space/8` gap between track and content
- Use `space/12` gap between stacked switches
- Use CENTER cross-axis alignment (not MIN/top)
- Keep labels sentence case
- Test with Hindi labels — Devanagari wraps differently

### DO NOT
- Do NOT use switch for form fields requiring submit — use Checkbox
- Do NOT use switch for mutually exclusive choice — use Radio
- Do NOT add error states — switches don't validate
- Do NOT add hover or focus states — V1 is mobile-first, these are deferred
- Do NOT use `color.border.default` for the Off track — too faint
- Do NOT use `stroke-small` (1px) for the track border — too thin
- Do NOT add shadow to switch — it's a flat inline control
- Do NOT add icons inside the thumb (check, X) — deferred
- Do NOT add ON/OFF text inside the track — thumb position is sufficient
- Do NOT use switch without a visible label (accessibility violation)
- Do NOT use MIN (top) alignment — use CENTER (track is taller than text)
- Do NOT change thumb color — always white (#FFFFFF)

### WHEN IN DOUBT
- Label should be in `type.body.md` (Regular) — switch labels are content, not controls
- Off track fill is `color.surface.muted` (#E8E4F0) — visible on base surface
- On track fill is `color.brand.primary` (#D9008D) — branded, no stroke
- Track border is `color.border.strong` — only in Off state, 2px inside
- Gap between stacked switches is `space/12` — dense but not cramped
- No hover/focus/error states in V1 — mobile-first product
- Thumb is always white — in all states, all selections

---

## Anti-Pattern Checklist

Before submitting any design or code with switches, verify NONE of these are present:

- [ ] Hardcoded hex values instead of semantic tokens
- [ ] Missing visible label text
- [ ] Border present on On track (filled = no border)
- [ ] `border.default` on Off track (should be `border.strong`)
- [ ] `stroke-small` (1px) on track (should be `stroke-medium` 2px)
- [ ] Shadow on switch (should be `shadow.none`)
- [ ] Hover, Focused, or Error state added (not in V1)
- [ ] `type.label.md` for switch label (should be `type.body.md`)
- [ ] Switch used as form checkbox (use Checkbox for submit forms)
- [ ] MIN (top) alignment instead of CENTER
- [ ] Thumb color other than white (#FFFFFF)
- [ ] ON/OFF text inside the track
- [ ] Icon inside the thumb
- [ ] Track dimensions other than 52×32px
- [ ] Thumb dimensions other than 24×24px
- [ ] Switch used alone without context (should describe what it controls)
- [ ] Spacing not on the 4px grid

---

## Quick Token Lookup

```
TRACK:       52×32px, radius-full (9999px), stroke-medium (2px, Off only)
THUMB:       24×24px ellipse, white (#FFFFFF), 4px inset
OFF FILL:    surface.muted (#E8E4F0) → disabled: surface.subtle (#F1EDF7)
ON FILL:     brand.primary (#D9008D) → disabled: brand.primary @ 0.4 frame opacity
STROKE:      border.strong (#665E75) → disabled: border.strong @ 40% → on: none
DISABLED:    surface.subtle fill + border.strong @40% (off) / brand.primary @0.4 opacity (on)
FOCUS:       Not in V1 (mobile-first, deferred to accessibility phase)
LABEL:       type.body.md (14sp/20sp/Regular), text.primary (#161021)
HELPER:      type.body.sm (12sp/16sp/Regular), text.secondary (#352D42)
GAP:         space/8 (track→content), space/4 (label→helper)
STACK:       space/12 between switches, space/24 section gap
ALIGN:       CENTER cross-axis (not MIN — 32px track > 20px text)
SHADOW:      shadow.none — always
```

---

Apply this switch specification to the user's design request. Use token names in your output (not just values) so the design is traceable back to the Wiom Design System. Reference variant properties (Selection, State, Show Helper Text) when describing switch instances.
