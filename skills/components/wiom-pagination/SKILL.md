---
name: wiom-pagination
description: >
  Wiom Pagination — three surfaces, three semantics. (1) `Pagination`
  (assembled wrapper · 3 styles · 18 ship variants) for discrete 2–6
  pages / steps with Dots-Simple, Dots-Expanded, or Bars indicators
  and an optional counter label. (2) `Pagination / Counter` (chevron +
  "N / Total" text · 3 ship variants) for long paginated lists. (3)
  `Pagination / Scroll Indicator` (4 dp track + thumb · 3 ship
  positions) for continuous horizontal scroll. Reference for
  generating Jetpack Compose code with `WiomPagination`,
  `WiomPaginationCounter`, `WiomPaginationScrollIndicator` and for
  instancing the Figma components via MCP. Use this skill EVERY TIME
  you place a pager beneath a swipeable carousel, a wizard step
  indicator, a list paginated by tap, or a horizontal scroll rail.
  Trigger on: pagination, pager, dots, bars, page indicator, step
  indicator, wizard step, "1 / 10", chevron prev / next, scroll
  indicator, scroll thumb, carousel indicator, instance of 1989:31
  / 1990:31 / 1976:74 / 3063:11520.
---

# Wiom Pagination

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
sets `1990:31` (Pagination), `3063:11520` (Pagination / Counter), and
`1976:74` (Pagination / Scroll Indicator), with atoms `1959:4`
(PaginationDot / Simple), `1958:4` (PaginationDot / Expanded), and
`1971:64` (PaginationBar). Tokens come from `wiom-design-foundations`.

**3 components · 3 indicator styles · 24 total ship variants.**

| Component | Variants | Use |
|---|---|---|
| `Pagination` | `Style × Current Page` → 3 × 6 = **18** | Discrete 2–6 pages / steps with optional counter label |
| `Pagination / Counter` | `Pagination Point = At Start / In Middle / At End` → **3** | Long paginated lists (10+ pages) where swipe isn't available |
| `Pagination / Scroll Indicator` | `Position = Start / Middle / End` → **3** | Continuous horizontal scroll rails |

Three components, three semantics — pick the one that matches the **kind of progress** you're showing, not the one that fits the empty space. Always use the assembled component; never draw pagination with manual rectangles or text.

---

## 1. Figma — instancing the components

When assembling a Figma frame via MCP, instance the relevant component set and set the variant strings. Strings are **case-sensitive**.

### Pagination — `1990:31` (3 styles × 6 pages = 18 variants)

| `Style` | `Current Page` | Use for |
|---|---|---|
| `Dots-Simple` | `1` … `6` | Onboarding carousels, low-key page indicators (2–4 slides) |
| `Dots-Expanded` | `1` … `6` | Promo / banner carousels — active item must stand out at a glance |
| `Bars` | `1` … `6` | Multi-step wizards, payment / recharge flows, story-style indicators (slots 1..N filled) |

Wrapper is **360 dp wide** in Figma (matches screen width); the indicator row is centered horizontally. Below the indicator, an optional counter label sits at `space-8` gap.

**Figma ships fixed 6 slots per variant.** To represent fewer pages in the canvas, instance the variant closest to your count and trim manually, or mock with a custom auto-layout — in code, the Compose API takes `total` so this constraint doesn't carry over.

### Pagination — properties (apply per instance)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Show Counter` | Boolean | `true` | Reveals the counter label below the indicator |
| `Counter Label` | Text | `"1 / 4"` | Free text — `"2 / 4"`, `"Step 3 of 5"`, `"2 of 10 बिल"`. Counter `FILL`s the wrapper width so you can realign **left / center / right** per instance (center is the recommended default) |

### Pagination / Counter — `3063:11520` (3 variants)

**Both chevrons are always rendered.** The variant changes the *tint* of the chevron the user can't act on, never its visibility — so the control's footprint stays constant across page transitions and the user always sees both directions.

| `Pagination Point` | Node | Back chevron tint | Forward chevron tint |
|---|---|---|---|
| `At Start` | `1974:62` | `icon.disabled` (#A7A1B2) — non-tappable | `icon.action` (#352D42) — tappable |
| `In Middle` | `3063:11521` | `icon.action` — tappable | `icon.action` — tappable |
| `At End` | `3063:11529` | `icon.action` — tappable | `icon.disabled` — non-tappable |

Properties (apply per instance):

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Label` | Text | `"1 / 10"` | Counter text. Keep format consistent across a flow |
| `Back Icon` | Instance swap | `chevron_left` (line) | Outline chevron at 24 dp |
| `Forward Icon` | Instance swap | `chevron_right` (line) | Outline chevron at 24 dp |

The variant maps to user position: `At Start` = page 1 (Back disabled), `At End` = last page (Forward disabled). The component does **not** expose `Show Back / Show Forward` booleans — chevrons stay rendered; the variant tints + disables the unavailable one.

### Pagination / Scroll Indicator — `1976:74` (3 variants)

| `Position` | Node | Thumb alignment |
|---|---|---|
| `Start` | `1976:64` | Thumb flush left, spacer fills right |
| `Middle` | `1976:67` | Spacers on both sides, thumb centered |
| `End` | `1976:71` | Spacer fills left, thumb flush right |

The track is `360 × 4 dp` in Figma; the thumb is `108 × 4 dp` (= 30% of track width). `Position` drives static thumb alignment in design; in code the thumb offset is computed from scroll state.

### Code enum ↔ Figma variant string

| Kotlin | Figma `Style` |
|---|---|
| `WiomPaginationStyle.DotsSimple` | `Dots-Simple` |
| `WiomPaginationStyle.DotsExpanded` | `Dots-Expanded` |
| `WiomPaginationStyle.Bars` | `Bars` |

| Kotlin | Figma `Position` (Scroll Indicator) |
|---|---|
| `ScrollPosition.Start` | `Start` |
| `ScrollPosition.Middle` | `Middle` |
| `ScrollPosition.End` | `End` |

---

## 2. Anatomy

### Pagination — wrapper · indicator row · optional counter

```
┌─────────────────────── 360 dp wrapper ──────────────────────────┐
│                                                                  │
│                ●  ○  ○  ○  ○  ○      ← Indicator row            │
│              [space-8 between dots]                             │
│                                                                  │
│   [ space-8 gap to counter ]                                    │
│                                                                  │
│                       1 / 4                ← Counter (optional) │
│                       type.label.md                             │
│                       text.subtle                               │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘

Dot anatomy:                       Bar anatomy:
  Default: 8 × 8 dp circle           Default: weight(1f) × 4 dp
           bg.muted                            bg.muted
  Active:  8 × 8 dp (Dots-Simple)    Active:  weight(1f) × 4 dp
           24 × 8 dp pill (Expanded)          bg.brand
           bg.brand                  Bars 1..Current are all "Active"
```

| Slot | Value |
|---|---|
| Wrapper width | 360 dp Figma; in code `Modifier.fillMaxWidth()` for Bars, content-size for Dots |
| Wrapper layout | Column · `verticalArrangement = spacedBy(space-8)` · `horizontalAlignment = CenterHorizontally` |
| Indicator (Dots) gap | `space-8` between dots |
| Indicator (Bars) gap | `space-4` between bars |
| Indicator (Bars) width | `fillMaxWidth()` — each bar uses `Modifier.weight(1f)` |
| Indicator height (Dots) | 8 dp |
| Indicator height (Bars) | 4 dp |
| Dot — Default | 8 × 8 dp · `radius-full` · `bg.muted` |
| Dot — Active (Simple) | 8 × 8 dp · `radius-full` · `bg.brand` |
| Dot — Active (Expanded) | 24 × 8 dp pill · `radius-full` · `bg.brand` |
| Bar — Default | `weight(1f)` × 4 dp · `radius-full` · `bg.muted` |
| Bar — Active | `weight(1f)` × 4 dp · `radius-full` · `bg.brand` |
| Counter typography | `type.label.md` (Noto Sans SemiBold 14 / 20 LH) |
| Counter color | `text.subtle` |

### Pagination / Counter — `[Back] · text · [Forward]`

```
┌─────────── content-sized ────────────┐
│                                       │
│   ●Back   1 / 10   ●Forward          │
│   48dp             48dp               │
│   chevron_left     chevron_right      │
│   24dp             24dp                │
│                                       │
└───────────────────────────────────────┘

  Back / Forward icon-button cell:
    space-12 padding around 24dp glyph → 48dp hit target
    Chevron is always rendered. Tint switches on availability:
      - tappable    → icon.action  (#352D42)
      - non-tappable → icon.disabled (#A7A1B2)
  Text:
    type.label.md · text.default
```

| Slot | Value |
|---|---|
| Layout | Row · `verticalAlignment = CenterVertically` · `horizontalArrangement = Arrangement.Center` |
| Icon-button cell | `space-12` all-sides padding around 24 dp glyph → 48 dp hit target |
| Chevron size | 24 dp · outline (line) — `chevron_left` / `chevron_right` rounded drawable |
| Chevron tint — tappable | `icon.action` (#352D42) — foundations rule for tappable chrome |
| Chevron tint — non-tappable | `icon.disabled` (#A7A1B2) — chevron stays visible, click is consumed (no-op) |
| Label typography | `type.label.md` (SemiBold 14 / 20 LH) |
| Label color | `text.default` (#161021) — full contrast since it's the primary readable info |
| Container gap | `0` (the `space-12` icon-button padding visually spaces the text) |

### Pagination / Scroll Indicator — track + thumb

```
┌──────────────────── 360 dp track (radius-full · bg.muted) ────────────────────┐
│ ████████████████████  ▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒│
│ ↑ thumb 108 × 4 dp                                                            │
│   bg.brand · radius-full                                                      │
│   30% of track width                                                          │
└────────────────────────────── 4 dp tall ──────────────────────────────────────┘
```

| Slot | Value |
|---|---|
| Track width | Caller-driven (`Modifier.fillMaxWidth()` typically) |
| Track height | 4 dp · `radius-full` · `bg.muted` |
| Thumb width | 30 % of track width |
| Thumb height | 4 dp · `radius-full` · `bg.brand` |
| Thumb position | `Start` (left), `Middle` (centered), `End` (right) — driven by scroll state in code |

---

## 3. Surface contract — placement, not edge-to-edge

Pagination components are **inside the screen body**, not surface chrome — so they live within the standard 16 dp screen gutter, **not** edge-to-edge. (Top bar and bottom nav bar are the edge-to-edge surfaces; pagination is inline.)

### What this means in code

- The Figma 360 dp wrapper width is the **maximum** width on a 360 dp screen. In code, use `Modifier.fillMaxWidth()` and let the parent gutter (16 dp inside a screen body) constrain the available width.
- **Bars must fill the available width** — each bar uses `Modifier.weight(1f)`, gap `space-4`. Don't fix bar widths to 48 dp from the atom; that only works at exactly 6 bars on a 360 dp screen.
- **Dots are content-sized** — 6 dots × 8 dp + 5 × `space-8` gap = ~88 dp; 6 expanded with one active = ~104 dp. Center the indicator row inside the wrapper.
- The counter label uses `Modifier.fillMaxWidth()` + `textAlign = TextAlign.Center` so realigning to left / right is a one-line override.

### Placement per pattern

| Pattern | Where to drop |
|---|---|
| Onboarding carousel · Dots-Simple | Below the swipeable area, above the CTA |
| Promo banner · Dots-Expanded | Below the banner, with a `space-12` gap to the next list section |
| Wizard · Bars | Pinned at the **top** of every step screen, just below the top bar |
| Long list · Counter | Below the list, sticky-bar-style or inline at the list end |
| Horizontal rail · Scroll Indicator | Centered below the rail, full-width minus 16 dp gutter |

---

## 4. Decision logic — which component

```
What kind of progress are you showing?

Discrete pages / steps the user can swipe through?
   2-6 of them?
      Onboarding intro / casual carousel?     → Pagination · Dots-Simple
      Promo banner where active matters?      → Pagination · Dots-Expanded
      Multi-step wizard / payment / recharge? → Pagination · Bars
   7+ of them?                                → Pagination / Counter

Discrete pages, NOT swipeable (tap chevrons to navigate)?
   → Pagination / Counter

Continuous horizontal scroll (rail of cards)?
   → Pagination / Scroll Indicator

Single page / single step?
   → Don't show pagination at all
```

**When NOT to show pagination:**

1. Single page / single step. Don't show "1 / 1".
2. Content fits with no scroll. A scroll indicator with non-overflowing content is noise.
3. Vertically scrolling feeds — use the platform native scroll bar.
4. Inside an infinite-scroll list — let the load indicator do the job.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

| Slot | Token (light theme) |
|---|---|
| Inactive dot / bar / track | `bg.muted` (#E8E4F0) |
| Active dot / bar / thumb | `bg.brand` (#D9008D) |
| Dot / bar / thumb radius | `radius-full` (9999) |
| Dot — Default size | 8 × 8 dp |
| Dot — Active (Simple) size | 8 × 8 dp |
| Dot — Active (Expanded) size | 24 × 8 dp pill |
| Bar height | 4 dp |
| Track / thumb height | 4 dp |
| Indicator gap (Dots) | `space-8` (8 dp) |
| Indicator gap (Bars) | `space-4` (4 dp) |
| Wrapper → counter gap | `space-8` (8 dp) |
| Counter typography | `type.label.md` (Noto Sans SemiBold 14 / 20 LH) |
| Counter color (under Pagination) | `text.subtle` (#5C5570) |
| Counter color (Pagination / Counter) | `text.default` (#161021) — full contrast since it's the only text |
| Chevron tint — tappable | `icon.action` (#352D42) |
| Chevron tint — non-tappable (page 1 Back / last-page Forward) | `icon.disabled` (#A7A1B2) — chevron stays visible |
| Chevron icon-cell padding | `space-12` (12 dp) → 48 dp hit target around the 24 dp glyph |

No shadow on any pagination surface. No hardcoded hex. Bar widths and dot positions never use fixed dp at the indicator level — bars use `Modifier.weight(1f)`, dots are centered intrinsics.

---

## 6. Universal rules

1. **One component per semantic.** Discrete pages → `Pagination`. Long lists → `Pagination / Counter`. Continuous scroll → `Pagination / Scroll Indicator`. Don't substitute one for another because it "looks nicer" in the layout.
2. **Max 6 slots in `Pagination`.** Seven dots / bars become meaningless. At 7+, switch to `Pagination / Counter`.
3. **Active-fill semantics differ by style.** Dots → single-active (one slot filled at a time). Bars → accumulated (slots 1..Current all filled, like a stepper). A wizard's Bars at step 3 fill 1, 2, 3; a carousel's Dot at page 3 highlights only page 3. Never two Active dots; never a non-prefix subset of bars.
4. **Both chevrons always render.** In `Pagination / Counter`, the unavailable chevron (Back on page 1, Forward on last page) tints to `icon.disabled` and consumes taps as a no-op — it is never collapsed or removed. Keeping the footprint constant prevents the layout from shifting between pages and keeps both directions discoverable.
5. **Don't stack Bars + Counter below them.** Bars already shows position; an extra "Step 3 of 5" line below repeats info. Use the counter only with Dots, where total isn't otherwise visible.
6. **Outline chevrons on `Pagination / Counter`.** Use the rounded outline drawable (`ic_chevron_left_rounded`) — never a filled chevron, never `Icons.Default.*`. Foundations rule for tappable chrome icons + Wiom's `Icons.Rounded.*`-only convention.
7. **Pagination sits inside the screen body, not edge-to-edge.** It respects the 16 dp screen gutter. Top bar / nav bar are the edge-to-edge surfaces — pagination is not.
8. **Token-only values.** No raw hex, no hardcoded sp / dp at the top level. Active = `bg.brand`. Inactive = `bg.muted`. Don't repaint with secondary brand colours to "theme" the indicator.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomPaginationStyle { DotsSimple, DotsExpanded, Bars }

@Composable
fun WiomPagination(
    total: Int,                                 // 2..6 — beyond 6, switch to WiomPaginationCounter
    current: Int,                               // 1-based; in 1..total
    style: WiomPaginationStyle = WiomPaginationStyle.DotsSimple,
    counterLabel: String? = null,               // null = no counter row
    counterAlign: TextAlign = TextAlign.Center,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomPaginationCounter(
    current: Int,                               // 1-based
    total: Int,
    onPrev: () -> Unit,
    onNext: () -> Unit,
    modifier: Modifier = Modifier,
)

enum class ScrollPosition { Start, Middle, End }

@Composable
fun WiomPaginationScrollIndicator(
    listState: LazyListState,                   // drives thumb offset; pass null to use static [position]
    modifier: Modifier = Modifier,
)

// Static-position overload for previews / non-Lazy scroll containers.
@Composable
fun WiomPaginationScrollIndicator(
    position: ScrollPosition,
    modifier: Modifier = Modifier,
)
```

### `WiomPagination` impl

```kotlin
@Composable
fun WiomPagination(
    total: Int,
    current: Int,
    style: WiomPaginationStyle = WiomPaginationStyle.DotsSimple,
    counterLabel: String? = null,
    counterAlign: TextAlign = TextAlign.Center,
    modifier: Modifier = Modifier,
) {
    require(total in 2..6) { "WiomPagination supports 2..6 slots; for 7+ use WiomPaginationCounter" }
    require(current in 1..total) { "current ($current) must be in 1..total ($total)" }

    Column(
        modifier = modifier.fillMaxWidth(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s8),
    ) {
        when (style) {
            WiomPaginationStyle.DotsSimple,
            WiomPaginationStyle.DotsExpanded -> DotsRow(total, current, style)
            WiomPaginationStyle.Bars         -> BarsRow(total, current)
        }
        if (counterLabel != null) {
            Text(
                text = counterLabel,
                style = Wiom.type.label.md,
                color = Wiom.text.subtle,
                textAlign = counterAlign,
                modifier = Modifier.fillMaxWidth(),
                maxLines = 1,
            )
        }
    }
}

@Composable
private fun DotsRow(total: Int, current: Int, style: WiomPaginationStyle) {
    Row(
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        repeat(total) { i ->
            val active = (i + 1) == current
            val width = if (active && style == WiomPaginationStyle.DotsExpanded) 24.dp else 8.dp
            Box(
                modifier = Modifier
                    .size(width = width, height = 8.dp)
                    .clip(RoundedCornerShape(percent = 50))      // radius-full
                    .background(if (active) Wiom.bg.brand else Wiom.bg.muted)
            )
        }
    }
}

@Composable
private fun BarsRow(total: Int, current: Int) {
    Row(
        modifier = Modifier.fillMaxWidth().height(4.dp),
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s4),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        repeat(total) { i ->
            val filled = (i + 1) <= current                        // accumulated progress
            Box(
                modifier = Modifier
                    .weight(1f)
                    .fillMaxHeight()
                    .clip(RoundedCornerShape(percent = 50))
                    .background(if (filled) Wiom.bg.brand else Wiom.bg.muted)
            )
        }
    }
}
```

### `WiomPaginationCounter` impl

```kotlin
@Composable
fun WiomPaginationCounter(
    current: Int,
    total: Int,
    onPrev: () -> Unit,
    onNext: () -> Unit,
    modifier: Modifier = Modifier,
) {
    require(current in 1..total) { "current ($current) must be in 1..total ($total)" }

    val canPrev = current > 1
    val canNext = current < total

    Row(
        modifier = modifier,
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.Center,
    ) {
        // Back — always rendered, tinted by availability, click consumed when disabled.
        ChevronCell(
            iconRes = R.drawable.ic_chevron_left_rounded,
            contentDescription = "Previous page",
            enabled = canPrev,
            onClick = onPrev,
        )
        Text(
            text = "$current / $total",
            style = Wiom.type.label.md,
            color = Wiom.text.default,
            maxLines = 1,
        )
        // Forward — always rendered, tinted by availability, click consumed when disabled.
        ChevronCell(
            iconRes = R.drawable.ic_chevron_right_rounded,
            contentDescription = "Next page",
            enabled = canNext,
            onClick = onNext,
        )
    }
}

@Composable
private fun ChevronCell(
    @DrawableRes iconRes: Int,
    contentDescription: String,
    enabled: Boolean,
    onClick: () -> Unit,
) {
    Box(
        modifier = Modifier
            .size(48.dp)
            // `enabled = false` keeps the cell laid out and the tap consumed (no-op),
            // so the user sees a stable hit target but nothing happens at boundaries.
            .clickable(enabled = enabled, onClick = onClick),
        contentAlignment = Alignment.Center,
    ) {
        Icon(
            painter = painterResource(iconRes),
            contentDescription = if (enabled) contentDescription else null,  // skip a11y when disabled
            tint = if (enabled) Wiom.icon.action else Wiom.icon.disabled,
            modifier = Modifier.size(24.dp),
        )
    }
}
```

### `WiomPaginationScrollIndicator` impl

```kotlin
@Composable
fun WiomPaginationScrollIndicator(
    listState: LazyListState,
    modifier: Modifier = Modifier,
) {
    // Approximate thumb fraction from first-visible-item index + sub-item offset.
    // For variable-height items, layoutInfo gives a more accurate estimate; this is the
    // typical "good enough" approach for horizontal rails of similar-sized cards.
    val info = listState.layoutInfo
    val itemCount = info.totalItemsCount.coerceAtLeast(1)
    val firstVisible = listState.firstVisibleItemIndex
    val firstOffsetPx = listState.firstVisibleItemScrollOffset.toFloat()
    val avgItemSize = info.visibleItemsInfo.firstOrNull()?.size?.toFloat()?.coerceAtLeast(1f) ?: 1f
    val visibleFraction = (info.viewportEndOffset - info.viewportStartOffset) /
                          (avgItemSize * itemCount).coerceAtLeast(1f)
    val scrolledFraction = (firstVisible + firstOffsetPx / avgItemSize) /
                           itemCount.coerceAtLeast(1)

    val thumbFractionOfTrack = visibleFraction.coerceIn(0.1f, 1f)              // floor at 10%
    val maxScrollFraction = (1f - thumbFractionOfTrack).coerceAtLeast(0f)
    val thumbStartFraction = (scrolledFraction.coerceIn(0f, 1f)) * maxScrollFraction

    Track(
        modifier = modifier,
        thumbStartFraction = thumbStartFraction,
        thumbFractionOfTrack = thumbFractionOfTrack,
    )
}

@Composable
fun WiomPaginationScrollIndicator(
    position: ScrollPosition,
    modifier: Modifier = Modifier,
) {
    val thumbFractionOfTrack = 0.30f                                            // Figma ship: 108 / 360
    val thumbStartFraction = when (position) {
        ScrollPosition.Start  -> 0f
        ScrollPosition.Middle -> (1f - thumbFractionOfTrack) / 2f
        ScrollPosition.End    -> 1f - thumbFractionOfTrack
    }
    Track(
        modifier = modifier,
        thumbStartFraction = thumbStartFraction,
        thumbFractionOfTrack = thumbFractionOfTrack,
    )
}

@Composable
private fun Track(
    modifier: Modifier,
    thumbStartFraction: Float,
    thumbFractionOfTrack: Float,
) {
    Box(
        modifier = modifier
            .fillMaxWidth()
            .height(4.dp)
            .clip(RoundedCornerShape(percent = 50))
            .background(Wiom.bg.muted),
    ) {
        BoxWithConstraints(modifier = Modifier.fillMaxSize()) {
            val trackWidth = maxWidth
            Box(
                modifier = Modifier
                    .offset(x = trackWidth * thumbStartFraction)
                    .width(trackWidth * thumbFractionOfTrack)
                    .fillMaxHeight()
                    .clip(RoundedCornerShape(percent = 50))
                    .background(Wiom.bg.brand),
            )
        }
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — wrong inactive token (stroke.default instead of bg.muted),
//             fixed bar widths, hardcoded chevron from Material Icons
Row(horizontalArrangement = Arrangement.spacedBy(4.dp)) {
    repeat(6) { i ->
        Box(Modifier.size(48.dp, 4.dp)                            // bars don't take fillMaxWidth
            .background(if (i < current) Wiom.bg.brand else Wiom.stroke.default))
    }
}
IconButton(onClick = onPrev) {
    Icon(Icons.Default.ChevronLeft, null)                          // wrong icon family
}

// ✅ RIGHT — bg.muted for inactive, bars take weight(1f), Wiom rounded drawables
WiomPagination(
    total = 5, current = 3,
    style = WiomPaginationStyle.Bars,
    counterLabel = "Step 3 of 5",
)

WiomPaginationCounter(
    current = currentPage, total = totalPages,
    onPrev = { vm.prev() },
    onNext = { vm.next() },
)
```

---

## 8. Screen-level recipes

### Onboarding — 3 slides, Dots-Simple

```kotlin
WiomPagination(
    total = 3,
    current = pagerState.currentPage + 1,
    style = WiomPaginationStyle.DotsSimple,
)
```

### Promo banner — 4 banners, Dots-Expanded

```kotlin
WiomPagination(
    total = 4,
    current = bannerIndex + 1,
    style = WiomPaginationStyle.DotsExpanded,
)
```

### Recharge wizard — 5 steps, Bars + counter at top

```kotlin
Column(modifier = Modifier.padding(horizontal = 16.dp).padding(top = 8.dp)) {
    WiomPagination(
        total = 5,
        current = step,
        style = WiomPaginationStyle.Bars,
        counterLabel = "Step $step of 5",
    )
    Spacer(Modifier.height(16.dp))
    StepContent(step)
}
```

### Bills list — 10 pages, Counter at the bottom

```kotlin
Column {
    BillsList(page = currentPage, items = items)
    Spacer(Modifier.height(8.dp))
    WiomPaginationCounter(
        current = currentPage,
        total = totalPages,
        onPrev = { vm.prevPage() },
        onNext = { vm.nextPage() },
    )
}
```

### Plans rail — horizontal scroll + Scroll Indicator

```kotlin
val listState = rememberLazyListState()

Column(modifier = Modifier.padding(horizontal = 16.dp)) {
    LazyRow(
        state = listState,
        horizontalArrangement = Arrangement.spacedBy(12.dp),
    ) {
        items(plans) { PlanCard(it) }
    }
    Spacer(Modifier.height(12.dp))
    WiomPaginationScrollIndicator(listState = listState)
}
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Drawing pagination with manual `Box` rectangles or `Text("• • •")` — always use the component.
- ❌ Mixing `Dots-Simple` and `Dots-Expanded` in the same product — pick one vocabulary for carousels and stick to it.
- ❌ Putting `Pagination` inside a tight horizontal strip with other inline controls — it needs the wrapper width to balance the indicator and the (optional) counter.
- ❌ Animating the indicator on every tap — the platform crossfade between Active / Default colors is enough; heavy animation distracts from the swipeable content.
- ❌ Using the Active brand pink to "theme" non-Active dots / bars — Active = `bg.brand`, Inactive = `bg.muted`, no in-between.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Pagination canvas: `3063:12446`.
Re-pull `get_variable_defs` on `1990:31` if foundations token names ever change.

| Node | Component / variant |
|---|---|
| `1959:4` | PaginationDot / Simple — atom (2 states) |
| `1958:4` | PaginationDot / Expanded — atom (2 states) |
| `1971:64` | PaginationBar — atom (2 states) |
| `1990:31` | Pagination component set — 18 ship variants (`Style × Current Page`) |
| `1989:31` … `1989:184` | Each `Style × Current Page` instance, sequentially numbered |
| `3063:11520` | Pagination / Counter component — 3 ship variants (`Pagination Point`) |
| `1974:62` · `3063:11521` · `3063:11529` | Counter · `At Start` · `In Middle` · `At End` |
| `1976:74` | Pagination / Scroll Indicator — 3 ship variants (`Position`) |
| `1976:64` · `1976:67` · `1976:71` | Scroll Indicator · `Start` · `Middle` · `End` |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- Inactive dot / bar / scroll-indicator track ships at **`bg.muted` (#E8E4F0)**, not `stroke.default`. Older skill drafts mapped these to `stroke.default` — the ship value is the source of truth.
- The Figma `Pagination` component ships **fixed at 6 slots per `Style × Current Page` variant**. There are no `Show Slot 3..6` booleans in the component (older drafts mentioned them). To represent fewer slots in Figma, customise the instance; in code, the `total` parameter handles 2..6 cleanly.
- The `Pagination / Counter` component switches Back / Forward visibility via the `Pagination Point` variant (`At Start` / `In Middle` / `At End`), **not** via `Show Back` / `Show Forward` booleans. The Compose API hides chevrons based on `current` vs `total` (rule 4 in §6).
- The `Scroll Indicator` thumb is **108 dp wide on a 360 dp track = 30%** of track width. This 30 % default holds in code unless the caller overrides via `LazyListState`-driven sizing. Don't shrink it below 10 % or it's hard to read.
- The standalone `Pagination / Counter` uses **`text.default`** for the counter text (full contrast — only piece of text in a compact control), while the **counter label below `Pagination`** uses `text.subtle` (lower hierarchy, the indicator is the primary signal). Same typography, different colour by context.

---

## See also

- `wiom-design-foundations` — token source. Especially `bg.brand` / `bg.muted` for Active / Inactive, `radius-full`, and the `icon.action` rule for tappable chrome chevrons.
- `wiom-compose-layout-rules` — Modifier discipline; `Icons.Rounded.*` enforcement and the rule against `Icons.Default / Filled / Outlined / Sharp / TwoTone`.
- `wiom-stepper` — sibling component when steps need labels (more verbose than Bars; pick this when each step has a name visible at the indicator).
- `wiom-tabs-filters` — for switching between sections inside one screen. Distinct from `Pagination`, which advances **through** content sequentially.
- `wiom-top-bar` — chevron / nav-button styling reference for the `Pagination / Counter` chevrons.
