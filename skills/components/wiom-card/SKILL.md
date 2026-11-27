---
name: wiom-card
description: >
  Wiom Card: a rounded container that groups related content, flat
  (border) or elevated (shadow), static, tappable or selectable. Use when
  building or reviewing a plan card, summary card, settings group or any
  grouped block in Compose for the Wiom Partner or Customer app. Not for
  list rows (wiom-list-item) or status callouts (wiom-inline-banner).
  Trigger on: Card, ElevatedCard, OutlinedCard, plan card, summary card,
  settings group, selectable card, grouped container.
---

# Wiom Card

A rounded container that groups related content. **Flat** (border) or **elevated** (shadow), never both. A card is static, tapped as one unit, or one choice in a selectable set. Same rules on iOS (`wiom-ios-card`), owner decision 27 Nov 2026.

## 1. Anatomy

| Slot | Value | Token |
|---|---|---|
| Fill | Page surface | `bg.default` |
| Fill (section card) | Grouped settings or section background | `bg.subtle` |
| Corner | 16 dp | `radius-large` |
| Border (flat) | 1 dp | `stroke-small` · `stroke.subtle` |
| Shadow (elevated) | Instead of the border | `shadow.md` |
| Padding | All sides | `space-16` |
| Title | Optional | `type.title.sm` · `text.default` |
| Body | Optional | `type.body.md` · `text.subtle` |
| Divider | Inset to the card padding | `stroke-small` · `stroke.default` |
| Gaps | Sections `space-16`; title to subtitle `space-8`; content to CTA `space-32` | |

Cards that host list rows have **0 padding**: the rows own it (`wiom-list-item`).

## 2. States

| State | Visual |
|---|---|
| Default | As anatomy |
| Pressed (tappable) | Ripple (`LocalIndication`) clipped to the card shape |
| Selected (selectable only) | Border `stroke-medium` (2 dp) · `stroke.selected`; fill unchanged; border fades on `WiomMotion.selection()` |
| Disabled | Content tokens swap to `text.disabled` / `icon.disabled`; never opacity |

## 3. Rules

1. **Shadow or border, never both** (foundations cross-rule 1).
2. **Full width** inside the screen gutter (16 dp); cards stack `space-16` apart.
3. **One tap target per card.** A tappable card holds no other buttons. If it needs a button, the card is static and the button is the target.
4. **Status meaning** goes in `wiom-inline-banner`, not in a tinted card.
5. Height comes from content; text wraps.

## 4. Compose

```kotlin
enum class WiomCardStyle { Flat, Elevated }

@Composable
fun WiomCard(
    style: WiomCardStyle = WiomCardStyle.Flat,
    selected: Boolean = false,
    onClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
    content: @Composable ColumnScope.() -> Unit,
) {
    val shape = RoundedCornerShape(Wiom.radius.large)
    val border by animateColorAsState(
        when {
            selected -> Wiom.stroke.selected
            style == WiomCardStyle.Flat -> Wiom.stroke.subtle
            else -> Wiom.stroke.selected.clear()
        },
        WiomMotion.selection(), label = "CardBorder",
    )
    Column(
        modifier = modifier
            .fillMaxWidth()
            .then(if (style == WiomCardStyle.Elevated) Modifier.shadow(elevation = 6.dp, shape = shape) else Modifier)   // shadow.md (0 2 6); Compose takes Dp only
            .clip(shape)
            .background(Wiom.bg.default)
            .border(if (selected) 2.dp else 1.dp, border, shape)
            .then(if (onClick != null) Modifier.clickable(onClick = onClick) else Modifier)
            .padding(Wiom.space.s16),
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s16),
        content = content,
    )
}
```

## 5. Anti-patterns

- ❌ Border and shadow together.
- ❌ A button inside a tappable card (two targets).
- ❌ Fixed card height, or `alpha()` for disabled.
- ❌ Tinting the card fill to show status; use `wiom-inline-banner`.

## 6. Open

- No Card component in Figma yet; this recipe comes from the foundations "Card (interactive, flat)" quick-build.
