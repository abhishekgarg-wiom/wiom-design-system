# Switch — Figma Reference Page Build

## Page purpose

This is the build-ready blueprint for the `Components / Switch` reference page in the `Wiom DS - Next` Figma file.

It documents every section that should appear on the Switch documentation page, in exact order, with layout specs, visual examples, and content — so the page can be built in Figma with zero ambiguity.

This is a **reference/documentation page only**. It does not replace or duplicate the component build spec (`figma/components/switch-build.md`). The component build spec defines what to build. This file defines what the documentation page looks like.

Sources:
- `figma/components/switch-build.md`
- `figma/components/checkbox-page-build.md` (layout pattern reference)
- `figma/foundations/01-foundations-color-build.md` (layout pattern reference)

---

## Page frame setup

| Property | Value |
|---|---|
| Frame width | 1440 px |
| Frame height | Hug contents |
| Background | `color/surface/base` (#FAF9FC) |
| Auto-layout | Vertical, top-to-bottom |
| Left/right padding | 64 px |
| Top padding | 48 px |
| Bottom padding | 64 px |

---

## Section order

Build the page top-to-bottom in this exact order:

| # | Section | Purpose |
|---|---|---|
| 1 | Page header | Title, purpose, status badge |
| 2 | Where to use / Where not to use | Usage guidance |
| 3 | Anatomy | Labeled diagram of switch parts |
| 4 | Selection states | Off and On visual examples |
| 5 | Interaction states | Default and Disabled visual examples |
| 6 | State matrix | Full grid: 2 selection x 2 interaction states |
| 7 | Color mapping | Token-to-value tables for track, thumb, label, helper text |
| 8 | Typography mapping | Token table for all text elements |
| 9 | Spacing mapping | Annotated spacing diagram and table |
| 10 | Radius, stroke, and shadow | Foundation token tables |
| 11 | Helper text | Examples with and without helper text |
| 12 | Figma component structure | Layer tree and properties |
| 13 | Design decisions | Rationale for key choices |
| 14 | V1 exclusions | What is not built |

---

## Page typography conventions

These apply to the page's own text elements (headings, body copy, annotations), not to the switch component instances shown on the page.

| Element | Font | Size | Weight | Line height | Color |
|---|---|---|---|---|---|
| Page title | Noto Sans | 32 px | Bold (700) | 48 px | `color/text/primary` #161021 |
| Page purpose text | Noto Sans | 16 px | Regular (400) | 24 px | `color/text/secondary` #352D42 |
| Section heading | Noto Sans | 20 px | SemiBold (600) | 28 px | `color/text/primary` #161021 |
| Subsection heading | Noto Sans | 16 px | SemiBold (600) | 24 px | `color/text/primary` #161021 |
| Body text / notes | Noto Sans | 14 px | Regular (400) | 20 px | `color/text/secondary` #352D42 |
| Table header | Noto Sans | 12 px | SemiBold (600) | 16 px | `color/text/primary` #161021 |
| Table cell | Noto Sans | 12 px | Regular (400) | 16 px | `color/text/secondary` #352D42 |
| Annotation / caption | Noto Sans | 12 px | Regular (400) | 16 px | `color/text/tertiary` #665E75 |
| Token name (in tables) | Noto Sans | 12 px | Regular (400) | 16 px | `color/text/tertiary` #665E75 |

---

## Page spacing conventions

| Element | Value | Token |
|---|---|---|
| Page header to first section | 48 px | `space/48` |
| Between sections | 48 px | `space/48` |
| Section heading to content | 16 px | `space/16` |
| Subsection heading to content | 12 px | `space/12` |
| Between subsections within a section | 24 px | `space/24` |
| Gap between visual examples in a row | 24 px | `space/24` |
| Gap between annotation label and visual element | 8 px | `space/8` |
| Table row height | 32 px minimum | — |

---

## Section 1 — Page header

### Content

- **Title:** `Switch`
- **Purpose text:** `A binary on/off toggle for instant-apply settings and preferences. Supports Off and On selection states with Default and Disabled interaction states.`
- **Status badge:** `V1` (simple text label, `type/label/sm`, `color/brand/primary` on `color/brand/primary-tint` background, `radius/sm` corner, 4 px horizontal padding, 2 px vertical padding)

### Layout

```
Title text                    [V1 badge]
Purpose text (max width 720 px)
```

- Title and badge on the same horizontal line, badge right-aligned or after the title with 12 px gap.
- Purpose text below, left-aligned, max width 720 px.

---

## Section 2 — Where to use / Where not to use

### Layout

Two columns side by side within a single section row.

```
┌─ Do use Switch ─────────────┐  ┌─ Don't use Switch ──────────┐
│  • Settings toggles          │  │  • Form options → Checkbox   │
│  • Feature enable/disable    │  │  • Mutually exclusive → Radio│
│  • Privacy toggles           │  │  • Multi-select → Checkbox   │
│  • Instant preferences       │  │  • Dropdown → Select         │
│  • Partner feature flags     │  │  • Actions needing confirm   │
└──────────────────────────────┘  └──────────────────────────────┘
```

### Specs

| Element | Spec |
|---|---|
| Column container | Auto-layout vertical, hug contents |
| Column width | Fill, 50% each with 24 px gap between columns |
| Column heading "Do use Switch" | Subsection heading style, `color/positive/primary` #008043 |
| Column heading "Don't use Switch" | Subsection heading style, `color/negative/primary` #D92130 |
| List items | Body text style (14 px, Regular), `color/text/secondary` |
| Bullet | 4 px circle, `color/text/tertiary`, 8 px gap to text |
| Don't use: redirect text | Annotation style (12 px), e.g. "use Checkbox" in `color/text/tertiary` after each item |

### Content

**Do use Switch:**
- Settings toggles (notifications, dark mode)
- Feature enable/disable
- Privacy and permission toggles
- Instant-apply preferences
- Partner dashboard feature flags

**Don't use Switch:**
- Form options requiring submit — use Checkbox
- Mutually exclusive choice — use Radio
- Multiple selections from a list — use Checkbox
- Dropdown selection — use Select
- Actions that need confirmation before applying

---

## Section 3 — Anatomy

### Layout

A single annotated diagram showing the switch in On state with helper text visible, with callout lines pointing to each part.

```
                   ┌── 1. Track (52×32px pill)
                   │
                   │    ┌── 2. Thumb (24×24px circle)
                   │    │
                   v    v
                ┌──────────────┐
                │         ┌──┐ │   Label text  ◄── 3. Label
                │         │  │ │
                │         └──┘ │   Helper text  ◄── 4. Helper text
                └──────────────┘
```

### Specs

| Element | Spec |
|---|---|
| Switch instance | Use the actual Switch component: Selection=On, State=Default, Show Helper Text=true |
| Instance scale | 1x (actual size, not enlarged) |
| Callout lines | 1 px stroke, `color/border/default` (#D7D3E0) |
| Callout numbers | 12 px, SemiBold, `color/text/primary`, inside a 20 px circle with `color/surface/subtle` fill |
| Callout labels | 12 px, Regular, `color/text/secondary` |
| Spacing between switch instance and callout zone | 24 px |

### Callout list

| # | Element | Required | Note |
|---|---|---|---|
| 1 | Track | Yes | 52 x 32 px pill container (radius-full) |
| 2 | Thumb | Yes | 24 x 24 px circle, 4 px inset from track edge |
| 3 | Label | Yes | Required. Describes what the toggle controls. |
| 4 | Helper text | Optional | Additional context below label. |

---

## Section 4 — Selection states

### Layout

Two switch instances side by side, each labeled below with the selection state name.

```
  [○────]  Label        [────●]  Label
  Off                    On
```

### Specs

| Element | Spec |
|---|---|
| Row layout | Auto-layout horizontal, gap 48 px between examples |
| Switch instances | Actual component instances, State=Default, Show Helper Text=false |
| Instance 1 | Selection=Off, Label="Label" |
| Instance 2 | Selection=On, Label="Label" |
| State name below | Annotation style (12 px, Regular, `color/text/tertiary`), 8 px below the instance |

---

## Section 5 — Interaction states

### Layout

Two switch instances in a row, each showing one interaction state in the Off selection state.

```
  [○────]  Label     [○────]  Label
  Default              Disabled
```

### Specs

| Element | Spec |
|---|---|
| Row layout | Auto-layout horizontal, gap 48 px |
| Switch instances | Selection=Off, Show Helper Text=false |
| Instance 1 | State=Default |
| Instance 2 | State=Disabled |
| State name below | Annotation style, 8 px below |

---

## Section 6 — State matrix

### Layout

A grid showing all 4 combinations: 2 selection states (rows) x 2 interaction states (columns).

```
                  Default    Disabled
Off               [inst]     [inst]
On                [inst]     [inst]
```

### Specs

| Element | Spec |
|---|---|
| Grid container | Auto-layout vertical for rows |
| Row | Auto-layout horizontal |
| Column header row | Annotation style (12 px, SemiBold, `color/text/tertiary`) |
| Row label column | Annotation style (12 px, SemiBold, `color/text/tertiary`), width 80 px, right-aligned |
| Cell gap (horizontal) | 48 px |
| Row gap (vertical) | 24 px |
| Column header to first row gap | 16 px |
| Switch instances | All 4 variants, Show Helper Text=false, Label="Label" |

### Instance mapping

| Row | Col 1: Default | Col 2: Disabled |
|---|---|---|
| **Off** | Off/Default | Off/Disabled |
| **On** | On/Default | On/Disabled |

### Note below the matrix

- "2 selection states × 2 interaction states. This is the complete variant set for V1."
- "Hover and Focused states excluded — mobile-first product. No Error state — switches are binary toggles with no form validation."
- Annotation style, `color/text/tertiary`.

---

## Section 7 — Color mapping

### Layout

Four subsection tables stacked vertically:
1. Off track colors
2. On track colors
3. Thumb colors
4. Label and helper text colors

### Subsection 7a — Off track colors

**Subsection heading:** "Off Track"

| Element | Default | Disabled |
|---|---|---|
| Fill | `color/surface/muted` #E8E4F0 | `color/surface/subtle` #F1EDF7 |
| Stroke | `color/border/strong` #665E75 | `color/border/strong` at 40% opacity |
| Stroke width | 2 px (`stroke/2`) | 2 px (`stroke/2`) |

### Subsection 7b — On track colors

**Subsection heading:** "On Track"

| Element | Default | Disabled |
|---|---|---|
| Fill | `color/brand/primary` #D9008D | `color/brand/primary` at 40% opacity |
| Stroke | None (filled = no border) | None |
| Opacity | 1.0 | 0.4 (on track frame) |

### Subsection 7c — Thumb colors

**Subsection heading:** "Thumb"

| Element | Default | Disabled |
|---|---|---|
| Fill | White #FFFFFF | White #FFFFFF |
| Size | 24 x 24 px | 24 x 24 px |
| Position (Off) | 4 px from left | 4 px from left |
| Position (On) | 4 px from right | 4 px from right |

### Subsection 7d — Label and helper text colors

**Subsection heading:** "Label & Helper Text"

| Element | Default | Disabled |
|---|---|---|
| Label | `color/text/primary` #161021 | `color/text/disabled` #A7A1B2 |
| Helper text | `color/text/secondary` #352D42 | `color/text/disabled` #A7A1B2 |

### Table presentation in Figma

- Build tables as auto-layout frames (not Figma's table widget).
- Header row: `color/surface/subtle` (#F1EDF7) background, table header text style.
- Body rows: alternating transparent and `color/surface/subtle` at 50% opacity for readability.
- Cell padding: 8 px horizontal, 4 px vertical.
- Column width: hug contents, minimum 100 px.
- Borders between cells: 1 px `color/border/subtle` (#E8E4F0).

---

## Section 8 — Typography mapping

### Content

| Element | Token | Font | Size / Line height | Weight |
|---|---|---|---|---|
| Label | `type/body/md` | Noto Sans | 14 px / 20 px | Regular (400) |
| Helper text | `type/body/sm` | Noto Sans | 12 px / 16 px | Regular (400) |

### Note to include below

- "Labels use Body (not Label token) — switch text describes what the toggle controls, not a compact UI label. Consistent with Checkbox and Radio."
- Annotation style.

---

## Section 9 — Spacing mapping

### Layout

An annotated visual diagram of the switch showing all spacing values, plus reference tables below.

### Annotated diagram

Show a Switch (On) instance with helper text, with dimension lines and values annotated:

```
              |<-- 52px -->|
              ┌────────────┐    |<-- 8px -->|
              │       ┌──┐ │    Label text
              │       │  │ │    |<-- 4px -->|
              └────────────┘    Helper text

Thumb: 24×24px, 4px inset from track edge
```

### Specs for dimension annotations

| Element | Spec |
|---|---|
| Dimension lines | 1 px stroke, `color/brand/primary-soft` (#FFB2E4) |
| Dimension value labels | 10 px, SemiBold, `color/brand/primary` (#D9008D) |
| Dimension line end caps | 4 px perpendicular tick marks |

### Reference table — Internal spacing

| Property | Token | Value | Note |
|---|---|---|---|
| Track to content gap (horizontal) | `space/8` | 8 px | Auto-layout horizontal gap |
| Label to helper text gap (vertical) | `space/4` | 4 px | Content frame vertical gap |
| Thumb inset (all sides) | — | 4 px | Absolute position within track |
| Track width | — | 52 px | Fixed dimension |
| Track height | — | 32 px | Fixed dimension |
| Thumb diameter | — | 24 px | Ellipse within track |

### Reference table — External spacing (layout guidance)

| Context | Token | Value |
|---|---|---|
| Between stacked switches | `space/12` | 12 px |
| Section gap (switch group to next section) | `space/24` | 24 px |
| Touch target (minimum) | — | 48 x 48 px (parent handles) |

---

## Section 10 — Radius, stroke, and shadow

### Content

| Foundation | Token | Value | Applied to |
|---|---|---|---|
| Radius | `radius/full` | 9999 px | Track (pill shape) |
| Radius | `radius/full` | 9999 px | Thumb (circle via ellipse) |
| Stroke (Off) | `stroke/2` | 2 px, inside | Track border in Off state |
| Stroke (On) | — | None | Filled = no border rule |
| Shadow | None | None | All states — flat control |

### Note to include

- "Filled state rule: When the switch is On (filled with `brand.primary`), the border is removed. The fill IS the boundary. Consistent with checked checkbox and filled button patterns."
- Annotation style.

---

## Section 11 — Helper text

### Layout

Two examples side by side:
1. Switch Off with helper text
2. Switch On with helper text

### Visual examples

| Instance | Selection | State | Show Helper Text | Label | Helper Text |
|---|---|---|---|---|---|
| 1 | Off | Default | true | "Enable notifications" | "You can change this later" |
| 2 | On | Default | true | "Auto-renew plan" | "Your plan renews monthly" |

Gap between instances: 48 px.

### Note below

- "Helper text is hidden by default. Toggle 'Show Helper Text' property to reveal. Switch has no Error state — binary toggles don't require form validation."
- Annotation style.

---

## Section 12 — Figma component structure

### Layout

Show the layer tree as a formatted text block on the page, using a monospace-styled frame or a text render of the layer structure.

### Content

```
Switch                        ← Component Set
├── Selection=Off, State=Default    ← Component (auto-layout H, gap: 8, center)
│   ├── Track                 ← Frame 52×32, radius-full, no auto-layout
│   │   └── Thumb             ← Ellipse 24×24, absolute position
│   └── Content               ← Frame (auto-layout V, gap: 4, hug)
│       ├── Label             ← Text: body.md (14/20 Regular)
│       └── HelperText        ← Text: body.sm (12/16 Regular) [hidden]
├── Selection=Off, State=Disabled
├── Selection=On, State=Default
└── Selection=On, State=Disabled
```

### Properties table

| Property | Type | Values | Default |
|---|---|---|---|
| Selection | Variant | Off, On | Off |
| State | Variant | Default, Disabled | Default |
| Show Helper Text | Boolean | true, false | false |
| Label | Text | (editable) | "Label" |
| Helper Text | Text | (editable) | "Helper text" |

### Note below

- "2 selection × 2 interaction = 4 variants. Boolean and text properties are handled by component properties, not separate variants."
- Annotation style.

---

## Section 13 — Design decisions

### Layout

A numbered list of decisions with brief rationale. Each item has a bold decision title and a one-to-two sentence explanation.

### Content

1. **Track uses `radius/full` (9999 px) for pill shape**
   Consistent with M3, iOS, and all top-10 design systems for switch controls.

2. **Filled = no border rule applied**
   On state removes track stroke. Fill IS the boundary. Matches checkbox, radio, and button patterns.

3. **Off track uses `color/surface/muted` fill + `color/border/strong` stroke**
   Provides clear visibility on `color/surface/base` backgrounds and budget Android screens.

4. **Thumb is white (#FFFFFF) in all states**
   Maximum contrast on both neutral and branded track fills.

5. **Disabled On state uses frame opacity 0.4**
   Matches checkbox checked + disabled pattern exactly.

6. **Disabled Off state uses stroke opacity 0.4 + `color/surface/subtle` fill**
   Matches checkbox unchecked + disabled pattern.

7. **No Error state**
   Switches are instant-apply binary toggles, not form inputs requiring validation. Consistent with M3, iOS, Polaris, and Primer.

8. **Track size 52 x 32 px with 24 x 24 px thumb**
   Follows M3 proportions, provides comfortable touch area on mobile.

9. **Label uses `type/body/md` (not label token)**
   Describes what the toggle controls (content), not a UI control label. Consistent with Checkbox and Radio.

10. **Cross-axis CENTER alignment**
    Vertically centers 32 px track with label text, unlike Checkbox/Radio which use MIN (20 px indicator matches 20 px text).

---

## Section 14 — V1 exclusions

### Layout

A bulleted list with a brief note on each excluded item.

### Content

- **Hover state** — mobile-first product, no web hover needed in V1
- **Focused state** — no keyboard focus ring needed for mobile V1. Deferred to accessibility phase.
- **Error state** — switches are binary toggles with instant apply, no form validation
- **Size variants (Small / Large)** — single size covers all V1 use cases. Deferred to V2.
- **Loading / indeterminate state** — not needed for V1 toggle patterns
- **Icon in thumb** — decorative icons (check, X) deferred. Thumb position is sufficient affordance.
- **Animation / transition specs** — handled by platform implementation, not Figma component
- **RTL (right-to-left) layout** — deferred to internationalization phase

---

## Exclusions for this page build

Do NOT add to this reference page:

- Actual interactive prototyping or click-through flows
- Code snippets or implementation instructions
- Native Figma component build steps (those belong in the build spec)
- Content from other components (Checkbox, Radio, Button, Input Field)
- Dark mode variants
- Tokens or values not present in `figma/components/switch-build.md`
- Speculative future features or sizes not in the V1 scope
