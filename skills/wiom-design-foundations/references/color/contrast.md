# Contrast Verification

All common Wiom color pairings are pre-verified against WCAG AA (4.5:1 for body text, 3:1 for large text and graphics). This file documents which pairs pass and at what level, so you don't need to re-verify when composing tokens per their documented `PAIRS` annotation.

---

## WCAG levels used

Real WCAG 2.1 levels:

- **AA** — ≥ 4.5:1 for body text, ≥ 3:1 for large text (18pt+ or 14pt+bold) and graphics/icons
- **AA-Large** — ≥ 3:1, passes for large text only (not small body)
- **AAA** — ≥ 7:1 for body text (only called out where it's load-bearing — currently `text.onWarning` on `bg.default`, which is why Wiom doesn't need a separate `text.warning` token)
- **Fail** — below these thresholds

If a pair exceeds AA, it's usually labelled "AA" — the ratio tells you how comfortable. AAA is labelled explicitly only where the headroom matters for a design decision.

---

## Core UI pairs

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.default` (#161021) | `bg.default` (#FAF9FC) | 17.71 | AA |
| `text.default` | `bg.subtle` (#F1EDF7) | 16.10 | AA |
| `text.default` | `bg.muted` (#E8E4F0) | 14.85 | AA |
| `text.onWarning` (#372902) | `bg.warning.subtle` (#FFE9A1) | 8.76 | AA |
| `text.default` | `bg.positive.subtle` (#E1FAED) | 16.90 | AA |
| `text.default` | `bg.critical.subtle` (#FFE5E7) | 15.58 | AA |
| `text.default` | `bg.info.subtle` (#F1E5FF) | 15.39 | AA |
| `text.subtle` (#5C5570) | `bg.default` | 6.70 | AA |
| `text.disabled` (#A7A1B2) | `bg.default` | 2.39 | Below AA (intentional — disabled) |
| `text.inverse` (#FAF9FC) | `bg.inverse` (#161021) | 17.71 | AA |

## Interactive — brand

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onBrand` (#FFFFFF) | `bg.brand` (#D9008D) | 4.84 | AA |
| `text.brand` (#D9008D) | `bg.default` | 4.62 | AA |
| `text.brand` | `bg.brand.subtle` (#FFE5F6) | 4.10 | **AA-Large only** |

**The brand/subtle pair passes AA-large only.** For small body text on `bg.brand.subtle` surfaces, use `text.default`. Reserve `text.brand` for button labels (18pt+ typography or bold) and headings.

## Interactive — destructive

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onCritical` (#FFFFFF) | `bg.critical` (#D92130) | 4.99 | AA |
| `text.critical` (#D92130) | `bg.default` | 4.75 | AA |
| `text.critical` | `bg.critical.subtle` (#FFE5E7) | 4.18 | **AA-Large only** |

Same pattern: `text.critical` is safe for headings/buttons/icons on error banners but not for small body text. Use `text.default` for body copy on error surfaces.

## Status — positive

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onPositive` (#FFFFFF) | `bg.positive` (#008043) | 5.04 | AA |
| `text.positive` (#008043) | `bg.default` | 4.80 | AA |
| `text.positive` | `bg.positive.subtle` (#E1FAED) | 4.58 | AA |
| `icon.positive` | `bg.default` | 4.80 | AA |

## Status — warning

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onWarning` (#372902) | `bg.warning` (#E2B203) | 7.16 | AA |
| `text.onWarning` (#372902) | `bg.warning.subtle` (#FFE9A1) | 8.76 | AA |
| `text.onWarning` (#372902) | `bg.default` (#FAF9FC) | **14.18** | **AAA** — warning-family text on white/light neutral surfaces |
| `icon.warning` (#E2B203) | `bg.warning.subtle` (#FFE9A1) | 1.64 | **Fails — OK for accent, not text** |
| `icon.warning` | `bg.default` | 1.89 | **Fails — token is invalid on bg.default** |

### Three things to know

1. **`text.onWarning` is a one-token family — valid on three surfaces.** `bg.warning` (solid gold, 7.16:1 AA), `bg.warning.subtle` (banner, 8.76:1 AA), and `bg.default` (white, 14.18:1 AAA). Warning is the only family where `text.on*` doubles as the family's default-surface text because warning bgs are light (yellow/gold) so `text.on*` is dark (olive). Critical/positive/info `text.on*` tokens are white, so they only work on their saturated-colored bgs — those families use separate `text.critical` / `text.positive` / `text.info` tokens for default-surface text. Warning doesn't need one. Do not use `text.default` on a warning surface.

2. **`icon.warning` is only AA-pass for graphics (≥3:1)** when it's inside `bg.warning.subtle` at sufficient size (≥24dp with 2dp+ stroke). The icon is decorative-tier on this surface — the warning signal is carried by the surface color and context, not by icon contrast. This is documented and accepted; the token is scoped to warning surfaces only.

3. **`icon.warning` on `bg.default` is 1.89:1 — invalid.** A lint rule checks this. If you need a yellow warning indicator on a white card, wrap it in a `bg.warning.subtle` chip or use `icon.critical` (red) for the alert instead.

## Status — info

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onInfo` (#FFFFFF) | `bg.info` (#6D17CE) | 7.88 | AA |
| `text.info` (#6D17CE) | `bg.default` | 7.52 | AA |
| `text.info` | `bg.info.subtle` (#F1E5FF) | 6.53 | AA |
| `icon.info` | `bg.default` | 7.52 | AA |

### Warning and Info have family-specific body-text tokens

Positive and Critical banners still use `text.default` for body text (family-colored text fails or is AA-large only on its own subtle bg). **Warning and Info each have a family-specific token:**

| Family | Body text on subtle bg | Ratio |
|---|---|---|
| Positive | `text.default` on `bg.positive.subtle` — reserve `text.positive` for headings (4.58:1, AA marginal) | 16.90:1 |
| Critical | `text.default` on `bg.critical.subtle` — reserve `text.critical` for headings (4.18:1, AA-Large only) | 15.58:1 |
| **Warning** | **`text.onWarning` on `bg.warning.subtle`** — the family's own body-text token | **8.76:1 AA** |
| **Info** | `text.info` on `bg.info.subtle` (or `text.default` for long body copy) | 6.53:1 AA |

Warning is now a cohesive-identity family: the dark olive `text.onWarning` that sits on the solid gold badge also sits on the subtle yellow banner, tying the family together. Info similarly has its own saturated purple body-text option.

Positive and Critical stay stricter — prefer `text.default` for body copy, reserve the family-colored text for headings and icons.

## Pre-booking CTA

| Foreground | Background | Ratio | Level |
|---|---|---|---|
| `text.onBrandAccent` (#161021) | `bg.brand.accent` (#FF66C9 rest) | 7.07 | AA |
| `text.onBrandAccent` | `bg.brand.accent.hover` (#FFB2E4) | 11.27 | AA |
| `text.onBrandAccent` | `bg.brand.accent.pressed` (#EA5DB8) | 5.98 | AA |

---

## Icons and graphics (3:1 minimum)

Non-text elements (icons, borders, focus rings) need 3:1 minimum against their background.

| Graphic | On | Ratio | Level |
|---|---|---|---|
| `stroke.brand.focus` (#D9008D) | `bg.default` | 4.62 | AA (both text and graphics) |
| `stroke.brand.focus` | `bg.brand` (focus ring on Primary CTA) | 1.00 | **Fails — ring must sit outside the button, not on it** |
| `stroke.subtle` (#D7D3E0) | `bg.default` | 1.40 | **Fails graphics AA** — OK because decorative only, not a focus ring |
| `stroke.strong` (#473F55) | `bg.default` | 9.49 | AA |
| `icon.action` (#352D42) | `bg.default` | 12.47 | AA |
| `icon.nonAction` (#898296) | `bg.default` | 3.48 | AA graphics (repointed to Neutral_500 to clear 3:1) |
| `icon.nonAction` (#898296) | `bg.subtle` (#F1EDF7) | 3.18 | AA graphics (marginal) |
| `icon.nonAction` (#898296) | `bg.muted` (#E8E4F0) | 2.92 | **Fails graphics AA** — avoid decorative icons on muted surfaces |
| `icon.brand` (#D9008D) | `bg.default` | 4.62 | AA |

### Icons inside CTAs

| Icon on CTA fill | Ratio | Level |
|---|---|---|
| `icon.inverse` (#FAF9FC) on `bg.brand` (#D9008D) | 4.62 | AA |
| `icon.inverse` on `bg.critical` (#D92130) | 4.75 | AA |
| `icon.brand` (#D9008D) on `bg.default` — Secondary/Tertiary icon | 4.62 | AA |
| `icon.action` (#352D42) on `bg.brand.accent` (#FF66C9) — Pre-booking | 4.98 | AA |
| `icon.disabled` (#A7A1B2) on `bg.disabled` (#D7D3E0) | 1.64 | **Intentionally sub-AA** — disabled signals unavailable; `icon.disabled` sits above `bg.disabled` hex so the icon stays visible |
| `text.disabled` (#A7A1B2) on `bg.disabled` (#D7D3E0) | 1.64 | **Intentionally sub-AA** — same pairing as icon, since `text.disabled` and `icon.disabled` now share hex |

All CTA icon pairings pass AA. Icon-vs-label hex mismatches on filled CTAs (icon `#FAF9FC` vs label `#FFFFFF` on Primary; icon `#352D42` vs label `#161021` on Pre-booking) are **intentional** to rebalance solid-icon visual weight against text glyphs — documented in `core-tokens.md` under `icon.*`.


### Focus ring on Primary CTA

`stroke.brand.focus` (Brand_600) doesn't contrast against `bg.brand` (also Brand_600). When a Primary CTA is focused, use a different focus pattern:
- **Option A:** Offset ring outside the button (ring sits on `bg.default`, not on the button surface)
- **Option B:** Second ring — a 1dp white ring inside a 2dp brand ring

The current convention is Option A — focus indicator sits outside the button edge.

---

## How ratios were calculated

Relative luminance per WCAG 2.1 formula. For every pair in this table, compute as:

```python
def rel_lum(hex_c):
    r, g, b = hex_to_linear(hex_c)
    def f(c):
        return c/12.92 if c <= 0.03928 else ((c+0.055)/1.055)**2.4
    return 0.2126*f(r) + 0.7152*f(g) + 0.0722*f(b)

def contrast(fg, bg):
    l1, l2 = rel_lum(fg), rel_lum(bg)
    if l1 < l2: l1, l2 = l2, l1
    return (l1 + 0.05) / (l2 + 0.05)
```

When adding a new semantic token, verify against the relevant background using this method before shipping.

---

## Pairings to AVOID (documented failures or ambiguity)

| Attempt | Issue |
|---|---|
| `text.critical` for body text on `bg.critical.subtle` | 4.18:1 — passes AA-large only, fails for small body |
| (none — `text.critical` on bg.default is 4.75:1, passes AA for body) | — |
| `text.brand` for body text on `bg.brand.subtle` | 4.10:1 — passes AA-large only |
| `text.default` on `bg.warning.subtle` | ratio fine (15.42:1) but **wrong token** — warning family uses `text.onWarning` for cohesive identity |
| `icon.warning` on `bg.default` | 1.89:1 — invalid |
| `icon.nonAction` on `bg.muted` | 2.92:1 — **fails graphics AA** on that specific surface. Keep decorative icons on `bg.default` or `bg.subtle`; if the surface must be `bg.muted` and the icon is meaningful, escalate to `icon.action` (Neutral_900). |
| White text on `bg.brand.accent` (#FF66C9) | 2.63:1 — fails. Must use `text.onBrandAccent` (dark) |
| White text on `bg.warning` (#E2B203) | 1.98:1 — fails. Must use `text.onWarning` (dark) |

---

## Audit procedure for new screens

1. For every text layer, identify the foreground token and its background.
2. Check the pairing in this table.
3. If the pairing is "AA-large only," confirm the text is 18pt+ or 14pt-bold.
4. If the pairing isn't listed, compute contrast manually and add to this table.
5. Flag any ratio below 4.5 for body text or below 3.0 for graphics.

Lint enforcement: a CI rule can parse the WiomColors.kt file, read each `PAIRS X (ratio:1)` comment, and re-verify that the hex values still produce that ratio. Drift is caught at build time.
