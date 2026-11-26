---
name: wiom-top-bar
description: >
  Wiom Top Bar — the persistent edge-to-edge header at the top of every
  screen. 1 component, 3 sizes (Small / Medium / Large), 8 ship
  variants. Reference for generating Jetpack Compose code with
  WiomTopBar and for instancing the Figma component (set 1576:143) via
  MCP. Owns the top-of-screen real estate including the OS status-bar
  strip above it. Use this skill EVERY TIME you create, edit, or
  review the top header on any screen, dialog, or full-screen sheet
  in the Wiom Partner or Customer Android app, or when assembling
  Figma frames with a top bar. Trigger on: TopBar, TopAppBar, Header,
  ActionBar, AppBar, NavigationBar (top), back arrow, hamburger,
  search header, screen title, status bar, edge-to-edge,
  enableEdgeToEdge, statusBarColor, instance of 1576:143.
---

# Wiom Top Bar

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
set `1576:143`, and the `wiom-design-foundations` token set.

**1 component · 3 sizes · 8 ship variants · edge-to-edge.**

| Size | Height | States |
|---|---|---|
| Small | 64 dp | Default · Centered · Scrolled · Search (4) |
| Medium | 112 dp | Default · Scrolled (2) |
| Large | 152 dp | Default · Scrolled (2) |

The top bar is **the surface that owns the top of the screen** — it
spans full width edge-to-edge (no horizontal screen margin), and it is
visually continuous with the OS status bar strip above it. Every screen
in the app starts with this component.

---

## 1. Figma — instancing the component

When assembling a Figma frame via MCP, instance the component set
`1576:143` and set the `Size` + `State` variants plus the booleans /
texts / icon swaps. Strings are **case-sensitive**.

### Variants — 8 ship combinations

| `Size` | `State` | Node | Use for |
|---|---|---|---|
| `Small` | `Default` | `1576:2` | Most screens; back arrow + title + 1–3 trailing actions |
| `Small` | `Centered` | `1576:19` | Modal / sheet headers: optional close leading, optional subtitle, max 1 trailing (centered title) |
| `Small` | `Scrolled` | `1576:36` | Same as Default + `shadow.sm` (used when content has scrolled past edge) |
| `Small` | `Search` | `1576:53` | Back + inline search pill + optional trailing text CTA |
| `Medium` | `Default` | `1576:71` | Section pages where title needs more presence (`type.heading.lg`) |
| `Medium` | `Scrolled` | `1576:89` | Medium with `shadow.sm` |
| `Large` | `Default` | `1576:107` | Hero pages (`type.heading.xl`) — wallet, dashboard, profile root |
| `Large` | `Scrolled` | `1576:125` | Large with `shadow.sm` |

### Properties (booleans + text + instance-swap, on every variant)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Title` | Text | `"Title"` | Screen title |
| `Subtitle` | Text | `"Subtitle"` | Optional second line (Small Default / Centered only) |
| `Show Subtitle` | Boolean | `false` | Reveals the subtitle slot below the title (Small only) |
| `Show Leading` | Boolean | `true` | Reveals the leading icon button (typically `arrow_back`) |
| `Leading Icon` | Instance swap | `arrow_back` | Swap to any 24 dp Material Symbols Rounded icon |
| `Show Trailing Text` | Boolean | `true` | Reveals a text CTA slot at the start of the trailing area (e.g. `Save`, `Edit`) |
| `Trailing Text` | Text | `"Save"` | Editable label, always renders as `text.brand` |
| `Show Trailing 1` · `Trailing Icon 1` | Boolean · Instance swap | `false` · `search` | First trailing icon button |
| `Show Trailing 2` · `Trailing Icon 2` | Boolean · Instance swap | `true` · `notifications` | Second trailing icon button |
| `Show Trailing 3` · `Trailing Icon 3` | Boolean · Instance swap | `true` · `more_vert` | Third trailing icon button |

> The default ship state of every variant has `Trailing 2 + 3 + Text =
> true` (`Save` text + `notifications` + `more_vert`). For most
> screens you'll override these — set `Show Trailing Text=false` if
> there's no inline action; set the boolean to `false` for any unused
> trailing icon.

### Code enum ↔ Figma variant string

| `WiomTopBarSize` (Kotlin) | Figma `Size` value |
|---|---|
| `Small` | `Small` |
| `Medium` | `Medium` |
| `Large` | `Large` |

| `WiomTopBarState` (Kotlin) | Figma `State` value | Available on |
|---|---|---|
| `Default` | `Default` | Small · Medium · Large |
| `Centered` | `Centered` | Small only |
| `Scrolled` | `Scrolled` | Small · Medium · Large |
| `Search` | `Search` | Small only |

---

## 2. Anatomy

All sizes share the same column structure: `Action row` (icons + title
on Small; icons only on Medium/Large) optionally followed by a
`Title area` (Medium / Large only).

### Small — 64 dp tall, single row

```
┌────────────────────────────── 360 dp (edge-to-edge) ───────────────────────────────┐
│                                                                                     │
│  ●Lead   Title (type.title.lg, 20sp Medium)        Save  ●T1  ●T2  ●T3            │
│  48dp    Subtitle (type.body.sm, optional)         CTA   48dp 48dp 48dp           │
│                                                                                     │
└─────────────────────────────────────── 64 dp ──────────────────────────────────────┘
        ↑                                                                       ↑
   space-4 outer L                                                       space-4 outer R
```

| Slot | Value |
|---|---|
| Container width | Edge-to-edge — `fillMaxWidth()` |
| Container height | 64 dp (fixed) |
| Outer padding | `space-4` H · `space-8` V |
| Each icon-button cell | `space-12` all-sides padding around 24 dp glyph → 48 dp hit target |
| Title column padding | `space-12` H |
| Icon size | 24 dp |

(Token mappings — typography, fills, tints — live in §5.)

### Search (Small only) — 64 dp

```
┌────────────────────────────── 360 dp ──────────────────────────────────────────────┐
│                                                                                     │
│  ●Lead   ┌─── search pill (bg.subtle, radius-full) ─────────┐    Save              │
│  48dp    │  ●search 20dp   "Search plans, recharges…"      │    CTA               │
│          └────────────────────────────────────────────────────┘                    │
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
        ↑                                                                       ↑
   space-4 L                                                                space-16 R
                                                                       (asymmetric — pill goes
                                                                        nearly to the edge)
```

Search-specific:

- Container outer padding: **`space-4` left, `space-16` right** (asymmetric — the only ship state with non-symmetric H padding)
- Search pill: `bg.subtle` fill, `radius-full`, `space-16` H pad, `space-8` V pad, gap `space-12` between leading icon and placeholder
- Pill leading icon: 20 dp `search` (smaller than the 24 dp action icons)
- Pill placeholder text: `type.body.lg` (16sp Regular) `text.disabled`
- No `Trailing 1 / 2 / 3` icons in this state — only optional `Trailing Text`

### Medium — 112 dp, action row + title area

```
┌────────────────────────────── 360 dp ──────────────────────────────────────────────┐
│  ●Lead                                              Save  ●T1  ●T2  ●T3            │   ← Action row 48dp
│  48dp                                               CTA  48dp 48dp 48dp            │
├─────────────────────────────────────────────────────────────────────────────────────┤
│  Title (type.heading.lg, 24sp Bold)                                                │   ← Title area 64dp
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

- **Action row** (top, same layout as Small Default) — `space-4` H, `space-8` V, height 48 dp
- **Title area** (bottom) — `space-16` H, `space-8` V, fills remaining height (~64 dp); title is `type.heading.lg` 24 sp Bold / 32 LH `text.default`, no subtitle
- No title / subtitle in the Action row — title moves to the dedicated area below

### Large — 152 dp, action row + larger title area

```
┌────────────────────────────── 360 dp ──────────────────────────────────────────────┐
│  ●Lead                                              Save  ●T1  ●T2  ●T3            │   ← Action row 48dp
│  48dp                                               CTA  48dp 48dp 48dp            │
├─────────────────────────────────────────────────────────────────────────────────────┤
│                                                                                     │
│  Title (type.heading.xl, 32sp Bold)                                                │   ← Title area 104dp
│                                                                                     │
└─────────────────────────────────────────────────────────────────────────────────────┘
```

- **Action row** — same as Medium / Small
- **Title area** — `space-16` H, `space-12` top, `space-32` bottom, fills the remaining ~104 dp; title is `type.heading.xl` 32 sp Bold / 44 LH `text.default`

**No fixed dimensions** can be stretched — sizes are part of the
component contract. Width is `fillMaxWidth()` (edge-to-edge). Heights
are fixed per Size variant: 64 / 112 / 152 dp.

---

## 3. Surface contract — edge-to-edge

The top bar is **not** a surface that respects the standard 16 dp screen
gutter. It owns the full width of the screen and sits flush against
both edges. This is unique to the top bar (and the bottom navigation
bar); every other component inside the screen body uses the 16 dp
gutter.

### What this means in code

- `Modifier.fillMaxWidth()` on the top bar — no horizontal padding
  applied by the parent.
- Inside the top bar, **the component itself owns its inner padding**
  (`space-4` outer + per-cell `space-12`). Don't add a parent
  `Modifier.padding(horizontal = ...)`.
- **Status bar inset** (the OS strip above) is consumed by the top bar
  via `WindowInsets.statusBars`, not by the screen content. The top
  bar sits *under* the status bar visually (`bg.default` continues
  upward into it) but the content of the bar (icons, title) lives
  below the status bar inset.

### Visual continuity with the status bar

The status bar should appear as **part of the top bar surface**, not a
separate strip. The host activity must:

1. Enable edge-to-edge — `enableEdgeToEdge()` in `onCreate`, or
   `WindowCompat.setDecorFitsSystemWindows(window, false)`.
2. Pad the top bar's inside-content by `WindowInsets.statusBars` so
   icons and title don't render *under* the status bar. The container
   fill (`bg.default`) extends behind the status bar so they read as
   one continuous surface.
3. Set the status bar **content color** (icon / time / signal glyphs)
   to match the top bar's lightness — see §6.

---

## 4. States

| State | Visual signal | Use when |
|---|---|---|
| `Default` | No shadow, `bg.default` flat | Resting state of any screen — what you see at scroll position 0 |
| `Centered` *(Small only)* | Title column (and optional subtitle) centred in the space between leading and trailing, not on the screen | Modal / full-screen sheet headers: close leading allowed, subtitle allowed, max 1 trailing action (as shipped, `WiomTopBar.kt:50`, `:276-310`; aligned with the shipped library and iOS skills, 26 Nov 2026) |
| `Scrolled` *(all sizes)* | `bg.default` + `shadow.sm` drop shadow | Switch from `Default` once content has scrolled past the bar's bottom edge — the shadow separates the bar from the content underneath |
| `Search` *(Small only)* | Title replaced by an inline `bg.subtle` search pill, asymmetric padding (`pl-4 pr-16`) | Search screens — back arrow + inline pill + optional `Save` / cancel text CTA |

The Scrolled state is **not** a different component — it's the same
top bar with a `shadow.sm` modifier added in response to scroll
position. Wire it via a `LazyListState` / `ScrollState` listener.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

| Slot | Token (light theme) |
|---|---|
| Container fill | `bg.default` (#FAF9FC) |
| Title color | `text.default` (#161021) |
| Subtitle color | `text.subtle` (#5C5570) |
| Trailing Text CTA color | `text.brand` (#D9008D) |
| Leading / Trailing icon tint | `icon.action` (#352D42) |
| Search pill fill | `bg.subtle` (#F1EDF7) |
| Search pill placeholder | `text.disabled` (#A7A1B2) |
| Search pill leading icon | `icon.nonAction` (decorative, as shipped) |
| Search pill cursor | `text.default` |
| Search pill outer margin | `space-8` horizontal around the pill |
| Scrolled state shadow | `shadow.sm` (drop, `0 1px 3px rgba(0,0,0,0.15)`) |
| Title typography (Small) | `type.title.lg` (Noto Sans Medium 20 / 28 LH) |
| Title typography (Medium) | `type.heading.lg` (Bold 24 / 32 LH) |
| Title typography (Large) | `type.heading.xl` (Bold 32 / 44 LH) |
| Subtitle typography | `type.body.sm` (Regular 12 / 16 LH) |
| Trailing Text typography | `type.label.md` (SemiBold 14 / 20 LH) |
| Search pill placeholder typography | `type.body.lg` (Regular 16 / 24 LH) |

---

## 6. Status bar pairing

The OS status bar (the strip above the top bar showing time / battery /
signal) is **not part of the Figma component** but is part of the user-
visible top-of-screen surface. Pair it with the top bar so they read as
one continuous block.

### Light top bar (the default ship)

The 8 Figma variants all use `bg.default` — a light, near-white surface.

| Status bar property | Value |
|---|---|
| Status bar background | `bg.default` (#FAF9FC) — match the top bar exactly |
| Status bar content (icons / time / signal glyphs) | **dark** — `isAppearanceLightStatusBars = true` |

```kotlin
// In Activity.onCreate
enableEdgeToEdge(
    statusBarStyle = SystemBarStyle.light(
        scrim = Wiom.bg.default.toArgb(),         // status bar bg
        darkScrim = Wiom.bg.default.toArgb(),
    )
)
```

Or imperatively per-screen if you can't use `enableEdgeToEdge`:

```kotlin
val view = LocalView.current
val color = Wiom.bg.default
SideEffect {
    val window = (view.context as Activity).window
    window.statusBarColor = color.toArgb()
    WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = true
}
```

### Dark top bar (Premium / Partner header — foundations rule)

Per `wiom-design-foundations`, the **non-action premium / partner
header** uses `bg.brand.bold` (#443152, Brand_900) with `text.inverse`
text. This is **not a Figma variant** of the top bar — it's a
foundations-level theming override applied to the same component, used
sparingly for partner-app branded headers and customer-app premium
screens.

When you switch to a dark top bar:

| Slot | Token (dark theme) |
|---|---|
| Container fill | `bg.brand.bold` (#443152) — the deepest brand neutral |
| Title color | `text.inverse` (#FAF9FC) |
| Trailing Text CTA color | `text.inverse` (brand pink fails AA on `bg.brand.bold` — use inverse, not `text.brand`) |
| Leading / Trailing icon tint | `icon.inverse` (#FAF9FC) |
| Scrolled state shadow | `shadow.sm` (still works on dark — the shadow is alpha-on-content) |

**Dark variant constraints — supported states only:**

Foundations only commits `bg.brand.bold` + `text.inverse` + `icon.inverse`. There is no dimmed-inverse text token and no translucent-inverse surface token. Therefore on the dark variant:

- ✅ **Supported**: Small × Default · Small × Scrolled · Small × Centered · Medium × Default · Medium × Scrolled · Large × Default · Large × Scrolled.
- ❌ **Not supported**: `Show Subtitle = true` (no dimmed-inverse text token for the subtle line) and `State = Search` (no translucent-inverse pill fill that contrasts with `bg.brand.bold`). If the design needs either, switch to the light variant or request the missing tokens from foundations.

**Status bar (dark top bar):**

| Status bar property | Value |
|---|---|
| Status bar background | `bg.brand.bold` (#443152) — match the top bar |
| Status bar content (icons / time / signal) | **light** — `isAppearanceLightStatusBars = false` |

```kotlin
enableEdgeToEdge(
    statusBarStyle = SystemBarStyle.dark(
        scrim = Wiom.bg.brand.bold.toArgb(),
    )
)
```

> **Don't half-pair.** If the top bar is dark, the status bar must be
> dark with light content; if the top bar is light, status bar must be
> light with dark content. Mixing (light top bar + dark status bar
> background) creates a visible seam at the boundary and reads as a
> bug.

> **No mid-screen status-bar color changes.** Set it once per screen
> in the Activity / Composable scope, not per scroll state. If a
> screen genuinely needs to change top-bar lightness mid-flow (e.g.,
> a hero image collapses from dark to light on scroll), animate the
> `statusBarColor` and the `isAppearanceLightStatusBars` flag in
> sync — but consider whether the design is fighting itself first.

---

## 7. Picking the State (runtime)

Size is a static design choice — see the *Use for* column in §1.
State is partly runtime:

- **`Default`** — at rest, content unscrolled. (Or **`Centered`** for modal / sheet headers.)
- **`Scrolled`** — wired to scroll position, *not* a static design choice. Bind via `rememberScrolledTopBarState(listState)` (§9). Adds `shadow.sm`.
- **`Search`** — user tapped search, or the screen is search-first. Replaces the title column with an inline `bg.subtle` pill.

---

## 8. Universal rules

1. **Heights are fixed per size** — 64 / 112 / 152 dp. Don't override with `Modifier.height(...)` / `heightIn(...)`.
2. **Pair status-bar lightness with the bar.** Light bar → dark status-bar content; dark bar → light. Set it once per screen via `enableEdgeToEdge` or `WiomTopBarStatusBar(isDarkVariant = …)`.
3. **Trailing area max 3 icons + 1 text CTA.** Beyond that, move overflow into a `more_vert` overflow menu via bottom sheet.
4. **Trailing Text CTA is `text.brand` (light) / `text.inverse` (dark).** Never a filled button; never `text.critical` (destructive should go through a confirmation dialog).
5. **Subtitle is Small × Default / Centered × light only.** Medium and Large have no subtitle slot; the dark variant has no dimmed-inverse token, so subtitle is also disabled there.
6. **Centered = modal header.** A close leading is allowed, a subtitle is allowed, and at most 1 trailing action. The title centres between leading and trailing (as shipped).
7. **Tokens only — element-first.** No raw hex. No legacy `color.brand.primary`. Dark-variant uses foundations' `bg.brand.bold` / `text.inverse` / `icon.inverse` — not a hardcoded dark theme.

---

## 9. Compose API + code

### Public API

```kotlin
enum class WiomTopBarSize { Small, Medium, Large }

sealed class WiomTopBarState {
    object Default : WiomTopBarState()
    object Centered : WiomTopBarState()              // Small only
    object Scrolled : WiomTopBarState()
    data class Search(                                // Small only
        val query: String,
        val placeholder: String,
        val onQueryChange: (String) -> Unit,
    ) : WiomTopBarState()
}

data class WiomTrailingAction(val icon: ImageVector, val onClick: () -> Unit)
data class WiomTrailingText(val label: String, val onClick: () -> Unit)

@Composable
fun WiomTopBar(
    title: String,
    size: WiomTopBarSize = WiomTopBarSize.Small,
    state: WiomTopBarState = WiomTopBarState.Default,
    subtitle: String? = null,                         // Small × Default / Centered only
    leadingIcon: ImageVector? = Icons.AutoMirrored.Rounded.ArrowBack,
    onLeadingClick: (() -> Unit)? = null,             // null disables the leading slot
    trailingText: WiomTrailingText? = null,
    trailingActions: List<WiomTrailingAction> = emptyList(),  // max 3
    isDarkVariant: Boolean = false,                    // light = bg.default; dark = bg.brand.bold
    modifier: Modifier = Modifier,
)

// Hooks the Scrolled state to a LazyListState — drop into the bar's `state` param.
@Composable
fun rememberScrolledTopBarState(
    listState: LazyListState,
): WiomTopBarState = remember(listState) {
    derivedStateOf {
        if (listState.firstVisibleItemIndex > 0 || listState.firstVisibleItemScrollOffset > 0)
            WiomTopBarState.Scrolled
        else
            WiomTopBarState.Default
    }
}.value
```

### Token resolver — light + dark variants

```kotlin
@Composable
private fun rememberTopBarColors(isDark: Boolean): TopBarColors =
    if (isDark) TopBarColors(
        container = Wiom.bg.brand.bold,                // #443152
        title     = Wiom.text.inverse,
        subtitle  = null,                              // dark variant doesn't support Subtitle (no dimmed-inverse token)
        trailing  = Wiom.text.inverse,                 // brand pink fails AA on bg.brand.bold; use inverse
        icon      = Wiom.icon.inverse,
        searchPill      = null,                        // dark variant doesn't support Search state
        searchPlaceholder = null,
    ) else TopBarColors(
        container = Wiom.bg.default,                   // #FAF9FC
        title     = Wiom.text.default,
        subtitle  = Wiom.text.subtle,
        trailing  = Wiom.text.brand,
        icon      = Wiom.icon.action,
        searchPill      = Wiom.bg.subtle,
        searchPlaceholder = Wiom.text.disabled,
    )

private data class TopBarColors(
    val container: Color,
    val title: Color,
    val subtitle: Color?,        // null on dark — Subtitle slot disabled
    val trailing: Color,
    val icon: Color,
    val searchPill: Color?,      // null on dark — Search state disabled
    val searchPlaceholder: Color?,
)

@Composable
private fun titleStyleFor(size: WiomTopBarSize): TextStyle = when (size) {
    WiomTopBarSize.Small  -> Wiom.type.title.lg          // 20sp Medium / 28 LH
    WiomTopBarSize.Medium -> Wiom.type.heading.lg        // 24sp Bold / 32 LH
    WiomTopBarSize.Large  -> Wiom.type.heading.xl        // 32sp Bold / 44 LH
}
```

### Status bar binding (call once per screen)

```kotlin
@Composable
fun WiomTopBarStatusBar(isDarkVariant: Boolean) {
    val view = LocalView.current
    val containerColor = if (isDarkVariant) Wiom.bg.brand.bold else Wiom.bg.default
    SideEffect {
        // Safe cast — skip silently if hosted in a non-Activity context (tests, previews,
        // Compose-in-Fragment with a custom ContextWrapper). The activity-level
        // `enableEdgeToEdge` path in §6 covers production.
        val window = (view.context as? Activity)?.window ?: return@SideEffect
        window.statusBarColor = containerColor.toArgb()
        // Light top bar → dark icons (true). Dark top bar → light icons (false).
        WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = !isDarkVariant
    }
}
```

Call `WiomTopBarStatusBar(isDarkVariant = false)` from inside any
screen that uses a `WiomTopBar`. If the activity already runs
`enableEdgeToEdge(...)` with the right `statusBarStyle`, you can skip
the per-screen call.

### `WiomTopBar` implementation skeleton

The body has 3 layouts that diverge by size — Small, Medium, Large.
All three share the action-row composable; Medium / Large add a
title-area composable below it.

```kotlin
@Composable
fun WiomTopBar(
    title: String,
    size: WiomTopBarSize,
    state: WiomTopBarState,
    subtitle: String?,
    leadingIcon: ImageVector?,
    onLeadingClick: (() -> Unit)?,
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    isDarkVariant: Boolean,
    modifier: Modifier,
) {
    require(trailingActions.size <= 3) { "Top bar supports max 3 trailing icons" }
    if (isDarkVariant) {
        require(subtitle == null) { "Dark variant doesn't support Subtitle (no dimmed-inverse token)" }
        require(state !is WiomTopBarState.Search) { "Dark variant doesn't support Search state" }
    }
    val colors = rememberTopBarColors(isDarkVariant)

    Surface(
        color = colors.container,
        modifier = modifier
            .fillMaxWidth()                                   // edge-to-edge
            .then(
                if (state == WiomTopBarState.Scrolled)
                    Modifier.shadow(elevation = 1.dp, shape = RectangleShape, clip = false)
                else Modifier
            )
            .statusBarsPadding(),                             // bar's content sits below the OS strip
    ) {
        // NOTE on shadow: foundations `shadow.sm` is a full drop-shadow spec
        // (offset + blur + color). Compose `Modifier.shadow` honours
        // elevation but flattens offset; the visual is close enough at 1dp.
        // If foundations later exposes `shadow.sm.elevation: Dp`, swap to it.
        when (size) {
            WiomTopBarSize.Small  -> SmallTopBar(title, state, subtitle, leadingIcon, onLeadingClick,
                                                  trailingText, trailingActions, colors)
            WiomTopBarSize.Medium -> MediumTopBar(title, leadingIcon, onLeadingClick,
                                                  trailingText, trailingActions, colors)
            WiomTopBarSize.Large  -> LargeTopBar(title, leadingIcon, onLeadingClick,
                                                 trailingText, trailingActions, colors)
        }
    }
}

// Small: action row only. Search state replaces the title column with an inline pill.
@Composable
private fun SmallTopBar(
    title: String,
    state: WiomTopBarState,
    subtitle: String?,
    leadingIcon: ImageVector?,
    onLeadingClick: (() -> Unit)?,
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    colors: TopBarColors,
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .height(64.dp)                                    // ship-fixed
            .padding(
                start  = if (state is WiomTopBarState.Search) 4.dp else Wiom.space.s4,
                end    = if (state is WiomTopBarState.Search) Wiom.space.s16 else Wiom.space.s4,
                top    = Wiom.space.s8,
                bottom = Wiom.space.s8,
            ),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        LeadingCell(leadingIcon, onLeadingClick, colors.icon)
        when (state) {
            is WiomTopBarState.Search -> SearchPill(
                state, colors,
                modifier = Modifier.weight(1f).padding(horizontal = Wiom.space.s8),   // as shipped
            )
            else -> TitleColumn(
                title, subtitle, colors,
                centered = state is WiomTopBarState.Centered,
                modifier = Modifier.weight(1f),
            )
        }
        TrailingArea(
            trailingText,
            if (state is WiomTopBarState.Search) emptyList() else trailingActions,
            colors,
        )
    }
}

// Medium / Large: action row (48dp) + title area (fills remaining height).
@Composable
private fun MediumTopBar(
    title: String,
    leadingIcon: ImageVector?,
    onLeadingClick: (() -> Unit)?,
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    colors: TopBarColors,
) {
    Column(modifier = Modifier.fillMaxWidth().height(112.dp)) {
        ActionRow(leadingIcon, onLeadingClick, trailingText, trailingActions, colors)
        TitleArea(
            title = title,
            style = Wiom.type.heading.lg,
            color = colors.title,
            modifier = Modifier
                .weight(1f)
                .padding(horizontal = Wiom.space.s16, vertical = Wiom.space.s8),
        )
    }
}

@Composable
private fun LargeTopBar(
    title: String,
    leadingIcon: ImageVector?,
    onLeadingClick: (() -> Unit)?,
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    colors: TopBarColors,
) {
    Column(modifier = Modifier.fillMaxWidth().height(152.dp)) {
        ActionRow(leadingIcon, onLeadingClick, trailingText, trailingActions, colors)
        TitleArea(
            title = title,
            style = Wiom.type.heading.xl,
            color = colors.title,
            modifier = Modifier
                .weight(1f)
                .padding(start = Wiom.space.s16, end = Wiom.space.s16,
                         top = Wiom.space.s12, bottom = Wiom.space.s32),
        )
    }
}
```

### Helper composables

```kotlin
// Renders nothing (and reserves no width) when icon == null — used by Centered
// state and root-tab Large hero where there's no back arrow.
@Composable
private fun LeadingCell(
    icon: ImageVector?,
    onClick: (() -> Unit)?,
    tint: Color,
) {
    if (icon == null) return                              // collapsed slot
    Box(
        modifier = Modifier
            .size(48.dp)                                  // hit target
            .then(if (onClick != null) Modifier.clickable(onClick = onClick) else Modifier),
        contentAlignment = Alignment.Center,
    ) {
        Icon(imageVector = icon, contentDescription = null, tint = tint, modifier = Modifier.size(24.dp))
    }
}

@Composable
private fun TrailingArea(
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    colors: TopBarColors,
) {
    Row(verticalAlignment = Alignment.CenterVertically) {
        trailingText?.let { ct ->
            Box(
                modifier = Modifier
                    .minimumInteractiveComponentSize()     // ≥48dp WCAG hit target
                    .clickable(onClick = ct.onClick)
                    .padding(horizontal = Wiom.space.s12),
                contentAlignment = Alignment.Center,
            ) {
                Text(ct.label, style = Wiom.type.label.md, color = colors.trailing)
            }
        }
        trailingActions.forEach { ta ->
            Box(
                modifier = Modifier.size(48.dp).clickable(onClick = ta.onClick),
                contentAlignment = Alignment.Center,
            ) {
                Icon(ta.icon, contentDescription = null, tint = colors.icon,
                     modifier = Modifier.size(24.dp))
            }
        }
    }
}

@Composable
private fun SearchPill(
    state: WiomTopBarState.Search,
    colors: TopBarColors,
    modifier: Modifier = Modifier,
) {
    val pillFill = checkNotNull(colors.searchPill) { "Search state requires light variant" }
    val placeholderColor = checkNotNull(colors.searchPlaceholder)
    Surface(
        color = pillFill,
        shape = RoundedCornerShape(50),                    // radius-full
        modifier = modifier.height(40.dp),                 // pill height inside the 64dp bar
    ) {
        Row(
            modifier = Modifier.padding(horizontal = Wiom.space.s16, vertical = Wiom.space.s8),
            verticalAlignment = Alignment.CenterVertically,
            horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
        ) {
            Icon(Icons.Rounded.Search, contentDescription = null, tint = Wiom.icon.nonAction,   // decorative
                 modifier = Modifier.size(20.dp))           // smaller than the 24dp action icons
            Box(modifier = Modifier.weight(1f)) {
                BasicTextField(
                    value = state.query,
                    onValueChange = state.onQueryChange,
                    singleLine = true,
                    textStyle = Wiom.type.body.lg.copy(color = colors.title),
                    cursorBrush = SolidColor(Wiom.text.default),     // as shipped
                )
                if (state.query.isEmpty()) {
                    Text(state.placeholder, style = Wiom.type.body.lg, color = placeholderColor)
                }
            }
        }
    }
}
```

```kotlin
// Action row used by Medium / Large — title is rendered separately below.
@Composable
private fun ActionRow(
    leadingIcon: ImageVector?,
    onLeadingClick: (() -> Unit)?,
    trailingText: WiomTrailingText?,
    trailingActions: List<WiomTrailingAction>,
    colors: TopBarColors,
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .height(48.dp)
            .padding(horizontal = Wiom.space.s4, vertical = Wiom.space.s8),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        LeadingCell(leadingIcon, onLeadingClick, colors.icon)
        Spacer(modifier = Modifier.weight(1f))
        TrailingArea(trailingText, trailingActions, colors)
    }
}

// Small Default / Centered title column. Subtitle renders only when both
// the string and the colors.subtitle token are non-null (light variant).
@Composable
private fun TitleColumn(
    title: String,
    subtitle: String?,
    colors: TopBarColors,
    centered: Boolean,
    modifier: Modifier = Modifier,
) {
    Column(
        modifier = modifier.padding(horizontal = Wiom.space.s12),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = if (centered) Alignment.CenterHorizontally else Alignment.Start,
    ) {
        Text(title, style = Wiom.type.title.lg, color = colors.title, maxLines = 1)
        val subtitleColor = colors.subtitle
        if (subtitle != null && subtitleColor != null) {
            Text(subtitle, style = Wiom.type.body.sm, color = subtitleColor, maxLines = 1)
        }
    }
}

// Medium / Large title area — single Text styled by the caller.
@Composable
private fun TitleArea(
    title: String,
    style: TextStyle,
    color: Color,
    modifier: Modifier = Modifier,
) {
    Box(modifier = modifier, contentAlignment = Alignment.BottomStart) {
        Text(title, style = style, color = color, maxLines = 1)
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — wrapped in horizontal padding (creates a gutter), Material default icon,
//             status bar not bound, fixed height override
Box(modifier = Modifier.padding(horizontal = 16.dp)) {       // top bar is edge-to-edge!
    WiomTopBar(
        title = "Wallet",
        leadingIcon = Icons.Default.ArrowBack,                // wrong icon family
        modifier = Modifier.height(56.dp),                    // never override
    )
}
// (And no status bar pairing — OS strip will be light/dark mismatched.)

// ✅ RIGHT — fillMaxWidth at the parent (edge-to-edge), Rounded icon, statusBarsPadding handled
//             by the component, status bar bound once per screen
WiomTopBarStatusBar(isDarkVariant = false)                    // status bar pairs with top bar
WiomTopBar(
    title = "Wallet",
    size = WiomTopBarSize.Medium,
    leadingIcon = Icons.AutoMirrored.Rounded.ArrowBack,
    onLeadingClick = { onBack() },
    trailingActions = listOf(
        WiomTrailingAction(Icons.Rounded.Notifications, onNotifications),
    ),
    modifier = Modifier.fillMaxWidth(),                        // already default; explicit for clarity
)
```

---

## 10. Screen-level recipes

### Standard screen — back + title + 1 trailing action

```kotlin
WiomTopBarStatusBar(isDarkVariant = false)
WiomTopBar(
    title = "टिकट details",
    leadingIcon = Icons.AutoMirrored.Rounded.ArrowBack,
    onLeadingClick = onBack,
    trailingActions = listOf(
        WiomTrailingAction(Icons.Rounded.MoreVert, onOverflow),
    ),
)
```

### Hero page — Large with Scrolled tied to scroll position

```kotlin
val listState = rememberLazyListState()
val barState = rememberScrolledTopBarState(listState)

WiomTopBarStatusBar(isDarkVariant = false)
Scaffold(
    topBar = {
        WiomTopBar(
            title = "Wallet",
            size = WiomTopBarSize.Large,
            state = barState,
            leadingIcon = null,                                  // root tab — no back
            trailingActions = listOf(
                WiomTrailingAction(Icons.Rounded.Notifications, onNotifications),
            ),
        )
    },
) { padding ->
    LazyColumn(state = listState, modifier = Modifier.padding(padding)) { /* … */ }
}
```

### Search header

```kotlin
var query by remember { mutableStateOf("") }
WiomTopBarStatusBar(isDarkVariant = false)
WiomTopBar(
    title = "",                                                  // unused in Search state
    state = WiomTopBarState.Search(
        query = query,
        placeholder = "Search plans, recharges…",
        onQueryChange = { query = it },
    ),
    leadingIcon = Icons.AutoMirrored.Rounded.ArrowBack,
    onLeadingClick = onBack,
    trailingText = WiomTrailingText("Cancel", onCancel),
)
```

### Modal header: Centered with close

```kotlin
WiomTopBarStatusBar(isDarkVariant = false)
WiomTopBar(
    title = "Plan details",
    subtitle = "Step 2 of 3",                                   // optional
    state = WiomTopBarState.Centered,
    leadingIcon = Icons.Rounded.Close,                           // close leading allowed
    trailingActions = emptyList(),                               // max 1
)
```

### Dark / premium header (foundations rule, not a Figma variant)

```kotlin
WiomTopBarStatusBar(isDarkVariant = true)                       // light icons on dark status bar
WiomTopBar(
    title = "Wiom Pro",
    size = WiomTopBarSize.Medium,
    leadingIcon = Icons.AutoMirrored.Rounded.ArrowBack,
    onLeadingClick = onBack,
    isDarkVariant = true,                                        // bg.brand.bold + text.inverse
    trailingActions = listOf(
        WiomTrailingAction(Icons.Rounded.MoreVert, onOverflow),
    ),
)
```

---

## 11. Anti-patterns

(Universal rules in §8 cover the bans. Listed here are extras.)

- ❌ Forgetting to call `WiomTopBarStatusBar` (or `enableEdgeToEdge` with the right `statusBarStyle`) — visible mismatch at the bar/status-bar seam.
- ❌ Light status-bar background + dark status-bar content (or vice versa) — reads as a system bug.
- ❌ Manually toggling Scrolled — it's a derived state from scroll position, not a static design choice.
- ❌ Centered with 2+ trailing actions. Centered allows at most 1.
- ❌ A custom search field instead of the Search state's pill — the pill is part of the variant; replicating it manually drifts.
- ❌ Using `WiomTopBarState.Search` or `Show Subtitle = true` on a dark-variant top bar — both are unsupported there (foundations gap).

---

## 12. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Top Bar canvas: `3069:3849`.
Re-pull `get_variable_defs` on `1576:143` if foundations token names
ever change.

| Node | Variant |
|---|---|
| `1576:143` | Top Bar component set — 8 ship variants |
| `1576:2` · `1576:19` · `1576:36` · `1576:53` | Small × Default · Centered · Scrolled · Search |
| `1576:71` · `1576:89` | Medium × Default · Scrolled |
| `1576:107` · `1576:125` | Large × Default · Scrolled |

**Description-vs-ship discrepancies** — if a stale doc says otherwise,
trust ship values:

- The Figma **page header** in this canvas reads "1 component · 4
  state variants". The actual ship is **1 component × 8 state
  variants** (Small 4 + Medium 2 + Large 2). Page header counted
  Small's states only.
- `Centered` and `Search` are **Small-only states**. Medium and Large
  ship Default + Scrolled only. Don't try to instance
  `Size=Medium, State=Search` — it doesn't exist.
- `Search` state has **asymmetric outer padding**: `pl-4 pr-16`. All
  other states are `px-4`. This is the only horizontal-asymmetric
  variant in the file.
- The trailing-area icon-button cell padding is `space-12` — combined
  with the 24 dp glyph this gives a 48 dp hit target without an
  explicit hit-area helper.
- Title typography differs by size: Small = `type.title.lg` (20 sp
  Medium); Medium = `type.heading.lg` (24 sp Bold); Large =
  `type.heading.xl` (32 sp Bold). Don't use `heading.xl` on a Small
  bar even if you want a bigger title — switch to Large.
- The dark-variant top bar (`bg.brand.bold` + `text.inverse`) is **not
  a Figma variant** — it's a foundations-level theming override
  applied to the same component. Foundations says: premium / partner
  header surfaces use this token pair.

---

## See also

- `wiom-design-foundations` — token source. Especially:
  `bg.default` / `bg.brand.bold` / `text.inverse` / `icon.inverse`
  for the dark-variant pairing, plus the warning-family rules.
- `wiom-compose-layout-rules` — Modifier discipline; edge-to-edge
  wrappers (`statusBarsPadding`, `navigationBarsPadding`,
  `windowInsetsPadding`); `Icons.Rounded.*` enforcement.
- `wiom-cta` — the Primary / Secondary / Tertiary CTAs that live
  *inside* the screen body (sticky bottom bar). The top-bar
  `trailingText` is **not** a CTA in that taxonomy — it's a
  text-only inline action specific to the top bar.
- `wiom-input-fields` — the Search state's pill is conceptually a
  read-only Input; if you're building a fully-functional search
  results screen, you may swap it for a real `WiomInput` with
  `state = Filled` (search header pattern).
- `wiom-tabs-filters` — Pill Tab / Underline Filter sit *under* the
  top bar (Pill Tab on top, Underline below), inside the screen
  scaffold's 16 dp gutter — they don't go edge-to-edge.
- `wiom-navigation-bar` — the persistent bottom navigation bar; same
  edge-to-edge contract as the top bar and the same status-bar-style
  pairing applies (use `navigationBarsPadding` instead of
  `statusBarsPadding`).
