---
name: wiom-tabs-filters
description: >
  Wiom Tabs & Filters — three distinct components for narrowing what the user
  sees. **Pill Tab** is a top-level context switch (changes the entire
  dataset — "whose data am I looking at?"). **Underline Filter** is a
  within-dataset narrower (single-select — "which subset do I want?").
  **Chip** is a multi-select toggle ("which of these apply?").
  Use this skill EVERY TIME you create, edit, or review any screen that
  needs tabs, segmented controls, list filters, status filters, or any
  data-narrowing UI in the Wiom Partner or Customer app. Trigger when you
  see TabRow, ScrollableTabRow, FilterChip, SegmentedButton, Tab, Pill,
  Underline filter, "All / Pending / Active", "Customer / Partner" switch,
  or any list-narrowing pattern.
---

# Wiom Tabs & Filters

The narrowing-UI foundation for the Wiom Android apps. Built from Figma
frame `2124:2544` (Pill Tab section) and `2124:2824` (Underline Filter
section) of file `ZPErh7YU7omdbyy2o268Qo`, plus the `wiom-design-foundations`
token set — no other inputs.

**3 components · element-first tokens · contained-pattern (each lives
inside its own surface)**

The three solve different problems and are not interchangeable:

| Component | Question it answers | Selection model |
|---|---|---|
| **Pill Tab** | *"Whose data am I looking at?"* | Single-select; swaps the **entire dataset** under the tab control |
| **Underline Filter** | *"Which subset do I want?"* | Single-select; narrows a **slice** of the current dataset |
| **Chip** | *"Which attributes do I want to combine?"* | **Multi-select**; each chip independent — selecting more shows more |

If you're unsure: ask whether tapping changes *what data is loaded*
(Pill Tab), *which slice of the loaded data is shown — pick one*
(Underline Filter), or *which independent attributes to combine — pick
many* (Chip).

---

## 1. Pill Tab — context switch

A small pill-shaped selector inside a rounded muted track. Use for binary
or low-cardinality top-level mode switches where each tap switches the
current screen's primary data source.

### 1.1 Anatomy

```
Pill Track (bg.muted, radius-medium 12dp, padding space-4 all sides)
┌──────────────────────────────────────────────────────┐
│ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ │
│ │   Tab (A)    │ │   Tab (I)    │ │   Tab (D)    │ │   ← items
│ └──────────────┘ └──────────────┘ └──────────────┘ │
└──────────────────────────────────────────────────────┘
                       ↑
   each item: weight(1f) — splits track width evenly,
              hugs content vertically to track interior
```

| Slot | Value | Token |
|---|---|---|
| Track width | Fills parent (`fillMaxWidth()`) — never a hard width | — |
| Track height | Derived: `space-4 + item content + space-4` ≈ 40 dp at default tokens (consequence, not input) | — |
| Track radius | 12 dp | `radius-medium` |
| Track fill | `bg.muted` (#E8E4F0) | `bg.muted` |
| Track padding (all sides) | 4 dp | `space-4` |
| Item width | Splits track interior evenly (`weight(1f)`) | — |
| Item height | Fills cross-axis of track (32 dp at default tokens) | — |
| Item radius | 8 dp | `radius-small` |
| Item horizontal padding | 12 dp | `space-12` |
| Item label-to-badge gap | 8 dp | `space-8` |
| Label style | Noto Sans SemiBold 14 / 20 LH | `type.label.md` |
| Inner row | Horizontally and vertically centered | — |
| Active item shadow | `shadow.sm` (drop) | `shadow.sm` |

Source: `get_design_context` on `1043:316` (Pill Track Count=2),
`1043:253` (Active item), `1043:265` (Disabled item).

### 1.2 States — Pill Tab Item

| State | Container fill | Container shadow | Label | Default badge fill | Default badge label |
|---|---|---|---|---|---|
| **Active** | `bg.default` (#FAF9FC) | `shadow.sm` (drop) | `text.selected` (#D9008D) | `bg.brand` (#D9008D) | `text.onBrand` (#FFFFFF) |
| **Inactive** | transparent | none | `text.subtle` (#5C5570) | `bg.muted` (#E8E4F0) | `text.subtle` |
| **Disabled** | `bg.disabled` (#D7D3E0) | none | `text.disabled` (#A7A1B2) | `bg.muted` | `text.subtle` |

> **Disabled fill detail.** Figma node `1043:265` ships with `bg.disabled`
> on the disabled item (an explicit greyed pill). This deviates from the
> Figma description block which says "Disabled: 40% opacity" — the actual
> ship value is `bg.disabled`. Use the ship value, not the description
> note. (Same foundations Pattern A as `wiom-cta`.)

### 1.3 Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Active` | `Active` · `Inactive` · `Disabled` |
| `Badge` | Boolean | `false` | Show / hide a numeric count badge after the label |

### 1.4 Track variants (Count)

Figma ships three Pill Track variants: **Count = 1 · 2 · 3** (per
metadata `1043:313 / 1043:316 / 1043:320`). The published doc says "Count:
2 / 3 / 4" — that's an outdated description. **Use the ship value: 1–3
fixed counts. Pill Track is hard-capped at 3.**

| Count | Layout |
|---|---|
| 1 | Single full-width pill (rare; usually used when only one mode applies but the chrome is preserved for layout consistency) |
| 2 | Two equal-width pills (most common — Customer / Partner, Hindi / English, Active / All) |
| 3 | Three equal-width pills |

### 1.5 4+ tabs — not supported

**Pill Track is hard-capped at 3.** There is no 4-tab variant and no
horizontally-scrollable Pill Track fallback — both have been considered
and explicitly excluded from Wiom DS. 4+ Pill Tabs almost always
indicate the wrong component. Route to one of these instead:

- 4+ items that are **mutually exclusive views of different data** →
  Bottom Navigation (`wiom-navigation-bar`, 2–5 tabs) or list-of-rows
  navigation.
- 4+ items that are **slices of the same data** → Underline Filter is
  the right component, not Pill Tab. Underline Filter has no cap and
  can scroll horizontally.
- 4+ items that are **categories / sub-pages** → section headers,
  list-of-rows, or a separate screen per category.

Implementations must enforce the cap in code (`require(count in 1..3)`).
If a screen looks like it needs 4+ pills, that's the signal to revisit
the design — not to relax the cap.

> **Implementor note:** Android v0.3.0 currently caps at `2..4` and
> should tighten to `1..3` in the next minor release to match this
> skill. Other consumers should ship with the `1..3` cap from day one.

---

## 2. Underline Filter — within-dataset narrower

A horizontal bar of single-select filters. The active filter shows a 2 dp
brand-pink indicator at its bottom edge. Used inside a screen whose
dataset is already loaded — to narrow the visible slice.

### 2.1 Anatomy

```
Underline Filter Bar (fillMaxWidth, bottom border 1dp stroke.subtle)
┌──────────────────────────────────────────────────────┐
│  Filter   Filter   Filter   Filter   ⋯  [⚙]         │
│  ────                                                  │   ← 2dp brand indicator under active item
└──────────────────────────────────────────────────────┘
   ↑                                              ↑
   Each item: hug width                           Optional "More" icon-only
   Padding: 16 H / 12 V                           (opens bottom-sheet of all filters)
```

| Slot | Value | Token |
|---|---|---|
| Bar width | Fills parent (`fillMaxWidth()`) | — |
| Bar height | Derived: `item content + bottom border` ≈ 46 dp at default tokens | — |
| Bar bottom border | 1 dp `stroke.subtle` (#D7D3E0) | `stroke-small` + `stroke.subtle` |
| Bar background | None (transparent — sits on `bg.default` screen) | — |
| Item width | Hugs content (multiple labels of varying length) | — |
| Item layout | Column: Content row above 2 dp Indicator row | — |
| Content horizontal padding | 16 dp | `space-16` |
| Content vertical padding | 12 dp | `space-12` |
| Content gap (icon ↔ text, when both shown) | 8 dp | `space-8` |
| Inner row (Content) | Horizontally and vertically centered | — |
| Indicator height | 2 dp | `stroke-medium` |
| Indicator radius | full pill | `radius-full` |
| Indicator width | Fills item width (matches the content row above) | — |
| Label style | Noto Sans SemiBold 14 / 20 LH | `type.label.md` |
| Icon size (icon-only / "More") | 20 dp | `icon.sm` |

Source: `get_design_context` on `2941:6684` (Active item),
`2941:6736` (Bar Scrollable=Yes), `get_variable_defs` on `2941:6683`.

### 2.2 States — Underline Filter Item

| State | Indicator | Label | Icon (icon-only variant) |
|---|---|---|---|
| **Active** | `bg.brand` (#D9008D), 2 dp, full-width | `text.brand` (#D9008D) | `icon.brand` (#D9008D) |
| **Inactive (Default)** | transparent / hidden | `text.subtle` (#5C5570) | `icon.action` (#352D42) |
| **Disabled** | transparent | `text.disabled` (#A7A1B2) | `icon.disabled` (#A7A1B2) |

> **Indicator note.** The Figma description names the indicator color
> `stroke/selected`; the actual rendered code uses `bg/brand`. Both
> resolve to the same hex (#D9008D), but element-first discipline says
> the indicator is a 2 dp filled bar — a `bg.*` slot, not a `stroke.*`
> one. Use `bg.brand` in Compose (`Modifier.background(Wiom.bg.brand)`),
> not `BorderStroke`.

### 2.3 Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Active` | `Active` · `Default` · `Disabled` |
| `Badge` | Variant | `Off` | `On` / `Off` (count badge in the content row) |
| `Icon Only` | Variant | `No` | `Yes` makes the item an icon-only square (used for the trailing "More" / "Filter list" affordance) |

### 2.4 Bar variants & overflow — count-based rule

Filter count drives the layout. **Never mix scroll + More icon — they
solve different problems and combining them is a UX bug.**

```
count    fit check                  resulting layout
─────    ────────────────────────   ──────────────────────────────────
1–6      Items fit on bar width     ALL visible, no scroll, no More
                                    (whitespace on the right is fine)

1–6      Items DON'T fit            Horizontal scroll, NO More
         (any count in this range   Items keep hug-content width;
         can fail the fit check —   active auto-scrolls into view.
         even 3 long-label filters
         on a 360dp screen)

7+       (no fit check)             Restrict to fit-N + trailing More
                                    icon, NO scroll. Typical fit-N is
                                    2–4 items. More opens a bottom
                                    sheet picker showing every filter.
                                    Active filter is always inline-
                                    visible — picking a non-visible
                                    filter from the More sheet drops
                                    the rightmost inline filter and
                                    inserts the chosen one.
```

**The fit check isn't strictly count-based.** A 4-item bar with short
labels ("All / Open / Closed / More") fits at 360 dp. A 3-item bar with
long Hindi labels ("एक्सपायर्ड / प्रोसेसिंग / नया") might not. Measure
the rendered width at runtime; if total item width > bar width, switch
to scroll. The count is a guard rail (≥7 always uses fit-N + More
regardless of fit), not the trigger.

Why no scroll-plus-More: scroll is "swipe to reveal more of the same
list"; More is "pick from the full list elsewhere". A user who's already
swiping wouldn't expect a More icon to also exist (and vice versa) —
they'd miss half the filters. Pick one mechanism per bar.

> **Rule of thumb.** If you find yourself with 7+ filters, you almost
> certainly have an IA problem — verify with design before shipping. A
> well-modeled filter set is usually 3–5 items.

#### Bar mode summary

| Filter count | Items fit? | Mechanism | Trailing More | Scroll |
|---|---|---|---|---|
| 1–6 | yes | All visible | No | No |
| 1–6 | no | Horizontal scroll | No | Yes |
| 7+ | n/a | Restrict to fit-N + More | Yes (icon-only) | No |

The mechanism is determined at composition time by measuring rendered
item widths against bar width — this isn't a designer-set property.

#### "Restrict" pattern — what gets shown

When count ≥ 7, the bar shows whatever fits **alongside** the More
icon, never more. Example with a 360 dp wide bar and 8 filters:

```
─────────────────────────────────────────────────────────────
| All  Open  Pending  Closed  ⚙ |
| ────                            |  ← Active = "All"
─────────────────────────────────────────────────────────────
                                ↑
                     More — sheet opens with ALL 8 filters,
                     "All" pre-selected, Radio list per item
```

If "Open" then becomes active:

```
─────────────────────────────────────────────────────────────
| All  Open  Pending  Closed  ⚙ |
|       ──                       |  ← Active = "Open"
─────────────────────────────────────────────────────────────
```

If the user picks "Refunded" from the More sheet (a filter that wasn't
visible inline), the bar **drops the rightmost inline filter and
inserts "Refunded" at the active position**. The active item is
always inline — never hidden behind More.

#### The More-sheet contract

Per `wiom-bottomsheet`. The sheet body is a single `wiom-list-item`
list with `wiom-selection-controls` Radio leading. Header is the screen
title ("Filter by status"). Sticky bottom CTA bar uses
`wiom-cta` Primary, full-width, label "Apply" / "लागू करें". The CTA is
disabled until the user picks an option different from the currently-
active one. On Cancel / sheet-dismiss, the bar's active filter doesn't
change.

### 2.5 The "More" icon-only filter

Last-item escape hatch when the filter set is large. A 20 dp `filter_list`
icon, no label, no bottom indicator. Tap behavior:

- Opens a `wiom-bottomsheet` containing a single-select list (use
  `wiom-list-item` rows + `wiom-selection-controls` Radio).
- The bottom sheet's CTA confirms the selection (use `wiom-cta` Primary,
  full-width sticky).
- On confirm, the chosen filter becomes active; the bar updates the
  indicator to that filter's position. If the chosen filter is off-
  screen, scroll it into view.
- Touch target for the More button is 48 dp minimum even though the
  glyph is 20 dp — increase the item's padding (or wrap in a 48 dp
  hit-target Box) so users on dense screens can hit it reliably.

---

## 3. Chip — multi-select toggle

Independent toggles. Each chip selects/deselects on its own — multiple
chips can be active at once. Used for **attribute filtering**: combine
WiFi + Fiber + Repair to filter a list by all three at the same time.

### 3.1 Anatomy

```
Chip Row (Modifier.fillMaxWidth, padding space-16 horizontal, wrap layout)
┌────────────────────────────────────────────────────────────┐
│  [ Chip ]   [ Chip ]   [ Chip ✕ ]   [ Chip ]              │   ← gap space-8 H
│  [ Chip ]   [ Chip ✕ ]                                    │   ← wraps; vertical gap space-8
└────────────────────────────────────────────────────────────┘

Single Chip
┌──────────────┐
│ ←—— 12 ——→   │
│  Chip   ✕    │   ← Selected: label + 16dp close icon, gap space-4
│ ←—— 12 ——→   │   Unselected: label only, no icon
└──────────────┘
   radius-small (8dp) · height 36dp · hugs content width
```

| Slot | Value | Token |
|---|---|---|
| Container width | Hugs content (label + optional close icon) | — |
| Container height | 36 dp (fixed — chips align to a shared baseline within a row) | — |
| Container radius | 8 dp | `radius-small` |
| Container shadow | none | `shadow.none` |
| Horizontal padding | 12 dp | `space-12` |
| Vertical padding | derived (~ 8 dp top/bottom from 36 dp − 20 LH) | — |
| Label-to-close gap | 4 dp | `space-4` |
| Label style | Noto Sans SemiBold 14 / 20 LH (all states) | `type.label.md` |
| Close icon size (Selected only) | 16 dp | — |
| Border width | 1 dp | `stroke-small` |
| Inner row | Horizontally and vertically centered | — |

Source: `get_design_context` on `1043:302` / `1043:304` / `1043:309`,
`get_variable_defs` on `1043:301`.

### 3.2 States

| State | Container fill | Border | Label | Trailing icon |
|---|---|---|---|---|
| **Unselected** | transparent | 1 dp `stroke.default` (#E8E4F0) | `text.default` (#161021) | none |
| **Selected** | `bg.selected` (#FFCCED) | 1 dp `stroke.selected` (#D9008D) | `text.selected` (#D9008D) | 16 dp close ✕, `icon.brand` (#D9008D) |
| **Disabled** | `bg.disabled` (#D7D3E0) | none | `text.disabled` (#A7A1B2) | none |

> **Filled-state-rule note.** The Figma description says "Selected = no
> border (filled state rule)". The actual Figma ship has a 1 dp brand
> border on Selected. Use the **ship value** (border present) — both
> the Figma component variant and the rendered design context include
> it. Disabled does follow the filled rule (no border, just `bg.disabled`).
> All Chip labels are SemiBold across all states (the old skill claimed
> Unselected was Regular — that was wrong; Figma ships SemiBold).

### 3.3 Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | Variant | `Unselected` | `Unselected` · `Selected` · `Disabled` |

The Selected variant ships with a trailing close (✕) by design — tapping
it deselects the chip. There is no separate boolean for the close
glyph; it's part of the Selected variant.

### 3.4 Chip Row (the container)

| Slot | Value | Token |
|---|---|---|
| Row width | Fills parent (`fillMaxWidth()`) — surface owns the gutter | — |
| Row horizontal padding | 16 dp | `space-16` |
| Chip-to-chip gap (horizontal) | 8 dp | `space-8` |
| Wrap gap (vertical, when wrapping) | 8 dp | `space-8` |
| Wrap behavior | Yes — chips wrap to the next line when row width is exceeded | — |
| Shadow | none | `shadow.none` |

Chips in a Row **wrap** rather than scroll. Unlike the Underline Filter
Bar (which scrolls or shows a More icon), a Chip Row simply flows to
multiple lines. This matches the "see all attributes at once" mental
model — users typically want every option visible to know what they're
combining.

### 3.5 What / where to use — Chip

- **Use for** — attribute filtering where multiple values combine
  additively (Service type, Tags, Topic). Each chip is an *AND* filter
  over the dataset.
- **Don't use for** — mutually exclusive choices (use Underline Filter);
  context switching (use Pill Tab); navigation (use list / drawer);
  status filtering (use Underline Filter — status is single-select).
- **Where** — typically inside a sort/filter bottom sheet, or directly
  on screen as a Chip Row above the content for rapid attribute
  toggling. Not inside Top Bar / Nav Bar.
- **Sample copy (Partner)** — "इंस्टॉलेशन", "रिपेयर", "रिचार्ज", "अपग्रेड";
  "WiFi", "Fiber", "Hotspot".
- **Sample copy (Customer)** — "1 BHK", "2 BHK", "Studio", "Furnished";
  "Hindi", "English", "Marathi".

### 3.6 Selection model

- Each chip has its own `checked: Boolean`. There's no "active index" —
  use a `Set<chipId>` of selected ids in state.
- Tapping an Unselected chip selects it; tapping a Selected chip (on
  the body or its close ✕) deselects it.
- "All / Clear" semantics are not built into Chips — if your design
  needs a "Clear all" affordance, add a separate Tertiary CTA or a
  reset row above the Chip Row.
- The row never has a "default selection" enforcement (unlike Underline
  Filter, which always has exactly one Active). Zero selected is a
  valid state — it usually means "no attribute filter applied".

### 3.7 Compose API

```kotlin
@Composable
fun WiomChipRow(
    chips: List<WiomChip>,
    onToggle: (chipId: String) -> Unit,
    modifier: Modifier = Modifier,
)

data class WiomChip(
    val id: String,
    val label: String,
    val selected: Boolean = false,
    val enabled: Boolean = true,
)
```

**Implementation notes** (derive the body from §3.1 anatomy + §3.2 states + §3.4 row spec):

- Use `androidx.compose.foundation.layout.FlowRow` (Compose 1.4+) so chips
  wrap on overflow. `Modifier.fillMaxWidth()` + `padding(horizontal =
  space.s16)` on the row; `horizontalArrangement` and
  `verticalArrangement` both `spacedBy(space.s8)`.
- Each item is a `Surface` of fixed `height(36.dp)`, `RoundedCornerShape(8.dp)`,
  `clickable(enabled = chip.enabled)`. Selected adds a 1 dp `BorderStroke`
  with `stroke.selected`; Unselected uses `stroke.default`; Disabled has
  no border (foundations Pattern A).
- Inner `Row(padding horizontal = space.s12, arrangement spacedBy(space.s4,
  CenterHorizontally), verticalAlignment = CenterVertically)`: label
  (`type.label.md`, color per §3.2) + on Selected only, a 16 dp
  `Icons.Rounded.Close` tinted `icon.brand`.

### 3.8 Chip-specific anti-patterns

- ❌ Single-select Chip Row (defeats the point — use Underline Filter).
- ❌ Forcing exactly-one-selected at all times (Chips support zero
  selected and N selected; that's the model).
- ❌ Horizontal scroll instead of wrap. Chips always wrap.
- ❌ Border on the Disabled chip (Disabled uses `bg.disabled` only, no
  border — actual filled-state rule).
- ❌ Regular-weight label on Unselected (Figma ships SemiBold across
  all states).
- ❌ Different chip heights in the same row (all chips = 36 dp for row-
  baseline alignment).
- ❌ Chip Row inside Top Bar / Nav Bar (Chip Row sits inline on the
  screen or inside a sort/filter sheet).
- ❌ Chip Row above a Pill Tab / Underline Filter (filter hierarchy is
  Pill → Underline → Chip; never reverse).
- ❌ Trailing close icon on Unselected (close ✕ is Selected-only).

---

## 4. Sizing and surface contract

Both components follow the same contract as `wiom-cta`:

1. **The screen / page surface decides the gutter** from the screen edge.
   Pill Track and Underline Filter Bar never carry their own horizontal
   margin — the parent (screen scaffold, sheet content padding, dialog
   padding) does.
2. **The component fills the remaining width** via
   `Modifier.fillMaxWidth()`. Width emerges from the surface; do not
   pin it.
3. **Pill Tab Item width** = track interior width / item count
   (via `Row.weight(1f)`). **Underline Filter Item width** = hug content
   (because labels vary in length and indicator must align with label
   bounds, not divide bar width evenly).
4. **Heights** derive from padding + content where possible (Underline
   Filter Item: 12 V pad + 20 LH + 12 V pad + 2 indicator = 46 dp).
   Pill Tab Item is the one place a fixed 32 dp item height is justified —
   inside the Track all items must align to the same baseline regardless
   of label length, and the Track's 40 dp height = `space-4` (top) + 32
   item + `space-4` (bottom).
5. **Inner content of every item is centered** both axes. No leading
   alignment; no space-between.

Anti-patterns this rules out:

- ❌ `Modifier.width(...)` on Pill Track or Underline Filter Bar.
- ❌ Hardcoding individual item widths inside an Underline Filter Bar
  (defeats the point of variable-width labels).
- ❌ `Modifier.padding(horizontal = 16.dp)` on the Pill Track or Bar
  itself — the surface owns the gutter.
- ❌ Underline Filter items splitting bar width evenly (use hug content).
- ❌ Pill Tab items hugging label width (must split evenly via
  `weight(1f)`).

---

## 5. Decision logic — which one to use

```
What does the control do when the user taps a different option?

Loads a different dataset (e.g. switches from Customer view to Partner
view; from Hindi tickets to English tickets; from "my data" to "team data"):
   → PILL TAB
   The user is changing context.

Filters / narrows the currently-loaded dataset (e.g. "show only Active",
"show only Pending", "show only this month"):
   → UNDERLINE FILTER
   The user is narrowing what's already loaded.

Multi-selects independent attributes that combine additively (e.g.
WiFi + Fiber + Repair to filter jobs by all three):
   → CHIP (multi-select)
   The user wants several attributes ON at once.

Shows a navigation menu / takes the user to a different screen:
   → NEITHER. Use a list, navigation drawer, or top-bar menu.
```

### Sample Pill Tab use cases

| Screen | Tabs | Why Pill Tab |
|---|---|---|
| Partner home | "Active jobs" / "Past jobs" | Each tab loads a different jobs dataset |
| Customer wallet | "Wallet balance" / "Pre-booking balance" | Two different ledgers |
| Bilingual content | "हिंदी" / "English" | Switches the content's language |
| Multi-account view | "My account" / "Family account" | Switches the data subject |

### Sample Underline Filter use cases

| Screen | Filters | Why Underline Filter |
|---|---|---|
| Tickets list | "All" / "Open" / "Closed" / "More" | Filters a single tickets dataset |
| Transactions | "All" / "Recharges" / "Refunds" / "Adjustments" | Slices one transactions dataset |
| Notifications | "All" / "Unread" / "Mentions" | Same notifications, different slice |
| Job board (Partner) | "All" / "Nearby" / "High priority" | Filters jobs in the same region |

---

## 6. Use cases — what / where to use

### 6.1 Pill Tab

- **Use for** — top-level context switches with 2 or 3 mutually-exclusive
  modes; bilingual toggles; persona / account-scope switches; before vs
  after states (Pre-booking vs Post-booking).
- **Don't use for** — narrowing within a dataset (use Underline Filter);
  navigation between unrelated screens (use list rows / nav drawer);
  4+ modes (reconsider the model — usually a sign that one of those
  modes should be a sub-page).
- **Where** — directly under the Top Bar, full-width within the screen
  scaffold's 16 dp gutter; inside a card when the entire card needs a
  context switch (rare); never inside dense list rows.
- **Sample copy** — "Active jobs" / "Past jobs", "हिंदी" / "English",
  "Wallet" / "Pre-booking", "Customer" / "Partner".

### 6.2 Underline Filter

- **Use for** — single-select narrowing of a list, table, or feed; status
  filters ("Open / Closed / All"); type filters ("Recharges / Refunds /
  All"); period filters when there are 3–5 fixed periods.
- **Don't use for** — multi-select filtering (use Chip, §3 of this skill); free-text search (use Search top
  bar); top-level context switching (use Pill Tab); date-range pickers
  (use Input field).
- **Where** — directly under the Top Bar (or under a Pill Tab if both
  exist on the same screen — Pill Tab on top, Underline Filter below);
  full-width within the screen scaffold's 16 dp gutter; never inside a
  bottom sheet or dialog (those use radio lists).
- **Sample copy** — "All", "Open", "Closed", "Pending", "नया",
  "पुराना", icon-only "More" trailing affordance for 7+ filters (§2.4).

### 6.3 Combinations & stacking — what's allowed on one screen

Filtering hierarchy goes **broad → narrow**, top → bottom. Pill Tab
swaps the dataset; Underline Filter slices it; never reverse the order
or stack three layers of narrowing UI.

| Combination | Verdict | When to use | Layout |
|---|---|---|---|
| **Pill Tab + Underline Filter** | ✅ Valid | Two-level data split — *whose* data + *which slice* | Pill on top, Underline below (directly under Top Bar → Pill → Underline → content) |
| **Pill Tab alone** | ✅ Valid | Single-level context switch (e.g. "हिंदी / English") | Directly under Top Bar |
| **Underline Filter alone** | ✅ Valid | Single-level subset filter on one dataset | Directly under Top Bar |
| **Two Pill Tabs** | ❌ Invalid | — | Only one context switch per screen. Reconsider IA. |
| **Two Underline Filters** | ❌ Invalid | — | Combine into one row, or move the second dimension to a sort/filter sheet (chips). |
| **Underline above Pill** | ❌ Invalid | — | Always Pill above. Broader (context) frames narrower (subset). Reversing breaks the user's mental model. |
| **Pill + Underline + a third filter row** | ❌ Invalid | — | Three filter levels = too complex. Move the 3rd dimension into section headers in content, into a sort/filter sheet, or onto the row itself. |
| **Chip Row alone (inline or in sheet)** | ✅ Valid | Multi-attribute narrowing of one dataset | Inline above content (rare) or inside a sort/filter bottom sheet (common) |
| **Pill + Chip Row** | ✅ Valid (rare) | Mode switch + multi-attribute filter | Pill on top, Chip Row in content area or in a sort/filter sheet — never two filter rows under the Top Bar |
| **Underline + Chip Row** | ✅ Valid (rare) | Status filter + multi-attribute filter | Underline under Top Bar; Chip Row in content area or in a sort/filter sheet |
| **Pill + Underline + Chip** | ❌ Invalid | — | Three filter layers stack to too much chrome. Drop one — usually the Chip Row moves into a sort/filter sheet. |
| **Two Chip Rows on one screen** | ❌ Invalid | — | One Chip Row per screen. Group all attributes into a single row (or sheet) with section headers if needed. |

#### When the user flips the Pill Tab

The Underline Filter **resets to its default** (typically the first
item, e.g. "All" / "सभी"). This is non-negotiable: a dataset switch
invalidates the previous slice. Don't carry the filter selection across
contexts — the user sees a fresh "All" view of the new dataset.

If the screen has both **Pill Tab + Underline Filter + scrollable
content**, the Pill and Underline together are a sticky header (or
scroll-collapsing top bar group, depending on `wiom-top-bar`
configuration). Content scrolls under them.

---

### 6.4 Pairing Pill Tab + Underline Filter on the same screen

Allowed and common — the two answer different questions:

```
┌──────────────── Top Bar ────────────────┐
│         Tickets                         │
└─────────────────────────────────────────┘
┌─────── Pill Tab (context) ──────────────┐
│  [ My tickets ]   Team tickets          │   ← which dataset
└─────────────────────────────────────────┘
┌──── Underline Filter (subset) ──────────┐
│  All   Open   Closed   ⋯  [⚙]          │   ← which slice of that dataset
└─────────────────────────────────────────┘
```

Order is **Pill Tab above Underline Filter**, never the reverse — the
context (whose data) frames the subset (which slice). When the user
flips Pill Tab, the Underline Filter resets to its default ("All" or
the first item).

---

### 6.5 Partner-app-specific rules

The Partner app trains repetitive muscle memory. These rules tighten
how filters / tabs behave for Partners specifically; the Customer app
is more forgiving.

1. **Filters always visible.** If a screen has a filter bar, show it
   regardless of list length. Don't conditionally hide it when the
   list is short — Partners learn the filter sits in the same spot
   every time and tap there reflexively.
2. **Low-frequency filter → bottom-sheet action.** If a filter is
   used fewer than ~3 times per session (analytics), promote it out
   of the persistent bar into a sort/filter sheet triggered from a
   trailing icon in `wiom-top-bar`. Persistent bar real estate is
   reserved for high-frequency filters.
3. **Two filter options → consider inline.** Don't add a Pill Tab or
   Underline Filter just because there are two states. If both can
   be shown side-by-side as plain content (e.g. two cards), do that
   instead. Component chrome adds friction.
4. **Pill Tab is always optional.** If a screen renders only one
   dataset (no "mine vs team" split, no "Hindi vs English"), skip the
   Pill Tab. Don't add it for layout symmetry.
5. **Max 2 filter levels per screen.** Pill Tab + Underline Filter is
   the ceiling. A third dimension goes into section headers in the
   content area, into a sort/filter sheet, or onto each row.
6. **Design-review threshold** (separate from the §2.4 overflow rule). A screen with 1 filter row and ≤4 filters
   ships. A screen with 2 filter rows or 5+ filters needs design
   review before shipping — that's a signal the model is getting
   complex.

---

### 6.6 Sample copy — Partner & Customer

| Component | Partner-app copy | Customer-app copy |
|---|---|---|
| Pill Tab | "मेरे टिकट" / "टीम के टिकट" | "वॉलेट" / "प्री-बुकिंग" |
| Underline Filter | "सभी" / "पेंडिंग" / "बंद" / "एक्सपायर्ड" | "सभी" / "रिचार्ज" / "रिफंड" / "अन्य" |
| More-sheet CTA | "लागू करें" / "Apply" | "लागू करें" / "Apply" |

Hindi is primary on both apps; English is the toggle. Keep filter
labels short (≤10 chars Hindi, ≤14 chars English) so 3–4 filters fit
without scroll on a 360 dp screen.

---

## 7. Universal rules

1. **No fixed dimensions on the bar / track containers.** Width emerges
   from `fillMaxWidth()`; height emerges from internal padding + content
   per the rules above.
2. **Pill Tab Items split track width evenly** via `weight(1f)`. **Underline
   Filter Items hug content** (do not split evenly).
3. **One Active per group.** Pill Tab is exactly-one selection;
   Underline Filter is exactly-one selection. Multi-select is out of
   scope — use chip filters in a sort/filter sheet.
4. **Active visual is mandatory and unique.** Pill Tab uses the elevated
   pill (bg.default + shadow.sm). Underline Filter uses the 2 dp brand
   indicator. Never both, never neither.
5. **Inactive must be visibly recessive but readable** — `text.subtle`
   (#5C5570) clears WCAG body contrast on `bg.default` and `bg.muted`.
   Do not use lighter greys for inactive state.
6. **Disabled = whole item is greyed and non-tappable.** Pill Tab uses
   `bg.disabled` + `text.disabled`. Underline Filter has no fill change
   (it's a transparent surface) — the label drops to `text.disabled` and
   the indicator stays hidden. Never pure opacity.
7. **Label style is `type.label.md` (14 sp SemiBold / 20 LH)** for both
   components. Smaller than CTA's `type.label.lg` because tabs / filters
   are denser navigation chrome, not primary actions.
8. **Tab / filter copy reflects the data, not the action.** "Active jobs"
   not "Show active". The user is choosing what they're looking at, not
   issuing a command.
9. **Switching state is instant and free.** No confirmation, no loading
   spinner blocking the tap — the new dataset / slice can resolve while
   the user is already looking at the new tab. If a network call is
   required, show a skeleton in the content area, not on the tab.
10. **Single-row only — never wrap to a second row.** Overflow is
    handled differently per component:
    - **Pill Tab:** capped at 3 in Figma. 4+ is a design-review smell;
      see §1.5.
    - **Underline Filter:** count-based per §2.4 — 1–6 fits or scrolls,
      7+ uses fit-N + More icon. **Never combine horizontal scroll with
      the More icon on the same bar** — pick one mechanism.
11. **Pill Tab sits above Underline Filter** when both appear on the
    same screen. Tab = context, Filter = subset. Flipping the Tab resets
    the Filter to default.
12. **Touch target 48 dp minimum** even when the visual height is
    smaller (Pill item 32 dp visual). Wrap in a 48 dp hit Box, or use
    `Modifier.minimumInteractiveComponentSize()`. Underline Filter Item
    at 46 dp is borderline — pad to 48 if your accessibility audit
    requires.

---

## 8. Compose implementation

### 8.1 Public API

```kotlin
// Pill Tab — context switch
@Composable
fun WiomPillTabRow(
    selectedIndex: Int,
    tabs: List<WiomPillTab>,            // 1–3 tabs ship; 4+ falls back to scrollable
    onSelect: (Int) -> Unit,
    modifier: Modifier = Modifier,
)

data class WiomPillTab(
    val label: String,
    val badgeCount: Int? = null,        // null = no badge
    val enabled: Boolean = true,
)

// Underline Filter — within-dataset narrower
@Composable
fun WiomUnderlineFilterBar(
    selectedIndex: Int,
    filters: List<WiomUnderlineFilter>,
    onSelect: (Int) -> Unit,
    overflow: FilterOverflow = FilterOverflow.None,
    modifier: Modifier = Modifier,
)

// Mutually exclusive overflow modes — sealed class enforces the
// "never combine scroll + More" rule at compile time. Pick one based
// on §2.4 (count + fit-check):
//   count ≤ 6 AND items fit         → FilterOverflow.None
//   count ≤ 6 AND items don't fit    → FilterOverflow.HorizontalScroll
//   count ≥ 7 (regardless of fit)    → FilterOverflow.More(...)
sealed class FilterOverflow {
    object None : FilterOverflow()
    object HorizontalScroll : FilterOverflow()
    data class More(
        val icon: ImageVector = Icons.Rounded.FilterList,
        val onClick: () -> Unit,
    ) : FilterOverflow()
}

data class WiomUnderlineFilter(
    val label: String,
    val badgeCount: Int? = null,
    val enabled: Boolean = true,
)
```

### 8.2 Implementation notes

Derive component bodies from the anatomy + state tables (§1.1, §1.2,
§2.1, §2.2). Key behaviors that must be encoded in code:

**WiomPillTabRow** — Outer `Row.fillMaxWidth().clip(RoundedCornerShape
(radius.medium)).background(bg.muted).padding(space.s4)`. Each item is
`Modifier.weight(1f)` (split track interior evenly). Active item =
`Surface(color = bg.default, shadowElevation = shadow.sm)`; Inactive =
transparent; Disabled = `bg.disabled`. Inner `Row(padding horizontal =
space.s12, arrangement spacedBy(space.s8, CenterHorizontally),
verticalAlignment = CenterVertically)` with `type.label.md` label +
optional `WiomCountBadge` (tone Brand on Active, Neutral otherwise).
Label color: Active = `text.selected`, Inactive = `text.subtle`,
Disabled = `text.disabled`.

**WiomUnderlineFilterBar** — Outer `Box.fillMaxWidth().drawBottomBorder
(stroke.small, stroke.subtle)`. Inner `Row` is wrapped with
`horizontalScroll(rememberScrollState())` only when `overflow ==
HorizontalScroll`; otherwise plain `Row`. Iterate `filters` then append
a "More" item only when `overflow is FilterOverflow.More`. Each item is
a `Column(horizontalAlignment = CenterHorizontally)`: a Content Row
(`padding horizontal = space.s16, vertical = space.s12, spacedBy
(space.s8, CenterHorizontally)`) above a 2 dp Indicator
(`Box.fillMaxWidth().height(2.dp).background(if (isActive) bg.brand
else Color.Transparent, RoundedCornerShape(radius.full))`). The
indicator is a **filled `Box`, not a `BorderStroke`** — the token
`bg.brand` is a fill slot, not a stroke slot. More item is icon-only
(20 dp `Icons.Rounded.FilterList`, tint `icon.action`) inside the same
padding.

**WiomChipRow** — see §3.7 implementation notes.

---

## 9. Anti-patterns

- ❌ Using Pill Tab for narrowing within a dataset (use Underline Filter).
- ❌ Using Underline Filter for context switching (use Pill Tab).
- ❌ More than one Active item in a group.
- ❌ No Active item ("nothing selected" state — there must always be one
  active item, even on first load).
- ❌ Pill Tab Items hugging content width (must split evenly).
- ❌ Underline Filter Items splitting bar width evenly (must hug content).
- ❌ `Modifier.width(...)` on the Track or Bar.
- ❌ `Modifier.padding(horizontal = 16.dp)` on the Track or Bar (surface
  owns the gutter).
- ❌ Active state visual that overlaps Inactive (e.g. a brand-tinted
  Active text *plus* an indicator under it on Pill Tab — pick one).
- ❌ Filter Bar items with `bg.*` fill (other than transparent) — the bar
  is flat. Active is signalled by the 2 dp indicator alone, not a fill.
- ❌ Mixing the two visual languages (a Pill Track with an underline
  indicator, or an Underline Bar with elevated active pills).
- ❌ Wrapping Pill Tab labels to 2 lines or shrinking font — rewrite
  shorter copy.
- ❌ Using `text.subtle` for Active label — Active must be `text.selected`
  (Pill) or `text.brand` (Underline).
- ❌ Using opacity for Disabled — use the foundations Pattern A
  (`bg.disabled` / `text.disabled`).
- ❌ Loading spinner / blocking UI on tab tap — render the new content
  with a skeleton, keep tabs interactive.
- ❌ Pill Tab below Underline Filter on the same screen — Tab is always
  on top.
- ❌ More than 3 Pill Tabs (reconsider the model; consider Underline
  Filter or a different navigation pattern).
- ❌ Multi-select Underline Filter (single-select only — for multi-select,
  use chip filters in a sort/filter sheet).
- ❌ **Combining horizontal scroll AND a More icon on the same Underline
  Filter Bar.** Pick one mechanism per bar: scroll when 1–6 items don't fit,
  More-icon for 7+ (§2.4). Mixing them means users miss filters because they're
  hidden in two different places.
- ❌ Underline Filter with 7+ items rendered as a horizontal scroll
  without the More icon. 7+ goes to fit-N + More — not scroll.
- ❌ More icon + scroll + 7+ items "for safety" (still mixing). Choose.
- ❌ Hiding the active filter behind the More icon. The active filter
  must always be inline-visible on the bar; if the user picks a
  non-visible filter from the More sheet, the bar drops the rightmost
  inline filter and inserts the chosen one at the active slot.
- ❌ Non-resetting Underline Filter when Pill Tab changes. Flipping the
  context resets the filter to default — never carry across.
- ❌ Stacking 3 narrowing components (Pill + Underline + a third row).
  Max 2 filter levels per screen.

---

## 10. Figma assembly recipes — composing in Figma using existing components

When designing a new screen in Figma, **reuse the published components**
(do not draw fresh frames). The Figma-component variants already encode
all the rules above, so picking the right variant + property values
gives a design that maps 1-to-1 to the Compose API.

### Component-to-property cheat sheet

| Component | Node | Properties to set | Mapping to Compose |
|---|---|---|---|
| **Pill Tab Item** | `1043:250` | `State` (`Active` / `Inactive` / `Disabled`), `Badge` (boolean) | One element of `tabs: List<WiomPillTab>`; `selectedIndex` derives Active. `Badge` → `WiomPillTab.badgeCount != null`. |
| **Pill Track** | `1043:312` | `Count` (1 / 2 / 3) | `WiomPillTabRow(tabs = listOf(... .size 1–3))` |
| **Underline Filter Item** | `2941:6683` | `State` (`Active` / `Default` / `Disabled`), `Badge` (`On` / `Off`), `Icon Only` (`Yes` / `No`) | One element of `filters: List<WiomUnderlineFilter>`; `Icon Only=Yes` is reserved for the More item only. |
| **Underline Filter Bar** | `2941:6714` | `Scrollable` (`No` / `Yes`), `tab3..tab6` booleans, `moreIconTab` boolean | `FilterOverflow.None` (Scrollable=No, no More) · `FilterOverflow.HorizontalScroll` (Scrollable=Yes, no More) · `FilterOverflow.More(...)` (Scrollable=No, moreIconTab=true) |
| **Chip** | `1043:301` | `State` (`Unselected` / `Selected` / `Disabled`) | One element of `chips: List<WiomChip>`; `selected: Boolean` toggles between Unselected ↔ Selected; `enabled = false` → Disabled |
| **Chip Row** | `2124:2801` (instance) | (no variant; just place chip instances inside a wrapping Auto-Layout frame with 8 H gap, 8 V gap when wrapped) | `WiomChipRow(chips, onToggle)` — uses `FlowRow` |

### Assembly recipe — pairing template

Standard pattern when combining components on one screen
(Top Bar → Pill Tab → Underline Filter → content; Pill flip resets
the Filter):

```kotlin
Column {
    WiomTopBar(title = …)
    WiomPillTabRow(            // optional — only when there's a context switch
        selectedIndex = mode,
        tabs = …,              // 1–3 WiomPillTab
        onSelect = { mode = it; status = 0 },   // reset filter on context flip
    )
    WiomUnderlineFilterBar(    // optional — only when there's a slice filter
        selectedIndex = status,
        filters = …,
        onSelect = { status = it },
        overflow = FilterOverflow.None,         // or HorizontalScroll / More(...)
    )
    // …content
}
```

Chip Row sits inline in content (or inside a sort/filter sheet),
not stacked under the Underline Filter.

### Figma-only tips

- Use the Figma component instances (`Cmd/Ctrl+Alt+K` on canvas → search
  "Pill Track" / "Underline Filter Bar" / "Pill Tab Item" / "Underline
  Filter Item"). Don't draw fresh rectangles.
- Set the **Variant** properties on the right panel to switch State /
  Count / Scrollable / Icon-Only. Don't override colors or padding
  manually — that breaks the bound design tokens.
- Add a `Frame` ancestor with the screen-padding (`space-16`) so the
  Track / Bar fills its parent — components are designed `FILL`.
- For a screen with both Pill + Underline, place them inside one
  vertical Auto-Layout frame with 0 gap; the bottom border on the
  Underline Bar visually separates it from content.

---

## 11. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo` (Wiom Design System V2). Confirmed via
`get_metadata`, `get_design_context`, `get_variable_defs` on:

| Node | What | Confirms |
|---|---|---|
| `2124:2544` | Tab / Filters / Chips section | Pill Tab + Pill Track frames |
| `1043:250` | Pill Tab Item component set | 3 states × 2 badge boolean |
| `1043:253` | Pill Tab Item Active | `bg.default` · `shadow.sm` · `text.selected` |
| `1043:259` | Pill Tab Item Inactive | transparent · `text.subtle` |
| `1043:265` | Pill Tab Item Disabled | `bg.disabled` · `text.disabled` |
| `1043:312` | Pill Track component set | Count = 1 / 2 / 3 |
| `1043:316` | Pill Track Count=2 | `bg.muted` · `radius-medium` · `space-4` padding · `weight(1f)` items |
| `2124:2824` | Underline Filter section | Item + Bar frames |
| `2941:6683` | Underline Filter Item component set | 3 states × Icon-Only boolean |
| `2941:6684` | Underline Filter Item Active | `text.brand` · 2 dp `bg.brand` indicator |
| `2941:6694` | Underline Filter Item Default | `text.subtle` · indicator transparent |
| `2941:6714` | Underline Filter Bar component set | Scrollable = No / Yes |
| `2941:6736` | Underline Filter Bar Scrollable=Yes | `stroke.subtle` 1 dp bottom border · items hug content |
| `2124:2823` | Chip section | Chip frame + Chip Row instance |
| `1043:301` | Chip component set | 3 states · 36 dp height · `radius-small` |
| `1043:302` | Chip Unselected | transparent fill · 1 dp `stroke.default` border · `text.default` |
| `1043:304` | Chip Selected | `bg.selected` fill · 1 dp `stroke.selected` border · `text.selected` · 16 dp close icon |
| `1043:309` | Chip Disabled | `bg.disabled` fill · no border · `text.disabled` |
| `2124:2801` | Chip Row instance | wraps · `space-8` H + V gaps · `space-16` H padding |

**Description-vs-ship discrepancies the skill resolves:**

- Pill Tab gap: doc says `space-4`, code uses `space-8`. Ship value
  wins (`space-8`).
- Pill Track count: doc says 2 / 3 / 4, variants ship 1 / 2 / 3. Ship
  values win.
- Pill Tab Disabled: doc says "40% opacity", node ships `bg.disabled`.
  Ship value wins (also matches foundations Pattern A).
- Underline Bar bottom border: doc says `stroke.default`, code uses
  `stroke.subtle`. Ship value wins.
- Underline Bar items: doc says "Filter items = Fill width", code
  renders items as `shrink-0` (hug). Ship behavior wins.
- Underline indicator color: doc says `stroke.selected` (a stroke
  slot), code uses `bg.brand` (a fill slot). Element-first wins:
  `bg.brand`.

Last source pull: today (per `currentDate`). Re-pull
`get_variable_defs` on `2124:2545` and `2941:6683` if foundations
update token names.

---

## See also

- `wiom-design-foundations` — token source. Color tables in
  `references/color/core-tokens.md`; spacing, radius, stroke, shadow,
  typography sub-references.
- `wiom-compose-layout-rules` — Modifier discipline (no width / height
  on Track or Bar; `Icons.Rounded.*` only).
- `wiom-bottomsheet` — the surface that hosts the "More" filter
  selection list.
- `wiom-list-item` + `wiom-selection-controls` — the rows + Radio
  control inside the More-filter bottom sheet.
- `wiom-cta` — the Primary CTA that confirms the selection inside the
  More-filter bottom sheet.
- `wiom-badge` — the count badge used optionally inside Pill Tab and
  Underline Filter items.
- `wiom-top-bar` — the surface that always sits directly above
  Pill Tab / Underline Filter.

---

## Out of scope (flagged gaps)

These were not in the Figma URLs you provided, so they are deliberately
not encoded in this skill:

- **Date-range / numeric-range filters** — input-driven, not chip-driven.
  See `wiom-input-fields` if needed.
- **Free-text search filter** — that's a Top Bar Search variant; see
  `wiom-top-bar`.
- **Sort options** (single-select sort orders) — typically a sort/filter
  bottom sheet with `wiom-list-item` + Radio rather than a chip.
