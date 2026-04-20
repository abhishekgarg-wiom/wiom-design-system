---
name: wiom-typography
description: Wiom Design System typography tokens and rules. Use this skill EVERY TIME you write, edit, or review any UI code for the Wiom Android app — Kotlin, Compose, XML layouts, or any file that touches text styling. Trigger when you see font sizes, text styles, TextStyle, Typography, font-weight, line-height, sp, or any legacy token names like T1_32_Bold, B2_14_Regular, L2_14_SemiBold, etc. Also trigger when creating new screens, components, or reviewing PRs for typography consistency. If you are writing ANY text style for Wiom, consult this skill first — do not guess font sizes or weights.
---

# Wiom Typography System — Next

**13 semantic tokens · 8 sizes · 5 core groups + 1 restricted utility · Font: Noto Sans · All values in sp (scales with Android accessibility settings)**

**Default size is 16sp.** Both reading text (`body.lg`) and interactive text (`label.lg`) default to 16sp/24lh. 14sp is used only in specific, named contexts (card description, chat bubble, chip/badge, tabs/filters, inline error/helper). Anything below 14sp is restricted — see Floor Rule.

---

## The 13 Approved Tokens

| Token | Figma Style | Size | Weight | LH | Tracking | Use for | Do NOT use for |
|-------|-------------|------|--------|----|----------|---------|----------------|
| `type.display.lg` | Type/Display/LG | 48sp | Bold 700 | 64sp | -0.02em | Splash hero, onboarding headline, marketing header | Section titles, body text, buttons |
| `type.heading.xl` | Type/Heading/XL | 32sp | Bold 700 | 44sp | -0.01em | Section heroes, feature highlights, promo headers, primary amounts | Card titles, nav titles, body, buttons |
| `type.heading.lg` | Type/Heading/LG | 24sp | Bold 700 | 32sp | -0.01em | **Default page title, dialog header, bottom sheet header.** Primary section heads. | Card titles, nav titles, buttons |
| `type.heading.md` | Type/Heading/MD | 20sp | Bold 700 | 28sp | -0.01em | Card group titles, secondary bottom sheet headers, sub-sections | Card titles, nav titles, buttons, body |
| `type.title.lg` | Type/Title/LG | 20sp | Medium 500 | 28sp | -0.01em | Nav titles, toolbar titles, prominent card titles | Section headings, buttons, body text |
| `type.title.sm` | Type/Title/SM | 16sp | Bold 700 | 24sp | 0 | Card titles, list group headers, section subheads, secondary amounts | Buttons (use label.lg), body, nav titles |
| `type.body.lg` | Type/Body/LG | 16sp | Regular 400 | 24sp | 0 | **Default reading text.** Paragraphs, descriptions, onboarding copy, toast body, input value, list item primary, empty-state message | Headings, titles, buttons, labels |
| `type.body.md` | Type/Body/MD | 14sp | Regular 400 | 20sp | 0 | **Specific contexts only:** card description, chat bubble message, list item secondary, banner/alert body, inline form error, helper text | Default paragraph, primary CTA, headings |
| `type.body.sm` | Type/Body/SM | 12sp | Regular 400 | 16sp | 0 | Captions, compact helper text, chat reply message preview, activity-list timestamps | Primary content, buttons, headings, card body |
| `type.label.lg` | Type/Label/LG | 16sp | SemiBold 600 | 24sp | +0.01em | **Default interactive text.** Primary/secondary/tertiary CTAs, input labels, radio/checkbox/switch labels, list item tappable rows, toast action | Card titles (use title.sm), headings, body |
| `type.label.md` | Type/Label/MD | 14sp | SemiBold 600 | 20sp | +0.01em | Chips, filter chips, badge labels, tabs, inline secondary buttons ("Edit", "See all", "Clear"), form error labels | Primary CTA (use label.lg), body, headings |
| `type.label.sm` | Type/Label/SM | 12sp | SemiBold 600 | 16sp | +0.01em | Chat reply sender name, bottom nav labels, tightly constrained tag labels | Primary buttons, body text, headings |
| `type.meta.xs` | Type/Meta/XS | 10sp | Regular 400 | 16sp | +0.02em | **Restricted:** timestamps inside chat bubbles and badge counts ONLY. Never for Hindi body text. | Any readable body text, buttons, labels, captions |

### Weight at shared sizes

- **20sp:** heading.md (Bold 700) vs title.lg (Medium 500) — 200 unit gap ✓
- **16sp:** title.sm (Bold 700) vs label.lg (SemiBold 600) vs body.lg (Regular 400) — three-way, never inline with each other; distinguished by role not proximity
- **14sp:** label.md (SemiBold 600) vs body.md (Regular 400) — 200 unit gap ✓
- **12sp:** label.sm (SemiBold 600) vs body.sm (Regular 400) — 200 unit gap ✓

---

## Token Selection — "What is this text's job?"

```
Hero / splash / marketing headline?          → display.lg   (48 Bold)
Section hero / primary amount?               → heading.xl   (32 Bold)
Page title / dialog / bottom sheet header?   → heading.lg   (24 Bold)   ← default page/sheet header
Card group title / sub-section?              → heading.md   (20 Bold)
Nav / toolbar / prominent card title?        → title.lg     (20 Medium)
Card title / list group header / sec. amount → title.sm     (16 Bold)
Reading text (default paragraph, toast, etc) → body.lg      (16 Regular)  ← default body
Card description / chat bubble / helper      → body.md      (14 Regular)
Caption / chat reply preview / activity ts   → body.sm      (12 Regular)
CTA / input label / selection control label  → label.lg     (16 SemiBold) ← default interactive
Chip / badge / filter / tab / inline btn     → label.md     (14 SemiBold)
Chat reply sender / bottom nav label         → label.sm     (12 SemiBold)
Timestamp in chat bubble / badge number?     → meta.xs      (10 Regular) ⚠
```

**If unsure:** default to `body.lg` for reading text, `label.lg` for interactive text. 16sp is the right answer most of the time.

---

## Title → Description Pairing Rule

Description size scales down one step from its title's size. This keeps visual hierarchy consistent across cards, sections, and dialogs.

| Title token | Title size | Description token | Description size |
|---|---|---|---|
| `heading.lg` | 24 Bold | `body.lg` | 16 Regular |
| `heading.md` / `title.lg` | 20 Bold / Medium | `body.lg` | 16 Regular |
| `title.sm` / `label.lg` | 16 Bold / SemiBold | `body.md` | 14 Regular |
| `label.md` | 14 SemiBold | `body.sm` | 12 Regular (avoid when possible) |

Examples:
- Page header (24) + subtitle → 16 body
- Card title (16 Bold) + description → 14 body
- Dialog title (24) + body copy → 16 body
- List item primary (16) + secondary (14) follows the same ratio

---

## Component-to-Token Mapping

| UI Component | Token | Why |
|---|---|---|
| Primary CTA button | `labelLg` | 16sp SemiBold — default CTA |
| Secondary CTA button | `labelLg` | 16sp SemiBold — same treatment as primary; type of button is carried by fill/stroke, not size |
| Tertiary CTA button | `labelLg` | 16sp SemiBold |
| Destructive CTA | `labelLg` | 16sp SemiBold |
| Pre-booking CTA | `labelLg` | 16sp SemiBold |
| Inline secondary button ("Edit", "See all", "Clear") | `labelMd` | 14sp SemiBold — lower-weight inline actions |
| Card title | `titleSm` | 16sp Bold — card-level structural anchor |
| Card description | `bodyMd` | 14sp Regular — follows title→description pairing (16 → 14) |
| Toolbar / nav bar title | `titleLg` | 20sp Medium — navigational emphasis |
| Bottom sheet header | `headingLg` | 24sp Bold — default sheet header |
| Dialog title | `headingLg` | 24sp Bold |
| Page title | `headingLg` | 24sp Bold |
| Section header on home (group label above cards) | `titleSm` | 16sp Bold — group anchor, not page-level |
| Secondary bottom sheet / compact dialog | `headingMd` | 20sp Bold — only when space-constrained |
| Chat bubble message | `bodyMd` | 14sp Regular — **intentional exception** to 16 default, density matters in dense-message domain |
| Chat reply preview — sender name | `labelSm` | 12sp SemiBold |
| Chat reply preview — message | `bodySm` | 12sp Regular |
| Chat timestamp (inside bubble) | `metaXS` | 10sp Regular — restricted ⚠ |
| Input field label | `labelLg` | 16sp SemiBold — default form label |
| Input field value | `bodyLg` | 16sp Regular — default form value |
| Input helper text | `bodyMd` | 14sp Regular (preferred) — `bodySm` 12sp acceptable if space-constrained |
| Input error text | `bodyMd` | 14sp Regular |
| Radio / Checkbox / Switch label | `labelLg` or `bodyLg` | 16sp — label when tappable control, body when descriptive row |
| List item primary text | `bodyLg` | 16sp Regular — or `labelLg` if the row is tappable |
| List item secondary text | `bodyMd` | 14sp Regular |
| Toast body | `bodyLg` | 16sp Regular |
| Toast action ("Undo", "Retry") | `labelLg` | 16sp SemiBold |
| Banner / alert heading | `titleSm` | 16sp Bold |
| Banner / alert body | `bodyMd` | 14sp Regular |
| Chip label | `labelMd` | 14sp SemiBold |
| Filter chip label | `labelMd` | 14sp SemiBold |
| Tab bar label | `labelMd` | 14sp SemiBold (prefer) — `labelLg` 16 acceptable if few tabs and space allows |
| Badge label (text chip badge) | `labelMd` | 14sp SemiBold |
| Badge count (numeric) | `metaXS` | 10sp Regular — restricted ⚠ |
| Bottom nav label | `labelSm` | 12sp SemiBold — space-constrained exception |
| Empty state message | `bodyLg` | 16sp Regular (primary message can use `titleSm` 16 Bold for emphasis) |
| Primary amount (hero pricing, plan value) | `headingXL` or `headingLg` | 32 or 24 Bold depending on prominence |
| Secondary amount (in a row) | `titleSm` | 16sp Bold |
| Activity list timestamp (standalone row) | `bodySm` | 12sp Regular |
| Inline link within body text | inherits parent | don't shrink — 16 inside 16 body, 14 inside 14 body |

---

## Floor Rule — minimum sizes

| Size | Allowed contexts | NEVER for |
|---|---|---|
| **10sp (`meta.xs`)** | Timestamps inside chat bubbles, numeric badge counts | Any readable content, Hindi text, labels, body copy |
| **12sp (`body.sm` / `label.sm`)** | Captions, chat reply preview, activity timestamps, bottom nav labels, tightly constrained tag labels | Paragraph body text, card content, CTAs, primary labels |
| **14sp (`body.md` / `label.md`)** | Specific contexts listed in the mapping table (chat bubble, card description, chip/badge, tabs/filters, inline errors/helpers, inline secondary buttons) | Default paragraph, primary CTA, input label/value, list item primary, toast body |
| **16sp (`body.lg` / `label.lg`)** | **Default for everything readable and everything tappable** | Headings, chips/badges (too big), meta/utility text |

If a design reaches for sub-16 text outside the listed contexts, flag it — don't approximate.

---

## Additional Contextual Rules

1. **Never use all-caps styling.** Devanagari has no case — UPPERCASE breaks Hindi rendering and hurts Latin legibility on budget panels.
2. **Italic is reserved** for titles of works or quoted long-form text. Not a UI state. Do not italicize labels, errors, or emphasis in UI copy.
3. **Link styling inside body text** — keep the size from the parent. Underline or color differentiation carries the interaction signal, not a size change.
4. **Truncation vs wrap** — row titles/labels truncate with ellipsis (one line); body/descriptions wrap, cap at 2–3 lines in list density contexts.
5. **Max line length** — body copy targets 60–75 characters per line. On mobile this is usually self-enforcing, but be deliberate on wide phones / landscape.
6. **Hierarchy via weight, not color** — at the same size, SemiBold for interactive vs Regular for reading. Color adjustments come from the color system, not from typography overrides.

---

## Chat Exception (called out explicitly)

Chat bubble message text uses `body.md` (14sp), not the 16sp default. This is intentional:

- Chat is a dense-message domain — many bubbles per screen, tight vertical rhythm matters.
- Bubbles are visually framed (colored background, tail), which adds readable contrast beyond raw size.
- Chat timestamps (inside bubble) use `meta.xs` 10sp; replies use 12sp tokens.

Do not generalize this exception to other dense lists. Non-chat list items use `body.lg` 16 for primary text.

---

## Hindi / Devanagari Legibility

- **16sp is the floor for Hindi body text.** Devanagari glyphs carry more strokes per unit width than Latin; at 14sp and below, conjuncts and matras lose definition on budget Android panels.
- **Weight uplift doesn't substitute for size** — 14sp Bold Hindi is still 14sp. If a design asks for "smaller Hindi to fit," the answer is re-layout, not a font-size cheat.
- **Never use `meta.xs` (10sp) for Hindi** — even timestamps with Hindi date formatting should use `body.sm` 12sp instead if Hindi is rendered.
- **No negative letter-spacing on Devanagari.** Heading tokens apply -0.01em to -0.02em — this is acceptable at heading sizes, but never apply tighter tracking to body or label tokens used with Hindi.

---

## Accessibility Scaling Target

Screens must remain usable when the user's Android system font scale is set to **130%**. At 130%:

- `body.lg` 16 → ~21sp
- `body.md` 14 → ~18sp
- `body.sm` 12 → ~16sp
- `meta.xs` 10 → ~13sp (readable but unforgiving)

Test criterion: no text truncation in single-line CTAs, no overlapping labels, no critical copy clipped at 130% scale on a 360dp-wide device. This is why `meta.xs` is restricted to timestamps and badge counts — failure at 130% is most likely there.

---

## Kotlin / Compose Implementation

```kotlin
package com.wiom.ui.theme

import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.Font
import androidx.compose.ui.text.font.FontFamily
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.em
import androidx.compose.ui.unit.sp

val NotoSans = FontFamily(
    Font(R.font.notosans_regular, FontWeight.Normal),
    Font(R.font.notosans_medium, FontWeight.Medium),
    Font(R.font.notosans_semibold, FontWeight.SemiBold),
    Font(R.font.notosans_bold, FontWeight.Bold),
)

object WiomType {

    // ── Display ──────────────────────────────
    val displayLg = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Bold,
        fontSize = 48.sp,
        lineHeight = 64.sp,
        letterSpacing = (-0.02).em,
    )

    // ── Heading ──────────────────────────────
    val headingXL = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Bold,
        fontSize = 32.sp,
        lineHeight = 44.sp,
        letterSpacing = (-0.01).em,
    )
    val headingLg = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Bold,
        fontSize = 24.sp,
        lineHeight = 32.sp,
        letterSpacing = (-0.01).em,
    )
    val headingMd = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Bold,
        fontSize = 20.sp,
        lineHeight = 28.sp,
        letterSpacing = (-0.01).em,
    )

    // ── Title ────────────────────────────────
    val titleLg = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Medium,
        fontSize = 20.sp,
        lineHeight = 28.sp,
        letterSpacing = (-0.01).em,
    )
    val titleSm = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Bold,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.em,
    )

    // ── Body ─────────────────────────────────
    val bodyLg = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Normal,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.em,
    )
    val bodyMd = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Normal,
        fontSize = 14.sp,
        lineHeight = 20.sp,
        letterSpacing = 0.em,
    )
    val bodySm = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Normal,
        fontSize = 12.sp,
        lineHeight = 16.sp,
        letterSpacing = 0.em,
    )

    // ── Label ────────────────────────────────
    val labelLg = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.SemiBold,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.01.em,
    )
    val labelMd = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.SemiBold,
        fontSize = 14.sp,
        lineHeight = 20.sp,
        letterSpacing = 0.01.em,
    )
    val labelSm = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.SemiBold,
        fontSize = 12.sp,
        lineHeight = 16.sp,
        letterSpacing = 0.01.em,
    )

    // ── Meta (restricted) ────────────────────
    // ⚠ Only for timestamps inside chat bubbles and badge counts.
    // Never for Hindi body text or any readable content.
    val metaXS = TextStyle(
        fontFamily = NotoSans,
        fontWeight = FontWeight.Normal,
        fontSize = 10.sp,
        lineHeight = 16.sp,
        letterSpacing = 0.02.em,
    )
}
```

### Usage

```kotlin
// ✅ Correct
Text(text = "रिचार्ज करें", style = WiomType.headingLg)          // page title
Text(text = "28 दिन का प्लान", style = WiomType.bodyLg)          // paragraph (default)
Text(text = "यह प्लान 28 दिन के लिए है", style = WiomType.bodyMd) // card description
Button(onClick = { }) {
    Text(text = "अभी रिचार्ज करें", style = WiomType.labelLg)     // CTA
}

// ❌ NEVER — hardcoded sizes
Text(text = "Title", fontSize = 18.sp, fontWeight = FontWeight.W600)

// ❌ NEVER — legacy tokens
Text(text = "Title", style = T3_20_Medium) // use WiomType.titleLg

// ❌ NEVER — sub-16 for default paragraph / CTA
Text(text = "Hello", style = WiomType.bodyMd)                   // use bodyLg for default paragraph
Button(onClick = { }) {
    Text(text = "Continue", style = WiomType.labelMd)           // CTA must be labelLg
}
```

---

## Legacy Token Migration

### Keep (direct rename)
| Legacy | New | Notes |
|--------|-----|-------|
| `D1_48_Bold` | `WiomType.displayLg` | — |
| `T1_32_Bold` | `WiomType.headingXL` | — |
| `T2_24_Bold` | `WiomType.headingLg` | — |
| `T3_20_Bold` | `WiomType.headingMd` | — |
| `T3_20_Medium` | `WiomType.titleLg` | — |
| `T4_16_Bold` | `WiomType.titleSm` | — |
| `B1_16_Regular` | `WiomType.bodyLg` | — |
| `B2_14_Regular` | `WiomType.bodyMd` | Note: 14sp now reserved for specific contexts — verify before reusing |
| `B3_12_Regular` | `WiomType.bodySm` | — |
| `L2_14_SemiBold` | `WiomType.labelMd` | Note: CTAs migrate to `labelLg` — verify the call site isn't a CTA |
| `L3_12_SemiBold` | `WiomType.labelSm` | — |
| `B4_10_Regular` | `WiomType.metaXS` | Now restricted — verify timestamps/badges only |

### Merge
| Legacy | Maps To | Decision |
|--------|---------|----------|
| `D2_40_Regular` | `displayLg` or remove | Use displayLg (48) if needed |
| `T4_16_Regular` | `bodyLg` | Same size/weight — was duplicate |
| `L1_16_Bold` | `titleSm` or `labelLg` | Card title → titleSm. Button → labelLg |
| `B1_16_Bold` | `titleSm` | If structural. If body emphasis → bodyLg + bold attr |
| `B1_16_SemiBold` | `labelLg` | Direct match |
| `B1_16_Medium` | `bodyLg` | Medium not distinct from Regular at 16sp |
| `B2_14_SemiBold` | `labelMd` | Was body emphasis → now label role |

### Remove
| Legacy | Use Instead | Why |
|--------|-------------|-----|
| `T2_24_Regular` | `headingLg` | No distinct role at 24 Regular |
| `L1_16_Regular` | `bodyLg` | Label at Regular = body text |
| `L2_14_Regular` | `bodyMd` | Label at Regular = body text |
| `B2_14_Medium` | `bodyMd` or `labelMd` | Medium not distinct from Regular on mobile |

### Legacy (existing screens only)
| Legacy | Notes |
|--------|-------|
| `T3_20_Regular` | Migrate to `titleLg` when screen is touched |

---

## Rules

### DO
- Always use `WiomType.*` tokens, never raw sp values
- Use weight to distinguish roles at the same size
- Match Figma style name to code token name exactly
- Test Hindi strings when verifying typography
- Ask "what is this text's job?" before picking a token
- **Default to `bodyLg` (16 Regular) for reading text if unsure**
- **Default to `labelLg` (16 SemiBold) for interactive text if unsure**
- Default page/dialog/bottom-sheet headers to `headingLg` (24 Bold)
- Apply the title→description pairing rule consistently
- Test every screen at 130% system font scale
- Migrate legacy tokens when you touch a screen
- Mark deprecated tokens with `@Deprecated` + `ReplaceWith`

### DON'T
- Use heading tokens for card titles (use `titleSm`)
- Use heading tokens for buttons (use `labelLg`)
- Use `titleSm` for buttons (use `labelLg` — semantic difference)
- Use label tokens for non-interactive text
- Use body tokens for buttons or headings
- Use `labelMd` (14sp) for a primary CTA — CTAs are always `labelLg` (16sp)
- Use `bodyMd` (14sp) for default paragraph/reading text — that's `bodyLg` (16sp)
- Use `bodySm` (12sp) for paragraph or card body content
- Use `metaXS` (10sp) for anything readable — timestamps in chat bubbles and badge counts only
- Use `metaXS` for Hindi text under any circumstance
- Use all-caps styling anywhere (breaks Devanagari, hurts Latin on budget panels)
- Use italics as a UI state (reserved for titles of works / long-form quotations)
- Shrink a link inside body text — inherit the parent size
- Use bottom-nav-style 12sp labels for anything other than bottom navigation and the listed exceptions
- Use `metaXS` in new contexts without design-system approval
- Pair SemiBold + Medium at the same font size
- Use sizes outside the scale (no 11, 13, 15, 18, 22, 28)
- Use line-heights that aren't 4px multiples
- Use any font other than Noto Sans
- Apply negative letter-spacing to Hindi body or label text

---

## Quick Reference

```
DISPLAY   display.lg   48  Bold      64  -0.02em  Hero / splash / marketing
──────────────────────────────────────────────────────────────────────
HEADING   heading.xl   32  Bold      44  -0.01em  Section hero / primary amount
          heading.lg   24  Bold      32  -0.01em  ★ Default page / dialog / sheet header
          heading.md   20  Bold      28  -0.01em  Card group / sub-section
──────────────────────────────────────────────────────────────────────
TITLE     title.lg     20  Medium    28  -0.01em  Nav / toolbar / card title
          title.sm     16  Bold      24   0       Card title / list header / 2nd amount
──────────────────────────────────────────────────────────────────────
BODY      body.lg      16  Regular   24   0       ★ Default reading text
          body.md      14  Regular   20   0       Card desc / chat / helper / banner body
          body.sm      12  Regular   16   0       Caption / chat reply / timestamps
──────────────────────────────────────────────────────────────────────
LABEL     label.lg     16  SemiBold  24  +0.01em  ★ Default CTA / input label / selection
          label.md     14  SemiBold  20  +0.01em  Chip / filter / tab / inline btn / badge
          label.sm     12  SemiBold  16  +0.01em  Chat reply sender / bottom nav
──────────────────────────────────────────────────────────────────────
META ⚠    meta.xs      10  Regular   16  +0.02em  Chat-bubble timestamp / badge count ONLY
```

★ = 16sp default. If unsure, use these.
