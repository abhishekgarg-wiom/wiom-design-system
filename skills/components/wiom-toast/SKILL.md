---
name: wiom-toast
description: >
  Wiom Toast / Snackbar component system — transient feedback messages shown at the bottom of the screen.
  Use this skill EVERY TIME you need to show quick feedback after a user action, API response, or system event
  in the Wiom Partner or Customer app. Trigger when you see Toast, Snackbar, Notification (in-app),
  feedback message, "action failed", "action succeeded", "offline", "retry", "undo", or any transient bottom-of-screen
  message pattern. Also trigger when deciding between toast, banner, inline error, empty state, or result screen.
---

# Wiom Toast / Snackbar System

The approved transient feedback foundation for the Wiom app ecosystem (Android/Kotlin/Compose).
**4 types · 3 behaviors · Smart properties · 1 component set**

---

## 1. What Is a Toast?

A passive, transient message shown at the **bottom of the screen** to provide quick feedback about a user action or system event. Auto-dismisses or requires minimal interaction.

**Key characteristics:**
- Appears from bottom, slides up
- Floats above screen content with shadow
- Short message, single line preferred
- Optional action or close button
- Max 1 visible at a time (no stacking)
- **Not** a replacement for inline errors, banners, or result screens

---

## 2. The 3 Toast Behaviors

| Behavior | Icon | Action CTA | Close X | Duration | Use Case |
|----------|------|-----------|---------|----------|----------|
| **Informational** | Yes | No | No | 2s auto | Server rejected action, call failed, status changed |
| **Actionable** | Yes | Yes | No | 8s auto | User just did something undoable — "Delete kiya `Undo`" |
| **Persistent** | Yes | Optional | Yes | Until dismissed | Offline mode, critical state user must acknowledge |

---

## 3. The 4 Types

Each type changes the background tint, the icon, and (for Warning only) the text color. Body text defaults to `text.default` on Error / Info / Success surfaces. **Warning is the exception — it uses `text.onWarning` (#372902 olive) because the gold-tinted surface would fail AA with `text.default`.**

| Type | Fill | Icon | Icon tint | Text | Sample |
|------|------|------|-----------|------|--------|
| **Error** | `bg.critical.subtle` #FFE5E7 | `Icons.Rounded.Error` | `icon.critical` #D92130 | `text.default` | "Server ne request reject kiya" |
| **Warning** | `bg.warning.subtle` #FFE9A1 | `Icons.Rounded.Warning` | `icon.warning` #E2B203 | `text.onWarning` #372902 | "Offline — purana data dikh raha hai" |
| **Info** | `bg.info.subtle` #F1E5FF | `Icons.Rounded.Info` | `icon.info` #6D17CE | `text.default` | "Status update ho gayi" |
| **Success** | `bg.positive.subtle` #E1FAED | `Icons.Rounded.CheckCircle` | `icon.positive` #008043 | `text.default` | "Nikaasi safal rahi" |

---

## 4. Token Reference

### Container

| Property | Value | Token |
|----------|-------|-------|
| Width | 328px (screen 360 − 16px × 2) | — |
| Default height | 48px (single line: 12V pad × 2 + 24 LH) | — |
| Max height | 72px (2 lines: 12V pad × 2 + 24 LH × 2) | — |
| Height | Hug content, max 2 lines | — |
| Radius | 8px | `radius-small` |
| Horizontal padding | 16px | `space-16` |
| Vertical padding | 12px | `space-12` |
| Gap (icon-text) | 8px | `space-8` |
| Gap (text-action) | 8px | `space-8` |
| Shadow | `shadow.lg` | 0 4px 12px rgba(0,0,0,0.15) |
| Position | Bottom, 16px margin each side, 8px above safe area | — |

### Typography

| Element | Style | Token |
|---------|-------|-------|
| Message | Noto Sans Regular 400, 16sp, LH 24 | `type.body.lg` |
| Action CTA | Noto Sans SemiBold 600, 16sp, LH 24 | `type.label.lg` |

### Icon

| Element | Size | Library | Token |
|---------|------|---------|-------|
| Leading icon | 24×24dp | Material Symbols Rounded (filled) | `icon.md` |
| Close icon | 24×24dp | Material Symbols Rounded (filled) | `icon.md` |

---

## 5. Decision Tree — Toast vs Other Feedback

```
Something happened that needs user feedback. Ask:

1. Did the ENTIRE screen fail to load? (no data at all)
   → EMPTY STATE screen (not toast)

2. Is it a FORM/INPUT VALIDATION error? (user typed wrong value)
   → INLINE ERROR on the field (not toast)

3. Is it a PERSISTENT STATE user must be aware of?
   (offline, account hold, quality review)
   → BANNER at top of screen (not toast)

4. Is it a TERMINAL OUTCOME of a multi-step flow?
   (withdrawal completed/failed, payment result)
   → DEDICATED RESULT SCREEN (not toast)

5. Is it QUICK FEEDBACK to a single tap?
   (server rejected, call failed, action queued, undoable delete)
   → TOAST ← use this

Then decide behavior:
  Informational (2s auto) — fire and forget
  Actionable (8s auto)   — user can Undo/Retry
  Persistent (dismissible) — critical, must acknowledge

Then decide type:
  Error / Warning / Info / Success
```

### One-liner

> **Toast = quick feedback to a single action, no explanation needed, user can retry immediately.**
> 
> **Everything else gets a bigger component.**

---

## 6. Type Decision

```
What does the feedback represent?

Server rejected / call failed / action blocked     → ERROR
Offline / degraded / non-blocking caution           → WARNING
Neutral state change / info / no semantic weight   → INFO
Positive outcome (rare — app avoids celebration)   → SUCCESS
```

**Wiom Constitution rule:** No congratulatory messages. Success toasts are used sparingly — only for outcomes where user genuinely needs confirmation (e.g., wallet withdrawal sent).

---

## 7. Behavior Decision

```
What does the user need to do with this toast?

Nothing — just informing them                → Informational (2s)
Act fast — offer Undo/Retry/View            → Actionable (8s)
Acknowledge — critical state that persists  → Persistent (dismissible)
```

### Duration reasoning
- **2s** — fast read, user moves on. Matches app spec.
- **8s** — long enough to read AND tap action button.
- **Persistent** — stays until user or state change dismisses.

---

## 8. Figma Component

Location: Sandbox - SAAD → Toast / Snackbar section

### Smart component structure

1 component set, 4 variants (Type), 6 additional properties:

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| **Type** | Variant | Error | Error / Warning / Info / Success |
| **Show Icon** | Boolean | true | Toggle leading icon |
| **Show Action** | Boolean | false | Toggle action CTA |
| **Show Close** | Boolean | false | Toggle close X |
| **Message** | Text | "Toast message" | Editable message text |
| **Action Text** | Text | "Retry" | Editable action label |
| **Icon Swap** | Instance Swap | error | Swap to any icon from library |

**Why only 4 variants?** Booleans handle Icon/Action/Close visibility. Text properties handle message content. Instance swap handles icon. Variants would have exploded to 4 × 2 × 2 × 2 = 32 — this structure keeps it lean.

---

## 9. Universal Rules

1. **Max 1 toast visible at a time** — never stack. Queue if needed.
2. **Bottom position only** — never top, never middle.
3. **16px margin each side** (328px on 360px screen) — matches screen-gutter insets.
4. **No retry loops** — one attempt, revert state + toast, user re-taps manually.
5. **Message max 2 lines** — target single line; 2 is tolerated for Hindi copy. Beyond 2, text truncates with ellipsis — that's a signal to rewrite shorter copy.
6. **Hindi is primary** — English as fallback only.
7. **No congratulatory messages** per app constitution. Avoid "Great!", "Awesome!", etc.
8. **Action CTA is optional, text-only** — no filled buttons in toasts.
9. **Token-only values** — every color, spacing, radius, shadow from foundation.
10. **Toast is passive** — body of toast is never tappable. Only the Action CTA or Close X are interactive.

---

## 10. Never Do

| Anti-Pattern | Why |
|-------------|-----|
| Stacking multiple toasts | Confuses hierarchy. Queue them. |
| Toast for form validation | Use inline error — stays visible until fixed. |
| Toast for persistent state | Use banner — doesn't auto-dismiss. |
| Toast for multi-step outcome | Use result screen — gives context + next step. |
| Long message (3+ lines, truncates) | Breaks quick-glance model. Rewrite shorter. Max 2 lines. |
| Tappable toast body | Only action button or close are tappable. |
| Success toast for every action | Feels spammy. Use only when user needs confirmation. |
| Icon-only toast (no text) | Text is the message. Icon is supporting. |
| Filled button in toast | Tertiary/text style only. Filled competes with screen CTAs. |
| Toast with no auto-dismiss | Unless Persistent (has close button), always auto-dismiss. |

---

## 11. Compose Implementation

```kotlin
// Informational toast (default, 2s auto-dismiss)
WiomToast(
    type = WiomToastType.Error,
    message = "Server ne request reject kiya",
)

// Actionable toast (8s auto-dismiss)
WiomToast(
    type = WiomToastType.Info,
    message = "Ticket delete kiya",
    actionText = "Undo",
    onActionClick = { /* restore ticket */ },
    duration = 8.seconds,
)

// Persistent toast (dismissible)
WiomToast(
    type = WiomToastType.Warning,
    message = "Offline — purana data dikh raha hai",
    showClose = true,
    onCloseClick = { /* dismiss */ },
    duration = null, // persistent
)

// Custom icon swap
WiomToast(
    type = WiomToastType.Info,
    message = "5 NetBox bhej diye",
    icon = Icons.Filled.Inventory2, // swap default info icon
)

// Scaffold integration
Scaffold(
    snackbarHost = {
        WiomToastHost(
            hostState = toastHostState,
            modifier = Modifier.padding(horizontal = 16.dp, vertical = 8.dp)
        )
    }
) { /* content */ }
```

### Toast host API

```kotlin
@Composable
fun WiomToastHost(
    hostState: WiomToastHostState,
    modifier: Modifier = Modifier
) {
    val current by hostState.currentToast.collectAsState()
    AnimatedVisibility(
        visible = current != null,
        enter = slideInVertically(
            initialOffsetY = { it },
            animationSpec = tween(300, easing = EaseOut)
        ) + fadeIn(),
        exit = slideOutVertically(
            targetOffsetY = { it },
            animationSpec = tween(300, easing = EaseIn)
        ) + fadeOut(),
    ) {
        current?.let { WiomToast(it) }
    }
}
```

---

## 12. Animation Specs

| Transition | Duration | Easing |
|------------|----------|--------|
| Slide in (from bottom) | 300ms | EaseOut |
| Slide out (to bottom) | 300ms | EaseIn |
| Auto-dismiss timer | 2s / 8s / infinite | — |

Used Smart Animate in Figma prototype to match code behavior.

---

## 13. Strict Rules

### DO
- Use the decision tree to pick toast vs other feedback
- Use `type.body.lg` (16sp Regular) for message
- Use `type.label.lg` (16sp SemiBold) for action CTA
- Use `shadow.lg` for the floating effect
- Position at bottom, 16px margin each side
- Auto-dismiss 2s (informational), 8s (actionable)
- Use icons from Material Symbols Rounded (filled) library
- Queue toasts — never stack

### DO NOT
- Do NOT use toast for form validation (use inline error)
- Do NOT use toast for persistent state (use banner)
- Do NOT use toast for multi-step outcomes (use result screen)
- Do NOT stack or overlap multiple toasts
- Do NOT wrap message to 2+ lines
- Do NOT make the toast body tappable
- Do NOT use filled buttons inside toasts
- Do NOT use generic text ("Error", "Success") — be specific about what happened
- Do NOT hardcode colors, spacing, radius, or typography — use tokens/styles

### WHEN IN DOUBT
Ask: "Can the user fix this right now by tapping the same button again?" — if yes, toast is fine. If no, use something bigger.

---

## See also

- `wiom-design-foundations` — color tokens, warning-family rules, patterns.md banner/toast tables
- `wiom-compose-layout-rules` — Compose structural rules, Icons.Rounded.* enforcement
- `wiom-badge` — related inline feedback surface (same token family)
- `wiom-cta` — action CTAs inside Actionable toasts
