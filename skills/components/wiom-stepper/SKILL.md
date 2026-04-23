---
name: wiom-stepper
description: Stepper components for Wiom DS — show progress through a known sequence of steps. Two layouts mapped to two semantics: `Stepper / Horizontal` (compact 2–6 steps with short labels — pinned at top of recharge / payment / onboarding wizards) and `Stepper / Vertical` (steps with title + description + optional Action slot per step — KYC, installation tracking, refund/order status). Shares a single `StepIndicator` atom across both layouts (4 states: Completed / Active / Upcoming / Error). Vertical's `StepItem` exposes an Action INSTANCE_SWAP so individual steps can hold a CTA, tertiary text link, Input, OTP field, or any Wiom interactive component. Tells you which layout to pick, how to configure step count, and which Wiom tokens back each part. Pairs with wiom-design-foundations for token values and wiom-input-fields / wiom-cta for Action slot components.
---

# Wiom Stepper

A Stepper shows where the user is in a **known, finite sequence** of steps. Two layouts; one shared atom.

- **`Stepper / Horizontal`** — row of indicators + short labels under each. 360dp wide. Compact. For wizards.
- **`Stepper / Vertical`** — column of indicators with title + description on the right and an optional **Action slot** (CTA / Tertiary CTA / Input / OTP / any component) per step. For status flows.
- **`StepIndicator`** (shared atom) — the 32×32 circle, 4 states.
- **`StepItem`** (atom for Vertical) — a single row of the vertical stepper. Editable independently when nested in the molecule.

If the count of steps is unknown or open-ended, use `Pagination / Bars` or a plain progress bar instead. A Stepper is wrong when steps may be added or skipped.

---

## When to use which

| Layout | Best for | Avoid for |
|---|---|---|
| **Stepper / Horizontal** | 2–5 step wizards with 1–2 word labels. Top of recharge / payment / KYC / onboarding flows. | Steps that need explanation. >6 steps. Bilingual labels that wrap. |
| **Stepper / Vertical** | Status flows (KYC, installation, refund, claim, order tracking). Anywhere each step needs a title + description + maybe an action. | Compact UIs without vertical room. Quick wizards where short labels are obvious — use Horizontal. |
| **Pagination / Bars** (existing) | Wizard progress where steps are unlabeled — just "how far am I". | Status flows where each step has meaning the user reads. Use Stepper. |

Rule of thumb: **swipeable + short labels → Horizontal · readable + scrollable + per-step content → Vertical · unlabeled progress → Bars**.

---

## When NOT to use a Stepper

1. **Unknown total steps.** If steps may be added or skipped, the count drifts and the Stepper misleads. Use a progress bar or skeleton.
2. **Single step.** Don't show "Step 1 of 1". Hide the component entirely.
3. **>6 discrete steps.** Restructure: merge micro-steps, or split into a sub-flow with its own Stepper.
4. **Unrelated tasks.** Stepper implies a sequence. For a checklist of independent items, use a list with checkboxes.
5. **Page-based long content.** If the user is paging through bills/transactions/items, use `Pagination / Counter`, not a Stepper.

---

## Step count — pick the smallest set that works

| Steps | When | How |
|---|---|---|
| **3** | Cleanest. Aim here first. Onboarding intros, simple wizards. | `Show Step 3 = on`, Step 4/5/6 = off |
| **4** | Standard for KYC, address+method+confirm+done. | Steps 3 + 4 on, 5 + 6 off |
| **5** | Recharge / payment with verify step. Common max for Horizontal. | Steps 3 + 4 + 5 on, 6 off |
| **6** | Hard cap. Installation tracking with all milestones. Vertical-only territory. | All Show Step booleans on |
| **2** | Valid minimum. Two genuinely peer steps. | All Show Step booleans off |

Step 1 and Step 2 are always visible — they cannot be hidden. Beyond 6 steps you have a scope problem.

---

## States — exactly one Active at a time

| State | Visual | Use when |
|---|---|---|
| `Completed` | Filled `brand/primary` circle + white check icon | Step the user has finished. |
| `Active` | White fill + 2px `brand/primary` ring + brand-colored number | The current step. Exactly one per stepper. |
| `Upcoming` | White fill + 1px `border/default` ring + secondary-text number | Step the user has not reached yet. |
| `Error` | Filled `negative/primary` circle + white `!` icon | Step the user must go back to fix (rejected document, payment failed, expired OTP). |

Skipped or optional states aren't supported as variants — fold the meaning into the description text on the step itself.

---

## StepIndicator (atom) — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | VARIANT | Active | One of Completed / Active / Upcoming / Error. |
| `Number` | TEXT | "1" | Step number shown for Active and Upcoming. Free text — supports `1`, `01`, `A`, etc. |

Use the bare atom only inside documentation or non-standard surfaces. In real screens, use `Stepper / Horizontal` or `Stepper / Vertical`.

---

## Stepper / Horizontal — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Current Step` | VARIANT | 1 | Which step is active (1–6). Steps before become Completed; after become Upcoming. |
| `Show Step 3` | BOOLEAN | true | Reveal step 3 + the connector before it. |
| `Show Step 4` | BOOLEAN | true | Reveal step 4 + the connector before it. |
| `Show Step 5` | BOOLEAN | true | Reveal step 5 + the connector before it. |
| `Show Step 6` | BOOLEAN | true | Reveal step 6 + the connector before it. Maximum 6. |

Step labels are inline frame text — override them via nested instance edits. Keep them to 1–2 words ("Plan", "Pay", "KYC").

---

## StepItem (atom for Vertical) — properties

This is the row used inside `Stepper / Vertical`. It can also be used standalone in custom layouts.

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | VARIANT | Active | Completed / Active / Upcoming / Error. |
| `Title` | TEXT | "Step title" | Step name. 1 short line. |
| `Description` | TEXT | "Step description here" | 1-line subtitle. Hide via `Show Description` if not needed. |
| `Show Description` | BOOLEAN | true | Toggle the description. |
| `Show Action` | BOOLEAN | false | Reveal the Action slot below the description. |
| `Action` | INSTANCE_SWAP | `CTA / Type=Primary, State=Default` | The action component. Swap to any compatible interactive: `CTA / Type=Tertiary` (text link), `Field / Input`, `Field / OTP`, custom button group, etc. |
| `Show Connector` | BOOLEAN | true | Show the vertical line below this step. Turn off on the last visible step. |

**The Action slot is the differentiator.** It lets a single step hold a button, a text CTA, an inline input, an OTP field — whatever the step actually requires. Mix per step in the same stepper. The connector below auto-stretches (FILL vertical) so the rail visually reaches the next indicator regardless of action height.

---

## Stepper / Vertical — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Current Step` | VARIANT | 1 | Same semantics as Horizontal. |
| `Show Step 3` | BOOLEAN | true | Reveal step 3. |
| `Show Step 4` | BOOLEAN | true | Reveal step 4. |
| `Show Step 5` | BOOLEAN | true | Reveal step 5. |
| `Show Step 6` | BOOLEAN | true | Reveal step 6. |

Per-step title / description / action edits happen on the **nested StepItem instances**. To turn an action on for one step, drill into that step instance and toggle `Show Action` + swap `Action`.

> **Plugin API limitation:** the molecule cannot auto-hide the previous step's connector when you toggle off `Show Step N`. After hiding the last visible step, drill into the now-last step and set `Show Connector = false` manually. Documented because it surprises people.

---

## Action slot — what to swap in (Vertical only)

| Action component | When to use |
|---|---|
| `CTA / Type=Primary, State=Default` (default) | Main step action — "Schedule installation", "Pay ₹500", "Continue". |
| `CTA / Type=Tertiary, State=Default` | Low-weight inline action — "View details →", "Track →", "Skip for now". |
| `CTA / Type=Secondary, State=Default` | When the step has both a primary and a secondary action elsewhere — keep the Stepper one secondary. |
| `Field / Input` | Inline data entry inside a step — "Enter pincode", "Enter UPI ID". |
| `Field / OTP` | OTP entry inline within a verification step. Pairs naturally with mobile / Aadhaar verification. |
| `Field / Textarea` | Rare — only when the step genuinely needs multi-line input. |
| Custom action group | If a step needs Input + Button side-by-side, build a small wrapper component (Input on left FILL, Button on right) and swap that in. |

Don't put two interactive elements directly inside a step row outside of the Action slot. The slot is the contract.

---

## Configuration patterns

### Recharge wizard, Step 2 of 5 (Horizontal)
`Current Step = 2`, `Show Step 3/4/5 = on`, `Show Step 6 = off`. Pin to the top of every step screen. Labels: "Plan", "Method", "Confirm", "Pay", "Done".

### KYC vertical with action on current step (Vertical)
Steps: "Personal info", "Address", "Aadhaar", "Selfie". `Current Step = 3`. On Step 3 (Aadhaar), drill in → `Show Action = true`, `Action = Field / OTP` for inline OTP entry. Other steps just title + description.

### Installation tracking (Vertical, status display)
Steps: "Order placed", "Approved", "Dispatched", "Installed", "Activated". `Current Step = 3`. On Step 4 (Installed), `Show Action = true`, `Action = CTA / Type=Tertiary` with text "Reschedule →". Others have no action.

### Order status with error (Vertical, Error state)
Override one step's State to `Error`. Use the description to say what went wrong: "Aadhaar photo blurred. Tap to retry." Add a primary CTA via Action slot: "Re-upload".

### 3-step onboarding (Horizontal)
`Show Step 4 / 5 / 6 = off`. Labels: "Welcome", "Connect", "Done". Stepper sits at top of each onboarding screen.

---

## Token bindings (summary)

Every part resolves to a Wiom foundation token. Full values live in `wiom-design-foundations`.

### StepIndicator (atom)
| Part | Token |
|---|---|
| Indicator size | 32 × 32 dp · `radius-full` |
| Completed fill | `bg.brand` |
| Completed icon | `check` (filled) · 20dp · `text.onBrand` |
| Active fill | `bg.default` |
| Active stroke | 2dp · `bg.brand` |
| Active number color | `bg.brand` · `type.label.md` (14sp SemiBold) |
| Upcoming fill | `bg.default` |
| Upcoming stroke | 1dp · `stroke.default` |
| Upcoming number color | `text.subtle` · `type.label.md` |
| Error fill | `bg.critical` |
| Error icon | `priority_high` (filled) · 20dp · `text.onBrand` |

### Connectors
| Layout | Part | Token |
|---|---|---|
| Horizontal | Line height | 2dp · `radius-full` |
| Vertical | Line width | 2dp · `radius-full` |
| Both | Filled (after Completed step) | `bg.brand` |
| Both | Default | `stroke.default` |

### Stepper / Horizontal
| Part | Token |
|---|---|
| Wrapper width | 360dp (FIXED) |
| Step column gap (indicator → label) | `space-8` |
| Label typography | `type.label.sm` (12sp SemiBold) |
| Label color (Active / Completed) | `text.default` |
| Label color (Upcoming) | `text.subtle` |

### StepItem (Vertical)
| Part | Token |
|---|---|
| Row gap (rail → content) | `space-12` |
| Rail width | 32dp |
| Rail gap (indicator → connector) | `space-4` |
| Title typography | `type.label.md` (14sp SemiBold) |
| Title color (Active / Completed / Error) | `text.default` |
| Title color (Upcoming) | `text.subtle` |
| Title → Description gap | `space-8` (matches card recipe: title → subtitle = 8px) |
| Description typography | `type.body.sm` (12sp Regular, 16 LH) |
| Description color | `text.subtle` |
| Description → Action gap (ActionWrap paddingTop) | `space-16` (matches card recipe: Content → CTA = 16px) |
| Content bottom padding (gap to next step) | `space-24` (comfortable section gap) |

No shadows. No hardcoded hex. No fixed inner widths inside StepItem — content `FILL`s the row width minus rail; action `FILL`s content width.

---

## Rules

1. **One Active step at a time.** Steps before become Completed automatically via the `Current Step` variant. Don't override two steps to Active.
2. **Steppers are for known, finite sequences.** If steps can be added, skipped, or are unknown, use a progress bar or `Pagination / Bars`.
3. **Step 1 and Step 2 are always visible.** Design for the 2-step floor; never put a critical step in slot 3+ that you then hide for some users.
4. **Max 6 steps.** Restructure beyond that — merge micro-steps or split into sub-flows.
5. **Token-only values.** Every color, size, spacing, radius, stroke must come from Wiom variables.
6. **Connector color matches what came before.** Brand-filled when previous step is Completed; subtle border otherwise. Don't repaint manually.
7. **Use Error sparingly and meaningfully.** Only when the user must go back and fix that step. Not for warnings or info.
8. **Horizontal labels are 1–2 words.** "Plan", "Pay", "KYC". Vertical title is 1 short line; description is 1 line.
9. **Action slot is one component.** If a step needs two interactive elements, wrap them into a small group component first and swap that in.
10. **Hide the last visible step's connector.** After turning off Show Step N, drill into step N-1 and set `Show Connector = false`. (Plugin API limit; documented above.)
11. **Numbers stay sequential.** Don't relabel "1, 2, 3" to "A, B, C" without reason. If the flow numbers steps differently, override the Number on each StepIndicator instance.
12. **Same Stepper across all step screens of a flow.** Don't redesign the bar between screens — only the Current Step variant changes.

---

## Anti-patterns

- Drawing custom indicator circles + lines instead of using the components.
- More than 6 steps; adding a "More" indicator; horizontal scroll on a Stepper.
- Putting verbs as Horizontal labels ("Pay now", "Skip"). Use nouns for steps ("Payment", "Skip" is an action, not a step).
- Showing the Stepper on a confirmation / success screen with all steps Completed and no further action — at that point the user is done, drop the Stepper or replace with a confirmation summary.
- Using a Stepper for a checklist of unrelated tasks.
- Putting a primary CTA both **inside** a StepItem's Action slot **and** below the Stepper as a sticky bottom button — duplicates the action and confuses focus.
- Mixing Horizontal and Vertical Steppers within the same flow (recharge using Horizontal, then jump to a Vertical for one step). Pick one layout per flow.
- Animating the entire Stepper on every state change. Platform-native crossfade on the Active indicator is enough.
- Repainting Active or Completed circles in a non-brand color to "match a section".
- Using `Field / OTP` in the Action slot but also showing a separate keyboard / OTP UI elsewhere on the same screen — pick one.

---

## Implementation hints (Jetpack Compose)

### StepIndicator
```kotlin
@Composable
fun StepIndicator(
  state: StepState,           // Completed / Active / Upcoming / Error
  number: Int,
  modifier: Modifier = Modifier,
) {
  Box(
    modifier
      .size(32.dp)
      .clip(CircleShape)
      .background(
        when (state) {
          StepState.Completed -> Wiom.bg.brand
          StepState.Error     -> Wiom.bg.critical
          else                -> Wiom.bg.default
        }
      )
      .border(
        width = if (state == StepState.Active) 2.dp else 1.dp,
        color = when (state) {
          StepState.Active   -> Wiom.bg.brand
          StepState.Upcoming -> Wiom.stroke.default
          else               -> Color.Transparent
        },
        shape = CircleShape
      ),
    contentAlignment = Alignment.Center,
  ) {
    when (state) {
      StepState.Completed -> Icon(painterResource(R.drawable.ic_check_filled), null,
                                   tint = WiomColors.iconInverse, modifier = Modifier.size(20.dp))
      StepState.Error     -> Icon(painterResource(R.drawable.ic_priority_high_filled), null,
                                   tint = WiomColors.iconInverse, modifier = Modifier.size(20.dp))
      else -> Text(
        number.toString(),
        style = Wiom.type.labelMd,
        color = if (state == StepState.Active) Wiom.bg.brand else Wiom.text.subtle,
      )
    }
  }
}
```

### Stepper / Horizontal
```kotlin
@Composable
fun StepperHorizontal(
  steps: List<HorizontalStep>,
  currentStep: Int,
  modifier: Modifier = Modifier,
) {
  Row(
    modifier.width(360.dp),
    horizontalArrangement = Arrangement.Center,
    verticalAlignment = Alignment.Top,
  ) {
    steps.forEachIndexed { idx, step ->
      val state = stateFor(idx + 1, currentStep)
      Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
      ) {
        StepIndicator(state, idx + 1)
        Text(step.label, style = Wiom.type.labelSm,
             color = if (idx + 1 <= currentStep) Wiom.text.default else Wiom.text.subtle)
      }
      if (idx < steps.lastIndex) {
        Box(
          Modifier
            .weight(1f)
            .height(32.dp)
            .padding(top = 15.dp)
            .height(2.dp)
            .background(if (idx < currentStep - 1) Wiom.bg.brand else Wiom.stroke.default,
                        shape = RoundedCornerShape(Wiom.radius.full))
        )
      }
    }
  }
}
```

### Stepper / Vertical with Action slot
```kotlin
@Composable
fun StepperVertical(steps: List<VerticalStep>, currentStep: Int) {
  Column(Modifier.fillMaxWidth()) {
    steps.forEachIndexed { idx, step ->
      val state = stateFor(idx + 1, currentStep)
      val isLast = idx == steps.lastIndex
      Row(verticalAlignment = Alignment.Top) {
        // Rail
        Column(
          Modifier.width(32.dp).fillMaxHeight(),
          horizontalAlignment = Alignment.CenterHorizontally,
          verticalArrangement = Arrangement.spacedBy(Wiom.space.s4),
        ) {
          StepIndicator(state, idx + 1)
          if (!isLast) {
            Box(
              Modifier
                .width(2.dp).weight(1f)
                .background(if (idx < currentStep - 1) Wiom.bg.brand else Wiom.stroke.default,
                            shape = RoundedCornerShape(Wiom.radius.full))
            )
          }
        }
        Spacer(Modifier.width(Wiom.space.s12))
        // Content
        Column(Modifier.weight(1f).padding(bottom = Wiom.space.s24)) {
          Text(step.title, style = Wiom.type.labelMd,
               color = if (state == StepState.Upcoming) Wiom.text.subtle else Wiom.text.default)
          step.description?.let {
            Text(it, style = Wiom.type.bodySm, color = Wiom.text.subtle)
          }
          step.action?.let {
            Spacer(Modifier.height(Wiom.space.s8))
            it()  // pass any composable: WiomCtaPrimary, WiomCtaTertiary, WiomInput, WiomOtp, etc.
          }
        }
      }
    }
  }
}
```

The Action slot in code is the same idea as in Figma — pass any composable. Don't hardcode action types into the StepItem.

---

*Wiom Design System — Stepper skill. Pairs with `wiom-design-foundations` for token values, `wiom-cta` for the CTA component used in the Action slot, and `wiom-input-fields` for Input / OTP options.*

---

## See also

- `wiom-design-foundations` — stroke.brand.focus for the Active ring, typography scale
- `wiom-compose-layout-rules` — Compose structural rules
- `wiom-cta` — Action-slot CTAs (Primary / Secondary / Tertiary)
- `wiom-input-fields` — Action-slot input / OTP / textarea variants
- `wiom-pagination` — sibling progress component when steps are unlabeled
