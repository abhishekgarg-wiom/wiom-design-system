---
name: wiom-input-fields
description: Input, Textarea, and OTP for Wiom DS. Use when writing forms, entry flows, OTP verification, search UIs, or any field that captures user text. Tells you which component to pick, which State variant + boolean/text/swap properties to set, and which Wiom tokens back each part. Pairs with the wiom-design-foundations skill for token values.
---

# Wiom Input Fields

Three components cover every form entry in Wiom: **Input** (single-line), **Textarea** (multi-line), and **OTP** (boxed code entry). **Search bar** is a configuration of Input, not a separate component.

Always use these components for text entry. Do not draw custom fields — they will not be token-bound and will drift from the system.

---

## Pick the right component

| Need | Use | Key signal |
|---|---|---|
| Single-line text, number, email, password, phone, amount, code | **Input** | One row of text |
| Address, comment, description, reason, feedback | **Textarea** | Multiple lines, grows with content |
| OTP / verification code entry (4 or 6 digits) | **OTP** | Boxed digits with auto-advance focus and timer |
| Search across a list or screen | **Input** with leading `search` icon, trailing `cancel` | Filters/queries something |

---

## State — pick one per context

Every instance carries a `State` variant. Pick based on the field's current UX moment, not visual preference.

| State | Use when |
|---|---|
| `Default` | Empty, unfocused. **Label is highlighted** (text/primary) to pull attention. |
| `Focused` | User has tapped in. Border upgrades to 2px brand/primary. Label softens to text/secondary. |
| `Filled` | Content present, not focused. Label stays soft — the value speaks for itself. |
| `Error` | Validation failed. Red border + error icon + red helper. Helper text **must** explain what's wrong. |
| `Success` | Validated OK (e.g., OTP matched, mobile confirmed). Green border + check icon. Use sparingly. |
| `Warning` | Value is valid but risky or unusual. Orange border + warning icon. Don't use for errors. |
| `Disabled` | Field is truly inactive — user cannot edit AND cannot copy. Everything dims. |
| `Read-only` | Display value user can read/copy but not edit (Customer ID, policy number, account number). Text stays text/primary. **Not the same as Disabled.** |

State coverage per component:
- **Input** — all 8 states.
- **Textarea** — 6 states. No `Warning`, no `Read-only`.
- **OTP** — 6 states. No `Warning`, no `Read-only`.

---

## Input — properties (instance overrides)

16 props total. Booleans toggle slot visibility; text props set content; swap props pick an icon.

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Title` | TEXT | "Label" | Field label above the input |
| `Show Title` | BOOL | true | Hide only when externally labelled by a parent section |
| `Body` | TEXT | "Enter your value" | Value or placeholder text inside the field |
| `Show Leading Icon` | BOOL | false | Reveal the leading icon slot |
| `Leading Icon` | INSTANCE_SWAP | search | Swap to any Material Symbols Rounded icon component |
| `Show Prefix` | BOOL | false | Reveal prefix text (e.g., `+91`, `₹`) |
| `Prefix` | TEXT | "+91" | Affix before the value, text/secondary |
| `Show Suffix` | BOOL | false | Reveal suffix text (e.g., `.00`, `kg`) |
| `Suffix` | TEXT | ".00" | Affix after the value, text/secondary |
| `Show Trailing Icon` | BOOL | false | Reveal trailing icon slot |
| `Trailing Icon` | INSTANCE_SWAP | cancel | Action/indicator icon |
| `Show Helper` | BOOL | true | Reveal the helper row below the field |
| `Helper` | TEXT | "Helper text" | Supporting description / validation message |
| `Show Counter` | BOOL | false | Reveal character counter at the right of helper row |
| `Counter` | TEXT | "0 / 100" | Character count text |
| `State` | VARIANT | Default | 8 values listed above |

Status icons (Error/Success/Warning) are **hardcoded per variant** — you do not toggle them. They always appear when State matches and they win over any user-set trailing icon.

## Textarea — properties

8 props. Same behavior as Input's shared props.

| Property | Type | Default |
|---|---|---|
| `Title`, `Show Title` | TEXT, BOOL | "Label", true |
| `Body` | TEXT | "Placeholder text..." |
| `Show Helper`, `Helper` | BOOL, TEXT | true, "Helper text" |
| `Show Counter`, `Counter` | BOOL, TEXT | false, "0 / 200" |
| `State` | VARIANT | Default (6 values) |

No icon slots. No prefix/suffix.

## OTP — properties

Boxed code entry with auto-advance focus, hardcoded timer, and a digit-count toggle. Use for verification codes — don't fake it with a single Input.

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Title` | TEXT | "Enter OTP" | Label above the boxes |
| `Show Title` | BOOL | true | Same rule as Input |
| `6-digit` | BOOL | true | `true` = 6 boxes (48dp each). `false` = 4 boxes (76dp each). Container stays 328dp wide. |
| `Show Helper` | BOOL | true | Reveal helper row |
| `Helper` | TEXT | "Didn't get the code? Resend in 00:24" | Helper / resend prompt |
| `State` | VARIANT | Default (6 values) | Default, Focused, Filled, Error, Success, Disabled |

Hardcoded per-state:
- **Focused** variant ships with a visible **timer `00:24`** inside the field — you don't toggle or theme it. The timer lives inside the component; treat it as part of the Focused appearance.
- **Error / Success** status icons are hardcoded, same rule as Input.

No prefix/suffix, no icon swap slots. No `Warning`, no `Read-only`.

---

## Configuration patterns

Quick configs for the most common fields. All values are property overrides on an Input instance.

### Mobile number (+91)
`Title=Mobile number` · `Show Prefix=true` · `Prefix=+91` · `Helper="We'll send an OTP"`

### OTP entry
Use the dedicated **OTP** component, not Input. `Title=Enter OTP` · `6-digit=true` (or `false` for 4-digit) · `State=Focused` while awaiting input · `Helper="Didn't get the code? Resend in 00:24"`. The timer inside the boxes is part of the Focused variant — don't recreate it.

### Search bar
`Show Leading Icon=true` · `Leading Icon=search` · `Show Trailing Icon=true` (only while there's a query) · `Trailing Icon=cancel` · `Helper="Try Wiom Pro"`

### Password
`Title=Password` · `Body=••••••••••••` · `Show Trailing Icon=true` · `Trailing Icon=visibility` (toggle to `visibility_off` on reveal)

### Currency amount
`Title=Recharge amount` · `Show Prefix=true` · `Prefix=₹` · `Show Suffix=true` · `Suffix=.00` · `Helper="Includes 18% GST"`

### Dropdown / Select
`Show Trailing Icon=true` · `Trailing Icon=expand_more` · `Body=` selected option text. Use `State=Read-only` if the field opens a bottom-sheet picker rather than accepting keyboard input.

### Clearable search / text
`Show Trailing Icon=true` · `Trailing Icon=cancel` — hide the clear icon in Default (empty) and Disabled states.

### Address (Textarea)
`Title=Installation address` · `Body=` full address · `Show Counter=true` · `Counter=60 / 200`

---

## Token bindings (summary)

Every property on every variant resolves to a Wiom foundation token. Full values live in `wiom-design-foundations`.

| Part | Token |
|---|---|
| Container fill (rest) | `color/surface/base` |
| Container fill (disabled, read-only) | `color/surface/subtle` |
| Border (rest) | `stroke/small` + `color/border/default` |
| Border (focused) | `stroke/medium` + `color/brand/primary` |
| Border (error / success / warning) | `stroke/medium` + matching status primary |
| Border (read-only) | `stroke/small` + `color/border/subtle` |
| Container radius | `radius/medium` (12px) |
| Padding | `space/16` H · `space/12` V |
| Internal gap (between slots) | `space/12` |
| Vertical gap (label → wrap → helper) | `space/8` |
| Label | `type/label/md` · text/primary on Default, text/secondary on all other active states, text/disabled on Disabled |
| Value / Prefix / Suffix | `type/body/lg` (16sp) · text/primary (text/disabled when Disabled; text/disabled for placeholder in Default) |
| Helper | `type/body/sm` (12sp) · text/secondary by default; matches border status color in Error/Success/Warning |
| Counter | `type/body/sm` · text/secondary always |
| Inline icons | `icon/sm` (20dp) · Material Symbols Rounded · FILL=1 · text/secondary by default; status color when in status state |

No shadow. No hardcoded hex. No fixed height anywhere — height hugs from padding + line-height.

---

## Rules

1. **Label-emphasis ramp is fixed.** Default = text/primary (highlighted). All other active states = text/secondary. Disabled = text/disabled. Do not override these per-instance.
2. **Error helper must be actionable.** Don't ship `Helper="Helper text"` in production — specify what's wrong and how to fix it.
3. **Affixes go in the Prefix/Suffix slot.** Never type `+91 ` or `₹` into the Body — it breaks validation, copy/paste, and semantic meaning.
4. **Read-only ≠ Disabled.** Use Read-only to display values the user can read and copy (text stays text/primary). Use Disabled only when the field is truly inactive and not relevant for now.
5. **One trailing icon at a time.** Status icons (Error/Success/Warning) take over the trailing slot. Don't stack multiple icons side-by-side — the field becomes noisy and touch targets crowd.
6. **Keep Title visible by default.** Don't rely on placeholder-only — once the user starts typing, the context disappears. Hide Title only when a parent section header already labels the field.
7. **Heights hug.** Wrap height = V padding + line-height. Textarea grows with content. Never set a fixed height on the wrap.
8. **Textarea alignment is top, not center.** When `singleLine = false` (Textarea), the placeholder/value Box must use `Alignment.TopStart` — not `CenterStart` — so the hint sits at the top of the multi-line area instead of floating to the vertical center. Likewise, the inner Row's `verticalAlignment` must switch to `Alignment.Top` so leading/trailing icons align to the first line, not the vertical middle of a tall field.
9. **Width on mobile = fill container** (usually 16px side margins). 328 is the reference width on a 360-wide screen.
10. **Icons swap, they don't stack.** Use `Leading Icon` / `Trailing Icon` INSTANCE_SWAP to pick the right glyph from Material Symbols Rounded. Do not draw a second icon on top.
11. **Counter only where it aids the user.** Character limits on textareas, chat drafts. Don't show it on short free-text fields. OTP has its own built-in timer — don't stack a counter on top.
12. **OTP is its own component.** Use `Field / OTP` for verification codes. Don't fake a 4/6-digit flow with a single Input + spaces in Body.

---

## Anti-patterns

- Drawing a custom input with manual rectangles + text — always use the component.
- Using `Disabled` to show non-editable but still-meaningful values — that's `Read-only`.
- Using `Error` state without updating the `Helper` prop.
- Putting the affix into `Body` (`+91 9876543210`) — use the `Prefix` slot.
- Piling icons: leading + prefix + trailing action + status = visual clutter. Pick the minimum set.
- Hiding Title with `Show Title=false` just to save vertical space — accessibility suffers.
- Setting a fixed height on a wrap override — defeats the hug layout.
- Using `CenterStart` / `CenterVertically` alignment on Textarea — placeholder drifts to the middle of the multi-line box. Use `TopStart` and `Alignment.Top`.
- Recoloring the label per instance — the state-driven color ramp exists for a reason.
- Building OTP as an Input with "4 3 8" spaced digits in `Body` — there's a dedicated OTP component with correct per-digit focus, box sizing, and timer.
- Redrawing the OTP timer on top of the Focused variant — it's already baked in.

---

## Implementation hint (Jetpack Compose)

Bind 1:1 to the Compose equivalent of the tokens above. Skeleton:

```kotlin
OutlinedTextField(
  value = state.body,
  onValueChange = { state.body = it },
  label = { Text(state.title, style = Wiom.type.labelMd, color = labelColorFor(state.state)) },
  placeholder = { Text(state.placeholder, color = Wiom.color.text.disabled) },
  leadingIcon = state.leadingIcon?.let { { WiomIcon(it, size = Wiom.icon.sm) } },
  trailingIcon = state.trailingIcon?.let { { WiomIcon(it, size = Wiom.icon.sm) } },
  isError = state.state == FieldState.Error,
  enabled = state.state != FieldState.Disabled,
  readOnly = state.state == FieldState.ReadOnly,
  shape = RoundedCornerShape(Wiom.radius.medium),
  colors = wiomFieldColors(state.state),
  supportingText = { Text(state.helper, style = Wiom.type.bodySm, color = helperColorFor(state.state)) }
)
```

`labelColorFor`, `helperColorFor`, and `wiomFieldColors` encode the state table. Prefix/Suffix use Compose's `prefix` / `suffix` slots (Material3 ≥ 1.1). Min-height comes from `body/lg` line-height + 12px vertical padding — do not pin a fixed height.
