---
name: wiom-bottomsheet
description: Wiom Bottom Sheet component spec, anatomy, token contract, property API, and Figma↔Kotlin mapping. Use this skill EVERY TIME you work with bottom sheets, modal sheets, drawer-style surfaces, option menus, payment confirmations, success/error feedback sheets, or any Figma design containing a handle-topped surface anchored to the screen bottom. Trigger when user mentions "bottom sheet", "modal sheet", "drawer", "option menu", "confirm sheet", "success sheet", "error sheet", "payment confirm", "sort/filter sheet", or shares a Figma design with a drag-handle + content + CTA layout. Also trigger when implementing bottom sheet UI in Kotlin/Compose for Wiom Android app.
---

# Wiom Bottom Sheet — Component Skill

You are generating a design or UI that includes a bottom sheet for the Wiom product ecosystem. Use ONLY the approved tokens, composition rules, and property API below. This skill covers the Bottom Sheet container + its Header sub-component + the Safe Space pattern. For CTA configuration inside the action bar, defer to the `wiom-cta` skill (§4 Button Group).

---

## Where it lives

- **Figma file:** Wiom DS - Next · `ZPErh7YU7omdbyy2o268Qo`
- **Page:** `Saurav / Bottomsheet`
- **Rule (hard):** all Bottom Sheet + Bottom Sheet Header changes happen on `Saurav / Bottomsheet` only. Never build duplicates on other pages.

### Component IDs

| Component | Node ID | Type |
|---|---|---|
| `Bottom Sheet` | `3022:424` | COMPONENT_SET (5 Size variants) |
| `Bottom Sheet Header` | `2993:8` | COMPONENT (booleans + text props) |
| `BS Illustration` | `2958:17` | COMPONENT_SET (5 Tone variants) |

### Composed-from (external, must stay in sync)

| Component | Node ID | Lives on |
|---|---|---|
| CTA | `2038:116` | `Saad / Components / CTA` |
| Button Group | `2982:357` | `Saad / Components / CTA` |
| List Item | `1655:487` | `Saurav / List items` |
| Foundation Icons | — | `Foundation / icons` |

---

## Context — why these rules matter

Wiom serves lower- and middle-income Indian households on budget Android devices. Bottom sheet decisions must account for:
- **Mobile-first** — small screens, outdoor use, thumb reach
- **Trust-heavy** — payment confirmations, consent, policy details
- **Bilingual** — Hindi + English, Devanagari is taller at the same size
- **Low cognitive load** — clear hierarchy, scannable rows, obvious dismiss affordance

---

## Component overview

```
PURPOSE:    Surface anchored to screen bottom for supplementary content and contextual actions.
WIDTH:      360px (standard Android viewport). Fills screen width.
TYPES:      Standard (no scrim) and Modal (scrim overlay) — controlled via `Is Modal` boolean.
LAYOUTS:    List-based (Compact / Half / Expanded / Full), Illustration-based (Illustration / Illustration Left), Share, and Form.
DISMISS:    Drag handle (primary) + scrim tap (Modal only) + back button.
SHADOW:     shadow.md effect style on the Sheet Surface.
NOTE:       Hover/Focused states excluded from V1 (mobile-first).
```

---

## Property API (Bottom Sheet)

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Size` | Variant | `Compact` | 8 options: Compact · Half · Expanded · Full · Illustration · Illustration Left · Share · Form |
| `Is Modal` | Boolean | `false` | Shows/hides scrim overlay above the sheet |
| `Show Handle` | Boolean | `true` | Toggles drag handle container |
| `Show Title` | Boolean | `true` | Toggles entire Header instance + divider (list variants only) |
| `Show Button Group` | Boolean | `true` | Toggles Action Bar (containing Button Group) |
| `Show Top Safe Space` | Boolean | `false` | Reveals 16px top spacer (use when `Show Title = false`) |
| `Show Bottom Safe Space` | Boolean | `false` | Reveals 16px bottom spacer (use when `Show Button Group = false`) |
| `Heading` | Text | `"Heading"` | Illustration variant heading |
| `Subtext` | Text | `"Description…"` | Illustration variant subtext |

**Design rule — why booleans over variants:** 2-choice properties are booleans per the Wiom design-system convention. `Size` is the only variant dimension because it has 5 values.

### Nested component properties (bubble up via instance overrides)

**Bottom Sheet Header properties** (access via the nested Header instance in a Bottom Sheet):

| Property | Type | Default |
|---|---|---|
| `Title` | Text | `"Title"` |
| `Subtitle` | Text | `"Subtitle description"` |
| `Trailing Action Label` | Text | `"Action"` |
| `Show Leading Icon` | Boolean | `false` |
| `Show Title` | Boolean | `true` |
| `Show Subtitle` | Boolean | `false` |
| `Show Trailing Action` | Boolean | `false` |
| `Leading Icon` | INSTANCE_SWAP | `arrow_back` |

**Button Group properties** (nested via Action Bar — see `wiom-cta` skill (§4 Button Group)):
- `Count`: 1 · 2 · 3
- `Layout`: Side by side · Stacked
- Per-CTA `Type` override for Destructive / Pre-booking / Acknowledge variants

**BS Illustration properties** (Illustration variant only):
- `Tone`: Neutral · Success · Error · Warning · Info (5 semantic tones)

---

## Anatomy

### List layout (Compact / Half / Expanded / Full)

```
Root [VERTICAL, 360 wide, HUG height]
├── Scrim                              ← visible: Is Modal
└── Sheet Surface [VERTICAL, shadow.md, top radius xlarge, fill bg/default]
    ├── Handle container               ← visible: Show Handle
    │   └── Indicator 32×4 pill
    ├── Top Safe Space (16px spacer)   ← visible: Show Top Safe Space
    ├── Header (BS Header instance)    ← visible: Show Title
    ├── Divider (1px)                  ← visible: Show Title
    ├── Content
    │   └── List Item instances (2 / 4 / 7 / 7)
    ├── Action Bar                     ← visible: Show Button Group
    │   └── Button Group instance
    └── Bottom Safe Space (16px)       ← visible: Show Bottom Safe Space
```

### Illustration layout

```
Root [VERTICAL, 360 wide, HUG height]
├── Scrim                              ← visible: Is Modal
└── Sheet Surface
    ├── Handle container
    ├── Top Safe Space                 ← visible: Show Top Safe Space
    ├── Illustration Block [VERTICAL, center-aligned]
    │   ├── BS Illustration instance (120×120)
    │   └── Text column
    │       ├── Heading (center)
    │       └── Subtext (center)
    ├── Action Bar                     ← visible: Show Button Group
    │   └── Button Group instance (Stacked default for illustration)
    └── Bottom Safe Space              ← visible: Show Bottom Safe Space
```

### BS Header anatomy

```
Header [HORIZONTAL, padding 16/16/8/16, center-aligned]
├── Leading Icon (24dp)                ← visible: Show Leading Icon (INSTANCE_SWAP)
├── Text Column [VERTICAL, FILL, gap 4]
│   ├── Title (type.heading.md)        ← visible: Show Title · characters: Title
│   └── Subtitle (type.body.sm)        ← visible: Show Subtitle · characters: Subtitle
└── Trailing Action (type.label.md, text/brand)  ← visible: Show Trailing Action
```

---

## Size presets

| Size | Height (approx) | Content | Action Bar default |
|---|---|---|---|
| Compact | ~200px | Handle + Header + 2 items | Visible |
| Half | ~370px | Handle + Header + 4 items | Visible |
| Expanded | ~600px | Handle + Header + 7 items | Visible |
| Full | ~600px | Handle + Header + 7 items (scrollable in real use) | Visible |
| Illustration | ~300px (hug) | Handle + centered Illustration Block (image top, text below) | Visible (Stacked default) |
| **Illustration Left** | ~300px (hug) | Handle + left-aligned Illustration Block (image left, text right) | Visible (Stacked default) |
| **Share** | ~220px (hug) | Handle + Header + horizontal row of share targets (chips / icons) + optional "More" row | Optional |
| **Form** | ~400px (hug) | Handle + Header + 1–3 input fields inline (from `wiom-input-fields`) + CTAs | Visible |

---

## Spacing contract (per `Wiom-spacing` skill)

This follows the **Bottom Sheet recipe**: 16px all-sides base, 16px Title → content, 24px Content → action buttons, 12px between CTAs.

```
Handle container      → padding top 8, bottom 8
Top Safe Space (when shown) → 16px tall spacer
Header                → padding 16L, 16R, 8T, 16B (16 = Title → content)
Divider               → 1px, FILL width, stroke/subtle
List Items            → each has 12 internal vertical padding (inherit from List Item)
Action Bar            → padding 16L, 16R, 24T, 16B (24 = Content → CTAs loose gap)
  └── Button Group    → 12px gap between CTAs (inherit from Button Group)
Bottom Safe Space (when shown) → 16px tall spacer
```

### Illustration Block spacing

```
Illustration Block    → padding 16L, 16R, 16T, 24B · gap 16 (circle → text)
Text column           → gap 8 (Heading → Subtext)
```

### Why no top border on Action Bar

Per the spacing skill, **related = tight, unrelated = loose**. Content and the CTA bar are DIFFERENT concerns — 24px loose gap (Action Bar's `paddingTop`) delivers the visual separation. A 1px border would be redundant and clutter. Don't add one.

---

## Token reference

All visual values bind to the file's variable collections. NEVER hardcode.

### Surface

| Property | Token |
|---|---|
| Sheet fill | `bg/default` (#FAF9FC) |
| Scrim fill | `overlay/scrim` (rgba #161021 @ 50%) |
| Top corner radius | `radius-xlarge` (24) |
| Bottom corner radius | `radius-none` (0) |
| Sheet shadow | `shadow.md` effect style |

### Handle

| Property | Token |
|---|---|
| Indicator size | 32 × 4 |
| Indicator fill | `stroke/strong` |
| Indicator radius | `radius-full` (9999) |

### Header

| Property | Token |
|---|---|
| Title style | `type.heading.md` (20sp Bold 28LH) |
| Title color | `text/default` |
| Subtitle style | `type.body.sm` (12sp Regular 16LH) |
| Subtitle color | `text/subtle` |
| Trailing Action style | `type.label.md` |
| Trailing Action color | `text/brand` (acts as link) |
| Leading Icon color | `icon/nonAction` |

### Divider

| Property | Token |
|---|---|
| Thickness | 1px |
| Fill | `stroke/subtle` |

### Action Bar

| Property | Token |
|---|---|
| Background | transparent (no fill) |
| Top border | **none** — spacing provides separation |
| Button Group gap | 12 (inherited) |

### Illustration tones

| Tone | Circle fill | Icon color | Default icon |
|---|---|---|---|
| Neutral | `bg/brand/subtle` | `icon/brand` | `celebration` |
| Success | `bg/positive/subtle` | `icon/positive` | `check_circle` |
| Error | `bg/critical/subtle` | `icon/critical` | `error` |
| Warning | `bg/warning/subtle` | `icon/warning` | `warning` |
| Info | `bg/info/subtle` | `icon/info` | `info` |

---

## Safe Space pattern (critical behavior)

The Bottom Sheet's bottom and top safe spaces are **conditional** — they appear only when the adjacent "anchor" element (Title or Button Group) is hidden.

### The rule

| Situation | Effective bottom space | Effective top space |
|---|---|---|
| Title shown + BG shown (default) | 16px (from Action Bar's padding-bottom) | 16px (from Handle + Header's top padding) |
| Title shown + BG **hidden** | 0px ❌ → need Bottom Safe Space | 16px ✓ |
| Title **hidden** + BG shown | 16px ✓ | ~20px (Handle's padding only — minimal) → need Top Safe Space for 16+ explicit gap |
| Title hidden + BG hidden | 0px ❌ | ~20px | 

### Platform limitation — no auto-inverse

**Figma's `visible` binding supports only direct-state logic** — there is no inverse (NOT) operator. So `Show Top Safe Space` and `Show Bottom Safe Space` **cannot auto-mirror** `Show Title` and `Show Button Group`. They are paired booleans designers toggle together.

### Designer pairings

| Intent | Toggles |
|---|---|
| Default sheet | All booleans at default |
| Hide title with breathing room | `Show Title = false` + `Show Top Safe Space = true` |
| Hide Button Group with safe area | `Show Button Group = false` + `Show Bottom Safe Space = true` |
| Bare list (no header, no CTAs) | Title=false, BG=false, both safe spaces=true |

**Document this pairing on any Figma Dev Mode handoff** so implementers know the safe space is required.

---

## Composition rules

### Action Bar always uses Button Group

Never recreate buttons inside the Action Bar. Always use a Button Group instance. To vary CTA count, layout, or types, use the Button Group's properties (see `wiom-cta` skill (§4 Button Group)):

- `Count`: 1 / 2 / 3 CTAs
- `Layout`: Side by side (horizontal) / Stacked (vertical)
- Inner CTA `Type` swap: Primary / Secondary / Tertiary / Destructive / Pre-booking / Acknowledge

**3-side-by-side rule (from button-group skill):** When Count=3, Layout=Side by side, NO Primary CTA allowed — the 101dp width collapses the hierarchy. Use Secondary + Tertiary mix, OR switch to Stacked.

### Header is composable

Don't recreate the header markup inline. Always use a `Bottom Sheet Header` instance and configure via its properties. This keeps padding, typography, colors, and leading/trailing patterns consistent across all sheets.

### Illustration tone must be semantic

- `Neutral` → tips, info announcements, first-run celebrations
- `Success` → payment success, activation success, verification done
- `Error` → payment failure, network errors, permission denied, destructive confirmations
- `Warning` → low balance, expiring plan, permission required
- `Info` → explanatory feedback, neutral status changes

---

## Decision trees

### When to use Bottom Sheet

```
Supplementary content without leaving the screen?
  → Bottom Sheet

Pick from a list of contextual actions?
  → Bottom Sheet (list layout, Compact or Half)

Confirm an action with visual context?
  → Bottom Sheet (Illustration + Button Group)

Show success/error/info feedback?
  → Bottom Sheet (Illustration layout)

Short form or flow?
  → Bottom Sheet (Expanded or Full)
```

### When NOT to use Bottom Sheet

```
Critical alert blocking all interaction?
  → Dialog (not Bottom Sheet)

Full page or multi-step flow?
  → New screen (not Bottom Sheet)

Toast-style notification?
  → Snackbar / Toast

Tooltip or hint?
  → Tooltip
```

### Which type (Is Modal)?

```
User may keep interacting with background content?
  → Standard (Is Modal = false)

Requires user attention before continuing?
  → Modal (Is Modal = true)

Payment / delete / consent (critical)?
  → Modal
```

### Which size?

```
≤2 items, quick action?                           → Compact
3-4 items, option menu / picker?                   → Half
5+ items with CTAs?                                → Expanded
Long scrollable content?                           → Full
Centered visual feedback / celebration?            → Illustration
Compact feedback with image left of text?          → Illustration Left
System share targets / Wiom share affordance?      → Share
1–3 inline inputs (e.g., enter PIN + label)?       → Form
```

### Which Button Group layout?

```
1 CTA?                      → Count=1 (layout irrelevant)
Confirm + Cancel?           → Count=2, Side by side (Secondary left, Primary right)
Long labels?                → Count=2, Stacked (Primary top, Secondary below)
3 short options?            → Count=3, Side by side (NO Primary — see button-group skill)
3 with hierarchy?           → Count=3, Stacked (Primary top)
```

---

## Wiom use cases

### Policy quick actions
```
Size:  Half · Type: Standard
Header: "Policy Options" (no subtitle, no icons)
Items: View Policy Details · Make a Payment · Contact Support · Download Documents
CTA:   Show Button Group = false (no action bar)
```

### Payment confirmation
```
Size:  Illustration · Type: Modal
Heading: "Pay Rs 299?"
Subtext: "Monthly recharge for Plan A — 1.5GB/day. Amount will be debited from your UPI account."
Tone:  Neutral
CTA:   Count=2, Stacked — Primary "Pay now" / Secondary "Cancel"
```

### Payment success
```
Size:  Illustration · Type: Modal
Heading: "Payment Successful"
Subtext: "Rs 299 has been charged. Your plan is now active until 15 May 2026."
Tone:  Success
CTA:   Count=1, Stacked — Primary "Done"
```

### Payment failure
```
Size:  Illustration · Type: Modal
Heading: "Payment Failed"
Subtext: "We couldn't process your payment. Please check your UPI app and try again."
Tone:  Error
CTA:   Count=2, Side by side — Secondary "Cancel" / Primary "Retry"
```

### Delete confirmation
```
Size:  Illustration · Type: Modal
Heading: "Delete Account?"
Subtext: "This will permanently remove your account and all associated data. This cannot be undone."
Tone:  Error
CTA:   Count=2, Stacked — Primary swapped to Destructive "Delete" / Secondary "Cancel"
```

### Filter sheet
```
Size:  Half · Type: Modal
Header: Title "Filter Plans" + Subtitle "3 filters applied" + Trailing Action "Clear all"
Items:  Filter rows (Checkbox list item type)
CTA:   Count=2, Side by side — Secondary "Cancel" / Primary "Apply"
```

### Nested flow sheet
```
Size:  Full · Type: Modal
Header: Leading Icon = arrow_back + Title "Terms & Conditions"
Content: Scrollable text
CTA:   Count=1 — Primary "I Agree"
```

---

## Strict rules

### DO

- Use ONLY the tokens listed above — no hardcoded values
- Always include a drag handle (dismiss affordance) unless `Show Handle = false` is explicit
- Use `shadow.md` effect style on the Sheet Surface — no borders
- Top corners `radius-xlarge` only; bottom corners always `radius-none`
- Use Button Group instance inside the Action Bar — never raw CTAs
- Use Bottom Sheet Header instance for headers — never recreate inline
- Use BS Illustration instance for illustration content — never raw icon + circle
- Bind every color/spacing/radius to Figma variables
- Pair `Show Title = false` with `Show Top Safe Space = true` when you need visible breathing room
- Pair `Show Button Group = false` with `Show Bottom Safe Space = true`
- Use BS Illustration `Tone` semantically (Success for wins, Error for failures, etc.)

### DO NOT

- Do NOT add a close button — drag handle is the dismiss affordance
- Do NOT add a top border on the Action Bar — spacing provides separation
- Do NOT use `shadow.xl` — `shadow.md` is standard for both Standard and Modal types (platform limitation: shadow can't be boolean-bound)
- Do NOT nest a bottom sheet inside another bottom sheet
- Do NOT use bottom sheet for blocking alerts — use Dialog
- Do NOT put more than 7 items in Expanded/Full without scrollable content
- Do NOT recreate CTAs, Headers, or Illustrations as custom groups — always instance
- Do NOT hardcode hex values — bind to variables
- Do NOT add bottom corner radius — the sheet anchors to the screen edge
- Do NOT enable `Show CTA Bar` + `Vertical CTAs` booleans simultaneously if this is a V1 design (the 2-boolean pairing conflicts — use only the current `Show Button Group` with a Stacked Button Group)

---

## Anti-pattern checklist

Before submitting any bottom sheet design, verify NONE of these are present:

- [ ] Hardcoded hex values instead of variable bindings
- [ ] Close button present (removed in V1 — drag handle is dismiss)
- [ ] Border on Sheet Surface (shadow only)
- [ ] Border on Action Bar (spacing only)
- [ ] Bottom corners with radius > 0
- [ ] Raw CTA instances instead of Button Group
- [ ] Inline header markup instead of BS Header instance
- [ ] Raw icon + circle instead of BS Illustration instance
- [ ] Missing drag handle (unless Show Handle = false is explicit)
- [ ] Modal type without scrim (Is Modal must be true)
- [ ] 3-Side-by-side Button Group with a Primary CTA
- [ ] Icon tone mismatch (Success tone with "Payment Failed" heading, etc.)
- [ ] Spacing not matching the Bottom Sheet recipe (16/16/8/16 header, 24T/16B Action Bar)
- [ ] Title hidden without Top Safe Space enabled
- [ ] Button Group hidden without Bottom Safe Space enabled
- [ ] Nested bottom sheet (never valid)

---

## Figma ↔ Kotlin mapping

### Bottom Sheet top level

```kotlin
@Composable
fun WiomBottomSheet(
    isModal: Boolean = false,
    showHandle: Boolean = true,
    size: SheetSize = SheetSize.Compact,
    content: @Composable ColumnScope.() -> Unit,
) { ... }
```

| Figma property | Compose mapping |
|---|---|
| `Is Modal` | `ModalBottomSheet(...)` vs `BottomSheetScaffold(...)` |
| `Show Handle` | `dragHandle = { WiomDragHandle() }` or `null` |
| `Size` | Height constraint (Compact 240 / Half 400 / Expanded 600 / Full 756) or Modifier.wrapContentHeight() for Illustration |
| `Heading` / `Subtext` | Text composables inside illustration layout |

### Bottom Sheet Header

```kotlin
@Composable
fun WiomBottomSheetHeader(
    title: String,
    subtitle: String? = null,
    leadingIcon: Painter? = null,
    trailingActionLabel: String? = null,
    onTrailingClick: (() -> Unit)? = null,
) { ... }
```

### Safe spaces

In Compose, safe spaces aren't needed — use `Modifier.navigationBarsPadding()` on the root sheet for bottom, and standard top padding. The Figma booleans exist only to represent the visual state in designs; Kotlin implementations get this for free from system insets.

### Illustration tone

```kotlin
enum class IllustrationTone { Neutral, Success, Error, Warning, Info }
```

Maps to circle fill + icon tint:
- Neutral → `colors.bg.brand.subtle` + `colors.icon.brand`
- Success → `colors.bg.positive.subtle` + `colors.icon.positive`
- Error → `colors.bg.critical.subtle` + `colors.icon.critical`
- Warning → `colors.bg.warning.subtle` + `colors.icon.warning`
- Info → `colors.bg.info.subtle` + `colors.icon.info`

---

## Known gaps (V1) — deferred work

These Material Design 3 patterns are NOT yet in the Wiom Bottom Sheet. Each is a separate work stream:

- **Dark mode** — blocked until Color collection gains a Dark mode
- **Keyboard push-up / form sheet** — when IME opens (text field inside sheet)
- **Search header variant** — autocomplete sheets
- **Grid content layout** — share sheet / contact picker patterns
- **Tabs inside sheet** — multi-section content switcher
- **Motion tokens** — spring-drag physics, snap points
- **Hover / Focused states** — excluded in V1, required if extending to tablet/web
- **CTA Label text property** — CTA component doesn't expose label; swapping CTA text requires drilling into the nested CTA inside Button Group

When these are scoped, they become new variants or properties on this same Bottom Sheet set.

---

## Related skills

- **`wiom-cta`** (Button Group) — CTA count, layout, 3-side-by-side rules, stacked vs side-by-side guidance
- **`wiom-list-item`** — the List Item component used inside Content
- **`wiom-design-foundations`** — color / type / icon / spacing / radius / stroke / shadow tokens
- **`Wiom-spacing`** — the 4px grid, bottom sheet recipe, gap-only convention, touch target tiers
- **(memory) `wiom-ds-project.md`** — file state, page map, and component registry for Wiom DS - Next

---

## When to read this skill

- Before creating, editing, or reviewing any bottom sheet in a Figma design
- Before generating Kotlin / Compose code for bottom sheet UI
- When the user references "bottom sheet", "modal sheet", "option menu", "payment confirm", "success sheet", "error sheet", "drawer", "filter sheet", or shares a Figma node with a drag-handle + anchored surface
- When the user asks to benchmark a sheet against Material Design — cross-reference the "Known gaps" section before proposing new variants
- **HARD RULE:** read this BEFORE any `use_figma` call that creates or modifies a Bottom Sheet component, instance, or variant on `Saurav / Bottomsheet`

---

## Picker patterns (replaces the deprecated `wiom-dropdown` menu UX)

The old `wiom-dropdown` skill is deleted. Its **field trigger** lives in `wiom-input-fields` (Dropdown field). Its **menu UX** — the open-list surface — lives here: a bottom sheet built from `wiom-list-item` rows. There is no popover / anchored menu in Wiom mobile; picker lists always open bottom-anchored.

### Single-select picker

- `Size=Half` (up to ~4 options visible) or `Expanded` (5+) or `Full` (long scrollable lists)
- `Is Modal=true` (picker is a commit moment — the scrim blocks background taps)
- Header: `Title` = the field's label (e.g., "Choose language"), `Show Subtitle=false`, `Show Trailing Action=false`
- Rows: `wiom-list-item` with `Type=Radio` — one per option. Radio Selection = Yes on the currently chosen row
- `Show Button Group=false` — tapping a row commits immediately and closes the sheet
- Close behavior: Scrim tap / drag-down / back = cancel (reverts to pre-open value); row tap = commit

```
┌─ 360w · Half ─────────────────────┐
│         ▬▬ (handle)                │
│  Choose language                   │
│  ─────────────────────────────    │
│  ( ) English                       │  ← List Item, Type=Radio, Selection=No
│  (●) हिन्दी                        │  ← List Item, Type=Radio, Selection=Yes
│  ( ) मराठी                         │
│  ( ) தமிழ்                         │
└────────────────────────────────────┘
```

### Multi-select picker

- `Size=Expanded` or `Full` (multi-select sheets usually have more options)
- `Is Modal=true`
- Header: `Title` = field label, optional `Subtitle` describing the rule ("Select up to 3"), `Show Trailing Action=false`
- Rows: `wiom-list-item` with `Type=Checkbox` — Selection = Selected / No / Indeterminate
- **`Show Button Group=true`** — user confirms with a Primary **"Done" / "Apply"** CTA. Add a Tertiary "Cancel" for explicit dismissal. Pattern:
  - 1 CTA → Primary "Done" only (swipe-down or scrim-tap cancels)
  - 2 CTAs → Primary "Apply" + Tertiary "Cancel" (stacked)
- Close behavior: "Done" commits; scrim tap / drag / "Cancel" reverts to pre-open selection

```
┌─ 360w · Expanded ─────────────────┐
│         ▬▬ (handle)                │
│  Filter notifications              │
│  Select up to 3                    │
│  ─────────────────────────────    │
│  [ ] Push                          │  ← List Item, Type=Checkbox
│  [✓] SMS                           │
│  [✓] Email                         │
│  [ ] WhatsApp                      │
│  [ ] Voice call                    │
│  ─────────────────────────────    │
│  ┌────────── Apply ──────────┐    │
│  └───────────────────────────┘    │
└────────────────────────────────────┘
```

### Action menu (list of actions, no selection state)

- `Size=Compact` (≤2 actions) or `Half` (3–4 actions)
- `Is Modal=true`
- Header: optional `Title` ("Choose action"), or omit Header entirely for very short menus (then `Show Top Safe Space=true`)
- Rows: `wiom-list-item` with `Type=Default`, `Has Leading=false`, `Has Trailing Icon=false`, `Selected=false` for all
- `Show Button Group=false` — tapping a row fires the action and dismisses the sheet

```
┌─ 360w · Compact ──────────────────┐
│         ▬▬ (handle)                │
│  Edit                              │  ← List Item, Type=Default, no leading, no chevron
│  Duplicate                         │
│  Share                             │
│  Delete                            │  (for destructive, leading icon `icon.critical` optional)
└────────────────────────────────────┘
```

### Search-header picker

For long lists (> 10 items). Use `Size=Full`. Header is replaced by / supplemented with a `wiom-input-fields` Search bar pinned at the top. Rows filter live as the user types.

### Picker vs Dialog Selection

| When | Use |
|---|---|
| ≤ 4 options, dialog-context flow (confirm + pick) | `wiom-dialog` with `Type=Selection` |
| > 4 options, or any search / scroll requirement | Bottom sheet picker (this section) |
| Picker triggered from an Input field | Bottom sheet picker (always) |

Dialogs forbid scrollable content; pickers with more than 4 rows belong here, not in Dialog Selection.

---

## See also

- `wiom-design-foundations` → Bottom Sheet component quick-reference, `overlay.scrim` token, `shadow.md`
- `wiom-compose-layout-rules` — Modifier discipline, Arrangement.spacedBy for stacked rows, no fixed heights
- `wiom-input-fields` — the Dropdown-field trigger that opens this sheet
- `wiom-list-item` — every row inside a picker / action menu
- `wiom-selection-controls` — Radio / Checkbox controls inside picker rows
- `wiom-cta` — button-group inside a multi-select picker (Done / Cancel)
- `wiom-dialog` — the alternate centered surface for very short selection flows (≤ 4 items)
- `wiom-icon-badge` — the illustration slot for Illustration / Illustration Left variants
