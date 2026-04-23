---
name: wiom-tabs-filters
description: Wiom Tab, Filter & Chip component system — context switching, single-select filtering, multi-select filtering. Use this skill EVERY TIME you create, edit, or review any screen that needs tabs, filters, segmented controls, chip filters, or any data-narrowing UI for the Wiom Partner or Customer app. Trigger when you see TabRow, ScrollableTabRow, FilterChip, SegmentedButton, Tab, Chip, or any list filtering pattern.
---

# Wiom Tabs, Filters & Chips System

The approved navigation/filtering foundation for the Wiom app ecosystem (Android/Kotlin/Compose).
**3 components · 3 hierarchy levels · Decision tree · Combination rules**

---

## 1. The 3 Components

### Level 1: Pill Tab — Context Switch

**Question:** "Whose data / which mode am I in?"

| Property | Value | Token |
|----------|-------|-------|
| Height | 40px (track) / 32px (pill) | — |
| Track background | `bg.muted` #E8E4F0 | Color skill |
| Track radius | 12px | `radius-medium` |
| Track padding | 4px | `space-4` |
| Pill radius | 8px | `radius-small` |
| Active pill | White fill + `shadow.sm` | Shadow skill |
| Active text | `bg.brand` #D9008D, SemiBold 600, 14sp | `type.label.md` |
| Inactive text | `text.subtle` #5C5570, Regular 400, 14sp | `type.body.md` |
| Disabled | 40% opacity | Color skill |
| Badge (active) | `bg.brand` fill, white text, `radius-tiny` (4px) | — |
| Badge (inactive) | `bg.muted` fill, secondary text, `radius-tiny` (4px) | — |
| Item spacing | 0 (items fill equally) | — |
| Tab gap (label-badge) | 4px | `space-4` |
| Variants | Count: 2 / 3 / 4 | — |
| Max options | 4 | Beyond 4 = rethink IA |

**Use when:**
- Switching between fundamentally different datasets (not filtering same one)
- Content structure may change between tabs
- Options are mutually exclusive modes
- Examples: मेरे टिकट / टीम के टिकट, आज / इस हफ्ते / इस महीने

**Never use for:**
- Status filtering (सभी/पेंडिंग/बंद) → use Underline
- More than 4 options → doesn't scale
- Options that can be combined → use Chips

---

### Level 2: Underline Filter — Single Select

**Question:** "Which subset of this data do I want to see?"

| Property | Value | Token |
|----------|-------|-------|
| Height | 48px | `space-48` (touch target) |
| Active indicator | 2px, `bg.brand`, pill ends | `stroke-medium`, `radius-full` |
| Bottom divider | 1px, `stroke.default` | `stroke-small` |
| Active text | `bg.brand` #D9008D, SemiBold 600, 14sp | `type.label.md` |
| Inactive text | `text.subtle` #5C5570, Regular 400, 14sp | `type.body.md` |
| Disabled | 40% opacity | Color skill |
| Shadow | `shadow.none` — filters are flat | Shadow skill |
| Badge | Same as Pill Tab badges | — |
| Label-badge gap | 4px | `space-4` |
| Variants | Count: 3 / 4, More: On / Off | — |
| Fixed (3-4 filters) | Equal width, all visible | No scroll needed |
| Scrollable (5-6 filters) | Hug content, horizontal scroll | Clips at 360px, peek indicates scroll |
| Overflow (7+ filters) | 3-4 visible + More button | More → bottom sheet with Radio + CTA |

**More Button (overflow):**
- 48x48px touch target (`space-48`)
- Placed at end of filter bar
- Opens bottom sheet with remaining filters
- Bottom sheet: list with Radio select + Primary CTA to apply
- CTA disabled until a selection is made

**Scrollable (5+ items):**
- Handled in code — items hug content width, horizontally scrollable
- Active indicator travels with scroll

**Use when:**
- Narrowing down the same list — data source doesn't change
- Only one filter active at a time
- Options represent status/state/category of same items
- 3-6 visible options
- Examples: सभी / पेंडिंग / बंद / एक्सपायर्ड

**Never use for:**
- Context switching (my vs team) → use Pill
- Multi-select (installation + repair) → use Chips
- More than ~8 total options → bad IA, restructure

---

### Level 3: Chips — Multi-Select Toggle

**Question:** "Which attributes do I want to include/combine?"

| Property | Value | Token |
|----------|-------|-------|
| Height | 36px | — |
| Padding | 12px horizontal | `space-12` |
| Gap (internal) | 4px | `space-4` |
| Radius | 8px | `radius-small` |
| Unselected | 1px border, `stroke.subtle` #E8E4F0 | `stroke-small` |
| Unselected text | `text.default`, Regular 400, 14sp | `type.body.md` |
| Selected fill | `bg.selected` #FFCCED | `wiom-design-foundations` |
| Selected text | `text.selected`, SemiBold 600, 14sp | `type.label.md` |
| Selected border | NONE — filled-state rule | `wiom-design-foundations` |
| Selected trailing icon | ✕ close only · `icon.brand` | — |
| Disabled | 40% opacity on base color (foundations §10) | `wiom-design-foundations` |
| Shadow | `shadow.none` — chips are flat | `wiom-design-foundations` |
| Chip Row gap | 8px | `space-8` |
| Chip Row padding | 16px horizontal | `space-16` |
| Chip Row wrap | Supported, 8px vertical gap | `space-8` |

**Use when:**
- Multiple criteria can be combined — not pick one
- Options are additive — selecting more shows more
- Used for attribute/type filtering, not status
- Examples: इंस्टॉलेशन + रिपेयर, WiFi + Fiber

**Never use for:**
- Mutually exclusive choices → use Underline
- Context switching → use Pill
- Navigation between pages

---

## 2. Decision Tree — Which Component?

```
New filter/tab needed on a screen. Ask:

1. Does selecting this option change the ENTIRE dataset
   (different source, different structure)?
   → YES: Pill Tab
   → NO: Continue

2. Can only ONE option be active at a time
   (statuses, states, single-category view)?
   → YES: Underline Filter
   → NO: Continue

3. Can MULTIPLE options be active simultaneously
   (combining attributes, additive filtering)?
   → YES: Chips
   → NO: Rethink — it should fit one of the above
```

---

## 3. Combination Rules

### Valid Combinations

| Combination | When | Layout |
|---|---|---|
| **Pill + Underline** | Two-level data split (whose + which status) | Pill above, Underline below |
| **Pill + Chips** | Mode switch + attribute filter (rare) | Pill above, Chips in content |
| **Underline + Chips** | Status + type filter (rare) | Underline below app bar, Chips in content |

### Invalid Combinations

| Combination | Why Not |
|---|---|
| **Pill + Underline + Chips** | 3 filter levels = too complex. Use section headers for 3rd level. |
| **Two Pills** | Only one context switch per screen |
| **Two Underlines** | Combine into one, or use Chips for second dimension |
| **Pill below Underline** | Broader decision (Pill) always above narrower (Underline). Never reverse. |

---

## 4. Partner App Specific Rules

1. **Filters always visible** → if a screen has filters, show them regardless of list length. No threshold logic. Partners learn the filter is always in the same spot.
2. **Filter used < 3 times per session** → make it a bottom sheet action, not a persistent filter bar.
3. **Only 2 options** → consider if you even need a component. Maybe show both inline.
4. **Pill is always optional** → if screen shows only one dataset (no "mine vs team" split), skip it. Don't add a Pill just to have one.
5. **Max 2 filter levels on screen** → if 3rd level needed, handle with section headers in content, not more UI controls.
6. **Underline More button** → opens bottom sheet with Radio select. CTA disabled until selection made. Single select only.

---

## 5. Figma Components

All components live on "Sandbox - SAAD" page in "Tab, Filters and Chip filters" section.

| Component | Type | Variants |
|---|---|---|
| Pill Tab Item | Component Set | State (Active/Inactive/Disabled) × Badge (On/Off) = 6 |
| Pill Track | Component Set | Count (2/3/4) = 3 |
| Underline Filter Item | Component Set | State (Active/Inactive/Disabled) × Badge (On/Off) = 6 |
| Underline Filter Bar | Component Set | Fixed (3/4), Scrollable (5/6), Scrollable+More (5) = 6 |
| Filter More Button | Component | Single |
| Chip | Component Set | State (Unselected/Selected/Disabled) = 3 |
| Chip Row | Component | Wrapping container |

**31 Figma Variables bound:**
- Color: 17 variables
- Spacing: 7 variables
- Radius: 7 variables

---

## 6. Compose Implementation

```kotlin
// Level 1: Pill Tab
WiomPillTrack(
    tabs = listOf("मेरे टिकट", "टीम के टिकट"),
    selectedIndex = 0,
    onTabSelected = { index -> }
)

// Level 2: Underline Filter
WiomUnderlineFilterBar(
    filters = listOf("सभी", "पेंडिंग", "बंद", "एक्सपायर्ड"),
    selectedIndex = 0,
    onFilterSelected = { index -> },
    showMore = false,
    onMoreClick = { }
)

// Scrollable variant (5+ items)
WiomScrollableFilterBar(
    filters = listOf("सभी टिकट", "नए", "पेंडिंग", "प्रोसेसिंग", "पूरे हुए", "रद्द हुए"),
    selectedIndex = 0,
    onFilterSelected = { index -> }
)

// Level 3: Chips
WiomChipRow(
    chips = listOf(
        ChipData("इंस्टॉलेशन", selected = true),
        ChipData("रिपेयर", selected = true),
        ChipData("रिचार्ज", selected = false),
        ChipData("अपग्रेड", selected = false),
    ),
    onChipToggle = { index, selected -> }
)

// Combined: Pill + Underline on same screen
Column {
    WiomPillTrack(
        tabs = listOf("मेरे टिकट", "टीम के टिकट"),
        selectedIndex = selectedMode,
        onTabSelected = { selectedMode = it }
    )
    WiomUnderlineFilterBar(
        filters = listOf("सभी", "पेंडिंग", "बंद", "एक्सपायर्ड"),
        selectedIndex = selectedFilter,
        onFilterSelected = { selectedFilter = it }
    )
    // Content...
}
```

---

## 7. Strict Rules

### DO
- Use the decision tree to pick the right component
- Pill above Underline — never reverse
- Max 2 filter levels per screen
- Underline More button opens bottom sheet with Radio + CTA
- Chips: no border on selected (filled state rule)
- All flat — `shadow.none` except Pill active (`shadow.sm`)
- Use Figma variables for all token values

### DO NOT
- Do NOT use Pill for status filtering — that's Underline
- Do NOT use Underline for multi-select — that's Chips
- Do NOT stack 3 filter components on one screen
- Do NOT use two Pills or two Underlines on same screen
- Do NOT add Pill "just to have one" — it's optional
- Do NOT use Chips for mutually exclusive choices
- Do NOT exceed 4 Pill options or ~8 Underline options
- Do NOT hardcode colors — use Figma variables / token names

### WHEN IN DOUBT
Ask: "Can the user select multiple?" → Yes = Chips, No = "Does it change the dataset?" → Yes = Pill, No = Underline.

---

## See also

- `wiom-design-foundations` — token values, selected-state tokens (`bg.selected`, `text.selected`)
- `wiom-compose-layout-rules` — Compose structural rules
- `wiom-badge` — badge-in-tab pattern
- `wiom-bottomsheet` — the overflow "More" filter sheet
- `wiom-selection-controls` — radio rows inside the overflow sheet
- `wiom-cta` — Apply / Reset CTAs inside the overflow sheet
