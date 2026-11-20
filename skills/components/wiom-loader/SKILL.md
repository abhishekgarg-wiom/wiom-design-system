---
name: wiom-loader
description: >
  Wiom Loader — five distinct loaders covering every wait scenario
  in the Wiom apps: inline / page-level spinners (`Spinner`),
  short-wait progress bars (`Linear`), chat typing indicators
  (`Dots`), content placeholders (`Skeleton`), and full-screen
  branded waits (`Brand`). 5 component sets · 20 ship variants
  total (Spinner 9 = 3 sizes × 3 colors · Linear 2 = Indeterminate /
  Determinate · Dots 3 = 3 colors · Skeleton 3 = Line / Card /
  ListItem · Brand 3 = Light / OnBrand / Overlay). Each loader is
  indicator-only — the message text, surface chrome, and tap
  affordance live on the parent. Reference for generating Jetpack
  Compose code with `WiomSpinner` / `WiomLinearProgress` /
  `WiomDots` / `WiomSkeleton` / `WiomBrandLoader` and for
  instancing the Figma component sets via MCP. Use this skill EVERY
  TIME you place a spinner, progress bar, typing indicator,
  skeleton placeholder, splash / brand loader, or any Figma frame
  involving a wait / loading state. Trigger on: loader, spinner,
  progress, loading, skeleton, shimmer, typing dots, splash screen,
  brand loader, instance of 2043:38 / 2048:8 / 2052:17 / 2053:28 /
  2061:26. Pairs with wiom-design-foundations (tokens), wiom-cta
  (Loading state uses Spinner sm), wiom-input-fields (trailing
  Spinner sm during validation), wiom-dialog (Loading dialog
  composes Spinner / Brand), wiom-progress-indicator (long-form
  determinate progress with milestones / KYC / install meters —
  Loader Linear is for short waits), and wiom-compose-layout-rules
  (Modifier discipline).
---

# Wiom Loader — 5 patterns for 5 moments

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component sets `2043:38` (Spinner), `2048:8` (Linear), `2052:17` (Dots), `2053:28` (Skeleton), `2061:26` (Brand), and the `wiom-design-foundations` token set.

**5 components · 20 ship variants · indicator-only — parents own the message + surface.**

| Loader | Set | Variants | Use for |
|---|---|---|---|
| `Spinner` | `2043:38` | 9 = 3 sizes × 3 colors | Inline waits in buttons / fields / cards / page-level |
| `Linear` | `2048:8` | 2 = Indeterminate / Determinate | Short progress with a known surface (snackbar-style, top-of-screen) |
| `Dots` | `2052:17` | 3 = 3 colors | Chat typing indicator (assistant / agent composing) |
| `Skeleton` | `2053:28` | 3 = Line / Card / ListItem | Content placeholders when layout is known but data isn't |
| `Brand` | `2061:26` | 3 = Light / OnBrand / Overlay | Full-screen branded waits — splash / blocking critical ops |

Each loader is **indicator-only**: it renders the wait visual and stops. Surrounding chrome — the dialog surface, the snackbar pill, the message text below the spinner, the chat-bubble shell behind the dots — lives on the parent component (`wiom-dialog` / `wiom-toast` / `wiom-cta` / chat host).

> **Linear vs Progress Indicator** — `Loader/Linear` is for **short waits** (≤ 5 s) with no milestones (a thin bar at the top of a snackbar / surface). `wiom-progress-indicator` is for **long-form determinate progress** with milestones, percentage labels, or step counters (KYC steps, upload meter). When in doubt, use the progress-indicator skill.

---

## 1. Figma — instancing the components

5 component sets, instanced separately. Strings are **case-sensitive**.

### Spinner — `2043:38` (9 ship variants)

| `Size` ↓ \ `Color` → | `brand` | `neutral` | `onColor` |
|---|---|---|---|
| `sm` (20 dp) | `2043:5` | `2043:9` | `2043:13` |
| `md` (32 dp) | `2043:16` | `2043:20` | `2043:24` |
| `lg` (48 dp) | `2043:27` | `2043:31` | `2043:35` |

Each variant is a circular spinner: a full-ring **track** (360°, light tone) + a **3/4-ring indicator** (270° arc, strong tone), both at the same inner radius. The `onColor` variants drop the track (white-on-brand reads cleanly without it).

### Linear — `2048:8` (2 ship variants)

| `Type` | Node | Behaviour |
|---|---|---|
| `Indeterminate` | `2048:2` | 40 % fill segment travels left → right at ~ 1.5 s loop · brand colour |
| `Determinate` | `2048:5` | Fill width animates from 0 % → caller's value · brand colour |

Container is 376 × 48 dp · `bg.default` · `radius-medium` (12 dp) · padding `24h × 22v`. Track is 328 × 4 dp · `bg.brand.subtle` · `radius-full`. Fill is `bg.brand` · `radius-full`. The container exists in Figma to show the loader on a card; in code the parent provides the surface — see §3.

### Dots — `2052:17` (3 ship variants)

| `Color` | Node | Use |
|---|---|---|
| `brand` | `2052:2` | Wiom assistant / bot composing — default |
| `neutral` | `2052:7` | Peer / human-agent typing — de-emphasised |
| `onColor` | `2052:12` | Inside a brand-filled chat bubble |

Anatomy: 3 dots (8 dp each, 4 dp gap, uniform opacity) inside a chat bubble (`radius-large 16 dp` with `radius-tiny 4 dp` on the bottom-left tail · padding `12h × 10v`).

### Skeleton — `2053:28` (3 ship variants)

| `Type` | Node | Anatomy |
|---|---|---|
| `Line` | `2053:2` | 3 stacked text-line bars (280 / 200 / 160 × 16 dp · `radius-tiny`) · padding 16 · gap 8 |
| `Card` | `2053:9` | 72 × 72 dp image block + 3 text-line bars (right) · 328 wide · `radius-large` · 1 dp `stroke.subtle` border · gap 16 |
| `ListItem` | `2053:19` | 40 dp avatar circle + 2 text bars + trailing 60 × 16 dp meta · 328 × 64 dp · padding `16h × 12v` · gap 16 |

All placeholder blocks fill `bg.muted` (#E8E4F0).

### Brand — `2061:26` (3 ship variants)

| `Style` | Node | Background | Wordmark | Infinity | Message |
|---|---|---|---|---|---|
| `Light` | `2061:2` | `bg.default` | `text.brand` | `icon.brand` | `text.subtle` |
| `OnBrand` | `2061:7` | `bg.brand` | `text.onBrand` | `icon.inverse` | `text.onBrand` |
| `Overlay` | `2061:13` | content + `overlay.scrim` (50 %) | (none) | `icon.inverse` | `text.onBrand` |

Frame is 360 × 720 dp (standard Wiom phone canvas) · `radius-xlarge` (24 dp) corners. Infinity glyph is the Wiom wordmark mark (128 × 53 dp · stroke-dash chase animation, see §5). Message is `type.body.md` centered below the mark.

### Code enum ↔ Figma variant string

| Kotlin | Figma value |
|---|---|
| `WiomSpinnerSize.{Sm, Md, Lg}` | `sm` / `md` / `lg` |
| `WiomSpinnerColor.{Brand, Neutral, OnColor}` | `brand` / `neutral` / `onColor` |
| `WiomLinearType.{Indeterminate, Determinate}` | `Indeterminate` / `Determinate` |
| `WiomDotsColor.{Brand, Neutral, OnColor}` | `brand` / `neutral` / `onColor` |
| `WiomSkeletonType.{Line, Card, ListItem}` | `Line` / `Card` / `ListItem` |
| `WiomBrandLoaderStyle.{Light, OnBrand, Overlay}` | `Light` / `OnBrand` / `Overlay` |

---

## 2. Anatomy

### Spinner — circular ring

```
┌─ 32 dp (md) ──┐
│               │       Track:     full 360° ring · stroke 3 dp · bg.brand.subtle
│   ╭───────╮   │       Indicator: 270° arc       · stroke 3 dp · bg.brand
│   │       │   │       Both share the same innerRadius (~ 0.81 of bounding box)
│   ╰───────╯   │       Rotation: clockwise · 1.0 s linear loop
│               │
└───────────────┘
```

| Size | Bounding box | Stroke thickness | innerRadius (% of box) |
|---|---|---|---|
| `Sm` | 20 × 20 dp | 2 dp | 0.80 |
| `Md` | 32 × 32 dp | 3 dp | 0.8125 |
| `Lg` | 48 × 48 dp | 4 dp | 0.833 |

Background is **transparent** at all sizes — the spinner floats on the parent surface. `OnColor` variant drops the track.

### Linear — short progress bar

```
┌───────────── 376 × 48 dp · bg.default · radius-medium ─────────────┐
│  [padding 24h · 22v]                                                │
│   ┌─────────────── track 328 × 4 dp · bg.brand.subtle ──────────┐  │
│   │■■■■■■■■■■■■■■■■■■■■■■■■                                    │  │
│   └────────────────────────────────────────────────────────────┘  │
│      ↑                              ↑
│     fill: bg.brand · radius-full · width = 40% (Indeterminate · animated travel)
│                                          or caller value (Determinate · static)
└────────────────────────────────────────────────────────────────────┘
```

The Figma container is illustrative — in code, the parent (snackbar / inline surface) supplies the surface chrome and the loader contributes only the 4 dp bar.

### Dots — typing indicator

```
┌─ 104 × 32 dp chat bubble · bg.subtle (brand/neutral) · bg.brand (onColor) ─┐
│  [padding 12h · 12v]                                                       │
│       ●  ●  ●        ← 3 dots · 8 dp each · 4 dp gap · uniform opacity     │
╰─                     ← bottom-left corner radius-tiny (4 dp) · tail        │
└─────────────────────────────────────────────────────────────────────────────┘
```

| Slot | Token |
|---|---|
| Bubble fill — Brand / Neutral | `bg.subtle` |
| Bubble fill — OnColor | `bg.brand` |
| Bubble radius | `radius-large` (16 dp) — except bottom-left = `radius-tiny` (4 dp) |
| Bubble padding | 12 h · 12 v (`space-12`) |
| Dot fill — Brand | `bg.brand` |
| Dot fill — Neutral | `text.subtle` |
| Dot fill — OnColor | `icon.inverse` |
| Dot animation | 3-dot bounce — phase-shifted by 150 ms each · `1.0 s` loop |

### Skeleton — content placeholder

```
Type=Line — text block placeholder
┌───── 312 × 96 dp · padding 16 · gap 8 ─────┐
│  ████████████████████████████████          │  280 × 16 · radius-tiny
│  ██████████████████████                    │  200 × 16
│  ████████████████                          │  160 × 16
└─────────────────────────────────────────────┘

Type=Card — image + text
┌───── 328 × 104 dp · radius-large · 1 dp stroke.subtle border · gap 16 ────┐
│  ┌──────┐  █████████████████████████████                                  │
│  │ 72×72│  ███████████████████████                                        │
│  │      │  █████████████████                                              │
│  └──────┘                                                                 │
└────────────────────────────────────────────────────────────────────────────┘

Type=ListItem — avatar + text + trailing
┌── 328 × 64 dp · padding 16h · 12v · gap 16 ──────────────────────────────┐
│  (40 dp)  █████████████████████              ████                         │
│  avatar   ████████████████                                                │
└────────────────────────────────────────────────────────────────────────────┘
```

| Slot | Token |
|---|---|
| Text-line bar fill | `bg.muted` (#E8E4F0) · `radius-tiny` (4 dp) |
| Image / avatar block fill | `bg.muted` |
| Image radius | `radius-medium` (12 dp) |
| Avatar radius | `radius-full` |
| Card container fill | `bg.default` · `radius-large` (16 dp) · 1 dp `stroke.subtle` border |
| Shimmer animation | `bg.muted` → 10 % lighter sweep · 1.5 s linear loop · left → right |

### Brand — full-screen splash / overlay

```
Style=Light                Style=OnBrand                Style=Overlay
┌─ 360×720 ─┐             ┌─ 360×720 ─┐                ┌─ 360×720 ─┐
│ bg.default│             │██bg.brand█│                │▒(content)▒│   overlay.scrim
│           │             │███████████│                │▒(behind)▒▒│   over content
│           │             │███████████│                │▒▒▒▒▒▒▒▒▒▒▒│
│   Wiom    │             │███Wiom████│                │           │
│    ∞      │  → splash   │████∞██████│  → branded     │     ∞     │  → blocking op
│           │             │███████████│     splash     │           │
│ Loading   │             │██Getting██│                │ Processing│
│ your      │             │███things██│                │ payment   │
│ dashboard…│             │███ready…██│                │ …         │
└───────────┘             └───────────┘                └───────────┘
```

| Slot | Light | OnBrand | Overlay |
|---|---|---|---|
| Background | `bg.default` | `bg.brand` | content + `overlay.scrim` (#161021 @ 50 %) |
| Wordmark "Wiom" | `type.heading.xl` · `text.brand` | `type.heading.xl` · `text.onBrand` | (none) |
| Infinity ∞ | 128 × 53 dp · `icon.brand` | 128 × 53 dp · `icon.inverse` | 128 × 53 dp · `icon.inverse` |
| Message | `type.body.md` · `text.subtle` | `type.body.md` · `text.onBrand` | `type.body.md` · `text.onBrand` |
| Frame radius | `radius-xlarge` (24 dp) — only when shown as a sheet · 0 when full-screen | same | same |

---

## 3. Surface contract — loaders are indicator-only

Every Wiom loader **renders the wait indicator and stops**. Message text, surface chrome (cards, snackbars, dialogs), tap targets, and dismiss affordances live on the parent component.

### What this means in code

- **No loader has a `message` parameter.** Pair text by stacking the loader and a `Text` composable in the parent's layout.
- **No loader has a `Modifier.background` / `Modifier.shadow` / `Modifier.border` baked in.** The parent surface owns those (per `wiom-dialog`, `wiom-toast`, `wiom-bottomsheet`, etc.).
- **No loader is tappable.** Loaders are display-only. Dismiss / cancel affordances belong to the surrounding surface (e.g. a `Cancel` CTA in a Loading Dialog).
- **Linear and Dots' Figma containers are illustrative.** In the Figma file, Linear ships inside a 376 × 48 dp `bg.default` card so it's visible on the canvas — but in code the host surface provides the chrome; the loader contributes the 4 dp bar.

### Animation ownership

| Loader | Animation | Owner |
|---|---|---|
| `Spinner` | Continuous clockwise rotation · 1.0 s linear loop | Loader composable (built-in `animateFloatAsState` / `infiniteTransition`) |
| `Linear` Indeterminate | 40 % fill segment travels L → R · 1.5 s loop | Loader composable (built-in) |
| `Linear` Determinate | Fill width animates from previous → caller value | Loader composable (built-in) |
| `Dots` | 3-dot bounce, 150 ms phase shift · 1.0 s loop | Loader composable (built-in) |
| `Skeleton` | 10 %-lighter shimmer sweep · 1.5 s loop · L → R | Loader composable (built-in) |
| `Brand` | Stroke-dash chase on the infinity glyph · 25 % visible · 75 % gap · 1.3 s linear loop · clockwise | Lottie `.json` (pending — falls back to a plain rotating spinner until shipped) |

### Brand-loader stroke-dash chase — implementation note

A single 25 %-long stroke segment travels the closed infinity path with no visible restart and no pause at the crossover. In SVG (or the rendered Lottie):

- `<path>` declares `pathLength="100"` — normalises the dash pattern to the actual path length and guarantees a seamless loop.
- `stroke-dasharray: 25 75;` · `stroke-dashoffset: 0;` · animate offset `0 → -100` over `1.3 s linear` infinite.

In Compose, until the Lottie file ships, render a plain `WiomSpinner.Lg` `OnBrand` color in the infinity slot — visually similar enough for staging.

---

## 4. Picking the right loader

For Wiom users on budget Android (low-spec devices, intermittent connectivity, trust-heavy moments), the loader should match the wait's **duration, layout knowledge, outcome certainty,** and **trust weight**:

| Wait | Layout known? | Trust weight | Use |
|---|---|---|---|
| < 1 s · in a button / field / chip | — | Low | **Spinner Sm** (replaces label · `OnColor` on filled / `Brand` on outlined) |
| 1–3 s · section / card / dialog body | No | Low–Mid | **Spinner Md** (centered in the surface) |
| 1–3 s · list / card / content area | **Yes** | Low | **Skeleton** (`ListItem` × N for lists, `Card` for cards, `Line` for text) |
| 1–5 s · top of a snackbar / inline surface | — | Low | **Linear** (`Indeterminate` if no progress signal) |
| Determinate progress with a known % | — | Low | **Linear** (`Determinate`) — for **milestone / KYC / completion-meter** progress, use `wiom-progress-indicator` instead |
| Chat — assistant / agent composing | — | — | **Dots** (`Brand` for assistant, `Neutral` for human agent, `OnColor` inside brand bubble) |
| Cold start / app launch / post-login splash | — | — | **Brand** (`Light` or `OnBrand`) |
| Blocking critical operation > 3 s — payment, KYC upload, plan activation | — | **High (money / identity)** | **Brand Overlay** with a multi-line message (e.g. "Processing your payment…") |
| Outcome-bearing wait — needs Success / Error confirmation | — | High | **Brand Overlay** during wait → **Bottom Sheet Illustration** (Positive / Critical) on completion |

### Don't reach for a loader when

| Moment | Use instead |
|---|---|
| Empty list with no data ever | Empty state illustration (not Skeleton) |
| Form validation error | Field error state (`wiom-input-fields`) |
| Async background sync that's user-invisible | No loader at all — show stale-then-refresh inline |
| Long determinate progress with milestones (KYC steps, install meter) | `wiom-progress-indicator` — Loader Linear has no step counter |
| Non-essential acknowledgment of a finished wait ("Saved") | `wiom-toast` (don't keep a loader past completion) |

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Spinner

| Slot | Brand | Neutral | OnColor |
|---|---|---|---|
| Track (full ring) | `bg.brand.subtle` (#FFE5F6) | `stroke.subtle` (#D7D3E0) | (no track) |
| Indicator (3/4 arc) | `bg.brand` (#D9008D) | `text.subtle` (#5C5570) | `icon.inverse` (#FAF9FC) |
| Background | transparent | transparent | transparent |

### Linear

| Slot | Token |
|---|---|
| Container fill (Figma) | `bg.default` (#FAF9FC) — illustrative; parent surface owns this in code |
| Container radius | `radius-medium` (12 dp) |
| Container padding | 24 h · 22 v |
| Track | 328 × 4 dp · `bg.brand.subtle` · `radius-full` |
| Fill | `bg.brand` · `radius-full` · width = 40 % (Indeterminate, animated) or caller's value (Determinate) |

### Dots

| Slot | Brand | Neutral | OnColor |
|---|---|---|---|
| Bubble fill | `bg.subtle` (#F1EDF7) | `bg.subtle` | `bg.brand` (#D9008D) |
| Bubble radius | `radius-large` 16 dp · bottom-left `radius-tiny` 4 dp | same | same |
| Bubble padding | 12 h · 12 v (`space-12`) | same | same |
<!-- Bubble padding is on the 4-dp grid (12 V, not 10 V). Older drafts said 10 V; corrected during V2 Android rebuild. -->

| Dot fill | `bg.brand` | `text.subtle` | `icon.inverse` |
| Dot size · gap · opacity | 8 dp · 4 dp · 1.0 — uniform | same | same |

### Skeleton

| Slot | Token |
|---|---|
| Text-line bar fill | `bg.muted` (#E8E4F0) |
| Text-line bar radius | `radius-tiny` (4 dp) |
| Image / avatar block fill | `bg.muted` |
| Image block radius | `radius-medium` (12 dp) |
| Avatar block radius | `radius-full` |
| Card container fill | `bg.default` |
| Card container radius | `radius-large` (16 dp) |
| Card container border | 1 dp · `stroke.subtle` |
| Shimmer | `bg.muted` → 10 % lighter sweep · 1.5 s linear loop |

### Brand

| Slot | Light | OnBrand | Overlay |
|---|---|---|---|
| Background | `bg.default` | `bg.brand` | content + `overlay.scrim` (#161021 @ 50 %) |
| Wordmark | `type.heading.xl` (32/44 Bold) · `text.brand` | `type.heading.xl` · `text.onBrand` (#FFFFFF) | (none) |
| Infinity glyph | 128 × 53 dp · `icon.brand` | 128 × 53 dp · `icon.inverse` | 128 × 53 dp · `icon.inverse` |
| Message | `type.body.md` (14/20) · `text.subtle` | `type.body.md` · `text.onBrand` | `type.body.md` · `text.onBrand` |
| Frame radius (when sheet-presented) | `radius-xlarge` (24 dp) | same | same |

---

## 6. Universal rules

1. **Loaders are indicator-only.** No `message` parameter, no surface chrome, no tap target, no dismiss. The parent component (dialog / toast / bottom sheet / chat host / button) owns those.
2. **Don't expose `Modifier.size` for Spinner / Dots.** Each variant has one bounding box; resizing distorts the indicator. Use the variant enum.
3. **Match loader to wait shape (per §4).** Layout-known waits → Skeleton; quick inline → Spinner Sm; chat → Dots; full-screen branded → Brand; short progress → Linear; long milestone progress → `wiom-progress-indicator`.
4. **`OnColor` only on dark / brand-filled surfaces.** White spinner on white reads as nothing; `OnColor` exists for filled CTAs, brand bubbles, and the `OnBrand` / `Overlay` Brand loaders.
5. **Brand `Overlay` is for blocking critical ops only.** Payment, KYC upload, plan activation, refund processing — moments where the user must wait for an outcome. Don't use `Overlay` for routine waits; that's `Spinner Md` / `Skeleton`.
6. **Don't keep a loader past completion.** When the wait ends in success, dismiss the loader and route to the outcome surface — `wiom-bottomsheet` Illustration (Positive / Critical) for trust-heavy outcomes, `wiom-toast` for non-essential acknowledgments. Loaders show *that the wait is happening*, not *what happened*.
7. **Skeleton mirrors the post-load layout.** A 3-row `ListItem` skeleton must show 3 rows of approximately the same dimensions as the real list rows. Don't show 5 skeleton rows if 3 is the real result count — that breaks expectation.
8. **Don't pair Spinner with Linear in the same wait.** Pick one indicator per surface; doubling them clutters the visual.
9. **Brand loader is reserved for branded moments.** Cold start / first launch / branded splash / blocking critical ops. Don't use it for routine 1–3 s waits.
10. **Tokens only — element-first.** No raw hex. No legacy `colors.*` namespaces; use `Wiom.bg.brand` / `Wiom.text.subtle` / `Wiom.bg.muted`.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomSpinnerSize  { Sm, Md, Lg }
enum class WiomSpinnerColor { Brand, Neutral, OnColor }
enum class WiomLinearType   { Indeterminate, Determinate }
enum class WiomDotsColor    { Brand, Neutral, OnColor }
enum class WiomSkeletonType { Line, Card, ListItem }
enum class WiomBrandLoaderStyle { Light, OnBrand, Overlay }

@Composable
fun WiomSpinner(
    size: WiomSpinnerSize = WiomSpinnerSize.Md,
    color: WiomSpinnerColor = WiomSpinnerColor.Brand,
    modifier: Modifier = Modifier,
)

// Indeterminate: progress = null. Determinate: progress = 0f..1f.
@Composable
fun WiomLinearProgress(
    progress: Float? = null,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomDots(
    color: WiomDotsColor = WiomDotsColor.Brand,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomSkeleton(
    type: WiomSkeletonType = WiomSkeletonType.Line,
    modifier: Modifier = Modifier,
)

@Composable
fun WiomBrandLoader(
    style: WiomBrandLoaderStyle = WiomBrandLoaderStyle.Light,
    message: String? = null,                              // optional caption — Brand has its own message slot
    modifier: Modifier = Modifier,
)
```

### `WiomSpinner` impl

```kotlin
@Composable
fun WiomSpinner(
    size: WiomSpinnerSize = WiomSpinnerSize.Md,
    color: WiomSpinnerColor = WiomSpinnerColor.Brand,
    modifier: Modifier = Modifier,
) {
    val box = when (size) {
        WiomSpinnerSize.Sm -> 20.dp
        WiomSpinnerSize.Md -> 32.dp
        WiomSpinnerSize.Lg -> 48.dp
    }
    val stroke = when (size) {
        WiomSpinnerSize.Sm -> 2.dp
        WiomSpinnerSize.Md -> 3.dp
        WiomSpinnerSize.Lg -> 4.dp
    }
    val (track, indicator) = when (color) {
        WiomSpinnerColor.Brand   -> Wiom.bg.brand.subtle to Wiom.bg.brand
        WiomSpinnerColor.Neutral -> Wiom.stroke.subtle   to Wiom.text.subtle
        WiomSpinnerColor.OnColor -> Color.Transparent    to Wiom.icon.inverse
    }

    val transition = rememberInfiniteTransition(label = "WiomSpinner")
    val angle by transition.animateFloat(
        initialValue = 0f,
        targetValue = 360f,
        animationSpec = infiniteRepeatable(
            animation = tween(1000, easing = LinearEasing),
            repeatMode = RepeatMode.Restart,
        ),
        label = "angle",
    )

    Canvas(modifier = modifier.size(box)) {
        val s = stroke.toPx()
        val arcSize = Size(this.size.width - s, this.size.height - s)
        val topLeft = Offset(s / 2, s / 2)
        if (track != Color.Transparent) {
            drawArc(
                color = track,
                startAngle = 0f, sweepAngle = 360f, useCenter = false,
                topLeft = topLeft, size = arcSize,
                style = Stroke(width = s, cap = StrokeCap.Round),
            )
        }
        rotate(angle) {
            drawArc(
                color = indicator,
                startAngle = -90f, sweepAngle = 270f, useCenter = false,
                topLeft = topLeft, size = arcSize,
                style = Stroke(width = s, cap = StrokeCap.Round),
            )
        }
    }
}
```

### `WiomLinearProgress` impl

```kotlin
@Composable
fun WiomLinearProgress(
    progress: Float? = null,
    modifier: Modifier = Modifier,
) {
    val isIndeterminate = progress == null
    val animatedFraction = if (!isIndeterminate) {
        animateFloatAsState(
            targetValue = progress.coerceIn(0f, 1f),
            animationSpec = tween(durationMillis = 300, easing = LinearOutSlowInEasing),
            label = "WiomLinearDeterminate",
        ).value
    } else 0f

    val transition = rememberInfiniteTransition(label = "WiomLinearIndeterminate")
    val travel by transition.animateFloat(
        initialValue = -0.4f,
        targetValue = 1.0f,
        animationSpec = infiniteRepeatable(
            animation = tween(1500, easing = LinearEasing),
            repeatMode = RepeatMode.Restart,
        ),
        label = "travel",
    )

    Canvas(
        modifier = modifier
            .height(4.dp)
            .fillMaxWidth()
            .clip(RoundedCornerShape(percent = 50))
            .background(Wiom.bg.brand.subtle),
    ) {
        if (isIndeterminate) {
            val w = size.width * 0.4f
            val x = travel * size.width
            drawRect(color = Wiom.bg.brand, topLeft = Offset(x, 0f), size = Size(w, size.height))
        } else {
            val w = size.width * animatedFraction
            drawRect(color = Wiom.bg.brand, topLeft = Offset.Zero, size = Size(w, size.height))
        }
    }
}
```

### `WiomDots` impl

```kotlin
@Composable
fun WiomDots(
    color: WiomDotsColor = WiomDotsColor.Brand,
    modifier: Modifier = Modifier,
) {
    val (bubble, dot) = when (color) {
        WiomDotsColor.Brand   -> Wiom.bg.subtle to Wiom.bg.brand
        WiomDotsColor.Neutral -> Wiom.bg.subtle to Wiom.text.subtle
        WiomDotsColor.OnColor -> Wiom.bg.brand  to Wiom.icon.inverse
    }
    val shape = RoundedCornerShape(topStart = 16.dp, topEnd = 16.dp, bottomEnd = 16.dp, bottomStart = 4.dp)

    Box(
        modifier = modifier
            .background(bubble, shape)
            .padding(horizontal = 12.dp, vertical = 10.dp),
    ) {
        Row(horizontalArrangement = Arrangement.spacedBy(4.dp)) {
            repeat(3) { i ->
                BouncingDot(color = dot, phaseMillis = i * 150)
            }
        }
    }
}

@Composable
private fun BouncingDot(color: Color, phaseMillis: Int) {
    val transition = rememberInfiniteTransition(label = "WiomDotsBounce")
    val offset by transition.animateFloat(
        initialValue = 0f,
        targetValue = 1f,
        animationSpec = infiniteRepeatable(
            animation = keyframes {
                durationMillis = 1000
                delayMillis = phaseMillis
                0f at 0
                -3f at 200
                0f at 400
            },
            repeatMode = RepeatMode.Restart,
        ),
        label = "offset",
    )
    Box(
        modifier = Modifier
            .offset(y = offset.dp)
            .size(8.dp)
            .background(color, CircleShape),
    )
}
```

### `WiomSkeleton` impl (Line / Card / ListItem)

```kotlin
@Composable
fun WiomSkeleton(
    type: WiomSkeletonType = WiomSkeletonType.Line,
    modifier: Modifier = Modifier,
) {
    when (type) {
        WiomSkeletonType.Line     -> SkeletonLine(modifier)
        WiomSkeletonType.Card     -> SkeletonCard(modifier)
        WiomSkeletonType.ListItem -> SkeletonListItem(modifier)
    }
}

@Composable
private fun SkeletonLine(modifier: Modifier = Modifier) {
    Column(
        modifier = modifier
            .fillMaxWidth()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
    ) {
        ShimmerBar(width = 280.dp, height = 16.dp)
        ShimmerBar(width = 200.dp, height = 16.dp)
        ShimmerBar(width = 160.dp, height = 16.dp)
    }
}

@Composable
private fun SkeletonCard(modifier: Modifier = Modifier) {
    Row(
        modifier = modifier
            .fillMaxWidth()
            .background(Wiom.bg.default, RoundedCornerShape(16.dp))
            .border(1.dp, Wiom.stroke.subtle, RoundedCornerShape(16.dp))
            .padding(16.dp),
        horizontalArrangement = Arrangement.spacedBy(16.dp),
    ) {
        ShimmerBlock(modifier = Modifier.size(72.dp), shape = RoundedCornerShape(12.dp))
        Column(verticalArrangement = Arrangement.spacedBy(8.dp), modifier = Modifier.weight(1f)) {
            ShimmerBar(width = 240.dp, height = 16.dp)
            ShimmerBar(width = 200.dp, height = 16.dp)
            ShimmerBar(width = 160.dp, height = 16.dp)
        }
    }
}

@Composable
private fun SkeletonListItem(modifier: Modifier = Modifier) {
    Row(
        modifier = modifier
            .fillMaxWidth()
            .padding(horizontal = 16.dp, vertical = 12.dp),
        horizontalArrangement = Arrangement.spacedBy(16.dp),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        ShimmerBlock(modifier = Modifier.size(40.dp), shape = CircleShape)
        Column(verticalArrangement = Arrangement.spacedBy(8.dp), modifier = Modifier.weight(1f)) {
            ShimmerBar(width = 200.dp, height = 16.dp)
            ShimmerBar(width = 160.dp, height = 16.dp)
        }
        ShimmerBlock(modifier = Modifier.size(width = 60.dp, height = 16.dp), shape = RoundedCornerShape(4.dp))
    }
}

@Composable
private fun ShimmerBar(width: Dp, height: Dp) {
    ShimmerBlock(
        modifier = Modifier.size(width = width, height = height),
        shape = RoundedCornerShape(4.dp),
    )
}

@Composable
private fun ShimmerBlock(modifier: Modifier = Modifier, shape: Shape) {
    val transition = rememberInfiniteTransition(label = "WiomSkeletonShimmer")
    val translate by transition.animateFloat(
        initialValue = 0f,
        targetValue = 1f,
        animationSpec = infiniteRepeatable(
            animation = tween(1500, easing = LinearEasing),
            repeatMode = RepeatMode.Restart,
        ),
        label = "translate",
    )
    val brush = Brush.linearGradient(
        colors = listOf(Wiom.bg.muted, Wiom.bg.muted.copy(alpha = 0.5f), Wiom.bg.muted),
        start = Offset(translate * 1000f - 1000f, 0f),
        end = Offset(translate * 1000f, 0f),
    )
    Box(modifier = modifier.background(brush, shape))
}
```

### `WiomBrandLoader` impl

```kotlin
@Composable
fun WiomBrandLoader(
    style: WiomBrandLoaderStyle = WiomBrandLoaderStyle.Light,
    message: String? = null,
    modifier: Modifier = Modifier,
) {
    val background = when (style) {
        WiomBrandLoaderStyle.Light   -> Wiom.bg.default
        WiomBrandLoaderStyle.OnBrand -> Wiom.bg.brand
        WiomBrandLoaderStyle.Overlay -> Wiom.bg.scrim                  // sits over content
    }
    val showWordmark = style != WiomBrandLoaderStyle.Overlay
    val wordmarkColor = if (style == WiomBrandLoaderStyle.OnBrand) Wiom.text.onBrand else Wiom.text.brand
    val infinityTint = if (style == WiomBrandLoaderStyle.Light) Wiom.icon.brand else Wiom.icon.inverse
    val messageColor = if (style == WiomBrandLoaderStyle.Light) Wiom.text.subtle else Wiom.text.onBrand

    Box(
        modifier = modifier
            .fillMaxSize()
            .background(background),
        contentAlignment = Alignment.Center,
    ) {
        Column(
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.spacedBy(16.dp),
        ) {
            if (showWordmark) {
                Text("Wiom", style = Wiom.type.heading.xl, color = wordmarkColor)
            }
            // Until the Lottie ships, use a spinner Lg as a structural stand-in.
            // When Lottie ships, replace with LottieAnimation(infinity_chase.json).
            WiomSpinner(
                size = WiomSpinnerSize.Lg,
                color = if (style == WiomBrandLoaderStyle.Light) WiomSpinnerColor.Brand else WiomSpinnerColor.OnColor,
            )
            if (message != null) {
                Text(message, style = Wiom.type.body.md, color = messageColor, textAlign = TextAlign.Center)
            }
        }
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material CircularProgressIndicator with raw colour, hand-rolled message,
//             custom size override that breaks the contract.
CircularProgressIndicator(
    color = Color(0xFFD9008D),
    modifier = Modifier.size(28.dp),                                  // not a ship size
)
Text("Loading…", color = Color.Gray)                                  // hand-rolled colour

// ✅ RIGHT — WiomSpinner renders only the indicator; parent provides message + spacing.
Column(
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(16.dp),
) {
    WiomSpinner(size = WiomSpinnerSize.Md, color = WiomSpinnerColor.Brand)
    Text("Loading your dashboard…", style = Wiom.type.body.md, color = Wiom.text.subtle)
}
```

---

## 8. Recipes

### Button loading state

```kotlin
// In wiom-cta — Loading variant uses WiomSpinner Sm.
WiomCta(
    label = "Pay",
    type = WiomCtaType.Primary,
    state = WiomCtaState.Loading,                            // CTA replaces label with WiomSpinner Sm OnColor
    onClick = {/* disabled while loading */},
)
```

### Inline field validation

```kotlin
// In wiom-input-fields — Spinner Sm in the trailing slot during validation.
WiomInputField(
    type = WiomInputFieldType.Default,
    value = phone,
    onValueChange = { phone = it },
    trailing = if (validating) {
        { WiomSpinner(size = WiomSpinnerSize.Sm, color = WiomSpinnerColor.Neutral) }
    } else null,
)
```

### Page-level wait with message

```kotlin
Column(
    modifier = Modifier.fillMaxSize().padding(32.dp),
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(16.dp, Alignment.CenterVertically),
) {
    WiomSpinner(size = WiomSpinnerSize.Md, color = WiomSpinnerColor.Brand)
    Text("Loading your dashboard…", style = Wiom.type.body.md, color = Wiom.text.subtle)
}
```

### Transaction list (Skeleton ListItem × N)

```kotlin
if (transactions == null) {
    Column(verticalArrangement = Arrangement.spacedBy(0.dp)) {
        repeat(5) { WiomSkeleton(type = WiomSkeletonType.ListItem) }
    }
} else {
    Column { transactions.forEach { TransactionRow(it) } }
}
```

### Chat — assistant typing

```kotlin
// At the end of the message list when the bot is composing.
Box(modifier = Modifier.padding(start = 16.dp, top = 8.dp)) {
    WiomDots(color = WiomDotsColor.Brand)
}
```

### Linear progress in a snackbar / inline surface

```kotlin
// Indeterminate while no progress signal:
WiomLinearProgress(
    progress = null,
    modifier = Modifier.padding(horizontal = 24.dp, vertical = 22.dp),
)

// Determinate when a percentage is available (short waits only —
// for KYC-style milestone progress use wiom-progress-indicator):
WiomLinearProgress(progress = uploadFraction)
```

### Cold start splash (Brand Light)

```kotlin
WiomBrandLoader(
    style = WiomBrandLoaderStyle.Light,
    message = "Loading your dashboard…",
)
```

### Blocking critical op — payment processing (Brand Overlay)

```kotlin
if (paymentInFlight) {
    WiomBrandLoader(
        style = WiomBrandLoaderStyle.Overlay,
        message = "Processing your payment…",
    )
}
// On completion, dismiss the overlay AND route to the outcome surface
// (wiom-bottomsheet Illustration · Positive / Critical) — never linger on the loader.
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Material `CircularProgressIndicator` / `LinearProgressIndicator` directly — wrong tokens, wrong stroke, no Wiom motion contract.
- ❌ `Modifier.size(…)` to resize a Spinner / Dots indicator — pick a different size variant; the ones that ship are the contract.
- ❌ Adding a `message` parameter inside `WiomSpinner` / `WiomDots` — labels live in the parent's layout.
- ❌ Building Skeleton manually with `Box` + `Modifier.background(Wiom.bg.muted)` — always use `WiomSkeleton(type = …)`; you'll miss the shimmer animation.
- ❌ Skeleton row count that doesn't match the post-load result count — breaks user expectation.
- ❌ `Brand Overlay` for routine 1–3 s waits — use `Spinner Md` / `Skeleton`. Overlay is for blocking critical ops only.
- ❌ Keeping the loader visible past completion ("It's done but the spinner is still spinning") — dismiss + route to the outcome surface.
- ❌ Pairing Linear and Spinner on the same surface — pick one indicator.
- ❌ White spinner on white background (`OnColor` on `bg.default`) — `OnColor` is for dark / brand-filled surfaces only.
- ❌ Using Loader Linear for milestone progress (KYC steps, install meter) — use `wiom-progress-indicator`.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Loader canvas: `3069:6033` (Section 1).
Re-pull `get_variable_defs` on each component set if foundations token names ever change.

| Node | Component / variant |
|---|---|
| `2043:38` | Spinner component set — 9 ship variants |
| `2043:5 / :9 / :13` | Spinner · `sm` × `brand / neutral / onColor` |
| `2043:16 / :20 / :24` | Spinner · `md` × `brand / neutral / onColor` |
| `2043:27 / :31 / :35` | Spinner · `lg` × `brand / neutral / onColor` |
| `2048:8` | Linear component set — 2 ship variants |
| `2048:2 / :5` | Linear · `Indeterminate` / `Determinate` |
| `2052:17` | Dots component set — 3 ship variants |
| `2052:2 / :7 / :12` | Dots · `brand` / `neutral` / `onColor` |
| `2053:28` | Skeleton component set — 3 ship variants |
| `2053:2 / :9 / :19` | Skeleton · `Line` / `Card` / `ListItem` |
| `2061:26` | Brand component set — 3 ship variants |
| `2061:2 / :7 / :13` | Brand · `Light` / `OnBrand` / `Overlay` |
| `2068:48` | Infinity glyph (BOOLEAN_OPERATION Union, 22 × 9 base; 128 × 53 in Brand) |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The ship has **5 component sets · 20 variants total** (Spinner 9 + Linear 2 + Dots 3 + Skeleton 3 + Brand 3). Older skill drafts described a `Loader/Status` set ("Wait → Success → Error") with TBD node IDs — that set was aspirational and **never shipped**. Outcome confirmation is now owned by **`wiom-bottomsheet` Illustration** (Positive / Critical tone) — the loader runs during the wait, the bottom sheet renders the outcome.
- **Linear is NOT deprecated.** Older drafts marked it "deprecated; use Status." The V2 ship has Linear alive with two variants — `Indeterminate` and `Determinate`. Use it for short waits with no milestones; for KYC / install / milestone progress use `wiom-progress-indicator`.
- **Brand-loader animation is stroke-dash chase via Lottie** — Lottie `.json` is pending. Until it ships, the Compose impl renders a `WiomSpinner.Lg` (`Brand` or `OnColor` per style) in the infinity slot as a structural stand-in. The Figma frame shows the static infinity glyph; motion is documented in §3.
- **`Loader/Linear` ships inside a 376 × 48 dp `bg.default` card** in the Figma file — that's illustrative only. In code the parent surface (snackbar / inline strip) provides the chrome; the loader contributes the 4 dp bar via `WiomLinearProgress`.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.brand` / `bg.brand.subtle` / `bg.muted` / `bg.subtle` / `bg.default` / `overlay.scrim`, `text.subtle` / `text.brand` / `text.onBrand`, `icon.brand` / `icon.inverse`, `stroke.subtle`, `radius-tiny` / `radius-medium` / `radius-large` / `radius-xlarge` / `radius-full`, `type.heading.xl` / `type.body.md`.
- `wiom-cta` — `Loading` state composes `WiomSpinner.Sm` (`OnColor` on Primary fills, `Brand` on Secondary / Tertiary outlines).
- `wiom-input-fields` — `WiomSpinner.Sm` in the trailing slot during async validation.
- `wiom-dialog` — Loading dialog type composes `WiomSpinner.Md` or `WiomBrandLoader.Light` as its body.
- `wiom-bottomsheet` — outcome surface that takes over from a loader at completion (Illustration · Positive / Critical for trust-heavy moments).
- `wiom-toast` — non-essential post-wait acknowledgment. Don't keep a loader running past the wait — dismiss and toast (or sheet, for money moments).
- `wiom-progress-indicator` — long-form determinate progress with milestones, percentage labels, or step counters. Loader Linear is for short, simple waits; the progress-indicator skill is for KYC / install meters / multi-stage completion.
- `wiom-compose-layout-rules` — Modifier discipline; `infiniteRepeatable` for animation; no `Modifier.size` overrides on Spinner / Dots.
