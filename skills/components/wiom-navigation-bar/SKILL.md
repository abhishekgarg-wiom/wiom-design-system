---
name: wiom-navigation-bar
description: >
  Wiom Navigation Bar — the persistent edge-to-edge bar at the bottom
  of root screens that switches between top-level destinations.
  2 components (NavItem · NavigationBar), 2 NavItem states (Default /
  Selected), 4 ship configurations of the bar (2 / 3 / 4 / 5 tabs via
  Show Tab3 / Tab4 / Tab5 booleans). Reference for generating Jetpack
  Compose code with WiomNavigationBar and for instancing the Figma
  components (set 1935:76 and 1913:10) via MCP. Owns the bottom of the
  screen including the OS gesture / 3-button bar inset below it. Use
  this skill EVERY TIME you create, edit, or review the bottom bar on
  any root screen, or when assembling Figma frames with a bottom nav.
  Trigger on: NavigationBar, BottomNavigation, BottomBar, NavBar, tab
  bar, bottom tabs, navigation bar inset, navigationBarsPadding, root
  screen, Tab1, Tab2, Show Tab3/4/5, instance of 1935:76.
---

# Wiom Navigation Bar

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
sets `1913:10` (NavItem) + `1935:76` (NavigationBar), and the
`wiom-design-foundations` token set.

**2 components · 2 NavItem states · 4 bar configurations · edge-to-edge.**

| Component | Variants |
|---|---|
| NavItem | `State = Default` · `State = Selected` (2) |
| NavigationBar | `Show Tab3/4/5` toggles → 2 / 3 / 4 / 5 tabs (4 ship configurations) |

The navigation bar is **the surface that owns the bottom of the screen**
on root destinations — it spans full width edge-to-edge (no horizontal
screen margin), and its bottom is visually continuous with the OS
gesture / 3-button bar strip below it. Only root screens carry it;
non-root screens hide it (see §3).

---

## 1. Figma — instancing the components

When assembling a Figma frame via MCP, instance `1935:76` (the assembled
bar) for screens, or `1913:10` (the NavItem atom) for documentation.
Strings are **case-sensitive**.

### NavigationBar — ship configurations

| Tabs | `Show Tab3` | `Show Tab4` | `Show Tab5` | Use for |
|---|---|---|---|---|
| 2 | `false` | `false` | `false` | Rare. Only when there are exactly two peer destinations |
| 3 | `true` | `false` | `false` | ★ Most apps should aim here — Home + 2 key destinations |
| 4 | `true` | `true` | `false` | When a fourth top-level destination is genuinely needed |
| 5 | `true` | `true` | `true` | Hard cap. Only if every tab is used weekly by most users |

Tab1 and Tab2 are **always visible** — they cannot be hidden. Put the
highest-traffic destination in Tab1.

### NavItem — variants

| `State` | Node | Use for |
|---|---|---|
| `Default` | `1910:2` | This is not the active destination |
| `Selected` | `1913:3` | This is the current destination — exactly one bar item carries this state |

### NavItem — properties (apply per item, on every variant)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `icon` | Instance swap | per-tab default | Material Symbols Rounded glyph at 24 dp. Use the *outline* (unfilled, FILL=0) Rounded glyph — not the banned `Icons.Outlined` family; `Selected` should swap to its **filled** pair (FILL=1) |
| `label` | Text | `"Home"` | One-word destination name |
| `Show Label` | Boolean | `true` | Reveals the label below the icon pill (always leave on — see §6) |
| `Show Badge` | Boolean | `false` | Reveals the 8 dp notification dot at the top-right of the IconSlot |
| `badge` | Instance swap | red dot | Default is `bg.critical` 8 dp dot. Swap for a count chip only when the count is meaningful |

> The bar does **not** expose a `Selected index` property — set one
> child NavItem to `Selected` via nested-instance overrides on the
> screen instance.

### Code enum ↔ Figma variant string

| `WiomNavItemState` (Kotlin) | Figma `State` value |
|---|---|
| `Default` | `Default` |
| `Selected` | `Selected` |

---

## 2. Anatomy

The bar is a `Row` of equal-width `NavItem`s on top of a 1 dp
`stroke.subtle` border, with `space-12` bottom padding above the OS
navigation-bar inset. There is no top padding on the bar itself; each
item carries its own `space-8` vertical padding.

### NavigationBar — 80 dp tall (excluding navigation-bar inset)

```
┌─────────────────────── 360 dp (edge-to-edge) ──────────────────────────┐
├─ 1 dp stroke.subtle top border ────────────────────────────────────────┤
│                                                                         │
│  ●NavItem    ●NavItem    ●NavItem    ●NavItem    ●NavItem               │
│  flex 1      flex 1      flex 1      flex 1      flex 1                │
│                                                                         │
└────────────────────────── 12 dp bottom pad ─────────────────────────────┘
                  ↓
            navigationBarsPadding (OS gesture / 3-button strip)
```

| Slot | Value |
|---|---|
| Container width | Edge-to-edge — `fillMaxWidth()` |
| Container height | Wraps content (≈ 80 dp) + navigation-bar inset |
| Top border | `stroke-small` (1 dp) `stroke.subtle` — only on the top edge |
| Bottom padding | `space-12` (then add OS inset below) |
| Top padding | `0` (each item carries its own `space-8`) |
| Item layout | Equal width via `Modifier.weight(1f)` on every `NavItem` |

### NavItem — 68 dp tall (icon pill + label)

```
        ┌──────────────────────────────┐
        │      [ space-8 V padding ]   │
        │                              │
        │      ┌──── IconPill ────┐    │
        │      │ space-16 H, 4 V  │    │   ← 32 dp tall
        │      │  ●●●● 24 dp icon │    │
        │      └──────────────────┘    │
        │      [ space-4 gap ]         │
        │                              │
        │           Home               │   ← type.label.sm 16 LH
        │      [ space-8 V padding ]   │
        └──────────────────────────────┘
              [ space-4 H padding ]
```

| Slot | Value |
|---|---|
| Container width | `weight(1f)` — fills equal share of the bar |
| Container padding | `space-4` H · `space-8` V |
| Item arrangement | Column · `verticalArrangement = spacedBy(space-4)` · `horizontalAlignment = CenterHorizontally` |
| IconPill padding | `space-16` H · `space-4` V (constant — pill width does not grow with label) |
| IconPill radius | `radius-full` |
| IconPill fill | Default → transparent · Selected → `bg.selected` |
| Icon size | 24 dp (`icon.md`), Rounded unfilled (FILL=0) glyph → filled on Selected. Not `Icons.Outlined.*` (banned by foundations) |
| Label style | `type.label.sm` (Noto Sans SemiBold 12 / 16 LH) |
| Badge | 8 dp `bg.critical` dot at IconSlot's top-right (offset `+2 dp` outside the slot, both axes) |

> **No fixed item width** — each item is `weight(1f)`, so a 2-tab bar
> spreads the same as a 5-tab bar. This is why the bar handles all
> tab counts with one component.

---

## 3. Surface contract — edge-to-edge

The navigation bar is **not** a surface that respects the standard
16 dp screen gutter. It owns the full width of the screen and sits
flush against both edges. Identical contract to the top bar; every
other component inside the screen body uses the 16 dp gutter.

### What this means in code

- `Modifier.fillMaxWidth()` on the bar — the component owns its inner padding (per-item `space-4` H + `space-8` V; bar bottom `space-12`).
- **Navigation-bar inset** (OS gesture / 3-button strip below) is consumed by the bar via `WindowInsets.navigationBars` / `Modifier.navigationBarsPadding()`, not by the screen content. The container fill (`bg.default`) extends downward into the inset so the bar reads as one continuous surface with the OS strip.

### When to **hide** the bar (don't disable, hide)

The bar is part of the surface contract for **root screens only**. On the following surfaces, hide it entirely — don't leave a disabled or dimmed bar underneath.

1. **Non-root screens** — any screen pushed on top of a tab (detail page, edit, form, picker). The back affordance takes over.
2. **Full-flow experiences** — onboarding, OTP, KYC, payment, installation scheduling. Linear flows shouldn't allow tab-switching mid-state.
3. **Immersive media** — video, full-screen image viewer, camera.
4. **System sheets** — bottom sheets, dialogs, alerts already own the bottom surface.
5. **Keyboard-open text-entry screens** — let the keyboard own the vertical space; the bar would sit above the keyboard and steal attention.

> A disabled bar on a non-root screen invites mis-taps and breaks the
> "where am I" affordance. Always remove it from composition, not
> just visually mute it.

---

## 4. NavItem states

| State | Visual signal | Use when |
|---|---|---|
| `Default` | No pill, outline icon `icon.nonAction` (#898296), label `text.subtle` | Not the active destination |
| `Selected` | `bg.selected` (#FFCCED) pill behind icon, **filled** icon `icon.brand` (#D9008D), label `text.selected` (#D9008D) | Active destination — exactly one item per bar carries this |

There is **no** `Disabled` state. A tab that can't be opened right now
shouldn't be in the bar at all — gate at navigation time, not by
greying out.

There is **no** `Pressed` state as a Figma variant — the platform
ripple / state-layer handles that at runtime.

> **Default uses `icon.nonAction`, not `icon.action`.** Foundations
> classifies nav-bar item icons as decorative-inside-tappable-row: the
> *label* carries the affordance signal, and the Selected pill carries
> the active-state signal. Using `icon.action` (#352D42) here would
> over-emphasize unselected tabs and dilute the Selected contrast.

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

| Slot | Token (light theme) |
|---|---|
| Bar background | `bg.default` (#FAF9FC) |
| Bar top border (color) | `stroke.subtle` (#D7D3E0) |
| Bar top border (width) | `stroke-small` (1 dp) |
| IconPill fill (Default) | transparent |
| IconPill fill (Selected) | `bg.selected` (#FFCCED) |
| IconPill radius | `radius-full` |
| Icon (Default) | `icon.nonAction` (#898296) — outline glyph |
| Icon (Selected) | `icon.brand` (#D9008D) — filled glyph (FILL=1) |
| Label typography | `type.label.sm` (Noto Sans SemiBold 12 / 16 LH) |
| Label color (Default) | `text.subtle` (#5C5570) |
| Label color (Selected) | `text.selected` (#D9008D) |
| Badge fill | `bg.critical` (#D92130) |
| Badge size | 8 dp · `radius-full` |
| Bar shadow | none |

No shadow on the bar. No hardcoded hex. No fixed item width — each item is `weight(1f)` so the bar redistributes evenly when tabs are toggled off.

---

## 6. Universal rules

1. **Edge-to-edge horizontally.** `Modifier.fillMaxWidth()` on the bar — no horizontal padding from the parent, no screen-gutter wrapper. The bar IS the surface and runs flush to both edges. The only horizontal inset inside it is the items' own `space-4` H padding (4 dp each side), which is intrinsic to the component, not a gutter.
2. **Exactly one Selected at a time.** The bar reflects the current route — not preferences, not recent activity. Update on navigation.
3. **Labels stay on.** 12 sp is small enough; `Show Label = false` regresses first-time and low-vision users. Don't hide labels to "clean up" the bar.
4. **One word per label.** "Home", "Plans", "Bills" — not "My Bills", not "View Plans". Don't shrink type to fit a long word; rename the destination instead.
5. **Icons use the filled pair on Selected.** Selected icon = filled variant of Default (Material Symbols `FILL = 1`). Never swap to an unrelated glyph between states.
6. **Badges are rare and meaningful.** Show only on actionable / unread state. Never badge more than one tab at once unless two things genuinely need attention — it dilutes signal.
7. **Never put a CTA in the bar.** No "Add" tab, no plus FAB merged into a slot. Primary actions belong elsewhere (top bar, inline CTA, FAB above the bar).
8. **Tab1 and Tab2 are always visible.** Design for the 2-tab fallback; don't put high-traffic destinations in Tab3–5 and toggle them off for some users. Don't role-gate by toggling slots — gate at navigation time.

---

## 7. Compose API + code

### Public API

```kotlin
data class WiomNavDestination(
    val route: String,
    val label: String,
    @DrawableRes val iconOutlineRes: Int,   // Default state — rounded outline drawable
    @DrawableRes val iconFilledRes: Int,    // Selected state — rounded filled drawable (same metaphor)
    val showBadge: Boolean = false,         // shows the default 8dp bg.critical dot
)

// Note on icons: foundations bans `Icons.Default / Filled / Outlined / Sharp / TwoTone`,
// and `Icons.Rounded.*` exposes only one glyph per metaphor (no outlined sibling).
// Therefore the outline + filled pair must come from custom rounded drawables in
// res/drawable — typically named `ic_<name>_rounded` (outline) and
// `ic_<name>_rounded_filled`. A consumer needing a custom badge surface (e.g. a
// count chip from wiom-badge) overlays it manually above the bar; this API
// supports only the default red dot.

@Composable
fun WiomNavigationBar(
    destinations: List<WiomNavDestination>,        // 2..5 entries
    currentRoute: String,
    onNavigate: (String) -> Unit,
    modifier: Modifier = Modifier,
)
```

The bar takes the destination list as the source of truth for tab
count: 3 entries → 3-tab bar, 5 entries → 5-tab bar. There is no
`showTabN` boolean in the Compose API — the equivalent is omitting the
entry from the list.

### Token resolver — per-item state

```kotlin
@Composable
private fun rememberNavItemColors(selected: Boolean): NavItemColors =
    if (selected) NavItemColors(
        pillFill   = Wiom.bg.selected,             // #FFCCED
        iconTint   = Wiom.icon.brand,              // #D9008D
        labelColor = Wiom.text.selected,           // #D9008D
    ) else NavItemColors(
        pillFill   = Color.Transparent,
        iconTint   = Wiom.icon.nonAction,          // #898296 — decorative-in-tappable-row
        labelColor = Wiom.text.subtle,             // #5C5570
    )

private data class NavItemColors(
    val pillFill: Color,
    val iconTint: Color,
    val labelColor: Color,
)
```

### `WiomNavigationBar` implementation

```kotlin
@Composable
fun WiomNavigationBar(
    destinations: List<WiomNavDestination>,
    currentRoute: String,
    onNavigate: (String) -> Unit,
    modifier: Modifier = Modifier,
) {
    require(destinations.size in 2..5) { "Navigation bar supports 2..5 destinations" }
    require(destinations.count { it.route == currentRoute } == 1) {
        "Exactly one destination must match currentRoute (the Selected item)"
    }

    val borderColor = Wiom.stroke.subtle           // captured outside drawBehind (DrawScope is not @Composable)
    Surface(
        color = Wiom.bg.default,
        tonalElevation = 0.dp,                     // foundations: no elevation tint on the bar
        shadowElevation = 0.dp,
        modifier = modifier.fillMaxWidth(),
    ) {
        Column(
            modifier = Modifier
                .drawBehind {
                    // 1 dp stroke.subtle top border — only on the top edge.
                    drawLine(
                        color = borderColor,
                        start = Offset(0f, 0f),
                        end   = Offset(size.width, 0f),
                        strokeWidth = 1.dp.toPx(),
                    )
                }
                .navigationBarsPadding()                // bar's content sits above the OS strip
                .padding(bottom = Wiom.space.s12),
        ) {
            Row(modifier = Modifier.fillMaxWidth()) {
                destinations.forEach { dest ->
                    WiomNavItem(
                        destination = dest,
                        selected = dest.route == currentRoute,
                        onClick = { onNavigate(dest.route) },
                        modifier = Modifier.weight(1f),  // equal-width — handles 2..5 tabs
                    )
                }
            }
        }
    }
}
```

### `WiomNavItem` — internal

```kotlin
@Composable
private fun WiomNavItem(
    destination: WiomNavDestination,
    selected: Boolean,
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
) {
    val colors = rememberNavItemColors(selected)
    Column(
        modifier = modifier
            .clickable(onClick = onClick)              // default ripple via LocalIndication.current
            .padding(horizontal = Wiom.space.s4, vertical = Wiom.space.s8),
        verticalArrangement = Arrangement.spacedBy(Wiom.space.s4, Alignment.CenterVertically),
        horizontalAlignment = Alignment.CenterHorizontally,
    ) {
        // IconPill — fixed padding, doesn't grow with label width
        Surface(
            color = colors.pillFill,
            shape = RoundedCornerShape(50),                 // radius-full
        ) {
            Box(
                modifier = Modifier.padding(horizontal = Wiom.space.s16, vertical = Wiom.space.s4),
                contentAlignment = Alignment.Center,
            ) {
                Box(modifier = Modifier.size(24.dp), contentAlignment = Alignment.Center) {
                    Icon(
                        painter = painterResource(
                            if (selected) destination.iconFilledRes else destination.iconOutlineRes
                        ),
                        contentDescription = destination.label,
                        tint = colors.iconTint,
                        modifier = Modifier.size(24.dp),
                    )
                    if (destination.showBadge) {
                        // 8dp bg.critical dot at IconSlot's top-right, +2dp outside on both axes.
                        Box(
                            modifier = Modifier
                                .align(Alignment.TopEnd)
                                .offset(x = 2.dp, y = (-2).dp)
                                .size(8.dp)
                                .background(Wiom.bg.critical, shape = CircleShape),
                        )
                    }
                }
            }
        }
        Text(
            text = destination.label,
            style = Wiom.type.label.sm,
            color = colors.labelColor,
            maxLines = 1,
        )
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — wrapped in horizontal padding (creates a gutter), Material default icons,
//             no navigation-bar inset, hardcoded heights, label hidden
Box(modifier = Modifier.padding(horizontal = 16.dp)) {       // bar is edge-to-edge!
    NavigationBar(modifier = Modifier.height(56.dp)) {       // never override height
        NavigationBarItem(
            icon = { Icon(Icons.Default.Home, null) },       // wrong icon family
            label = null,                                    // labels stay on
            selected = true, onClick = { /*…*/ },
        )
    }
}

// ✅ RIGHT — fillMaxWidth at the parent (edge-to-edge), custom rounded outline / filled
//             drawables (no Material Icons.* — see API note above), navigationBarsPadding
//             handled by the component, label visible
WiomNavigationBar(
    destinations = listOf(
        WiomNavDestination(
            route = "home",
            label = "Home",
            iconOutlineRes = R.drawable.ic_home_rounded,           // outline glyph
            iconFilledRes  = R.drawable.ic_home_rounded_filled,    // filled glyph (same metaphor)
        ),
        // …
    ),
    currentRoute = "home",
    onNavigate = { route -> nav.navigate(route) },
)
```

---

## 8. Screen-level recipe

```kotlin
val destinations = remember {
    listOf(
        WiomNavDestination("home",    "Home",    R.drawable.ic_home_rounded,    R.drawable.ic_home_rounded_filled),
        WiomNavDestination("plans",   "Plans",   R.drawable.ic_plans_rounded,   R.drawable.ic_plans_rounded_filled),
        WiomNavDestination("bills",   "Bills",   R.drawable.ic_bills_rounded,   R.drawable.ic_bills_rounded_filled,
                           showBadge = true),                                       // unread bill
        WiomNavDestination("profile", "Profile", R.drawable.ic_profile_rounded, R.drawable.ic_profile_rounded_filled),
    )
}
val currentRoute by viewModel.route.collectAsState()

Scaffold(
    bottomBar = {
        WiomNavigationBar(
            destinations = destinations,
            currentRoute = currentRoute,
            onNavigate = { route -> viewModel.navigate(route) },
        )
    },
) { padding ->
    // Root content. Push screens (detail, edit, flow) navigate to a separate
    // Scaffold without a bottomBar — the bar is gone, not disabled.
    HomeRoot(modifier = Modifier.padding(padding))
}
```

`Scaffold`'s `bottomBar` slot already pads its content above the bar's
height + navigation-bar inset, so the root `LazyColumn` / `Column`
content never collides with the bar. Push screens use a separate
Scaffold without `bottomBar` — the bar isn't rendered, isn't dimmed,
isn't disabled. It's gone.

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Drawing a custom bar with manual rectangles + text — always use `WiomNavigationBar`.
- ❌ Action verbs in labels ("Pay", "Add") — use nouns for destinations.
- ❌ Using the Selected pill colour on non-Selected items as decoration.
- ❌ Animating the bar on every route change — platform-native crossfade on icon / label tint is enough.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Navigation Bar canvas: `3069:4373`.
Re-pull `get_variable_defs` on `1935:76` if foundations token names
ever change.

| Node | Component / variant |
|---|---|
| `1913:10` | NavItem component set — 2 state variants |
| `1910:2` | NavItem · `State = Default` |
| `1913:3` | NavItem · `State = Selected` |
| `1935:76` | NavigationBar component — `Show Tab3 / Tab4 / Tab5` booleans (4 ship configs) |

**Description-vs-ship discrepancies** — if a stale doc says otherwise,
trust the values that ship in `1935:76`:

- The Default-state icon ships at `icon.nonAction` (#898296 Neutral_500), not `icon.action` (#352D42 Neutral_900). Foundations classifies these as decorative-inside-tappable-row icons; the label provides the affordance signal. Older skill drafts mapped this to `icon.action` — the ship value is the source of truth.
- The bar's top border uses `stroke.subtle` (#D7D3E0), not `stroke.default`. Some older doc strings reference `stroke.default` for the bar — the ship value is `stroke.subtle`.
- The Selected label uses the `text.selected` token (#D9008D), which equals `text.brand` in hex but is the foundations-canonical name for selected-state text. Use `text.selected` everywhere the design context shows the Selected variant.
- The IconPill's horizontal padding (`space-16`) is **fixed** — the pill width does not grow with label width. A long Selected label will be wider than its pill; that's intentional.

---

## See also

- `wiom-design-foundations` — token source. Especially `bg.selected` / `text.selected` / `icon.nonAction` / `stroke.subtle`, plus the `icon.action` vs `icon.nonAction` rule (decorative-in-tappable-row).
- `wiom-compose-layout-rules` — Modifier discipline; edge-to-edge wrappers (`navigationBarsPadding`, `windowInsetsPadding`); `Icons.Rounded.*` enforcement.
- `wiom-top-bar` — the paired top-of-screen surface. Same edge-to-edge contract; uses `statusBarsPadding` instead of `navigationBarsPadding`.
- `wiom-badge` — the 8 dp notification dot pattern (`bg.critical`).
- `wiom-cta` — primary actions belong here (sticky bar / inline / FAB), **not** inside the navigation bar. The nav bar holds destinations only.
- `wiom-tabs-filters` — for switching between sections **inside** a single screen (Pill Tab / Underline Filter). Distinct from the navigation bar, which switches between top-level destinations.
