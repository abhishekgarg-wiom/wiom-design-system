# Core Semantic Tokens

47 tokens for non-action UI: surfaces, text, borders, icons, overlays. For CTA/button colors see `actions.md`. For chat see `chat.md`.

## bg.* — backgrounds and fills (15 tokens)

Subtle-first ordering within each role.

### Neutrals
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.default` | #FAF9FC | Neutral_50 | Page bg, card, modal, input fill |
| `bg.subtle` | #F1EDF7 | Neutral_100 | Hover rows, section cards, shimmer |
| `bg.muted` | #E8E4F0 | Neutral_200 | Grouped sections, skeleton fills |
| `bg.disabled` | #D7D3E0 | Neutral_300 | Disabled control backgrounds |
| `bg.inverse` | #161021 | Neutral_950 | Dark hero, footer, dark sections |
| `bg.selected` | #FFCCED | Brand_200 | Selected tab bg, active chip, current row |

### Brand (non-action surfaces)
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.brand.subtle` | #FFE5F6 | Brand_100 | Soft brand surface · Secondary + Tertiary CTA pressed fill |
| `bg.brand.bold` | #443152 | Brand_900 | Premium surfaces, partner headers (not an action) |

### Status
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.positive.subtle` | #E1FAED | Positive_100 | Success banner, success card bg |
| `bg.positive` | #008043 | Positive_600 | Solid success fill (badge, chip) |
| `bg.critical.subtle` | #FFE5E7 | Critical_100 | Error banner, error card bg |
| `bg.warning.subtle` | #FFE9A1 | Warning_100 | Warning banner, warning card bg |
| `bg.warning` | #E2B203 | Warning_300 | Solid warning fill (badge, chip) |
| `bg.info.subtle` | #F1E5FF | Info_100 | Info banner, info card bg |
| `bg.info` | #6D17CE | Info_600 | Solid info fill (badge) |

**Note:** `bg.critical` (the solid red) lives in Actions, not here — it's exclusively for destructive CTAs.

### Skeleton / loading states

Compose shimmer and skeleton placeholders from two existing tokens — no dedicated skeleton tokens.

| Layer | Token | Hex |
|---|---|---|
| Placeholder block fill | `bg.muted` | #E8E4F0 (Neutral_200) |
| Shimmer highlight sweep | `bg.subtle` | #F1EDF7 (Neutral_100) |

The shimmer animates `bg.subtle` across the `bg.muted` fill at ~1.5s per cycle. No separate skeleton color family. This keeps loading states visually consistent with the rest of the neutral surface system.

```kotlin
Box(
    modifier = Modifier
        .height(16.dp)
        .fillMaxWidth()
        .background(WiomColors.bgMuted)         // placeholder fill
        .shimmer(
            highlightColor = WiomColors.bgSubtle,  // shimmer sweep
        )
)
```

---

## text.* — foreground text (11 tokens)

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `text.default` | #161021 | Neutral_950 | Headings, body, labels, **warning banner text** |
| `text.subtle` | #5C5570 | Neutral_700 | Descriptions, placeholders, metadata |
| `text.disabled` | #A7A1B2 | Neutral_400 | Disabled text (sub-AA intentional) |
| `text.inverse` | #FAF9FC | Neutral_50 | Text on dark neutral surfaces |
| `text.selected` | #D9008D | Brand_600 | Selected tab label, active chip, current nav |
| `text.positive` | #008043 | Positive_600 | Success text |
| `text.onPositive` | #FFFFFF | Gray_50 | Text on `bg.positive` (green badge) |
| `text.critical` | #D92130 | Critical_600 | Error text, required asterisk |
| `text.onCritical` | #FFFFFF | Gray_50 | Text on `bg.critical` (red button) |
| `text.onWarning` | #372902 | Warning_900 | Text on `bg.warning` (yellow badge) — **dark olive on gold** |
| `text.info` | #6D17CE | Info_600 | Info text |
| `text.onInfo` | #FFFFFF | Gray_50 | Text on `bg.info` (purple badge) |

**No `text.warning` token exists.** Warning text is always `text.default`.

### text.brand is in actions.md
`text.brand`, `text.brand.hover`, `text.onBrand`, `text.onBrandAccent` live in `actions.md` because they're interactive.

---

## stroke.* — borders, dividers, focus rings (11 tokens)

### Neutrals
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `stroke.default` | #E8E4F0 | Neutral_200 | Inner-card dividers, light separators |
| `stroke.subtle` | #D7D3E0 | Neutral_300 | Card outlines, input borders (rest) |
| `stroke.strong` | #473F55 | Neutral_800 | Strong control borders (checkbox, radio) |
| `stroke.selected` | #D9008D | Brand_600 | Selected tab, active chip border |

### Brand
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `stroke.brand` | #FFB2E4 | Brand_300 | Soft decorative brand borders |
| `stroke.brand.focus` | #D9008D | Brand_600 | Secondary CTA border + keyboard focus ring |

### Status
| Token | Hex | Primitive | Use |
|---|---|---|---|
| `stroke.positive` | #A5E5C6 | Positive_300 | Success card outline |
| `stroke.positive.focus` | #008043 | Positive_600 | Focus ring on positive elements |
| `stroke.critical` | #FFB3B9 | Critical_300 | Error outline, critical card border |
| `stroke.critical.focus` | #D92130 | Critical_600 | Invalid input, destructive focus |
| `stroke.warning` | #E2B203 | Warning_300 | Warning card border (inside `bg.warning.subtle` only) |
| `stroke.info` | #D6B2FF | Info_300 | Info card outline |
| `stroke.info.focus` | #6D17CE | Info_600 | Focus ring on info elements |

**Note the swap:** `stroke.brand` is the **soft** variant (Brand_300, for decorative containers), `stroke.brand.focus` is the **saturated** variant (Brand_600, for the Secondary CTA border and keyboard focus ring). Secondary CTA deliberately uses the focus hex as its rest border — the "focus" naming signals that the ring persists around the edge.

---

## icon.* — icon tints (9 tokens)

Wiom uses solid/filled icons. A filled icon fills its entire bounding area and looks visually heavier than a text glyph at the same hex. So icons step one lightness level lighter than their text equivalents — `icon.action` at Neutral_900 (not Neutral_950 like `text.default`); `icon.inverse` at #FAF9FC (not #FFFFFF like white label text). This calibration applies inside CTAs too — see `actions.md`.

Two-tier neutral split covers the light-surface case:

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `icon.nonAction` | #A7A1B2 | Neutral_400 | Decorative / non-actionable icons |
| `icon.action` | #352D42 | Neutral_900 | Actionable icons (back, close, menu, tappable) · also used inside Pre-booking CTAs |
| `icon.inverse` | #FAF9FC | Neutral_50 | Icons on dark neutral surfaces · also used inside Primary and Destructive CTAs |
| `icon.brand` | #D9008D | Brand_600 | Brand accent (selected check, focused chevron) |
| `icon.positive` | #008043 | Positive_600 | Success check, confirmation |
| `icon.critical` | #D92130 | Critical_600 | Error alerts |
| `icon.warning` | #E2B203 | Warning_300 | Warning indicator (inside `bg.warning.subtle`) · **also reused for star ratings** (gold is the industry convention) |
| `icon.info` | #6D17CE | Info_600 | Info indicators |
| `icon.disabled` | #D7D3E0 | Neutral_300 | Disabled icons (lighter than `icon.nonAction`) |

### icon.nonAction vs icon.action — when to use which

- `icon.nonAction`: chrome, navigation bar icons you can't tap, decorative icons, list-item leading icons that don't have their own tap target
- `icon.action`: close (✕), back (←), menu (☰), overflow (⋯), filter, any icon the user taps directly

If the icon is inside a tappable card/row but the icon itself isn't the tap target, use `icon.nonAction`. The whole-row tap is handled by the row.

### icon.warning is dangerous outside warning surface

`#E2B203` on `bg.default` is 1.6:1 — invisible as a solo icon, and fails graphics AA (3:1). The token is only valid inside `bg.warning.subtle` where the surrounding yellow surface and context carry the warning meaning.

**One explicit exception: star ratings.** A filled gold star has a recognizable shape that users identify regardless of contrast — a 1.6:1 gold star on white still reads as "a star, probably a rating" because the visual language is culturally baked in (Amazon, Google Play, Swiggy, Zomato all use the same gold-on-white pattern). Star ratings reference `icon.warning` deliberately, not accidentally. A lint rule should allow `icon.warning` inside a `RatingBar` or similar rating component; flag it everywhere else.

---

## overlay.* — scrims (1 token)

| Token | Value | Use |
|---|---|---|
| `overlay.scrim` | rgba(22, 16, 33, 0.50) | Modal backdrop, bottom sheet scrim |

Pair with `shadow.xl` for elevated dialogs.

---

## Form control states (checkbox, radio, input)

Small controls (~20dp) use `stroke.strong` (#473F55 ← Neutral_800) for their resting border — the heavier weight keeps them visible at small sizes. Three states change the fill or stroke:

| State | Checkbox/radio fill | Checkbox/radio stroke | Input stroke |
|---|---|---|---|
| Unchecked (rest) | transparent | `stroke.strong` #473F55 | `stroke.subtle` #D7D3E0 |
| Checked | `bg.brand` #D9008D | (no stroke — fill replaces it) | n/a |
| Focused (keyboard) | (unchanged) | `stroke.brand.focus` #D9008D ring outside | `stroke.brand.focus` #D9008D |
| **Checked + in error** | `bg.critical` #D92130 | (no stroke) | n/a |
| Error (unchecked) | transparent | `stroke.critical.focus` #D92130 | `stroke.critical.focus` #D92130 |
| Disabled | `bg.disabled` #D7D3E0 | `bg.disabled` #D7D3E0 | `bg.disabled` #D7D3E0 |

**The error-checkbox rule:** when a checkbox or radio is checked AND the form has a validation error, its fill switches from `bg.brand` to `bg.critical`. This signals "this selection is causing the error" more clearly than a red ring around a pink fill.

```kotlin
// Error state on a checked checkbox
Checkbox(
    checked = true,
    onCheckedChange = { /* ... */ },
    colors = CheckboxDefaults.colors(
        checkedColor = if (hasError) WiomColors.bgCritical else WiomColors.bgBrand,
        uncheckedColor = if (hasError) WiomColors.strokeCriticalFocus else WiomColors.strokeStrong,
        checkmarkColor = WiomColors.textOnBrand,
    ),
)
```

Text labels next to the control stay `text.default` in all states — the control carries the error signal, not the label.

---
