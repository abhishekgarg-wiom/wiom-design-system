---
name: wiom-toast
description: >
  Wiom Toast / Snackbar system — passive transient feedback at the
  bottom of the screen. 4 types × 3 behavior modes. Reference for
  generating Jetpack Compose code with WiomToast / WiomToastHost and
  for instancing the existing Figma Toast component (set 2222:478) via
  MCP. Use this skill EVERY TIME you show quick feedback after a user
  action, API response, or system event in the Wiom Partner or Customer
  Android app, or when assembling Figma frames with toasts. Trigger on:
  Toast, Snackbar, Notification (in-app), feedback message, action
  failed, action succeeded, offline, retry, undo, instance of 2222:478,
  bottom-of-screen transient message. Also trigger when deciding
  between toast, banner, inline error, empty state, or result screen.
---

# Wiom Toast / Snackbar System

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component
set `2222:478`, and the `wiom-design-foundations` token set.

**4 types · 3 behavior modes · 1 component set.**

A passive, transient message shown at the **bottom of the screen** after
a user action or system event. Auto-dismisses or — if Persistent —
requires an explicit close. Toast is a feedback layer, not a content
layer; it's not a replacement for inline errors, banners, or result
screens (see §6 Decision logic).

---

## 1. Figma — instancing the component

When assembling a Figma frame via MCP, instance the component set
`2222:478` and set the variant + booleans below. Strings are
**case-sensitive**.

| Property | Type | Default | Accepted values |
|---|---|---|---|
| `Type` | Variant | `Error` | `Error` · `Warning` · `Info` · `Success` |
| `Show Icon` | Boolean | `true` | Toggles the leading 24 dp icon. Almost always `true` (icon is the type's main signifier). |
| `Show Action` | Boolean | `false` | Toggles the trailing brand-pink action label. Set `true` for Actionable behavior (Undo / Retry / View). |
| `Show Close` | Boolean | `false` | Toggles the trailing 24 dp close ✕. Set `true` for Persistent behavior. |
| `Message` | Text | `"Toast message"` | Editable message string |
| `Action Text` | Text | `"Retry"` | Editable action label (visible only when `Show Action = true`) |
| `Icon Swap` | Instance swap | per-type default | Swap to any icon from `Foundation/icons` (renders at 24 dp) |

> **Why 4 ship variants and not 32?** Figma uses booleans + text props
> instead of exploding variants — a Type × showIcon × showAction ×
> showClose matrix would be 4 × 2 × 2 × 2 = 32. The 4-variant + 3-
> boolean structure ships the same designs with one quarter of the
> variant explosion.

### Code enum ↔ Figma variant string

| `WiomToastType` (Kotlin) | Figma `Type` value |
|---|---|
| `Error` | `Error` |
| `Warning` | `Warning` |
| `Info` | `Info` |
| `Success` | `Success` |

| `WiomToastBehavior` (Kotlin) | Figma boolean toggles |
|---|---|
| `Informational` | `Show Icon = true`, `Show Action = false`, `Show Close = false` |
| `Actionable` | `Show Icon = true`, `Show Action = true`, `Show Close = false` |
| `Persistent` | `Show Icon = true`, `Show Close = true`, `Show Action = optional` |

---

## 2. Anatomy

| Slot | Value | Token |
|---|---|---|
| Container width | Derived — `fillMaxWidth()` inside the surface margin (toast surface owns `space-8` start / end) | — |
| Container height | Derived — `space-12 V` + content (24 LH at `body.lg` for single line) + `space-12 V` = 48 dp single line | — |
| Container radius | 8 dp | `radius-small` |
| Container fill | `bg.default` (#FAF9FC) — **same for all 4 types**; only the icon (and Warning's text) carries the type color | `bg.default` |
| Container shadow | `shadow.lg` (drop) | `shadow.lg` |
| Horizontal padding | 16 dp | `space-16` |
| Vertical padding | 12 dp | `space-12` |
| Inner row | Vertically centered, items align center; gap `space-8` between every adjacent item | — |
| Leading icon | 24 dp | (`icon.md` size) |
| Message style | Noto Sans Regular 16 / 24 LH | `type.body.lg` |
| Action label style | Noto Sans SemiBold 16 / 24 LH | `type.label.lg` |
| Close icon | 24 dp | (`icon.md` size) |

**No fixed dimensions on the container.** Width emerges from the surface
margin contract; height emerges from padding + content. Don't set
`Modifier.width(...)`, `Modifier.height(...)`, `heightIn`, `widthIn`, or
`Modifier.size(...)` on the toast container.

---

## 3. The 4 types — same container, different icon

All 4 types share the **same `bg.default` container**. The only per-type
differences are the leading icon color (and one text-color exception
for Warning).

| Type | Icon (Material Symbols Rounded, filled) | Icon color | Message text color | Sample copy |
|---|---|---|---|---|
| **Error** | `error` | `icon.critical` (#D92130) | `text.default` | "Server ne request reject kiya" |
| **Warning** | `warning` | `icon.action` (#352D42 dark) inside a `bg.warning.subtle` circle | `text.onWarning` (#372902 olive) | "Offline — purana data dikh raha hai" |
| **Info** | `info` | `icon.info` (#6D17CE) | `text.default` | "Status update ho gayi" |
| **Success** | `check_circle` | `icon.positive` (#008043) | `text.default` | "Nikaasi safal rahi" |

**Action label colour is always `text.brand` (#D9008D)**, regardless of
type — the Undo / Retry / View affordance is the screen's brand action,
not the toast's status colour.

> **Warning text exception.** Warning is the only type whose message
> text uses a non-default colour. White on `bg.warning` would fail AA;
> the foundations rule is that `text.onWarning` (dark olive) is the
> only text token paired with the warning family across surfaces. On
> `bg.default` it still passes AAA, so Warning toasts adopt it for
> consistency with banners / chips.

---

## 4. The 3 behavior modes

Behavior is **not** a Figma variant — it's a pattern composed from the
booleans + duration. Pick the row matching the user's intent.

| Behavior | Show Action | Show Close | Duration | Use for |
|---|---|---|---|---|
| **Informational** | No | No | 2 s auto | Fire-and-forget feedback. Server rejected an action; status changed; queued for processing. |
| **Actionable** | Yes | No | 8 s auto | User just did something **undoable** — "Ticket delete kiya" + Undo. Long enough to read AND tap. |
| **Persistent** | Optional | **Yes** | None (until close ✕ or state change) | Critical state the user must acknowledge — Offline mode, account on hold, quality review pending. |

### Duration reasoning

- **2 s** — fast read, user moves on. Anything shorter feels skipped.
- **8 s** — the floor for an action a user must read AND tap. Match
  Material's 8-second snackbar guidance.
- **No auto-dismiss** — only when state-driven (offline → online clears
  it; or user taps close). Anything else risks the user missing it.

---

## 5. Surface contract — placement

| Placement | What the surface (host) owns | What the toast does |
|---|---|---|
| Bottom of screen, above nav bar | `space-8` start / end horizontal margin · `space-16` bottom margin above safe-area inset · vertical animation slot | `fillMaxWidth()` inside that margin (lands at 344 dp on a 360 dp screen). No width set on the toast itself. |

**The host is the surface, not the toast.** `WiomToastHost` (defined
in §8) places the toast inside the surface margin and runs the animation
in/out. The `WiomToast` composable itself doesn't know about screen
edges.

```
┌─────────────────── 360 dp screen ────────────────────┐
│                                                       │
│                     content                           │
│                                                       │
│  ←8→ ┌─────────────────────────────────┐ ←8→         │   ← toast surface margin
│       │ ●  Message text…       Action │              │
│       └───────────────────────────────┘              │
│                                                       │
│  ─────────── safe-area / nav bar ──────────────       │
└───────────────────────────────────────────────────────┘
```

Anchors above OS navigation / gesture bar via `navigationBarsPadding()`
inside the host.

---

## 6. Decision logic — toast vs other feedback

```
Step 1 — Is toast even the right surface?
  ENTIRE screen failed to load (no data at all)?
    → Empty state screen (not toast)
  FORM / INPUT validation error (user typed wrong value)?
    → Inline error on the field (not toast)
  PERSISTENT state user must continually be aware of (offline, account hold)?
    → Banner at the top of screen (not toast)
  TERMINAL outcome of a multi-step flow (withdrawal completed, payment result)?
    → Dedicated result screen (not toast)
  QUICK feedback to a single tap (server rejected, call failed, queued, undoable delete)?
    → Toast (proceed)

Step 2 — Type
  Server rejected / call failed / action blocked      → Error
  Offline / degraded / non-blocking caution           → Warning
  Neutral state change / info, no semantic weight     → Info
  Positive outcome the user genuinely needs to know   → Success (use sparingly)

Step 3 — Behavior
  User just informed, nothing to do                    → Informational (2 s)
  User can undo / retry / view                         → Actionable (8 s)
  Critical state until acknowledged                    → Persistent (close ✕)
```

### One-liner

> **Toast = quick feedback to a single action, no explanation needed,
> user can retry immediately. Everything else gets a bigger component.**

### Wiom Constitution rules (copy)

- **No congratulatory messages.** Avoid "Great!", "Awesome!", "Done!".
  Success toasts only when the user genuinely needs confirmation
  (e.g., wallet withdrawal sent).
- **Hindi primary, English fallback.** Toast copy is short — Hindi
  reads denser; favor it.
- **Specific over generic.** "Server ne request reject kiya" beats
  "Error". "Ticket delete kiya" beats "Success".

---

## 7. Universal rules

1. **Max 1 toast visible at a time.** Never stack. Queue if the host
   gets a second toast while one is showing — show the new one only
   after the first dismisses.
2. **Bottom position only.** Never top, never middle.
3. **No fixed dimensions on the toast.** Width comes from the host's
   surface margin (`space-8` each side); height comes from
   `space-12 V` + content. No `width(...)`, `height(...)`,
   `heightIn`, `widthIn`, `size(...)`.
4. **Toast body is passive — never tappable.** Only the Action label
   and Close ✕ are interactive. Don't wrap the whole toast in
   `Modifier.clickable`.
5. **Action label is text-only, brand-pink** (`text.brand`,
   `type.label.lg`). No filled buttons inside a toast.
6. **No retry loops.** One attempt — revert state + show toast — user
   re-taps the originating control manually if they want to retry.
7. **Message: 1 line target, 2 lines max at the default text size**
   (about 80 Hindi characters). At larger font scales the toast grows
   so the whole message shows; it is never cut off (no `maxLines`).
   The toast caps at 40 % of the screen height; beyond that the message
   scrolls inside it (`Modifier.heightIn(max = …)` + `verticalScroll`).
   Owner decision 27 Nov 2026, same on iOS.
8. **Tokens only.** Element-first (`bg.*` / `text.*` / `icon.*`).
   No raw hex. No legacy `color.*` role-first tokens.
9. **All 4 types share `bg.default`.** Only the icon (and Warning's
   message text) carries the type colour.
10. **Action label colour is always `text.brand`**, regardless of
    toast type.
11. **Persistent toasts must show Close ✕.** A toast with no
    auto-dismiss and no close is a stuck toast.
12. **No congratulatory copy.** Per Wiom Constitution.

---

## 8. Compose API + code

### Public API

```kotlin
enum class WiomToastType { Error, Warning, Info, Success }

// Label + handler are paired — eliminates the "label without handler" bug class.
data class ToastAction(val label: String, val onClick: () -> Unit)

sealed class WiomToastBehavior {
    object Informational : WiomToastBehavior()                     // 2 s, no action, no close
    data class Actionable(                                         // 8 s, action visible, no close
        val action: ToastAction,
    ) : WiomToastBehavior()
    data class Persistent(                                         // until close ✕ or state change
        val action: ToastAction? = null,                           // optional; pair is required-together
        val onClose: () -> Unit,
    ) : WiomToastBehavior()
}

data class WiomToastSpec(
    val type: WiomToastType,
    val message: String,
    val behavior: WiomToastBehavior = WiomToastBehavior.Informational,
    val icon: ImageVector? = null,                                 // null = type's default icon
)

@Composable
fun WiomToast(
    spec: WiomToastSpec,
    modifier: Modifier = Modifier,
)

// Host — manages queue, animation in/out, auto-dismiss timer.
// `bottomInset` reserves space for a Wiom bottom nav bar, sticky CTA bar,
// or any other bottom chrome. Pass 0 (default) when there's only the OS
// nav bar — `navigationBarsPadding()` handles that automatically.
@Composable
fun WiomToastHost(
    hostState: WiomToastHostState,
    bottomInset: Dp = 0.dp,
    modifier: Modifier = Modifier,
)

class WiomToastHostState {
    private val channel = Channel<WiomToastSpec>(Channel.UNLIMITED)
    private val _current = MutableStateFlow<WiomToastSpec?>(null)
    val current: StateFlow<WiomToastSpec?> = _current.asStateFlow()

    // Pumps the channel one-at-a-time. The host calls this in a LaunchedEffect.
    internal suspend fun consume() {
        for (spec in channel) {
            _current.value = spec
            // Host's auto-dismiss timer (or user close) sets _current back to null.
            // Wait until _current becomes null before pulling the next.
            _current.first { it == null }
        }
    }

    /** Enqueue a toast; returns immediately. The host displays it FIFO. */
    fun show(spec: WiomToastSpec) { channel.trySend(spec) }

    /** Force-close the currently visible toast. */
    fun dismiss() { _current.value = null }
}
```

### Token resolver

```kotlin
@Composable
private fun rememberToastColors(type: WiomToastType): ToastColors = when (type) {
    WiomToastType.Error   -> ToastColors(Wiom.icon.critical, Wiom.text.default)
    WiomToastType.Warning -> ToastColors(Wiom.icon.action,   Wiom.text.onWarning)   // dark glyph on a bg.warning.subtle circle; gold is ratings-only
    WiomToastType.Info    -> ToastColors(Wiom.icon.info,     Wiom.text.default)
    WiomToastType.Success -> ToastColors(Wiom.icon.positive, Wiom.text.default)
}

@Composable
private fun rememberDefaultIcon(type: WiomToastType): ImageVector = when (type) {
    WiomToastType.Error   -> Icons.Rounded.Error
    WiomToastType.Warning -> Icons.Rounded.Warning
    WiomToastType.Info    -> Icons.Rounded.Info
    WiomToastType.Success -> Icons.Rounded.CheckCircle
}

private data class ToastColors(val icon: Color, val message: Color)
```

### `WiomToast` implementation

```kotlin
@Composable
fun WiomToast(spec: WiomToastSpec, modifier: Modifier = Modifier) {
    val colors = rememberToastColors(spec.type)
    val behavior = spec.behavior

    // Resolve action — single source of truth, no nullable-pair gymnastics.
    val action: ToastAction? = when (behavior) {
        is WiomToastBehavior.Actionable -> behavior.action
        is WiomToastBehavior.Persistent -> behavior.action
        WiomToastBehavior.Informational -> null
    }

    Surface(
        color = Wiom.bg.default,                                   // same for all 4 types
        shape = RoundedCornerShape(Wiom.radius.small),             // 8dp
        modifier = modifier
            .fillMaxWidth()                                        // host owns the gutter
            // shadow.lg has Y=4 offset + 12 blur — Compose `shadowElevation` only takes Dp
            // (no offset). Use Modifier.shadow with a clipped shape and live with the limitation,
            // OR draw a custom offset shadow via drawBehind. Simple path:
            .shadow(elevation = 12.dp, shape = RoundedCornerShape(Wiom.radius.small)),
    ) {
        Row(
            modifier = Modifier.padding(
                horizontal = Wiom.space.s16,
                vertical = Wiom.space.s12,
            ),
            horizontalArrangement = Arrangement.spacedBy(Wiom.space.s8),
            verticalAlignment = Alignment.CenterVertically,
        ) {
            // Leading icon. Warning: dark glyph in an Icon Badge Sm
            // (24 dp bg.warning.subtle circle, 16 dp glyph).
            if (spec.type == WiomToastType.Warning) {
                WiomIconBadge(
                    icon = spec.icon ?: rememberDefaultIcon(spec.type),
                    size = WiomIconBadgeSize.Sm,
                    tone = WiomIconBadgeTone.Warning,               // bg.warning.subtle + icon.action
                )
            } else {
                Icon(
                    imageVector = spec.icon ?: rememberDefaultIcon(spec.type),
                    contentDescription = null,                      // message carries the meaning
                    tint = colors.icon,
                    modifier = Modifier.size(24.dp),
                )
            }

            // Message: fills; never truncated (copy guide: 2 lines at default size)
            Text(
                text = spec.message,
                style = Wiom.type.body.lg,
                color = colors.message,
                modifier = Modifier.weight(1f),
            )

            // Action label — wrapped to ensure ≥48dp hit area
            action?.let { a ->
                Box(
                    modifier = Modifier
                        .minimumInteractiveComponentSize()         // ≥48dp WCAG hit area
                        .clickable { a.onClick() },
                    contentAlignment = Alignment.Center,
                ) {
                    Text(
                        text = a.label,
                        style = Wiom.type.label.lg,
                        color = Wiom.text.brand,                    // always brand pink
                        maxLines = 1,
                    )
                }
            }

            // Close ✕ (Persistent only) — 24dp glyph inside ≥48dp hit area
            if (behavior is WiomToastBehavior.Persistent) {
                Box(
                    modifier = Modifier
                        .minimumInteractiveComponentSize()
                        .clickable { behavior.onClose() },
                    contentAlignment = Alignment.Center,
                ) {
                    Icon(
                        imageVector = Icons.Rounded.Close,
                        contentDescription = "Dismiss",
                        tint = Wiom.icon.action,
                        modifier = Modifier.size(24.dp),
                    )
                }
            }
        }
    }
}
```

### `WiomToastHost` implementation (animation + auto-dismiss + surface margin)

```kotlin
@Composable
fun WiomToastHost(
    hostState: WiomToastHostState,
    bottomInset: Dp = 0.dp,                                         // pass nav-bar / sticky-CTA height
    modifier: Modifier = Modifier,
) {
    val current by hostState.current.collectAsState()

    // Drains the queue one toast at a time — never overlaps.
    LaunchedEffect(hostState) { hostState.consume() }

    // Auto-dismiss timer (cancels if behavior is Persistent).
    LaunchedEffect(current) {
        val spec = current ?: return@LaunchedEffect
        val durationMs = when (spec.behavior) {
            WiomToastBehavior.Informational -> 2_000L
            is WiomToastBehavior.Actionable -> 8_000L
            is WiomToastBehavior.Persistent -> null                 // no auto-dismiss
        }
        durationMs?.let { delay(it); hostState.dismiss() }
    }

    // Surface margin lives here, not in WiomToast.
    Box(
        modifier = modifier
            .fillMaxWidth()
            .padding(horizontal = Wiom.space.s8)                    // 8dp: toast floats wider than the 16dp screen gutter
            .padding(bottom = Wiom.space.s16 + bottomInset)         // above any bottom chrome
            .navigationBarsPadding(),                               // OS gesture inset
        contentAlignment = Alignment.BottomCenter,
    ) {
        AnimatedVisibility(
            visible = current != null,
            // 300 ms, standard easing, slide + fade both ways (as shipped, WiomToast.kt:256-263).
            enter = slideInVertically(initialOffsetY = { it }, animationSpec = tween(300)) +
                    fadeIn(animationSpec = tween(300)),
            exit  = slideOutVertically(targetOffsetY = { it }, animationSpec = tween(300)) +
                    fadeOut(animationSpec = tween(300)),
        ) {
            current?.let { WiomToast(it) }
        }
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — hardcoded width, per-type tinted bg, body wrapped in clickable
Box(
    modifier = Modifier
        .width(344.dp)                                              // never hardcode
        .background(Color(0xFFFFE5E7))                              // old per-type tint
        .clickable { /* ... */ },                                   // toast body is passive
) {
    Text("Error: server rejected", style = WiomType.bodyMd /* 14sp old */)
}

// ✅ RIGHT — host owns the gutter, all 4 types share bg.default, action only is tappable
WiomToastHost(hostState = toastHostState)

// elsewhere: enqueue the spec
toastHostState.show(WiomToastSpec(
    type = WiomToastType.Error,
    message = "Server ne request reject kiya",
    behavior = WiomToastBehavior.Actionable(
        action = ToastAction(label = "Retry", onClick = { retry() }),
    ),
))
```

---

## 9. Screen-level recipes

End-to-end usage. Pick the matching pattern, then swap copy.

### Scaffold integration

```kotlin
val toastHostState = remember { WiomToastHostState() }

Scaffold(
    snackbarHost = { WiomToastHost(toastHostState) },
) { padding ->
    // your screen content
}
```

### Informational — fire-and-forget (server rejected)

```kotlin
toastHostState.show(WiomToastSpec(
    type = WiomToastType.Error,
    message = "Server ne request reject kiya",
    behavior = WiomToastBehavior.Informational,                     // 2s auto
))
```

### Actionable — undoable delete

```kotlin
toastHostState.show(WiomToastSpec(
    type = WiomToastType.Info,
    message = "Ticket delete kiya",
    behavior = WiomToastBehavior.Actionable(
        action = ToastAction(label = "Undo", onClick = { restoreTicket() }),
    ),                                                              // 8s auto
))
```

### Persistent — offline mode banner-toast hybrid

```kotlin
// Tied to a connectivity StateFlow — shows on offline, dismisses on online.
LaunchedEffect(isOffline) {
    if (isOffline) {
        toastHostState.show(WiomToastSpec(
            type = WiomToastType.Warning,
            message = "Offline — purana data dikh raha hai",
            behavior = WiomToastBehavior.Persistent(
                onClose = { /* user-acknowledged; reshow on next offline */ },
            ),
        ))
    } else {
        toastHostState.dismiss()
    }
}
```

### With a sticky CTA bar or bottom nav — pass `bottomInset`

```kotlin
val ctaBarHeight = 88.dp                                             // measure or pass from layout
WiomToastHost(toastHostState, bottomInset = ctaBarHeight)
```

---

## 10. Anti-patterns

(Universal rules in §7 cover the bans. Listed here are extras that
aren't already a numbered rule.)

- ❌ Using `bg.warning` / `bg.critical` etc. as the toast container fill.
  All 4 types share `bg.default`. The type colour only paints the icon
  (and Warning's text).
- ❌ Auto-dismissing a Persistent toast. Persistent has no timer — it
  dismisses on close ✕ or on the state-change that produced it.
- ❌ Toast for **form validation** (use inline error on the field) ·
  for **persistent state** (use banner) · for **multi-step outcome**
  (use result screen). All in §6 decision tree.
- ❌ Generic copy ("Error", "Success", "Done"). Be specific:
  "Server ne request reject kiya" / "Ticket delete kiya" /
  "Nikaasi safal rahi".
- ❌ Congratulatory copy ("Great!", "Awesome!", "Done!"). Wiom
  Constitution.
- ❌ Showing a Success toast for every user action — feels spammy.
  Only when the user genuinely needs confirmation.
- ❌ Multiple toasts visible at once. Queue, don't stack.

---

## 11. Animation specs

| Transition | Duration | Easing |
|---|---|---|
| Slide in (from bottom) + fade in | 300 ms | Standard (`tween(300)` default, `FastOutSlowInEasing`) |
| Slide out (to bottom) + fade out | 300 ms | Standard (`tween(300)` default, `FastOutSlowInEasing`) |
| Auto-dismiss timer (Informational) | 2 s | — |
| Auto-dismiss timer (Actionable) | 8 s | — |
| Persistent | until close ✕ / state change | — |

> Easing aligned with the shipped library and iOS skills, 26 Nov 2026 (was `EaseOut` in / `EaseIn` out).

Match Smart Animate in the Figma prototype to keep design ↔ code parity.

---

## 12. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Toast / Snackbar canvas: `2279:5820`.
Re-pull `get_variable_defs` on `2222:478` if foundations token names
ever change.

| Node | Component / variant |
|---|---|
| `2222:478` | Toast component set — 4 type variants × 3 booleans × 2 text props × 1 icon swap |
| `2222:449` | Type=Error — `bg.default` · `icon.critical` · `text.default` |
| `2222:456` | Type=Warning — `bg.default` · Figma still shows gold `icon.warning`; skill uses dark `icon.action` on `bg.warning.subtle` (Figma to update) · `text.onWarning` (olive) |
| `2222:463` | Type=Info — `bg.default` · `icon.info` · `text.default` |
| `2222:470` | Type=Success — `bg.default` · `icon.positive` · `text.default` |

**Description-vs-ship discrepancies** — if a stale doc or old skill
says otherwise, trust ship values:

- **All 4 types share `bg.default`** (#FAF9FC, near-white). Old skill
  said per-type tinted backgrounds (`bg.critical.subtle` for Error,
  `bg.warning.subtle` for Warning, etc.). New ship: same neutral
  container for all, only the icon (and Warning's text) carries the
  status colour.
- **Width is 344 dp** (surface margin 8 dp each side on a 360 dp
  screen), matching foundations ("Toast: 8 dp left + 8 dp right")
  and the shipped app. The toast deliberately floats wider than the
  16 dp gutter used by CTA, list-item, top-bar and bottom-sheet content.
- **Vertical padding is 12 dp** (`space-12`), not 16. Single-line
  height = 12 + 24 LH + 12 = 48 dp.
- **Typography is `type.body.lg` (16 sp Regular)** for the message and
  `type.label.lg` (16 sp SemiBold) for the action label. Old skill
  said 14 sp / `body.md` and `label.md` — Figma now uses 16 sp.
- **Action label colour is always `text.brand`** (#D9008D), not the
  type's status colour.
- **Warning is the only type whose message text uses
  `text.onWarning`** (the dark olive that doubles as warning-family
  text on every surface, per foundations). Other types use
  `text.default`.
- **Warning icon is dark, never gold.** `icon.action` on a
  `bg.warning.subtle` circle behind the glyph; the toast itself stays
  `bg.default`. Gold `icon.warning` is for rating stars only (foundations
  iconography § Warning icons). **Size:** Icon Badge Sm, a 24 dp circle with a 16 dp glyph (Owner decision 27 Nov 2026, same on iOS).
- **Icon colours come from `icon.critical` / `icon.action` (Warning) /
  `icon.info` / `icon.positive`** — element-first `icon.*` tokens,
  not `color.*.primary` legacy.

---

## See also

- `wiom-design-foundations` — token source. Especially the
  warning-family rule (`text.onWarning` works on `bg.warning`,
  `bg.warning.subtle`, AND `bg.default`).
- `wiom-cta` — the action label inside a toast borrows the brand-pink
  treatment but is text-only; no filled CTA inside a toast.
- `wiom-list-item` · `wiom-top-bar` — banner / inline-error patterns
  that toast deliberately is NOT (see §6 decision tree).
- `wiom-bottomsheet` — when an outcome needs more context than a
  toast can give.
- `wiom-icon-badge` — distinct passive indicator (not a toast).
