---
name: wiom-loader
description: Wiom Loader component spec, usage patterns, and Figma node IDs. Use this skill EVERY TIME you work with loading states, spinners, progress bars, typing indicators, skeleton screens, splash/brand loaders, or any Figma design involving wait/loading UX from the Wiom DS file. Trigger when user mentions "loader", "spinner", "progress", "loading", "skeleton", "shimmer", "typing dots", "splash screen", or "brand loader".
---

# Wiom Loader — Component Skill

5 loader component sets covering every loading scenario in the Wiom app — quick inline waits, page-level progress, chat typing, content placeholders, and branded full-screen waits.

## Where it lives

- **Figma file:** Wiom DS - Next (fileKey `ZPErh7YU7omdbyy2o268Qo`)
- **Page:** `Saurav / Loader` (`2002:175`) — **ALL Loader changes happen on this page. Never on any other page.**
- **Animation status:** Brand motion spec'd (stroke-dash chase, see §5). Lottie `.json` files pending for all 5 sets.

> **Rule (hard):** When asked to modify, audit, or extend any Loader, always navigate to `Saurav / Loader` page first (`await figma.setCurrentPageAsync(...)`). Do not create any Loader variant or copy on any other page.

---

## 1. Loader/Spinner — Circular spinner

**Set ID:** `2043:38` · 9 variants

### Variant API

| Property | Type | Options |
|---|---|---|
| `Size` | **Variant** | `sm` (20dp) · `md` (32dp) · `lg` (48dp) |
| `Color` | **Variant** | `brand` · `neutral` · `onColor` |

### Anatomy

```
┌──────────────┐
│   ╭──track──╮  │   Track: full donut ring (arcData 360°)
│   │         │  │   Indicator: 3/4 donut arc (arcData -90° → 180°)
│   ╰─indicator  │   Both share same innerRadius
└──────────────┘
```

| Size | Glyph | Ring thickness | innerRadius | Use |
|---|---|---|---|---|
| `sm` | 20dp | 2dp | 0.80 | Inline in buttons, fields, compact UI |
| `md` | 32dp | 3dp | 0.8125 | Default. Cards, dialogs, section waits |
| `lg` | 48dp | 4dp | 0.833 | Full-screen, empty states, hero |

### Color contexts

| Color | Indicator token | Track token | Canvas | Use |
|---|---|---|---|---|
| `brand` | `color/brand/primary` | `color/brand/primary-tint` | transparent | Default on light surfaces |
| `neutral` | `color/text/secondary` | `color/border/subtle` | transparent | De-emphasized, non-branded contexts |
| `onColor` | `color/white` | *(no track)* | transparent | Use ON brand-filled or dark surfaces |

**Background:** All variants have transparent fill — the spinner floats on whatever surface it's placed on. The `onColor` variant has no track ring (white-on-brand makes the track invisible).

### Figma construction

Each variant is a COMPONENT containing:
- `spinner` (FRAME, layoutMode=NONE, size = glyph size)
  - `track` (ELLIPSE, arcData full ring, fill = track token) — absent for onColor
  - `indicator` (ELLIPSE, arcData 3/4 ring from -π/2 to π, fill = indicator token)

### Usage patterns

| Scenario | Size | Color | How |
|---|---|---|---|
| Button loading state | `sm` | `brand` or `onColor` | Replace button label with spinner instance |
| Inline field validation | `sm` | `neutral` | Place in trailing slot of input field |
| Card content loading | `md` | `brand` | Center in card body |
| Full-page load | `lg` | `brand` | Center on page with body/md message below |
| On brand-filled button | `sm` | `onColor` | White spinner on brand fill |

---

## 2. Loader/Status — Wait → Success / Error

**Set ID:** TBD · 3 state variants

Circular trace with outcome-aware state transitions. Use whenever a wait has a clear success/error endpoint (payment, KYC upload, plan activation). Replaces the deprecated Linear loader.

### Variant API

| Property | Type | Options |
|---|---|---|
| `State` | **Variant** | `Wait` · `Success` · `Error` |

### Anatomy

- **Canvas:** 160×160dp
- **Ring radius:** 70dp, `stroke-width` 8dp, `stroke-linecap: round`
- **Ring pathLength:** normalised to `100`

### State specs

| State | Ring color | Ring behavior | Glyph |
|---|---|---|---|
| `Wait` | `color/text/secondary` | Chase — 25% dash travels the ring at 1.3s linear loop | none |
| `Success` | `#008043` (success/primary) | Ring fills from neutral to solid green over 450ms | Checkmark draws in over 300ms |
| `Error` | `#d92130` (error/primary) | Ring fills from neutral to solid red over 450ms | Cross draws in over 300ms |

### Transition sequence

```
0ms           1300ms        1750ms         2050ms         ~3000ms
│ Wait (chase)│ Ring fills  │ Glyph draws │ Hold outcome │ (next flow)
│             │ to solid    │             │              │
```

### Tokens used

| Element | Wait | Success | Error |
|---|---|---|---|
| Ring stroke | `color/text/secondary` | `color/success/primary` (#008043) | `color/error/primary` (#d92130) |
| Glyph stroke | — | `color/success/primary` | `color/error/primary` |
| Message text | `color/text/secondary` | `color/success/primary` | `color/error/primary` |

### Usage patterns

| Scenario | State flow |
|---|---|
| Payment processing | Wait → Success / Error |
| KYC document upload | Wait → Success / Error |
| Plan activation / recharge | Wait → Success / Error |
| Plan migration (yellow → active) | Wait → Success / Error |
| OTP verification | Wait → Success / Error |

### Icon note (open)

The current Success checkmark and Error cross glyphs are placeholders. They communicate the state correctly but the exact icon treatment (stroke weight, curve style, corner caps, animated entry style) is not finalised. **Anyone working on a screen that uses Status is free to swap these glyphs** — prefer a treatment that matches the screen's context (e.g. a softer checkmark for low-stakes confirmations, a sharper cross for hard errors). Once the Wiom icon library exposes a canonical pair, update this skill and the Figma doc (`2785:2`) together.

### Deprecations

- **Loader/Linear (old `2048:8`)** — deprecated. Use Status (for outcome-driven waits) or Skeleton (for layout-known loads). Don't build new instances.

---

## 3. Loader/Dots — Typing indicator

**Set ID:** `2052:17` · 3 variants

### Variant API

| Property | Type | Options |
|---|---|---|
| `Color` | **Variant** | `brand` · `neutral` · `onColor` |

### Anatomy

```
┌─────────────────────────┐
│         ● ● ●           │   3 dots, 8dp each, 4dp gap
│                         │   Chat bubble: 16px radius,
╰─                        │   bottom-left 4px (tail)
```

| Property | Value |
|---|---|
| Dot size | 8dp (uniform, all 3 identical) |
| Dot gap | 4dp |
| Dot opacity | 1.0 (uniform) |
| Bubble padding | 12h × 10v |
| Bubble radius | 16/16/16/4 (top-left, top-right, bottom-right, bottom-left) |
| Bubble fill | `color/surface/subtle` (brand/neutral) or `color/brand/primary` (onColor) |

### Color contexts

| Color | Dot fill | Bubble fill | Use |
|---|---|---|---|
| `brand` | `color/brand/primary` | `color/surface/subtle` | Default — Wiom assistant typing |
| `neutral` | `color/text/secondary` | `color/surface/subtle` | De-emphasized, peer typing |
| `onColor` | `color/white` | `color/brand/primary` | On dark/brand surfaces |

### Usage

Chat screens only. Place at the bottom of the message list when the other party (Wiom bot or human agent) is composing a response.

---

## 4. Loader/Skeleton — Content placeholder

**Set ID:** `2053:28` · 3 variants

### Variant API

| Property | Type | Options |
|---|---|---|
| `Type` | **Variant** | `Line` · `Card` · `ListItem` |

### Type details

**Type=Line** — Text block placeholder (3 lines)
```
┌──────────────────────────────────────┐
│  ████████████████████████████████     │  280×16, radius-tiny
│  ██████████████████████              │  200×16
│  ████████████████                    │  160×16
└──────────────────────────────────────┘
   padding: 16 all sides, gap: 8
```

**Type=Card** — Image + text placeholder
```
┌────────────────────────────────────────────┐
│  ┌──────┐  ████████████████████████████     │
│  │      │  ██████████████████              │
│  │ 72×72│  ████████████████                │
│  └──────┘                                  │
└────────────────────────────────────────────┘
   328 wide, radius-large, 1px border, gap: 16
```

**Type=ListItem** — Avatar + text + trailing
```
┌──────────────────────────────────────────────────┐
│  (40)   ██████████████████         ████          │
│  avatar ██████████████████████████                │
└──────────────────────────────────────────────────┘
   328×64, padding: 16h × 12v, gap: 16
```

### Tokens used

| Element | Fill | Radius |
|---|---|---|
| Placeholder blocks (text lines) | `color/surface/muted` | `radius/tiny` (4) |
| Image placeholder | `color/surface/muted` | `radius/medium` (12) |
| Avatar circle | `color/surface/muted` | `radius/full` (9999) |
| Card container (Card type) | `color/surface/base` | `radius/large` (16) |
| Card border | `color/border/default`, `stroke/small` (1px) | — |

### Usage

Use skeletons when the layout is known before data arrives. They make waits feel faster than spinners because the user sees where content will appear.

| Scenario | Type |
|---|---|
| Transaction history loading | `ListItem` × N stacked |
| Plan card loading | `Card` |
| Chat message loading | `Line` inside a bubble frame |
| Dashboard section loading | `Card` + `Line` combined |

---

## 5. Loader/Brand — Full-screen branded loader

**Set ID:** `2061:26` · 3 variants

### Variant API

| Property | Type | Options |
|---|---|---|
| `Style` | **Variant** | `Light` · `OnBrand` · `Overlay` |

### Frame

360×720 (standard Wiom phone canvas) · corner radius 24 · clips content

### Variant details

**Style=Light** — Splash, cold start, first load
```
┌────────────────────┐
│                    │   surface/base background
│                    │
│       Wiom         │   heading/xl, color/brand/primary
│       ∞            │   Infinity icon 128×53, color/brand/primary
│  Loading your      │   body/md, color/text/secondary
│  dashboard…        │
│                    │
└────────────────────┘
```

**Style=OnBrand** — Branded splash, app launch
```
┌────────────────────┐
│████████████████████│   brand/primary background
│████████████████████│
│███████Wiom█████████│   heading/xl, color/text/on-color
│████████∞███████████│   Infinity icon 128×53, color/white
│███Getting things███│   body/md, color/text/on-color
│███████ready…███████│
│████████████████████│
└────────────────────┘
```

**Style=Overlay** — Blocking operation over existing screen
```
┌────────────────────┐
│▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒│   Simulated content (top bar, 3 cards, bottom nav)
│▒▒┌────────────┐▒▒▒│   underneath color/overlay/default backdrop
│▒▒│░░░░░░░░░░░░│▒▒▒│   (rgba(22,16,33,0.50))
│▒▒└────────────┘▒▒▒│
│▒▒▒▒▒▒▒∞▒▒▒▒▒▒▒▒▒▒│   Infinity icon 128×53, color/white
│▒▒Processing your▒▒│   body/md, color/text/on-color
│▒▒▒▒payment…▒▒▒▒▒▒▒│
│▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒│
└────────────────────┘
```

### Infinity icon

- **Source:** User-selected icon at node `2068:48` (BOOLEAN_OPERATION Union, 22×9 original size)
- **In Brand variants:** Cloned at 128×53, fill rebound to brand/white tokens
- **Shape:** Two vector paths unioned — classic outlined infinity loop

### Motion — stroke-dash chase (finalised)

A 25%-long segment of the stroke continuously travels the closed infinity path. No visible restart, no pause at the crossover.

| Parameter | Value |
|---|---|
| Style | Stroke-dash chase (single segment chasing around the loop) |
| Dash | 25% visible · 75% gap |
| Duration | `1.3s` per full cycle |
| Easing | `linear` |
| Direction | Clockwise (offset decreasing) |
| Delivery | Lottie `.json` (pending) |

**Implementation notes (prototype / handoff):**
- SVG `<path>` must declare `pathLength="100"` — this normalises the dashpattern to the actual path length and guarantees a seamless loop.
- `stroke-dasharray: 25 75;` · `stroke-dashoffset: 0;` · animate offset `0 → -100`.
- Reference prototype: `Downloads/wiom-loaders.html` (Section 5 · Brand).

### Tokens used

| Element | Light | OnBrand | Overlay |
|---|---|---|---|
| Background | `surface/base` | `brand/primary` | `surface/base` + `overlay/default` |
| Wordmark | `brand/primary` | `text/on-color` | *(none)* |
| Infinity stroke | `brand/primary` | `white` | `white` |
| Message text | `text/secondary` | `text/on-color` | `text/on-color` |

### Use cases

| Variant | When to use |
|---|---|
| **Light** | Cold start / first app launch · Dashboard initial load · Post-login splash (1–3s) · Empty state before data arrives |
| **OnBrand** | Branded splash after install · Marketing / promo hero moment · New feature intro or tutorial entry · Onboarding step transitions |
| **Overlay** | Payment processing (>3s) · KYC document upload · Plan activation / recharge · Refund processing · Any blocking critical transaction |

---

## Picking the right loader

| Wait duration | Layout known? | Context | Use |
|---|---|---|---|
| < 1s | — | Inline (button, field) | **Spinner sm** |
| 1–3s | No | Section/card | **Spinner md** |
| 1–3s | Yes | List, card, content | **Skeleton** |
| Any | — | Has Wait→Success/Error outcome | **Status** |
| Any | Chat | Bot/agent composing | **Dots** |
| 3–10s | — | App launch, indefinite wait | **Brand** |
| > 10s | — | Blocking critical transaction | **Brand (Overlay)** + **Status** for outcome |

## Pending work

- **Lottie export:** Brand motion spec is finalised (§5 chase). Remaining motion decisions:
  1. Typing dots — monochrome brand bounce or multi-color
  2. Linear indeterminate — single sweep vs double-chase
  3. Spinner — simple rotation at what RPM
  4. Skeleton — shimmer sweep speed and direction
- **Button-embedded spinner:** Should CTA get a built-in loading boolean, or is instance-swap at render time sufficient?
- **Pull-to-refresh:** Native Android SwipeRefreshLayout or custom styled indicator?
- **Kotlin/Compose mapping:** To be authored once Lottie files are finalised
