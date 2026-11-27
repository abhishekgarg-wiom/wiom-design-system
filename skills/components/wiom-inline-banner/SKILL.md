---
name: wiom-inline-banner
description: >
  Wiom Inline Banner: a status callout inside the page flow (info,
  positive, warning, critical) with an icon, text and an optional text
  action. Use when a screen must explain a state in place, such as an
  account on hold, a partial failure or a recharge about to end. Not for
  transient messages (wiom-toast) or field errors (wiom-input-fields).
  Trigger on: banner, callout, alert strip, info box, status message
  inside a screen, warning box.
---

# Wiom Inline Banner

A status callout in the page flow. It explains a state that stays true while the screen is open; it does not float, time out or block. Same rules on iOS (`wiom-ios-inline-banner`), owner decision 27 Nov 2026.

## 1. Anatomy

| Slot | Value | Token |
|---|---|---|
| Fill | Status subtle background | `bg.{info,positive,warning,critical}.subtle` |
| Corner | 16 dp | `radius-large` (D62) |
| Border | None | |
| Padding | All sides | `space-16` |
| Row | Icon aligned to the first text line; gap | `space-8` |
| Icon | 24 dp Material Rounded filled glyph | `icon.md` |
| Title (optional) | One short line | `type.title.sm` |
| Body | Wraps | `type.body.md` |
| Action (optional) | Text action below the body | Tertiary `WiomButton` |

## 2. Tones

| Tone | Fill | Icon | Title | Body |
|---|---|---|---|---|
| Info | `bg.info.subtle` | `Info` · `icon.info` | `text.info` | `text.default` |
| Positive | `bg.positive.subtle` | `CheckCircle` · `icon.positive` | `text.positive` | `text.default` |
| Warning | `bg.warning.subtle` | `Warning` · `icon.action` (dark; gold is ratings-only) | `text.onWarning` | `text.onWarning` |
| Critical | `bg.critical.subtle` | `Error` · `icon.critical` | `text.critical` | `text.default` |

## 3. Rules

1. **In place, not floating.** Full width inside the gutter, at the top of the section it explains.
2. **One banner per section.** Several issues go in one banner as a short list.
3. **Never gold** for the Warning glyph.
4. **Not a toast.** A message about a tap that just happened is a `wiom-toast`.
5. **Not dismissable** by default. If one ever is, add a close icon (`icon.action`, 48 dp target).
6. Text wraps; never truncates. Copy shape: `[state] + [effect]`.

## 4. Compose

```kotlin
enum class WiomBannerTone { Info, Positive, Warning, Critical }

@Composable
fun WiomInlineBanner(
    tone: WiomBannerTone,
    body: String,
    title: String? = null,
    actionLabel: String? = null,
    onAction: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    val c = rememberBannerColors(tone)
    Row(
        modifier = modifier
            .fillMaxWidth()
            .background(c.fill, RoundedCornerShape(Wiom.radius.large))
            .padding(Wiom.space.s16)
            .semantics(mergeDescendants = true) {},
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
    ) {
        Icon(c.glyph, contentDescription = null, tint = c.icon, modifier = Modifier.size(24.dp))
        Column(verticalArrangement = Arrangement.spacedBy(Wiom.space.s4), modifier = Modifier.weight(1f)) {
            if (title != null) Text(title, style = Wiom.type.title.sm, color = c.title)
            Text(body, style = Wiom.type.body.md, color = c.body)
            if (actionLabel != null && onAction != null) {
                WiomButton(text = actionLabel, type = WiomButtonType.Tertiary, onClick = onAction)
            }
        }
    }
}
```

## 5. Anti-patterns

- ❌ Gold `icon.warning` on a Warning banner.
- ❌ A banner for a one-off result (use a toast).
- ❌ Stacking several banners in one section.
- ❌ Border or shadow on the banner.

## 6. Open

- No banner component in Figma yet; this recipe comes from the foundations status-surface rules.
