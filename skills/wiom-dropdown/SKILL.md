---
name: wiom-dropdown
description: Wiom Dropdown component spec, usage patterns, and Figma↔Kotlin code mapping. Use this skill EVERY TIME you work with select pickers, multi-select fields, action menus, popovers, language pickers, sort/filter dropdowns, plan selectors, or any Figma design containing a closed field with chevron + list of options. Trigger when user mentions "dropdown", "select", "picker", "menu", "combobox", "action menu", "options menu", or shares a Figma design with a chevron-down field. Also trigger when implementing dropdown UI in Kotlin/Compose for Wiom Android app.
---

# Wiom Dropdown — Component Skill

One unified component covering both the closed trigger and the open menu. The `State` variant toggles between Collapsed (Default / Filled / Error / Disabled) and Expanded (menu visible beneath the field). Menu rows are List Item instances — no separate Menu Item component.

## Where it lives

- **Figma file:** Wiom DS - Next (fileKey `ZPErh7YU7omdbyy2o268Qo`)
- **Dropdown page:** `Saurav / Dropdown` (`1803:476`) ← **ALL Dropdown changes happen on this page**
  - `Dropdown` set ID `1821:137` (15 variants — Type × State, with Expanded variants containing a built-in menu)
  - Documentation frame: `1969:157`
- **List Item page:** `Saurav / List items` (`1554:1867`) ← **ALL List Item changes happen on this page**
  - List Item set ID `1655:487` (4 Type variants, v3.1 with Selected and Has Trailing Icon booleans)

> **Rules (hard):**
> - Field changes → `await figma.setCurrentPageAsync(<Saurav / Dropdown>)`
> - List Item changes (including `Selected` and `Has Trailing Icon` booleans) → `await figma.setCurrentPageAsync(<Saurav / List items>)`
> - Never duplicate Menu Item as a separate component again — reuse List Item.

## Why this structure

| Decision | Rationale |
|---|---|
| One unified component (not Field + Menu separately) | The Expanded state IS just Collapsed + menu beneath. A single component lets designers preview both states from one library entry and avoids "which Menu pairs with which Field" confusion. |
| Type×State variants (15 = 3×5) | State changes need color + structure changes — booleans only drive visibility. Mirrors existing `Field / Input` pattern. |
| Booleans for slot visibility (Has Label / Has Helper Text / Has Leading Icon) | Slots are pure visibility toggles → booleans avoid variant explosion. Same hybrid as List Item. |
| **Reuse List Item for menu rows inside Expanded** | List Item already has Type=Default/Checkbox/Radio with the same nested-control bubbling. Adding `Selected` (bg + dot) and `Has Trailing Icon` booleans was cheaper than building a parallel component. |
| Built-in menu in Expanded variants (4 rows per Type) | Designers get a working preview with one variant pick. To customise (more rows, different items), detach or override per-instance. |

## Anatomy

### Collapsed (Default / Filled / Error / Disabled)
```
[Label]                              ← optional, type/label/md
┌─────────────────────────────────┐
│ [icon] Value                 [▾] │ ← 48 tall, padding 16h × 12v
└─────────────────────────────────┘
[Helper text]                        ← optional, type/meta/xs
```

### Expanded (= Collapsed + Menu beneath)
```
[Label]
┌─────────────────────────────────┐  brand border, chevron up
│ [icon] Value                 [▴] │
└─────────────────────────────────┘
[Helper text]
┌─────────────────────────────────┐  surface/base, shadow/lg
│ [leading]  Item text  [meta]  ●  │  ← List Item (Selected=true)
│ ───────────────────────────────  │
│ [leading]  Item text             │  ← List Item (Selected=false)
│ ...                              │
└─────────────────────────────────┘
```

**Width:** 328 default · **Field height:** 48 · **Border radius:** `radius/medium` (12) · **Field→menu gap:** 8dp

| Slot | Default | Token |
|---|---|---|
| Label | "Label" | `type/label/md` + `color/text/primary` |
| Leading icon | hidden (toggle Has Leading Icon) | 20×20 inside field |
| Value | "Select an option" / "Selected value" | `type/body/lg` |
| Trailing chevron | ▾ (rotates to ▴ when Open) | 20×20, color follows state |
| Helper | "Helper text" / "Please choose an option" (Error) | `type/meta/xs` |

## Properties

| Property | Type | Default | What it does |
|---|---|---|---|
| `Type` | **Variant** | `Single Select` | Single Select · Multi Select · Action Menu (changes default placeholder + value text + the rows shown when Expanded) |
| `State` | **Variant** | `Default` | Default · Filled · Error · Disabled · Expanded (drives bg, border, chevron color, helper color, menu visibility) |
| `Has Label` | Boolean | `true` | Show/hide label above |
| `Has Helper Text` | Boolean | `true` | Show/hide helper below |
| `Has Leading Icon` | Boolean | `false` | Show/hide leading icon (right-click swap to any icon) |
| `Label Text` | Text | `"Label"` | Per-instance label |
| `Value Text` | Text | `"Selected value"` | Per-instance value (used in Filled/Error states) |
| `Placeholder Text` | Text | `"Select an option"` | Per-instance placeholder (Default/Open states) |
| `Helper Text` | Text | `"Helper text"` | Per-instance helper / error text |

### State variant — visual rules

| State | Bg | Border | Chevron | Helper | Menu |
|---|---|---|---|---|---|
| Default | `color/surface/subtle` | `color/border/default` `stroke/small` (1px) | secondary | secondary | hidden |
| Filled | `color/surface/base` | `color/border/strong` `stroke/small` (1px) | secondary | secondary | hidden |
| Error | `color/surface/base` | `color/negative/primary` `stroke/medium` (2px) | negative | negative | hidden |
| Disabled | `color/surface/subtle` | `color/border/default` `stroke/small` (1px) | disabled | disabled | hidden |
| **Expanded** | `color/surface/base` | `color/brand/primary` `stroke/medium` (2px) | brand, rotated 180° | secondary | **visible (4 rows by default)** |

**Foundation alignment notes:**
- Disabled bg uses `surface/subtle` (per cross-foundation rule #10) — disabled-ness is signalled by text/icon colors, not by a separate disabled bg.
- Active border (Expanded) and Error border are `stroke/medium` (2px) — matches the `Field / Input` convention.
- Menu uses `shadow/md` (foundations default for dropdowns) with **no border** (rule #1: never both shadow and border).

## Menu rows — use the List Item component

For dropdown menu rows, instance the existing List Item (`1655:487`) and override these properties:

| Use case | List Item settings |
|---|---|
| Single Select picker | `Type=Default` · `Has Leading=false` · `Has Trailing Icon=false` · `Selected=true` for the chosen row (pink bg + brand dot) |
| Multi Select picker | `Type=Checkbox` · `Has Trailing Icon` doesn't apply · bubble nested Checkbox `Selection: Checked/Unchecked` |
| Single Select with explicit radio | `Type=Radio` · bubble nested Radio `Selection: Selected/Unselected` |
| Action menu (Edit / Duplicate / Share / Delete) | `Type=Default` · `Has Leading=false` · `Has Trailing Icon=false` · `Selected=false` for all |

> **Plugin API — bubbling nested Checkbox/Radio Selection:**
> ```js
> const inst = listItemSrc.createInstance();
> inst.setProperties({ 'Selected#1945:0': true, 'Has Trailing Icon#1951:0': false });
> // For Checkbox/Radio types, also bubble the nested control's Selection
> const nested = inst.findOne(n => n.type === 'INSTANCE' && (n.name === 'Checkbox' || n.name === 'Radio'));
> nested?.setProperties({ Selection: 'Checked' /* or 'Selected' */ });
> ```

## Menu container (built into Expanded variants)

The menu sits inside the 3 Expanded variants (one per Type). It's a card with `surface/base` bg, `radius/medium`, `stroke/small` border, `shadow/lg` effect, 4dp top/bottom padding. Inside: 4 List Item instances with 1px inset dividers (16dp inset).

Default contents per Type:
| Type | Row component | Sample items |
|---|---|---|
| Single Select Expanded | List Item Type=Default, `Has Leading=false`, `Has Trailing Icon=false` | English, Hindi (Selected=true), Telugu, Tamil |
| Multi Select Expanded | List Item Type=Checkbox | Push notifications (✓), Email updates, SMS alerts (✓), Weekly digest |
| Action Menu Expanded | List Item Type=Default, `Has Leading=false`, `Has Trailing Icon=false`, no Selected | Edit, Duplicate, Share, Delete |

To customise:
- Change item text per-instance via the List Item's `Primary Text` override
- Toggle Selected per-instance for picker rows
- Add/remove rows: detach the menu and edit; or detach the whole Dropdown instance

## Common usage patterns

### Single Select picker (closed)
```
Dropdown [Type=Single Select, State=Filled] "Language: English"
```

### Single Select picker (open)
```
Dropdown [Type=Single Select, State=Expanded] — auto-includes 4 menu rows
```

### Multi Select filter (closed)
```
Dropdown [Type=Multi Select, State=Filled] "Filters: Push, SMS (+1)"
```

### Action Menu (closed)
```
Dropdown [Type=Action Menu, State=Default] "Choose action"
```

### Inline filter row (sort)
```
Dropdown [Type=Single Select, Has Label=false, Has Helper Text=false] "Sort by: Most recent ▾"
```

## Figma → Kotlin / Compose code mapping

| Figma | Compose function |
|---|---|
| `Type=Single Select` | `WiomDropdown<T>(value, options, onValueChange, ...)` |
| `Type=Multi Select` | `WiomMultiDropdown<T>(values, options, onValuesChange, ...)` |
| `Type=Action Menu` | `WiomActionMenu(actions, ...)` (Material `DropdownMenu` style, no value) |
| Menu row (Default + Selected) | `WiomListItem(primary, selected = true, hasTrailingIcon = false)` |
| Menu row (Checkbox) | `WiomListItem(primary, type = ListItemType.Checkbox, checked = …)` |
| Menu row (Radio) | `WiomListItem(primary, type = ListItemType.Radio, selected = …)` |

Base implementation:

```kotlin
@Composable
fun <T> WiomDropdown(
    value: T?,
    options: List<DropdownOption<T>>,
    onValueChange: (T) -> Unit,
    label: String? = null,
    placeholder: String = "Select an option",
    helper: String? = null,
    leadingIcon: Painter? = null,
    isError: Boolean = false,
    enabled: Boolean = true,
) {
    var open by remember { mutableStateOf(false) }
    val state = when {
        !enabled -> FieldState.Disabled
        isError -> FieldState.Error
        open -> FieldState.Open
        value != null -> FieldState.Filled
        else -> FieldState.Default
    }
    Column(verticalArrangement = Arrangement.spacedBy(4.dp)) {
        label?.let {
            Text(it, style = WiomType.labelM,
                color = if (state == FieldState.Disabled) WiomColor.textDisabled else WiomColor.textPrimary)
        }
        Box {
            Row(
                Modifier
                    .fillMaxWidth()
                    .height(48.dp)
                    .clip(RoundedCornerShape(WiomRadius.medium))
                    .background(state.bg())
                    .border(WiomStroke.small, state.borderColor(), RoundedCornerShape(WiomRadius.medium))
                    .clickable(enabled = enabled) { open = !open }
                    .padding(horizontal = 16.dp, vertical = 12.dp),
                verticalAlignment = Alignment.CenterVertically,
                horizontalArrangement = Arrangement.spacedBy(8.dp),
            ) {
                leadingIcon?.let { Icon(it, null, Modifier.size(20.dp), tint = WiomColor.textSecondary) }
                Text(
                    text = value?.let { options.first { o -> o.value == it }.label } ?: placeholder,
                    style = WiomType.bodyL,
                    modifier = Modifier.weight(1f),
                    color = if (value != null) WiomColor.textPrimary else WiomColor.textSecondary,
                )
                Icon(
                    painterResource(R.drawable.rounded_keyboard_arrow_down),
                    null,
                    Modifier.size(20.dp).graphicsLayer { rotationZ = if (open) 180f else 0f },
                    tint = state.chevronColor(),
                )
            }
            DropdownMenu(expanded = open, onDismissRequest = { open = false }) {
                options.forEach { opt ->
                    // Reuse the same ListItem composable as everywhere else
                    WiomListItem(
                        primary = opt.label,
                        selected = opt.value == value,
                        hasTrailingIcon = false,
                        onClick = { onValueChange(opt.value); open = false },
                    )
                }
            }
        }
        helper?.let {
            Text(it, style = WiomType.metaXs,
                color = if (isError) WiomColor.negativePrimary else WiomColor.textSecondary)
        }
    }
}
```

## Token references (always use these, never hardcode)

| What | Token | Value |
|---|---|---|
| Field horizontal padding | `space/16` | 16dp |
| Field vertical padding | `space/12` | 12dp |
| Label→field gap | `space/4` | 4dp |
| Field→helper gap | `space/4` | 4dp |
| Field→menu gap (Expanded) | `space/8` | 8dp |
| Leading icon→value gap | `space/8` | 8dp |
| Field & menu radius | `radius/medium` | 12dp |
| Default / Filled / Disabled border | `stroke/small` | 1dp |
| Expanded / Error border | `stroke/medium` | 2dp |
| Field height | — | 48dp |
| Menu row height (List Item inside menu) | — | 48dp (denser than List Item's default 64dp — appropriate for popovers) |
| Default / Disabled bg | `color/surface/subtle` | #F1EDF7 |
| Filled / Error / Expanded bg | `color/surface/base` | #FAF9FC |
| Selected row bg (List Item Selected boolean) | `color/surface/selected` | #FFE5F6 |
| Selected dot (List Item Default + Selected) | `color/brand/primary` 8×8dp ellipse | #D9008D — design constant, not a token (foundations don't cover dot indicators) |
| Default border | `color/border/default` | #D7D3E0 |
| Expanded border | `color/brand/primary` | #D9008D |
| Error border / helper | `color/negative/primary` | #D92130 |
| Filled border | `color/border/strong` | #665E75 |
| Label text | `type/label/md` + `color/text/primary` | 14sp SemiBold, LH 20 |
| Value text | `type/body/md` + `color/text/primary` | 14sp Regular, LH 20 (matches `Field / Input` convention) |
| Helper / placeholder | `type/body/sm` + `color/text/secondary` | 12sp Regular, LH 16 (`meta/xs` is reserved for timestamps/badges only) |
| Menu shadow | `shadow/md` (effect style) | foundations default for dropdowns |

## Composition rules (when to use, when not to)

### ✓ Use Dropdown for
- Single-select picker with 4+ options (Language, Plan, Sort by)
- Multi-select filter (notifications, categories)
- Action menu / overflow menu (Edit / Duplicate / Share / Delete)
- Inline filter rows in tables / lists
- Form selects (state, country, type-of)

### ✗ Don't use Dropdown for
- 2–3 mutually exclusive options → use **Pill Tabs** or **Radio Group**
- Single binary toggle → use **Switch**
- Free-text input → use **Field / Input**
- Date/time selection → use a dedicated date picker (when built)
- Primary CTAs disguised as a menu → use **CTA**

### Layout rules when stacking menu rows
- Menu width: hug content for short labels (action menus) · match field width for selects with long labels
- Position: open below the field by default · flip above if not enough space
- Max menu height: 6 items visible (288dp) · scroll if longer
- 1px inset dividers between items, OR no dividers for tighter look (be consistent within a screen)
- Don't full-bleed dividers inside the menu card

## Don'ts (component-specific)

| Don't | Why |
|---|---|
| Create a separate Menu Item component | List Item already covers all the cases (Default/Checkbox/Radio + Selected + Has Trailing Icon=false). Duplicating wastes maintenance. |
| Leave the chevron on menu rows | Set `Has Trailing Icon=false` on List Item — chevron means "navigates", which menu rows don't |
| Enable Open and Disabled together | Visual conflict, semantic nonsense |
| Mix Checkbox and Radio rows in the same menu | Confuses single vs multi selection model |
| Hardcode menu width to field width | Long item labels get truncated — let menu hug |
| Override the bound padding per instance | Breaks tokenization — change `space/16`/`space/12` instead |
| Use Action Menu for primary CTAs | Use the CTA component for primary actions |
| Replace the chevron with a non-chevron icon | Chevron is the universal "this opens a menu" affordance |

## Memory dependencies

This skill works alongside:
- **wiom-list-item** — the actual menu row component. Includes `Selected` and `Has Trailing Icon` booleans (v3.1).
- **wiom-design-foundations** — color, typography, spacing, radius, stroke, shadow tokens
- **wiom-icon-library** — Material Symbols Rounded for leading icons (right-click swap)

## When to read this file

- Before building a Figma screen that contains a select / picker / dropdown / action menu
- Before implementing dropdown UI in Kotlin / Compose
- When the user asks "how do I make a single-select / multi-select / action menu"
- When auditing an existing Figma design to ensure dropdowns use Field + List Item rows
- When the user asks to extend or modify the Dropdown component

## Design decisions (foundations cross-check)

These are intentional choices made during the foundations audit on 2026-04-15:

| Decision | Rationale |
|---|---|
| **Action Menu shares Field shape with Single/Multi Select** | Foundations don't define an "Action Menu" trigger pattern. Treating it as a Field-with-no-value keeps visual consistency across all 3 Types. The Material/Apple "icon button + popover" pattern is a separate composition (use a CTA + manual menu) and isn't covered here. |
| **List Item primary text stays at `type/body/lg` (16sp)** | Inherits from List Item v3. Foundations don't explicitly cover list rows. 16sp matches Material/iOS convention. |
| **Menu row height inside Expanded = 48dp (List Item's compact form)** | Denser than List Item's default 64dp height. Standard for popovers. Achieved by keeping `Has Leading=false` on Single/Action menus (40dp leading container collapsed). |
| **Selected dot 8×8 brand ellipse — design constant, not a token** | Foundations cover icons (16/20/24/48dp) but not dot indicators. 8×8 reads as "selected" without competing with the 20dp chevron. |
| **Disabled field bg = `surface/subtle`** | Matches cross-foundation rule #10. Disabled-ness comes from `text/disabled` color on label, value, and helper — not from a separate bg. |

## Outstanding work

- [ ] Publish file as Figma library so INSTANCE_SWAP for leading icon shows in properties panel
- [ ] Add a Combobox variant (filterable/searchable) as a 4th Type — defer until a usage demands it
- [ ] Decide whether `Selected` on List Item should also tint the primary text brand colour (currently bg + dot only — text stays primary)

## Change history

- **v2.1 (2026-04-15):** Cross-foundation audit. Value text → `body.md` (was `body.lg`). Helper text → `body.sm` (was `meta.xs`, which is reserved for timestamps/badges). Expanded + Error borders → `stroke/medium` (2px) to match `Field / Input` active/error convention. Menu — dropped border (rule #1: never shadow + border) and downgraded `shadow/lg` → `shadow/md` (foundations default for dropdowns). Disabled field bg → `surface/subtle` (rule #10). Logged 5 design decisions for foundations gaps (Action Menu shape, list item type size, menu row height, dot indicator size, disabled bg signal).
- **v2 (2026-04-15):** Unified Field + Menu into a single `Dropdown` component (renamed from `Dropdown / Field`). State variant `Open` renamed to `Expanded`; Expanded variants now include a built-in menu of 4 List Item rows below the field. Standalone sample menus deleted (the Expanded variants are the samples). Documentation frame replaced.
- **v1.1 (2026-04-15):** Removed the duplicate `Dropdown / Menu Item` component. Menu rows now reuse List Item with the new `Selected` and `Has Trailing Icon` booleans (List Item v3.1). 4 sample menus rebuilt. Documentation frame replaced.
- **v1 (2026-04-14):** Initial build. `Dropdown / Field` (15 variants — Type=Single/Multi/Action × State=Default/Open/Filled/Error/Disabled) + `Dropdown / Menu Item` + 4 sample menus + documentation frame. All on `Saurav / Dropdown` page.
