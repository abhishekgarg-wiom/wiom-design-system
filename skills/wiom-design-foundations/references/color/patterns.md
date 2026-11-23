# Patterns Beyond Core Tokens

Small rules for recurring UI patterns. Each is an application of existing tokens — no new tokens needed.

---

## Status bar

Status bar `background` always follows the top bar it sits under — never set independently.

- Top bar = `bg.default` → status bar = `bg.default`
- Top bar = `bg.brand` → status bar = `bg.brand`
- Top bar = `bg.inverse` → status bar = `bg.inverse`

If the top-bar spec is in a separate skill or component library, read from there.

---

## Notification badges / count dots

Red count dots ("3 unread") use `bg.critical` + `text.onCritical`. **Badges are not CTAs** — the "1 destructive per screen" limit does not apply. Multiple badges per screen are fine.

```kotlin
Badge(
    containerColor = WiomColors.bgCritical,
    contentColor = WiomColors.textOnCritical,
) { Text("3") }
```

---

## Status badges (solid fill + optional leading icon)

A status badge pairs a solid `bg.*` fill with a `text.on*` label and, optionally, a leading icon. The icon tint follows the same dark-fill / light-fill logic as CTA icons: a saturated dark fill gets a light icon; a light/gold fill gets a dark icon. White icons fail AA on gold (1.98:1).

| Badge fill | Label (text.on*) | Icon tint (icon.*) | Example use |
|---|---|---|---|
| `bg.brand` (#D9008D) | `text.onBrand` (#FFFFFF) | `icon.inverse` (#FAF9FC) | "Recommended", "Sale" |
| `bg.critical` (#D92130) | `text.onCritical` (#FFFFFF) | `icon.inverse` | "Overdue", "Failed" |
| `bg.positive` (#008043) | `text.onPositive` (#FFFFFF) | `icon.inverse` | "Paid", "Verified" |
| `bg.info` (#6D17CE) | `text.onInfo` (#FFFFFF) | `icon.inverse` | "New", "Beta" |
| `bg.warning` (#E2B203 gold) | `text.onWarning` (#372902) | `icon.action` (#352D42) | "Pending", "Low balance" |

The warning-badge rule is the one exception: dark icon on gold, because `icon.inverse` (#FAF9FC) on `bg.warning` fails AA. `icon.action` aligns visually with the dark olive `text.onWarning` pairing.

```kotlin
// Positive status badge with leading icon
Row(
    modifier = Modifier
        .background(WiomColors.bgPositive, shape = RoundedCornerShape(WiomRadius.Full))
        .padding(horizontal = 8.dp, vertical = 2.dp),
    horizontalArrangement = Arrangement.spacedBy(4.dp),
    verticalAlignment = Alignment.CenterVertically,
) {
    Icon(
        imageVector = Icons.Rounded.CheckCircle,
        contentDescription = null,
        modifier = Modifier.size(WiomIcon.XS),
        tint = WiomColors.iconInverse,        // light icon on dark-green fill
    )
    Text("Paid", color = WiomColors.textOnPositive, style = WiomType.labelSm)
}

// Warning badge with leading icon
Row(
    modifier = Modifier
        .background(WiomColors.bgWarning, shape = RoundedCornerShape(WiomRadius.Full))
        .padding(horizontal = 8.dp, vertical = 2.dp),
    horizontalArrangement = Arrangement.spacedBy(4.dp),
    verticalAlignment = Alignment.CenterVertically,
) {
    Icon(
        imageVector = Icons.Rounded.HourglassTop,
        contentDescription = null,
        modifier = Modifier.size(WiomIcon.XS),
        tint = WiomColors.iconAction,         // dark icon on gold fill — not icon.inverse
    )
    Text("Pending", color = WiomColors.textOnWarning, style = WiomType.labelSm)
}
```

### Subtle badges (on `bg.*.subtle` surface)

Badges can also use the subtle-tier fill (e.g., `bg.positive.subtle` + `text.positive`). In that case the leading icon uses the saturated family token, same as a status banner:

| Badge fill (subtle) | Label | Icon tint |
|---|---|---|
| `bg.positive.subtle` | `text.positive` (for headings/short labels) or `text.default` (for body) | `icon.positive` |
| `bg.critical.subtle` | `text.critical` (AA-large only — large text or bold) or `text.default` | `icon.critical` |
| `bg.warning.subtle` | `text.onWarning` (warning family's own body-text token — do not use `text.default`) | `icon.action` (dark; gold is ratings-only) |
| `bg.info.subtle` | `text.info` or `text.default` | `icon.info` |

---

## Empty states

Compose from existing tokens:

- Card background → `bg.subtle`
- Heading → `text.default`
- Body copy → `text.subtle`
- Illustration tint (monochrome only) → `icon.nonAction`

Placeholder until empty-state template is formalized.

---

## Tooltips

`bg.info` (#6D17CE) + `text.onInfo` (#FFFFFF). 7.88:1. Paired with `shadow.md`.

```kotlin
PlainTooltip(
    containerColor = WiomColors.bgInfo,
    contentColor = WiomColors.textOnInfo,
) { Text("…") }
```

Leading icon inside a tooltip → `icon.inverse`.

---

## Star ratings

`icon.warning` (#E2B203). Gold is the industry standard for rating stars; reusing the warning token keeps the system lean.

Star ratings are the **only** use of `icon.warning`: a gold star shape reads as a rating regardless of contrast. Warning indicators never use gold; they use `icon.action` on a warning bg (iconography.md § Warning icons).

---

## Trust / verified indicators

Green, not blue:

- Icon (check, lock, shield) → `icon.positive`
- Label ("Verified", "Secure payment") → `text.positive`
- Banner background (if any) → `bg.positive.subtle`

---

## Avatar fallback colors

4-color palette when a user/CSP has no profile photo. Hash `user.name` to index.

| Index | Background | Initials |
|---|---|---|
| 0 | `bg.brand` | `text.onBrand` |
| 1 | `bg.positive` | `text.onPositive` |
| 2 | `bg.critical` | `text.onCritical` |
| 3 | `bg.info` | `text.onInfo` |

```kotlin
val palette = listOf(
    WiomColors.bgBrand to WiomColors.textOnBrand,
    WiomColors.bgPositive to WiomColors.textOnPositive,
    WiomColors.bgCritical to WiomColors.textOnCritical,
    WiomColors.bgInfo to WiomColors.textOnInfo,
)
val (bg, fg) = palette[abs(user.name.hashCode()) % 4]
```

Warning yellow is excluded — `text.onWarning` is dark and would break the consistent white-initials look.

`bg.critical` in the avatar context is **not** a destructive signal — lint rules should exempt avatar components.

---

## Image overlays and media darkening

All darkening uses `overlay.scrim` (rgba(22, 16, 33, 0.50)). No variants. If a case genuinely needs a different opacity, flag as a gap — do not invent `overlay.scrim.light`/`.dark`.
