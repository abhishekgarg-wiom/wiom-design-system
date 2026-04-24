---
name: wiom-navigation-bar
description: Navigation Bar component for Wiom DS — the persistent bottom bar that switches between top-level destinations. Covers 2–5 tabs via Show Tab3 / Tab4 / Tab5 toggles, 2 item states (Default / Selected), icon+label+optional badge per item. Tells you when to use it (and when not to), how to configure tab count, and which Wiom tokens back each part. Pairs with wiom-design-foundations for token values.
---

# Wiom Navigation Bar

The bottom bar that lets the user switch between the app's top-level destinations. One bar per app, always at the bottom of a root screen, always persistent while on those root screens.

This skill covers two components together:
- **NavItem** — atom with 2 states (`Default`, `Selected`).
- **NavigationBar** — the assembled bar. Fixed slots for Tab1 and Tab2, plus `Show Tab3`, `Show Tab4`, `Show Tab5` booleans so one component handles 2–5 tabs.

Always use the assembled `NavigationBar` on screens. Use bare `NavItem` only inside documentation or when composing a non-standard surface (rare).

---

## When to use

| Use | Don't use |
|---|---|
| Switching between 2–5 **top-level** destinations (Home, Plans, Bills, Profile, Support). | Switching between sections inside a single screen — that's Tabs/Filters. |
| Every tab is independently reachable any time. | The destinations form a linear flow (onboarding, KYC, checkout). |
| The user should always see where they are and where else they can go. | A detail, edit, or modal screen that the user *returned to* from a tab — hide the bar there. |

---

## When NOT to show the navigation bar

Hide the bar (don't just disable it) on:

1. **Non-root screens** — any screen pushed on top of a tab (detail page, edit, form, picker). The back affordance takes over.
2. **Full-flow experiences** — onboarding, OTP verification, KYC, payment, installation scheduling. These are linear; switching tabs mid-flow loses state.
3. **Immersive media** — video, full-screen image viewer, camera.
4. **System sheets** — bottom sheets, dialogs, and alerts already own the bottom surface.
5. **Keyboard-open text entry screens** — let the keyboard have the vertical space; the bar would sit above the keyboard and steal attention.

---

## Tab count — pick the smallest set that works

| Tab count | When | How |
|---|---|---|
| **3** | Most apps should aim here. Home + 2 key destinations. | `Show Tab3=on`, `Show Tab4=off`, `Show Tab5=off` |
| **4** | When a fourth top-level destination is genuinely needed. | `Show Tab3=on`, `Show Tab4=on`, `Show Tab5=off` |
| **5** | Only if every tab is used weekly by most users. Five is the hard cap. | `Show Tab3=on`, `Show Tab4=on`, `Show Tab5=on` |
| **2** | Technically supported. Valid only when you have exactly two peer destinations — otherwise use a top bar or tabs inside one screen. | `Show Tab3=off`, `Show Tab4=off`, `Show Tab5=off` |

Tab1 and Tab2 are always visible — they cannot be hidden. Put the highest-traffic destination in Tab1.

Do not add a sixth tab by stretching, inserting a "More" menu, or swapping the bar for a scrollable one. If you need six, you have a scope problem — merge destinations or demote one to a top-bar action.

---

## NavItem — states

Exactly one item in the bar is `Selected` at any time. All others are `Default`.

| State | Use when |
|---|---|
| `Default` | This is not the active destination. Icon outline in `icon.nonAction`, label in `text.subtle`, no pill fill. |
| `Selected` | This is the current destination. Icon filled (`icon.brand`), sitting on a `bg.selected` pill, label in `text.selected`. |

There is no `Disabled` state. A tab that can't be opened right now shouldn't be in the bar at all.

There is no `Pressed` state as a variant — ripple/state-layer is handled by the platform (Material ripple in Compose, touch highlight on iOS). Don't draw a pressed variant in Figma instances.

---

## NavItem — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `State` | VARIANT | Default | `Default` or `Selected` |
| `icon` | INSTANCE_SWAP | per-tab default | Swap to any Material Symbols Rounded icon. `icon.md` = 24dp. Filled automatically in `Selected`. |
| `label` | TEXT | "Home" | One-word destination name. Keep it short — see rules below. |
| `Show Label` | BOOL | true | Hide only in exceptional icon-only configurations (accessibility regression — avoid). |
| `Show Badge` | BOOL | false | Reveal the notification dot at the top-right of the icon pill. |
| `badge` | INSTANCE_SWAP | red dot | Default is an 8dp `negative/primary` dot. Swap for a count chip only when the count is meaningful. |

## NavigationBar — properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Show Tab3` | BOOL | true | Reveal the third tab |
| `Show Tab4` | BOOL | true | Reveal the fourth tab |
| `Show Tab5` | BOOL | true | Reveal the fifth tab |

The bar does not expose a `Selected index` as a variant — you set one child NavItem to `Selected` via nested-instance overrides.

---

## Configuration patterns

Quick configs. All values are property overrides on a NavigationBar instance, plus per-NavItem overrides for icon/label/state.

### Standard 5-tab (Home / Plans / Profile / Bills / Support)
Default bar. Set Tab1 to `Selected` on Home screen, Tab2 on Plans screen, etc.

### 4-tab (Home / Plans / Bills / Profile)
`Show Tab5=off`. Drop Support from the bar; move it into Profile or a top-bar help icon.

### 3-tab (Home / Plans / Profile)
`Show Tab4=off`, `Show Tab5=off`. Cleanest option for a focused app.

### Unread indicator on Bills
On the Bills NavItem, `Show Badge=true`. Keep the default red dot — don't use a number unless the count is load-bearing (e.g., "3 due today").

### Selected = Home (default launch)
Tab1.State=Selected, Tab2–5.State=Default. Update on route change.

---

## Token bindings (summary)

Every part resolves to a Wiom foundation token. Full values live in `wiom-design-foundations`.

| Part | Token |
|---|---|
| Bar background | `bg.default` |
| Bar top border | `stroke-small` + `stroke.subtle` |
| Bar bottom padding | `space-12` (+ OS safe-area inset below that) |
| Item container padding | `space-4` horizontal · `space-8` vertical |
| Item gap (pill → label) | `space-4` |
| IconPill padding | `space-16` horizontal · `space-4` vertical |
| IconPill radius | `radius-full` |
| IconPill fill (Default) | transparent |
| IconPill fill (Selected) | `bg.selected` (#FFCCED Brand_200) |
| Icon size | `icon.md` (24dp) · `Icons.Rounded.*` |
| Icon color (Default) | **`icon.nonAction`** (#898296 — soft-unselected chrome; Nav Bar unselected icons use `icon.nonAction`, NOT `icon.action`) |
| Icon color (Selected) | `icon.brand` (#D9008D) · FILL=1 |
| Label typography | `type.label.sm` (12sp SemiBold, 16 line-height) |
| Label color (Default) | `text.subtle` |
| Label color (Selected) | `text.selected` (#D9008D) |
| Badge fill | `bg.critical` |
| Badge size | 8dp · `radius-full` · positioned at top-right of IconSlot |

No shadow. No hardcoded hex. No fixed item width — each item is `flex: 1 0 0`, so the bar redistributes evenly when tabs are hidden.

---

## Rules

1. **Exactly one Selected at a time.** The bar reflects the current route — not preferences, not recent activity. Update on navigation.
2. **Labels stay on.** 12sp is small enough; hiding labels fails first-time users and low-vision users. Don't set `Show Label=false` to "clean up" the bar.
3. **One word per label.** "Home", "Plans", "Bills" — not "My Bills", not "View Plans". Use `type.label.sm` as-is; don't shrink the type.
4. **Icons use the filled pair on Selected.** The selected icon should be the filled variant of the default (Material Symbols FILL axis). Never use two unrelated glyphs across states.
5. **Badges are rare and meaningful.** Show only when there's an unread/actionable item. Never badge more than one tab at once unless two things genuinely need attention — it dilutes signal.
6. **Never put a CTA in the bar.** No "Add", no plus FAB merged into a tab. If you need a primary action, it belongs elsewhere (top bar, inline CTA, FAB above the bar).
7. **Hide on push.** When the user navigates into a detail or flow, the bar goes away. Don't leave a disabled bar underneath — it invites mis-taps.
8. **Respect the safe area.** The bar has `space-12` intrinsic bottom padding; add OS bottom inset on top of that so content doesn't collide with home indicators / gesture bars.
9. **Tabs 1 and 2 are always visible.** Design for the 2-tab fallback; don't put high-traffic destinations in Tab3–5 and then toggle them off for some users.
10. **Don't restyle per screen.** Same bar on every root destination — same order, same icons, same labels. Consistency is the whole point.

---

## Anti-patterns

- Drawing a custom bar with manual rectangles + text — always use the component.
- More than 5 tabs; adding a "More" menu; horizontal scroll.
- Putting action verbs in labels ("Pay", "Add") — use nouns for destinations.
- Keeping the bar visible on detail / flow / modal screens and relying on disabled state to prevent taps.
- Badging every tab to highlight "new" features — the bar turns into noise.
- Using the selected pill color as decoration on non-selected items.
- Hiding labels to save vertical space — the bar's height comes from padding + typography, not label visibility.
- Swapping the icon to a completely different glyph between Default and Selected (different metaphor = confusion).
- Using `Show Tab3..5=off` to hide role-gated destinations per user type. If a destination is gated, it should still be in the same slot when it appears — don't reshuffle the bar by user.
- Animating the bar on every route change — platform-native crossfade on icon/label is enough.

---

## Implementation hint (Jetpack Compose)

Use `NavigationBar` + `NavigationBarItem` from Material 3 and bind colors/typography to Wiom tokens. Skeleton:

```kotlin
NavigationBar(
  containerColor = Wiom.bg.default,
  tonalElevation = 0.dp,
  modifier = Modifier
    .fillMaxWidth()
    .drawTopBorder(Wiom.stroke.small, Wiom.stroke.subtle)
    .windowInsetsPadding(WindowInsets.navigationBars)
    .padding(bottom = Wiom.space.s12)
) {
  destinations.forEach { dest ->
    NavigationBarItem(
      selected = dest.route == currentRoute,
      onClick = { navigate(dest.route) },
      icon = {
        BadgedBox(badge = { if (dest.hasBadge) WiomDot() }) {
          Icon(
            painter = painterResource(if (dest.route == currentRoute) dest.iconFilled else dest.iconOutline),
            contentDescription = null,
            modifier = Modifier.size(Wiom.icon.md)
          )
        }
      },
      label = { Text(dest.label, style = Wiom.type.labelSm) },
      alwaysShowLabel = true,
      colors = NavigationBarItemDefaults.colors(
        selectedIconColor = Wiom.icon.brand,
        selectedTextColor = Wiom.text.selected,
        indicatorColor = Wiom.bg.selected,
        unselectedIconColor = Wiom.icon.nonAction,
        unselectedTextColor = Wiom.text.subtle,
      )
    )
  }
}
```

`alwaysShowLabel = true` is non-negotiable (see Rule 2). `indicatorColor` is the pill behind the selected icon — keep it `bg.selected`, do not deepen it to `bg.brand`. The destinations list is the source of truth for tab count; at render time, a list of 3 items produces a 3-tab bar with no extra config needed.

---

## See also

- `wiom-design-foundations` — `bg.selected` token, icon.action rules for chrome, badge pattern
- `wiom-compose-layout-rules` — Modifier discipline, Icons.Rounded.* enforcement
- `wiom-top-bar` — paired top chrome
- `wiom-badge` — the 8dp notification dot (`bg.critical`)
