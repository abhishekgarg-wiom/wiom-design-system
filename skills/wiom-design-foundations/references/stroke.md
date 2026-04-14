# CA Design System — Stroke & Border Foundation Guidelines

> Stroke widths, border position, border style, and structural rules for the CA (Wiom) Design System.
> **This skill defines thickness and structure only.** All border colors are handled by the wiom-color skill.
> Platform: **Android (Jetpack Compose)** — focus ring is accessibility-only, not primary interaction.
> Benchmarked against Material Design 3, Apple HIG, Carbon (IBM), Ant Design, Atlassian & modern SaaS (Stripe/Vercel/Linear).

---

## 1. Stroke Width Tokens

### Two widths — no exceptions

| Token | Value | Purpose |
|-------|-------|---------|
| `stroke-small` | 1px | Dividers, card outlines, input borders (rest) |
| `stroke-medium` | 2px | Outlined buttons, checkbox/radio, selected/active states, error states, focus rings (a11y), active inputs, active tabs |

### When to use what

| Width | Use Cases | Never Use For |
|-------|-----------|---------------|
| **1px** | Card outlines, input borders (rest), dividers, list separators, table borders | Buttons, checkbox/radio, selected/active/error states |
| **2px** | Outlined buttons, checkbox/radio, selected/active states, error states, active inputs, active tabs, focus rings (a11y) | Card borders, dividers |

### Industry comparison

| System | Default | Focus |
|--------|---------|-------|
| **CA (Wiom)** | **1px** | **2px** |
| Material 3 | 1dp | 3px |
| Carbon (IBM) | 1px | 2px |
| Ant Design | 1px | 3px |
| Atlassian | 1px | 2px |
| Stripe/Vercel | 1px | 2px (box-shadow) |

---

## 2. Stroke Position — Always Inside

### The rule

**All strokes are aligned INSIDE the component boundary.** No exceptions.

| Platform | Implementation |
|----------|---------------|
| Figma | Stroke align: **Inside** |
| Compose | `Modifier.border()` draws inside by default |
| CSS | `box-sizing: border-box` (default) + `border` |

### Why inside?

1. **Predictable sizing** — Component dimensions don't change when border is added/removed
2. **No layout shift** — Adding a focus ring doesn't push neighbors
3. **Clean alignment** — Adjacent bordered components align perfectly
4. **State transitions** — Toggling border on/off doesn't cause reflow

### The one exception: accessibility focus rings

Focus rings for TalkBack/keyboard render at 2px outside the component boundary. This is the only case where stroke appears "outside" — but since it's accessibility-only and not part of primary layout, it doesn't affect spacing or alignment for touch users.

---

## 3. Border Style

### The rule

**All production borders are solid.** Dashed borders are allowed only for empty states, upload zones, and in some cases as a separator.

### The three exceptions

| Exception | Style | When |
|-----------|-------|------|
| **Empty state / drop zone** | Dashed, 1px | Upload areas, drag targets, empty containers with "add" CTA |
| **Placeholder / skeleton** | Dashed, 1px | Content loading placeholders (rare — prefer shimmer instead) |
| **Separator (optional)** | Dashed, 1px | When a lighter visual separation is needed instead of a solid divider |

### Why solid only?

1. **Visual noise** — Dashed/dotted patterns create flickering texture that competes with content
2. **Rendering inconsistency** — Dash patterns render differently across browsers and devices
3. **Accessibility** — Dashed borders have lower perceived contrast than solid at the same color
4. **Brand consistency** — CA's visual language is clean and minimal; dashed borders feel unfinished

---

## 4. Dividers & Separators

### Specification

| Property | Value |
|----------|-------|
| Width | 1px (`stroke-small`) |
| Color | From wiom-color |
| Style | Solid |
| Opacity | 100% — no transparency tricks |

### Full-bleed vs Inset

| Type | Description | When to use |
|------|-------------|-------------|
| **Full-bleed** | Edge to edge of parent container | Between major sections, below app bars, above bottom nav |
| **Inset (left)** | Left-aligned with content start (16px indent) | Between list items with leading icons/avatars |
| **Inset (both)** | Indented from both sides (16px each) | Between list items without leading elements |

### Dividers inside cards — respect card padding

> **Dividers inside a card must be inset to match the card's content padding.** They never bleed to the card's edges.

A card with 16px padding has its divider indented 16px from both left and right edges — the divider aligns with the content, not the card boundary.

```
┌──────────────────────────────┐  ← Card edge
│  16px padding                │
│  ┌──────────────────────┐    │
│  │  Content row 1       │    │
│  └──────────────────────┘    │
│  ────────────────────────    │  ← Divider: inset 16px from both edges
│  ┌──────────────────────┐    │
│  │  Content row 2       │    │
│  └──────────────────────┘    │
│  16px padding                │
└──────────────────────────────┘
```

**Why:** A full-bleed divider inside a card breaks the visual containment — the line runs into the padding zone, making the card feel like a table instead of a grouped surface.

**Exception — List cards:** If a card's entire purpose is a scrollable list with no other content, the divider may go full-bleed within the card to signal density. But even then, prefer inset.

### When to use a divider vs when to skip

| Use divider | Skip divider |
|-------------|-------------|
| Between list items in flat lists | Between cards (gap + background contrast is enough) |
| Below headers/app bars | Between sections already separated by 24px+ spacing |
| Between form sections on same surface | Inside a card with clear visual grouping |
| In tables between rows | Between chips/tags in a horizontal row |

### The rule of contrast

> **If background color already creates sufficient visual separation, a divider is redundant.** Cards on a page surface don't need borders between them — the surface color gap does the work.

---

## 5. The Filled State Rule

> **When a component transitions to a filled state, the border is removed.** The fill becomes the boundary.

| Component | Unfilled (has border) | Filled (no border) |
|-----------|----------------------|-------------------|
| Checkbox | 2px border | Fill, no stroke |
| Radio | 2px border | Fill, no stroke |
| Toggle/Switch | 1px border | Fill, no stroke |
| Chip (selected) | — | Fill, no stroke |
| Button (filled) | — | Always filled, never has border |
| Tab (active) | 2px bottom accent | Exception — bottom bar, not a full border |

**Why:** A solid fill already defines the component boundary clearly. Adding a border on top of a fill is visual redundancy — it adds noise without information.

---

## 6. The Shadow vs Border Rule

> **A component uses EITHER a border OR a shadow — never both.**

| Elevation level | Visual treatment |
|----------------|-----------------|
| Flat (0dp) | 1px border |
| Low (2dp shadow) | Shadow only, no border |
| High (8dp+ shadow) | Shadow only, no border |

**Why:** Combining border + shadow creates a "double edge" effect — the border darkens the shadow edge unnaturally. Pick one depth cue.

---

## 7. Focus Ring — Android Accessibility

### When focus rings appear on Android

On Android, **touch users never see focus rings.** Touch feedback uses the Material ripple effect. Focus rings only appear for:

| Input Method | When it happens | Who uses it |
|-------------|-----------------|-------------|
| **TalkBack** | Screen reader navigates with swipe/D-pad | Visually impaired users |
| **External keyboard** | Bluetooth/USB keyboard connected | Power users, tablets |
| **Switch Access** | Accessibility switch device | Motor-impaired users |
| **D-pad / Android TV** | Remote control or directional pad | TV apps, kiosk mode |

### Focus ring spec

| Property | Value |
|----------|-------|
| Width | 2px (`stroke-medium`) |
| Offset | 2px (outside component edge) |
| Color | From wiom-color (border token for focus) |
| Corner radius | Component radius + 2px |
| Style | Solid, continuous |

### Focus ring rules

1. **Every interactive element must have a focus ring** — even though touch users won't see it
2. **Focus ring appears for TalkBack/keyboard only** — never on touch
3. **Corner radius tracks the component** — card 16px → focus ring 18px
4. **Never remove focus indicators** — WCAG 2.4.7 violation
5. **3:1 contrast minimum** against adjacent background (WCAG 2.4.11)

---

## 8. Component Stroke Map

### Which components get borders and which don't

| Component | Has Border? | Width | Notes |
|-----------|-------------|-------|-------|
| **Card (flat)** | Yes | 1px | No border if card uses elevation (shadow) |
| **Card (elevated)** | No | — | Shadow replaces border — never both |
| **Input field (rest)** | Yes | 1px | — |
| **Input field (active/focus)** | Yes | 2px | Active typing or TalkBack focus |
| **Input (error)** | Yes | 2px | Persistent, not just on focus |
| **Input (success)** | Yes | 1px | After validation passes |
| **Button (outlined)** | Yes | 2px | Interactive control — needs 2px |
| **Button (filled)** | No | — | Fill defines boundary |
| **Toggle/Switch (off)** | Yes | 1px | — |
| **Toggle/Switch (on)** | No | — | Filled state — fill IS the boundary |
| **Checkbox (unchecked)** | Yes | 2px | Small control — needs thicker border |
| **Checkbox (checked)** | No | — | Filled state — no stroke |
| **Radio (unselected)** | Yes | 2px | Small control — needs thicker border |
| **Radio (selected)** | No | — | Filled state — no stroke |
| **Chip (default)** | Yes | 1px | — |
| **Chip (selected)** | No | — | Filled state — no stroke |
| **Tab (active)** | Yes | 2px | Bottom accent bar only |
| **Tab (inactive)** | No | — | — |
| **Bottom sheet** | No | — | Shadow + surface sufficient |
| **Dialog/Modal** | No | — | Shadow + overlay sufficient |
| **Snackbar** | No | — | Elevated surface, no border |
| **Avatar** | Yes | 1px | Optional — for contrast on similar backgrounds |
| **Badge** | No | — | Fill defines boundary |
| **Progress bar** | No | — | Track uses fill, not stroke |
| **Divider** | Yes | 1px | Respects parent padding when inside cards |
| **Empty state container** | Yes | 1px dashed | Empty states, upload zones only |

---

## 9. State Transitions

### How border width changes across states

| State | Width | Notes |
|-------|-------|-------|
| **Rest** | 1px | Default |
| **Pressed** | — | Android: ripple effect, no border change |
| **TalkBack/Keyboard Focus** | 2px | Accessibility only |
| **Selected (outlined)** | 2px | All selected/active outlined components |
| **Selected (filled)** | — | Fill replaces border entirely |
| **Error** | 2px | Persistent |
| **Disabled** | 1px | Muted (40% opacity) |

### Android-specific notes

- **No hover state** — Android is touch-only (except mouse on tablets, rare)
- **Pressed = ripple** — Android uses Material ripple overlay, not border changes
- **Focus = TalkBack/keyboard only** — regular touch doesn't trigger focus ring
- **Selected states** — outlined components get 2px border; filled components drop border entirely

---

## 10. Anti-Patterns — What NOT to Do

### 1. Double borders
Combining `border` + `outline` or `border` + `box-shadow` border on the same element. **Use one method only.**

### 2. Border + Shadow on the same component
Card with both border and shadow creates a double-edge artifact. **Choose border (flat) or shadow (elevated), never both.**

### 3. Border on filled components
Adding border to a checked checkbox, toggled switch, or filled button. **Filled state = no border. The fill IS the boundary.**

### 4. Mixing border widths on same-level siblings
Card A with 1px, Card B with 2px at the same level. **All sibling components at the same level must use the same width.**

### 5. Full-bleed divider inside a card
A divider running edge-to-edge inside a card with padding. **Dividers inside cards must be inset to match card padding.**

### 6. Removing focus indicators
Disabling focus ring because "it's Android, nobody uses keyboard." **TalkBack users need it. WCAG 2.4.7 requires it.**

### 7. Using raw values instead of tokens
Hardcoding `1` or `2` in code instead of referencing `stroke-small` or `stroke-medium`. **Always use tokens.**

### 8. Border radius mismatch
Inner content 16px radius but border 12px. **Border radius must match the component's radius token exactly.**

### 9. Focus ring radius = component radius
Component 16px, focus ring also 16px. **Focus ring = component radius + offset (16 + 2 = 18px).**

---

## 11. Industry Benchmark

| Feature | CA | M3 | Apple | Carbon | Ant | Atlassian | Stripe/Vercel |
|---------|-----|-----|-------|--------|-----|-----------|---------------|
| Default 1px | Yes | Yes | 0.5pt | Yes | Yes | Yes | Yes |
| Focus 2px | Yes | 3px | OS | 2px | 3px | 2px | 2px |
| Inside position | Yes | Yes | — | Yes | Yes | Outside (focus) | Yes |
| Solid-only rule | Yes | Yes | Yes | Nearly | Nearly | Yes | Yes |
| Shadow vs border rule | Explicit | Implicit | — | Implicit | Implicit | Implicit | Implicit |
| Filled = no border | Yes | Yes | — | Yes | Yes | Yes | Yes |
| Divider respects padding | Yes | Yes | Yes | Yes | Yes | Yes | Yes |

---

## 12. Quick Reference Card

```
STROKE WIDTHS
Small: 1px (containers, dividers, rest states)
Medium: 2px (buttons, checkbox/radio, active/selected/error, focus)

POSITION
Always inside. Focus rings outside (a11y only, no layout shift).

BORDER STYLE
Solid for all production UI.
Dashed for empty states, upload zones, and optional separators.

DIVIDERS
1px, solid. Full-bleed on screen level. Inset inside cards.

FILLED STATE RULE
Filled = no border. Checkbox checked, radio selected, toggle on,
chip selected, filled button — fill IS the boundary.

SHADOW vs BORDER
Flat → border. Elevated → shadow. Never both.

FOCUS RING (Android — accessibility only)
2px width, 2px offset, radius = component radius + 2px.
Only for: TalkBack, external keyboard, Switch Access, D-pad.

STATE ORDER
Rest (1px) → Focus (2px) → Selected/Active (2px) → Filled (no border) → Error (2px)

COLORS
All border colors handled by wiom-color skill. This skill defines thickness only.
```

---

*Generated April 2026 — Benchmarked against Material 3, Apple HIG, Carbon (IBM), Ant Design, Atlassian, Stripe, Vercel & Linear.*
*Platform: Android (Compose). Colors: wiom-color skill. See wiom-stroke-removals.md for change log.*
