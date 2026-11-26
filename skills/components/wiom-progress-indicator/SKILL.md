---
name: wiom-progress-indicator
description: >
  Wiom Progress Indicator — the determinate-progress family for the
  Wiom apps. 4 component sets · 29 ship variants total: `Linear`
  (plain bar · 6 = 2 sizes × 3 states) · `Completion` (rich
  completion meter with title + status pill + value · 4 state
  variants) · `Milestones` (named-stage bar · 14 = 4 stage counts
  (2/3/4/5) × current stage 1..N + Has Labels boolean) · `Circular`
  (donut, sm 24 dp / lg 48 dp, no centre label · 5 progress variants). All
  reversed semantics — more = better, greener — since Wiom is an
  unlimited-data ISP and never renders a "usage meter". State
  colour ladder: < 35 % `bg.info` (purple) → 35–80 % `bg.brand`
  (pink) → > 80 % `bg.positive` (green). Reference for generating
  Jetpack Compose code with `WiomProgressLinear` /
  `WiomProgressCompletion` / `WiomProgressMilestones` /
  `WiomProgressCircular` and for instancing the Figma component
  sets via MCP. Use this skill EVERY TIME you place a determinate
  progress bar, completion meter, installation tracker, KYC /
  onboarding / profile completion card, delivery tracker, or any
  Figma frame involving forward motion toward a known target.
  Trigger on: progress, progress indicator, progress bar,
  completion meter, milestone, tracker, installation progress, KYC
  progress, profile completion, onboarding progress, circular
  progress, donut progress, instance of 2814:14 / 2815:38 /
  3108:496 / 3109:110. Pairs with wiom-design-foundations
  (tokens), wiom-loader (sibling skill — `Loader/Linear`
  Indeterminate is for short waits with no progress signal;
  `Loader/Brand` Overlay for blocking critical ops),
  wiom-list-item (host for completion cards), wiom-bottomsheet
  (outcome surface when progress completes), and
  wiom-compose-layout-rules (Modifier discipline).
---

# Wiom Progress Indicator — 4 patterns for determinate progress

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component sets `2814:14` (Linear), `2815:38` (Completion), `3108:496` (Milestones), `3109:110` (Circular), and the `wiom-design-foundations` token set.

**4 components · 29 ship variants · all determinate · reversed semantics (more = better).**

| Component | Set | Variants | Use for |
|---|---|---|---|
| `Linear` | `2814:14` | 6 = 2 sizes × 3 states | Plain bar with no chrome — file upload · inline % next to other content |
| `Completion` | `2815:38` | 4 = `Just Started` / `In Progress` / `Almost There` / `Complete` | Rich completion meter with title + status pill + value numbers · KYC / referral / earnings cards |
| `Milestones` | `3108:496` | 14 = `Stage Count` 2/3/4/5 × `Current Stage` 1..N | Multi-stage compact tracker with named stages · install / delivery / verification |
| `Circular` | `3109:110` | 5 = `Progress` 0/25/50/75/100 % | Donut, sm 24 dp or lg 48 dp, no centre label (the host shows the %) · compact dashboards · status tile |

> **Wiom progress is reversed-semantics.** Wiom is an unlimited-data ISP; we never render a "usage meter" where full = bad. Every progress visual in the app shows forward motion toward something the user wants — KYC done, install done, target hit, file uploaded.

---

## 1. Figma — instancing the components

4 component sets, instanced separately. Strings are **case-sensitive**.

### Linear — `2814:14` (6 ship variants)

| `Size` ↓ \ `State` → | `brand` | `info` | `positive` |
|---|---|---|---|
| `sm` (4 dp) | `2814:2` | `2814:4` | `2814:6` |
| `md` (8 dp) | `2814:8` | `2814:10` | `2814:12` |

All 6 variants render a **60 % preview fill** in Figma — Figma has no NUMBER component property, so per-instance percentages can't be set via Properties. Designers resize the inner `Fill` rect manually; in code, `progress: Float` drives the width.

| Property | Value |
|---|---|
| Track width | 328 dp · `bg.muted` · `radius-full` |
| Track height | 4 dp (`Sm`) · 8 dp (`Md`) |
| Fill | per state colour · `radius-full` · width = `progress × track width` |

### Completion — `2815:38` (4 ship variants)

State drives **fill %** AND the **state colour** AND the **status-pill copy** in lockstep.

| `State` | Node | Fill % | Fill colour | Pill bg / text | Pill copy |
|---|---|---|---|---|---|
| `Just Started` | `2815:2` | 20 % | `bg.info` | `bg.info.subtle` / `text.info` | `● Just started` |
| `In Progress` | `2815:11` | 60 % | `bg.brand` | `bg.brand.subtle` / `text.brand` | `● In progress` |
| `Almost There` | `2815:20` | 88 % | `bg.positive` | `bg.positive.subtle` / `text.positive` | `● Almost there` |
| `Complete` | `2815:29` | 100 % | `bg.positive` | `bg.positive.subtle` / `text.positive` | `● Complete` |

Title (`type.label.md`), value numbers (`type.body.sm`), and pill label are **hardcoded per variant** — Figma's TEXT property has a single shared default across all variants, so they can't bubble up. Designers override per-instance; in code, the helper composable accepts `title` / `value` / `subtitle` strings and resolves the pill / fill from the `state` enum.

### Milestones — `3108:496` (14 ship variants)

`Stage Count` × `Current Stage` matrix — only valid pairs ship.

| `Stage Count` | `Current Stage = 1` | `= 2` | `= 3` | `= 4` | `= 5` |
|---|---|---|---|---|---|
| `2` | `3108:62` | `3108:80` | — | — | — |
| `3` | `3108:98` | `3108:123` | `3108:148` | — | — |
| `4` | `3108:173` | `3108:205` | `3108:237` | `3108:269` | — |
| `5` | `3108:301` | `3108:340` | `3108:379` | `3108:418` | `3108:457` |

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Stage Count` | Variant | `5` | The total number of stages — 2 / 3 / 4 / 5 |
| `Current Stage` | Variant | `1` | The 1-based current stage (must be ≤ `Stage Count`) |
| `Has Labels` | Boolean | `true` | Reveals the per-stage label row below the bar |
| `Title` | Text | `"Net box installation"` | The title above the bar |

Subtitle line ("Step 3 of 5 · Router") is **NOT** a component property — it's a direct text layer; designers edit per instance. Same Figma TEXT-property limitation as Completion. Stage labels and stage icons are also direct-edit (no INSTANCE_SWAP property yet).

### Circular — `3109:110` (5 ship variants)

| `Progress` | Node | Fill arc | Center label |
|---|---|---|---|
| `0` | `3109:91` | (none) | `0%` |
| `25` | `3109:94` | 25 % arc · brand | `25%` |
| `50` | `3109:98` | 50 % arc · brand | `50%` |
| `75` | `3109:102` | 75 % arc · brand | `75%` |
| `100` | `3109:106` | 100 % arc · positive | `100%` |

Two sizes: **sm 24 dp / 3 dp stroke** (default) and **lg 48 dp / 4 dp stroke**. **No centre label**: the host text carries the % (Figma's 48 dp variants still show a centre %, Figma to update). Arc colour is the `tone` the caller passes (default Brand); use `ladderTone(progress)` for the §3 ladder.

> Aligned with the shipped library and iOS skills, 26 Nov 2026 (`WiomProgressIndicator.kt:36-48`, `:167-223`).

### Code enum ↔ Figma variant string

| Kotlin | Figma value |
|---|---|
| `WiomProgressLinearSize.{Sm, Md}` | `sm` / `md` |
| `WiomProgressLinearState.{Brand, Info, Positive}` | `brand` / `info` / `positive` |
| `WiomProgressCompletionState.{JustStarted, InProgress, AlmostThere, Complete}` | `Just Started` / `In Progress` / `Almost There` / `Complete` *(case-sensitive — capitalised, with spaces)* |

Milestones and Circular take a `progress: Float` (0 f..1 f) or a `currentStage: Int` (1-based) at the call site — code resolves the closest Figma variant for design-handoff parity.

---

## 2. Anatomy

### Linear — plain bar

```
┌─ 328 dp wide ────────────────────────────────────────────────┐
│ ████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   Track: bg.muted · radius-full
└──────────────────────────────────────────────────────────────┘   Fill:  state colour · radius-full · width = progress × 328
   ↑ 4 dp (Sm) or 8 dp (Md) tall
```

### Completion — rich completion meter

```
┌──────────────── 328 dp wide ────────────────────────────────────┐
│ Net box installation                            3 of 5 steps    │   Header row · type.label.md (title) · type.body.sm (value)
│  ●  In progress                                                  │   ↑ space-4 ↓
│                                                                  │   Status pill · type.label.sm · pill bg + text per state
│ ───────────────── ↑ space-12 ↓ ────────────────────              │
│ ████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░         │   Bar (Md, 8 dp) · width = state's fill %
└──────────────────────────────────────────────────────────────────┘
```

| Slot | Token |
|---|---|
| Title | `type.label.md` (14/20 SemiBold) · `text.default` |
| Value (right-aligned) | `type.body.sm` (12/16) · `text.subtle` |
| Status pill | `type.label.sm` (12/16 SemiBold) · pill bg + text per state |
| Pill padding · radius | 8 h · 2 v · `radius-full` |
| Header → bar gap | `space-12` (12 dp) |
| Title → pill gap | `space-4` (4 dp) |
| Bar | 328 × 8 dp · per `Linear` Md spec |

### Milestones — named-stage tracker

```
┌──────────────── 328 dp wide ────────────────────────────────────┐
│ Net box installation                                            │   Title · type.label.lg (16/24 SemiBold) · text.default
│ ↑ space-4 ↓                                                      │
│ Step 3 of 5 · Router                                            │   Subtitle · type.body.sm · text.subtle
│                                                                  │
│ ↑ space-20 ↓                                                     │
│  ●━━━━●━━━━●━━━━○━━━━○                                           │   Bar row · 24 dp circles · 4 dp connectors · fluid spacing
│  Survey Cable Router  Activate  Done                             │   Stage labels (when Has Labels = true)
│  ↑ space-20 ↓                                                    │     · type.body.sm (12/16) · centred in equal-width columns
└──────────────────────────────────────────────────────────────────┘
```

| Slot | Token |
|---|---|
| Title | `type.label.lg` (16/24 SemiBold) · `text.default` |
| Subtitle | `type.body.sm` · `text.subtle` |
| Title → subtitle gap | `space-4` |
| Header → bar row gap | `space-20` (20 dp) — non-canonical token; lays out as raw `20.dp` |
| Bar row → labels gap | `space-20` |
| Stage circle | 24 × 24 dp · `radius-full` · padding 4 |
| Stage glyph | 16 × 16 dp · `icon.inverse` (reached) · `icon.nonAction` (unreached) |
| Stage circle fill | `bg.brand` (reached) · `bg.muted` (unreached) |
| Connector | 4 dp tall · `radius-full` · `bg.brand` (reached side) · `bg.muted` (after current) |
| Stage label — past | `text.default` |
| Stage label — current | **`text.brand` (#D9008D pink)** — the current-stage emphasis |
| Stage label — future | `text.subtle` |

> **Layout is fluid, not fixed.** Stage circles are auto-sized; connectors are `flex 1` between them; labels sit in equal-width columns, each label centred in its column (as shipped; the first and last labels are not exactly under their circles). Older skill drafts described fixed pixel positions (12 / 86 / 160 / 234 / 308) — V2 ships flexbox-style geometry, so the same anatomy works for 2 / 3 / 4 / 5 stages without manual re-positioning.

### Circular: sm 24 dp / lg 48 dp donut, no centre label

```
┌─ 48 × 48 dp ─┐       lg · sm is 24 × 24 dp with a 3 dp stroke
│   ╭──────╮   │       Track:    full ring · 4 dp stroke · bg.muted
│   │      │   │       Progress: arc from 12 o'clock clockwise · 4 dp stroke ·
│   │      │   │                 bg.{tone} (caller-passed; default brand)
│   ╰──────╯   │       Centre:   empty (host text carries the %)
└──────────────┘
```

| Slot | Token |
|---|---|
| Bounding box | sm 24 × 24 dp (default) · lg 48 × 48 dp |
| Ring stroke | sm 3 dp · lg 4 dp |
| Track | `bg.muted` |
| Progress arc | caller `tone` (default `bg.brand`); `ladderTone(progress)` gives `bg.info` (< 35 %) · `bg.brand` (35–80 %) · `bg.positive` (> 80 %) |
| Centre label | none |
| Stroke cap | round |
| Start angle | -90° (12 o'clock) |
| Direction | clockwise |

---

## 3. Surface contract — bar / arc + light chrome

Each progress component **owns its bar / arc / chrome** as a self-contained surface: track, fill, optional title / status pill. Hosting cards, screen padding, and tap affordances live on the parent.

### What this means in code

- `Modifier.fillMaxWidth()` is fine on Linear / Completion / Milestones; Circular has a fixed box (24 or 48 dp).
- **No tap target** on any progress visual — they're display-only. If a card hosts a Completion meter and tapping should open a detail screen, the **card** is the tap target (e.g. a `wiom-list-item` row), not the progress visual.
- **Progress is NOT a CTA.** A progress visual plus an ambiguous label is not a call to action. If the user can act, route via a `wiom-cta` next to the progress, or wrap the card in a tap-to-open row.

### Reversed semantics — colour ladder

| Range | Fill colour | Status mood |
|---|---|---|
| `< 35 %` | `bg.info` (#6D17CE — purple) | Just started — neutral, not concerning |
| `35 – 80 %` | `bg.brand` (#D9008D — pink) | In progress — active, brand-forward |
| `> 80 %` | `bg.positive` (#008043 — green) | Almost there / complete — success |

**Never use `bg.warning` / `bg.critical`** on progress fill. Wiom progress is forward motion toward something good — there's no "you're doing badly" state. Stuck / blocked / error flows belong on the **outcome surface** (a `wiom-bottomsheet` Illustration in Critical / Warning tone) or a status pill *next to* the progress, not as a fill colour.

### Animation ownership

| Component | Animation | When |
|---|---|---|
| `Linear` | Fill width tweens to new `progress` over 600 ms cubic-bezier; colour cross-fades over 300 ms when crossing a threshold | On `progress` value change |
| `Completion` | Same as Linear, plus pill copy + bg cross-fade over 300 ms | On `state` change |
| `Milestones` | Connector colour cross-fades over 300 ms; stage circle fill + glyph swap over 300 ms; current-stage label colour cross-fades | On `currentStage` change |
| `Circular` | Arc length tweens over 600 ms cubic-bezier; colour cross-fades when the caller's tone changes | On `progress` value change |

All transitions are caller-driven — no infinite animation. Determinate progress shouldn't *appear* to be doing something when nothing is changing.

---

## 4. Picking the right progress component — and when not to use one at all

| Need | Component | Why |
|---|---|---|
| Plain % bar with no chrome — file upload, inline progress next to a label | **`Linear`** | Lowest weight; fits in a row |
| Rich completion card — title + status pill + value numbers — KYC, referral, earnings | **`Completion`** | All three signals at once on a home / dashboard card |
| Multi-stage compact bar with named stages (2 / 3 / 4 / 5) — install, delivery, verification | **`Milestones`** | Conveys *where* you are, not just *how far* |
| Compact donut on a status tile or KPI summary | **`Circular`** | Small footprint, single % at a glance |
| Per-step detail with description + action (CTA / input / OTP) per step | **Build per-step screens** — each step is its own screen / bottom sheet with its own header, body and CTA; surface position via `Pagination/Bars` or a compact `Milestones` strip at the top | Progress here doesn't carry per-step content, and Wiom doesn't ship a dedicated Stepper component — compose from existing primitives |
| Top-of-screen wizard nav with short labels | **`Pagination/Bars`** with labels under each bar | This skill is for status visuals, not for navigation; Pagination Bars handle position-on-known-list with labels just fine |
| Indeterminate "something is happening" with no progress signal | **`Loader/Spinner`** *(see `wiom-loader`)* | This skill is determinate only |
| Short determinate wait inside a snackbar / inline strip (no milestones) | **`Loader/Linear`** *(see `wiom-loader`)* | Loader's Linear = brand-only, snackbar-style; this skill's Linear = state-coloured for completion meters |
| Unlabeled "how far am I" wizard dots | **`Pagination/Bars`** *(see `wiom-pagination`)* | Pagination owns position-on-known-list patterns |
| Blocking critical op > 3 s (payment, KYC upload) | **`Loader/Brand` Overlay** *(see `wiom-loader`)* + outcome bottom sheet | Routine determinate progress isn't the right surface for trust-heavy waits |
| Data-usage meter (full = bad) | **None** — Wiom is unlimited; this use case doesn't exist | If imported from another product, push back on the requirement |

### Use the lightest progress that fits

| Moment | Right call |
|---|---|
| Home-screen status summary that links to a detail screen | `Milestones` (compact) → tap card → detail screen with per-step cards (one screen / sheet per step) |
| KYC / profile completion card | `Completion` |
| File / image upload progress | `Linear` (Md, brand) — flips to `Linear` (positive) on completion |
| Single status tile on a dashboard ("Network 75 %") | `Circular` |
| Delivery tracker on a tracking screen | `Milestones` with `Stage Count = 3` (Origin / In transit / Delivered) |
| Partner installation dashboard card | `Milestones` with `Stage Count = 5` |

### Don't reach for a progress indicator when

- The wait has no progress signal — use `Loader/Spinner` / `Loader/Brand` instead.
- The user can't act on the progress and it never advances — drop it; show static state instead.
- The data is "not yet started, may never start" — no surface; show the flow's entry CTA.
- The progress represents "how much you've used" of a quota — Wiom doesn't ship usage meters.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Shared

| Slot | Token |
|---|---|
| Track (all components) | `bg.muted` (#E8E4F0) |
| Track / fill / arc radius | `radius-full` |
| Fill — `< 35 %` | `bg.info` (#6D17CE) |
| Fill — `35 – 80 %` | `bg.brand` (#D9008D) |
| Fill — `> 80 %` | `bg.positive` (#008043) |
| Pill bg — info / brand / positive | `bg.info.subtle` (#F1E5FF) · `bg.brand.subtle` (#FFE5F6) · `bg.positive.subtle` (#E1FAED) |
| Pill text — info / brand / positive | `text.info` · `text.brand` · `text.positive` |

### Linear

| Slot | Token |
|---|---|
| Track height | 4 dp (`Sm`) · 8 dp (`Md`) |
| Track width | 328 dp (Figma) — `fillMaxWidth()` in code |
| Fill | per `state` enum |

### Completion

| Slot | Token |
|---|---|
| Title | `type.label.md` (14/20 SemiBold) · `text.default` |
| Value (right) | `type.body.sm` (12/16 Regular) · `text.subtle` |
| Status pill label | `type.label.sm` (12/16 SemiBold) · per state |
| Pill padding | 8 h · 2 v |
| Title → pill gap | `space-4` |
| Header → bar gap | `space-12` |
| Bar | 328 × 8 dp (`Md`) · per state colour |

### Milestones

| Slot | Token |
|---|---|
| Title | `type.label.lg` (16/24 SemiBold) · `text.default` |
| Subtitle | `type.body.sm` · `text.subtle` |
| Title → subtitle gap | `space-4` |
| Header → bar row gap | 20 dp |
| Bar row → labels gap | 20 dp |
| Stage circle | 24 × 24 dp · `radius-full` · padding 4 |
| Stage glyph | 16 × 16 dp · `icon.inverse` (reached) · `icon.nonAction` (unreached) |
| Stage circle fill | `bg.brand` (reached) · `bg.muted` (unreached) |
| Connector | 4 dp · `radius-full` · `bg.brand` / `bg.muted` |
| Label — past · current · future | `text.default` · `text.brand` · `text.subtle` |
| Label style | `type.body.sm` (12/16) · centred in equal-width columns |

### Circular

| Slot | Token |
|---|---|
| Bounding box | sm 24 × 24 dp (default) · lg 48 × 48 dp |
| Ring stroke | sm 3 dp · lg 4 dp · round cap |
| Track | `bg.muted` |
| Progress arc | caller `tone` (default brand; `ladderTone(progress)` for the ladder) · arc start −90° · clockwise |
| Centre label | none |

---

## 6. Universal rules

1. **Reversed semantics — more = better, greener.** No usage meters. No `bg.warning` / `bg.critical` on the fill. Stuck / failed flows route to an outcome surface (bottom sheet illustration) or a status pill *next to* the progress, never colour the bar red.
2. **Colour ladder by threshold, applied by the caller.** `< 35 %` → info · `35 – 80 %` → brand · `> 80 %` → positive. Linear and Circular take a `tone` (default Brand) and do not auto-ladder (as shipped); pass `ladderTone(progress)` when the ladder applies, never hand-pick a tone per screen. Aligned with the shipped library and iOS skills, 26 Nov 2026.
3. **Progress is display-only.** No tap target, no CTA, no dismiss. The hosting card / row owns interaction.
4. **Pick the right component per §4.** Plain bar → Linear · rich card → Completion · named stages → Milestones · compact donut → Circular. Don't stack multiple progress visuals on the same card.
5. **Milestones supports 2 / 3 / 4 / 5 stages — pick the right count for the flow.** Don't pad a 3-stage flow to 5 just to fit the original Figma layout; V2 ships all four stage counts.
6. **Stage labels and icons are direct-edit per instance.** Figma TEXT / INSTANCE_SWAP properties have a single shared default across variants; the V2 file works around this by leaving them as direct layers. In code, the helper composable accepts `List<WiomMilestoneStage>` so call sites name their stages explicitly. **Stage icons:** prefer Material3 `Icons.Rounded.*` (`WiomMilestoneIcon.vector(...)`) wherever the family has a fit; fall back to a brand drawable (vector / animated json / raster) via `WiomMilestoneIcon.resource(R.drawable.ic_*_rounded)` only when no Material3 rounded icon matches. Never `Icons.Default / Filled / Outlined / Sharp / TwoTone`.
7. **Animate caller-driven changes only.** Determinate progress shouldn't appear animated when nothing is changing. Tween fill / arc / colour over 300–600 ms when `progress` / `state` / `currentStage` updates.
8. **For long indeterminate / blocking waits, use a loader instead.** `Loader/Spinner` for short, `Loader/Brand` Overlay for blocking critical ops (payment, KYC upload). When the wait completes, route to a `wiom-bottomsheet` Illustration outcome surface — don't linger on a progress visual.
9. **Progress vs per-step screens:** if the user is acting per step (filling a form, tapping CTAs), build per-step screens — one screen / bottom sheet per step, with its own header, body and CTA — and surface position with `Pagination/Bars` or a compact `Milestones` strip. If they're observing forward motion, use this skill. Compact tracker cards on home screens link through to the per-step detail.
10. **Tokens only — element-first.** No raw hex. No legacy `colors.bg.brand` / `WiomColor.brandPrimary` namespaces; use `Wiom.bg.brand` / `Wiom.text.subtle` / `Wiom.bg.muted`.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomProgressLinearSize     { Sm, Md }
enum class WiomProgressLinearState    { Brand, Info, Positive }
enum class WiomProgressCompletionState { JustStarted, InProgress, AlmostThere, Complete }

/**
 * A named stage in a Milestones bar.
 *
 * Prefer the Material3 `Icons.Rounded.*` family — use `vector(...)`.
 * Fall back to a brand-specific drawable (vector / animated json /
 * raster) only when no Material3 rounded icon fits — use `resource(...)`.
 */
sealed interface WiomMilestoneIcon {
    data class Vector(val icon: ImageVector) : WiomMilestoneIcon
    data class Resource(@DrawableRes val resId: Int) : WiomMilestoneIcon

    companion object {
        fun vector(icon: ImageVector): WiomMilestoneIcon = Vector(icon)
        fun resource(@DrawableRes resId: Int): WiomMilestoneIcon = Resource(resId)
    }
}

data class WiomMilestoneStage(
    val label: String,
    val icon: WiomMilestoneIcon,
)

/** The §3 ladder, shared by Linear and Circular. */
fun ladderTone(progress: Float): WiomProgressLinearState = when {
    progress < 0.35f -> WiomProgressLinearState.Info
    progress > 0.80f -> WiomProgressLinearState.Positive
    else             -> WiomProgressLinearState.Brand
}

enum class WiomProgressCircularSize { Sm, Lg }                // 24 dp / 3 dp · 48 dp / 4 dp

@Composable
fun WiomProgressLinear(
    progress: Float,                                          // 0f..1f
    size: WiomProgressLinearSize = WiomProgressLinearSize.Sm, // default sm (4 dp), as shipped
    tone: WiomProgressLinearState = WiomProgressLinearState.Brand, // pass ladderTone(progress) for the ladder
    modifier: Modifier = Modifier,
)

@Composable
fun WiomProgressCompletion(
    title: String,
    value: String,                                            // "3 of 5 steps" · "₹4,500 of ₹10,000" · "60%"
    state: WiomProgressCompletionState,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomProgressMilestones(
    title: String,
    subtitle: String,
    stages: List<WiomMilestoneStage>,                         // size 2..5
    currentStage: Int,                                        // 1-based; must be in 1..stages.size
    showLabels: Boolean = true,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomProgressCircular(
    progress: Float,                                          // 0f..1f
    size: WiomProgressCircularSize = WiomProgressCircularSize.Sm,
    tone: WiomProgressLinearState = WiomProgressLinearState.Brand,
    modifier: Modifier = Modifier,
)                                                             // no centre label: the host shows the %
```

### `WiomProgressLinear` impl

```kotlin
@Composable
fun WiomProgressLinear(
    progress: Float,
    size: WiomProgressLinearSize = WiomProgressLinearSize.Sm,
    tone: WiomProgressLinearState = WiomProgressLinearState.Brand,
    modifier: Modifier = Modifier,
) {
    val fillColor = when (tone) {
        WiomProgressLinearState.Brand    -> Wiom.bg.brand
        WiomProgressLinearState.Info     -> Wiom.bg.info
        WiomProgressLinearState.Positive -> Wiom.bg.positive
    }
    val animatedColor by animateColorAsState(fillColor, tween(300), label = "fill")
    val animatedFraction by animateFloatAsState(
        targetValue = progress.coerceIn(0f, 1f),
        animationSpec = tween(600, easing = FastOutSlowInEasing),
        label = "progress",
    )
    val height = when (size) {
        WiomProgressLinearSize.Sm -> 4.dp
        WiomProgressLinearSize.Md -> 8.dp
    }

    Canvas(
        modifier = modifier
            .fillMaxWidth()
            .height(height)
            .clip(RoundedCornerShape(percent = 50))
            .background(Wiom.bg.muted),
    ) {
        drawRect(
            color = animatedColor,
            topLeft = Offset.Zero,
            size = Size(size.width * animatedFraction, size.height),
        )
    }
}
```

### `WiomProgressCompletion` impl

```kotlin
@Composable
fun WiomProgressCompletion(
    title: String,
    value: String,
    state: WiomProgressCompletionState,
    modifier: Modifier = Modifier,
) {
    val (fillState, pillBg, pillText, pillCopy, fraction) = when (state) {
        WiomProgressCompletionState.JustStarted ->
            ProgressCompletionVisual(WiomProgressLinearState.Info,     Wiom.bg.info.subtle,     Wiom.text.info,     "Just started", 0.20f)
        WiomProgressCompletionState.InProgress ->
            ProgressCompletionVisual(WiomProgressLinearState.Brand,    Wiom.bg.brand.subtle,    Wiom.text.brand,    "In progress",  0.60f)
        WiomProgressCompletionState.AlmostThere ->
            ProgressCompletionVisual(WiomProgressLinearState.Positive, Wiom.bg.positive.subtle, Wiom.text.positive, "Almost there", 0.88f)
        WiomProgressCompletionState.Complete ->
            ProgressCompletionVisual(WiomProgressLinearState.Positive, Wiom.bg.positive.subtle, Wiom.text.positive, "Complete",     1.00f)
    }

    Column(modifier = modifier.fillMaxWidth(), verticalArrangement = Arrangement.spacedBy(Wiom.space.s12)) {
        Row(
            verticalAlignment = Alignment.Top,
            horizontalArrangement = Arrangement.SpaceBetween,
            modifier = Modifier.fillMaxWidth(),
        ) {
            Column(verticalArrangement = Arrangement.spacedBy(Wiom.space.s4), modifier = Modifier.weight(1f)) {
                Text(title, style = Wiom.type.label.md, color = Wiom.text.default)
                StatusPill(label = pillCopy, bg = pillBg, fg = pillText)
            }
            Text(value, style = Wiom.type.body.sm, color = Wiom.text.subtle)
        }
        WiomProgressLinear(progress = fraction, size = WiomProgressLinearSize.Md, tone = fillState)
    }
}

@Composable
private fun StatusPill(label: String, bg: Color, fg: Color) {
    Row(
        modifier = Modifier
            .background(bg, RoundedCornerShape(percent = 50))
            .padding(horizontal = 8.dp, vertical = 2.dp),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(4.dp),
    ) {
        Box(modifier = Modifier.size(6.dp).background(fg, CircleShape))
        Text(label, style = Wiom.type.label.sm, color = fg)
    }
}

private data class ProgressCompletionVisual(
    val fillState: WiomProgressLinearState,
    val pillBg: Color,
    val pillText: Color,
    val pillCopy: String,
    val fraction: Float,
)
```

### `WiomProgressMilestones` impl

```kotlin
@Composable
fun WiomProgressMilestones(
    title: String,
    subtitle: String,
    stages: List<WiomMilestoneStage>,
    currentStage: Int,
    showLabels: Boolean = true,
    modifier: Modifier = Modifier,
) {
    require(stages.size in 2..5) { "Milestones supports 2..5 stages; got ${stages.size}" }
    require(currentStage in 1..stages.size) { "currentStage must be in 1..${stages.size}; got $currentStage" }

    Column(modifier = modifier.fillMaxWidth(), verticalArrangement = Arrangement.spacedBy(20.dp)) {
        Column(verticalArrangement = Arrangement.spacedBy(Wiom.space.s4)) {
            Text(title,    style = Wiom.type.label.lg, color = Wiom.text.default)
            Text(subtitle, style = Wiom.type.body.sm,  color = Wiom.text.subtle)
        }

        // Bar row: stage circles + flex connectors
        Row(
            modifier = Modifier.fillMaxWidth(),
            verticalAlignment = Alignment.CenterVertically,
        ) {
            stages.forEachIndexed { i, stage ->
                val stageNumber = i + 1
                val reached = stageNumber <= currentStage
                StageCircle(stage = stage, reached = reached)
                if (i != stages.lastIndex) {
                    val connectorReached = stageNumber < currentStage
                    Box(
                        modifier = Modifier
                            .weight(1f)
                            .height(4.dp)
                            .background(
                                if (connectorReached) Wiom.bg.brand else Wiom.bg.muted,
                                RoundedCornerShape(percent = 50),
                            ),
                    )
                }
            }
        }

        if (showLabels) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                verticalAlignment = Alignment.CenterVertically,
            ) {
                stages.forEachIndexed { i, stage ->
                    val stageNumber = i + 1
                    val labelColor = when {
                        stageNumber  < currentStage -> Wiom.text.default
                        stageNumber == currentStage -> Wiom.text.brand
                        else                        -> Wiom.text.subtle
                    }
                    // Each label is centred in an equal-width column (as shipped,
                    // WiomProgressIndicator.kt:505-507).
                    Box(modifier = Modifier.weight(1f), contentAlignment = Alignment.Center) {
                        Text(stage.label, style = Wiom.type.body.sm, color = labelColor, textAlign = TextAlign.Center)
                    }
                }
            }
        }
    }
}

@Composable
private fun StageCircle(stage: WiomMilestoneStage, reached: Boolean) {
    val tint = if (reached) Wiom.icon.inverse else Wiom.icon.nonAction

    Box(
        modifier = Modifier
            .size(24.dp)
            .background(if (reached) Wiom.bg.brand else Wiom.bg.muted, CircleShape)
            .padding(4.dp),
        contentAlignment = Alignment.Center,
    ) {
        when (val icon = stage.icon) {
            is WiomMilestoneIcon.Vector ->
                Icon(
                    imageVector = icon.icon,                      // Icons.Rounded.* (preferred)
                    contentDescription = null,
                    tint = tint,
                    modifier = Modifier.size(16.dp),
                )
            is WiomMilestoneIcon.Resource ->
                Icon(
                    painter = painterResource(icon.resId),         // brand drawable fallback
                    contentDescription = null,
                    tint = tint,
                    modifier = Modifier.size(16.dp),
                )
        }
    }
}
```

### `WiomProgressCircular` impl

```kotlin
@Composable
fun WiomProgressCircular(
    progress: Float,
    size: WiomProgressCircularSize = WiomProgressCircularSize.Sm,
    tone: WiomProgressLinearState = WiomProgressLinearState.Brand,
    modifier: Modifier = Modifier,
) {
    val (diameter, strokeDp) = when (size) {
        WiomProgressCircularSize.Sm -> 24.dp to 3.dp
        WiomProgressCircularSize.Lg -> 48.dp to 4.dp
    }
    val fraction = progress.coerceIn(0f, 1f)
    val animatedFraction by animateFloatAsState(
        targetValue = fraction,
        animationSpec = tween(600, easing = FastOutSlowInEasing),
        label = "circular",
    )
    val arcColor = when (tone) {                              // caller-passed; ladderTone(progress) for the ladder
        WiomProgressLinearState.Info     -> Wiom.bg.info
        WiomProgressLinearState.Positive -> Wiom.bg.positive
        WiomProgressLinearState.Brand    -> Wiom.bg.brand
    }
    val animatedColor by animateColorAsState(arcColor, tween(300), label = "arcColor")

    Box(
        modifier = modifier.size(diameter),
        contentAlignment = Alignment.Center,
    ) {
        Canvas(modifier = Modifier.fillMaxSize()) {
            val stroke = strokeDp.toPx()
            val arcSize = Size(size.width - stroke, size.height - stroke)
            val topLeft = Offset(stroke / 2, stroke / 2)
            // Track
            drawArc(
                color = Wiom.bg.muted,
                startAngle = 0f, sweepAngle = 360f, useCenter = false,
                topLeft = topLeft, size = arcSize,
                style = Stroke(width = stroke, cap = StrokeCap.Round),
            )
            // Progress arc — start at 12 o'clock, clockwise
            drawArc(
                color = animatedColor,
                startAngle = -90f, sweepAngle = 360f * animatedFraction, useCenter = false,
                topLeft = topLeft, size = arcSize,
                style = Stroke(width = stroke, cap = StrokeCap.Round),
            )
        }
        // No centre label (as shipped); the host text carries the %.
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material LinearProgressIndicator with raw colour, no state ladder,
//             tap-handler treating progress as a CTA, hand-rolled milestones.
LinearProgressIndicator(
    progress = 0.6f,
    color = Color(0xFFD9008D),                           // wrong — raw hex; no auto-ladder
    modifier = Modifier.clickable { openDetail() },      // banned — progress is display-only
)

Row {                                                     // hand-rolled milestones
    repeat(5) { i ->
        Box(modifier = Modifier.size(24.dp).background(if (i < 3) Color.Magenta else Color.Gray, CircleShape))
        if (i != 4) Spacer(Modifier.weight(1f).height(4.dp).background(Color.Gray))
    }
}

// ✅ RIGHT — Wiom helpers; tap target is the hosting card, not the progress.
WiomListItem(
    primary = "Net box installation",
    onClick = { openInstallationDetail() },              // card owns the tap
) {
    WiomProgressMilestones(
        title = "Net box installation",
        subtitle = "Step 3 of 5 · Router",
        stages = installStages,                           // 5 WiomMilestoneStage
        currentStage = 3,
    )
}
```

---

## 8. Recipes

### File / image upload — Linear

```kotlin
WiomProgressLinear(
    progress = uploadFraction,                            // 0f..1f from upload state
    size = WiomProgressLinearSize.Md,
)
```

### KYC profile completion — Completion card on home

```kotlin
WiomProgressCompletion(
    title = "Profile completion",
    value = "3 of 5 steps",
    state = WiomProgressCompletionState.InProgress,
)
```

### Partner earnings target — Completion card

```kotlin
WiomProgressCompletion(
    title = "Monthly target",
    value = "₹4,500 of ₹10,000",
    state = WiomProgressCompletionState.JustStarted,
)
```

### Net box installation tracker — Milestones (5 stages)

```kotlin
val installStages = remember {
    listOf(
        // Material3 rounded icons — preferred wherever the icon exists in the family.
        WiomMilestoneStage("Survey",   WiomMilestoneIcon.vector(Icons.Rounded.HomeWork)),
        // Brand-specific drawable fallback — for glyphs not in Icons.Rounded.*.
        WiomMilestoneStage("Cable",    WiomMilestoneIcon.resource(R.drawable.ic_cable_rounded)),
        WiomMilestoneStage("Router",   WiomMilestoneIcon.resource(R.drawable.ic_router_rounded)),
        WiomMilestoneStage("Activate", WiomMilestoneIcon.vector(Icons.Rounded.Bolt)),
        WiomMilestoneStage("Done",     WiomMilestoneIcon.vector(Icons.Rounded.CheckCircle)),
    )
}

WiomProgressMilestones(
    title = "Net box installation",
    subtitle = "Step 3 of 5 · Router",
    stages = installStages,
    currentStage = 3,
)
```

### Delivery tracker — Milestones (3 stages, fewer steps)

```kotlin
val deliveryStages = listOf(
    WiomMilestoneStage("Origin",      R.drawable.ic_warehouse_rounded),
    WiomMilestoneStage("In transit",  R.drawable.ic_local_shipping_rounded),
    WiomMilestoneStage("Delivered",   R.drawable.ic_home_rounded),
)

WiomProgressMilestones(
    title = "Order #4521",
    subtitle = "Step 2 of 3 · In transit",
    stages = deliveryStages,
    currentStage = 2,
)
```

### Verification pipeline — Milestones (4 stages)

```kotlin
val kycStages = listOf(
    WiomMilestoneStage("PAN",       R.drawable.ic_credit_card_rounded),
    WiomMilestoneStage("Aadhaar",   R.drawable.ic_badge_rounded),
    WiomMilestoneStage("Photo",     R.drawable.ic_photo_camera_rounded),
    WiomMilestoneStage("Done",      R.drawable.ic_check_circle_rounded),
)

WiomProgressMilestones(
    title = "Identity verification",
    subtitle = "Step 2 of 4 · Aadhaar",
    stages = kycStages,
    currentStage = 2,
)
```

### Dashboard KPI tile — Circular

```kotlin
Card(
    shape = RoundedCornerShape(16.dp),
    colors = CardDefaults.cardColors(containerColor = Wiom.bg.default),
    border = BorderStroke(1.dp, Wiom.stroke.subtle),
    modifier = Modifier.padding(16.dp),
) {
    Row(
        modifier = Modifier.padding(16.dp),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(16.dp),
    ) {
        WiomProgressCircular(progress = 0.75f, size = WiomProgressCircularSize.Lg, tone = ladderTone(0.75f))
        Column {
            Text("Network health",  style = Wiom.type.label.md, color = Wiom.text.default)
            Text("3 of 4 nodes up", style = Wiom.type.body.sm,  color = Wiom.text.subtle)
        }
    }
}
```

### Compact home card → tap → per-step detail

```kotlin
// Home: compact Milestones inside a tappable list-item card.
WiomListItem(
    primary = "Your installation",
    onClick = { nav.openInstallationDetail() },
) {
    WiomProgressMilestones(
        title = "Net box installation",
        subtitle = "Step 3 of 5 · Router",
        stages = installStages,
        currentStage = 3,
    )
}
// Detail screen renders one screen / bottom sheet per step (each with its own
// header, body and CTA) and surfaces position with a Pagination/Bars indicator
// or a compact Milestones strip pinned at the top.
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Material `LinearProgressIndicator` / `CircularProgressIndicator` directly — wrong tokens, no ladder, no completion-card chrome.
- ❌ Painting the progress fill `bg.warning` / `bg.critical` — Wiom progress is forward motion only. Use a status pill *next to* the progress for "stuck" / "needs attention".
- ❌ Adding a tap handler to the progress visual — host card owns the click. `Modifier.clickable` on the bar is a bug.
- ❌ Hand-rolled milestone bar with `Row` + `Box` + `Spacer` — always use `WiomProgressMilestones` so the colour ladder, label colours, and connector logic stay correct.
- ❌ `Stage Count = 5` for a 3-stage flow padded with empty stages — V2 ships `Stage Count = 2 / 3 / 4 / 5`; pick the one that matches.
- ❌ Indeterminate animation on a determinate progress (e.g. infinite shimmer on `Linear`) — use `Loader/Linear` for indeterminate; this skill is determinate.
- ❌ Using `Loader/Linear Determinate` for completion-card progress — that loader is brand-only (no info / positive ladder); use `WiomProgressLinear` here for state-coloured progress.
- ❌ Lingering on a progress visual past completion — when `progress = 1.0f` and the flow is "done", route to the outcome surface (`wiom-bottomsheet` Illustration · Positive) and hide the progress.
- ❌ Stacking two progress visuals on the same card — pick one; doubling them clutters the visual.
- ❌ Rendering a usage / quota meter (data, storage) — Wiom is unlimited; this use case doesn't exist.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Progress canvas: `3069:6031` (Section 1).
Re-pull `get_variable_defs` on each component set if foundations token names ever change.

| Node | Component / variant |
|---|---|
| `2814:14` | Linear component set — 6 ship variants |
| `2814:2 / :4 / :6` | Linear · `sm` × `brand / info / positive` |
| `2814:8 / :10 / :12` | Linear · `md` × `brand / info / positive` |
| `2815:38` | Completion component set — 4 ship variants |
| `2815:2 / :11 / :20 / :29` | Completion · `Just Started` / `In Progress` / `Almost There` / `Complete` |
| `3108:496` | Milestones component set — 14 ship variants |
| `3108:62 / :80` | Milestones · `Stage Count = 2` × Current `1 / 2` |
| `3108:98 / :123 / :148` | Milestones · `Stage Count = 3` × Current `1 / 2 / 3` |
| `3108:173 / :205 / :237 / :269` | Milestones · `Stage Count = 4` × Current `1 / 2 / 3 / 4` |
| `3108:301 / :340 / :379 / :418 / :457` | Milestones · `Stage Count = 5` × Current `1 / 2 / 3 / 4 / 5` |
| `3109:110` | Circular component set — 5 ship variants |
| `3109:91 / :94 / :98 / :102 / :106` | Circular · `Progress = 0 / 25 / 50 / 75 / 100` |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The ship has **4 component sets · 29 ship variants total** (Linear 6 + Completion 4 + Milestones 14 + Circular 5). Older skill drafts described 4 components but counted Milestones as 5 fixed variants; V2 ships 14 (Stage Count 2/3/4/5 × Current Stage). The old `Progress/Icon Bar` set (`2879:132`) is **not in V2** — it was replaced by `Progress/Circular` (`3109:110`).
- **Milestones supports 2 / 3 / 4 / 5 stages.** Older drafts said "Fixed at 5 stages — 3-stage / 7-stage flows need a parallel component." V2 resolves this with the `Stage Count` variant. 7+ stages still aren't supported (and shouldn't be — at that point build per-step screens with one screen / bottom sheet per step).
- **Bar geometry is fluid (flexbox), not fixed pixel positions.** Older drafts described stage circles at fixed x = 12 / 86 / 160 / 234 / 308 with labels positioned via `iconCenter − labelWidth / 2`. V2 ships the circles + connectors as a flex Row, so 2 / 3 / 4 / 5-stage layouts compose without manual repositioning.
- **Stage labels and stage icons are NOT INSTANCE_SWAP / TEXT properties** — they're direct layers, edited per instance in Figma. Same with Completion's title / value / pill copy. This is a Figma platform constraint (TEXT properties have a single shared default across variants), not a Wiom design choice. The Compose API works around it with explicit `title` / `subtitle` / `stages` parameters.
- **Reversed semantics — never `bg.warning` / `bg.critical` on the fill.** Wiom progress represents forward motion toward a desirable outcome. Stuck / blocked / errored flows route to a `wiom-bottomsheet` Illustration outcome surface or a status pill *adjacent* to the progress, never colour the bar amber / red.
- **No stuck / blocked / error variant.** Older skill drafts proposed a `State = Blocked` variant on Completion / Milestones — V2 didn't add it. Outcome handling lives in `wiom-bottomsheet`; the progress visual only signals forward motion.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.muted` (track), `bg.info` / `bg.brand` / `bg.positive` (fill ladder), `bg.info.subtle` / `bg.brand.subtle` / `bg.positive.subtle` (pill bg), `text.info` / `text.brand` / `text.positive` / `text.default` / `text.subtle`, `icon.inverse` / `icon.nonAction`, `radius-full`, `space-4` / `space-12`, `type.label.lg` / `type.label.md` / `type.label.sm` / `type.body.sm`.
- `wiom-loader` — `Loader/Linear` (Indeterminate / Determinate, brand-only) for short waits with no progress signal; `Loader/Brand` Overlay for blocking critical ops; `Loader/Spinner` for inline waits. This skill is for state-coloured determinate completion progress; loader is for indeterminate / branded waits.
- `wiom-list-item` — host for completion-card rows that link to detail screens (the row owns the tap; the progress visual is display-only).
- `wiom-bottomsheet` — outcome surface that takes over from a progress visual at completion (Illustration · Positive for success, Critical for failure).
- `wiom-pagination` — for unlabeled "how far am I" wizard dots / position-on-known-list patterns. Not the same as named-stage progress.
- `wiom-cta` — the action next to a progress visual when the user can act ("Resume", "Continue", "View detail"). The progress itself is never the CTA.
- `wiom-compose-layout-rules` — Modifier discipline; `animateFloatAsState` / `animateColorAsState` for caller-driven transitions; no infinite animation on determinate progress.
