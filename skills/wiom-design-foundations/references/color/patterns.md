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

Star ratings are the one explicit exception to the "`icon.warning` only inside `bg.warning.subtle`" rule — a gold star shape reads as a rating regardless of contrast.

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
