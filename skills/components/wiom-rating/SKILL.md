---
name: wiom-rating
description: >
  Wiom Rating: 5-star rating input / display for the Wiom apps (e.g. the
  post-installation rating ask). The only place gold icon.warning is
  allowed. Use when building or reviewing a star rating in Compose.
  Trigger on: rating, stars, star rating, RatingBar, feedback score.
---

# Wiom Rating

Five stars the user taps to rate. **The only use of gold `icon.warning`** (see foundations iconography § Warning icons). Owner decision 27 Nov 2026, same on iOS (`wiom-ios-rating`).

## 1. Anatomy

| Slot | Value | Token |
|---|---|---|
| Stars | 5 in a row | n/a |
| Star size | 32 dp glyph | `icon.xl` (D43) |
| Gap between stars | 8 dp | `space-8` |
| Tap area | 48 × 48 dp per star, glyph centred | touch-target tier |
| Filled star | `Icons.Rounded.Star` | `icon.warning` (gold) |
| Empty star | `Icons.Rounded.StarBorder` | `icon.nonAction` |
| Background | Host surface | `bg.default` |

## 2. Behaviour

- Tap star *n* = rating *n*; stars 1..*n* fill. No half stars. The value commits on tap; colour eases on `WiomMotion.selection()`.
- Read-only display: same look, not clickable, one TalkBack node ("Rated 4 of 5").
- Always put the question in text above the stars (illustrative: "आपका इंस्टॉलेशन कैसा रहा?").

## 3. Universal rules

1. Gold is for stars only; never a Warning glyph.
2. Always 5 stars; no 10-point or emoji scales.
3. Empty stars stay visible (`icon.nonAction`) so the scale is readable.

## 4. Compose

```kotlin
@Composable
fun WiomRating(
    value: Int,                          // 0..5
    onValueChange: ((Int) -> Unit)?,     // null = read-only
    modifier: Modifier = Modifier,
) {
    Row(
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
        modifier = modifier.semantics(mergeDescendants = true) {
            stateDescription = "$value of 5"
        },
    ) {
        for (n in 1..5) {
            val filled = n <= value
            val tint by animateColorAsState(
                if (filled) Wiom.icon.warning else Wiom.icon.nonAction,
                WiomMotion.selection(), label = "Star$n",
            )
            Box(
                modifier = Modifier
                    .size(48.dp)
                    .then(if (onValueChange != null) Modifier.clickable { onValueChange(n) } else Modifier),
                contentAlignment = Alignment.Center,
            ) {
                Icon(
                    imageVector = if (filled) Icons.Rounded.Star else Icons.Rounded.StarBorder,
                    contentDescription = null,
                    tint = tint,
                    modifier = Modifier.size(32.dp),          // icon.xl (D43)
                )
            }
        }
    }
}
```

## 5. Open

- Figma has no rating component yet; design to draw it from this spec.
