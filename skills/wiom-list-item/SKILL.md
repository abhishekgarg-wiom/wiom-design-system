---
name: list-item
description: Wiom List Item component spec, usage patterns, and Figma↔Kotlin code mapping. Use this skill EVERY TIME you work with list rows, settings lists, selection lists, toggle lists, or any Figma design containing list-row patterns from the Wiom DS - Next file. Trigger when user mentions "list item", "list row", "settings list", "selection list", "toggle list", "list cell", or shares a Figma design with stacked rows. Also trigger when implementing list UI in Kotlin/Compose for Wiom Android app.
---

# Wiom List Item — Component Skill

The unified List Item is the single component used for every list row in the Wiom app — navigation, settings, selection, toggles, and information display. Built as a 4-variant set with **`Type` dropdown** (Default · Checkbox · Radio · Switch) plus 5 booleans + 3 text props.

## Where it lives

- **Figma file:** Wiom DS - Next (fileKey `ZPErh7YU7omdbyy2o268Qo`)
- **Page:** `Saurav / List items` ← **ALL List Item changes happen on this page. Never on `Components / Core`, `Design Garage`, or any other page.**
- **Component set ID:** `1655:487` (4 variants — `Type=Default | Checkbox | Radio | Switch`)
- **Documentation root frame ID:** `1557:340` on the same page — full anatomy, examples, usage guidelines

> **Rule (hard):** When asked to modify, audit, or extend the List Item, always navigate to `Saurav / List items` page first (`await figma.setCurrentPageAsync(...)`). Do not create any new List Item variant or copy on any other page.

## Why it's structured this way

The previous version had **288 variants** across 3 sets (Content / Selection / Toggle). This was replaced by a single component with boolean toggles + instance swaps because:

- Variant explosion makes Figma sluggish
- Designers couldn't remember which variant served which use case
- Boolean properties are simpler to reason about and compose
- Pressed/Disabled states are visual layers, not structural changes

## Component anatomy

```
┌─────────────────────────────────────────────────────┐
│  [Leading]   [Primary text         ]   [Meta] [▶]   │
│              [Secondary text       ]                 │
└─────────────────────────────────────────────────────┘
   40×40       fills width             auto    20×20
```

| Slot | Default content | Size |
|---|---|---|
| Leading | `settings` icon (24×24 inside 40×40 container) | 40×40 |
| Primary | "Primary text" — `type/body/lg` (16/24 Regular) | fills width |
| Secondary | "Supporting text" — `type/body/sm` (12/16 Regular) | fills width |
| Trailing Meta | "Meta" — `type/body/sm` (12/16 Regular) | hugs |
| Trailing Icon | `chevron_right` (20×20) | 20×20 |

**Padding:** 16h × 12v · **Min height:** 48 (Action touch target) · **Background:** transparent (adopts parent surface)

## All component properties

| Property | Type | Default | What it does |
|---|---|---|---|
| `Type` | **Variant** | `Default` | Dropdown — Default (icon + chevron) · Checkbox · Radio · Switch (icon + toggle) |
| `Pressed` | Boolean | `false` | Toggles `surface/subtle` background overlay (tap feedback) |
| `Selected` | Boolean | `false` | **(v3.1+)** Toggles `surface/selected` background overlay. In v3.2 the brand dot was removed — the bg overlay is the sole selection signal. Skipped for Switch type. |
| `Disabled` | Boolean | `false` | Toggles `surface/base @ 62%` overlay on top — fades content to ~38% |
| `Has Leading` | Boolean | `true` | Show/hide the leading slot (default-type only — for Checkbox/Radio the leading IS the control) |
| `Has Trailing Icon` | Boolean | `true` | **(v3.1)** Show/hide the trailing chevron (Default type only — Switch's trailing must stay visible). Set `false` for menu rows and status rows. |
| `Has Secondary` | Boolean | `false` | Show/hide the secondary text line |
| `Has Trailing Meta` | Boolean | `false` | Show/hide trailing meta text |
| `Primary Text` | Text | `"Primary text"` | Per-instance primary text |
| `Secondary Text` | Text | `"Supporting text"` | Per-instance secondary text |
| `Trailing Meta Text` | Text | `"Meta"` | Per-instance meta text |

**Hybrid model:** 1 variant (Type, 4 options) for structural archetype + booleans for state/visibility.

## Type variant — what each does

| Type | Leading | Trailing | Use case |
|---|---|---|---|
| `Default` | settings icon (right-click swap to any icon) | chevron_right | Navigation, settings, info rows |
| `Checkbox` | Checkbox component (Selection Off/On controlled by nested instance) | none | Multi-select lists |
| `Radio` | Radio component (Selection Off/On controlled by nested instance) | none | Single-select lists |
| `Switch` | settings icon (swap to any) | Switch component (Selection Off/On controlled by nested instance) | Settings toggles (notifications, dark mode, auto-pay) |

For Checkbox / Radio / Switch types, the nested control's properties show in the List Item instance's properties panel — designers toggle them directly without right-click swap. Specifically:

> **Plugin API note:** When setting nested control properties programmatically, you must navigate to the nested instance and call `setProperties` on IT — the parent List Item's `setProperties` does NOT surface nested variant properties.
> ```js
> // ✓ correct
> const nestedSwitch = listItemInst.findOne(n => n.type === 'INSTANCE' && n.name === 'Switch');
> nestedSwitch.setProperties({ Selection: 'On' });
> // Same pattern for nested Checkbox / Radio
> ```


| Type | Bubbled-up properties (visible in instance) |
|---|---|
| `Checkbox` | `Selection` (Unchecked/Checked) · `State` (Default/Hover/Focused/Disabled/Error) · `Label='' & Show Helper Text=false` (locked, hides label) |
| `Radio` | `Selection` (Unselected/Selected) · `State` (Default/Disabled/Error) · `Show Label=false & Show Helper Text=false` (locked) |
| `Switch` | `Selection` (Off/On) · `State` (Default/Disabled) · `Label='' & Show Helper Text=false` (locked) |

## Slot composition (right-click swap for icons only)

Default-type leading icon and chevron trailing are right-click swappable to any icon from the library:

| Use case | Type | Leading swap | Trailing swap |
|---|---|---|---|
| Navigation row | Default | (default settings icon) | (default chevron) |
| Settings row | Default | category icon (`payments`, `bolt`, etc.) | (default chevron) |
| Status row | Default | status icon (`check_circle`, `error`, etc.) | `Has Trailing Icon = false` to hide chevron |
| Profile/contact | Default | Avatar (right-click swap) | (default chevron) |
| Selection row (multi) | Checkbox | (Checkbox is the leading) | n/a |
| Selection row (single) | Radio | (Radio is the leading) | n/a |
| Settings toggle | Switch | category icon (right-click swap) | (Switch is the trailing) |
| Dropdown menu row (single-select) | Default | (icon optional) | `Has Trailing Icon = false` + `Selected = true` for the chosen row |
| Dropdown menu row (multi-select) | Checkbox | (Checkbox is the leading) | n/a |
| Dropdown menu row (action) | Default | (no leading) | `Has Trailing Icon = false` |

## Common usage patterns

### Settings list
```
List Item × 4 in a card with 1px inset dividers
[Account]            ▶
[Notifications]      ▶
[Language]    English ▶    ← Has Trailing Meta = true
[Privacy]            ▶
```

### Status list
```
List Item × 3 in a card
[Storage]     12.4 GB ▶
[Plan]        28 days ▶
[Build]       v3.4.1     ← Has Trailing Icon = false
```

### Two-line contacts
```
[Avatar] Saurav Gupta             ▶    ← Has Secondary = true
         +91 98xxx xxxxx
```

### Toggle settings (Type=Switch)
```
[Bell] Notifications  [●━━]   ← Type=Switch, Selection=Off/On per row
[Eye]  Auto-play     [━━○]
```

### Multi-select list (Type=Checkbox)
```
[☑] Save my login
[☐] Email me updates
[☑] Auto-pay enabled
```

### Single-select list (Type=Radio)
```
[●] English
[○] हिन्दी
[○] తెలుగు
```

## Figma → Kotlin / Compose code mapping

The `Type` variant maps to function variants in code:

| Figma Type | Compose function |
|---|---|
| `Default` | `WiomListItem(...)` (icon + chevron) |
| `Checkbox` | `WiomListItemCheckbox(checked, onCheckedChange, ...)` (replaces leading) |
| `Radio` | `WiomListItemRadio(selected, onSelected, ...)` (replaces leading) |
| `Switch` | `WiomListItemSwitch(checked, onCheckedChange, leadingIcon, ...)` (replaces trailing) |

Base implementation:

```kotlin
@Composable
fun WiomListItem(
    primary: String,
    secondary: String? = null,
    leadingIcon: Painter? = painterResource(R.drawable.rounded_settings),
    trailingIcon: Painter? = painterResource(R.drawable.rounded_chevron_right),
    trailingMeta: String? = null,
    pressed: Boolean = false,
    disabled: Boolean = false,
    onClick: (() -> Unit)? = null,
) {
    val bg = when {
        pressed -> WiomColor.surfaceSubtle
        else -> Color.Transparent
    }
    val contentAlpha = if (disabled) 0.38f else 1f
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .heightIn(min = 48.dp)
            .background(bg)
            .then(if (onClick != null && !disabled) Modifier.clickable { onClick() } else Modifier)
            .padding(horizontal = 16.dp, vertical = 12.dp)
            .alpha(contentAlpha),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(12.dp),
    ) {
        leadingIcon?.let {
            Box(Modifier.size(40.dp), contentAlignment = Alignment.Center) {
                Icon(it, null, modifier = Modifier.size(WiomIcon.MD), tint = WiomColor.textSecondary)
            }
        }
        Column(Modifier.weight(1f), verticalArrangement = Arrangement.spacedBy(4.dp)) {
            Text(primary, style = WiomType.bodyL, color = WiomColor.textPrimary)
            secondary?.let { Text(it, style = WiomType.bodyS, color = WiomColor.textSecondary) }
        }
        if (trailingMeta != null || trailingIcon != null) {
            Row(verticalAlignment = Alignment.CenterVertically, horizontalArrangement = Arrangement.spacedBy(8.dp)) {
                trailingMeta?.let { Text(it, style = WiomType.bodyS, color = WiomColor.textSecondary) }
                trailingIcon?.let { Icon(it, null, modifier = Modifier.size(20.dp), tint = WiomColor.textSecondary) }
            }
        }
    }
}
```

## Token references (always use these, never hardcode)

| What | Token | Value |
|---|---|---|
| Horizontal padding | `space/16` | 16dp |
| Vertical padding | `space/12` | 12dp |
| Leading→content gap | `space/12` | 12dp |
| Primary→secondary gap | `space/4` | 4dp |
| Trailing meta→icon gap | `space/8` | 8dp |
| Min height | — | 48dp |
| Primary text | `type/body/lg` + `color/text/primary` | 16/24 Regular, #161021 |
| Secondary text | `type/body/sm` + `color/text/secondary` | 12/16 Regular, #5C5570 |
| Pressed bg | `color/surface/subtle` | #F1EDF7 |
| Disabled overlay | `color/surface/base @ 62%` | #FAF9FC at 0.62 |

## Composition rules (when to use, when not to)

### ✓ Use List Item for
- Stacked navigation rows (settings, account, etc.)
- Selection lists with checkbox/radio leading
- Status/info rows with trailing meta
- Contact/profile rows with avatar leading
- Settings toggles (with trailing toggle swap)

### ✗ Don't use List Item for
- Tappable cards with rich content → use Card component
- Marketing/feature blocks → use Card with body text
- Form input rows → use Input Field component
- Tab bar items → use Pill Tab Item or Underline Filter
- Dense data tables → use Table component (when built)

### Layout rules when stacking
- Stack inside a card with `radius/large` (16dp) corners and `surface/subtle` background
- Use **inset 1px dividers** between rows (16dp inset on both sides — match card padding)
- Don't full-bleed dividers inside a card
- Card padding: 4dp top/bottom, 0 left/right (rows have their own 16dp horizontal padding)

## Don'ts (component-specific)

| Don't | Why |
|---|---|
| Enable Pressed AND Disabled together | Visual conflict, semantic nonsense |
| Override the bound padding per instance | Breaks tokenization — change `space/16`/`space/12` instead |
| Reduce min height below 48 | WCAG action touch target violation |
| Hardcode font properties | Text styles `type/body/lg` and `type/body/sm` are bound — edit the style |
| Hardcode colors | Color variables are bound — edit the variable, all instances update |
| Use for >2 lines of text | If you need 3+ lines, you're describing a card |

## Memory dependencies

This skill works alongside:
- **wiom-design-foundations** — color, typography, spacing, radius, stroke, shadow tokens
- **wiom-icon-library** ([memory file](../../projects/C--Users-Saurav-Gupta/memory/wiom-icon-library.md)) — ~3,513 Material Symbols Rounded icons. Naming: flat snake_case = Material Symbols identifier = `R.drawable.rounded_<name>` in Kotlin.

## When to read this file

- Before building a Figma screen that contains list rows (use the unified component, don't build new ones)
- Before implementing a list UI in Kotlin/Compose
- When the user asks "how do I make a settings list / toggle list / selection list"
- When auditing an existing Figma design to ensure list rows use the unified component (not detached or duplicate)
- When the user asks to extend or modify the List Item component

## Outstanding work tracked in memory

- [ ] Publish library so INSTANCE_SWAP for leading/trailing icon shows in properties panel as a dropdown (currently right-click swap only)
- [ ] Delete legacy 3 sets on `Design Garage - Saurav` page (1078:859, 1085:602, 1088:314) — superseded by `1655:487`
- [ ] Apply same hybrid pattern (Type variant + booleans) to CTA component (currently 56 variants)
- [ ] Decide on row height: currently 64 (40dp leading container + 24dp padding). Material standard is 48 (24dp leading icon + 24dp padding) — pending product decision
- [ ] Audit Sandbox - SAAD page for stale duplicates of components

## Change history

- **v3.2 (2026-04-15):** Removed the brand dot from Default+Selected — selection is signalled by the `surface/selected` bg overlay alone. Simpler, avoids competing with trailing chevron when a row has both.
- **v3.1 (2026-04-15):** Added `Selected` boolean (full-frame `surface/selected` overlay on Default/Checkbox/Radio + brand dot 8×8 in trailing slot of Default type; skipped on Switch). Added `Has Trailing Icon` boolean (default true) to hide the chevron — wired on Default variant only because Switch's trailing slot contains the toggle and must stay visible. Both additions enable List Item to be reused as the bottom-sheet menu row (no separate Menu Item component needed).
- **v3 (2026-04-14):** Added `Type` variant (Default/Checkbox/Radio/Switch). Each non-Default Type uses real component instances (Checkbox, Radio, Switch from their respective pages) with their `Selection` and `State` properties bubbled up to the List Item instance. Radio uses `Show Label = false` (added to source Radio component). Component set ID: `1655:487`.
- **v2:** Single COMPONENT (not a SET) with 0 variants — Pressed/Disabled as boolean overlays. Replaced by v3.
- **v1:** 6-variant set (State × Compact). Replaced by v2.
- **v0 (legacy):** 288 variants across 3 sets on `Design Garage - Saurav`. Superseded.
