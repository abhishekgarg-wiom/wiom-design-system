---
name: wiom-top-bar
description: TopBar component for Wiom DS — the header at the top of every screen. Covers 3 Sizes (Small 64dp / Medium 112dp / Large 152dp) × 4 States (Default / Centered / Scrolled / Search). Tells you which Size × State to pick, what the boolean/text/swap properties do per variant, and which Wiom tokens back each part. Pairs with wiom-design-foundations for token values.
---

# Wiom TopBar

One component covers every top-of-screen header. Pick a **Size** for visual weight, a **State** for structural intent, then compose content with boolean toggles, instance-swap (icons) and text properties.

Always use this component for screen headers. Never assemble a custom row of icons + title — it will drift from Wiom spacing and Material proportions.

---

## Step 1 — Pick a Size

| Size | Height | When to use |
|---|---|---|
| **Small** | 64dp | **Default for 90% of screens.** Secondary screens (detail, settings, forms), modals (with Centered state), search UI. If unsure, use Small. |
| **Medium** | 112dp | Only when the screen is content-heavy and the **title deserves more weight** than Small can give. Examples: "Payment history", "All invoices", a major list that benefits from an anchored label. Use sparingly — maybe 10% of screens. |
| **Large** | 152dp | **Strictly for hero / landing surfaces.** One Large per app flow at most. Examples: home tab root, dashboard greeting ("Good morning, X"), profile root. **Do not use Large for anything else** — overuse flattens its effect and eats scroll real-estate on budget devices. |

### Large rules (strict)
- One Large per flow. If two screens in a row want Large, only the landing one gets it.
- Never on list screens, form screens, detail screens, modals, or settings — always Small there.
- If you can't name a single reason this specific screen deserves a hero title, use Small.
- Large collapses to Small on scroll — that's its whole point. If the screen doesn't scroll meaningfully, it probably doesn't need Large either.

---

## Step 2 — Pick a State

| State | Use when | Available on |
|---|---|---|
| `Default` | Standard screen header. | Small · Medium · Large |
| `Centered` | **Modal / bottom-sheet headers only.** Constrained to 1 trailing icon (or a Text CTA) so the title stays truly centered. | Small only |
| `Scrolled` | Switch to this **programmatically** when scroll offset > 0. Applies `shadow/sm`. Never ship at rest. | Small · Medium · Large |
| `Search` | Search mode is active. Leading becomes back (exit search) + pill-shaped search field replaces the title. | Small only |

### Size × State matrix
Eight valid combinations. The four blank cells are intentional — the picker shows them as gaps.

| | Default | Centered | Scrolled | Search |
|---|---|---|---|---|
| Small | ✓ | ✓ | ✓ | ✓ |
| Medium | ✓ | — | ✓ | — |
| Large | ✓ | — | ✓ | — |

---

## Properties

16 total (14 content + 2 variants). Properties are defined at the set level — some don't apply to every variant. The table below flags which props do something per variant.

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Show Leading` | BOOL | true | Hide only on root/home screens with no back target |
| `Leading Icon` | INSTANCE_SWAP | `arrow_back` | Swap to `close` (modal), `menu` (drawer), `account_circle` (profile) |
| `Title` | TEXT | "Title" | Screen title · keep ≤ 24 chars on 360dp |
| `Show Subtitle` | BOOL | false | Enable only when supporting context adds real value (status, timestamp, location) |
| `Subtitle` | TEXT | "Subtitle" | 2nd line · Small only |
| `Show Trailing 1` | BOOL | false | First (rightmost-priority) icon action |
| `Trailing Icon 1` | INSTANCE_SWAP | `search` | Swap: search / share / tune / settings |
| `Show Trailing 2` | BOOL | true | Second icon action |
| `Trailing Icon 2` | INSTANCE_SWAP | `notifications` | Action icon |
| `Show Trailing 3` | BOOL | true | Third action — usually overflow |
| `Trailing Icon 3` | INSTANCE_SWAP | `more_vert` | Overflow menu icon |
| `Show Trailing Text` | BOOL | true | Text CTA (e.g., "Save", "Done", "Cancel") instead of or next to icons |
| `Trailing Text` | TEXT | "Save" | CTA label · `type/label/md` · `color/brand/primary` |
| `Size` | VARIANT | Small | Small / Medium / Large |
| `State` | VARIANT | Default | Default / Centered / Scrolled / Search |

### Which props actually do something per variant

| Variant | Leading | Title | Subtitle | Trailing 1 | Trailing 2 | Trailing 3 | Trailing Text |
|---|---|---|---|---|---|---|---|
| Small Default | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Small Centered | ✓ | ✓ | ✓ | ✓ | — | — | ✓ |
| Small Scrolled | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Small Search | ✓ | — | — | — | — | — | ✓ |
| Medium Default/Scrolled | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |
| Large Default/Scrolled | ✓ | ✓ | — | ✓ | ✓ | ✓ | ✓ |

Toggling a boolean where there's no slot does nothing — Figma still shows the property in the panel.

---

## Common configurations

### Secondary screen (most common)
`Size=Small`, `State=Default`, `Title="Profile"`, `Show Trailing 1=true`, `Trailing Icon 1=search`

### Modal / bottom-sheet header with text action
`Size=Small`, `State=Centered`, `Leading Icon=close`, `Title="Edit profile"`, `Show Trailing Text=true`, `Trailing Text="Save"`

### Home / root screen (no back)
`Size=Small`, `State=Default`, `Show Leading=false`, `Title="Wiom"`, `Show Trailing 1/2/3=true`, `Trailing Icon 1=search`, `Trailing Icon 2=notifications`, `Trailing Icon 3=account_circle`

### Drawer home
`Size=Small`, `State=Default`, `Leading Icon=menu`, `Title="Dashboard"`, `Show Trailing 2=true`, `Trailing Icon 2=notifications`

### Screen with subtitle
`Size=Small`, `State=Default`, `Title="Notifications"`, `Show Subtitle=true`, `Subtitle="Last updated just now"`

### Scrolled (runtime transition)
Same props as Default, just swap `State=Default` → `State=Scrolled` when scroll offset > 0.

### Search active
`Size=Small`, `State=Search`, `Show Trailing Text=true`, `Trailing Text="Cancel"` (optional — back arrow already exits search)

### Content-heavy header (Medium)
`Size=Medium`, `State=Default`, `Title="Payment history"`, `Show Trailing 1=true`, `Trailing Icon 1=tune`, `Show Trailing 3=true`, `Trailing Icon 3=more_vert`

### Hero landing (Large — restricted)
`Size=Large`, `State=Default`, `Leading Icon=menu`, `Title="Good morning,\nAbhishek"`, `Show Trailing 2=true`, `Trailing Icon 2=notifications`, `Show Trailing 3=true`, `Trailing Icon 3=account_circle`

---

## Token bindings

| Part | Token |
|---|---|
| Container fill | `color/surface/base` |
| Search pill fill | `color/surface/subtle` |
| Search pill radius | `radius/full` |
| Search pill placeholder | `type/body/lg` · `color/text/disabled` |
| Title — Small | `type/title/lg` (20sp Medium 28lh) · `color/text/primary` |
| Title — Medium | `type/heading/lg` (24sp Bold 32lh) · `color/text/primary` |
| Title — Large | `type/heading/xl` (32sp Bold 44lh) · `color/text/primary` |
| Subtitle | `type/body/sm` (12sp Regular 16lh) · `color/text/secondary` |
| Text CTA | `type/label/md` (14sp SemiBold 20lh) · `color/brand/primary` · `space/12` padding (44dp touch target) |
| Icons | `icon/md` (24dp) · Material Symbols Rounded FILL=1 · `color/text/secondary` |
| Icon touch target | 48dp (Material standard) — via `space/12` padding + 24dp glyph |
| Small horizontal padding | `space/4` outer (yields 16dp from glyph edge to screen edge) |
| Small vertical padding | `space/8` (with 48dp slot → 64dp total) |
| Search variant right padding | `space/16` (keeps pill 16dp from right edge) |
| Small title column inset | `space/12` left/right |
| Title ↔ subtitle gap | `space/4` |
| Medium title area | `space/8` top + 32lh + `space/8` bottom = 48dp |
| Large title area | `space/12` top + 44lh + `space/32` bottom = 88dp |
| Elevation (Scrolled) | `shadow/sm` |

**All heights hug from padding + line-height.** Nothing is pinned.

---

## Rules

### Size selection
1. **Small is the default.** If you can't name a specific reason for Medium or Large, use Small.
2. **Large is for landing screens only.** One per flow. Home tab root, dashboard greeting, profile root. Never on lists, forms, content detail, modals, settings.
3. **Medium sits between.** Use when the screen has a prominent title that Small can't carry but that doesn't warrant Large (e.g., "Payment history", "All invoices" when those are major destinations).
4. **Don't mix Sizes on the same flow step.** If navigating from a Small screen to a detail, stay Small. Size changes should be navigational markers (root → detail), not decorative.

### State selection
5. **Centered is for modals only.** Modal / bottom-sheet headers use `close` leading + centered title. Never on full-screen nav destinations.
6. **Scrolled is a runtime state.** Apply only when scroll offset > 0. Ship `Default` at rest.
7. **Search owns the bar.** In Search state the pill replaces the title — no icon trailing actions. Text CTA "Cancel" is optional.

### Leading
8. **Match the leading icon to the interaction.** `arrow_back` (navigation back), `close` (dismiss modal), `menu` (drawer), `account_circle` (profile shortcut), logo component (branded root).
9. **Root screens don't have back.** Toggle `Show Leading=false` on tab-root destinations — or swap to `menu` / logo.

### Trailing actions
10. **Cap icon trailing at 3.** The 3rd slot is usually `more_vert` (overflow). More than 3 and the bar becomes cluttered — move secondary actions into the overflow menu or contextual sheets.
11. **Text CTA vs icon CTA.**
    - Use **Text CTA** for explicit textual actions in modal/form contexts: "Save", "Done", "Cancel", "Post".
    - Use **icon** for glanceable actions: search, share, notifications.
    - Don't stack a Text CTA *and* 3 icons — pick one or the other for the trailing slot.
12. **Text CTA is never destructive.** For irreversible actions (delete account, cancel subscription), use a full Button component inside the screen — not a top-bar CTA.

### Title & subtitle
13. **Titles ≤ 24 chars on 360dp.** Longer and they truncate with ellipsis. Rephrase or move length to a subtitle.
14. **Subtitle earns its space.** Only enable `Show Subtitle` when the second line adds real context (live status, timestamp, location). Don't use it for decoration.
15. **Never hardcode title color per instance.** The state-driven color is baked into the component.

### Layout
16. **Height is never fixed.** Every height hugs from padding + typography. Don't override with a fixed pixel height — if padding tokens change, the bar recomputes.
17. **Width fills the parent.** 360dp is the preview width; in real use the component fills whatever container it's dropped into.

---

## Do / Don't

**Large size**
- ✅ DO use Large on the home/dashboard/profile root screen — one per flow.
- ❌ DON'T use Large on list, form, detail, modal, or settings screens — those are Small.

**Centered state**
- ✅ DO use Centered on modal and bottom-sheet headers with `close` leading.
- ❌ DON'T use Centered on full-screen navigation destinations — Material convention is start-aligned.

**Scrolled state**
- ✅ DO apply `State=Scrolled` programmatically when the underlying scroll offset is > 0.
- ❌ DON'T ship `State=Scrolled` at rest — the shadow is a scroll affordance, not decoration.

**Leading icon**
- ✅ DO swap leading to match the interaction: back / close / menu / profile / logo.
- ❌ DON'T reuse `arrow_back` on root screens or modals — the leading tells the user what happens on tap.

**Trailing actions**
- ✅ DO cap at 3 icons, or combine 1 Text CTA + 1–2 supporting icons.
- ❌ DON'T stack 4+ icons or combine Text CTA + 3 icons — the bar becomes noisy.

**Subtitle**
- ✅ DO show a subtitle when it carries live status, timestamp, or a secondary identifier.
- ❌ DON'T show a subtitle just to fill space — the title alone is enough for most screens.

---

## Anti-patterns

- Large on a list / form / detail / modal / settings screen.
- Two consecutive Large screens in the same flow.
- Centered title on a full navigation screen.
- Scrolled shadow visible before the user has scrolled.
- `arrow_back` leading on a tab-root screen.
- 4+ trailing icons (move to overflow).
- Text CTA for destructive actions (use a Button inside the screen).
- Title and subtitle typed as one string into the Title field (use the Subtitle slot).
- Drawing custom icons instead of swapping via `Leading Icon` / `Trailing Icon`.
- Overriding title color per instance — the token binding is already correct for the state.

---

## Implementation hint (Jetpack Compose)

Map 1:1 to Material3's TopAppBar variants.

```kotlin
when (cfg.size) {
  Size.Small -> when (cfg.state) {
    State.Default, State.Scrolled -> TopAppBar(
      title = { SmallTitle(cfg.title, cfg.subtitle) },
      navigationIcon = { if (cfg.showLeading) WiomIconButton(cfg.leadingIcon) },
      actions = { Trailing(cfg) },
      colors = wiomTopBarColors(),
      modifier = Modifier.shadowIf(cfg.state == State.Scrolled, Wiom.shadow.sm)
    )
    State.Centered -> CenterAlignedTopAppBar(
      title = { Text(cfg.title, style = Wiom.type.titleLg, textAlign = TextAlign.Center) },
      navigationIcon = { WiomIconButton(cfg.leadingIcon) },
      actions = { Trailing(cfg, maxIcons = 1) }
    )
    State.Search -> TopAppBar(
      title = { SearchPill(cfg.query, cfg.onQueryChange) },
      navigationIcon = { WiomIconButton(WiomIcon.ArrowBack, cfg.onExitSearch) },
      actions = { if (cfg.showTrailingText) TextCta(cfg.trailingText) }
    )
  }
  Size.Medium -> MediumTopAppBar(
    title = { Text(cfg.title, style = Wiom.type.headingLg) },
    navigationIcon = { if (cfg.showLeading) WiomIconButton(cfg.leadingIcon) },
    actions = { Trailing(cfg) },
    scrollBehavior = cfg.scrollBehavior
  )
  Size.Large -> LargeTopAppBar(
    title = { Text(cfg.title, style = Wiom.type.headingXl) },
    navigationIcon = { if (cfg.showLeading) WiomIconButton(cfg.leadingIcon) },
    actions = { Trailing(cfg) },
    scrollBehavior = cfg.scrollBehavior // collapse to Small
  )
}
```

- `Trailing(cfg)` renders Text CTA (if shown) followed by up to 3 icons.
- Collapse behaviour (Large → Small) uses Material3's `TopAppBarDefaults.enterAlwaysScrollBehavior()` — don't animate height manually.
- `SearchPill` = `Surface` + `Wiom.color.surface.subtle` + `RoundedCornerShape(999.dp)` + `BasicTextField` with `Wiom.type.bodyLg`.
