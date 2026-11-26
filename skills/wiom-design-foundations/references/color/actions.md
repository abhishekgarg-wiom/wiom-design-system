# Action Tokens (CTAs and interactive elements)

17 tokens across 5 CTA types: Primary, Secondary, Tertiary, Pre-booking, Destructive. Plus focus rings and text links.

---

## CTA hierarchy rules

1. **One Primary CTA per screen.** This is enforceable — lint flags screens with 2+.
2. **One Destructive CTA per screen.** Also enforceable.
3. Secondary and Tertiary can appear multiple times.
4. Primary is always the highest-priority action on the screen.
5. Destructive is only for irreversible destruction — never for Dismiss, Skip, Cancel, Close, etc.

**The per-screen rate limits apply to CTA *buttons*, not to the underlying tokens.** `bg.critical` may appear multiple times on a screen for non-CTA patterns — notification badges (red dot with count), avatar fallback fills (one of the 4-color palette), form error underlines. `bg.brand` likewise appears in many non-CTA contexts (selected tabs, icons, links) without counting against the "1 Primary per screen" rule. If you're looking at a screen and counting, count buttons, not hex occurrences.

---

## Icons inside CTAs

**Leading or trailing icons inside any CTA use the `icon.*` family** — the same namespace rule as icons everywhere else in the system. Each CTA type maps to one existing icon token. No new tokens needed.

### Mapping per CTA type

| CTA type | Icon tint token | Hex | Paired with text token |
|---|---|---|---|
| Primary (`bg.brand`) | `icon.inverse` | #FAF9FC (Neutral_50) | `text.onBrand` (#FFFFFF) |
| Secondary (outlined) | `icon.brand` | #D9008D (Brand_600) | `text.brand` (#D9008D) |
| Tertiary (text-only) | `icon.brand` | #D9008D (Brand_600) | `text.brand` (#D9008D) |
| Pre-booking (`bg.brand.accent`) | `icon.action` | #352D42 (Neutral_900) | `text.onBrandAccent` (#161021) |
| Destructive (`bg.critical`) | `icon.inverse` | #FAF9FC (Neutral_50) | `text.onCritical` (#FFFFFF) |
| Any CTA, disabled | `icon.disabled` | #A7A1B2 (Neutral_400) | `text.disabled` (#A7A1B2, Neutral_400) |

All pairings are AA-verified (see `contrast.md`). Every icon-vs-text pair is either the identical hex or differs by less than 0.05 in relative luminance — visually indistinguishable.

### Why icon and label hexes differ by one step on filled CTAs

Solid icons carry more visual mass than text glyphs at the same color. Stepping the icon one lightness level lighter rebalances the pair (icon `#FAF9FC` vs label `#FFFFFF` on Primary/Destructive; icon `#352D42` vs label `#161021` on Pre-booking). Same principle as `icon.action` = Neutral_900 and `text.default` = Neutral_950 elsewhere. Full rationale in `core-tokens.md`.

### Code example

```kotlin
// Primary CTA with leading icon
Button(
    onClick = onRecharge,
    colors = ButtonDefaults.buttonColors(
        containerColor = WiomColors.bgBrand,
        contentColor = WiomColors.textOnBrand,
    ),
) {
    Icon(
        imageVector = Icons.Default.FlashOn,
        contentDescription = null,
        tint = WiomColors.iconInverse,              // ← from icon.* family
    )
    Spacer(Modifier.width(8.dp))
    Text("Recharge now")
}

// Destructive CTA
Button(
    onClick = onDeletePlan,
    colors = ButtonDefaults.buttonColors(
        containerColor = WiomColors.bgCritical,
        contentColor = WiomColors.textOnCritical,
    ),
) {
    Icon(
        imageVector = Icons.Default.Delete,
        contentDescription = null,
        tint = WiomColors.iconInverse,              // ← same token as Primary
    )
    Spacer(Modifier.width(8.dp))
    Text("Delete plan")
}
```

### Set `tint` explicitly on CTA icons

`Button(contentColor)` propagates to child Icons automatically — but it would push `#FFFFFF` (pure white) into the Icon, overriding `icon.inverse` (#FAF9FC). Set `tint` explicitly to preserve the icon-weight calibration.

### Disabled state (per CTA type)

All disabled CTAs share one pattern: `bg.disabled` fill + `text.disabled` label + `icon.disabled` glyph. The three tokens have two distinct hexes — `bg.disabled` (#D7D3E0, Neutral_300, lighter) vs. `text.disabled` / `icon.disabled` (#A7A1B2, Neutral_400, darker) — so content stays visible above the disabled surface at ~1.64:1 (sub-AA, intentional). Disabled signals "unavailable," not "readable."

| CTA | Fill | Label | Icon | Stroke |
|---|---|---|---|---|
| **Primary** | `bg.brand` → **`bg.disabled`** | `text.onBrand` → **`text.disabled`** | `icon.inverse` → **`icon.disabled`** | — |
| **Destructive** | `bg.critical` → **`bg.disabled`** | `text.onCritical` → **`text.disabled`** | `icon.inverse` → **`icon.disabled`** | — |
| **Pre-booking** | `bg.brand.accent` → **`bg.disabled`** | `text.onBrandAccent` → **`text.disabled`** | `icon.action` → **`icon.disabled`** | — |
| **Secondary** (outlined) | transparent — unchanged | `text.brand` → **`text.disabled`** | `icon.brand` → **`icon.disabled`** | `stroke.brand.focus` → **`stroke.subtle`** (#D7D3E0) |
| **Tertiary** (text-only) | transparent — unchanged | `text.brand` → **`text.disabled`** | `icon.brand` → **`icon.disabled`** | none |

**Why one unified pattern:**

`icon.disabled` used to be #D7D3E0 (same hex as `bg.disabled`), which made the icon invisible on disabled filled surfaces — so the old guidance was "dim one side, not both" and filled CTAs had to keep their rest-state `text.on*` / `icon.*` tokens. With `icon.disabled` repointed to Neutral_400 (#A7A1B2, same hex as `text.disabled`), triple-dim now works: bg is lighter, content is darker, the component reads as "muted but structured."

**All disabled CTAs:** no pointer events (`cursor: not-allowed` on web, `onClick = null` in Compose).

**Stroke on disabled Secondary:** `stroke.subtle` (#D7D3E0, Neutral_300) matches the disabled-surface gray in hex — visually the border blends with the `bg.disabled` family without needing a dedicated `stroke.disabled` token.

### Icons OUTSIDE the button (but near it)

A help icon next to a form field, a chevron on a list row, a status icon in a banner — those are not inside a CTA, and they use the regular `icon.*` tokens by their existing rules (`icon.action` for tappable chrome, `icon.nonAction` for decorative, `icon.critical` for alerts inside error banners, etc.).

---

## Primary CTA — filled, max 1 per screen

Used for the main action: "Recharge", "Book now", "Confirm payment", "Continue".

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.brand` | #D9008D | Brand_600 | Primary fill (rest) |
| `bg.brand.hover` | #C00080 | Brand_700 | Primary hover |
| `bg.brand.pressed` | #A30070 | Brand_800 | Primary pressed |
| `text.onBrand` | #FFFFFF | Gray_50 | Primary label |

Contrast: `text.onBrand` on `bg.brand` = 4.84:1 (AA pass). See the Compose example under "Icons inside CTAs" below for the Button setup pattern (same for all filled CTAs).

---

## Secondary CTA — outlined, no rest fill

Used for alternate actions: "Learn more", "See details", "Change plan". Pairs naturally next to a Primary CTA.

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `text.brand` | #D9008D | Brand_600 | Label (rest + pressed) |
| `text.brand.hover` | #C00080 | Brand_700 | Label on hover |
| `stroke.brand.focus` | #D9008D | Brand_600 | Border (rest + pressed) + focus ring |
| `bg.brand.subtle` | #FFE5F6 | Brand_100 | Pressed state fill (shared with Tertiary) |

**No rest-state background.** The button is transparent at rest, with a saturated pink border.

When pressed, add `bg.brand.subtle` fill. Contrast: `text.brand` on `bg.default` = 4.62:1 (AA).

---

## Tertiary CTA — text-only, no rest fill or border

Used for lightest-weight actions, often inline: "Edit", "Clear", "Skip", "Change".

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `text.brand` | #D9008D | Brand_600 | Label (rest + pressed) — **reused from Secondary** |
| `text.brand.hover` | #C00080 | Brand_700 | Label on hover — **reused from Secondary** |
| `bg.brand.subtle` | #FFE5F6 | Brand_100 | Pressed state fill — **reused from Secondary** |

**No rest bg. No stroke.** Just `text.brand` label. Pressed state adds `bg.brand.subtle`.

---

## Pre-booking CTA — customer app only, bright pink + dark label

Customer-app-specific for pre-booking flows. The bright pink distinguishes it from Primary CTAs in the same flow.

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.brand.accent` | #FF66C9 | Brand_400 | Pre-booking fill (rest) |
| `bg.brand.accent.hover` | #FFB2E4 | Brand_300 | Pre-booking hover (**placeholder** — see note) |
| `bg.brand.accent.pressed` | #EA5DB8 | Brand_500 | Pre-booking pressed |
| `text.onBrandAccent` | #161021 | Neutral_950 | Label (all states) |

**Dark text, not white.** Bright pink is a light fill; white label fails AA (2.5:1). Dark label gives 7.07:1 on rest state.

**Hover is a placeholder** (Brand_300 is lighter than rest — unusual). Will be revisited when system-wide hover conventions are defined. Do not copy this pattern elsewhere.

Only for pre-booking flows. Do not use for generic "book", "buy", or other transactional CTAs.

---

## Destructive CTA — irreversible actions only

Used for **actual destruction**: delete data, cancel plan, remove payment method. Rate-limited to 1 per screen.

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `bg.critical` | #D92130 | Critical_600 | Destructive fill (rest) |
| `bg.critical.hover` | #BF1D2A | Critical_700 | Destructive hover |
| `bg.critical.pressed` | #A31824 | Critical_800 | Destructive pressed |
| `text.onCritical` | #FFFFFF | Gray_50 | Destructive label |

### When NOT to use Destructive CTA

Secondary or Tertiary (not Destructive) for: Skip, Cancel (closing a form), No thanks, Maybe later, Go back, Close, Dismiss, Not interested.

Red is only justified when the user is losing data they can't recover. If there's an undo, it's not destructive.

### Where `bg.critical` IS allowed outside the destructive CTA

The red hex shows up elsewhere in the system, and that's fine:

- **Form error states** — input border, error helper text, checked-control-in-error fill
- **Notification badges** — `bg.critical` dot with a count number on an icon (not rate-limited)
- **Avatar fallback fills** — one of the 4-color palette (see SKILL.md)
- **Focus rings on destructive inputs** — `stroke.critical.focus`

None of these count as a "Destructive CTA" for the per-screen limit. Count buttons, not hex occurrences.

---

## Focus — depends on the element type

Focus rings are keyboard-only (not shown on touch). The token you use depends on whether the focused element is destructive.

| Element focused | Focus ring token | Hex |
|---|---|---|
| Primary CTA (`bg.brand`) | `stroke.brand.focus` | #D9008D (Brand_600) |
| Secondary CTA (outlined brand) | `stroke.brand.focus` | #D9008D — already the border, ring emphasizes on focus |
| Tertiary CTA (text-only brand) | `stroke.brand.focus` | #D9008D |
| Pre-booking CTA | `stroke.brand.focus` | #D9008D |
| **Destructive CTA** (`bg.critical`) | **`stroke.critical.focus`** | **#D92130 (Critical_600)** |
| Non-CTA interactive (input, checkbox, radio) | `stroke.brand.focus` | #D9008D |
| Input in error state | `stroke.critical.focus` | #D92130 |

**The rule is simple: destructive elements get red focus rings, everything else gets brand pink.** This is a mechanical mapping — agents reading the skill pick the right token by asking "is this destructive?"

### Ring geometry

- 2dp thick, sits **outside** the component edge (not inside)
- For Primary CTAs where the ring would touch same-colored border: offset by 2dp gap so the ring is on `bg.default`, not on the button fill
- For outlined Secondary CTAs: the existing border is Brand_600; on focus, a second ring appears outside, also Brand_600, separated by a 1-2dp gap

Status elements (positive, info) also have `stroke.positive.focus` and `stroke.info.focus` tokens. Use those for focused positive/info elements (e.g., a focused success card). Warning doesn't have a focus variant — warning is a feedback surface, not an interactive one.

---

## Links

Inline links in body text use:
- `text.brand` for the link color (rest)
- `text.brand.hover` for hover
- Underline on hover (or always, per design decision)

---

## What's intentionally missing

### No `action.positive` / green CTA
Confirmation actions ("Confirm", "Proceed", "Yes", "Save") use `bg.brand` — the primary CTA. Wiom doesn't use green "yes" / red "no" color coding. If an action is affirmative, the button label says so.

### No hover on Pre-booking other than the placeholder
The current Brand_300 hover is a stand-in. Don't invent a new token to replace it — wait for the system-wide hover convention.

### No `action.warning` / yellow CTA
Warning is a feedback signal, not an action. Buttons inside a warning banner use brand styling.

### No `action.info` / purple CTA
Same reasoning. Info-colored buttons don't exist.

---

## CTA decision tree

```
User needs a button. Which token set?

1. Is this the one main action on the screen?
   YES → Primary CTA (bg.brand + text.onBrand)

2. Is this an alternate but prominent action?
   YES → Secondary CTA (text.brand + stroke.brand.focus)

3. Is this a lightweight inline action (Edit, Skip, Clear)?
   YES → Tertiary CTA (text.brand only)

4. Is this a customer-app pre-booking action?
   YES → Pre-booking CTA (bg.brand.accent + text.onBrandAccent)

5. Is the user about to lose data irreversibly?
   YES → Destructive CTA (bg.critical + text.onCritical)
   NO  → Go back to step 2 or 3. It's not destructive.
```

---

## Contrast summary (all AA-verified)

| Pairing | Ratio | Tier |
|---|---|---|
| `text.onBrand` / `bg.brand` | 4.84 | AA |
| `text.onCritical` / `bg.critical` | 4.99 | AA |
| `text.onBrandAccent` / `bg.brand.accent` | 7.07 | AA+ |
| `text.brand` / `bg.brand.subtle` | 4.10 | AA-large |
| `text.brand` / `bg.default` | 4.62 | AA |

`text.brand` on `bg.brand.subtle` passes AA-large only (for 18pt+ / 14pt-bold). Use `text.default` for small body text on brand-subtle surfaces.
