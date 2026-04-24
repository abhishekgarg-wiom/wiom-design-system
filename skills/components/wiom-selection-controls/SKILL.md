---
name: wiom-selection-controls
description: >
  Wiom Checkbox, Radio, and Switch — the three small (≤20dp indicator) selection
  controls, unified because they share sizing, token patterns, and the rule that
  label + helper text live on the parent row (wiom-list-item), never inside the
  control itself. Use this skill EVERY TIME you create, edit, or review any
  Checkbox, RadioButton, Radio, or Switch for the Wiom Partner or Customer app.
  Trigger on Checkbox, RadioButton, Radio, Switch, Toggle, multi-select, consent,
  T&C, opt-in, opt-out, single-select, plan selection, payment method, preference,
  notification toggle, privacy toggle, auto-pay, or any binary / exclusive
  selection pattern. Pairs with wiom-design-foundations (tokens), wiom-list-item
  (row anatomy + label), and wiom-compose-layout-rules (scaffolding).
---

# Wiom Selection Controls — Checkbox · Radio · Switch

One skill for the three small selection controls. They share more than they
differ: same 20dp indicator grid (switch is a track-and-thumb variant), same
small-control stroke rules, same "filled = no border" transition, same deferred
ownership of labels.

**Platform:** Android (Jetpack Compose). Mobile-first.
**Context:** Low- and middle-income Indian households. Budget Android devices,
outdoor use, Hindi + English content. Selection controls are critical-path for
consent (T&C, auto-debit), plan selection, and instant-apply settings.

---

## The shared rule — no label, no helper, no row chrome

None of these three controls ships with a built-in label, helper text, description
slot, or row background. The control is the **indicator only** — the
square-with-check, the circle-with-dot, the track-with-thumb. Label + helper +
disclosure + row padding all live on `wiom-list-item`.

```
❌ WRONG                                ✅ RIGHT
WiomCheckbox(                           WiomListItem(
    label = "I agree to terms",            type = ListItemType.Checkbox,
    helper = "You can review…",            headline = "I agree to terms",
    checked = true,                        supportingText = "You can review…",
)                                          leadingContent = {
                                               WiomCheckbox(
                                                   state = CheckboxState.Selected,
                                                   ...
                                               )
                                           },
                                       )
```

Consequence: every spec below stops at the indicator. The indicator hugs its
bounding box. Touch target, row padding, and tap ripple belong to the list-item
or the parent composable.

**Exception — inline form usage.** When a single checkbox sits inline (e.g., the
Acknowledge CTA's compound checkbox), the parent composable owns the label; the
selection-control still renders just the indicator.

---

## Shared small-control rules

These apply to all three controls — they descend from `wiom-design-foundations`
and exist here for one-stop reference.

| Rule | Value |
|---|---|
| Checkbox / Radio — Figma component container | 24 × 24 dp |
| Checkbox / Radio — visible indicator glyph | 20 × 20 dp (centered in the 24 dp cell) |
| Switch track | 52 × 32 dp · thumb 24 × 24 dp · 4 dp inset all sides |
| Unselected stroke width | `stroke-medium` (2 dp, inside) |
| Unselected stroke color | `stroke.strong` (#473F55 Neutral_800) — small controls need the heavier weight |
| Filled state | No stroke (filled = no border; foundations cross-rule #2) |
| Radius | Checkbox `radius-tiny` (4 dp) · Radio `radius-full` · Switch track `radius-full` |
| Shadow | `shadow.none` always — all three are flat inline controls |
| Focus ring (a11y only) | `stroke.brand.focus` (#D9008D), `stroke-medium` (2 dp), 2 dp outside, radius = indicator radius + 2 dp |
| Keyboard focus | Visual only via focus ring — indicator fill/stroke do not change |

Composer shouldn't set a height or width on the outer control — the indicator
dimensions above ARE the control size.

---

## Checkbox

**Purpose.** Binary or tri-state selection — consent, multi-select, preference
toggles that require a submit, opt-in / opt-out.

### Anatomy

24 × 24 dp bounding container (Figma component size); visible indicator is
20 × 20 dp rounded-square, centered, `radius-tiny` (4 dp). Content-state glyph
(check or dash) comes from `Icons.Rounded.*` and fills ~65% of the indicator.

### Variants — 3 selection × 3 state = 9

| State ↓ \ Selection → | No (unchecked) | Indeterminate | Selected |
|---|---|---|---|
| **Default** | `stroke-medium` `stroke.strong` · transparent fill | `bg.brand` fill · `Icons.Rounded.Remove` glyph `icon.inverse` | `bg.brand` fill · `Icons.Rounded.Check` glyph `icon.inverse` |
| **Disabled** | `stroke-medium` `stroke.subtle` · transparent fill | `bg.disabled` fill · `icon.disabled` glyph | `bg.disabled` fill · `icon.disabled` glyph |
| **Error** | `stroke-medium` `stroke.critical.focus` · transparent fill | `bg.critical` fill · `Icons.Rounded.Remove` glyph `icon.inverse` | `bg.critical` fill · `Icons.Rounded.Check` glyph `icon.inverse` |

**The checked-error rule.** When a checkbox is Selected AND the form is in error,
the fill switches from `bg.brand` to `bg.critical` (foundations §Form control
states). The glyph stays `icon.inverse`.

### Indeterminate — new in V2

A third selection state for "some children selected" in a parent checkbox
(hierarchical multi-select). Uses `Icons.Rounded.Remove` (horizontal dash) as the
glyph. Render the same fill + tint rules as Selected.

### States deliberately NOT shipped

Hover and Focused (as distinct visual variants) are not Compose states for Wiom
selection controls. Keyboard focus shows the `stroke.brand.focus` ring only; no
fill or stroke change on the indicator itself. Hover doesn't exist on
touch-first Android. Don't invent them.

---

## Radio

**Purpose.** Exactly-one-from-a-set. Always used in a group of 2+ — a standalone
radio is not a valid pattern.

### Anatomy

24 × 24 dp bounding container (Figma component size); visible indicator is
20 × 20 dp circular (`radius-full`), centered. Inner 10 × 10 dp dot visible only
when Selected.

### Variants — 2 selection × 3 state = 6

| State ↓ \ Selection → | No (unselected) | Yes (selected) |
|---|---|---|
| **Default** | `stroke-medium` `stroke.strong` · transparent fill | `bg.brand` fill · no stroke · 10×10 dot `icon.inverse` |
| **Disabled** | `stroke-medium` `stroke.subtle` · transparent fill | `bg.disabled` fill · 10×10 dot `icon.disabled` |
| **Error** | `stroke-medium` `stroke.critical.focus` · transparent fill | `bg.critical` fill · no stroke · 10×10 dot `icon.inverse` |

Same selected-error fill swap as checkbox.

### States deliberately NOT shipped

Hover / Focused — same reasoning as checkbox; keyboard focus uses
`stroke.brand.focus` ring only.

---

## Switch

**Purpose.** Instant-apply binary setting — notifications, auto-pay, privacy
toggles, dark mode. Never for form fields that require a submit (use Checkbox).

### Anatomy

52 × 32 dp pill track (`radius-full`), 24 × 24 dp circular thumb, 4 dp inset from
track edge on all sides. Thumb fill is always `#FFFFFF` (visually white on every
track tone — the thumb is a physical affordance, not a tokened surface).

### Variants — 2 selection × 3 state = 6

| State ↓ \ Selection → | Off | On |
|---|---|---|
| **Default** | `bg.muted` track + `stroke-medium` `stroke.strong` · thumb left | `bg.brand` track · no stroke · thumb right |
| **Pressed** | `bg.subtle` track + `stroke-medium` `stroke.strong` · thumb left | `bg.brand.pressed` track · no stroke · thumb right |
| **Disabled** | `bg.disabled` track + `stroke-medium` `stroke.subtle` · thumb left | `bg.disabled` track · no stroke · thumb right |

### Pressed — new in V2

Compose `InteractionSource` pressed state. Track fill steps one level (off: muted
→ subtle; on: brand → brand.pressed). Thumb position does NOT change on press;
thumb only moves on state transition (selection change).

### States deliberately NOT shipped

Error (switches are instant-apply; no form validation), Hover, Focused. Keyboard
focus ring: `stroke.brand.focus` around the track, offset 2 dp, radius = track
radius + 2 dp.

---

## Decision trees

**Which control?**

```
Exactly one from a visible set of 2–5 options        → Radio
One or more from a set, requires submit              → Checkbox
Binary on/off with instant effect                    → Switch
Consent / T&C / opt-in                               → Checkbox
Binary preference (notifications, auto-pay, dark)    → Switch
Tri-state parent of a multi-select tree              → Checkbox (Indeterminate)
Filter chip                                          → Chip (wiom-tabs-filters), not these
Dropdown picker with long list                       → wiom-input-fields + wiom-bottomsheet
```

**Stroke color when unselected:**

```
Default                          → stroke.strong (#473F55)
Disabled                         → stroke.subtle (#D7D3E0)
Error (checkbox + radio only)    → stroke.critical.focus (#D92130)
```

**Fill color when selected:**

```
Default                          → bg.brand (#D9008D)
Disabled                         → bg.disabled (#D7D3E0)
Error + Selected (cb + radio)    → bg.critical (#D92130)
Switch On · Pressed              → bg.brand.pressed
```

---

## Wiom use cases

Preserved from the pre-merge skills. All labels live on the parent `wiom-list-item`
(or the inline form parent) — this list names the flows, not the control-level
copy.

**Checkbox**
- **Consent (critical path).** T&C, Privacy Policy, auto-debit authorization on
  recharge, plan activation, account setup. Never pre-checked — user must
  actively opt in. Error message below the row: "You must agree to continue."
- **Multi-select lists.** Plan comparison multi-pick, feature selection, category
  filters on browse screens.
- **Preference settings.** Notification opt-ins, marketing consent, auto-renew.
- **Partner onboarding checklists.** ID proof uploaded, address verified,
  training module completed — some items disabled until prerequisites pass.
- **Tri-state parent (Indeterminate).** A section-level checkbox that reflects
  mixed child state in a bulk-selection list.

**Radio**
- **Plan selection (critical path).** Monthly vs quarterly vs annual; data-bundle
  tiers; OTT inclusion. One option pre-selected (usually recommended or cheapest).
- **Payment method selection.** UPI, Debit/Credit Card, Cash-on-delivery.
  Pre-select the most-used method.
- **Language preference.** English / हिन्दी. Must render at the same size for both
  scripts.
- **Notification frequency.** Immediately / Daily / Weekly / Never.
- **Filter / sort options.** Price low-to-high, most popular, newest-first.

**Switch**
- **Notification toggles.** Push / SMS / Email independently.
- **Privacy settings.** Share usage data, location access, marketing comms.
  Off by default for privacy-sensitive items.
- **Auto-pay / billing.** Auto-renew plan. Consider a confirmation dialog before
  enabling because it affects billing.
- **Partner dashboard features.** Enable customer self-service, show plan
  comparison, allow online payments.
- **Quick settings.** Dark mode, Hindi language, offline mode.

---

## Figma → Compose mapping

All three live in the Figma V2 Selection Controls page (`node-id=3069-5843`).

| Figma property | Compose parameter | Values |
|---|---|---|
| Checkbox `State` | `state` | Default · Disabled · Error |
| Checkbox `Selection` | `selection` | No · Indeterminate · Selected |
| Radio `State` | `state` | Default · Disabled · Error |
| Radio `Selected` | `selected` | No · Yes |
| Switch `State` | `state` | Default · Pressed · Disabled |
| Switch `Selection` | `checked` | Off · On |

No `Show Helper Text`, no `Label`, no `Show Supporting Text` — Figma V2 ships the
indicator only.

---

## Minimal Compose shape

```kotlin
// Checkbox — exposed states only. No label slot.
@Composable
fun WiomCheckbox(
    selection: CheckboxSelection,        // No, Indeterminate, Selected
    state: CheckboxState,                // Default, Disabled, Error
    onToggle: (() -> Unit)?,             // null when state == Disabled
    modifier: Modifier = Modifier,       // size hints NOT exposed — 20x20 is the control
)

// Radio — no `state.Error` is valid only when the group is in error; flag at group level.
@Composable
fun WiomRadio(
    selected: Boolean,
    state: RadioState,                   // Default, Disabled, Error
    onSelect: (() -> Unit)?,
    modifier: Modifier = Modifier,
)

// Switch — `state.Pressed` is typically driven by InteractionSource, not user code.
@Composable
fun WiomSwitch(
    checked: Boolean,
    state: SwitchState = SwitchState.Default,   // Default, Disabled (Pressed auto)
    onCheckedChange: ((Boolean) -> Unit)?,
    modifier: Modifier = Modifier,
)
```

**Size parameters are NOT exposed.** There is one size of each control.
**`Modifier` accepts alignment / padding from the parent only — no dimensional
overrides.**

---

## Token quick-lookup

```
SHARED
Indicator stroke (unselected)  stroke-medium (2dp) + stroke.strong (#473F55)
Indicator stroke (disabled)    stroke-medium + stroke.subtle (#D7D3E0)
Indicator stroke (error)       stroke-medium + stroke.critical.focus (#D92130)
Shadow                         shadow.none (all controls, all states)
Focus ring                     stroke-medium + stroke.brand.focus outside +2dp

CHECKBOX
Container (Figma)              24×24dp
Visible indicator              20×20dp centered · radius-tiny (4dp)
Selected fill                  bg.brand (#D9008D) · no stroke
Selected glyph                 Icons.Rounded.Check · icon.inverse
Indeterminate glyph            Icons.Rounded.Remove · icon.inverse
Error + Selected fill          bg.critical (#D92130) · glyph icon.inverse
Disabled Selected fill         bg.disabled · glyph icon.disabled

RADIO
Container (Figma)              24×24dp
Visible indicator              20×20dp centered · radius-full
Inner dot                      10×10dp · radius-full · icon.inverse
Selected fill                  bg.brand (#D9008D) · no stroke
Error + Selected fill          bg.critical (#D92130)
Disabled Selected fill         bg.disabled · dot icon.disabled

SWITCH
Track                          52×32dp · radius-full
Thumb                          24×24dp · #FFFFFF · 4dp inset
Off track fill (default)       bg.muted (#E8E4F0) + stroke.strong
Off track fill (pressed)       bg.subtle (#F1EDF7) + stroke.strong
Off track fill (disabled)      bg.disabled + stroke.subtle
On track fill (default)        bg.brand (#D9008D) · no stroke
On track fill (pressed)        bg.brand.pressed · no stroke
On track fill (disabled)       bg.disabled · no stroke
```

---

## Anti-pattern checklist

- [ ] Label or helper text baked into the control (delegate to `wiom-list-item`)
- [ ] Exposing a `size` parameter (there is one size per control)
- [ ] `stroke.default` or `stroke.subtle` on an unselected default indicator
      (should be `stroke.strong` — small controls need the heavy weight)
- [ ] `stroke-small` (1 dp) on the indicator (should be `stroke-medium` 2 dp)
- [ ] Shadow on any of the three controls (always `shadow.none`)
- [ ] Hover or Focused as a *visual* variant (keyboard focus is ring-only)
- [ ] Checkbox with `radius-full` (that's Radio) or radio with `radius-tiny`
      (that's Checkbox)
- [ ] Switch without Pressed state (ship it; Figma V2 has it)
- [ ] Switch with an Error state (switches are instant-apply; no validation)
- [ ] Error + Selected using `bg.brand` instead of `bg.critical`
- [ ] Raw `#FFFFFF` anywhere other than the switch thumb (for neutral-on-dark
      use `icon.inverse` or `text.inverse`)
- [ ] Standalone radio without a group of 2+
- [ ] Pre-checked consent checkbox
- [ ] Focus ring with blur > 0 (must be 0; hard edge)

---

## See also

- `wiom-design-foundations` — token values, color tables, `stroke.brand.focus`,
  Form Control States (full state matrix)
- `wiom-list-item` — label, helper text, supporting text, row padding, tap ripple
- `wiom-compose-layout-rules` — Modifier discipline, no explicit size on content
  frames, Icons.Rounded.* enforcement
