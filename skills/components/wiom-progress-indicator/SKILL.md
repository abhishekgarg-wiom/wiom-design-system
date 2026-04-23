---
name: wiom-progress-indicator
description: Wiom Progress Indicator component spec, usage patterns, and Figma node IDs. Use this skill EVERY TIME you work with progress bars, completion meters, installation trackers, milestone indicators, or any Figma design involving determinate progress from the Wiom DS file. Trigger when user mentions "progress", "progress indicator", "progress bar", "completion meter", "milestone", "tracker", "installation progress", "KYC progress", "profile completion", "onboarding progress", or when designing flows where the system advances progress (upload, install, background task). Also trigger when user is unsure whether to use a Stepper vs a Progress Indicator.
---

# Wiom Progress Indicator — Component Skill

4 progress component sets covering determinate progress in the Wiom app — plain bars, rich completion meters, named-stage trackers, and icon-on-bar trackers. All reversed-semantics (more = better, greener) since Wiom gives unlimited data and has no "usage meter" use case.

## Where it lives

- **Figma file:** Wiom DS - Next (fileKey `ZPErh7YU7omdbyy2o268Qo`)
- **Page:** `Saurav / Progress Indicator` (`2792:1384`) — **ALL Progress Indicator changes happen on this page. Never on any other page.**

> **Rule (hard):** When asked to modify, audit, or extend any Progress Indicator, always navigate to `Saurav / Progress Indicator` page first (`await figma.setCurrentPageAsync(...)`). Do not create any Progress variant or copy on any other page.

---

## Progress Indicator vs Stepper — when to use which

Both live in Wiom DS. They overlap visually (dots on a line, advancing left→right) but differ on information density, layout, and per-step interactivity.

**Stepper** — see [wiom-stepper](../wiom-stepper/SKILL.md)
- `Stepper / Horizontal` — row of 2–5 indicators with short labels under each. 360 dp wide. For top-of-screen wizard nav.
- `Stepper / Vertical` — column where each step has title + description + optional **Action slot** (CTA, Tertiary, Input, OTP, any Wiom component). For status flows (KYC, installation, refund, order tracking).
- `StepIndicator` atom — shared 32×32 circle with 4 states (Completed / Active / Upcoming / Error).

**Progress Indicator** — this skill
- Compact bar with % fill, optional icon markers, optional status pill / title / value numbers.
- Same status flows the vertical stepper covers, but visualized as a horizontal bar rather than a scrollable column — no per-step description or action slot.

**Pick between them:**

| Need | Use |
|---|---|
| Each step has a description or action (CTA, input, OTP) | **Stepper / Vertical** |
| Compact wizard nav at top of screen with short labels | **Stepper / Horizontal** |
| Compact horizontal bar showing stage progress, no per-step content | **Progress/Milestones** (this skill) |
| % bar only, no named stages | **Progress/Linear** or **Progress/Completion** |
| Single moving marker on a bar (delivery tracker, download head) | **Progress/Icon Bar** |
| Unlabeled "how far am I" progress | `Pagination / Bars` (see wiom-pagination) |
| Indeterminate "something is happening" | `Loader/Spinner` or `Loader/Status` (see wiom-loader) |

**Overlap cases.** Installation tracking, KYC, onboarding can use either component family. Guideline:

- **Stepper / Vertical** when each step needs a title + description + maybe an action the user takes.
- **Progress/Milestones** when space is tight and you only need the "N of 5" horizontal bar visualization — e.g. a home-screen summary card that links to the full Stepper / Vertical on a detail screen.
- These two often stack: a compact **Progress/Milestones** card on home → tapping opens a **Stepper / Vertical** with full per-step detail.

---

## Design philosophy — reversed semantics

**More progress = better = greener.** This is the opposite of typical usage meters (data, storage) where full = bad. Since Wiom's ISP product has unlimited data, we never render a usage meter — every progress bar in the app represents forward motion toward something desirable.

**Color ladder (thresholds):**

| Range | Fill color | Status mood |
|---|---|---|
| `<35%` | `bg/info` (purple) | Just started — neutral, not concerning |
| `35–80%` | `bg/brand` (pink) | In progress — active, brand-forward |
| `>80%` | `bg/positive` (green) | Almost there / complete — success |

**Never use warning/critical colors** on progress fill (those are for *Attention* states, see §5 — not yet built).

---

## 1. Progress/Linear — Plain bar

**Set ID:** `2814:14` · 6 variants

### Variant API

| Property | Type | Options |
|---|---|---|
| `Size` | **Variant** | `sm` (4px) · `md` (8px) |
| `State` | **Variant** | `brand` · `info` · `positive` |

### Anatomy

```
┌──────────────────────────────────────────────────┐
│████████████████████                              │   Track (full width, bg/muted)
└──────────────────────────────────────────────────┘   Fill (60%, state color) overlays
   328 wide · 4 or 8 tall · radius/full
```

| Size | Height | Use |
|---|---|---|
| `sm` | 4px | Inline, compact cards, supplementary info |
| `md` | 8px | Default. Standalone rows, prominent indicators |

### Tokens

| Element | Token |
|---|---|
| Track | `bg/muted` |
| Fill (brand) | `bg/brand` |
| Fill (info) | `bg/info` |
| Fill (positive) | `bg/positive` |
| Radius (track + fill) | `radius/full` |

### Limitation

All 6 variants render at **60% fill** as a preview — Figma doesn't support numeric component properties, so arbitrary percentages can't be set via the Properties panel. Designers resize the inner `Fill` rectangle manually at instance level. For code, the width is driven by `progress: Float` in Compose.

### Usage

Use when you need a plain determinate bar with no surrounding chrome — file upload, payment processing (determinate), inline completion % next to other content.

---

## 2. Progress/Completion — Rich completion meter

**Set ID:** `2815:38` · 4 variants

Compose of header (title + status pill + value numbers) and bar. State drives the color ladder.

### Variant API

| Property | Type | Options |
|---|---|---|
| `State` | **Variant** | `Just Started` (20%) · `In Progress` (60%) · `Almost There` (88%) · `Complete` (100%) |

### Anatomy

```
┌─────────────────────────────────────────────────────────────────┐
│ Net box installation                                3 of 5 steps│   Header
│ ● In progress                                                   │
│ ────────────────────────────────────────                        │   Bar (md, filled to state %)
└─────────────────────────────────────────────────────────────────┘
```

Top-left stack: Title (`type/label/md`) + Status Pill (`type/meta/xs` on subtle-bg pill)
Top-right: Value numbers (`type/body/sm`)
Below: Bar at the variant's %

### State → fill / pill mapping

| State | Fill % | Fill color | Pill bg | Pill text | Pill label |
|---|---|---|---|---|---|
| `Just Started` | 20% | `bg/info` | `bg/info/subtle` | `text/info` | `● Just started` |
| `In Progress` | 60% | `bg/brand` | `bg/brand/subtle` | `text/brand` | `● In progress` |
| `Almost There` | 88% | `bg/positive` | `bg/positive/subtle` | `text/positive` | `● Almost there` |
| `Complete` | 100% | `bg/positive` | `bg/positive/subtle` | `text/positive` | `● Complete` |

### Text styles

| Element | Style |
|---|---|
| Title | `type/label/md` |
| Status pill | `type/meta/xs` |
| Value numbers | `type/body/sm` |

### Usage

Use for rich progress cards where the user sees all three pieces of info at once — what's happening, how far along, and numerically. Good for:

- KYC completion card on home
- Net box installation tracker (without stage names)
- Partner earnings target card
- Referral milestone card
- Profile completion card

### Limitation

Title, value, and pill label are hardcoded per variant — no component-property overrides, because Figma TEXT properties share a single default across all variants (see §6 — Known gaps). Designers can override text on the instance directly.

---

## 3. Progress/Milestones — Named-stage tracker

**Set ID:** `2862:177` · 5 variants · 2 component properties

### Variant API

| Property | Type | Options |
|---|---|---|
| `Current Stage` | **Variant** | `1` · `2` · `3` · `4` · `5` |
| `Has Labels` | **Boolean** | `true` (default) · `false` |
| `Title` | **TEXT** | default: `"Net box installation"` |

### Anatomy

```
┌─────────────────────────────────────────────────────────────────┐
│ Net box installation                                            │   Title (type/label/lg)
│ Step 3 of 5 · Router                                            │   Subtitle (type/body/sm)
│                                                                 │
│ ● ─── ● ─── ● ─── ○ ─── ○                                        │   Bar + 5 icons (24px circles, 16px icons)
│ Survey  Cable  Router  Activate  Done                           │   Stage labels (type/meta/xs) — toggleable via Has Labels
└─────────────────────────────────────────────────────────────────┘
```

**Bar:** 328 wide, 4px tall, centered vertically in 24-tall frame. Track `bg/muted`, fill `bg/brand` extending from x=12 to center of current stage icon.

**Icons:** 5 fixed positions at x = 0, 74, 148, 222, 296 (left edges) → centers at 12, 86, 160, 234, 308. 24×24 circle containing 16×16 icon.

- **Reached stages** (stage ≤ current): circle `bg/brand`, icon `icon/inverse` (white)
- **Future stages** (stage > current): circle `bg/muted`, icon `icon/nonAction`

**Labels (when `Has Labels = true`):**

- **Past:** `text/default`
- **Current:** `text/brand` (pink) — the visual emphasis for current stage
- **Future:** `text/subtle`

Labels are center-aligned under each icon. Positions computed as `iconCenter - labelWidth / 2` (labels auto-hug text width, so the position is dynamic).

### Default icons for net box install (can be swapped per use case)

| Stage | Default icon | Node ID |
|---|---|---|
| 1 | `home_work` | `1232:3079` |
| 2 | `cable` | `1232:1083` |
| 3 | `router` | `1232:5141` |
| 4 | `bolt` | `1232:907` |
| 5 | `check_circle` | `1232:1263` |

### Subtitle text — per variant (not a component property)

Subtitle is hardcoded per variant (`"Step N of 5 · {Stage name}"`). **Attempted but reverted:** making Subtitle a TEXT component property caused all variants to share the same default, losing per-stage context. Now each variant keeps its own subtitle; designers edit the subtitle layer directly on the instance if they need to customize.

### Usage

Use for step-by-step processes where named stages matter AND the user isn't steering:

- Partner installing net box (customer view, 5 stages shown)
- Onboarding wizard progress (observed, not driven)
- Verification pipeline (PAN → Aadhaar → Photo → Signature → Done)
- Delivery tracking (Ordered → Shipped → In Transit → Out for Delivery → Delivered)

### Limitations

- **Stage labels are hardcoded** per variant — "Survey/Cable/Router/Activate/Done". For another flow (e.g. KYC: "PAN/Aadhaar/Photo/Signature/Review"), designer overrides each label manually at instance level.
- **Icons are hardcoded** as direct instances (not INSTANCE_SWAP properties). Designer right-clicks → Swap Instance to change each.
- **Fixed at 5 stages.** 3-stage or 7-stage flows need a parallel component (not yet built).
- **No blocked/error state.** If installation is stuck, there's no "Attention needed" variant.

---

## 4. Progress/Icon Bar — Icon-on-bar tracker (no labels)

**Set ID:** `2879:132` · 5 variants · 5 component properties

### Variant API

| Property | Type | Options |
|---|---|---|
| `Progress` | **Variant** | `0` · `25` · `50` · `75` · `100` (% complete) |
| `Show Icon 1` | **Boolean** | `true` (default) |
| `Show Icon 2` | **Boolean** | `true` (default) |
| `Show Icon 3` | **Boolean** | `true` (default) |
| `Show Icon 4` | **Boolean** | `true` (default) |
| `Show Icon 5` | **Boolean** | `true` (default) |

### Anatomy

```
┌─────────────────────────────────────────────────────────────────┐
│ ● ─── ● ─── ● ─── ○ ─── ○                                        │   Bar (md) + up to 5 toggleable icons
└─────────────────────────────────────────────────────────────────┘
   Same geometry as Milestones — icons at centers 12, 86, 160, 234, 308
```

**Icon auto-color (by reached/unreached):** Each icon's bg color is set in the variant based on whether `iconCenter <= fill edge`:

- Reached: circle `bg/brand`, icon `icon/inverse`
- Unreached: circle `bg/muted`, icon `icon/nonAction`

### Fill edge per variant

| Progress | Fill extends to x= |
|---|---|
| 0% | 0 (fill hidden) |
| 25% | 86 (center of icon 2) |
| 50% | 160 (center of icon 3) |
| 75% | 234 (center of icon 4) |
| 100% | 308 (center of icon 5) |

### Toggle pattern

All 5 booleans default to `true` — so a fresh instance shows all 5 icons. **Designer disables the ones they don't need.** This is the opposite UX of the ideal "enable what you need" pattern, but Figma's boolean component properties have a single default value (can't vary per-variant), so we can't default to showing only the current-stage icon.

### Usage

Use when icons represent progress points on a bar but don't need names (unlike Milestones):

- Delivery tracker — enable 1, 3, 5 → Origin / In-transit / Destination
- Two-point journey — enable only 1, 5 → Start / End
- All-points tracker — leave all 5 on → full milestone path
- Single moving marker — enable only the icon at the current % → just the "head" of progress

### Default icon

All 5 slots default to `router` (`1232:5141`) — a placeholder fitting the net-box-install context. Designers right-click → Swap Instance for each slot to change.

### Limitations

- **Fixed at 5 positions** — can't drop an icon at 37%, snaps to 0/25/50/75/100% gridline.
- **No INSTANCE_SWAP properties yet** — icon swap is manual per slot.
- **Default all-on is imperfect UX** — see §6.

---

## Picking the right progress component

| Scenario | Component |
|---|---|
| Plain % bar, no chrome | **Progress/Linear** |
| % bar + title + value + status pill | **Progress/Completion** |
| Multi-stage compact horizontal bar with named labels | **Progress/Milestones** |
| Multi-stage compact horizontal bar, icons only, no labels | **Progress/Icon Bar** |
| Single moving marker | **Progress/Icon Bar** (enable only 1 icon) |
| Per-step detail + description + action slot | **Stepper / Vertical** (see wiom-stepper) |
| Top-of-screen wizard nav with short labels | **Stepper / Horizontal** (see wiom-stepper) |
| Indeterminate loading ("something is happening") | **Loader/Spinner** or **Loader/Status** (see wiom-loader) |

---

## 5. Tokens & text styles used

### Color tokens

| Purpose | Token |
|---|---|
| Track background | `bg/muted` |
| Fill — just started | `bg/info` |
| Fill — in progress | `bg/brand` |
| Fill — almost there / complete | `bg/positive` |
| Pill bg (subtle) | `bg/info/subtle` · `bg/brand/subtle` · `bg/positive/subtle` |
| Pill text | `text/info` · `text/brand` · `text/positive` |
| Icon (reached) | `icon/inverse` |
| Icon (unreached) | `icon/nonAction` |
| Circle bg (reached) | `bg/brand` |
| Circle bg (unreached) | `bg/muted` |
| Label — past | `text/default` |
| Label — current | `text/brand` |
| Label — future | `text/subtle` |

### Spacing, radius, stroke

| Purpose | Token |
|---|---|
| Bar height (md) | 8px |
| Bar height (sm) | 4px |
| Icon container | 24×24 |
| Icon glyph | 16×16 (`icon/xs`) |
| Bar / icon container radius | `radius/full` |
| Stage spacing (5 stages across 328) | 74px between left edges |

### Text styles

| Element | Style |
|---|---|
| Completion title | `type/label/md` |
| Milestones title | `type/label/lg` |
| Subtitle | `type/body/sm` |
| Value numbers | `type/body/sm` |
| Status pill label | `type/meta/xs` |
| Stage labels | `type/meta/xs` |

---

## 6. Known gaps & v1.1 roadmap

### API gaps

1. **No numeric percentage property.** Figma supports TEXT / BOOLEAN / INSTANCE_SWAP / VARIANT properties only — no NUMBER. Fill width is locked to preset variants (Linear: 60% fixed; Completion: 20/60/88/100; Icon Bar: 0/25/50/75/100). Designers manually resize the inner `Fill` rect for custom %.
2. **TEXT properties clobber per-variant defaults.** Figma's TEXT component property has a single shared default across all variants. Tried binding Subtitle in Milestones — every variant ended up showing the same subtitle. Workaround: keep subtitle as a direct layer, designer edits per-instance.
3. **BOOLEAN properties have the same single-default issue.** Icon Bar's "Show Icon N" booleans all default to true, can't have per-variant defaults (e.g. "default to showing only the icon at the current progress %").

### Missing variants / states

4. **No blocked / attention state.** No "Installation stuck" or "Verification failed" variant. `bg/warning` and `bg/critical` tokens exist but unused. Proposed: add `State=Blocked` variant to Completion + Milestones.
5. **No minimum visible sliver at 0%.** Material does the same (empty track) but may feel like nothing happened. Open question.
6. **Only `md` size on Completion / Milestones / Icon Bar.** Only Linear has sm + md.

### INSTANCE_SWAP / flexibility gaps

7. **Icons not swappable via Properties panel** in Milestones or Icon Bar. Designer right-clicks → Swap Instance per slot. Adding INSTANCE_SWAP properties is v1.1 work.
8. **Stage labels and icons are fixed to the net-box-install context.** Reusing for KYC, onboarding, etc. requires manual per-instance overrides.
9. **Fixed at 5 stages.** 3-stage or 7-stage flows need parallel components.

### Component relationships

10. **Overlap with `Loader/Linear Determinate`** — unresolved. Two components do similar work. Should deprecate one or document the split clearly. Recommendation: deprecate Loader/Linear Determinate, keep Loader/Linear as indeterminate-only; all determinate lives here.
11. **Relationship with Stepper** — documented in §Stepper vs Progress above, but no cross-links in Figma yet. Stepper uses should reference this skill's "When NOT to use" section.

### Missing artifacts

12. **Figma component descriptions are empty** on all 4 sets. Design-to-code tools pull from there — populate in v1.1.
13. **Kotlin / Compose mapping not documented.** Compose has `LinearProgressIndicator` with customizable colors. Milestones/Icon Bar need custom layout (5 positioned `Box` with `Canvas` bar underneath).
14. **Animation spec not captured.** HTML prototype used 600ms cubic-bezier for fill width + 300ms ease for color cross-fade. Not captured in Figma (static) or in docs.
15. **Not published as a library.** Until published, instances can't swap variants from the Properties panel.

---

## Use-case matrix (for Wiom app)

| Screen / flow | Component | Why |
|---|---|---|
| Partner installing net box — detail screen, per-step descriptions/actions | `Stepper / Vertical` (see wiom-stepper) | Each step needs title + description + possibly action |
| Partner installing net box — home summary card | `Progress/Milestones` | Compact "Step 3 of 5" visualization, links to Stepper Vertical |
| Partner installing net box — compact icon strip, no labels | `Progress/Icon Bar` (enable all 5) | Minimal card, icon-only |
| KYC profile completion card on home | `Progress/Completion` | % with rich title + value + status pill |
| KYC form itself (PAN → Aadhaar → Photo → Signature) | `Stepper / Vertical` (see wiom-stepper) | Per-step inputs and actions |
| File upload (KYC document) | `Progress/Linear` | Plain bar, fast-moving % |
| Monthly referral target | `Progress/Completion` | %-driven with value "2 of 5 friends" |
| Partner earnings target | `Progress/Completion` | Value "₹4,500 of ₹10,000" |
| Delivery tracker — with per-step details | `Stepper / Vertical` (see wiom-stepper) | Reading content per step |
| Delivery tracker — compact map overlay / card | `Progress/Icon Bar` (enable 1, 3, 5) | Origin / In-transit / Destination, minimal |
| Multi-step signup wizard | `Stepper / Horizontal` (see wiom-stepper) | Top-of-screen nav, short labels |
| Payment processing | `Loader/Status` (see wiom-loader) | Indeterminate Wait → Success/Error |

---

## When to read this skill

- Before any work on progress bars, completion meters, installation/delivery trackers, or milestone indicators in the Wiom app
- Before generating Compose code from any Figma design that uses a progress component
- When a designer asks whether to use a Stepper or a Progress Indicator
- When building a new "completion card" on the home screen or dashboard
- Before touching any component on the `Saurav / Progress Indicator` page

## When NOT to use this skill

- Per-step content + actions (KYC form, detailed status tracking) → load [`wiom-stepper`](../wiom-stepper/SKILL.md) instead
- Top-of-screen wizard nav → load [`wiom-stepper`](../wiom-stepper/SKILL.md) (Horizontal layout)
- Indeterminate loading states → load [`wiom-loader`](../wiom-loader/SKILL.md) instead
- Unlabeled "how far am I" wizard dots → load `wiom-pagination`
- Data usage meters → Wiom is unlimited, this use case doesn't exist
- Plan validity countdowns → use text + colored tag, not a progress bar

---

## See also

- `wiom-design-foundations` — status color tokens, `bg.muted` track, `radius-full`, `bg.brand` / `bg.positive` / `bg.info` fill tokens
- `wiom-compose-layout-rules` — Compose structural rules
- `wiom-loader` — sibling component for indeterminate / full-screen waits (spinner, status ring, skeleton, dots, brand)
- `wiom-stepper` — sibling component when progress has labeled steps
