# CA Design System — Spacing Foundation Guidelines

> Spacing rules, tokens, and best practices for the CA (Wiom) Design System.
> Benchmarked against Material Design 3, Apple HIG, Carbon (IBM), Ant Design & Atlassian.

---

## 1. The 4px Grid Rule

Every spacing, sizing, padding, and margin value must be a **multiple of 4px**.

### Why 4px?

- Divides evenly into all common screen widths (360, 375, 390, 412, 768, 1024, 1440, 1920)
- Scales cleanly at all display densities — 1x, 1.5x, 2x, 3x (no sub-pixel rendering)
- Fine enough for component internals, coarse enough to prevent pixel-level debates
- Industry standard — used by Google Material, Ant Design, and adopted by most modern systems

### What's exempt from the 4px grid?

| Exempt | Reason |
|--------|--------|
| Font sizes (10px, 14px) | Industry convention — all major systems use these |
| Stroke widths (1px, 2px) | Serve visual weight, not layout |
| Sub-grid primitives (1px, 2px) | Utility-only — for hairlines and optical adjustments |

---

## 2. Spacing Tokens

### Primitives (14 values)

| Token | Value | On 4px Grid | Usage |
|-------|-------|-------------|-------|
| `space/1` | 1px | No (utility) | Hairline borders, pixel-precise adjustments |
| `space/2` | 2px | No (utility) | Tight icon gaps, fine borders |
| `space/4` | 4px | Yes | Minimum spacing between elements |
| `space/8` | 8px | Yes | Tight padding, inline gaps |
| `space/12` | 12px | Yes | Component internal spacing |
| `space/16` | 16px | Yes | Default padding, card insets |
| `space/20` | 20px | Yes | Progress Milestones gaps (D42) |
| `space/24` | 24px | Yes | Section spacing, comfortable gaps |
| `space/32` | 32px | Yes | Large component gaps |
| `space/40` | 40px | Yes | Section breaks |
| `space/48` | 48px | Yes | Touch target height, page margins |
| `space/64` | 64px | Yes | Large layout spacing |
| `space/72` | 72px | Yes | Hero/section padding |
| `space/84` | 84px | Yes | Extra-large layout areas |

### Semantic Tokens (12 values)

These alias the primitives and are **identical in Light and Dark modes**.

`space-4` `space-8` `space-12` `space-16` `space-20` `space-24` `space-32` `space-40` `space-48` `space-64` `space-72` `space-84`

> `space/1` and `space/2` intentionally have no semantic aliases — they're utility-only, not for general layout.

---

## 3. When to Use What

| Spacing | When to Use |
|---------|-------------|
| **4px** | Chip icon → label, hairline adjustments, very tight inline elements |
| **8px** | Inline label pairings (icon + label, checkbox + text, status icon + label), related items in a group, input field internal padding, chip padding |
| **12px** | Between list items, component internal padding, between label and input |
| **16px** | Card padding, screen horizontal margins, between heading and body text |
| **24px** | Between sections within a card, dialog padding, comfortable list gaps |
| **32px** | Between major content blocks, between sections |
| **40px** | Section breaks on a page |
| **48px** | Touch target minimum height, page-level vertical margins |
| **64px** | Large layout gaps, hero section padding |
| **72–84px** | Hero content areas, full-width section padding |

---

## 4. Spacing Hierarchy Principle

> **Related = Tight, Unrelated = Loose.** Spacing communicates grouping.

### Three Levels

| Level | Spacing Range | What it groups | Examples |
|-------|--------------|----------------|----------|
| **Tight** | 8–12px | Elements that belong together as one unit | Icon + label, title + subtitle, checkbox + text |
| **Medium** | 16–24px | Elements within the same section | List items, card internals, input + label |
| **Loose** | 32–40px+ | Separate sections or content blocks | Card to card, section to section, content → CTA |

### The Decision Test

If you're unsure which token to use, ask: **"If I removed everything else on screen, would these two elements still make sense together?"**

- **Yes** → Tight (8–12px) — they're a unit
- **Kind of** → Medium (16–24px) — same context, different roles
- **No** → Loose (32–40px) — separate concerns

### How It Looks in Practice

```
[Icon 8px Label]              ← TIGHT: one unit
       12px
[Subtitle text]               ← TIGHT: still same header group
           24px
[Info row 1]                   ← MEDIUM: same section
      16px
[Info row 2]                   ← MEDIUM: same section
      16px
[Info row 3]                   ← MEDIUM: same section
           32px
[CTA Button]                   ← LOOSE: separate action zone
```

### Wrong vs Right

**Wrong — same spacing everywhere:**
Title→subtitle 16px, info rows 16px, info→CTA 4px. Everything feels flat, no visual grouping. Eye can't distinguish sections.

**Right — hierarchy-driven:**
Title→subtitle 8px (tight unit), info rows 16px (same section), info→CTA 32px (different concern). Eye reads the grouping instantly.

---

## 5. Margin Direction Convention: Gap-Only

> **Parents own the spacing between children. Children own their internal spacing. No margins — ever.**

### The Rule

| Responsibility | Controlled by | Property |
|---------------|--------------|----------|
| Space between siblings | Parent container | `gap` / `itemSpacing` |
| Space inside a component | The component itself | `padding` |
| Space between a component and its neighbors | Never the component | Handled by parent's gap |

### How It Maps Across Platforms

| Platform | Gap property |
|----------|-------------|
| Figma | `itemSpacing` (auto-layout) |
| Compose | `Arrangement.spacedBy()` |
| Flutter | `spacing` / `SizedBox` in Column/Row |
| CSS | `gap` in flex/grid |

### Example

```
Screen (padding: 16px)
  └── Content List (gap: 16px)
        ├── Card A (padding: 16px, no margin)
        │     └── internal (gap: 8px)
        │           ├── Title
        │           └── Subtitle
        ├── Card B (padding: 16px, no margin)
        └── Card C (padding: 16px, no margin)
```

- Cards don't know how far apart they are — the Content List decides (16px gap)
- Title and Subtitle don't know their gap — the card's internal layout decides (8px gap)
- Removing Card B leaves 16px between A and C automatically — no orphan spacing

### Why No Margins

| Problem with margins | Gap-only solution |
|---------------------|-------------------|
| Bottom padding + top padding = double spacing | Gap is set once on parent |
| Removing a child leaves orphan margin | Gap adjusts automatically |
| Last-child needs margin:0 exception | No exceptions needed |
| Spacing logic split across parent and child | Single source of truth |

---

## 6. Touch Targets

### Three-Tier System

| Tier | Target Size | Token | Use Case | Examples |
|------|-------------|-------|----------|----------|
| **Action** | 48 × 48 px | `space/48` | High-consequence interactive icons | Delete, edit, share, close, menu, back |
| **Navigation** | 32 × 32 px | `space/32` | Medium-priority frequent-use icons | Expand/collapse, sort, arrows, pagination, filters |
| **Info** | 24 × 24 px | `space/24` | Low-stakes informational icons | Help, tooltip triggers, status indicators, info |

### Buttons

Buttons have **no explicit touch target rule** — padding already handles it. With `space-12` vertical padding + 16px text (line-height 24px), the smallest button is 48px tall by default.

### Implementation

A 16px icon with a 48px touch target needs 16px of transparent padding per side. Use hit-area extension, not visible padding.

### Industry Compliance

| Standard | Minimum | Our Coverage |
|----------|---------|-------------|
| Material Design 3 | 48dp | Action tier meets it |
| Apple HIG | 44pt | Action tier exceeds it |
| WCAG 2.2 (AA) | 24px | All three tiers meet it |
| WCAG 2.1 (AAA) | 44px | Action tier exceeds it |

---

## 7. Screen-Level Layout Spacing (Android)

### Screen Size

All screens are designed at **360 × 720px** — fixed, no exceptions.

### Fixed Heights

| Element | Height | Token | Notes |
|---------|--------|-------|-------|
| Status bar | 24px | `space-24` | System UI, affects top offset |
| Bottom safe area (gesture nav) | 16px | `space-16` | Added as extra bottom padding on the last component — never a separate frame |

### Screen Horizontal Margins

| Context | Margin | Token |
|---------|--------|-------|
| All screens (default) | 16px | `space-16` |
| Chat screens | 12px | `space-12` |

### Vertical Section Spacing

| Between | Spacing | Token |
|---------|---------|-------|
| App bar → first content | 16px | `space-16` |
| Content sections | 32px | `space-32` |
| Between cards | 16px | `space-16` |
| Last content → bottom nav | 16px | `space-16` |
| FAB above bottom nav | 16px | `space-16` |
| Content bottom → sticky element (CTA bar/keyboard) | 16px | `space-16` |

---

## 8. Component Spacing Recipes

> Basic spacing only. Detailed component specs will live in separate skill files.

### Cards

| Property | Token | Value |
|----------|-------|-------|
| Outer padding | `space-16` | 16px all sides — always, no exceptions |
| Nested card padding (left/right) | `space-0` | 0px — parent card already provides horizontal margin |
| Between sections inside card | `space-16` | 16px |
| Title → subtitle | `space-8` | 8px |
| Title → body text | `space-12` | 12px |
| Icon → text (inline) | `space-12` | 12px |
| Content → CTA | `space-32` | 32px |

### List Items

| Property | Token | Value |
|----------|-------|-------|
| Horizontal padding | `space-16` | 16px |
| Vertical padding | `space-12` | 12px |
| Between list items | `space-8` | 8px |
| Icon/avatar → text | `space-12` | 12px |
| Primary text → secondary text | `space-4` | 4px |
| Text → trailing action | `space-12` | 12px |

### Input Fields

| Property | Token | Value |
|----------|-------|-------|
| Internal padding (horizontal) | `space-16` | 16px |
| Internal padding (vertical) | `space-12` | 12px |
| Label → input | `space-8` | 8px |
| Input → helper/error text | `space-4` | 4px |
| Between stacked inputs | `space-16` | 16px |

### Bottom Sheets / Modals

| Property | Token | Value |
|----------|-------|-------|
| Padding (all sides) | `space-16` | 16px |
| Title → content | `space-16` | 16px |
| Content → action buttons | `space-24` | 24px |
| Between action buttons | `space-12` | 12px |

### Snackbar / Toast

| Property | Token | Value |
|----------|-------|-------|
| Internal padding (horizontal) | `space-16` | 16px |
| Internal padding (vertical) | `space-12` | 12px |
| Text → action button | `space-12` | 12px |
| Bottom offset from screen edge | `space-16` | 16px |

### Chips / Tags

| Property | Token | Value |
|----------|-------|-------|
| Internal padding (horizontal) | `space-8` | 8px |
| Internal padding (vertical) | `space-4` | 4px |
| Icon → label | `space-4` | 4px |
| Between chips | `space-8` | 8px |

---

## 9. Anti-Patterns — What NOT to Do

### 1. Arbitrary pixel values
Using 13px, 17px, 23px — values not in the scale. **Always use tokens.** This includes computed widths — if dividing available space produces a non-grid value (e.g. 73px from `328/4`), round down to the nearest 4px multiple (72px) and accept the leftover space.

### 2. Inconsistent same-level padding
Card A = 16px, Card B = 20px, Card C = 24px at the same hierarchy. **Pick one token per hierarchy level.**

### 3. Ignoring line-height in text spacing
"8px gap" between heading and body — but the heading's line-height adds 4px of built-in bottom space. Visual gap = 12px, not 8px.

### 4. Spacing by eye per screen
Hand-picking spacing without tokens creates unmaintainable designs that drift. **Always reference the token scale.**

### 5. Fixed spacing without responsive
64px margins on desktop without reducing for mobile. Use smaller tokens at smaller breakpoints.

### 6. 8px everywhere
The scale exists for a reason — hierarchy requires varied spacing. Dense elements need 4px; sections need 32px+.

---

## 10. Industry Benchmark — Where We Stand

### Spacing Scale Coverage

| px | CA | Material 3 | Apple | Carbon | Ant | Atlassian |
|----|-----|-----------|-------|--------|-----|-----------|
| 4 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 8 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 12 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 16 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 24 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 32 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| 40 | ✓ | — | ✓ | ✓ | — | ✓ |
| 48 | ✓ | ✓ | — | ✓ | ✓ | ✓ |
| 64 | ✓ | ✓ | ✓ | ✓ | — | ✓ |

**Core set (4, 8, 12, 16, 24, 32, 48, 64)** matches all five systems.

### Grid Base

| System | Base Grid | Touch Target |
|--------|-----------|-------------|
| **CA (Wiom)** | **4px** | **48px** |
| Material 3 | 4px | 48dp |
| Apple HIG | ~8pt | 44pt |
| Carbon (IBM) | 8px | 48px |
| Ant Design | 4px | 32px min |
| Atlassian | 8px | 32px min |

---

## 11. Quick Reference Card

```
SPACING SCALE
4  8  12  16  24  32  40  48  64  72  84

HIERARCHY
Tight (8–12)  Medium (16–24)  Loose (32–40+)

TOUCH TARGETS
Action: 48px  Navigation: 32px  Info: 24px

SCREEN MARGINS
Default: 16px  Chat: 12px

SPACING OWNERSHIP
Parents → gap between children
Children → padding inside themselves
Margins → never

THE RULE
Every layout value must be divisible by 4.
If it's not in the token list, don't use it.
```

---

*Generated April 2026 — Benchmarked against Material 3, Apple HIG, Carbon (IBM), Ant Design & Atlassian.*
