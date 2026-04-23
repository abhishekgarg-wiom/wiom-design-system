---
name: wiom-pagination
description: Pagination components for Wiom DS — show users where they are within a multi-page, multi-step, or continuous-scroll view. Covers three components mapped to three semantics: `Pagination` (unified Dots-Simple / Dots-Expanded / Bars for discrete 2–6 slots with optional counter label), `Pagination / Counter` (prev/next chevrons + "N / Total" text for long paginated lists), and `Pagination / Scroll Indicator` (thin track+thumb for continuous horizontal scroll). Tells you which component to pick, how to configure it, and which Wiom tokens back each part. Pairs with wiom-design-foundations for token values.
---

# Wiom Pagination

Three components, three semantics. Pick the one that matches the **kind of progress** you are showing — not the one that happens to fit the empty space.

- **`Pagination`** — discrete pages / steps. 2–6 slots. One unified component, three visual styles (`Dots-Simple`, `Dots-Expanded`, `Bars`), with an optional counter label below.
- **`Pagination / Counter`** — "1 / 10" text with prev/next chevrons. For long paginated lists where swipe isn't available.
- **`Pagination / Scroll Indicator`** — thin rail + thumb. For continuous horizontal scroll, not discrete pages.

Always use the assembled component. Never draw pagination with manual rectangles or text.

---

## When to use which

| Pattern | Best for | Avoid for |
|---|---|---|
| `Pagination` · **Dots-Simple** | Onboarding carousels, 2–4 intro slides. Low visual weight. | Long paginated lists. Counts that change often. |
| `Pagination` · **Dots-Expanded** | Promo / banner carousels where the current item must stand out at a glance. | Very small or tight layouts where the pill adds clutter. |
| `Pagination` · **Bars** | Multi-step wizards, payment / recharge flows, story-style indicators. | Long lists. Continuous scroll. |
| `Pagination / Counter` | Paginated lists without swipe — bills, transactions, order history. | Step-by-step flows. 2–6 page carousels (use Dots/Bars). |
| `Pagination / Scroll Indicator` | Horizontal rails with continuous scroll — recommended plans, offers, product strips. | Discrete paged content. Step flows. |

Rule of thumb: **discrete + swipeable → Dots/Bars · discrete + tap-to-navigate → Counter · continuous → Scroll Indicator**.

---

## When NOT to show pagination at all

1. **Single page / single step.** Don't show "1 / 1". Hide the component entirely.
2. **Content fits on screen with no scroll.** Scroll Indicator with a non-overflowing rail is noise.
3. **Vertically scrolling feeds.** Use the scroll bar native to the platform, not Pagination.
4. **Inside a fixed-height list that paginates via infinite scroll.** Let the load indicator do the job.

---

## Pagination — slot count (2–6)

| Slot count | When | How |
|---|---|---|
| **3** | Sweet spot for onboarding. Aim here first. | `Show Slot 3=on`, `Show Slot 4=off`, `Show Slot 5=off`, `Show Slot 6=off` |
| **4** | Promo carousels, short wizards. | Turn on Slot 3 and 4. |
| **5** | Standard payment / recharge wizard. | Turn on Slots 3–5. |
| **6** | Hard cap. Beyond 6, switch to `Pagination / Counter`. | All `Show Slot` on. |
| **2** | Valid minimum. Only when you genuinely have two peer items. | All `Show Slot` off. |

Slot 1 and Slot 2 are always visible — they cannot be hidden. If you need more than 6, you have the wrong component; use `Pagination / Counter`.

---

## Pagination — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Style` | VARIANT | `Dots-Simple` | Visual style — `Dots-Simple`, `Dots-Expanded`, or `Bars`. |
| `Current Page` | VARIANT | `1` | Which slot is active (1–6). For `Bars`, Slots 1..N are all filled to show accumulated progress. |
| `Show Slot 3` | BOOLEAN | true | Reveal slot 3. Off → 2-slot pagination. |
| `Show Slot 4` | BOOLEAN | true | Reveal slot 4. |
| `Show Slot 5` | BOOLEAN | true | Reveal slot 5. |
| `Show Slot 6` | BOOLEAN | true | Reveal slot 6. Max supported. |
| `Show Counter` | BOOLEAN | true | Toggle the counter label below the indicator. |
| `Counter Label` | TEXT | "1 / 4" | Free text — `"2 / 4"`, `"Step 3 of 5"`, `"2 of 10 बिल"`. |

Wrapper width is **360dp** to match mobile screen width. The counter label `FILL`s the wrapper so you can realign it **left / center / right** per instance. Center is the recommended default.

---

## Pagination / Counter — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Label` | TEXT | "1 / 10" | Counter text. Keep format consistent across a flow. |
| `Show Back` | BOOLEAN | true | Chevron-left button. Hide on page 1. |
| `Show Forward` | BOOLEAN | true | Chevron-right button. Hide on last page. |
| `Back Icon` | INSTANCE_SWAP | `chevron_left` (line) | Swap for another outlined icon if needed (e.g. `arrow_back_ios`). |
| `Forward Icon` | INSTANCE_SWAP | `chevron_right` (line) | Swap for another outlined icon. |

Chevron buttons are full 48dp touch targets. Outlined icons (line) match Wiom's iconography rule for interactive controls.

---

## Pagination / Scroll Indicator — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Position` | VARIANT | `Start` | Where the thumb sits — `Start`, `Middle`, `End`. Drives thumb alignment in the track. |

Track width is resizable per instance. Default thumb is ~30% of the track width.

---

## Configuration patterns

All values are property overrides on an instance. Use these as starting points.

### Onboarding — 3 slides, Dots-Simple
`Style=Dots-Simple`, `Current Page=1`, `Show Slot 3=on`, Slots 4–6 off, `Show Counter=off`. Sits under the illustration + headline block; doesn't compete for attention.

### Home promo banner — 4 banners, Dots-Expanded
`Style=Dots-Expanded`, `Current Page=2`, `Show Slot 3=on`, `Show Slot 4=on`, Slots 5–6 off, `Show Counter=off`. The expanded pill is unmistakable in a busy home feed.

### Recharge wizard — 5 steps, Bars
`Style=Bars`, `Current Page=3`, `Show Slot 3=on`, `Show Slot 4=on`, `Show Slot 5=on`, `Show Slot 6=off`, `Show Counter=on`, `Counter Label="Step 3 of 5"`. Pin to the top of every step screen.

### Bills list — 10 pages, Counter
Drop `Pagination / Counter` below the list. `Label="1 / 10"`. On page 1, `Show Back=off`. On last page, `Show Forward=off`.

### Plans rail — horizontal scroll
Drop `Pagination / Scroll Indicator` under the rail, centered. Use `Position=Start` initially; the real app updates it as the user scrolls (in code).

### Counter aligned left under Dots
`Style=Dots-Expanded`, `Show Counter=on`, then on the instance override `Counter` text `Text Align = Left`. Useful when the pagination sits under left-aligned content (e.g. a promo list item).

---

## Token bindings (summary)

Every part resolves to a Wiom foundation token. Full values live in `wiom-design-foundations`.

### Pagination (all styles)
| Part | Token |
|---|---|
| Wrapper width | 360dp (FIXED) |
| Wrapper gap (indicator → counter) | `space-8` |
| Indicator row gap (Dots) | `space-8` |
| Indicator row gap (Bars) | `space-4` |
| Dot — Default size | 8 × 8 dp · `radius-full` · `stroke.default` |
| Dot — Active (Simple) | 8 × 8 dp · `radius-full` · `bg.brand` |
| Dot — Active (Expanded) | 24 × 8 dp pill · `radius-full` · `bg.brand` |
| Bar — Default | height 4dp · FILL width · `radius-full` · `stroke.default` |
| Bar — Active | height 4dp · FILL width · `radius-full` · `bg.brand` |
| Counter typography | `type.label.md` (14sp SemiBold, 20 line-height) |
| Counter color | `text.subtle` |

### Pagination / Counter
| Part | Token |
|---|---|
| Label typography | `type.label.md` · `text.default` |
| Chevron size | `icon.md` (24dp), outlined (line) |
| Chevron color | `text.subtle` |
| Button padding | `space-12` all sides (→ 48dp touch target) |
| Container gap | 0 |

### Pagination / Scroll Indicator
| Part | Token |
|---|---|
| Track height | 4dp · `radius-full` · `bg.muted` |
| Thumb | height 4dp · ~30% track width · `radius-full` · `bg.brand` |

No shadows. No hardcoded hex. No fixed inner widths — dots are intrinsic 8dp, bars `FILL` the 360dp rail, the scroll thumb sits inside auto-layout spacers so it aligns correctly at any track width.

---

## Rules

1. **One component per semantic.** Discrete pages → `Pagination`. Long lists → `Counter`. Continuous scroll → `Scroll Indicator`. Do not substitute one for another because it "looks nicer".
2. **Max 6 slots in `Pagination`.** Seven dots/bars become meaningless. At 7+, switch to `Pagination / Counter`.
3. **Never stack Bars + Counter below it.** `Bars` already shows position. Adding a counter below repeats information and clutters. Pick one.
4. **Exactly one slot is active in Dots.** For Bars, slots 1..Current are all filled to show accumulated progress. Do not show two active dots.
5. **Counter label is center-aligned by default.** Realign per instance only when the surrounding content is off-center (e.g. left-aligned list under the pagination).
6. **Hide prev/next when out of bounds.** On page 1 of a `Counter`, set `Show Back=off`. On last page, `Show Forward=off`. Don't disable them visually — hide them.
7. **Use outlined chevrons.** Interactive nav controls use line icons (matches iconography skill: "Outlined only for toggle-off / nav-unselected"). Never use filled chevrons for prev/next.
8. **Pagination sits below the content it describes.** Dots/Bars for onboarding / carousels → below the swipeable area. Bars for wizards → top of the step screen. Counter → below the list.
9. **Don't show a counter label when it duplicates visible info.** A `Dots-Expanded` with 3 dots and "2 / 3" below is redundant. Use the counter only when the total isn't obvious (many similar bars, long rails).
10. **Token-only values.** No raw hex, no hardcoded sp/dp, no "close enough" sizes. Every measurement resolves to a Wiom variable.

---

## Anti-patterns

- Using `Pagination / Dots` for a 10-page list — dots can't communicate "page 4 of 10" meaningfully. Use `Counter`.
- Using `Pagination / Counter` on a 3-step onboarding — too formal for a casual intro. Use `Dots-Simple`.
- Putting `Scroll Indicator` under a rail with only 3 items that fit on screen — no overflow, nothing to indicate.
- Keeping a counter visible on a single-page view. Hide the component entirely.
- Mixing `Dots-Simple` and `Dots-Expanded` in the same product — pick one vocabulary for carousels and stick to it.
- Using a filled chevron (`chevron_right` filled) in `Counter` — violates the outlined-for-interactive rule.
- Setting `Current Page=1` but leaving Slot 1 invisible somehow (can't, but guard against edits) — the active slot must always render.
- Animating the pagination on every tap — platform crossfade on the active slot is enough; heavy animation distracts from content.
- Putting `Pagination` inside a tight horizontal strip with other controls. It needs the full 360dp to breathe and to allow counter alignment.
- Repainting the active slot with a secondary brand color to "theme" it. Always `bg.brand`.

---

## Implementation hints (Jetpack Compose)

### Pagination — Dots
```kotlin
@Composable
fun WiomPaginationDots(
  total: Int,
  current: Int,                // 1-based
  style: DotStyle = DotStyle.Simple,
  counterLabel: String? = null,
  modifier: Modifier = Modifier,
) {
  Column(
    modifier = modifier.fillMaxWidth(),
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
  ) {
    Row(horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8)) {
      repeat(total) { i ->
        val active = (i + 1) == current
        val width = if (active && style == DotStyle.Expanded) 24.dp else 8.dp
        Box(
          Modifier
            .size(width = width, height = 8.dp)
            .clip(RoundedCornerShape(Wiom.radius.full))
            .background(if (active) Wiom.bg.brand else Wiom.stroke.default)
        )
      }
    }
    counterLabel?.let {
      Text(it, style = Wiom.type.labelMd, color = Wiom.text.subtle,
           textAlign = TextAlign.Center, modifier = Modifier.fillMaxWidth())
    }
  }
}
```

### Pagination — Bars
```kotlin
@Composable
fun WiomPaginationBars(
  total: Int,
  current: Int,
  counterLabel: String? = null,
  modifier: Modifier = Modifier,
) {
  Column(modifier.width(360.dp), horizontalAlignment = Alignment.CenterHorizontally,
         verticalArrangement = Arrangement.spacedBy(Wiom.space.s8)) {
    Row(
      Modifier.fillMaxWidth().height(4.dp),
      horizontalArrangement = Arrangement.spacedBy(Wiom.space.s4),
    ) {
      repeat(total) { i ->
        val filled = (i + 1) <= current
        Box(
          Modifier
            .weight(1f).fillMaxHeight()
            .clip(RoundedCornerShape(Wiom.radius.full))
            .background(if (filled) Wiom.bg.brand else Wiom.stroke.default)
        )
      }
    }
    counterLabel?.let {
      Text(it, style = Wiom.type.labelMd, color = Wiom.text.subtle,
           textAlign = TextAlign.Center, modifier = Modifier.fillMaxWidth())
    }
  }
}
```

### Pagination / Counter
```kotlin
@Composable
fun WiomPaginationCounter(
  current: Int,
  total: Int,
  onPrev: () -> Unit,
  onNext: () -> Unit,
) {
  Row(verticalAlignment = Alignment.CenterVertically) {
    if (current > 1) IconButton(onClick = onPrev, modifier = Modifier.size(48.dp)) {
      Icon(painterResource(R.drawable.ic_chevron_left_line), null,
           tint = Wiom.text.subtle, modifier = Modifier.size(Wiom.icon.md))
    }
    Text("$current / $total", style = Wiom.type.labelMd, color = Wiom.text.default)
    if (current < total) IconButton(onClick = onNext, modifier = Modifier.size(48.dp)) {
      Icon(painterResource(R.drawable.ic_chevron_right_line), null,
           tint = Wiom.text.subtle, modifier = Modifier.size(Wiom.icon.md))
    }
  }
}
```

### Pagination / Scroll Indicator
Drive thumb offset from `LazyListState.firstVisibleItemIndex` + `firstVisibleItemScrollOffset` divided by total scrollable width; render a 4dp rail with a `RoundedCornerShape(Wiom.radius.full)` thumb translated by that fraction. Keep thumb width at ~30% of rail width.

---

*Wiom Design System — Pagination skill. Pairs with `wiom-design-foundations` for token values.*

---

## See also

- `wiom-design-foundations` — icon.action for chevrons, radius-full, bg.muted for inactive track
- `wiom-compose-layout-rules` — Compose structural rules, Icons.Rounded.* enforcement
- `wiom-stepper` — sibling component when steps have labels
- `wiom-top-bar` — chevron / nav-button reference
