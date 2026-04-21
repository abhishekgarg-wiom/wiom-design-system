# Wiom Color System

Element-first color tokens for Wiom's Android app (Jetpack Compose) and web. This file replaces the old role-first color reference. Legacy tokens like `color.brand.primary` or `color.warning.primary` (#B85C00 brown) are deprecated — do not use them.

**One system across all Wiom apps.** Customer app, CSP/partner app, and internal tools all use identical tokens. There are no partner-namespaced variants. If a screen in the CSP app needs a color, it comes from the same `bg.*`, `text.*`, `stroke.*`, `icon.*` tokens as the customer app.

---

## The single rule that prevents 90% of color mistakes

**Match the Compose slot to the token namespace. Same prefix on both sides.**

| Compose property you're filling | Token namespace to use |
|---|---|
| `Box(background=…)`, `Surface(color=…)`, `Modifier.background(…)` | `bg.*` |
| `Text(color=…)` | `text.*` |
| `BorderStroke(color=…)`, `Divider(color=…)`, `Modifier.border(…)` | `stroke.*` |
| `Icon(tint=…)`, `painterResource + tint` | `icon.*` |

If you're writing `Text(color = X)`, `X` must come from `text.*`. Never from `bg.*`. This is enforceable by lint and it's the first thing to check when reviewing PRs.

---

## When to read sub-reference files

Read the sub-reference **before** writing code for that area. Each file is a full token table with Compose code examples.

| You're working on… | Read |
|---|---|
| Any core UI chrome (text, backgrounds, borders, icons, banners) | `color/core-tokens.md` |
| Any button or CTA (primary, secondary, tertiary, pre-booking, destructive) | `color/actions.md` |
| Status bar, badges, empty states, tooltips, ratings, avatars, overlays | `color/patterns.md` |
| Chat screens (bubbles, reply, system messages) | `color/chat.md` |
| Primitive palette | `color/primitives.md` |
| Contrast verification, WCAG pairs | `color/contrast.md` |

---

## The seven primitive families

Primitives are `internal` in Kotlin. **Never import a primitive directly in feature code.** Always use a semantic token.

| Family | Range | Purpose |
|---|---|---|
| **Neutral** | 50–950 (11 steps) | Product UI chrome, warm mauve undertone carries brand warmth into grays |
| **Gray** | 50–950 (11 steps) | Pure grayscale for chat domain + source of pure white (Gray_50 = #FFFFFF) |
| **Brand** | 100–900 (9 steps) | Brand_600 = #D9008D (non-negotiable anchor) |
| **Positive** | 100–900 (9 steps) | Success green |
| **Critical** | 100–900 (9 steps) | Error red / destructive |
| **Warning** | 100–900 (9 steps) | Yellow throughout — never orange. Warning_300 (#E2B203) is the gold accent |
| **Info** | 100–900 (9 steps) | Purple. Info_600 = #6D17CE (anchor) |

Step numbers match the Tailwind/Radix convention so AI pattern-matching works. Neutral and Gray have steps 50 and 950 because they cover the full light-to-dark UI range; colored families skip those extremes (step 50 would be indistinguishable from Neutral_50, step 950 would be near-black).

**Total: 67 primitives.**

---

## What's intentionally missing

These gaps are by design. Do not invent tokens to fill them.

**No hover/pressed states on Positive, Info, Warning, or Neutral.** These are feedback families (banners, toasts, badges) — not interactive surfaces. Actions inside a feedback surface (e.g., "Undo" on a success toast) use brand or critical tokens, which have interaction states.

**No green CTA.** Confirmation buttons use `bg.brand`. Wiom presents choices neutrally — no green "yes" / red "no" pressure.

**`bg.critical` as a CTA fill is scoped to destructive actions only.** Delete, cancel-plan, remove-payment-method. Never "Dismiss", "Skip", "Cancel" (closing a form), "Close", "Go back". Those are Secondary or Tertiary. Non-CTA uses of `bg.critical` (badges, avatar fills, form errors) are exempt — see `color/patterns.md`.

---

## Six most common decisions (memorize these)

### 1. "What color is body text?"
`text.default` (#161021 ← Neutral_950) on every light surface — **except warning surfaces.** Warning body and label text uses `text.onWarning` (#372902 ← Warning_900), the one family with its own body-text token. It pairs with both `bg.warning` (solid gold badge, 7.16:1) and `bg.warning.subtle` (warning banner, 8.76:1), creating a cohesive dark-olive identity across the warning family.

### 2. "What's the fill for the main CTA on this screen?"
`bg.brand` (#D9008D ← Brand_600). Max one primary CTA per screen — this is a rate limit, not a suggestion. If you have two actions and both feel primary, one of them is actually Secondary (use Secondary CTA tokens).

### 3. "How do I build a Secondary CTA?"
No rest-state fill. `text.brand` label + `stroke.brand.focus` border (saturated #D9008D). Pressed state adds `bg.brand.subtle` fill. No hover state token — hover uses text color change only.

### 4. "How do I color an error banner?"
`bg.critical.subtle` (#FFE5E7) background, `text.default` body text, `text.critical` (#D92130) for headings/emphasis, `icon.critical` for the alert icon. **Not** white text on red — that's the destructive CTA pattern, used only on buttons.

### 5. "How do I color a warning surface (badge or banner)?"
**Solid badge:** `bg.warning` (#E2B203 ← Warning_300) + `text.onWarning` (#372902 ← Warning_900) label. 7.16:1, AA.
**Subtle banner/card:** `bg.warning.subtle` (#FFE9A1 ← Warning_100) + `text.onWarning` body + `icon.warning` (#E2B203) for the alert icon + optional `stroke.warning` (#E2B203) outline. 8.76:1, AA.
Dark olive text on both — cohesive warning identity across the family. Note: white text on either fill fails AA (1.98:1 on solid gold, ~1.75:1 on subtle yellow); `text.onWarning` is the only valid warning-text token.

### 6. "My CTA has an icon next to the label — what tint?"
Use the `icon.*` family, with one token per CTA type:
- Primary & Destructive → `icon.inverse` (near-white)
- Secondary & Tertiary → `icon.brand` (pink)
- Pre-booking → `icon.action` (dark neutral)
- Any disabled CTA → `icon.disabled`

Icons and labels use slightly different hexes on filled CTAs (e.g., icon `#FAF9FC` vs label `#FFFFFF` on Primary) — this is intentional to rebalance the visual weight of filled icons against text glyphs. Set `tint` explicitly on the `Icon` composable; don't rely on `Button(contentColor)` inheritance for CTA icons. Full mapping: `color/actions.md`.

---

## CTA types and when to use each

| CTA | Use for | Visual |
|---|---|---|
| **Primary** | One main action per screen ("Recharge", "Book", "Continue") | Filled pink |
| **Secondary** | Alternate actions, ~40% weight ("Learn more", "See details") | Outlined pink |
| **Tertiary** | Lightest-weight actions, often inline ("Edit", "Clear", "Skip") | Text only |
| **Pre-booking** | Customer-app booking flows specifically | Filled bright pink (Brand_400) |
| **Destructive** | Delete, cancel-plan, remove — irreversible only | Filled red |

Secondary and Tertiary share the same `bg.brand.subtle` pressed-state fill.

Pre-booking is a customer-app-specific accent, not a generic variant. Do not use Pre-booking colors for non-booking flows.

Full CTA token specs: `color/actions.md`.

---

## Patterns beyond core tokens

Rules for recurring UI patterns (status bar, notification badges, empty states, tooltips, star ratings, trust indicators, avatar fallbacks, image overlays) live in `color/patterns.md`. Each pattern is an application of existing tokens — no new tokens needed.

---

## Dark mode

Not yet defined. Flag to the design system team if a dark-mode screen is requested — do not implement ad-hoc.

---

## Chat domain

Chat uses its own Gray family (pure grayscale, no brand warmth) with namespaced `chat.*` tokens. The tokens are scoped — `chat.bg.canvas` is only valid inside a chat screen. Full token list: `color/chat.md`.

---

## Contrast guarantees (pre-verified AA)

Every common token pairing has been verified against WCAG AA and documented in `color/contrast.md`. When you pair tokens per the rules in this skill, contrast is guaranteed. When you invent pairings or use tokens against their documented `PAIRS` annotation, verify manually.

The one pairing with tight margins: `text.critical` on `bg.critical.subtle` is 4.18:1 (AA-Large only, not AA for small text). Use `text.default` for body copy on error banners and reserve `text.critical` for headings/icons.

---

## Enforcement (lint rules that work)

These rules are mechanically enforceable because the element is in the token namespace:

1. `bg.*` tokens only valid on background properties — Detekt rule can flag `Text(color = WiomColors.bgCriticalSubtle)` as a type-mismatch error
2. `text.*` tokens only valid on Text/label properties
3. `bg.brand` (primary CTA) requires `text.onBrand` child
4. `bg.critical` **used as a CTA button fill** is rate-limited to 1 per screen — the check applies to `Button(containerColor = bgCritical)`, not to badges, avatar fills, or form error states
5. `icon.warning` only valid inside `bg.warning.subtle` **or** inside a rating component (e.g., `RatingBar`, `StarIcon`) — flag everywhere else
6. No raw hex outside the design-system module — primitives are `internal`

If you're writing a lint rule, reference these invariants.

---

## Anti-patterns (mistakes we've seen)

- **Using `bg.brand.subtle` as a text color**: It's a background. Use `text.brand` for brand-tinted text.
- **Using `text.default` on a warning surface**: Warning is the one family with its own body-text token — `text.onWarning` (#372902) pairs with both `bg.warning` (solid badge, 7.16:1) and `bg.warning.subtle` (banner, 8.76:1). `text.warning` as a separate token does not exist; `text.onWarning` is the warning-family text.
- **Adding hover to a success toast button**: The button uses brand tokens (`text.brand` with `text.brand.hover`), not positive tokens.
- **Red button for "Cancel subscription flow"**: If the user is dismissing a form, that's Secondary, not Destructive. Red is only for actual destruction.
- **`#FFFFFF` hardcoded**: Use `Gray_50` — but even then, only inside a semantic token definition. In feature code, use `text.onBrand` / `text.onCritical` / `chat.bg.userBubble` etc.
- **Inventing Brand_550, Brand_575**: These don't exist. Pre-booking hover uses Brand_300 (placeholder). Pre-booking pressed uses Brand_500.
- **Info_600 confusion**: `#6D17CE` is Info_600 (re-anchored). Older docs called this Info_700 — those docs are stale.
- **Using `text.brand.hover` (#C00080) as a rest-state text color**: Hover tokens are only for hover states. If small brand text feels low-contrast on white, the fix is bigger type, not a darker token (see "Small brand text" below).
- **Using `icon.brand` for a Primary CTA's icon**: Brand pink on brand pink is invisible. Primary and Destructive CTAs use `icon.inverse` (near-white) for icons; see "Six most common decisions" #6.
- **Using `text.onBrand` as an `Icon(tint=)` value**: The namespace rule holds — `Icon(tint=)` always uses `icon.*` tokens, even inside CTAs. See the mapping table in actions.md.

---

## Flag a gap, don't invent

If you need a color that isn't in the system:

1. Check the decision trees in `color/core-tokens.md` and `color/actions.md` first
2. If nothing fits, flag it as a gap (PR comment: `// GAP: needed X for Y, no token exists`)
3. Don't approximate with the "closest" token — silent drift is worse than a flagged gap
4. Don't eyedrop from Figma — raw hex outside the design-system module is a build-time failure

Only `overlay.scrim` and the 40% disabled-opacity fallback are system-provided exceptions.

---

## Small brand text on white

`text.brand` (#D9008D) on `bg.default` is 4.62:1 — passes AA but marginal on budget Android at low brightness.

Safe sizes: **14sp+ regular, 12sp+ bold.** Below that, use `text.default` for body and put brand emphasis elsewhere (icon, underline). Do not substitute a darker token — brand pink is fixed at #D9008D.

---

## Disabled states

| Element | Disabled token |
|---|---|
| Container fill | `bg.disabled` (#D7D3E0 ← Neutral_300) |
| Text/label | `text.disabled` (#A7A1B2 ← Neutral_400) |
| Icon | `icon.disabled` (#D7D3E0 ← Neutral_300) |

**Fallback when no dedicated token exists** (e.g., disabled Pre-booking CTA, disabled colored badge):

- Base color from Neutral family → use the dedicated disabled token above
- Base color from non-Neutral family (Brand, Positive, Critical, Warning, Info) → apply **40% opacity** to the base color

Example: disabled Pre-booking CTA = `bg.brand.accent` at 40% opacity. Never apply opacity to Neutral-family tokens — use the dedicated tokens.

---

## Output format

- Compose: `WiomColors.bgBrand` (not `Color(0xFFD9008D)`)
- CSS: `var(--color-bg-brand)` (not `#D9008D`)
- Figma: use the named style (`bg/brand`), never a loose color fill
