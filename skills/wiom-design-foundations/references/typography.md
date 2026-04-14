---
name: wiom-typography
description: Wiom Design System typography tokens and rules. Use this skill EVERY TIME you write, edit, or review any UI code for the Wiom Android app — Kotlin, Compose, XML layouts, or any file that touches text styling. Trigger when you see font sizes, text styles, TextStyle, Typography, font-weight, line-height, sp, or any legacy token names like T1_32_Bold, B2_14_Regular, L2_14_SemiBold, etc. Also trigger when creating new screens, components, or reviewing PRs for typography consistency. If you are writing ANY text style for Wiom, consult this skill first — do not guess font sizes or weights.
---

# Wiom Typography System — Next

**13 semantic tokens · 8 sizes · 5 core groups + 1 restricted utility · Font: Noto Sans · All values in sp (scales with Android accessibility settings)**

---

## The 13 Approved Tokens

| Token | Figma Style | Size | Weight | LH | Tracking | Use for | Do NOT use for |
|-------|-------------|------|--------|----|----------|---------|----------------|
| `type.display.lg` | Type/Display/LG | 48sp | Bold 700 | 64sp | -0.02em | Splash hero, onboarding headline, marketing header | Section titles, body text, buttons |
| `type.heading.xl` | Type/Heading/XL | 32sp | Bold 700 | 44sp | -0.01em | Section heroes, feature highlights, promo headers | Card titles, nav titles, body, buttons |
| `type.heading.lg` | Type/Heading/LG | 24sp | Bold 700 | 32sp | -0.01em | Page titles, dialog headers, primary section heads | Card titles, nav titles, buttons |
| `type.heading.md` | Type/Heading/MD | 20sp | Bold 700 | 28sp | -0.01em | Card group titles, bottom sheet headers, sub-sections | Card titles, nav titles, buttons, body |
| `type.title.lg` | Type/Title/LG | 20sp | Medium 500 | 28sp | -0.01em | Nav titles, toolbar titles, prominent card titles | Section headings, buttons, body text |
| `type.title.sm` | Type/Title/SM | 16sp | Bold 700 | 24sp | 0 | Card titles, list group headers, section subheads | Buttons (use label.lg), body, nav titles |
| `type.body.lg` | Type/Body/LG | 16sp | Regular 400 | 24sp | 0 | Long-form reading, descriptions, onboarding copy | Headings, titles, buttons, labels |
| `type.body.md` | Type/Body/MD | 14sp | Regular 400 | 20sp | 0 | Default paragraph, input values, chat messages | Headings, titles, buttons, labels |
| `type.body.sm` | Type/Body/SM | 12sp | Regular 400 | 16sp | 0 | Captions, helper text, footnotes, supporting info | Primary content, buttons, headings |
| `type.label.lg` | Type/Label/LG | 16sp | SemiBold 600 | 24sp | +0.01em | Default button text, primary/secondary CTA labels | Card titles (use title.sm), headings, body |
| `type.label.md` | Type/Label/MD | 14sp | SemiBold 600 | 20sp | +0.01em | Input labels, nav items, form fields, smaller buttons | Primary CTA (use label.lg), body, headings |
| `type.label.sm` | Type/Label/SM | 12sp | SemiBold 600 | 16sp | +0.01em | Chips, badges, small buttons, tag labels | Primary buttons, body text, headings |
| `type.meta.xs` | Type/Meta/XS | 10sp | Regular 400 | 16sp | +0.02em | **Restricted:** timestamps and badges ONLY. Never for Hindi body text. | Any Hindi reading text, buttons, labels, body |

### Weight at shared sizes

- **20sp:** heading.md (Bold 700) vs title.lg (Medium 500) — 200 unit gap ✓
- **16sp:** title.sm (Bold 700) vs label.lg (SemiBold 600) vs body.lg (Regular 400) — label.lg is acceptable because buttons are isolated components that never sit inline beside card titles or body
- **14sp:** label.md (SemiBold 600) vs body.md (Regular 400) — 200 unit gap ✓
- **12sp:** label.sm (SemiBold 600) vs body.sm (Regular 400) — 200 unit gap ✓

---

## Token Selection — "What is this text's job?"

```
Hero / splash / marketing headline?          → display.lg   (48 Bold)
Section hero / feature highlight?            → heading.xl   (32 Bold)
Page title / dialog header?                  → heading.lg   (24 Bold)
Card group title / sub-section?              → heading.md   (20 Bold)
Nav / toolbar / prominent card title?        → title.lg     (20 Medium)
Card title / list group header?              → title.sm     (16 Bold)
Long-form reading / description?             → body.lg      (16 Regular)
Default paragraph / chat / input value?      → body.md      (14 Regular)
Caption / helper text / footnote?            → body.sm      (12 Regular)
Default button / primary-secondary CTA?      → label.lg     (16 SemiBold)
Input label / nav item / compact button?     → label.md     (14 SemiBold)
Chip / badge / tag?                          → label.sm     (12 SemiBold)
Timestamp / badge number?                    → meta.xs      (10 Regular) ⚠
```

**If unsure:** default to `body.md` for reading text, `label.md` for interactive text.

---

## Component-to-Token Mapping

| UI Component | Token | Why |
|-----------|-------|-----|
| Primary CTA button | `labelLg` | 16sp SemiBold — confident with Hindi text |
| Secondary button | `labelMd` | 14sp SemiBold — secondary interactive |
| Full-width action button | `labelLg` | 16sp SemiBold — wide CTAs need presence |
| Compact / inline button | `labelMd` | 14sp SemiBold — tighter spaces |
| Card title | `titleSm` | 16sp Bold — card-level structural anchor |
| Toolbar / nav bar title | `titleLg` | 20sp Medium — navigational emphasis |
| Bottom sheet header | `headingMd` | 20sp Bold — structural anchor |
| Section header on home | `headingMd` | 20sp Bold — groups cards under it |
| Page title / dialog header | `headingLg` | 24sp Bold — page-level anchor |
| Chat message body | `bodyMd` | 14sp Regular — default paragraph |
| Plan description text | `bodyLg` | 16sp Regular — longer reading |
| Input field label | `labelMd` | 14sp SemiBold — interactive form label |
| Input field value | `bodyMd` | 14sp Regular — user-entered content |
| Input helper text | `bodySm` | 12sp Regular — supporting context |
| Chip / tag label | `labelSm` | 12sp SemiBold — small interactive |
| Tab bar label | `labelMd` | 14sp SemiBold — interactive navigation |
| Price on card | `titleSm` | 16sp Bold — emphasis, card-level |
| Recharge amount (hero) | `headingLg` | 24sp Bold — page-level number |
| Timestamp in chat | `metaXS` | 10sp Regular — restricted ⚠ |
| Badge count | `metaXS` | 10sp Regular — restricted ⚠ |

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
    Font(R.font.noto_sans_regular, FontWeight.Normal),
    Font(R.font.noto_sans_medium, FontWeight.Medium),
    Font(R.font.noto_sans_semibold, FontWeight.SemiBold),
    Font(R.font.noto_sans_bold, FontWeight.Bold),
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
    // ⚠ Only for timestamps and badges.
    // Never for Hindi body text.
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
Text(text = "रिचार्ज करें", style = WiomType.headingMd)
Text(text = "28 दिन का प्लान", style = WiomType.bodyMd)
Button(onClick = { }) {
    Text(text = "अभी रिचार्ज करें", style = WiomType.labelLg)
}

// ❌ NEVER — hardcoded sizes
Text(text = "Title", fontSize = 18.sp, fontWeight = FontWeight.W600)

// ❌ NEVER — legacy tokens
Text(text = "Title", style = T3_20_Medium) // use WiomType.titleLg
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
| `B2_14_Regular` | `WiomType.bodyMd` | — |
| `B3_12_Regular` | `WiomType.bodySm` | — |
| `L2_14_SemiBold` | `WiomType.labelMd` | — |
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
- Default to `bodyMd` for reading text if unsure
- Default to `labelMd` for interactive text if unsure
- Migrate legacy tokens when you touch a screen
- Mark deprecated tokens with `@Deprecated` + `ReplaceWith`

### DON'T
- Use heading tokens for card titles (use `titleSm`)
- Use heading tokens for buttons (use `labelLg`)
- Use `titleSm` for buttons (use `labelLg` — semantic difference)
- Use label tokens for non-interactive text
- Use body tokens for buttons or headings
- Use `metaXS` for Hindi body text (minimum 12sp = `bodySm`)
- Use `metaXS` in new contexts without approval
- Pair SemiBold + Medium at the same font size
- Use sizes outside the scale (no 11, 13, 15, 18, 22, 28)
- Use line-heights that aren't 4px multiples
- Use any font other than Noto Sans

---

## Quick Reference

```
DISPLAY   display.lg   48  Bold      64  -0.02em  Hero / splash / marketing
──────────────────────────────────────────────────────────────────────
HEADING   heading.xl   32  Bold      44  -0.01em  Section hero
          heading.lg   24  Bold      32  -0.01em  Page title / dialog
          heading.md   20  Bold      28  -0.01em  Card group / sub-section
──────────────────────────────────────────────────────────────────────
TITLE     title.lg     20  Medium    28  -0.01em  Nav / toolbar / card title
          title.sm     16  Bold      24   0       Card title / list header
──────────────────────────────────────────────────────────────────────
BODY      body.lg      16  Regular   24   0       Long-form / description
          body.md      14  Regular   20   0       Paragraph / chat / input
          body.sm      12  Regular   16   0       Caption / helper / footnote
──────────────────────────────────────────────────────────────────────
LABEL     label.lg     16  SemiBold  24  +0.01em  Default button / CTA
          label.md     14  SemiBold  20  +0.01em  Input label / nav / small btn
          label.sm     12  SemiBold  16  +0.01em  Chip / badge / tag
──────────────────────────────────────────────────────────────────────
META ⚠    meta.xs      10  Regular   16  +0.02em  Timestamp / badge ONLY
```
