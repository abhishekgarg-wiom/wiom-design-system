---
name: wiom-icon-badge
description: >
  Wiom Icon Badge — a single glyph inside a filled circular container, used as a
  leading affordance in list rows, empty-state heros, settings entries, status
  summaries, and inline information hits. Three sizes, six tones. Distinct from
  wiom-badge (which is a count / dot / label chip). Use this skill EVERY TIME you
  render an icon inside a colored circular background for the Wiom Partner or
  Customer app. Trigger on icon-with-bg, icon container, settings icon, list
  leading icon, avatar-ish icon, status icon bubble, empty-state icon, or any
  compose call that wraps an Icon inside a Box with a background.
---

# Wiom Icon Badge (icon-with-bg)

A glyph inside a filled, circular container. In Figma this is the
`icon_with_bg` component on the Selection Controls page (`node-id=3069-5843`);
in code it is the `WiomIconBadge` composable used everywhere a list row, empty
state, settings entry, or status summary needs an icon with visual weight.

**Not the same as `wiom-badge`.** Badge is a count chip, dot indicator, or
status label (numeric or text-bearing). Icon Badge is a glyph-in-container
affordance (visual only, no text). Different anatomy, different purpose.

**Platform:** Android (Jetpack Compose).

---

## Relationship to foundations

The **sizes × tones matrix is authoritative in `wiom-design-foundations` →
Component Quick-Build Reference → Icon Badge**. Do not re-invent it here or
in calling code — cite the foundations entry.

Quick recap for trigger-level use:

| Size | Container | Glyph | Padding |
|---|---|---|---|
| `sm` | 24 × 24 dp | `icon.xs` (16 dp) | `space-4` |
| `md` | 48 × 48 dp | `icon.md` (24 dp) | `space-12` |
| `lg` | 96 × 96 dp | `icon.lg` (48 dp) | `space-24` |

Tones: `neutral`, `brand`, `positive`, `warning`, `critical`, `info`. Pair
container `bg.*.subtle` with glyph `icon.*` from the same family —
don't mix tones.

---

## Decision tree

```
Rendering a text-free, circular container around a single glyph?
  → wiom-icon-badge

Showing a count (1, 2, 99+), a dot, or a label text chip?
  → wiom-badge (NOT this skill)

A full avatar (user photo, initials, or logo)?
  → wiom-avatar (separate — not this skill)

An emoji / flag / illustration (multi-color, detailed)?
  → use an illustration drawable directly, not icon-badge

A square thumbnail?
  → use radius-small or radius-medium container, not icon-badge (which is always full-round)
```

---

## Which size?

```
Inside a list-item leading slot, compact row                 → sm (24dp)
Inside a list-item leading slot, comfortable row             → md (48dp)
Settings entry / category row with prominent icon            → md (48dp)
Empty-state hero illustration (icon, not drawing)            → lg (96dp)
Onboarding step illustration                                 → lg (96dp)
Status summary card (e.g., "Plan Active" with check-badge)   → md (48dp)
Inline info badge inside a chat bubble or toast              → sm (24dp)
```

## Which tone?

```
Default / decorative / neutral info                          → neutral
Featured, promoted, brand-flagged                            → brand
Success confirmation, completed step                         → positive
Warning / attention required / expiring soon                 → warning
Error / failed / critical alert                              → critical
Informational tip / help / neutral system message            → info
```

---

## Anatomy rules

- **Always full-round.** `radius-full` on the container, in every size, every
  tone. No exceptions. If the design asks for a rounded-square glyph container,
  that's a different component — flag it.
- **Always filled.** No border, no shadow. Foundations "shadow vs border"
  rule — the fill is the only visual weight.
- **Single glyph only.** The inside is always one `Icons.Rounded.*` (or one
  `painterResource(R.drawable.ic_wiom_*)` / `ic_partner_*`). Never two glyphs,
  never text, never a count overlay.
- **Display, not tap target.** The row / CTA / parent owns the tap. Icon Badge
  sm (24 dp) in particular does not satisfy a touch target — do not make the
  badge itself clickable.
- **`sm` glyph at 16 dp uses `icon.xs`** — the restricted size. Valid here
  because the badge container provides the padded hit zone when embedded in a
  larger tappable parent.

---

## Wiom use cases

**List rows** (primary use). The leading slot of a `wiom-list-item` when that
row's icon deserves visual weight beyond a bare `Icons.Rounded.*`. Settings,
account sections, service listings, payment-method rows.

**Empty states.** The hero graphic at `lg` (96 dp) when an illustration is
overkill — "No transactions yet" with a wallet icon-badge, "No partners
found" with a magnifier icon-badge.

**Status summary cards.** "Plan Active" card with a positive-tone check
icon-badge at `md`. "Action required" with warning-tone.

**Category chips in onboarding / feature lists.** At `md`, tone varies by
category.

**Inline within other components.** A critical-tone icon-badge at `sm` next
to a toast title. A positive-tone icon-badge at `sm` inside a completion
meter label.

---

## Figma → Compose mapping

Figma component: `icon_with_bg` (frame `3063:10031` inside the Selection
Controls page).

| Figma property | Compose parameter |
|---|---|
| `Property 1` (`24 x 24` / `48 x 48` / `96 x 96`) | `size: IconBadgeSize` → `Sm` / `Md` / `Lg` |
| Instance-swap glyph | `icon: ImageVector` or `icon: Painter` |
| Fill tone (neutral by default in Figma) | `tone: IconBadgeTone` → `Neutral` / `Brand` / `Positive` / `Warning` / `Critical` / `Info` |

Figma ships only the neutral tone today. The other five tones are synthesized
from foundations tokens — do not wait for Figma variants before using them in
code.

---

## Minimal Compose shape

```kotlin
@Composable
fun WiomIconBadge(
    icon: ImageVector,               // or accept a painter variant for ic_wiom_* drawables
    size: IconBadgeSize = IconBadgeSize.Md,
    tone: IconBadgeTone = IconBadgeTone.Neutral,
    modifier: Modifier = Modifier,   // no explicit width/height — size enum decides
    contentDescription: String? = null,
)

enum class IconBadgeSize { Sm, Md, Lg }
enum class IconBadgeTone { Neutral, Brand, Positive, Warning, Critical, Info }
```

- **No tap handler.** Icon Badge is display-only. Tap belongs to the parent
  row / card / CTA.
- **`modifier` accepts alignment + parent-driven padding only.** No
  `.size(...)`, no `.width(...)`, no `.height(...)` — size comes from the
  enum.
- **Glyph color derives from tone**, not a parameter. One tone = one
  `bg.*.subtle` + one `icon.*` pair, always.

---

## Anti-pattern checklist

- [ ] Rounded-square container (use radius-small / rounded-rect helper instead,
      not this component)
- [ ] Border or shadow on the container (filled only; no border, no shadow)
- [ ] Mixing tones (e.g., `bg.brand.subtle` with `icon.positive`) — always pair
      container bg and glyph tint from the same family
- [ ] Multi-color illustration instead of a single `Icons.Rounded.*` /
      `ic_wiom_*` glyph (use an illustration drawable, not this component)
- [ ] `Modifier.size(...)` / `.width(...)` / `.height(...)` on the badge
      (size comes from the `size` enum)
- [ ] Tap handler on the badge itself (tap belongs to the parent)
- [ ] Text inside the container (use `wiom-badge` for count / label chips)
- [ ] `icon.warning` on `bg.default` container (invalid — `icon.warning` is
      only legible inside warning-tinted bg; foundations rule)
- [ ] `sm` badge used as a standalone tap target without a parent tap parent

---

## See also

- `wiom-design-foundations` → Component Quick-Build Reference → **Icon Badge**
  (the canonical tokens + matrix)
- `wiom-badge` — count, dot, and label chips (different component)
- `wiom-list-item` — the primary host for sm and md icon-badges
- `wiom-compose-layout-rules` — icons come from `Icons.Rounded.*` or
  `painterResource(R.drawable.ic_wiom_*)` only
