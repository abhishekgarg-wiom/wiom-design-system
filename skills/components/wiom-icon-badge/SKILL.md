---
name: wiom-icon-badge
description: >
  Wiom Icon Badge (`icon_with_bg`) — a single glyph inside a filled,
  always-circular container. 1 component · 3 sizes (Sm 24 / Md 48 /
  Lg 96 dp) · 6 tones (Neutral · Brand · Positive · Warning ·
  Critical · Info) → 18 effective combinations. Figma ships only
  the Neutral tone; the other 5 tones are synthesised from
  foundations bg.*.subtle + icon.* token pairs. Display-only — never
  a tap target on its own (the parent row / card / CTA owns the
  tap). Distinct from `wiom-badge` (count / dot / label chip).
  Reference for generating Jetpack Compose code with `WiomIconBadge`
  and for instancing the Figma component (frame `3063:10031`) via
  MCP. Use this skill EVERY TIME you render a glyph inside a
  coloured circular background — list-row leading affordance, empty-
  state hero, settings entry, status summary, inline information
  hit. Trigger on: icon-with-bg, icon container, settings icon,
  list leading icon, status icon bubble, empty-state icon, or any
  Compose call that wraps an Icon inside a Box with a circular
  background. Pairs with wiom-design-foundations (tokens),
  wiom-list-item (primary host),
  wiom-dialog (Alert type header), and wiom-compose-layout-rules
  (Modifier discipline).
---

# Wiom Icon Badge — `icon_with_bg`

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2),
component frame `3063:10031` (on the Selection Controls canvas
`3069:5843`), and the `wiom-design-foundations` token set.

**1 component · 3 sizes · 6 tones · 18 effective combinations · always full-round · display-only.**

| Size | Container | Glyph | All-sides padding | Use for |
|---|---|---|---|---|
| `Sm` | 24 × 24 dp | 16 dp | `space-4` (4 dp) | Leading slot of compact list rows · inline within toast / chat / completion meter |
| `Md` | 48 × 48 dp | 24 dp | `space-12` (12 dp) | Comfortable list-row leading · settings entry · category row · status summary card |
| `Lg` | 96 × 96 dp | 48 dp | 24 dp | Empty-state hero · onboarding step illustration |

**Distinct from `wiom-badge`.** Badge is the count / dot / label chip (numeric or text-bearing). Icon Badge is glyph-in-container (visual only, no text). Different anatomy, different purpose — don't confuse them.

---

## 1. Figma — instancing the component

Instance the component frame `3063:10031` and set `Property 1` to one of the three size strings. Strings are **case-sensitive**. Tones are **not** Figma variants today — only Neutral ships; the other five tones are synthesised in code from foundations tokens (see §5).

### Variants — 3 ship sizes (Neutral tone only in Figma)

| `Property 1` | Node | Container | Glyph | Padding |
|---|---|---|---|---|
| `24 x 24` | `3115:7508` | 24 dp · `radius-full` · `bg.subtle` | 16 dp | `space-4` H · 4 dp V (Figma reads `radius/tiny` here, same 4 dp value) |
| `48 x48` | `3063:10030` | 48 dp · `radius-full` · `bg.subtle` | 24 dp | `space-12` all sides |
| `96 x 96` | `3063:10032` | 96 dp · `radius-full` · `bg.subtle` | 48 dp | `space-24` (24 dp) all sides |

### Properties (apply per instance)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Property 1` | Variant | `48 x48` | The size — picks one of the three ship rows above |
| Glyph (instance swap) | Instance swap | `settings` (1232:5391) | Swap to any 16 / 24 / 48 dp Material Symbols Rounded glyph (size matches the variant) or a `painterResource(R.drawable.ic_wiom_*)` for a brand drawable |

> **Tones are not in Figma yet.** Neutral (`bg.subtle` + `icon.nonAction`) is the only tone Figma ships. Brand · Positive · Warning · Critical · Info are synthesised from foundations token pairs at code time — don't wait for Figma variants. See §5 for the full pair table.

### Code enum ↔ Figma variant string

| Kotlin | Figma `Property 1` value |
|---|---|
| `WiomIconBadgeSize.Sm` | `24 x 24` |
| `WiomIconBadgeSize.Md` | `48 x48` *(note: Figma's value lacks the space; case-sensitive)* |
| `WiomIconBadgeSize.Lg` | `96 x 96` |

| Kotlin | Tone (synthesised in code) |
|---|---|
| `WiomIconBadgeTone.Neutral` (Figma default) | `bg.subtle` + `icon.nonAction` |
| `WiomIconBadgeTone.Brand` | `bg.brand.subtle` + `icon.brand` |
| `WiomIconBadgeTone.Positive` | `bg.positive.subtle` + `icon.positive` |
| `WiomIconBadgeTone.Warning` | `bg.warning.subtle` + `icon.warning` |
| `WiomIconBadgeTone.Critical` | `bg.critical.subtle` + `icon.critical` |
| `WiomIconBadgeTone.Info` | `bg.info.subtle` + `icon.info` |

---

## 2. Anatomy

All three sizes share the same envelope — circular container, single glyph centred inside, padding derived from `(container − glyph) / 2`.

```
        ┌─── Container ───┐         Container = glyph + 2 × padding
        │                  │         (24 = 16+8 ·  48 = 24+24 ·  96 = 48+48)
        │      ●●●●        │
        │      ●  ●        │         radius-full · always
        │      ●●●●        │         filled · no border
        │                  │         no shadow
        └──────────────────┘
              ↑ glyph
```

(Per-size dimensions, padding, and per-tone color pairs live in §5 — single source of truth.)

The badge is **content-sized** to its container. Don't apply `Modifier.size(…)` from the call site — the size enum is the contract.

---

## 3. Surface contract — intrinsic-sized · centered · display-only

The icon badge is one of the few Wiom components where the **component owns its size and the parent owns the tap.** Two implications:

### Intrinsic sizing (no `Modifier.size` overrides)

- The composable internally applies `Modifier.size(24.dp / 48.dp / 96.dp)` keyed off the `size` enum. Don't override from the call site — `size` is the contract.
- The `modifier` parameter accepts **alignment and parent-driven padding only**. No `width(…)`, no `height(…)`, no `size(…)`.

### Display-only — no tap handler

- `WiomIconBadge` does **not** accept an `onClick` parameter. The parent row, card, dialog, or CTA owns the tap and handles a11y semantics (`Modifier.semantics { role = Role.Button }` belongs to the parent).
- This matters most at `Sm` (24 dp): a 24 dp standalone tap target fails WCAG 2.5.5 (≥ 48 dp). When `Sm` ships standalone, wrap the parent with `Modifier.minimumInteractiveComponentSize().clickable { … }` — the badge stays purely decorative inside.

### Foundations compliance

Filled, not bordered. Shadow XOR border (`shadow vs border` foundations rule) — and the icon badge is filled with no shadow. If you find yourself reaching for `Modifier.shadow` or `Modifier.border` on this component, you've drifted; back out and reconsider whether the design needs a different component (e.g., `wiom-list-item` row, `wiom-card`).

---

## 4. Decision logic — Size + Tone

### Which size?

```
Inside a list-item leading slot, compact / dense row    → Sm (24 dp)
Inside a list-item leading slot, comfortable row        → Md (48 dp)
Settings entry / category row with prominent icon       → Md (48 dp)
Status summary card ("Plan Active", "Action required")  → Md (48 dp)
Empty-state hero (icon, not full illustration)          → Lg (96 dp)
Onboarding step icon                                    → Lg (96 dp)
Inline within toast / chat bubble / completion meter    → Sm (24 dp)
Bigger than 96 dp?                                      → Use a real illustration drawable, not this
```

### Which tone?

```
Default · decorative · neutral info                     → Neutral
Featured · promoted · brand-flagged                     → Brand
Success · completed step · all-clear                    → Positive
Warning · attention required · expiring soon           → Warning
Error · failed · critical alert                         → Critical
Informational tip · help · neutral system message       → Info
```

### Standalone or inside another component?

| Pattern | Hit-target ownership |
|---|---|
| Inside `wiom-list-item` (primary use) | The row — full row click → `onSelect`; badge is decorative |
| Inside `wiom-dialog` Alert header | The dialog body is non-interactive; badge is decorative |
| Standalone in an empty-state hero | No tap needed — the page CTA below handles action |
| Standalone with click intent | Wrap parent with `Modifier.minimumInteractiveComponentSize().clickable { }` — badge stays decorative |

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Per-size structural tokens (same across all tones)

| Slot | Sm | Md | Lg |
|---|---|---|---|
| Container size | 24 dp | 48 dp | 96 dp |
| Container radius | `radius-full` (9999) | `radius-full` | `radius-full` |
| Padding | `space-4` (4 dp) | `space-12` (12 dp) | `space-24` (24 dp) |
| Glyph size | 16 dp (`icon.xs`) | 24 dp (`icon.md`) | 48 dp (`icon.lg`) |
| Stroke | none | none | none |
| Shadow | none | none | none |

### Per-tone color pair (container fill + glyph tint)

| Tone | Container fill | Glyph tint |
|---|---|---|
| `Neutral` | `bg.subtle` (#F1EDF7) | `icon.nonAction` (#898296) |
| `Brand` | `bg.brand.subtle` (#FFE5F6) | `icon.brand` (#D9008D) |
| `Positive` | `bg.positive.subtle` | `icon.positive` |
| `Warning` | `bg.warning.subtle` | `icon.warning` |
| `Critical` | `bg.critical.subtle` (#FFE5E7) | `icon.critical` (#D92130) |
| `Info` | `bg.info.subtle` | `icon.info` |

> **Always pair within a family.** `bg.warning.subtle` ⇿ `icon.warning`. Never cross-pair (e.g., `bg.brand.subtle` + `icon.positive`) — the foundation tokens are tuned for AA contrast within the family. Cross-pairing breaks contrast and reads as a bug.

> **`icon.warning` is only legible on warning-tinted bg.** Don't use `icon.warning` (gold #E2B203) on `bg.default` or any non-warning surface — it fails contrast there. Foundations rule.

---

## 6. Universal rules

1. **Always full-round.** `radius-full` on the container in every size, every tone. No `radius-medium` rounded-square variant exists for this component — if a design asks for one, that's a different component (likely a custom illustration container or `wiom-card`).
2. **Always filled · no border · no shadow.** Foundations cross-rule (shadow XOR border, and this is filled). The fill is the only visual weight.
3. **Single glyph only.** One `Icons.Rounded.*` or one `painterResource(R.drawable.ic_wiom_*)`. Never two glyphs, never text, never a count overlay (that's `wiom-badge`), never a multi-color illustration (use an illustration drawable).
4. **Display-only — no tap handler on the badge.** Tap belongs to the parent row / card / CTA. `Sm` (24 dp) standalone fails the ≥ 48 dp WCAG hit target — wrap the parent in a 48 dp clickable surface, keep the badge decorative.
5. **Pair container and glyph from the same tone family.** `bg.brand.subtle` + `icon.brand`, `bg.warning.subtle` + `icon.warning`, etc. Never cross-pair.
6. **One size per use site.** Don't pass `Modifier.size(…)` from the call site to enlarge / shrink the badge — the `size` enum is the contract. If a design wants 32 dp or 64 dp, ask whether it really should be `Sm` or `Md`; the system has 3 sizes by design.
7. **`icon.warning` is warning-bg-only.** Never tint a glyph `icon.warning` outside a warning-tinted container.
8. **Tokens only — element-first.** No raw hex except the documented edge cases (none in this component). No legacy `color.brand.primary`.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomIconBadgeSize { Sm, Md, Lg }
enum class WiomIconBadgeTone { Neutral, Brand, Positive, Warning, Critical, Info }

@Composable
fun WiomIconBadge(
    icon: ImageVector,                                      // or use the painter overload below
    size: WiomIconBadgeSize = WiomIconBadgeSize.Md,
    tone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    contentDescription: String? = null,                     // null = decorative; non-null when the badge itself conveys meaning
    modifier: Modifier = Modifier,                          // alignment / parent padding only — no size overrides
)

// Drawable / painter overload — for ic_wiom_* and ic_partner_* brand glyphs.
@Composable
fun WiomIconBadge(
    @DrawableRes iconRes: Int,
    size: WiomIconBadgeSize = WiomIconBadgeSize.Md,
    tone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    contentDescription: String? = null,
    modifier: Modifier = Modifier,
)
```

> **No `onClick`.** Display-only by design. Tap belongs to the parent.

### Implementation

```kotlin
@Composable
fun WiomIconBadge(
    icon: ImageVector,
    size: WiomIconBadgeSize = WiomIconBadgeSize.Md,
    tone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    contentDescription: String? = null,
    modifier: Modifier = Modifier,
) {
    val dims = dimsFor(size)
    val colors = colorsFor(tone)

    Box(
        modifier = modifier
            .size(dims.container)                            // intrinsic — don't override from caller
            .background(colors.containerFill, CircleShape),
        contentAlignment = Alignment.Center,
    ) {
        Icon(
            imageVector = icon,
            contentDescription = contentDescription,
            tint = colors.glyphTint,
            modifier = Modifier.size(dims.glyph),
        )
    }
}

@Composable
fun WiomIconBadge(
    @DrawableRes iconRes: Int,
    size: WiomIconBadgeSize = WiomIconBadgeSize.Md,
    tone: WiomIconBadgeTone = WiomIconBadgeTone.Neutral,
    contentDescription: String? = null,
    modifier: Modifier = Modifier,
) {
    val dims = dimsFor(size)
    val colors = colorsFor(tone)

    Box(
        modifier = modifier
            .size(dims.container)
            .background(colors.containerFill, CircleShape),
        contentAlignment = Alignment.Center,
    ) {
        Icon(
            painter = painterResource(iconRes),
            contentDescription = contentDescription,
            tint = colors.glyphTint,
            modifier = Modifier.size(dims.glyph),
        )
    }
}

private data class IconBadgeDims(val container: Dp, val glyph: Dp)
private data class IconBadgeColors(val containerFill: Color, val glyphTint: Color)

private fun dimsFor(size: WiomIconBadgeSize): IconBadgeDims = when (size) {
    WiomIconBadgeSize.Sm -> IconBadgeDims(container = 24.dp, glyph = 16.dp)
    WiomIconBadgeSize.Md -> IconBadgeDims(container = 48.dp, glyph = 24.dp)
    WiomIconBadgeSize.Lg -> IconBadgeDims(container = 96.dp, glyph = 48.dp)
}

@Composable
private fun colorsFor(tone: WiomIconBadgeTone): IconBadgeColors = when (tone) {
    WiomIconBadgeTone.Neutral  -> IconBadgeColors(Wiom.bg.subtle,           Wiom.icon.nonAction)
    WiomIconBadgeTone.Brand    -> IconBadgeColors(Wiom.bg.brand.subtle,     Wiom.icon.brand)
    WiomIconBadgeTone.Positive -> IconBadgeColors(Wiom.bg.positive.subtle,  Wiom.icon.positive)
    WiomIconBadgeTone.Warning  -> IconBadgeColors(Wiom.bg.warning.subtle,   Wiom.icon.warning)
    WiomIconBadgeTone.Critical -> IconBadgeColors(Wiom.bg.critical.subtle,  Wiom.icon.critical)
    WiomIconBadgeTone.Info     -> IconBadgeColors(Wiom.bg.info.subtle,      Wiom.icon.info)
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material Surface used to fake the badge, hardcoded 32 dp size,
//             border + shadow, click handler on the badge itself
Surface(
    onClick = onTap,                                       // tap belongs to parent, not badge
    shape = CircleShape,
    color = Color(0xFFFFE5F6),                             // raw hex, no token
    border = BorderStroke(1.dp, Color.Gray),               // never bordered
    shadowElevation = 2.dp,                                // never shadowed
    modifier = Modifier.size(32.dp),                       // 32 isn't a Wiom size
) {
    Icon(Icons.Default.Settings, null)                     // wrong icon family (Default → Rounded)
}

// ✅ RIGHT — intrinsic-sized, tone-paired tokens, no tap, Rounded icon family
WiomIconBadge(
    icon = Icons.Rounded.Settings,
    size = WiomIconBadgeSize.Md,
    tone = WiomIconBadgeTone.Brand,                         // bg.brand.subtle + icon.brand pair
    contentDescription = null,                              // decorative — parent row owns a11y
)
```

---

## 8. Recipes

### List-row leading (primary use)

```kotlin
WiomListItem(
    headline = "Profile",
    supportingText = "Manage your account details",
    leadingContent = {
        WiomIconBadge(
            icon = Icons.Rounded.PersonOutline,
            size = WiomIconBadgeSize.Md,
            tone = WiomIconBadgeTone.Neutral,               // settings rows = neutral by default
        )
    },
    onClick = onProfile,                                    // row owns the tap
)
```

### Status summary card

```kotlin
Card(modifier = Modifier.fillMaxWidth()) {
    Row(
        modifier = Modifier.padding(16.dp),
        horizontalArrangement = Arrangement.spacedBy(12.dp),
        verticalAlignment = Alignment.CenterVertically,
    ) {
        WiomIconBadge(
            icon = Icons.Rounded.Check,
            size = WiomIconBadgeSize.Md,
            tone = WiomIconBadgeTone.Positive,              // success state
        )
        Column {
            Text("Plan Active", style = Wiom.type.label.lg, color = Wiom.text.default)
            Text("Renews on 28 Apr 2026", style = Wiom.type.body.md, color = Wiom.text.subtle)
        }
    }
}
```

### Empty-state hero

```kotlin
Column(
    modifier = Modifier.fillMaxWidth().padding(24.dp),
    horizontalAlignment = Alignment.CenterHorizontally,
    verticalArrangement = Arrangement.spacedBy(16.dp),
) {
    WiomIconBadge(
        icon = Icons.Rounded.AccountBalanceWallet,
        size = WiomIconBadgeSize.Lg,                        // 96 dp hero
        tone = WiomIconBadgeTone.Neutral,
    )
    Text("No transactions yet", style = Wiom.type.heading.md, color = Wiom.text.default)
    Text(
        "Recharge to see your transaction history here.",
        style = Wiom.type.body.lg,
        color = Wiom.text.subtle,
        textAlign = TextAlign.Center,
    )
}
```

### Inline within a toast (compact)

```kotlin
Row(verticalAlignment = Alignment.CenterVertically, horizontalArrangement = Arrangement.spacedBy(8.dp)) {
    WiomIconBadge(
        icon = Icons.Rounded.Warning,
        size = WiomIconBadgeSize.Sm,                        // 24 dp inline
        tone = WiomIconBadgeTone.Warning,
    )
    Text("Plan expires in 2 days", style = Wiom.type.body.md, color = Wiom.text.default)
}
```

### Brand drawable (partner / customer brand glyph)

```kotlin
WiomIconBadge(
    iconRes = R.drawable.ic_wiom_pro,
    size = WiomIconBadgeSize.Md,
    tone = WiomIconBadgeTone.Brand,
    contentDescription = "Wiom Pro",                        // brand glyph is meaningful, not decorative
)
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Rounded-square container — use a different component (`wiom-card` or a custom rounded-rect helper).
- ❌ Multi-color illustration inside the container — use an illustration drawable (`R.drawable.illus_*`), not this component.
- ❌ A count overlay or text on top of the badge — that's `wiom-badge`. Compose them as siblings if you need both: badge + dot indicator.
- ❌ Using `Sm` (24 dp) as the sole tap target — fails WCAG ≥ 48 dp. Wrap the parent.
- ❌ Cross-tone pairing — `bg.brand.subtle` + `icon.positive` etc. Stick to same-family pairs.
- ❌ Custom 32 / 64 dp size — three sizes by design (Sm 24 / Md 48 / Lg 96). Pick one.

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Icon Badge frame: `3063:10031` (lives on the Selection Controls canvas `3069:5843`; not a stand-alone canvas).
Re-pull `get_variable_defs` on `3063:10031` if foundations token names ever change.

| Node | Variant |
|---|---|
| `3063:10031` | `icon_with_bg` component frame — 3 ship sizes |
| `3115:7508` | `Property 1 = 24 x 24` (Sm) |
| `3063:10030` | `Property 1 = 48 x48` (Md) — note the missing space in the variant string |
| `3063:10032` | `Property 1 = 96 x 96` (Lg) |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- **Tones are not Figma variants.** Figma ships Neutral only (`bg.subtle` + `icon.nonAction`). Brand · Positive · Warning · Critical · Info are synthesised in code from foundations token pairs. Don't wait for Figma variants — the tokens are stable.
- **The `48 x48` variant string has no space** between `48` and `x48` (the `24 x 24` and `96 x 96` variants do have spaces). Match Figma's spelling exactly when instancing — variant strings are case- and whitespace-sensitive.
- **Some Figma variants render dimensions as raw numbers instead of tokens** — `Sm` vertical padding via `radius/tiny` (= 4 dp), `Lg` padding as raw `24px` (bind it to `space-24` in code), and the `Lg` radius as `19998px` instead of the `radius-full` token. These are authoring quirks; visually identical to the canonical tokens. In code, always use the tokens (`space-4` / `radius-full`) and treat 24 dp as a derived dimension.
- **The icon-badge component is parented to the Selection Controls canvas** (`3069:5843`) in Figma. That's a stray instance — the icon badge is conceptually independent. The selection-controls skill explicitly excludes the `icon_with_bg` frame from its variant count.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.subtle` / `bg.brand.subtle` / `bg.positive.subtle` / `bg.warning.subtle` / `bg.critical.subtle` / `bg.info.subtle`, `icon.nonAction` / `icon.brand` / `icon.positive` / `icon.warning` / `icon.critical` / `icon.info`, `radius-full`, `space-4` / `space-12`. Also the warning-tone-only-on-warning-bg rule.
- `wiom-badge` — the **count / dot / label chip** family. Different component: text-bearing or numeric, not glyph-only. Don't confuse the two.
- `wiom-list-item` — the primary host for `Sm` and `Md` icon-badges in the leading slot.
- `wiom-dialog` — the Alert dialog header is a `Md` icon-badge with status-coded tone.
- `wiom-compose-layout-rules` — `Icons.Rounded.*` enforcement; no `Modifier.size(…)` overrides on intrinsic-sized components.
