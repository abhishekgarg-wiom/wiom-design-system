---
name: wiom-badge
description: >
  Wiom Badge component system — Dot, Count, and Label badges with size, color, and style variants.
  Use this skill EVERY TIME you create, edit, or review any screen, card, list item, tab, header, or
  component that contains a status indicator, count, tag, or label for the Wiom Partner or Customer app.
  Trigger when you see Badge, Tag, Label, Status, Count, Dot, Indicator, Chip (status), or any passive
  status/state display element.
---

# Wiom Badge System

The approved badge/indicator foundation for the Wiom app ecosystem (Android/Kotlin/Compose).
**3 types · 2 sizes · 5 colors · 2 styles · 15 variants**

---

## 1. The 3 Badge Types

| Type | Purpose | Height | Example |
|------|---------|--------|---------|
| **Dot** | Unread/active indicator, no text | 8px | Red dot on bell icon |
| **Count** | Numeric quantity | 18px | Notification count "5", tab count "9+" |
| **Label** | Text status / state / role | 24–28px | "पक्का", "असफल", "एडमिन" |

---

## 2. Token Reference

### Dot Badge

| Property | Value | Token |
|----------|-------|-------|
| Size | 8×8px | — |
| Radius | 9999px | `radius/full` |
| Colors | Brand / Negative / Neutral | See color table |

### Count Badge

| Property | Value | Token |
|----------|-------|-------|
| Min size | 18×18px | — |
| Radius | 9999px | `radius/full` |
| Horizontal padding | 5px | — |
| Font | Noto Sans Regular 400 | `type.meta.xs` |
| Font size | 10sp | `type.meta.xs` |
| Line height | 16sp | `type.meta.xs` |
| Text color | #FFFFFF | `color.text.on-color` |
| Max display | 9+ or 99+ | — |
| Colors | Brand / Negative | See color table |

### Label Badge — Default (28px)

| Property | Value | Token |
|----------|-------|-------|
| Height | 28px | Derived: 4+20+4 |
| Radius | 4px | `radius/tiny` |
| Horizontal padding | 12px | `space/12` |
| Vertical padding | 4px | `space/4` |
| Font | Noto Sans SemiBold 600 | `type.label.md` |
| Font size | 14sp | `type.label.md` |
| Line height | 20sp | `type.label.md` |

### Label Badge — Small (24px)

| Property | Value | Token |
|----------|-------|-------|
| Height | 24px | Derived: 4+16+4 |
| Radius | 4px | `radius/tiny` |
| Horizontal padding | 8px | `space/8` |
| Vertical padding | 4px | `space/4` |
| Font | Noto Sans SemiBold 600 | `type.label.sm` |
| Font size | 12sp | `type.label.sm` |
| Line height | 16sp | `type.label.sm` |

---

## 3. Color Variants

### Dot Badge Colors

| Variant | Fill | Token |
|---------|------|-------|
| Brand | #D9008D | `color.brand.primary` |
| Negative | #D92130 | `color.negative.primary` |
| Neutral | #E8E4F0 | `color.surface.muted` |

### Count Badge Colors

| Variant | Fill | Text | Token (fill) |
|---------|------|------|-------------|
| Brand | #D9008D | #FFFFFF | `color.brand.primary` |
| Negative | #D92130 | #FFFFFF | `color.negative.primary` |

### Label Badge Colors — Filled (Default size only)

| Color | Fill | Text | Token (fill) |
|-------|------|------|-------------|
| Positive | #008043 | #FFFFFF | `color.positive.primary` |
| Warning | #B85C00 | #FFFFFF | `color.warning.primary` |
| Negative | #D92130 | #FFFFFF | `color.negative.primary` |
| Info | #6D17CE | #FFFFFF | `color.info.primary` |
| Neutral | #E8E4F0 | #5C5570 | `color.surface.muted` / `color.text.secondary` |

### Label Badge Colors — Tinted (Default + Small)

| Color | Fill | Text | Token (fill) | Token (text) |
|-------|------|------|-------------|-------------|
| Positive | #E1FAED | #008043 | `color.positive.tint` | `color.positive.primary` |
| Warning | #FFF2BF | #B85C00 | `color.warning.subtle` | `color.warning.primary` |
| Negative | #FFE5E7 | #D92130 | `color.negative.tint` | `color.negative.primary` |
| Info | #F1E5FF | #6D17CE | `color.info.tint` | `color.info.primary` |
| Neutral | #F1EDF7 | #5C5570 | `color.surface.subtle` | `color.text.secondary` |

---

## 4. Decision Tree — Which Badge?

```
Need a visual indicator? Ask:

1. Is it just "something new / unread" with NO number or text?
   → DOT

2. Is it a numeric count (notifications, unread items)?
   → COUNT

3. Is it a text status / state / label?
   → LABEL — then ask:

   3a. Size: "If I remove this badge, does the row still serve its purpose?"
       YES (badge supports other content) → Small (24px)
       NO (badge IS the information)      → Default (28px)

   3b. Style: "Is this state final?"
       YES (terminal: completed, failed, confirmed, expired, revoked) → Filled
       NO (transitional: processing, provisional, pending, active)    → Tinted

       Exception: Small badges are ALWAYS Tinted. No Filled variant exists for Small.

   3c. Color: "What does the status represent?"
       Success / confirmed / complete / eligible  → Positive (green)
       Warning / pending / provisional / attention → Warning (orange)
       Error / failed / expired / revoked          → Negative (red)
       Active / in-progress / info                 → Info (purple)
       Default / proposed / neutral / inactive     → Neutral (grey)
```

---

## 5. Size Decision — Default vs Small

| | **Default (28px)** | **Small (24px)** |
|---|---|---|
| **Role** | Badge IS the information | Badge supports other content |
| **Test** | Remove badge → row loses purpose | Remove badge → row still works |
| **Typography** | `type.label.md` (14sp) | `type.label.sm` (12sp) |
| **Styles** | Filled + Tinted | Tinted only |
| **Emphasis** | High — user scans for this | Low — supplementary context |

### Default examples (badge is the point):
- Transaction: "निकासी — ₹2,000 → SBI `असफल`" — user checking outcome
- Slot: "14 अप्रैल, 10:00 AM `Confirmed`" — user checking if locked
- Device: "NB#2451 `Deployed`" — user checking state
- Zone: "`Suspended`" — user checking eligibility

### Small examples (badge supports other content):
- Wallet: "₹8,200 `पक्का`" — amount is primary, badge confirms certainty
- Header: "Rajesh `एडमिन`" — name is primary, badge shows role
- Tab: "टिकट `5`" — tab name is primary, count is secondary

---

## 6. Style Decision — Filled vs Tinted

| | **Filled** (loud) | **Tinted** (soft) |
|---|---|---|
| **State** | Terminal, won't change | Transitional, may change |
| **Emphasis** | High contrast, demands attention | Low contrast, informational |
| **Available in** | Default only | Default + Small |

### Common pairings:

| Color | Filled (terminal) | Tinted (transitional) |
|-------|-------------------|----------------------|
| Positive | पूरा हुआ, Confirmed | पक्का, Eligible |
| Warning | रुका हुआ *(rare)* | अपडेट हो रहा है, Provisional |
| Negative | असफल, Expired, Revoked | समय खत्म *(rare)* |
| Info | चालू *(rare)* | प्रोसेसिंग, Active |
| Neutral | बाकी *(rare)* | प्रस्तावित, Pending |

Pattern: Filled + Positive/Negative is common. Tinted + Warning/Info is common.

---

## 7. Figma Components

All components live in "Badges" section on "Sandbox - SAAD" page.

| Component | Variants | Properties |
|-----------|----------|------------|
| Badge / Dot | 3 | Color (Brand / Negative / Neutral) |
| Badge / Count | 2 | Color (Brand / Negative) |
| Badge / Label | 15 | Size (Default / Small) × Color (5) × Style (Filled / Tinted). Small has no Filled. |

**Total: 20 Figma variables bound** across all badge components.

---

## 8. Universal Rules

1. **Badges are passive** — never interactive, never tappable. If it needs a tap target, it's a Chip, not a badge.
2. **No icons in badges** — color + text is sufficient. If you need icon + text + color, that's a Status Indicator (different component).
3. **Small = Tinted only** — if the badge is important enough to be Filled, use Default size.
4. **Count overflow** — display "9+" for single-digit max, "99+" for double-digit max. Never show raw large numbers.
5. **Hide at zero** — Count badge is invisible when count = 0. Don't show "0".
6. **One badge per row** — never stack multiple badges on the same line item.
7. **Color matches status family** — a Positive badge never appears on a Negative row. Don't mix status families.
8. **Token-only values** — every color, spacing, radius, and typography value must come from the foundation skill. No hardcoded values.
9. **Badge text never wraps** — single line only. If text is too long, write shorter copy.
10. **Dot is overlay-only** — Dot badges are positioned absolutely over icons/avatars (top-right). Never inline.

---

## 9. Never Do

| Anti-Pattern | Why |
|-------------|-----|
| Tappable badge | Badge ≠ Chip. Use Chip for interactive filtering. |
| Icon inside badge | Adds complexity without value. Color already communicates. |
| Small + Filled | Contradicts itself — loud style on quiet size. Use Default if emphasis needed. |
| Count showing "0" | No unread = no badge. Hide it. |
| Multiple badges per row | Visual clutter. One status per line item. |
| "Continue" / "Status" as badge text | Too generic. Use specific state: "पूरा हुआ", "असफल", "चालू". |
| Raw hex colors in code | Use semantic tokens from foundation skill. |
| Arbitrary spacing values | Use tokens from foundation skill. |
| Badge for navigation | Badges are status indicators, not nav elements. |
| Mixing status families | Green badge on a red-bordered card = confusion. |

---

## 10. Compose Implementation

```kotlin
// Dot Badge (overlay on icon)
Box {
    Icon(
        painter = painterResource(R.drawable.ic_notifications),
        contentDescription = "सूचनाएं",
        modifier = Modifier.size(24.dp)
    )
    WiomBadgeDot(
        color = WiomBadgeColor.Negative,
        modifier = Modifier.align(Alignment.TopEnd)
    )
}

// Count Badge (on notification bell)
Box {
    Icon(
        painter = painterResource(R.drawable.ic_notifications),
        contentDescription = "सूचनाएं, 5 नई",
        modifier = Modifier.size(24.dp)
    )
    WiomBadgeCount(
        count = 5,
        color = WiomBadgeColor.Negative,
        modifier = Modifier.align(Alignment.TopEnd)
    )
}

// Label Badge — Default, Filled (terminal status on card)
WiomBadgeLabel(
    text = "असफल",
    size = WiomBadgeSize.Default,
    color = WiomBadgeColor.Negative,
    style = WiomBadgeStyle.Filled,
)

// Label Badge — Default, Tinted (transitional status)
WiomBadgeLabel(
    text = "प्रोसेसिंग",
    size = WiomBadgeSize.Default,
    color = WiomBadgeColor.Info,
    style = WiomBadgeStyle.Tinted,
)

// Label Badge — Small, Tinted (inline with amount)
Row(
    verticalAlignment = Alignment.CenterVertically,
    horizontalArrangement = Arrangement.spacedBy(8.dp)
) {
    Text("सेटलमेंट के लिए", style = WiomType.bodyMd)
    Text("₹8,200", style = WiomType.labelMd)
    WiomBadgeLabel(
        text = "पक्का",
        size = WiomBadgeSize.Small,
        color = WiomBadgeColor.Positive,
        // Small is always Tinted — no style parameter needed
    )
}

// Transaction row with Default badge
Row(
    verticalAlignment = Alignment.CenterVertically,
    modifier = Modifier.fillMaxWidth()
) {
    Column(modifier = Modifier.weight(1f)) {
        Text("निकासी — ₹2,000 → SBI", style = WiomType.labelMd)
        Text("12 अप्रैल, 2026", style = WiomType.bodySm)
    }
    WiomBadgeLabel(
        text = "असफल",
        size = WiomBadgeSize.Default,
        color = WiomBadgeColor.Negative,
        style = WiomBadgeStyle.Filled,
    )
}
```

---

## 11. Strict Rules

### DO
- Use the decision tree to pick type → size → style → color
- Use `type.meta.xs` for Count, `type.label.md` for Default, `type.label.sm` for Small
- Use `radius/tiny` (4px) for Count and Label, `radius/full` for Dot
- Hide Count badge when count = 0
- Cap Count display at 9+ or 99+
- Use Filled for terminal states, Tinted for transitional
- Position Dot badges top-right on icons/avatars

### DO NOT
- Do NOT make badges tappable — they are passive indicators
- Do NOT put icons inside badges
- Do NOT use Filled style on Small badges
- Do NOT show Count = 0
- Do NOT stack multiple badges on one row
- Do NOT use generic text ("Status", "State") — use specific Hindi/English copy
- Do NOT hardcode colors or spacing — use foundation tokens
- Do NOT use badges for navigation or interactive filtering

### WHEN IN DOUBT
Ask: "Is this passive information (badge) or interactive control (chip)?" — if passive, use badge. If interactive, use chip.
