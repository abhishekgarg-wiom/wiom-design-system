# Wiom Design System — Shadow Token Skill

You are generating a design or UI for the Wiom product ecosystem. You MUST use ONLY the approved Wiom shadow tokens listed below. Do NOT invent shadows, use raw `box-shadow` values outside this list, or approximate custom elevations. Every shadow in your output must map to exactly one token from this system.

## Context

Wiom serves lower- and middle-income Indian households. Shadow decisions must account for:
- **Mobile-first** — budget Android devices (720p, low contrast ratios ~800-1200:1)
- **Outdoor use** — shadows can wash out in direct sunlight; pair with borders on critical surfaces
- **Performance** — minimize GPU compositing cost on budget devices
- **Trust-heavy** — transactional flows where elevation hierarchy reinforces action gravity

---

## Architecture: 5 Core Shadow Tokens

```
ONE CATEGORY: CORE SHADOWS — Standard elevation for UI surface hierarchy.
NO GLOWS:     Glow effect tokens were removed (Decision 016) — no product usage.
ONE FORMULA:  offset-y = dp, blur = dp × 3, spread = 0, opacity = 15%.
ONE SCALE:    1, 2, 4, 8 (power-of-2 progression).
```

**Rule:** Every shadow in your design must be one of the 5 tokens below. If you need emphasis, use the color token system — not a shadow effect.

---

## Complete Token Reference — 5 Shadow Tokens

| # | Token | CSS value | CSS custom property | Role |
|---|-------|-----------|---------------------|------|
| 1 | `shadow.none` | `none` | `var(--shadow-none)` | Flat baseline — no elevation |
| 2 | `shadow.sm` | `0 1px 3px 0 rgba(0,0,0,0.15)` | `var(--shadow-sm)` | Subtle lift |
| 3 | `shadow.md` | `0 2px 6px 0 rgba(0,0,0,0.15)` | `var(--shadow-md)` | Default elevation |
| 4 | `shadow.lg` | `0 4px 12px 0 rgba(0,0,0,0.15)` | `var(--shadow-lg)` | Strong elevation |
| 5 | `shadow.xl` | `0 8px 24px 0 rgba(0,0,0,0.15)` | `var(--shadow-xl)` | Highest elevation (modals) |

### Use for / Do NOT use for

| Token | Use for | Do NOT use for |
|-------|---------|----------------|
| `shadow.none` | Buttons, inputs, checkboxes, radios, chips, banners, steppers — any surface intentionally flat. Also for resetting elevation (e.g., card embedded inside a bottom sheet). | — |
| `shadow.sm` | Sticky nav (scrolled state only), tooltips, passive info cards — surfaces that need slight lift without dominating | Default card elevation, dropdowns — too subtle for interactive surfaces |
| `shadow.md` | Interactive cards, bottom sheets, dropdown menus, popovers, FAB — the everyday default shadow | Every surface — not all surfaces need elevation. Elevation is intentional. |
| `shadow.lg` | Floating toasts/snackbars, dropdown over visually busy backgrounds — surfaces that need to clearly "pop" | Common default — use on max 1–2 elements per screen |
| `shadow.xl` | Modal dialogs, full-screen bottom sheets — always paired with `color.overlay.default` backdrop | Regular cards, dropdowns — reserved for full-attention overlays only |

---

## Decision Tree — Which Shadow?

Start from the top. The first match is your shadow.

```
Is the surface intentionally flat (button, input, chip, banner, checkbox, radio, stepper, nav bar)?
  → shadow.none

Is the surface a tooltip, passive info card, or sticky nav on scroll?
  → shadow.sm

Is the surface an interactive card, bottom sheet, dropdown, or popover?
  → shadow.md

Is the surface a floating toast or dropdown over a busy background?
  → shadow.lg

Is the surface a modal dialog or full-screen overlay?
  → shadow.xl

None of the above? Default to shadow.md.
```

---

## Component-to-Shadow Mapping

This is the source of truth. Every component has exactly one shadow assignment.

### shadow.none — Flat surfaces

| Component | Why flat |
|-----------|---------|
| Button (all variants) | Inline interactive element, not an elevated surface |
| Input Field | Inline form control, uses stroke for boundary |
| Checkbox | Inline form control |
| Radio | Inline form control |
| Chip / Tag | Inline label element |
| Banner / Toast (inline) | Inline notification, uses color/border for distinction |
| Stepper | Inline navigation indicator |
| Navigation bar | Fixed position, uses border-bottom for separation |

### shadow.sm — Subtle lift

| Component | Why subtle |
|-----------|-----------|
| Sticky navigation (scrolled) | Subtle separation from content scrolling behind. Only when scrolled — at rest, nav is flat (shadow.none). |
| Tooltip | Small floating element, slight lift without dominating |
| Card (passive, non-interactive) | Information-only display. Example: plan detail card in a list. |

### shadow.md — Default elevation

| Component | Why default |
|-----------|------------|
| Card (interactive, primary) | Main content cards: recharge amount, plan comparison, payment summary |
| Bottom sheet | Standard sliding surface, clear separation from page content |
| Dropdown menu (Select) | Floating menu over content |
| Floating action button (FAB) | Persistent floating element, needs clear elevation |
| Popover | Contextual floating surface |

### shadow.lg — Strong elevation

| Component | Why strong |
|-----------|-----------|
| Toast / Snackbar (floating) | Floating notification that must read against arbitrary page content |
| Dropdown over complex content | When a dropdown appears over a visually busy background (dense form, complex table), stronger shadow aids separation |

**Limit:** Max 1–2 elements with `shadow.lg` per screen. If many surfaces use strong elevation, the hierarchy flattens.

### shadow.xl — Highest elevation

| Component | Why highest |
|-----------|------------|
| Modal dialog | Full-attention overlay. Trust-heavy flows: payment confirmation, refund authorization, plan commitment. Always with `color.overlay.default` backdrop. |
| Full-screen bottom sheet (expanded) | Behaves like a modal when expanded to near-full-screen |

**Rule:** `shadow.xl` is ALWAYS paired with `color.overlay.default` (rgba(22,16,33,0.50)) backdrop.

---

## Shadow + Border Coexistence

Shadow and stroke can coexist on the same surface. This is critical for Wiom's audience:

- **Good conditions (indoor, flagship phone):** Shadow provides clear depth; border may be visually redundant.
- **Challenging conditions (outdoor, budget phone):** Shadow may wash out at 15% opacity; border provides structural boundary.

**Rule:** On critical interactive surfaces (payment cards, plan selection), pair `shadow.md` with `stroke.1` + `color.border.default` for maximum clarity across conditions.

---

## Figma Effect Styles

| Token | Figma effect style name |
|-------|------------------------|
| `shadow.none` | No Figma style (Figma has no "no effect" style). Remove all effects. |
| `shadow.sm` | `shadow/sm` |
| `shadow.md` | `shadow/md` |
| `shadow.lg` | `shadow/lg` |
| `shadow.xl` | `shadow/xl` |

---

## Strict Rules

### DO
- Use ONLY the 5 shadow tokens from this list — never raw box-shadow values
- Default to `shadow.md` when adding elevation and unsure which level
- Use `shadow.none` explicitly when a component is intentionally flat
- Reserve `shadow.xl` for modal dialogs and full-screen overlays only
- Always pair `shadow.xl` with `color.overlay.default` backdrop
- Pair shadow with border on critical surfaces for budget device robustness
- Limit `shadow.lg` to 1–2 elements per screen
- Use CSS custom properties: `var(--shadow-sm)`, `var(--shadow-md)`, etc.

### DO NOT
- Do NOT invent intermediate shadow values (e.g., halfway between sm and md)
- Do NOT use negative/upward shadows — they are permanently removed from the system
- Do NOT reference old dp names (`1dp`, `2dp`, `4dp`, `-2dp`, `-4dp`)
- Do NOT add shadow to components that should be flat (buttons, inputs, checkboxes, radios)
- Do NOT use opacity tricks to create unnamed shadow variants
- Do NOT use colored glow shadows — glows were removed (Decision 016), use color tokens for emphasis
- Do NOT use `shadow.lg` or `shadow.xl` as the default for cards — use `shadow.md`
- Do NOT apply shadow to every surface — flat is valid, elevation is intentional

### WHEN IN DOUBT
If no token fits your use case, default to `shadow.md`. If that doesn't work either, flag it as a gap — do not invent a shadow.

---

## Quick CSS Lookup

```css
--shadow-none: none;
--shadow-sm:   0 1px 3px 0 rgba(0,0,0,0.15);
--shadow-md:   0 2px 6px 0 rgba(0,0,0,0.15);
--shadow-lg:   0 4px 12px 0 rgba(0,0,0,0.15);
--shadow-xl:   0 8px 24px 0 rgba(0,0,0,0.15);
```

---

Apply this shadow system to the user's design request. Use token names in your output (not just CSS values) so the design is traceable back to this system. If generating code, use the CSS custom property format: `var(--shadow-md)`, `var(--shadow-xl)`, etc.
