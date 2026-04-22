# Primitive Palette

67 primitives across 7 families. **All primitives are `internal` in Kotlin.** Feature code must use semantic tokens (`bg.*`, `text.*`, `stroke.*`, `icon.*`). This file is for design-system maintainers, dark-mode work, and theme extensions only.

Step numbers reflect measured OKLCH lightness — matching Tailwind / Radix convention so AI pattern-matching works.

---

## Naming

`{Family}_{Step}` — e.g., `Brand_600`, `Neutral_50`, `Gray_950`. Step numbers go 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950.

- **Neutral** and **Gray** use the full 50–950 range (11 steps each) because they cover the full light-to-dark UI range
- **Brand, Positive, Critical, Warning, Info** use 100–900 (9 steps each). Step 50 would be indistinguishable from Neutral_50; step 950 would be near-black, losing color identity

---

## Brand — 9 steps

Anchor: **Brand_600 = #D9008D** (non-negotiable).

| Step | Hex | Notes |
|---|---|---|
| Brand_100 | #FFE5F6 | `bg.brand.subtle` |
| Brand_200 | #FFCCED | `bg.selected` |
| Brand_300 | #FFB2E4 | `stroke.brand` (soft), Pre-booking hover placeholder |
| Brand_400 | #FF66C9 | `bg.brand.accent` (Pre-booking rest) |
| Brand_500 | #EA5DB8 | Pre-booking pressed |
| Brand_600 | #D9008D | **Anchor** — Primary CTA fill, `stroke.brand.focus` |
| Brand_700 | #C00080 | Primary CTA hover |
| Brand_800 | #A30070 | Primary CTA pressed |
| Brand_900 | #443152 | `bg.brand.bold` (premium surfaces) |

## Neutral — 11 steps

Warm mauve undertone (hue ~320°). Carries brand warmth into all grays.

| Step | Hex | Notes |
|---|---|---|
| Neutral_50 | #FAF9FC | `bg.default` |
| Neutral_100 | #F1EDF7 | `bg.subtle` |
| Neutral_200 | #E8E4F0 | `bg.muted`, `stroke.default` |
| Neutral_300 | #D7D3E0 | `bg.disabled`, `stroke.subtle` |
| Neutral_400 | #A7A1B2 | `text.disabled`, `icon.disabled` |
| Neutral_500 | #898296 | `icon.nonAction` |
| Neutral_600 | #736C82 | (reserved) |
| Neutral_700 | #665E75 | `text.subtle` |
| Neutral_800 | #473F55 | `stroke.strong` |
| Neutral_900 | #352D42 | `icon.action` |
| Neutral_950 | #161021 | `text.default`, `bg.inverse`, `text.onBrandAccent` |

## Gray — 11 steps

Pure grayscale, zero brand warmth. Used for chat domain and as source of pure white.

| Step | Hex | Notes |
|---|---|---|
| Gray_50 | #FFFFFF | Pure white — all `text.on*` tokens, `chat.bg.userBubble` |
| Gray_100 | #F5F6F7 | `chat.bg.canvas` |
| Gray_200 | #EBEDF0 | `chat.bg.systemBubble` |
| Gray_300 | #B6B9BF | `chat.stroke.replyAccent` |
| Gray_400 | #9FA1A7 | (reserved — typing indicator) |
| Gray_500 | #84868A | (reserved) |
| Gray_600 | #6E7074 | (reserved) |
| Gray_700 | #595B5E | (reserved) |
| Gray_800 | #424446 | (reserved) |
| Gray_900 | #2A2B2D | (reserved — dark-mode chat canvas future) |
| Gray_950 | #121314 | (reserved — dark-mode chat canvas future) |

## Positive — 9 steps

Success green (hue ~160°).

| Step | Hex | Notes |
|---|---|---|
| Positive_100 | #E1FAED | `bg.positive.subtle` |
| Positive_200 | #C9F0DD | (reserved) |
| Positive_300 | #A5E5C6 | `stroke.positive` |
| Positive_400 | #66B588 | (reserved) |
| Positive_500 | #409B66 | (reserved) |
| Positive_600 | #008043 | `bg.positive`, `text.positive`, `icon.positive`, `stroke.positive.focus` |
| Positive_700 | #0B6D3A | (reserved — future hover if positive ever becomes interactive) |
| Positive_800 | #06522A | (reserved) |
| Positive_900 | #033519 | (reserved) |

## Critical — 9 steps

Error/destructive red (hue ~25°).

| Step | Hex | Notes |
|---|---|---|
| Critical_100 | #FFE5E7 | `bg.critical.subtle` |
| Critical_200 | #FFCCD0 | (reserved) |
| Critical_300 | #FFB3B9 | `stroke.critical` |
| Critical_400 | #F07778 | (reserved) |
| Critical_500 | #E2484B | (reserved) |
| Critical_600 | #D92130 | `bg.critical`, `text.critical`, `icon.critical`, `stroke.critical.focus` |
| Critical_700 | #BF1D2A | `bg.critical.hover` |
| Critical_800 | #A31824 | `bg.critical.pressed` |
| Critical_900 | #5C0005 | (reserved) |

## Warning — 9 steps

Yellow throughout (hue ~95°). Never orange. Lightness ladder modified so yellow stays yellow in dark steps (standard ladders produce brown/olive at L<0.65).

| Step | Hex | Notes |
|---|---|---|
| Warning_100 | #FFE9A1 | `bg.warning.subtle` |
| Warning_200 | #FED461 | (reserved) |
| Warning_300 | #E2B203 | `bg.warning`, `icon.warning`, `stroke.warning` — the gold accent |
| Warning_400 | #C59C17 | (reserved) |
| Warning_500 | #A48111 | (reserved) |
| Warning_600 | #896C0D | (reserved — dark gold) |
| Warning_700 | #705708 | (reserved) |
| Warning_800 | #544105 | (reserved) |
| Warning_900 | #372902 | `text.onWarning` — dark olive on gold badge |

## Info — 9 steps

Purple (hue ~296°). Anchor: **Info_600 = #6D17CE**.

| Step | Hex | Notes |
|---|---|---|
| Info_100 | #F1E5FF | `bg.info.subtle` |
| Info_200 | #E4CCFF | (reserved) |
| Info_300 | #D6B2FF | `stroke.info` |
| Info_400 | #AF81FF | (reserved) |
| Info_500 | #965AFD | (reserved) |
| Info_600 | #6D17CE | **Anchor** — `bg.info`, `text.info`, `icon.info`, `stroke.info.focus` |
| Info_700 | #5600A9 | (reserved) |
| Info_800 | #3C007C | (reserved) |
| Info_900 | #23004E | (reserved) |

---

## Anchor pinning

Brand_600 = #D9008D is pinned by convention (measured OKLCH places it closer to step 500, but every design system anchors brand at step 500/600). Info_600 = #6D17CE is pinned the same way.

Reserved steps (marked in the tables above) exist for dark mode, future semantic tokens, and partner theming. Do not reference reserved primitives in feature code — add a semantic token first.
