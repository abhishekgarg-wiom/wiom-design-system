---
name: wiom-visual-skill
description: >
  The complete visual design system skill for the Wiom CSP Partner App. Governs ALL
  visual decisions across 4 domains:

  DOMAIN A — Visual Hierarchy: heading→body type progression, section vs. element
  spacing, action prominence, and information density.

  DOMAIN B — Screen States: empty, loading, error, and offline state handling.

  DOMAIN C — Accessibility: color contrast, touch targets, TalkBack, focus navigation,
  text scaling, and motion preferences.

  DOMAIN D — Forms: field anatomy, input types, validation, multi-step forms, selection
  patterns, form layout, and keyboard interaction.

  Derived from Universal Principles of Design (Lidwell, Holden, Butler, 2003). Every
  rule traces to a named principle. All values come from WiomTokens.kt.

  Trigger when: type sizes look wrong, sections feel cramped, CTAs don't pop, screen
  too dense or too empty, blank screen during API call, English error messages, toast
  disappears before user reads, contrast too low, touch target too small, TalkBack
  reads disconnected text, form validation unclear, keyboard covers button, or any
  screen design review.

  Depends on: wiom-android-layout (SKILL 01) for token values.
---

# Wiom Visual Skill

**Stack:** Android · Jetpack Compose · Material 3
**Target:** Budget Android (2-3GB RAM, 5-inch, 360x720px viewport)
**Language:** Hindi-first. Noto Sans Devanagari. Arabic numerals always.
**Law:** Every value comes from  (aliased as ). Zero raw numbers.

---

## Skill Map

| Domain | Areas | Section |
|---|---|---|
| **A — Visual Hierarchy** | Type scale, spacing, CTA prominence, density | Sections 01-04 |
| **B — Screen States** | Empty, loading, error, offline | Sections 05-09 |
| **C — Accessibility** | Contrast, touch targets, TalkBack, focus, scaling, motion | Sections 10-15 |
| **D — Forms** | Field anatomy, input types, validation, multi-step, selection, layout, keyboard | Sections 16-22 |

---

# DOMAIN A — Visual Hierarchy

## Four areas at a glance

| # | Area | Governs | Read when |
|---|---|---|---|
| 01 | [Heading→Body Progression](#01-headingbody-progression) | Type scale, weight, color across hierarchy levels | "Everything looks the same size", type feels flat |
| 02 | [Section > Element Spacing](#02-section--element-spacing) | Gaps between groups vs. gaps within groups | "Sections bleed together", "too cramped", "too floaty" |
| 03 | [Action Prominence](#03-action-prominence) | CTA visibility, placement, visual weight | "Button doesn't stand out", "I missed the action" |
| 04 | [Density](#04-density) | Information per viewport, breathing room, scan speed | "Too much on screen", "screen feels empty", "can't scan" |

---

## Source principles

Every rule in this skill traces to a named principle from *Universal Principles of Design*
(Lidwell, Holden, Butler, Rockport 2003). The book is the authority. The principle name
is cited inline as `[Book: Principle Name, pXXX]`.

| Principle | Book page | Role in this skill |
|---|---|---|
| Hierarchy | p104 | "The simplest structure for visualizing complexity." Type scale IS the hierarchy. |
| Inverted Pyramid | p116 | "Information in descending order of importance." Headlines first, details last. |
| Entry Point | p64 | "A point of physical or attentional entry into a design." The largest type IS the entry point. |
| Legibility | p124 | "Visual clarity of text — size, typeface, contrast, spacing." Minimum sizes for budget screens. |
| Readability | p162 | "Degree to which prose can be understood." Fewer words, shorter lines, simpler structure. |
| Proximity | p160 | "Elements close together are perceived as more related." Spacing = semantic grouping. |
| Similarity | p184 | "Similar elements are perceived as a single group." Same type style = same hierarchy level. |
| Uniform Connectedness | p200 | "Elements connected by uniform visual properties are perceived as related." Cards, stripes, fills. |
| Chunking | p30 | "Working memory holds 7±2 items." Group into visual chunks. |
| Figure-Ground | p80 | "Elements are perceived as either figures or ground." Cards vs. screen, CTA vs. card. |
| Gutenberg Diagram | p100 | "Eyes follow a Z-path: top-left → top-right → bottom-left → bottom-right." CTA in terminal area. |
| Highlighting | p108 | "Highlight no more than 10% of the visible design." One accent, one focus card, one hero number. |
| Signal-to-Noise | p182 | "Highest possible signal-to-noise ratio is desirable." Every pixel must carry meaning. |
| Progressive Disclosure | p154 | "Only necessary information displayed at any given time." Collapse secondary info. |
| Visibility | p202 | "Usability improves when status and methods are clearly visible." Actions must be seen. |
| Layering | p122 | "Organizing information into related groupings." Foreground/midground/background visual layers. |

---

## 01 Heading→Body Progression

> **[Book: Hierarchy, p104]** "The simplest structure for visualizing and understanding
> complexity." In a mobile screen, typographic hierarchy IS the information hierarchy.
> If the user cannot tell what is a heading and what is a body, the screen has no structure.

> **[Book: Inverted Pyramid, p116]** "Information presented in descending order of importance,
> with critical information presented first." The type scale must descend in visual weight
> from the most important element to the least.

### The type scale

The Wiom type scale has exactly **5 levels**. Each level has a fixed size, weight, and color.
No screen uses more than 4 levels simultaneously. Using all 5 on one screen violates
**[Book: Signal-to-Noise, p182]** — too many levels creates visual noise.

| Level | Name | Size | Weight | Color | Use | Example |
|---|---|---|---|---|---|---|
| L1 | Hero number | 20–24sp | Bold (700) | `T.State.caution` or `T.Text.primary` | The ONE number that matters most | ₹240, ₹340/दिन |
| L2 | Section title | 16sp | Bold (700) | `T.Text.primary` | Screen title, card group heading | 1000 NetBox, NB#201 |
| L3 | Card identity | 14sp | SemiBold (600) | `T.Text.primary` | Device ID, row label, CTA label | NB#201, कैरी शुल्क |
| L4 | Body | 13–14sp | Regular (400) | `T.Text.secondary` | Supporting text, descriptions | भेज दिया गया — पुष्टि करें |
| L5 | Caption | 11–12sp | Regular (400) | `T.Text.secondary` or state color | Metadata, badges, timestamps | खाली, 12 दिन, अब तक |

### Progression rules

> **[Book: Entry Point, p64]** "The initial impression of a system greatly influences
> subsequent perceptions." The largest, boldest element on screen is where the eye enters.
> If that element is the wrong thing, the entire screen fails.

**Rule 1: Each level must be visually distinguishable from the next.**

Minimum differentiation between adjacent levels:
- Size jump: ≥2sp between levels
- Weight jump: at least one weight step (Regular→SemiBold, SemiBold→Bold)
- Color jump: if size and weight are similar, color MUST differ

If two elements look the same, the user reads them as the same hierarchy level —
**[Book: Similarity, p184]** "Similar elements are perceived as a single group."

**Rule 2: The hero number owns the screen.**

On Device Detail, `₹240` at 22-24sp Bold in `T.State.caution` (#E65100) is the dominant
visual element. Everything else is smaller. This is the entry point.

On Action Window, the carry fee strip `₹340/दिन कट रहा है` at 13sp SemiBold is NOT the
hero — the summary count `1000 NetBox` at 16sp Bold is. The carry fee is L4/L5 urgency
signaling, not L1 hierarchy.

> **[Book: Highlighting, p108]** "Highlight no more than 10 percent of the visible design;
> highlighting effects are diluted as the percentage increases." Only ONE element per
> screen gets hero treatment.

**Rule 3: Weight carries more hierarchy signal than size on small screens.**

On a 360px-wide screen, the difference between 14sp and 16sp is 2sp = ~1.3mm physical.
At arm's length on a 5-inch phone, this is barely perceptible. But the difference between
Regular (400) and Bold (700) is ALWAYS perceptible because it changes the stroke width
of every glyph.

This means: when space is tight, use weight to differentiate, not size.

```
WRONG:  16sp Regular heading, 14sp Regular body  ← looks like one block
RIGHT:  16sp Bold heading, 14sp Regular body      ← clear hierarchy
BETTER: 14sp Bold heading, 13sp Regular body       ← saves space, still clear
```

**Rule 4: Color is the third hierarchy channel.**

> **[Book: Layering, p122]** "Organizing information into related groupings to manage
> complexity and reinforce relationships." Color creates a visual layer on top of size
> and weight.

| Color | Signal | Used for |
|---|---|---|
| `T.Text.primary` (#161021) | "Read me first" | Headings, IDs, primary labels |
| `T.Text.secondary` (#5C5469) | "Read me if you need detail" | Body text, metadata |
| `T.State.caution` (#E65100) | "This costs you money" | Carry fee numbers, urgent values |
| `T.State.negative` (#C62828) | "This is critical" | Damage claims, cap reached |
| `T.State.info` (#0D47A1) | "This is informational" | Order status, pending states |
| `T.Brand.primary` (#D9008D) | "Tap me" | CTA labels, interactive links |

**Rule 5: Hindi text needs +1sp baseline over Latin.**

> **[Book: Legibility, p124]** "Visual clarity of text — size, typeface, contrast, text
> block, and spacing of the characters used."

Devanagari script has taller ascenders and a connecting headline (shirorekha). At sizes
below 12sp, the matras and conjuncts become illegible on 720p budget screens. Rules:

- Minimum body text: 12sp (not 10sp like Latin)
- Minimum badge/caption text: 11sp (not 9sp like Latin)
- Line height: 1.4× for Devanagari (not 1.2× like Latin) — the shirorekha needs room
- Never condense Hindi text horizontally — conjuncts collapse

### Per-screen type map

| Screen | L1 | L2 | L3 | L4 | L5 |
|---|---|---|---|---|---|
| Action Window | — (no hero) | 1000 NetBox | NB#201, NB#502 | ₹340/दिन कट रहा है | खाली, 12 दिन |
| Device Detail | ₹240 | NB#201 | कैरी शुल्क | हर दिन ₹20 लग रहा है | अब तक, अधिकतम |
| Carry Fee Detail | ₹340/दिन | कैरी शुल्क | NB#410, NB#201 | ₹20/दिन, ₹240 अब तक | 30 दिन |
| Return Flow | — | वापसी की पुष्टि | NB#201 | ₹20/दिन कट रहा | खाली |
| Inventory | — | सभी NetBox | ग्राहक के पास: 870 | Filter pills | NB#1001, NB#1002 |

### What goes wrong

| Mistake | Looks like | Principle violated | Fix |
|---|---|---|---|
| All text same size/weight | A wall of grey text | Hierarchy, Similarity | Add weight contrast between L2 and L4 |
| Two hero numbers on one screen | User doesn't know which matters | Highlighting, Entry Point | Demote one to L3 |
| Badge text at 9sp | Illegible on 720p | Legibility | Minimum 11sp for Devanagari |
| Body text in `T.Text.primary` | Everything screams, nothing stands out | Signal-to-Noise | Body = `T.Text.secondary` |
| CTA label same style as body | CTA invisible in the content flow | Similarity (negative) | CTA uses `T.Brand.primary` color or inverse white |

---

## 02 Section > Element Spacing

> **[Book: Proximity, p160]** "Elements that are close together are perceived to be more
> related than elements that are farther apart." This is the SINGLE most important
> principle in spacing. Spacing is not decoration — it is semantic.

> **[Book: Chunking, p30]** "Working memory holds 7±2 items." If the user sees 15
> ungrouped items, they cannot process them. If they see 3 groups of 5, they can.
> Spacing creates the groups.

### The spacing hierarchy

Every gap in the app belongs to exactly one of these six levels. Using a gap from the
wrong level creates a false semantic signal.

| Level | Meaning | Token | Value | Visual signal |
|---|---|---|---|---|
| **Intra-element** | "These are ONE thing" | `space-4` | 4px | Invisible seam — label+value, icon+text |
| **Intra-group** | "These are siblings" | `space-8` | 8px | Breathing room — items in a card, cards in a list |
| **Inter-group** | "These are related groups" | `space-12` / `space-16` | 12-16px | Clear separation — banner→cards, form fields |
| **Inter-section** | "These are separate concerns" | `space-24` | 24px | Hard break — state→cost→actions→outcomes |
| **Page-level** | "Major layout break" | `space-32` / `space-40` | 32-40px | Hero sections, page margins, large containers |
| **Touch target** | "Minimum tappable area" | `space-48` | 48px | Material 3 / Apple HIG / WCAG compliant touch area |

### 4px Grid System

All layout-affecting spacing tokens are multiples of 4px. This is the industry standard
used by Material Design 3, Apple HIG, Carbon (IBM), Ant Design, and Atlassian.

**Why 4px?**
- Divides evenly into all common screen widths (360, 375, 390, 412, 768, 1024, 1280, 1440, 1920)
- Scales cleanly at all display densities: 1x, 1.5x, 2x, 3x — no sub-pixel rendering (4 x 1.5 = 6px clean, vs 5 x 1.5 = 7.5px broken)
- Fine enough for component internals (unlike 8px-only), coarse enough to prevent pixel-level debates (unlike 1px)
- Creates automatic visual rhythm and reduces decision fatigue

### Full CA Design System spacing scale (audited, 4px grid)

**Audit result: 11 of 11 semantic tokens (100%) are on the 4px grid. B+ overall score.**
Benchmarked against Material 3, Apple HIG, Carbon (IBM), Ant Design, Atlassian.

#### Spacing Primitives (13 values)

| Token | Value | /4 | Grid | Primary use |
|---|---|---|---|---|
| `space/1` | 1px | 0.25 | SUB-GRID | Hairline borders, optical adjustments |
| `space/2` | 2px | 0.5 | SUB-GRID | Utility token, fine optical spacing |
| `space/4` | 4px | 1 | ON-GRID | Minimum element spacing, label↔value |
| `space/8` | 8px | 2 | ON-GRID | Tight padding, inline gaps, card↔card siblings |
| `space/12` | 12px | 3 | ON-GRID | Component internal spacing, chip padding |
| `space/16` | 16px | 4 | ON-GRID | Default padding, card insets, screen margins |
| `space/24` | 24px | 6 | ON-GRID | Section spacing, comfortable gaps |
| `space/32` | 32px | 8 | ON-GRID | Large component gaps, flow screen top padding |
| `space/40` | 40px | 10 | ON-GRID | Section breaks, hero area padding |
| `space/48` | 48px | 12 | ON-GRID | Touch target height, page margins, header height |
| `space/64` | 64px | 16 | ON-GRID | Large layout spacing |
| `space/72` | 72px | 18 | ON-GRID | Hero/section padding, bottom sheet primary row |
| `space/84` | 84px | 21 | ON-GRID | Extra-large layout areas |

`space/1` and `space/2` are sub-grid utility tokens for hairline borders and optical
adjustments. All major spacing tokens (4px+) are fully 4px-grid compliant.

#### Semantic Spacing Tokens (11 values)

These alias primitives and are identical in Light/Dark modes.

| Semantic Token | Value | Aliases | Status |
|---|---|---|---|
| `space-4` | 4px | → `space/4` | OK |
| `space-8` | 8px | → `space/8` | OK |
| `space-12` | 12px | → `space/12` | OK |
| `space-16` | 16px | → `space/16` | OK |
| `space-24` | 24px | → `space/24` | OK |
| `space-32` | 32px | → `space/32` | OK |
| `space-40` | 40px | → `space/40` | OK |
| `space-48` | 48px | → `space/48` | OK |
| `space-64` | 64px | → `space/64` | OK |
| `space-72` | 72px | → `space/72` | OK |
| `space-84` | 84px | → `space/84` | OK |

#### Industry Core Alignment

The following subset matches ALL five benchmark systems (Material 3, Apple HIG, Carbon, Ant, Atlassian):
**4, 8, 12, 16, 24, 32, 48, 64** — this is the universal core.

Unique to CA: `space/72` and `space/84` — not found in any other system. These serve
specific layout needs (hero sections, large containers, bottom sheet primary rows).

### Radius tokens (6 levels, aliased from spacing)

All radius tokens are aliased from spacing primitives — ensures radius values share the
same rhythm as spacing (DRY principle).

| Token | Value | Alias | Use case | Grid |
|---|---|---|---|---|
| `radius-below-24dp` | 4px | → `space/4` | Small elements under 24dp (chips, tags, badges) | ON-GRID |
| `radius-below-40dp` | 8px | → `space/8` | Medium elements under 40dp (buttons, inputs) | ON-GRID |
| `radius-inner-card` | 12px | → `space/12` | Inner cards, nested containers | ON-GRID |
| `radius-main-card` | 16px | → `space/16` | Main cards, primary containers | ON-GRID |
| `radius-large` | 24px | → `space/24` | Bottom sheets, modals, FABs | ON-GRID |
| `radius-full` | 9999px | — | Pill shapes, fully rounded elements | N/A |

**Inner radius rule:** Inner radius = Outer radius - padding. Card (16px radius, 16px
padding) → nested element uses 0px or `radius-inner-card` (12px). CA's 16→12
relationship follows Material 3's inner/outer nesting pattern.

### Stroke sizes (3 levels, exempt from 4px grid)

Stroke widths are exempt from the 4px grid by industry convention — they serve visual
weight, not layout.

| Token | Value | Use case | Industry match |
|---|---|---|---|
| `stroke-small` | 1px | Default borders, dividers, input outlines | Standard — all systems use 1px as base |
| `stroke-medium` | 2px | Focus rings, active states, emphasis | Standard — Material focus ring = 2px |
| `stroke-large` | 4px | Strong emphasis, accent stripes, progress bars | Standard — common for thick accents |

### Touch target compliance

Minimum tappable areas for accessible, usable mobile interfaces.

| Standard | Minimum size | CA token | Status |
|---|---|---|---|
| Material Design 3 | 48 x 48 dp | `space-48` = 48px | COMPLIANT |
| Apple HIG | 44 x 44 pt | `space-48` = 48px (exceeds) | COMPLIANT |
| WCAG 2.2 (Level AA) | 24 x 24 css-px | `space-24` = 24px | COMPLIANT |
| WCAG 2.1 (Level AAA) | 44 x 44 css-px | `space-48` = 48px (exceeds) | COMPLIANT |

**Key rule:** Visual size ≠ Touch target. A 24px icon button must still have a 48px
tappable area. Achieve this with transparent padding or hit-area extension.

### Typography-spacing relationship

How font sizes and line-heights interact with the 4px spacing grid. This is where
optical spacing vs mathematical spacing becomes critical.

| Size | Line Height | LH/Size Ratio | Grid Fit | Notes |
|---|---|---|---|---|
| 10sp | 14sp | 1.40 | OK | Minimum caption |
| 12sp | 16sp | 1.33 | OK — 16/4=4 | Body minimum for Devanagari |
| 14sp | 20sp | 1.43 | OK — 20/4=5 | Standard body |
| 16sp | 24sp | 1.50 | OK — 24/4=6 | Section titles |
| 20sp | 28sp | 1.40 | OK — 28/4=7 | Hero numbers |
| 24sp | 32sp | 1.33 | OK — 32/4=8 | Large headings |

**Industry rule:** Line-height ratio decreases as font size increases. Body (12-16sp) needs
1.4-1.6 ratio for readability. Headings (24-48sp) need 1.1-1.3 for tighter visual grouping.
For mobile apps, tighter is preferred.

**Optical spacing warning:** Text bounding boxes include extra space above cap-height and
below baseline. When you set "16px padding above a heading", the visual space is actually
16px + the line-height padding. Measure from cap-height/baseline, not bounding box.

### Spacing anti-patterns

| # | Anti-pattern | Problem | Fix |
|---|---|---|---|
| 1 | Arbitrary pixel values (13px, 17px, 23px) | Breaks grid alignment, creates visual noise | Always use tokens from the scale |
| 2 | Doubled spacing at boundaries | Container bottom (16px) + next container top (16px) = 32px visual gap | Use single-direction margin convention |
| 3 | Inconsistent same-level padding | Card A=16px, Card B=20px, Card C=24px at same hierarchy | Pick one token per hierarchy level, enforce it |
| 4 | Ignoring line-height in text spacing | "8px gap" + heading's line-height padding = 12px visual gap | Account for line-height box in spacing calculations |
| 5 | Spacing by eye per screen | Hand-picking values without tokens, creates drift | Always reference the token scale |
| 6 | Nesting elevation shadows | Card (2px shadow) inside modal (4px shadow) = compounding artifacts | Inner elements in elevated containers should be flat |
| 7 | Fixed spacing without responsive | 64px margins on desktop without reducing for mobile | Use smaller tokens at smaller breakpoints |
| 8 | 8px everywhere | Not everything should be 8px — hierarchy requires varied spacing | Dense elements need 4px; sections need 24px+ |

### The cardinal rule: SECTION gap > ELEMENT gap. Always.

> **[Book: Proximity, p160]** If the gap between two sections is the same as the gap
> between two elements within a section, the sections merge visually. The user cannot
> tell where one section ends and another begins.

```
WRONG:
  [State card]
  8px                    ← same gap everywhere
  [Cost card]
  8px
  [CTA row]

RIGHT:
  [State card]
  16px                   ← inter-section gap (larger)
  [Cost card]
  16px
  [CTA row]
    8px                  ← intra-element gap (smaller) between two buttons
```

### Spacing patterns per screen

**Action Window (vertical flow):**

```
Header (48px)
├─ 16px ──────────────────── T.Space.lg (screen padding top)
├─ [Order Banner]
├─ 12px ──────────────────── T.Space.md (banner→summary, inter-group)
├─ [Summary Bar]
│   ├─ "1000 NetBox"
│   ├─ 8px ────────────────── T.Space.sm (intra-group: count→strip)
│   └─ [Carry Fee Strip]
├─ 12px ──────────────────── T.Space.md (summary→cards, inter-group)
├─ [Card NB#201]
│   ├─ ID row (intra: 6px between ID · badge)
│   ├─ 5px ────────────────── intra-element (body lines)
│   ├─ Cost line
│   ├─ 4px
│   └─ CTA row
├─ 8px ───────────────────── T.Space.sm (card→card, intra-group siblings)
├─ [Card NB#502]
├─ 8px
├─ [Card NB#410]
├─ 8px
├─ [Card NB#105]
├─ 8px
├─ [Card NB#220]
├─ 4px
└─ [Overflow link]
```

**Device Detail (vertical flow):**

```
Header (48px)
├─ 20px ──────────────────── T.Space.xl (content top padding)
├─ "NB#201" (L2 title)
├─ 16px ──────────────────── T.Space.lg (title→first section, inter-section)
├─ [State Card]
│   ├─ Badge
│   ├─ 6px ────────────────── intra-element
│   ├─ "10 मार्च 2026 से"
│   ├─ 6px
│   └─ "ग्राहक चर्न — 12 दिन से खाली"
├─ 16px ──────────────────── T.Space.lg (inter-section)
├─ [Cost Card]
│   ├─ "कैरी शुल्क" (L5 label)
│   ├─ 4px ────────────────── intra-element (tightest: label→value)
│   ├─ "₹20/दिन" (L3)
│   ├─ 4px
│   ├─ "₹240" (L1 HERO)
│   ├─ 4px
│   ├─ "अब तक" (L5)
│   ├─ 4px
│   ├─ "अधिकतम ₹2,000" (L5)
│   ├─ 8px ────────────────── slightly larger: consequence is a different sub-group
│   └─ "हर दिन ₹20 लग रहा है" (L4)
├─ 16px ──────────────────── inter-section
├─ [Action Row: CTA CTA]
│   ├─ 12px between buttons ─ T.Space.md (intra-group siblings)
├─ 16px ──────────────────── inter-section
├─ [Outcome Card]
├─ 16px
└─ [Timeline Card (collapsed)]
```

### Validation test

> **[Book: Figure-Ground, p80]** "Elements are perceived as either figures or ground."
> If spacing is correct, you can identify section boundaries by squinting at the screen
> until text becomes unreadable. The gaps should still show the grouping structure.

**The squint test:** Blur the screen (or squint). If you can still see 4-5 distinct blocks
on Device Detail (state, cost, actions, outcome, timeline), the spacing is correct. If it
looks like one undifferentiated column, the inter-section gaps are too small.

### What goes wrong

| Mistake | Looks like | Principle violated | Fix |
|---|---|---|---|
| Same gap everywhere (all 8px) | One blob of content | Proximity, Chunking | Use 4 spacing levels |
| Section gap < element gap | Sections merge, elements separate | Proximity | Section gap must be > element gap |
| 24px between every card | Cards float, feel disconnected | Proximity | Cards are siblings: 8px |
| 0px between label and value | Label runs into value | Legibility | Minimum 4px intra-element |
| No gap before CTA row | CTA feels attached to body text | Figure-Ground | 4-8px padding-top on CTA row |

---

## 03 Action Prominence

> **[Book: Visibility, p202]** "Systems are more usable when they clearly indicate their
> status, the possible actions that can be performed, and the consequences of actions."
> If the user cannot see the action, the action does not exist.

> **[Book: Gutenberg Diagram, p100]** "Eyes follow a Z-path: top-left (entry) → top-right
> → bottom-left → bottom-right (terminal)." The primary action belongs in the terminal
> area — bottom-right of its container.

### Prominence levels

Every interactive element in the app belongs to exactly one of four prominence levels.
The visual treatment descends in weight from P1 to P4.

> **[Book: Highlighting, p108]** "Highlight no more than 10% of the visible design."
> If every action is P1, nothing is prominent. The budget is: 1 P1, 1 P2, unlimited P3/P4.

| Level | Name | Visual treatment | Where | Max per screen |
|---|---|---|---|---|
| **P1** | Primary action | `T.Cta.primaryBg` (#D9008D) filled, white text, 16px radius, 40-48px height | Bottom-right of card (Gutenberg terminal) or centered below content | **1** |
| **P2** | Secondary action | `T.Cta.secondaryBg` (#FFE5F6) fill, `T.Cta.secondaryBorder` border, brand text | Adjacent to P1, always smaller or lighter | **1** |
| **P3** | Tertiary action | Text-only in `T.Brand.primary`, no fill, no border | Inline in content flow | Unlimited |
| **P4** | Navigation/meta | Chevron (›), icon-only, `T.Text.secondary` or `T.Text.hint` | Trailing edge of rows | Unlimited |

### Placement rules

> **[Book: Gutenberg Diagram, p100]** The Z-path means:
> - Top-left = entry point (screen title, device ID)
> - Top-right = secondary status (chevron, overflow icon)
> - Bottom-left = dead zone (never put primary actions here)
> - Bottom-right = terminal area (primary CTA)

**Rule 1: Primary CTA in the Gutenberg terminal position.**

In a card: CTA row right-aligned (`Arrangement.End`).
In a full screen: CTA centered or full-width at the bottom.
In a bottom sheet: primary item FIRST (top) because the sheet itself is the terminal of the ⋮ flow.

**Rule 2: P1 is visually heaviest. Always.**

The magenta (#D9008D) filled button is the heaviest element on any card. It must outweigh:
- State badges (lightweight: small text + tinted bg)
- Accent stripes (thin: 4px)
- Body text (grey: `T.Text.secondary`)

If ANY of these outweighs the CTA, the hierarchy is broken.

```
WRONG:  Giant red "कैप पूरा" badge + tiny "वापस करें" button → badge dominates
RIGHT:  Small "कैप पूरा" badge (11sp) + standard "वापस करें" button (13sp, filled) → CTA dominates
```

**Rule 3: Distance between reading and acting must be minimal.**

> **[Book: Fitts' Law]** "Time to reach a target is a function of distance and size."

The CTA must be CLOSE to the information that motivates the action:
- "₹240 अब तक" (cost) → "वापस करें" (return CTA): same card, <50px apart
- Carry fee strip → chevron: inline, <10px apart
- Bottom sheet items → drag handle: <100px thumb travel

**Rule 4: Disabled state must be visible but unprominent.**

> **[Book: Visibility, p202]** A disabled CTA must still be visible (the user needs to know
> an action COULD exist). But it must not attract attention.

- Disabled: `T.Cta.primaryDisabledBg` (#A7A1B2) — grey, no contrast with `T.Bg.screen`
- The visual weight drops from P1 to below P4 — it fades into the ground

### Action map per screen

| Screen | P1 | P2 | P3 | P4 |
|---|---|---|---|---|
| Action Window card | वापस करें | — | — | › chevron |
| Device Detail | वापस करें | स्थिति बताएं | — | Timeline ⌄ |
| Return Confirm | वापस करें | रद्द करें | — | ← back |
| Carry Fee Detail row | वापस करें | — | — | › row tap |
| Bottom Sheet | NetBox वापसी दर्ज करें (elevated) | — | नया अनुरोध, कई वापस करें, सभी देखें | › chevrons |
| Inventory | — | — | Filter pills | › row tap, 🔍 search |

### What goes wrong

| Mistake | Looks like | Principle violated | Fix |
|---|---|---|---|
| Two P1 buttons on one card | User hesitates — which one? | Hick's Law, Highlighting | Demote one to P2 |
| CTA in top-left of card | Eye hasn't reached it yet | Gutenberg Diagram | Move to bottom-right |
| CTA same color as body text | Invisible in content flow | Visibility, Figure-Ground | P1 must use filled brand bg |
| CTA far from motivating info | User forgets why they'd tap | Fitts' Law, Proximity | Keep CTA within same card as cost info |
| Badge visually heavier than CTA | Badge dominates, CTA recedes | Highlighting, Hierarchy | Reduce badge size, increase CTA weight |
| Metrics have colored backgrounds (chips) | Everything equal weight, nothing stands out | Highlighting, Hierarchy | Metrics = plain text only. See weight budget below. |

### Visual weight budget — the 3-level rule

> **[Book: Highlighting, p108]** "Highlight no more than 10% of the visible design;
> highlighting effects are diluted as the percentage increases."

Every card has exactly THREE visual weight levels. No more. Adding a fourth
level (e.g., colored chips for metrics) collapses the hierarchy — when everything
has a colored background, nothing stands out.

| Weight | What gets it | Treatment | Why |
|---|---|---|---|
| **Heavy** | CTA only | Filled brand bg (#D9008D), white text | The ONLY filled action element — must dominate |
| **Medium** | Status badge only | Small pill: tinted bg + colored text | Contained, compact — signals state without screaming |
| **Light** | Metrics, cost lines, metadata | **Plain text only**, colored or secondary, NO background | Information layer — must be readable but NOT decorative |

**The rule:** If you add a colored background to a metric, it becomes a badge.
If everything is a badge, nothing is a badge. Metrics are INFORMATION — they
inform the decision. Badges are STATUS — they signal the state. CTAs are
ACTIONS — they enable the response. Three roles, three weights.

**Anti-patterns (learned from iteration):**
- ❌ Metric chips with colored backgrounds → metrics compete with badges AND CTAs
- ❌ Full-width CTA spanning the card → CTA dominates the card, content recedes
- ❌ All elements same size/weight/color → flat visual hierarchy, user can't scan
- ✓ Metrics as plain text, badges as small pills, CTA as compact filled button
- ✓ Metrics LEFT + CTA RIGHT on same row → balanced without equal weight

**Test:** Squint at the card. You should see exactly TWO colored elements:
the badge (small, tinted) and the CTA (compact, magenta). Everything else
should fade to grey text. If you see three or more colored blocks, the
weight budget is blown.

---

## 04 Density

> **[Book: Signal-to-Noise, p182]** "The highest possible signal-to-noise ratio is desirable.
> All communication involves the creation, transmission, and reception of information.
> During each stage, the signal is degraded and noise is added."

> **[Book: Progressive Disclosure, p154]** "Only necessary or requested information is
> displayed at any given time. It keeps displays clean and uncluttered and helps people
> manage complexity without becoming confused."

### The density spectrum

Density is not "how much fits on screen." It is **signal per viewport**. A screen with 30
data points and high noise has LOW effective density. A screen with 10 data points and
zero noise has HIGH effective density.

```
WRONG thinking:  "More items visible = higher density = more efficient"
RIGHT thinking:  "More USEFUL items visible with ZERO noise = optimal density"
```

### Density budget per screen type

> **[Book: Chunking, p30]** Working memory = 7±2 items. A "chunk" (card, section, group)
> counts as 1 item. So a screen can display 5-9 chunks before cognitive overload.

| Screen type | Max chunks visible | Max items per chunk | Total items in first viewport |
|---|---|---|---|
| **Dashboard** (Action Window) | 4-5 | 3-4 lines | 15-20 data points |
| **Detail** (Device Detail) | 3-4 | 4-6 lines | 12-18 data points |
| **List** (Inventory) | 6-8 | 1-2 lines | 8-16 data points |
| **Flow** (Return, Request) | 2-3 | 3-5 lines | 6-12 data points |
| **Result** (Success/Failure) | 1-2 | 2-3 lines | 3-5 data points |

### Measuring density

**Method: the 3-second scan test.**

> **[Book: Entry Point, p64]** "People do judge books by their covers." The first 3 seconds
> determine whether the user understands the screen. In 3 seconds, the user can:
> - Read 1 heading
> - Scan 2-3 data points
> - Identify 1 action

If the screen cannot deliver this in 3 seconds, it is either too dense (too much competing
for attention) or too sparse (the important things are too small or too far away).

**First viewport inventory:** List every visible element when the screen first loads (before
scroll). Categorize each as Signal or Noise:

| Element type | Signal or Noise | Justification |
|---|---|---|
| Device ID | Signal | Answers "which device?" |
| State badge | Signal | Answers "what state?" |
| Cost line | Signal | Answers "what's the impact?" |
| CTA | Signal | Answers "what can I do?" |
| Accent stripe | Signal (compressed) | Encodes urgency in 4px |
| Card border | Structural noise (necessary) | Creates figure-ground, not information |
| Chevron | Structural noise (necessary) | Affords tappability |
| Screen bg | Ground (invisible) | Disappears when perceived correctly |
| Decorative icon | Noise | Remove unless it encodes unique information |
| Redundant label | Noise | Remove if the value is self-evident |

### Density rules

**Rule 1: No element without a job.**

> **[Book: Signal-to-Noise, p182]** "Noise is any information that is not relevant to the
> purpose of the communication."

Every element on screen must answer ONE of these questions:
- What is this? (identity)
- What state is it in? (status)
- What does it cost? (impact)
- What can I do? (action)
- Where am I? (navigation)

If an element doesn't answer any of these, it is noise. Remove it.

**Rule 2: Compress, don't remove.**

> **[Book: Progressive Disclosure, p154]** "Separating information into multiple layers."

When density is too high, the instinct is to remove items. But sometimes the information
is necessary — it just needs compression:

| Full form | Compressed form | Compression method |
|---|---|---|
| "कैरी शुल्क: ₹20 प्रतिदिन" | "₹20/दिन" | Remove label, use unit suffix |
| "12 दिन से खाली पड़ा है" | "12 दिन" | Remove auxiliary verb, context supplies meaning |
| Device state + separate badge + separate line | State badge inline with ID | Merge into single line |
| "वापस करें" + "स्थिति बताएं" stacked | Side by side | Horizontal layout saves vertical space |

**Rule 3: Vertical space is the scarcest resource.**

On a 360×720px screen with a 48px header and 56px nav bar, the content area is
**616px tall**. After 16px top padding and 16px bottom padding: **584px usable**.

At 8px card gaps and ~90px per card: **maximum 6 cards visible** before scroll.

The Action Window shows 5 cards + summary + banner. That is EXACTLY at the limit.
Adding ANY visual weight (larger gaps, taller cards, extra labels) pushes cards below
the fold and reduces scan efficiency.

**Rule 4: Density decreases as consequence increases.**

> **[Book: Layering, p122]** "Only certain groupings available at any one time."

| Screen | Consequence of action | Density target |
|---|---|---|
| Action Window | Low (scanning) | HIGH — max info per viewport |
| Device Detail | Medium (understanding) | MEDIUM — balanced info + breathing room |
| Return Confirm | HIGH (irreversible action) | LOW — only the decision + confirmation |
| Success/Failure | None (result) | MINIMAL — one message, one action |

The more irreversible the action, the sparser the screen. A confirmation screen with
high density creates anxiety. A result screen with high density steals the moment of
completion.

### Density audit

For any screen, count the elements in the first viewport:

| Count | Verdict | Action |
|---|---|---|
| <5 data points | Too sparse | The screen isn't justifying its existence. Merge with parent. |
| 5-15 data points | Optimal | Most screens should be here. |
| 15-25 data points | Dense but manageable | Only acceptable for dashboards with strong chunking. |
| >25 data points | Overloaded | Apply progressive disclosure. Collapse sections. Remove noise. |

### What goes wrong

| Mistake | Looks like | Principle violated | Fix |
|---|---|---|---|
| Every label spelled out | "कैरी शुल्क प्रतिदिन दर: ₹20" | Signal-to-Noise | Compress: "₹20/दिन" |
| All sections expanded | 40+ data points visible | Progressive Disclosure | Collapse timeline, hide secondary |
| Equal spacing everywhere | Dense wall, no grouping | Proximity, Chunking | Use spacing hierarchy (4/8/12/16/24px) |
| Confirmation screen is dense | User hesitates, anxious | Layering | Strip to: summary + 2 buttons |
| Empty screen with 1 element | Feels broken | Signal-to-Noise | Add context or merge with parent |

---

## Proven component patterns

These patterns were validated through iteration. Use them as-is.

### Cost card pattern (Device Detail)

The cost card communicates: what is the fee, how much has accumulated, what is the cap,
and what should the user do about it. All in one unified card.

```
┌─────────────────────────────────────────┐
│  कैरी शुल्क · ₹20/दिन                   │  ← Identity row: label + rate together
│                                         │     [Mapping] fee label IS ₹20/दिन
│  ₹240                                   │  ← Hero (28sp Bold, caution color)
│  अब तक                                  │  ← Qualifier (11sp, secondary)
│                                         │
│  ████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  │  ← Progress bar (full width)
│  ₹240              अधिकतम ₹2,000        │  ← Labels below bar: current left, cap right
│                                         │
│  ● हर दिन ₹20 लग रहा है                 │  ← Consequence (dot + medium weight)
│  ─────────────────────────────────────  │  ← Divider
│  [ वापस करें ]   [ स्थिति बताएं ]        │  ← CTAs inside card (equal width, fill)
└─────────────────────────────────────────┘
```

**Why this layout works:**
- Identity row pairs label + value → no confusion about what the fee IS
- Hero ₹240 stands alone with breathing room → undisputed focal point
- Progress bar connects ₹240 to ₹2,000 visually → [Uniform Connectedness]
- Labels under the bar (not above) → numbers don't scatter into corners
- CTAs inside the card → [Proximity] to motivating cost information
- Divider separates reading zone from action zone → [Chunking]

**Anti-patterns caught:**
- ❌ कैरी शुल्क above ₹240 without ₹20/दिन → reads as "carry fee is ₹240"
- ❌ ₹240 left, ₹20/दिन top-right, ₹2,000 bottom-right → numbers in 3 corners
- ❌ CTAs floating between cards → disconnected from motivating information
- ❌ All text left-aligned in a single column → right half is dead space

### State context pattern (Device Detail)

State information must be CONTAINED in a card — not floating on the screen background.
Floating text on `T.Bg.screen` feels unanchored and disconnected.

```
┌─────────────────────────────────────────┐
│  [खाली] · 10 मार्च 2026 से              │  ← Badge + date on one line
│  ग्राहक चर्न — 12 दिन से खाली            │  ← Subtitle below
└─────────────────────────────────────────┘
```

**Why:** [Figure-Ground] — text on the grey screen background has no figure to anchor it.
A white card creates the figure boundary. The card also groups the badge + date + subtitle
as one chunk via [Proximity].

### Action Window card pattern

Each device card in the action window follows this 2-row structure:

```
┌──────────────────────────────────────────────────┐
│▌ NB#201 · [खाली]                              › │  Row 1: ID(bold) · Badge(pill) · Chevron(hint)
│▌ ₹20/दिन ₹240 अब तक · 12 दिन    [वापस करें]   │  Row 2: Cost(plain text) ↔ CTA(filled)
└──────────────────────────────────────────────────┘
```

**3 weight levels per card:**
- **Heavy:** CTA (magenta fill) — the ONLY filled action element in the card
- **Medium:** Badge (small pill with tinted bg) — status indicator, compact
- **Light:** Cost metrics (plain colored text, NO background) — information, not decoration

**Rules:**
- Accent stripe (4px) encodes urgency by color (orange/red/grey)
- Row 1: ID (15sp Bold) + Badge (11sp pill) + Chevron (hint grey) — identity
- Row 2: Cost text LEFT (12sp Regular, plain) + CTA RIGHT (compact filled) — balanced
- Both rows use BOTH sides of the card — no dead space
- Primary focus card: `T.Bg.brandTint` (#FFE5F6) background, only ONE per screen
- Cards without actions (e.g., NB#105): single row, naturally compact
- Max 1 CTA per card [Hick's Law]

**Adaptive card height — content dictates structure:**

Card height communicates meaning. More content = taller. Less content = shorter.
Never pad a card to match its siblings' height — variable height IS information.

| Content available | Rows | Layout | Example |
|---|---|---|---|
| ID + badge + cost text + CTA | **2 rows** | Row 1: identity. Row 2: cost LEFT ↔ CTA RIGHT | NB#201 (₹20/दिन ₹240 अब तक) |
| ID + badge + subtitle + CTA | **2 rows** | Row 1: identity. Row 2: subtitle LEFT ↔ CTA RIGHT | NB#410 (शुल्क 5 दिन में शुरू) |
| ID + badge + CTA only (no text) | **1 row** | ID + badge LEFT ↔ CTA RIGHT (all on Row 1) | NB#502 (कैप पूरा — no cost text needed) |
| ID + badge only (no CTA, no text) | **1 row** | ID + badge LEFT ↔ chevron RIGHT | NB#105 (वापसी हो रही — no action) |

**The rule:** If Row 2 would have content only on ONE side (e.g., only CTA, no text),
merge it into Row 1. A row with a single element floating in empty space is dead weight.
Only create Row 2 when BOTH sides have content (text LEFT + CTA RIGHT).

**Anti-patterns:**
- ❌ Metric chips with colored backgrounds → equal weight as badge and CTA
- ❌ Full-width CTA spanning card → CTA dominates content
- ❌ CTA on separate row below content → dead space, card too tall
- ❌ All text same size/weight → flat hierarchy, can't scan
- ❌ Empty Row 2 with just a CTA floating right → wasted vertical space
- ❌ All cards same height despite different content → false uniformity

### Earnings card pattern (Bottom Sheet)

The earnings card communicates: what is the earning type, how much has been earned,
what is the target, and at what rate. Mirrors the cost card's spacing discipline exactly.

```
┌─────────────────────────────────────────┐
│  कमाई (पक्की) · ₹300/recharge          │  ← Identity row: label + rate together
│                                         │     [Mapping] earning type IS ₹300/recharge
│  ₹9,000                                │  ← Hero (28sp Bold, positive green)
│  अब तक                                 │  ← Qualifier (11sp, secondary)
│                                         │
│  █████████░░░░░░░░░░░░░░░░░░░░░░░░░░░  │  ← Progress bar (full width, 60%)
│  ₹9,000               ₹15,000 लक्ष्य   │  ← Labels below bar: current left, target right
│                                         │
│  ● 30 recharges × ₹300 · 2 घंटे पहले   │  ← Summary (dot + compressed detail)
└─────────────────────────────────────────┘
```

**Spacing anatomy (exact px values):**

```
16px pad top                              ← space-16: generous card padding
Identity row: label · rate                ← 14sp SemiBold + positive color
  8px                                     ← space-8: inter-group (identity → hero)
Hero: ₹9,000                              ← 28sp Bold, positive green (#2E7D32)
  4px                                     ← space-4: intra-element (hero → qualifier)
Qualifier: अब तक                          ← 11sp Regular, secondary color
  12px                                    ← space-12: inter-group (qualifier → progress)
[Progress bar, 6px tall]                  ← visual break between hero zone and detail zone
  4px                                     ← space-4: intra-element (bar → labels)
Labels: ₹9,000 left / ₹15,000 right      ← 11sp, SPACE_BETWEEN layout
  12px                                    ← space-12: inter-group (labels → summary)
Summary: ● 30 recharges × ₹300           ← 12sp SemiBold, dot prefix
16px pad bottom                           ← space-16: symmetric with top
```

**Why this layout works:**
- Identity row pairs label + rate → same pattern as cost card (कैरी शुल्क · ₹20/दिन)
- Hero ₹9,000 stands alone with 8px above, 4px below → undisputed focal point
- Progress bar creates a visual break between hero zone and detail zone → [Uniform Connectedness]
- Labels under the bar (not above) → numbers don't scatter into corners
- Summary line uses dot prefix (●) → same weight as cost card's consequence line
- Symmetric 16px padding top = 16px bottom → card feels anchored and balanced

**Anti-patterns caught:**
- ❌ ₹9,000 crammed next to breakdown text with 4px gap → hero loses dominance
- ❌ No progress bar → no visual connection between current and target
- ❌ "Last recharge: 2 hours ago" as full sentence → compress to "2 घंटे पहले"
- ❌ Qualifier "अब तक" on same line as hero → hero needs its own line to own the space

### Bottom sheet list pattern (Connections, Inventory)

A scrollable list inside a bottom sheet. Section headers use tinted backgrounds,
list items use indented dividers, and warning items get accent stripes.

```
┌─────────────────────────────────────────┐  ← radius-large (24px) for bottom sheet
│              ═══                        │  ← Drag handle (40×4px, centered)
│                                         │
│  🔗 कनेक्शन — 45 active · 51 total     │  ← Sheet title (L2, 16sp Bold)
│─────────────────────────────────────────│  ← Full-bleed divider under title
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
│░ ▾ चालू 45            ⚡3 recharge     ░│  ← Section header: tinted bg (#F8F7FA)
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│     14sp Bold + caution badge right
│  #CN-4021  Sector 15                 › │  ← Normal item: 13sp SemiBold ID + Regular location
│  चालू                                  │     11sp status in positive green
│           ─────────────────────────     │  ← Indented divider (16px left inset)
│  #CN-4022  Sector 22                 › │
│  चालू                                  │
│           ─────────────────────────     │
│▌ #CN-4023  Sector 8                  › │  ← WARNING: caution accent stripe (3px)
│▌ ⚠ Recharge expiring                  │     11sp status in caution orange
│           ─────────────────────────     │
│  #CN-4024  DLF Phase 4               › │
│  चालू                                  │
│           ─────────────────────────     │
│▌ #CN-4025  Sector 14                 › │  ← CRITICAL: negative accent stripe (3px, red)
│▌ ⚠ आज expire होगा                     │     11sp status in negative red
│─────────────────────────────────────────│  ← Full-bleed divider before collapsed sections
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
│░ ▸ रुका हुआ 3          ⚡1 recharge   ░│  ← Collapsed section: tinted bg, ▸ chevron
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
│─────────────────────────────────────────│
│░ ▸ बंद हो रहा है 1                    ░│
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
└─────────────────────────────────────────┘
```

**Divider hierarchy (two levels):**
- **Indented dividers** (16px left inset): between sibling list items within a section
- **Full-bleed dividers** (edge to edge): between sections — heavier visual break

**Section header rules:**
- Tinted background (`#F8F7FA`) → distinguishes headers from list items [Figure-Ground]
- 14sp Bold title + count → L3 level, clearly above L4 list items
- Badge on right (⚡ + count) → caution-colored warning, right-aligned [Gutenberg terminal]
- ▾ expanded / ▸ collapsed chevrons → clear affordance for expand/collapse

**List item rules:**
- Normal items: no accent stripe, positive green "चालू" status
- Warning items: accent stripe (3px, caution orange or negative red)
- All items: consistent chevron (›) on right → affords tappability [Similarity]
- ID at 13sp SemiBold (L4), location at 13sp Regular (L4 secondary), status at 11sp (L5)

**Anti-patterns:**
- ❌ Full-bleed dividers between every item → too heavy, all separations look equal
- ❌ No accent stripe on warning items → warnings blend with normal items
- ❌ Section header same background as items → header doesn't group its children
- ❌ Inconsistent chevrons (some rows have ›, some don't) → breaks [Similarity]

### Home dashboard pattern (360×720)

The home screen at the budget Android target viewport. All content must fit in
a single viewport (no scroll for first view).

```
┌──────────────────────────────────────┐  ← 360×720px viewport
│ ☰  Rajesh Kumar · एडमिन         🔔³ │  ← Header (48px, headerBg)
│──────────────────────────────────────│
│  ┌─────────┐ 8px ┌─────────┐       │  ← Metric Row 1: 8px gap (intra-group)
│  │ 45    › │     │ ₹9,000› │       │     L1 hero (20sp) + L2 (16sp)
│  │कनेक्शन │     │ कमाई    │       │     11sp labels (L5)
│  └─────────┘     └─────────┘       │
│       8px                           │  ← space-8: metric rows are siblings
│  ┌─────────┐ 8px ┌─────────┐       │  ← Metric Row 2
│  │ ⚠     › │     │✓ अच्छा›│       │     State-colored values
│  │क्वालिटी │     │काम स्थिति│       │
│  └─────────┘     └─────────┘       │
│      12px                           │  ← space-12: inter-group (metrics → banner)
│  ┌──────────────────────────────┐   │
│  │ ℹ ऑफ़लाइन — पिछला डेटा      │   │  ← Info banner (12sp SemiBold, info blue)
│  └──────────────────────────────┘   │
│      12px                           │  ← space-12: inter-group (banner → tasks)
│  ┌──────────────────────────────┐   │
│  │▌ 🔧 रिस्टोर · #SR-8842  देखें›│   │  ← Task card: P1 CTA (ONLY this card)
│  │▌ [⚠ नेट बंद · 8 मि.] [शुरू]│   │     Red stripe + magenta filled CTA
│  └──────────────────────────────┘   │
│       8px                           │  ← space-8: card siblings
│  ┌──────────────────────────────┐   │
│  │▌ 🏠 इंस्टॉल · #CN-4021  देखें›│   │  ← Task card: P2 CTA (outlined)
│  │▌ [जल्द · 5 दिन]  [समय तय करें]│   │     Caution stripe + outlined brand CTA
│  └──────────────────────────────┘   │
│       8px                           │
│  ┌──────────────────────────────┐   │
│  │▌ 🏠 इंस्टॉल · #CN-5110  देखें›│   │  ← More task cards...
│  │▌ [⚠ देरी · 2 घंटे]  [ठीक करें]│   │
│  └──────────────────────────────┘   │
│      12px                           │  ← space-12: inter-group (tasks → network)
│  ┌──────────────────────────────┐   │
│  │ ═══ amber bar                │   │  ← Network issue banner
│  │ इलाके में नेटवर्क समस्या  देखें›│   │     14sp Bold title + 12sp detail
│  │ 12 कनेक्शन प्रभावित · चालू   │   │
│  └──────────────────────────────┘   │
└──────────────────────────────────────┘
```

**Viewport budget (360×720px):**

```
Status bar:       24px
Header:           48px
Content padding:  12px top + 16px bottom = 28px
─────────────────────────
Available:        720 - 24 - 48 - 28 = 620px
Content used:     ~614px (fits with 6px margin)
```

**P1/P2 CTA hierarchy:**
- **P1** (solid magenta fill): only the MOST URGENT task card — 1 max per screen
- **P2** (outlined, light brand bg): all other task cards with actions
- This follows [Highlighting p108] — "no more than 10% of the visible design"

**Metric card hero rule:**
- "45" at 20sp Bold = L1 hero entry point (biggest number on screen)
- "₹9,000" at 16sp Bold = L2 (visibly secondary)
- State values (⚠, ✓ अच्छा) use state colors (caution, positive)

---

## Cross-area interactions

These four areas are not independent. They interact:

| Interaction | Resolution |
|---|---|
| Larger type (01) increases density (04) | Use weight instead of size for hierarchy on dense screens |
| Larger section gaps (02) decrease density (04) | On dashboards, keep inter-section at 12px not 24px |
| More prominent CTAs (03) increase density (04) | CTA inside card (inline) is denser than CTA below card (stacked) |
| Higher density (04) requires stronger hierarchy (01) | Dense screens need MORE type contrast, not less |

### The golden balance

A well-designed Wiom screen has:
- **3 type levels visible** in any viewport (not 1, not 5)
- **2 spacing levels** visible (intra + inter, never uniform)
- **1 primary action** visible without scrolling
- **10-15 data points** in the first viewport (dashboard) or 6-10 (detail)

> **[Book: Hierarchy, p104]** "The simplest structure for visualizing complexity." If the
> screen feels complex, the hierarchy isn't working. Add structure (type, spacing, grouping)
> rather than removing content.

---

## Review checklist

| # | Check | Pass criteria | Area |
|---|---|---|---|
| 1 | Can you identify the hero element in <1 second? | One element is clearly largest/boldest | 01 |
| 2 | Are there exactly 3-4 type levels visible? | No flat text walls, no 5-level chaos | 01 |
| 3 | Is Hindi text ≥12sp body, ≥11sp caption? | Readable on 720p budget screens | 01 |
| 4 | Do section gaps > element gaps? | Squint test passes — groups are visible | 02 |
| 5 | Is the spacing hierarchy 4-level? | xs/sm/md-lg/xxl all used correctly | 02 |
| 6 | Is the primary CTA the heaviest visual element? | Magenta filled button outweighs everything | 03 |
| 7 | Is the primary CTA in Gutenberg terminal position? | Bottom-right of card, or centered below content | 03 |
| 8 | Is there max 1 P1 + 1 P2 per context? | No two magenta buttons competing | 03 |
| 9 | Does the first viewport have 5-15 data points? | 3-second scan delivers heading + 2 facts + 1 action | 04 |
| 10 | Is every element answering one of the 5 questions? | No decorative elements, no redundant labels | 04 |
| 11 | Does density decrease as consequence increases? | Confirm screens are sparse, dashboards are dense | 04 |
| 12 | Are cross-area interactions resolved? | Larger type hasn't blown the density budget | All |
| 13 | Are accent stripes clipped inside card bounds? | `.clip(shape)` on parent Row before `.background()` | 03 |
| 14 | Does the cost card include a progress bar? | LinearProgressIndicator connecting accumulated↔cap | 01, 04 |
| 15 | Are CTAs inside the cost card, not floating below? | Divider separates reading zone from action zone | 03 |

---

## Implementation rules (learned from v2.0 build)

These rules were discovered during the first complete implementation of this skill
against the NetBox module. They address Compose-level details that the visual
principles above do not cover.

### Compose clipping rule

**Problem:** Accent stripes (4px colored bars on card left edge) overflow outside
rounded card corners on real devices, even when the stripe has its own
`RoundedCornerShape(topStart)`. The stripe's corners do not align perfectly with
the card's corners because they are separate shapes.

**Fix:** Always apply `.clip(shape)` on the parent container (Row/Column) BEFORE
`.background()` and `.border()`. This clips ALL children — including the accent
stripe — to the card's rounded boundary.

```kotlin
// WRONG — stripe corners overflow the card
Row(
    Modifier
        .background(cardBg, shape)
        .border(1.dp, stroke, shape)
) {
    Box(Modifier.width(4.dp).background(accentColor, RoundedCornerShape(topStart = 12.dp)))
}

// RIGHT — .clip() on parent clips all children
Row(
    Modifier
        .clip(shape)              // ← clip FIRST
        .background(cardBg, shape)
        .border(1.dp, stroke, shape)
) {
    Box(Modifier.width(4.dp).background(accentColor))  // no shape needed
}
```

**Rule:** When a card has internal elements that touch the card edge (accent stripes,
colored headers, progress bars), the parent MUST use `.clip(shape)`. The child
element needs NO shape of its own — the parent clips it.

### Type token mapping (WiomTokens.kt)

The Visual Skill's 5-level type scale maps to these WiomTokens entries:

| Visual Skill Level | Token name | Size | Weight | When to use |
|---|---|---|---|---|
| L1 Hero | `T.Type.heroNumber` | 22sp | Bold (700) | ONE dominant number per screen (₹240, ₹340/दिन) |
| L2 Section | `T.Type.sectionTitle` | 16sp | Bold (700) | Screen title, card group heading (1000 NetBox) |
| L3 Card Identity | `T.Type.cardIdentity` | 14sp | SemiBold (600) | Device ID, row label (NB#201) |
| L4 Body | `T.Type.body` | 14sp | Regular (400) | Supporting text, descriptions |
| L5 Caption | `T.Type.caption` | 11sp | Regular (400) | Badges, timestamps, metadata |

**Note:** `T.Type.cta` is 14sp Bold — visually between L2 and L3, but used ONLY for
CTA labels. It is NOT a hierarchy level — it is an action signal.

### Progress bar in cost card

The cost card includes a `LinearProgressIndicator` connecting the accumulated amount
to the cap. Implementation:

```kotlin
val accNum = accumulated.replace("[^0-9]".toRegex(), "").toFloatOrNull() ?: 0f
val capNum = cap.replace("[^0-9]".toRegex(), "").toFloatOrNull() ?: 1f
val progress = (accNum / capNum).coerceIn(0f, 1f)

LinearProgressIndicator(
    progress = { progress },
    modifier = Modifier.fillMaxWidth().height(6.dp).clip(RoundedCornerShape(3.dp)),
    color = if (capReached) T.State.negative else T.State.caution,
    trackColor = T.Bg.secondary,
    strokeCap = StrokeCap.Round,
)
```

- Bar color: `T.State.caution` (orange) normally, `T.State.negative` (red) when cap reached
- Track: `T.Bg.secondary` — neutral, does not compete
- Height: 6px — visible but not dominant
- Labels below bar: accumulated LEFT, cap RIGHT (not above)

### Adaptive card height rule

Card height is NOT fixed — it adapts to content. This is intentional information design:

| Content available | Rows | Why |
|---|---|---|
| ID + badge + cost + CTA | 2 rows | Both sides of Row 2 have content |
| ID + badge + subtitle + CTA | 2 rows | Subtitle LEFT, CTA RIGHT |
| ID + badge + CTA only | 1 row | CTA merges into Row 1 — no empty Row 2 |
| ID + badge only (no CTA) | 1 row | Naturally compact — no padding needed |

**Never** pad a short card to match sibling height. Variable height IS information —
a tall card signals "more going on here" and a short card signals "nothing to act on."


---
---

# DOMAIN B — Screen States

## Four states at a glance

| # | State | Governs | Read when |
|---|---|---|---|
| 01 | [Empty](#01-empty-states) | No data, no results, first-time use | Blank space below header, "no data" with no explanation |
| 02 | [Loading](#02-loading-states) | Initial load, refresh, action processing | Blank screen during API call, spinner that never resolves |
| 03 | [Error](#03-error-states) | Network failure, server error, timeout | English error, toast disappears, no retry button |
| 04 | [Offline](#04-offline-states) | No connectivity, cached data, sync pending | Spinner on no signal, write fails silently, stale data with no indicator |

---

## Source principles

| Principle | Book page | Role in this skill |
|---|---|---|
| Horror Vacui | p54 | "Tendency to fill empty spaces." A blank screen is NOT empty — it is filled with user anxiety. The empty state IS content. |
| Forgiveness | p43 | "Designs should help users avoid errors and minimize negative consequences." The system failed, not the user. Recovery must be obvious. |
| Visibility | p202 | "Status and possible actions must be clearly visible." Loading progress, error state, offline status — all must be visible, not hidden. |
| Constraint | p36 | "Limiting actions to reduce errors." Offline mode disables write actions — preventing silent failures. |
| Feedback | p70 | "Information about the result of an action." Every state change must be communicated: loading→success, loading→error, offline→online. |

---

## State architecture

### The five screen states

Every Wiom screen exists in exactly one of these states:

```
                    ┌─────────┐
                    │ LOADING │
                    └────┬────┘
                         │
              ┌──────────┼──────────┐
              ▼          ▼          ▼
         ┌─────────┐ ┌───────┐ ┌────────┐
         │ SUCCESS │ │ EMPTY │ │ ERROR  │
         └─────────┘ └───────┘ └────────┘
                                    │
                               ┌────▼────┐
                               │ OFFLINE │
                               └─────────┘
```

### Priority (when multiple states conflict)

| Priority | State | Shows when |
|---|---|---|
| P0 | Offline (no cache) | No internet + no cached data. User can do nothing. |
| P1 | Full-screen error | API failed + no cached data. |
| P2 | Loading (initial) | First load, no data yet. |
| P3 | Offline (with cache) | No internet but cached data exists. Show data + banner. |
| P4 | Partial error | Most content loaded, one section failed. Inline error. |
| P5 | Empty | API succeeded but returned zero items. |
| P6 | Loading (refresh) | Content exists, refreshing in background. |

### Conflict resolution

```
IF offline AND no cache      → Offline full-screen (P0)
IF offline AND cache exists  → Cached content + connection banner (P3)
IF loading AND offline       → Skip loading, go to offline immediately
IF error AND partial data    → Show data + inline error card (P4)
IF empty AND offline         → Offline state, not empty state
IF refresh AND error         → Keep existing content + banner error
```

---

## 01 Empty states

> **[Book: Horror Vacui, p54]** "A tendency to fill empty spaces with content."
> A blank screen below the header is not "empty" — it is filled with user anxiety.
> The partner sees white space and concludes: the app is broken, or data was lost.
> The empty state IS content. It explains what happened and what to do.

### The anatomy (3 parts required)

Every empty state has exactly three parts. Optional: a fourth (action CTA).

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│                                      │
│         ┌──────────────┐             │
│         │  ○ 96x96px   │  ← Illustration circle
│         │  icon 48px   │    T.Bg.info (#E3F2FD) + T.State.info icon
│         └──────────────┘    OR T.Bg.brandTint (#FFE5F6) for first-time
│           ↕ 16px (T.Space.lg)        │
│         ┌──────────────────┐         │
│         │ कोई NetBox नहीं  │  ← Headline: 16sp Bold, T.Text.primary
│         └──────────────────┘    Answers "क्या हुआ?" (what happened?)
│           ↕ 8px (T.Space.sm)         │
│         ┌──────────────────┐         │
│         │ नया NetBox जोड़ने │  ← Explanation: 14sp Regular, T.Text.secondary
│         │ पर यहाँ दिखेगा   │    Answers "अब क्या करें?" (what to do?)
│         └──────────────────┘    Center-aligned. Max 2 lines.
│           ↕ 24px (T.Space.xxl)       │
│         ┌──────────────────┐         │
│         │ [नया अनुरोध करें]│  ← Action CTA (optional): secondary style
│         │  48px height     │    T.Cta.secondaryBg (#FFE5F6)
│         └──────────────────┘    Only if user can resolve the empty state
│                                      │
├──────────────────────────────────────┤
│  [Bottom Nav — 56px + inset]         │
└──────────────────────────────────────┘
```

### Three variants

| Variant | Illustration | Tone | Example |
|---|---|---|---|
| **No data yet** (server hasn't sent data) | `T.Bg.info` circle, `T.State.info` icon | Neutral: "अभी कोई [X] नहीं है" | Device list before first order |
| **No results** (search/filter returned 0) | `T.Bg.info` circle, `T.State.info` icon | Helpful: "कोई नतीजा नहीं मिला" | Search with no matches |
| **First-time use** (user has never created data) | `T.Bg.brandTint` circle, `T.Brand.primary` icon | Encouraging: "यहाँ आपके [X] दिखेंगे" | First login, empty task feed |

### Rules

1. **Every list screen must define its empty composable.** No exceptions.
2. **Headline answers "क्या हुआ?"** Explanation answers "अब क्या करें?"
3. **First-time-use is NOT an error.** Use brand pink, not error red. Tone is encouraging.
4. **No-results preserves the filter UI.** Empty message appears below active filters, never replacing them.
5. **Never use "कोई डेटा नहीं" generically.** Each screen names the entity.

### Screen map

| Screen | Variant | Headline (Hindi) | Explanation (Hindi) | CTA |
|---|---|---|---|---|
| Home — device list | First-time | यहाँ आपके NetBox दिखेंगे | नया NetBox अनुरोध करें — मंज़ूरी के बाद दिखेगा | नया अनुरोध करें |
| Home — filter active | No results | इस फ़िल्टर में कोई NetBox नहीं | दूसरा फ़िल्टर चुनें या सब देखें | फ़िल्टर हटाएं |
| Task feed | No data yet | अभी कोई टास्क नहीं है | नए टास्क आने पर यहाँ दिखेंगे | — |
| Transaction history | No data yet | अभी कोई लेन-देन नहीं हुआ | पहला रिचार्ज करने पर यहाँ दिखेगा | — |
| Notifications | No data yet | कोई नई सूचना नहीं | नई सूचनाएँ आने पर यहाँ दिखेंगी | — |
| Search results | No results | कोई नतीजा नहीं मिला | दूसरे शब्दों से खोजें | — |

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| Blank white space below header | Partner thinks app is broken | Add empty state with illustration + text |
| "No data" in English | Invisible to Hindi-first user | Hindi copy, screen-specific |
| Generic "कोई डेटा नहीं" for all screens | No context, dead end | Name the entity, explain when data appears |
| Red icon/text for empty state | Empty treated as error = alarm | Use info blue. Empty is not an error |
| Hiding filter bar when no results | User can't modify filters to get results | Keep filters visible, empty below |
| "रिफ्रेश करें" on first-time-use | Refreshing empty returns same empty | Show creation CTA instead |

---

## 02 Loading states

> **[Book: Visibility, p202]** "Systems are more usable when they clearly indicate their status."
> A blank screen during an API call is not "loading" — it is invisible. The partner
> sees white for 3 seconds and concludes the app is frozen. Skeleton shimmer shows
> structure immediately. The user understands "data is coming."

### The rule: skeleton for initial, spinner for action

| When | Pattern | Why |
|---|---|---|
| **Initial page load** | Skeleton shimmer (matches content layout) | Shows structural preview. User sees cards forming. |
| **Pull-to-refresh** | Branded spinner over existing content | Content stays visible. Only a hint that refresh is happening. |
| **User-initiated action** (submit, pay) | Processing overlay (scrim + spinner + text) | Blocks interaction. Prevents double-tap. |
| **Button loading** | Spinner replaces button text, button disabled | Same dimensions. Clear that action is in progress. |

### Skeleton pattern

Skeleton must match the actual content layout. If the screen shows cards with a title and 2 body lines, the skeleton shows card shapes with matching bars.

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│  ┌──────────────────────────────┐    │
│  │ ▓▓▓▓▓▓▓▓░░░ (60%, 20px)     │    │  ← title bar
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓░░ (100%,14px) │    │  ← body line 1
│  │ ▓▓▓▓▓▓▓▓░░░░░░ (80%, 14px)  │    │  ← body line 2
│  └──────────────────────────────┘    │
│    ↕ 8px (T.Space.sm)               │
│  ┌──────────────────────────────┐    │
│  │ ▓▓▓▓▓▓▓▓░░░ (60%, 20px)     │    │
│  │ ▓▓▓▓▓▓▓▓▓▓▓▓▓░░ (100%,14px) │    │
│  │ ▓▓▓▓▓▓▓▓░░░░░░ (80%, 14px)  │    │
│  └──────────────────────────────┘    │
│    ↕ 8px                             │
│  ┌──────────────────────────────┐    │
│  │ (repeat 3-5 skeleton cards)  │    │
│  └──────────────────────────────┘    │
├──────────────────────────────────────┤
│  [Bottom Nav — 56px + inset]         │
└──────────────────────────────────────┘
```

### Processing overlay pattern

```
┌──────────────────────────────────────┐
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│  ← scrim 50% black
│░░░░░░░░░░ ╭────╮ ░░░░░░░░░░░░░░░░░░│
│░░░░░░░░░░ │ ◠  │ ░░░░░░░░░░░░░░░░░░│  ← spinner T.Brand.primary
│░░░░░░░░░░ ╰────╯ ░░░░░░░░░░░░░░░░░░│
│░░░░░ प्रोसेस हो रहा है... ░░░░░░░░░│  ← 16sp Regular, white
│░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
└──────────────────────────────────────┘
```

### Rules

1. **Skeleton for initial load. Always.** Never a blank screen. Never a centered spinner.
2. **Skeleton matches content layout.** Card widths, title bar width, body bar count.
3. **Skeleton appears within 100ms.** If API responds within 300ms, skip skeleton (show content directly).
4. **Show 3-5 skeleton items** regardless of expected list length.
5. **Pull-to-refresh: T.Brand.primary (#D9008D).** Never Material default blue.
6. **Refresh never replaces existing content.** Spinner over existing data.
7. **Processing overlay for write actions.** Scrim + spinner + Hindi text. Blocks double-tap.
8. **Button loading: spinner replaces text, button disabled.** Same dimensions.
9. **Shimmer: T.Stroke.primary (#E8E4F0) base, T.Bg.surface (#FFFFFF) highlight.** 1500ms loop.

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| Blank white screen during API call | Partner thinks app is frozen | Skeleton shimmer matching content layout |
| Centered spinner for initial load | No structural preview, feels slow | Skeleton shows content structure |
| Generic full-width shimmer block | No hint of what will appear | Varied widths matching actual text lines |
| Replacing content with skeleton on refresh | Layout shift, hides known data | Pull-to-refresh spinner over existing content |
| Material default blue refresh spinner | Off-brand, breaks visual trust | T.Brand.primary (#D9008D) |
| No text on processing overlay | "Is it working or frozen?" | Spinner + "प्रोसेस हो रहा है..." |
| Button text unchanged during API call | Invites double-tap, duplicate requests | Spinner replaces text, button disabled |

---

## 03 Error states

> **[Book: Forgiveness, p43]** "Designs should help users avoid errors and minimize
> negative consequences." The system failed, not the user. The error state must make
> recovery obvious and effortless.

### The rule: inline, Hindi, retry

Every error is:
1. **Inline** — never toast/snackbar (auto-dismiss = error disappears before user reads it)
2. **In Hindi** — English errors are invisible to Annu
3. **Equipped with retry** — every retriable error has "फिर से कोशिश करें"

### Three error patterns

**Full-screen error** (entire screen failed to load):

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│                                      │
│         ┌──────────────┐             │
│         │  ○ 96x96px   │  ← T.Bg.negative (#FFEBEE) circle
│         │  ✕ icon 48px │    T.State.negative (#C62828) icon
│         └──────────────┘             │
│           ↕ 16px                     │
│         कनेक्शन में दिक्कत है         │  ← 16sp Bold, T.State.negative
│           ↕ 8px                      │
│         इंटरनेट चेक करें और           │  ← 14sp Regular, T.Text.primary
│         फिर से कोशिश करें            │    (NOT T.State.negative — not all red)
│           ↕ 24px                     │
│         [फिर से कोशिश करें]          │  ← Secondary CTA
│                                      │
├──────────────────────────────────────┤
│  [Bottom Nav]                        │
└──────────────────────────────────────┘
```

**Inline error card** (part of screen failed):

```
┌──────────────────────────────────────┐
│ 3px │ ⚠ कनेक्शन में दिक्कत है      │  ← accent stripe T.State.negative
│ red │   [फिर से कोशिश करें]         │    bg T.Bg.negative (#FFEBEE)
│strip│                                │    radius: radius-main-card (16px)
└──────────────────────────────────────┘
```

**Connection banner** (persistent, transient condition):

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│ ⚠ इंटरनेट बंद है — ऑफ़लाइन डेटा    │  ← T.Bg.negative, T.State.negative icon
│   दिख रहा है                        │    auto-dismiss when condition resolves
├──────────────────────────────────────┤
│  [Content continues below]           │
└──────────────────────────────────────┘
```

### Error copy templates (Hindi)

| Error type | Headline | Explanation | CTA |
|---|---|---|---|
| Network | कनेक्शन में दिक्कत है | इंटरनेट चेक करें और फिर से कोशिश करें | फिर से कोशिश करें |
| Timeout | जवाब आने में देर हो रही है | नेटवर्क धीमा हो सकता है | फिर से कोशिश करें |
| Server (5xx) | कुछ गड़बड़ हो गई | हम इसे ठीक कर रहे हैं — थोड़ी देर में फिर कोशिश करें | फिर से कोशिश करें |
| Partial load | [section] लोड नहीं हो पाया | [section] के लिए फिर से कोशिश करें | फिर से कोशिश करें |
| Payment failed | पेमेंट नहीं हो पाई | पैसे नहीं कटे हैं — फिर से कोशिश करें | फिर से कोशिश करें |
| Submit failed | अनुरोध नहीं भेजा जा सका | नेटवर्क चेक करें और फिर से भेजें | फिर से भेजें |

### Rules

1. **Always inline. Never toast/snackbar.** Toasts auto-dismiss. Error disappears. Retry path gone.
2. **Every retriable error has a retry CTA.** "फिर से कोशिश करें." Fires same API call.
3. **Non-retriable errors have an exit path.** "वापस जाएं" or "होम पर जाएं." Never a dead end.
4. **Never blame the user.** "कनेक्शन में दिक्कत है" not "आपका इंटरनेट नहीं चल रहा."
5. **Error headline: T.State.negative. Body: T.Text.primary.** Never all-red text.
6. **Timeout: 15 seconds.** No API call waits forever.
7. **Partial failure: show what loaded + inline error for what failed.**
8. **Form errors: preserve all valid input.** Only highlight errored fields. Scroll to first error.

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| Toast/snackbar for errors | Error disappears in 3 seconds, no retry | Inline persistent error card |
| "Something went wrong" (English) | Invisible to Hindi-first user | Hindi, specific to error type |
| All-red error text | Hard to read, feels aggressive | Red headline + dark body text |
| Error with no retry CTA | Dead end, must navigate away | Always include retry button |
| Clearing form on submit error | Punishes user for system failure | Preserve all valid fields |
| Waiting forever for API | "Is the app frozen?" | 15-second timeout with retry |
| "आपका इंटरनेट नहीं चल रहा" | Blames user for infrastructure failure | Neutral: "कनेक्शन में दिक्कत है" |

---

## 04 Offline states

> **[Book: Constraint, p36]** "Limiting actions to reduce errors." Offline mode
> constrains the UI to read-only cached data and clearly disables write actions,
> preventing the user from attempting operations that will silently fail.

### Two offline modes

| Mode | What shows | When |
|---|---|---|
| **Cached data available** | Content + connection banner + disabled write CTAs | Internet gone but app has cached data |
| **No cache available** | Full-screen offline state (illustration + "इंटरनेट नहीं है") | Internet gone AND no cached data |

### Cached data + connection banner

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│ ☁✕ ऑफ़लाइन — 5 मिनट पहले का डेटा   │  ← T.Bg.info (#E3F2FD)
│    दिख रहा है                        │    T.State.info icon + text
├──────────────────────────────────────┤
│  ┌──────────────────────────────┐    │
│  │ Device Card 1 (cached)       │    │  ← normal card, alpha 1.0 if <1hr
│  └──────────────────────────────┘    │
│  ┌──────────────────────────────┐    │
│  │ Device Card 2 (cached)       │    │  ← alpha 0.7 if >1hr stale
│  └──────────────────────────────┘    │
├──────────────────────────────────────┤
│  [ CTA — disabled #A7A1B2 ]         │  ← Write CTA greyed out
│  (tap shows: "इंटरनेट आने पर        │
│   यह काम कर पाएंगे")               │
├──────────────────────────────────────┤
│  [Bottom Nav]                        │
└──────────────────────────────────────┘
```

### No cache + full-screen offline

```
┌──────────────────────────────────────┐
│  [Header — 48px]                     │
├──────────────────────────────────────┤
│                                      │
│         ┌──────────────┐             │
│         │  ○ 96x96px   │  ← T.Bg.info circle
│         │  ☁✕ icon     │    T.State.info icon
│         └──────────────┘             │
│           ↕ 16px                     │
│         इंटरनेट नहीं है               │  ← 16sp Bold, T.Text.primary
│           ↕ 8px                      │
│         इंटरनेट आने पर               │  ← 14sp Regular, T.Text.secondary
│         अपने-आप लोड होगा             │
│                                      │
├──────────────────────────────────────┤
│  [Bottom Nav]                        │
└──────────────────────────────────────┘
```

### Rules

1. **Detect offline at repository layer, not UI.** `ConnectivityManager` + `NetworkCallback`. Expose `StateFlow<Boolean>`.
2. **Connection banner: T.Bg.info (#E3F2FD).** Not error red. Offline is a condition, not an error.
3. **Relative timestamp in banner.** "5 मिनट पहले", "1 घंटे पहले." Never absolute UTC.
4. **Write CTAs disabled when offline.** Background T.Cta.primaryDisabledBg (#A7A1B2). Tap shows snackbar.
5. **Stale cache (>1hr): reduce opacity to 0.7f.** Subtle signal that data may not be current.
6. **Cache lifetime: 24 hours.** After 24hr: "पुराना डेटा — इंटरनेट आने पर अपडेट होगा."
7. **On reconnection: auto-retry pending syncs.** Snackbar: "कनेक्शन वापस आ गया." Remove banner.
8. **Sync pending badge on affected items.** Icon: sync, tint T.State.caution. Text: "भेजा जा रहा है..."
9. **No offline treatment for screens with no network dependency.** Settings, Help/FAQ work fully offline.

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| Spinner that never resolves offline | "Is the app frozen?" | Immediate offline detection, show cached or offline screen |
| Write action silently fails | Ghost data user trusts but server never received | Disabled CTA + snackbar explaining why |
| "No internet" in English | Invisible to Annu | "इंटरनेट नहीं है" in Hindi |
| Hiding all content when offline | Useful cached data wasted | Show cached data + connection banner |
| No timestamp on cached data | User can't assess freshness | Relative timestamp in banner |
| Full opacity on 2-hour-old data | User trusts stale data for decisions | Reduce to 0.7 alpha after 1 hour |
| Offline state on Settings/Help | Unnecessary alarm | No treatment for screens without network dependency |

---

## 05 Transition rules

### State transition map

```
LOADING → SUCCESS        fade skeleton out (200ms), fade content in (300ms, stagger 50ms/item)
LOADING → EMPTY          fade skeleton out (200ms), fade empty state in (300ms)
LOADING → ERROR          fade skeleton out (200ms), fade error in (300ms)
LOADING → OFFLINE        immediate swap (no animation — urgency)
SUCCESS → LOADING(refr)  show LinearProgressIndicator below header (fade in 200ms)
SUCCESS → ERROR(refr)    show banner error (slide down 300ms) — keep existing content
SUCCESS → OFFLINE        show connection banner (slide down 300ms) — keep existing content
OFFLINE → ONLINE         remove banner (slide up 200ms), auto-refresh
ERROR   → LOADING(retry) show spinner on retry CTA (button loading pattern)
ERROR   → SUCCESS        fade error out (200ms), fade content in (300ms)
```

### Interaction with visual hierarchy

| Visual hierarchy rule | How states interact |
|---|---|
| 3-second scan test | Every state must pass. Skeleton provides structural preview. Error provides icon + headline + retry. |
| ONE focal point | Loading: shimmer animation. Error: retry CTA. Empty: action CTA or illustration. Offline: connection banner. |
| 5-level type scale | Error headline: L2 (16sp Bold). Error body: L4 (14sp Regular). Empty headline: L2. Banner: L4. |
| Max 1 P1 CTA | Error retry = P2 (secondary). Empty action = P2. P1 slot reserved for success state. |
| Density decreases with consequence | Error/empty states are spacious (large illustration, generous padding). Loading matches normal density. |

---

## Review checklist

### Loading (1-4)

| # | Check | Pass criteria |
|---|---|---|
| 1 | Skeleton exists for initial load? | Never blank white screen. Skeleton matches content layout. |
| 2 | Skeleton appears within 100ms? | No delay. Skip if API < 300ms. |
| 3 | Pull-to-refresh uses T.Brand.primary? | Not Material default blue. Doesn't replace existing content. |
| 4 | Processing overlay for write actions? | Scrim + spinner + Hindi text. Button shows spinner during call. |

### Empty (5-7)

| # | Check | Pass criteria |
|---|---|---|
| 5 | Every list has empty state composable? | No LazyColumn without zero-items path. |
| 6 | Empty text is screen-specific and Hindi? | Not generic "कोई डेटा नहीं." Names the entity. |
| 7 | First-time-use uses brand styling, not error? | Pink circle, encouraging tone. Not red. |

### Error (8-11)

| # | Check | Pass criteria |
|---|---|---|
| 8 | Errors are inline and persistent? | No toast/snackbar. Error visible until resolved. |
| 9 | Every retriable error has retry CTA? | "फिर से कोशिश करें" button fires API call. |
| 10 | Error copy is Hindi, specific, blame-free? | Not English. Not "Something went wrong." Not user-blaming. |
| 11 | API timeout set to 15 seconds? | No call waits forever. Timeout → specific error + retry. |

### Offline (12-15)

| # | Check | Pass criteria |
|---|---|---|
| 12 | Offline detected via ConnectivityManager? | Shows immediately, not after failed request. |
| 13 | Cached screens show banner + cached content? | Banner has relative timestamp. Write CTAs disabled. |
| 14 | No-cache screens show full-screen offline? | Not spinner. Not error. Specific offline illustration + text. |
| 15 | Reconnection triggers auto-retry? | Pending syncs retry. Banner dismissed. Snackbar confirms. |

---

## Quick reference: state selection flowchart

```
Screen entered →
  Check connectivity
    OFFLINE →
      Cache exists?
        YES → Show cached content + connection banner + disable write CTAs
        NO  → Full-screen offline: "इंटरनेट नहीं है"
    ONLINE →
      Fire API → show skeleton
        Response < 300ms → skip skeleton, show content
        Response > 300ms → skeleton visible until response
          Success + data  → Success state
          Success + empty → Empty state (pick variant)
          Failure + cache → Show cache + inline error banner
          Failure + no cache → Full-screen error + retry
          No response > 15s → Timeout error + retry
```

---

*CA Design System — DS States Skill — April 2026*
*Source: Universal Principles of Design (Lidwell, Holden, Butler, 2003)*
*Depends on: wiom-android-layout (tokens), wiom-visual-skill (hierarchy)*


---
---

# DOMAIN C — Accessibility

## Six areas at a glance

| # | Area | Governs | Read when |
|---|---|---|---|
| 01 | [Color Contrast](#01-color-contrast) | Text/icon visibility against backgrounds | "Can't read this in sunlight", badge text invisible on tinted bg |
| 02 | [Touch Targets](#02-touch-targets) | Tappable area size and spacing | "Button too small", mis-taps on budget screens |
| 03 | [Screen Reader](#03-screen-reader-talkback) | TalkBack announcements, semantic order | "TalkBack reads disconnected text", English state names in Hindi app |
| 04 | [Focus Navigation](#04-focus-navigation) | Keyboard/switch access traversal | "Can't see where focus is", focus escapes dialog |
| 05 | [Text Scaling](#05-text-scaling) | Response to user font size preference | "Text clips at large font", card overflows |
| 06 | [Motion](#06-motion-and-reduced-motion) | Respecting prefers-reduced-motion | "Animations play with Remove Animations enabled" |

---

## Why this matters for Wiom

Partners are NOT in a climate-controlled office on a Pixel 7.

- **Sunlight:** 3.8:1 contrast disappears outdoors. Budget 720p screens have lower brightness than flagships.
- **Sweaty hands + screen protectors:** 36px (5mm) targets are unreliable. ISO recommends 7mm minimum (48px).
- **Hindi TalkBack:** Reads Devanagari correctly but announces "DEPLOYED" as English gibberish. Must be "ग्राहक के पास."
- **Large font settings:** Semi-literate partners often set 1.3-1.5x font. Fixed-height cards clip text.
- **Budget CPU:** Animations >300ms cause frame drops. "Remove animations" toggle must be respected.

Every accessibility failure is a support call. Every support call costs money.

---

## 01 Color Contrast

> **[Book: Legibility, p124]** "Visual clarity of text — size, typeface, contrast, spacing."
> On a budget 720p screen in sunlight, contrast below 4.5:1 is functionally invisible.

### Contrast audit — all token pairs

**Text on backgrounds:**

| Foreground | Background | Ratio | Required | Verdict |
|---|---|---|---|---|
| `T.Text.primary` #161021 | `T.Bg.surface` #FFFFFF | 16.5:1 | 4.5:1 | PASS |
| `T.Text.primary` #161021 | `T.Bg.screen` #F5F6F7 | 14.8:1 | 4.5:1 | PASS |
| `T.Text.secondary` #5C5469 | `T.Bg.surface` #FFFFFF | 6.5:1 | 4.5:1 | PASS |
| `T.Text.secondary` #5C5469 | `T.Bg.screen` #F5F6F7 | 5.9:1 | 4.5:1 | PASS |
| `T.Text.hint` #A7A1B2 | `T.Bg.surface` #FFFFFF | 2.9:1 | N/A | Disabled only — exempt |
| `T.Text.inverse` #FFFFFF | `T.Brand.secondary` #443152 | 8.9:1 | 4.5:1 | PASS |

**State colors on state backgrounds (FAILURES FOUND):**

| Foreground | Background | Ratio | Required | Verdict | Fix |
|---|---|---|---|---|---|
| `T.State.caution` #E65100 | `T.Bg.urgent` #FFF3E0 | 3.8:1 | 4.5:1 | **FAIL** | Use `T.State.cautionText` #BF4000 (5.2:1) |
| `T.State.positive` #2E7D32 | `T.Bg.positive` #E8F5E9 | 4.1:1 | 4.5:1 | **FAIL** | Use `T.State.positiveText` #1B5E20 (5.8:1) |
| `T.State.negative` #C62828 | `T.Bg.negative` #FFEBEE | 5.6:1 | 4.5:1 | PASS | — |
| `T.State.info` #0D47A1 | `T.Bg.info` #E3F2FD | 7.1:1 | 4.5:1 | PASS | — |

**CTA pairs:**

| Foreground | Background | Ratio | Required | Verdict | Fix |
|---|---|---|---|---|---|
| #FFFFFF | `T.Cta.primaryBg` #D9008D | 4.2:1 | 3:1 (large bold) | PASS at >=16sp Bold only | CTA label MUST be >=16sp Bold |
| `T.Cta.secondaryText` #D9008D | `T.Cta.secondaryBg` #FFE5F6 | 3.7:1 | 3:1 (large bold) | PASS at >=16sp Bold only | Same: >=16sp Bold mandatory |

### New tokens required

```kotlin
// Add to WiomTokens.kt — accessible text variants for state-on-background
val State.cautionText = Color(0xFFBF4000)   // 5.2:1 on T.Bg.urgent
val State.positiveText = Color(0xFF1B5E20)  // 5.8:1 on T.Bg.positive
```

### Rules

1. **Normal text (<18sp Bold):** minimum 4.5:1 against its direct background.
2. **Large text (>=18sp Bold or >=24sp Regular):** minimum 3:1. CTA at 16sp Bold qualifies.
3. **State text on state backgrounds:** always use `*Text` variant. Base state color is for icons on neutral bg only.
4. **Never rely on color alone.** Every status must combine color + icon + text label.
5. **UI components (icons, borders):** minimum 3:1. If a border fails 3:1, it must not be the sole indicator.

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| `T.State.caution` text on `T.Bg.urgent` | Carry fee text invisible in sunlight (3.8:1) | Use `T.State.cautionText` #BF4000 |
| `T.State.positive` text on `T.Bg.positive` | "चालू" badge barely visible (4.1:1) | Use `T.State.positiveText` #1B5E20 |
| <16sp text on magenta CTA | Small text fails 4.5:1 on #D9008D | CTA labels always >=16sp Bold |
| Color-only status differentiation | Color-blind partner can't distinguish states | Always add icon + text label |

---

## 02 Touch Targets

> **[Book: Fitts' Law]** "Time to reach a target is a function of distance and size."
> On a budget phone with screen protector and sweaty hands, a 36px target (5mm)
> requires precision most partners don't have in field conditions.

### Touch target audit — all interactive elements

| Element | Current | Required | Verdict | Fix |
|---|---|---|---|---|
| Primary CTA (full-width) | 48px | 48px | PASS | — |
| Primary CTA (in card) | 40px | 44px | **FAIL** | Increase to 44px |
| Inline row CTA | 32px | 44px | **FAIL** | Add touch padding via `minimumInteractiveComponentSize()` |
| Filter pill | 36px | 44px | **FAIL** | Increase to 44px |
| Timeline row | 36px | 44px | **FAIL** | Increase to 44px |
| Menu item row | 48px | 48px | PASS | — |
| Batch row | 44px | 44px | PASS | — |
| Option card | 56px | 48px | PASS | — |
| Header icons | 48px | 48px | PASS | — |
| Bottom sheet drag handle | ~20px visible | 44px touch | **FAIL** | Wrap in 44px transparent touch target |
| Checkbox / Radio | 20px default | 48px | **FAIL** | Use `minimumInteractiveComponentSize()` |

### Token updates required

```kotlin
// Updated in WiomTokens.kt
val Size.listCardCtaHeight = 44.dp     // was 40 — now meets minimum
val Size.togglePillHeight = 44.dp      // was 36 — now meets minimum
val Size.timelineRowHeight = 44.dp     // was 36 — now meets minimum
```

### Rules

1. **All tappable elements: minimum 44px in both dimensions.** Prefer 48px.
2. **Visual size can be smaller than touch area.** A 32px button can have 44px touch target using transparent padding.
3. **Adjacent targets: minimum 8px gap** (T.Space.sm) between edges.
4. **Use `minimumInteractiveComponentSize()`** on all small interactive elements (checkboxes, radios, icon buttons).

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| 36px filter pill | Mis-taps in field conditions | Increase to 44px |
| 32px inline CTA without touch expansion | Partner taps wrong element | Add `minimumInteractiveComponentSize()` |
| Two tappable elements 4px apart | Guaranteed mis-taps | Minimum 8px gap |
| 20px drag handle | Can't grab bottom sheet | Wrap in 44px transparent target |

---

## 03 Screen Reader (TalkBack)

> **[Book: Visibility, p202]** "Status and possible actions must be clearly visible."
> For TalkBack users, "visible" means "announced." If TalkBack doesn't announce it,
> it doesn't exist.

### Heading structure

Every screen must have heading hierarchy for TalkBack navigation:

```kotlin
// Screen title — primary heading
Text(
    text = "मेरे NetBox",
    modifier = Modifier.semantics { heading() }
)

// Section within screen — also heading
Text(
    text = "वापसी बाकी (3)",
    modifier = Modifier.semantics { heading() }
)
```

### Content grouping

Cards must announce as ONE coherent unit, not 5 disconnected text nodes:

```kotlin
// Card announces: "NetBox NB-0042, ग्राहक Sharma Ji के पास, कैरी फ़ीस ₹150 बाकी"
Card(
    modifier = Modifier.semantics(mergeDescendants = true) {
        contentDescription = "NetBox NB-0042, ग्राहक Sharma Ji के पास, कैरी फ़ीस ₹150 बाकी"
    }
) {
    // child Text nodes merged into parent announcement
}

// Button inside card announces separately with context
Button(
    modifier = Modifier.semantics {
        contentDescription = "NetBox NB-0042 वापस करें"
    }
) { Text("वापस करें") }
```

### contentDescription rules

| Element | Rule | Example |
|---|---|---|
| Decorative icon | `contentDescription = null` | Wiom logo, divider icon |
| Meaningful icon | Describe the meaning in Hindi | "सफल" not "green checkmark" |
| Icon button (no label) | Describe the action in Hindi | "मेनू खोलें" not "hamburger" |
| Badge / chip | Include state meaning | "स्थिति: ग्राहक के पास" |
| Numeric value | Include unit + context | "कैरी फ़ीस: 150 रुपये" not "150" |
| Toggle / switch | Include current state | "हिंदी भाषा, चालू है" |

### Live regions (dynamic content)

```kotlin
// Carry fee total changes — polite announcement
Text(
    text = "कुल कैरी फ़ीस: ₹${total}",
    modifier = Modifier.semantics {
        liveRegion = LiveRegionMode.Polite
    }
)

// Error appears — assertive announcement
Text(
    text = errorMessage,
    modifier = Modifier.semantics {
        liveRegion = LiveRegionMode.Assertive
        error(errorMessage)
    }
)
```

| When | LiveRegionMode | Why |
|---|---|---|
| Fee total updates | Polite | Informational, not urgent |
| List count refreshes | Polite | Context update |
| Error appears | Assertive | Must hear immediately |
| Form validation fails | Assertive | Blocks progress |
| Success result shown | Polite | Good news can wait |

### Rules

1. **Every screen: exactly ONE primary heading** (screen title with `semantics { heading() }`).
2. **Section dividers: also headings.** Enables TalkBack heading navigation.
3. **Cards: `mergeDescendants = true`** with meaningful Hindi `contentDescription`.
4. **All meaningful icons: Hindi contentDescription.** Decorative icons: `null`.
5. **Never announce English in a Hindi app.** "DEPLOYED" → "ग्राहक के पास."
6. **Errors: Assertive live region.** Content updates: Polite live region.

### What goes wrong

| Mistake | Looks like | Fix |
|---|---|---|
| No `mergeDescendants` on cards | TalkBack reads 5 disconnected strings per card | Add `semantics(mergeDescendants = true)` |
| No headings | TalkBack user must swipe through all 50 cards linearly | Mark titles with `heading()` |
| English state names in announcements | "DEPLOYED" in Hindi pronunciation = meaningless | Hindi: "ग्राहक के पास" |
| `contentDescription` on decorative icons | TalkBack noise on every divider icon | Set `contentDescription = null` |
| No live region on error | Error appears but TalkBack doesn't announce | `LiveRegionMode.Assertive` |

---

## 04 Focus Navigation

> **[Book: Constraint, p36]** "Limiting actions to reduce errors."
> Focus management constrains navigation to a predictable, logical path.

### Focus order

Must match visual reading order: left-to-right, top-to-bottom (LTR for Devanagari).

**List screen:** Header actions → Filter pills (L-R) → Card 1 → Card 1 CTA → Card 2 → ... → Bottom nav tabs
**Form screen:** Back arrow → Input 1 → Input 2 → ... → Primary CTA → Secondary CTA
**Dialog:** Primary CTA → Secondary CTA → wraps back (focus trapped)

### Focus indicator

`T.Stroke.brand` (#FFB2E4) at 1.9:1 on white is invisible. Use `T.Brand.primary` (#D9008D) instead:
- On `T.Bg.surface`: 4.2:1 → PASS
- On `T.Bg.screen`: 3.2:1 → PASS (3:1 required for UI components)
- Width: 2px. Shape: matches component radius.

### Focus trap for dialogs

```kotlin
Card(
    modifier = Modifier
        .focusGroup()  // traps focus within dialog
        .semantics { paneTitle = "डायलॉग" }
) { content() }
```

### Rules

1. **Focus order = visual reading order.** Top-to-bottom, left-to-right, CTA last.
2. **Focus indicator: 2px T.Brand.primary (#D9008D).** Not T.Stroke.brand.
3. **Dialogs and bottom sheets trap focus.** Tab cycles within until dismissed.
4. **Form IME actions chain fields.** `imeAction = ImeAction.Next` → `focusRequester.requestFocus()`.

---

## 05 Text Scaling

Android users can set font scale from 0.85x to 2.0x. Partners with low vision use 1.3-1.5x.

### Rules

1. **Always `sp` for text. Never `dp` for text.** (Already convention — don't regress.)
2. **Never `Modifier.height()` on text containers.** Use `defaultMinSize(minHeight = X)` instead.
3. **CTA labels: `maxLines = 1, overflow = Ellipsis`.** Single-line at all scales.
4. **Test every screen at 1.0x, 1.3x, and 2.0x** using `@Preview(fontScale = 1.5f)`.

### What breaks at 1.5x

| Component | Behavior | Handling |
|---|---|---|
| Card | Grows vertically | Use `minHeight`, not `height` |
| List row | Grows if text wraps | Use `minHeight` |
| CTA button | Grows vertically | Single-line text, `minHeight = 48px` |
| App bar | Fixed 48px | Title truncates with ellipsis |
| Bottom nav | Fixed 56px | Labels truncate |

---

## 06 Motion and Reduced Motion

Android "Remove animations" toggle must be respected.

### Rules

1. **Check `ANIMATOR_DURATION_SCALE == 0f`** before decorative animations.
2. **Essential animations exempt:** progress bars, loading spinners, pull-to-refresh.
3. **All UI transitions: <=300ms.** Longer = frame drops on budget CPU.
4. **No auto-playing motion covering >1/3 of screen** unless reduced motion is off.

### Duration reference

| Animation | Standard | Reduced motion |
|---|---|---|
| Screen transition | 200ms fade + slide | Instant |
| Card expand | 200ms | Instant |
| Bottom sheet | 250ms | Instant |
| Loading spinner | Continuous | Same (essential) |
| Progress bar | Matches progress | Same (essential) |
| Success checkmark | 300ms draw | Instant (show final state) |

---

## Review checklist

### Contrast (1-3)

| # | Check | Pass criteria |
|---|---|---|
| 1 | Normal text >=4.5:1 on its background? | Measure every foreground/background pair |
| 2 | State text uses `*Text` variant on state bg? | `cautionText` on urgent, `positiveText` on positive |
| 3 | CTA label is >=16sp Bold? | Never <16sp on T.Cta.primaryBg |

### Touch (4-6)

| # | Check | Pass criteria |
|---|---|---|
| 4 | Every tappable element >=44px? | Layout Inspector or Modifier audit |
| 5 | Adjacent targets >=8px apart? | No mis-tap-prone arrangements |
| 6 | Small elements use `minimumInteractiveComponentSize()`? | Checkboxes, radios, icon buttons |

### TalkBack (7-10)

| # | Check | Pass criteria |
|---|---|---|
| 7 | Screen has exactly one primary heading? | Title has `semantics { heading() }` |
| 8 | Cards use `mergeDescendants` with Hindi description? | Card announces as one coherent unit |
| 9 | Meaningful icons have Hindi contentDescription? | No English state names announced |
| 10 | Errors use Assertive live region? | Error announced immediately on appear |

### Focus + Scaling + Motion (11-15)

| # | Check | Pass criteria |
|---|---|---|
| 11 | Visible focus indicator (2px T.Brand.primary)? | Tab through with Bluetooth keyboard |
| 12 | Focus order matches visual order? | Top-to-bottom, L-to-R, CTA last |
| 13 | Dialogs trap focus? | Tab cycles within dialog |
| 14 | Screen correct at 1.5x font scale? | No clipping, no overflow, CTA visible |
| 15 | Decorative animations off with "Remove animations"? | Essential animations still work |

---

## Quick reference: accessibility fixes needed

```
TOKEN ADDITIONS:
  T.State.cautionText  = #BF4000  (replaces #E65100 on T.Bg.urgent)
  T.State.positiveText = #1B5E20  (replaces #2E7D32 on T.Bg.positive)

TOKEN UPDATES:
  T.Size.listCardCtaHeight  = 44px  (was 40)
  T.Size.togglePillHeight   = 44px  (was 36)
  T.Size.timelineRowHeight  = 44px  (was 36)

FOCUS INDICATOR:
  2px border, T.Brand.primary (#D9008D)
  Not T.Stroke.brand (#FFB2E4 — fails at 1.9:1)

CTA CONSTRAINT:
  Label text MUST be >=16sp Bold on T.Cta.primaryBg
  White on magenta only passes at large bold text (4.2:1 < 4.5:1)
```

---

*CA Design System — DS Accessibility Skill — April 2026*
*Standard: WCAG 2.1 AA minimum*
*Source: Universal Principles of Design (Lidwell, Holden, Butler, 2003)*
*Depends on: wiom-android-layout (tokens), wiom-visual-skill (hierarchy), wiom-ds-states (state announcements)*


---
---

# DOMAIN D — Forms

## Seven areas at a glance

| # | Area | Governs | Read when |
|---|---|---|---|
| 01 | [Field Anatomy](#01-field-anatomy) | Label, input, helper, error, spacing | "What spacing between label and input?" |
| 02 | [Input Types](#02-input-types) | Keyboard type, IME action, format per field | "Wrong keyboard shows up", "OTP input broken" |
| 03 | [Validation](#03-validation) | When to validate, error display, Hindi messages | "Error unclear", "form clears on error" |
| 04 | [Multi-Step Forms](#04-multi-step-forms) | Step indicator, progress, data persistence | "Form clears on back press", "no review before payment" |
| 05 | [Selection Patterns](#05-selection-patterns) | Radio cards, checkbox, dropdown, bottom sheet | "Dropdown opens keyboard", "can't tell what's selected" |
| 06 | [Form Layout](#06-form-layout) | Field spacing, section grouping, CTA placement | "CTA hidden behind keyboard", "fields too cramped" |
| 07 | [Keyboard](#07-keyboard-interaction) | Keyboard type, IME chaining, CTA visibility | "Keyboard covers button", "have to tap each field manually" |

---

## 01 Field Anatomy

Every input field in the app uses this exact vertical stack. No variation.

### The structure

```
┌──────────────────────────────────────────────┐
│ Label                        Character count │  14sp SemiBold, T.Text.primary
│                                              │  (T.Text.hint when disabled)
├── 4px gap (T.Space.xs) ─────────────────────┤
│ ┌──────────────────────────────────────────┐ │
│ │ [icon]  input text / placeholder  [trail]│ │  Height: 48px min
│ └──────────────────────────────────────────┘ │  Radius: T.Radius.card (12px)
├── 4px gap (T.Space.xs) ─────────────────────┤
│ Helper text  OR  Error text                  │  12sp Regular
│                                              │  Helper: T.Text.secondary
│                                              │  Error: T.State.negative
└──────────────────────────────────────────────┘
```

### Field states

| State | Border | Width | Background | Placeholder |
|---|---|---|---|---|
| Default | `T.Stroke.secondary` #D7D3E0 | 1px | `T.Bg.surface` #FFFFFF | `T.Text.secondary` |
| Focused | `T.Stroke.brand` #FFB2E4 | 2px | `T.Bg.surface` | — |
| Error | `T.State.negative` #C62828 | 1px | `T.Bg.negative` #FFEBEE | — |
| Disabled | `T.Stroke.primary` #E8E4F0 | 1px | `T.Bg.disabled` #D7D3E0 | `T.Text.hint` |

Three unbreakable rules:
1. **Placeholder = `T.Text.secondary`, NOT `T.Text.hint`.** Hint = disabled. Placeholder = active prompt.
2. **Focus border = 2px.** The 1→2px jump is the visual signal the field is active.
3. **Error = background AND border.** Red border alone is a hairline on a 5-inch screen.

### Spacing between fields

| Gap | Token | Between |
|---|---|---|
| 4px | `T.Space.xs` | Label → input, input → helper/error |
| 12px | `T.Space.md` | One complete field → next field's label |
| 24px | `T.Space.xxl` | Last field in section → next section header |
| 12px | `T.Space.md` | Section header → first field |

### What goes wrong

| Mistake | Fix |
|---|---|
| Using `OutlinedTextField` from Material 3 | Use `BasicTextField` with custom `decorationBox` for full state control |
| Placeholder in `T.Text.hint` | Use `T.Text.secondary` — hint looks disabled |
| Error with border only, no background | Add `T.Bg.negative` fill — border alone invisible in sunlight |
| 16px between label and input | 4px (`T.Space.xs`) — 16px wastes screen space |
| Field height < 48px | Always `heightIn(min = 48.dp)` — touch target |

---

## 02 Input Types

### Type map

| Input | keyboardType | imeAction | Max length | Screens |
|---|---|---|---|---|
| Text (name) | `Text` | `Next` | 50 | KYC, Profile |
| Phone | `Phone` | `Next` | 10 | ISP Recharge, KYC |
| Amount (₹) | `Number` | `Done` | 7 | ISP Recharge |
| OTP | `NumberPassword` | `Done` | 4 or 6 | OTP Verification |
| Email | `Email` | `Next` | 100 | KYC |
| Pincode | `Number` | `Next` | 6 | KYC Address |
| Aadhaar | `Number` | `Next` | 12 | KYC |
| Search | `Text` | `Search` | 100 | Search |
| Textarea | `Text` | `Default` | 500 | Return (notes) |
| Dropdown | N/A — opens bottom sheet | N/A | N/A | Return reason, Plan Change |

### OTP input — special component

```
┌───┐ 8px ┌───┐ 8px ┌───┐ 8px ┌───┐ 8px ┌───┐ 8px ┌───┐
│ 4 │     │ 7 │     │ 2 │     │ _ │     │   │     │   │
└───┘     └───┘     └───┘     └───┘     └───┘     └───┘
 48px wide each
 Active: T.Stroke.brand 2px border
 Filled: T.Stroke.secondary 1px, T.Text.primary text
 Error: ALL boxes get T.State.negative border + T.Bg.negative
 Auto-submit when all digits filled. Auto-dismiss keyboard.
```

### Amount input — Indian formatting

Leading "₹" icon. Digits only, max 7 characters (up to 99,99,999). `KeyboardType.Number` (not Phone — Phone shows +, *, #).

### What goes wrong

| Mistake | Fix |
|---|---|
| `KeyboardType.Text` for phone field | Shows QWERTY. Partner fumbles. Use `Phone`. |
| `ImeAction.Done` on non-last field | Keyboard dismisses. User taps next field manually. Use `Next`. |
| OTP as single text field | Partner can't see progress. Use per-digit boxes. |
| Dropdown opens keyboard | Dropdown is tap-to-select. Open bottom sheet, no keyboard. |
| No max length on amount | User enters absurd numbers. Set `maxLength = 7`. |

---

## 03 Validation

### When to validate

| Trigger | Use when | Why |
|---|---|---|
| **On blur** (focus leaves field) | ALL text inputs by default | Don't show errors while partner is still typing |
| **On submit** (tap CTA) | Form-level, cross-field rules | Catch anything missed on blur |
| **Real-time** | ONLY: OTP (auto-submit), amount (format), character count | Immediate feedback where delay causes confusion |
| **Never on keystroke** | Name, email, phone | Errors while typing = anxiety for semi-literate users |

### Validation flow

```
User types → onValueChange: clear existing error for THIS field only. No validation.

User taps next field → onFocusChanged(false) on previous field:
  Run field validation.
  If error: show error text + T.Bg.negative + T.State.negative border.
  If valid: restore Filled state.

User taps CTA:
  Run ALL field validations.
  If errors exist:
    1. Show ALL errors at once (not one at a time)
    2. Scroll to first errored field
    3. Focus first errored field
  If all valid: proceed.
```

### Hindi error templates

| Validation | Hindi error message |
|---|---|
| Required field empty | यह भरना ज़रूरी है |
| Invalid phone | 10 अंकों का फ़ोन नंबर डालें |
| Phone wrong start digit | सही फ़ोन नंबर डालें (6, 7, 8, या 9 से शुरू) |
| Invalid email | सही ईमेल डालें (जैसे name@example.com) |
| Wrong OTP | OTP गलत है। दोबारा डालें या नया OTP भेजें |
| OTP expired | OTP का समय ख़त्म हो गया। नया OTP भेजें |
| Amount too low | कम से कम ₹{min} डालें |
| Amount too high | ज़्यादा से ज़्यादा ₹{max} डाल सकते हैं |
| Invalid Aadhaar | 12 अंकों का आधार नंबर डालें |
| Invalid pincode | 6 अंकों का पिनकोड डालें |
| Name too short | कम से कम 2 अक्षर डालें |
| Notes too long | {max} अक्षरों से ज़्यादा नहीं लिख सकते |
| Dropdown not selected | एक विकल्प चुनें |
| Payment failed | पेमेंट नहीं हो पाई। दोबारा कोशिश करें |
| Server error | कुछ गड़बड़ हो गई। थोड़ी देर बाद दोबारा कोशिश करें |

### Error message rules

- **Always: what is wrong + what to do.** "10 अंकों का फ़ोन नंबर डालें" = specifies format.
- **Never blame the user.** "सही नंबर डालें" not "आपने गलत नंबर डाला."
- **Always use specific numbers.** "10 अंकों का" not "सही लंबाई का."
- **Field errors go under the field. Server errors go in inline error card above CTA.**

### What goes wrong

| Mistake | Fix |
|---|---|
| Validating on every keystroke | Partner sees red while still typing. Validate on blur. |
| Showing errors one at a time | Partner fixes one, submits, sees another. Show ALL at once. |
| Error clears on timer | Error should persist until user modifies the field. |
| Form clears on submit error | Preserve all valid input. Only highlight errored fields. |
| English error messages | "Invalid phone number" invisible to Hindi user. Use Hindi templates. |
| No scroll to first error | Partner doesn't see error below fold. Scroll + focus first error. |

---

## 04 Multi-Step Forms

### When to use multi-step

| Field count | Use |
|---|---|
| 1-4 fields | Single screen |
| 5-8 fields | Multi-step (1 group per step) |
| 9+ fields | Multi-step + progressive disclosure |
| Any with payment/submit | ALWAYS add review step before final action |

### Step indicator

```
  Step 1 of 3: व्यक्तिगत जानकारी

  (1)━━━━(2)━━━━(3)

  Done:    green circle + checkmark icon
  Active:  magenta circle + step number
  Pending: grey circle + step number
  Line:    T.Brand.primary (done) or T.Stroke.secondary (pending)
```

### Review screen (mandatory before payment/irreversible action)

```
┌──────────────────────────────────────┐
│ स्टेप 3/3: जानकारी चेक करें          │
│ (1)━━━━(2)━━━━(3)                    │
├──────────────────────────────────────┤
│ ┌──────────────────────────────────┐ │
│ │ व्यक्तिगत जानकारी      [बदलें] │ │  ← taps to step 1
│ │ नाम: अजय कुमार                  │ │
│ │ फ़ोन: 9876543210                  │ │
│ └──────────────────────────────────┘ │
│                                      │
│ ┌──────────────────────────────────┐ │
│ │ पता                    [बदलें] │ │  ← taps to step 2
│ │ 123 मेन रोड, पटना               │ │
│ │ पिनकोड: 800001                   │ │
│ └──────────────────────────────────┘ │
│                                      │
│ [ ═══════ सबमिट करें ═══════ ]       │
└──────────────────────────────────────┘
```

### Rules

1. **Back button decrements step. NEVER clears data.** Partner re-entering data is catastrophic.
2. **Forward validates only current step.** Don't validate future steps.
3. **Review screen MANDATORY before payment or irreversible submit.**
4. **Step label in header:** "स्टेप {current}/{total}."
5. **Store data in ViewModel, not `remember`.** ViewModel survives back navigation + config change.
6. **Persist across process death** via `SavedStateHandle`.

### What goes wrong

| Mistake | Fix |
|---|---|
| Data clears on back press | Store in ViewModel, not composable remember |
| No review before payment | Add review step — partner needs to verify before money moves |
| Validating all steps on forward | Only validate current step's fields |
| No step indicator | Partner doesn't know how much is left. Show step count. |

---

## 05 Selection Patterns

### Decision tree

```
How many options?
  1 (toggle)        → Switch or Checkbox
  2-3 (visual)      → Radio cards (side-by-side or stacked)
  4-5 (list)        → Bottom sheet picker
  6+ (searchable)   → Full-screen list with search
  Dynamic / server   → Dropdown that opens bottom sheet
```

### Radio cards (single select)

Used for: plan selection, payment method, return reason

```
Unselected:                     Selected:
┌───────────────────┐           ┌───────────────────┐
│ bg: T.Bg.surface  │           │ bg: T.Bg.brandTint│  #FFE5F6
│ border: 1px grey  │           │ border: 2px brand │  #D9008D
│                   │           │           [check] │
│ Plan name         │           │ Plan name         │
│ ₹299/month        │           │ ₹299/month        │
└───────────────────┘           └───────────────────┘
  Min height: 56px (T.Size.optionCardHeight)
```

### Multi-select (checkbox cards)

Same as radio cards but: allow multiple selected. Show count badge: "{n} चुने गए" above the list.

### Dropdown (bottom sheet picker)

Looks like a text field but taps open a bottom sheet. Trailing arrow-down icon. Never opens keyboard.

### What goes wrong

| Mistake | Fix |
|---|---|
| Native Android Spinner | Tiny target, no DS styling. Use bottom sheet. |
| Radio without card wrapper | Tap target is 20px radio circle. Use full-width tappable card. |
| No visual difference selected vs unselected | Use brand tint bg + 2px border on selected. |
| Dropdown opens keyboard | Dropdown = bottom sheet, not keyboard input. |

---

## 06 Form Layout

### Spacing rules

```
┌──────────────────────────────────────┐
│ Section header (16sp Bold)           │
│  ↕ 12px (T.Space.md)                │
│ [Field 1: label + input + helper]    │
│  ↕ 12px (T.Space.md)                │
│ [Field 2: label + input + helper]    │
│  ↕ 12px (T.Space.md)                │
│ [Field 3: label + input]             │
│  ↕ 24px (T.Space.xxl)               │  ← section break
│ Next section header                  │
│  ↕ 12px                             │
│ [Field 4]                            │
│  ↕ 24px                             │  ← before CTA
│ [CTA]                                │
└──────────────────────────────────────┘
  Horizontal padding: 16px (T.Space.lg) on both sides
```

### CTA placement

| Does user need CTA visible at all times? | Strategy |
|---|---|
| YES (payment, submit, action) | Pinned CTA above keyboard |
| NO (optional save, long form) | CTA scrolls with content |

### Screen map: which strategy

| Screen | Strategy | Why |
|---|---|---|
| ISP Recharge | Pinned | "₹X जमा करें" must always be visible |
| Plan Change | Pinned | "प्लान बदलें" must stay visible during selection |
| Return Flow | Scroll | Short form, CTA after notes is natural |
| Request NetBox | Pinned | "रिक्वेस्ट भेजें" is always-visible confirmation |
| KYC (multi-step) | Scroll per step | Each step is short, "आगे बढ़ें" after fields |
| Batch Operations | Pinned | Selection count + action must stay visible |
| OTP | Pinned | "सत्यापित करें" always visible |

---

## 07 Keyboard Interaction

### IME action chaining

```
Field 1 (name)     → ImeAction.Next   → focus moves to Field 2
Field 2 (phone)    → ImeAction.Next   → focus moves to Field 3
Field 3 (pincode)  → ImeAction.Done   → dismisses keyboard, validates all
```

Every non-last field: `ImeAction.Next`. Last field: `ImeAction.Done`.
Search field: `ImeAction.Search`.

### CTA visibility when keyboard is open

```
Strategy 1 (Scroll):              Strategy 2 (Pinned):
┌─────────────────┐               ┌─────────────────┐
│ Header          │               │ Header          │
├─────────────────┤               ├─────────────────┤
│ Field 1         │               │ Field 1         │
│ Field 2         │  scrolls      │ Field 2         │  scrolls
│ Field 3         │               │ Field 3         │
│ [CTA]           │               ├─────────────────┤
├─────────────────┤               │ [CTA]           │  FIXED
│ ▓▓ KEYBOARD ▓▓ │               ├─────────────────┤
└─────────────────┘               │ ▓▓ KEYBOARD ▓▓ │
                                  └─────────────────┘
```

### Rules

1. **Always set `keyboardType` explicitly.** Never rely on default.
2. **Chain fields with `ImeAction.Next`.** Last field: `Done`.
3. **Never mix keyboard strategies on one screen.**
4. **`adjustResize` in manifest.** Never `adjustPan`.
5. **Dismiss keyboard before opening bottom sheet.** Otherwise sheet opens behind keyboard.
6. **Use `bringIntoViewRequester` on focus.** Field must be visible when focused.

### What goes wrong

| Mistake | Fix |
|---|---|
| No `imePadding()` | Keyboard covers input and CTA. Add `imePadding()`. |
| Mixed strategies | Layout jumps in two directions. Pick ONE. |
| `adjustPan` in manifest | Content pans unpredictably. Use `adjustResize`. |
| No IME action chaining | User manually taps each field. Chain with Next/Done. |
| Keyboard open when dropdown tapped | Dismiss keyboard first, then open bottom sheet. |

---

## Form screen map

| Screen | Fields | Validation | Steps | CTA | Selection |
|---|---|---|---|---|---|
| ISP Recharge | Amount, phone | On blur + submit | 2 (input + review) | Pinned | Plan radio cards |
| Plan Change | Dropdown | On select | 2 (select + confirm) | Pinned | Radio cards |
| Return Flow | Dropdown, textarea | On blur + submit | 1 | Scroll | Dropdown (reason) |
| Request NetBox | Number, dropdown | On blur + submit | 1 | Pinned | Dropdown |
| KYC/Profile | 8-12 fields | On blur per step | 3-4 | Scroll/step | Dropdowns |
| Batch Operations | Multi-select | On selection | 1 | Pinned | Checkbox cards |
| OTP | Per-digit input | Real-time | 1 | Pinned | — |
| Search | Search input | None | 0 | — | Filter chips |

---

## Review checklist

### Field anatomy (1-4)

| # | Check | Pass criteria |
|---|---|---|
| 1 | Every field uses `WiomTextField` or typed variant? | No raw `TextField` or `OutlinedTextField` |
| 2 | All tokens from `WiomTokens`? | Zero hardcoded dp/sp/color values |
| 3 | Error uses background AND border? | `T.Bg.negative` + `T.State.negative` border |
| 4 | Focus border = 2px, default = 1px? | Visual thickness change on focus |

### Input types + validation (5-9)

| # | Check | Pass criteria |
|---|---|---|
| 5 | `keyboardType` set per field type? | Phone=Phone, Amount=Number, OTP=NumberPassword |
| 6 | `ImeAction.Next` on all except last? | Last field = Done. Search = Search. |
| 7 | Validation on blur, not keystroke? | No red errors while user is still typing |
| 8 | ALL errors shown at once on submit? | Not one at a time. Scroll to first error. |
| 9 | Hindi error messages? | "10 अंकों का फ़ोन नंबर डालें" not "Invalid phone" |

### Forms + keyboard (10-15)

| # | Check | Pass criteria |
|---|---|---|
| 10 | One keyboard strategy per screen? | Resize+scroll OR pinned CTA. Never mixed. |
| 11 | `adjustResize` in manifest? | Never `adjustPan` |
| 12 | Pinned CTA has `navigationBarsPadding()`? | CTA not behind gesture bar |
| 13 | Multi-step data in ViewModel? | Back press preserves all data |
| 14 | Review screen before payment? | Partner verifies before money moves |
| 15 | Dropdown opens bottom sheet, not keyboard? | Tap field → sheet appears, no keyboard |

---

*CA Design System — DS Forms Skill — April 2026*
*Source: Universal Principles of Design (Lidwell, Holden, Butler, 2003)*
*Depends on: wiom-android-layout (tokens), wiom-visual-skill (hierarchy), wiom-ds-states (submit states)*
