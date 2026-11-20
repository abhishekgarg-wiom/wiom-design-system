---
name: wiom-bottomsheet
description: >
  Wiom Bottom Sheet — the unified bottom-anchored surface for
  contextual content, picker UX (single / multi-select / action
  menus), success / error / confirm feedback, share targets, and
  short forms. 1 component (set `3022:424`) · 8 sizes (Compact ·
  Half · Expanded · Full · Illustration · Illustration Left ·
  Share · Form) · 6 booleans (Is Modal, Show Handle, Show Title,
  Show Button Group, Show Top Safe Space, Show Bottom Safe Space) ·
  2 Illustration text props (Heading, Subtext). Hosts
  `wiom-list-item` rows for list / picker layouts, `wiom-cta`
  Button Group for the action bar, `wiom-icon-badge` Lg for the
  Illustration tones, and `wiom-input-fields` for the Form variant.
  Replaces the deprecated `wiom-dropdown` menu UX — picker lists
  always open bottom-anchored. Reference for generating Jetpack
  Compose code with `WiomBottomSheet` + `WiomBottomSheetHeader`
  and for instancing the Figma component sets via MCP. Use this
  skill EVERY TIME you place a bottom sheet, modal sheet, picker,
  payment confirmation, success / error feedback sheet, share
  sheet, filter sheet, action menu, or any Figma frame containing
  a drag-handle + content + CTA layout. Trigger on: bottom sheet,
  modal sheet, drawer, option menu, confirm sheet, success sheet,
  error sheet, payment confirm, share sheet, sort sheet, filter
  sheet, picker, instance of 3022:424. Pairs with
  wiom-design-foundations (tokens), wiom-list-item (rows in list
  variants), wiom-cta (Button Group in Action Bar), wiom-icon-badge
  (Illustration Lg badge), wiom-input-fields (Form variant + the
  Dropdown trigger that opens this sheet), wiom-dialog (the
  alternate centered surface for ≤ 4-option pickers), and
  wiom-compose-layout-rules (Modifier discipline).
---

# Wiom Bottom Sheet

Built from Figma file `ZPErh7YU7omdbyy2o268Qo` (Wiom DS V2), component set `3022:424` (Bottom Sheet), component `2993:8` (Bottom Sheet Header), and the `wiom-design-foundations` token set. Hosts `wiom-list-item`, `wiom-cta` Button Group, `wiom-icon-badge`, and `wiom-input-fields` as nested instances.

**1 component set · 8 sizes · 6 booleans · 360 dp width · top corners `radius-xlarge` · `shadow.md` only.**

| Size | Body content | Default Action Bar | Use for |
|---|---|---|---|
| `Compact` | 2 list rows | Visible | Quick contextual menu (≤ 2 actions) · short confirms |
| `Half` | 4 list rows | Visible | Option menu · short picker · filter sheet |
| `Expanded` | 7 list rows | Visible | Standard picker · multi-select |
| `Full` | 7+ rows (scrollable) | Visible | Long lists · T&C · scrollable content |
| `Illustration` | 96 dp icon-badge + heading + subtext (centered) | Visible (Stacked) | Payment confirm · success · error · destructive confirm · celebration |
| `Illustration Left` | 96 dp icon-badge + heading + subtext (left-aligned) | Visible (Stacked) | Compact feedback with image left of text |
| `Share` | Horizontal row of share targets | Optional | System share · Wiom share affordance |
| `Form` | 1–3 inline `wiom-input-fields` | Visible | Enter PIN · short label · single-step input |

The bottom sheet **owns the surface**: width fill, top radius, shadow, scrim, drag-handle, divider, action bar gutter, safe-space mechanics. It hosts but never re-implements its children — list rows live in `wiom-list-item`, CTAs in `wiom-cta` (via Button Group), the illustration in `wiom-icon-badge` Lg.

---

## 1. Figma — instancing the component

Instance the component set `3022:424` and set `Size`. Strings are **case-sensitive**. Booleans toggle independently.

### Variants — 8 ship sizes

| `Size` | Node | Body anatomy |
|---|---|---|
| `Compact` | `3019:73` | Handle · Header · Divider · 2 list rows · Action Bar |
| `Half` | `3020:124` | Handle · Header · Divider · 4 list rows · Action Bar |
| `Expanded` | `3020:227` | Handle · Header · Divider · 7 list rows · Action Bar |
| `Full` | `3020:371` | Handle · Header · Divider · 7+ rows (scroll) · Action Bar |
| `Illustration` | `3021:403` | Handle · Illustration Block (centered) · Action Bar |
| `Share` | `3090:1365` | Handle · Header · share-target row · optional Action Bar |
| `Illustration Left` | `3091:1379` | Handle · Illustration Block (left-aligned) · Action Bar |
| `Form` | `3092:1391` | Handle · Header · 1–3 input fields · Action Bar |

### Properties (apply per instance)

| Property | Type | Default | Applies to | Purpose |
|---|---|---|---|---|
| `Is Modal` | Boolean | `false` | All | Reveals the `overlay.scrim` (rgba #161021 @ 50%) above the sheet — required for commit moments |
| `Show Handle` | Boolean | `true` | All | Reveals the 32 × 4 dp drag indicator in its 8 dp-padded container |
| `Show Title` | Boolean | `true` | All except Illustration · Illustration Left | Reveals the Header instance + 1 dp divider below it |
| `Show Button Group` | Boolean | `true` | All | Reveals the Action Bar (Button Group + 16 dp gutters) |
| `Show Top Safe Space` | Boolean | `true` | All | Reveals 16 dp spacer between Handle and Header / content. **V2 ships this `true` by default** (V1 was `false`) |
| `Show Bottom Safe Space` | Boolean | `false` | All | Reveals 16 dp spacer below Action Bar / content (use when `Show Button Group = false`) |
| `Heading` | Text | `"Heading"` | Illustration · Illustration Left | Centered (or left-aligned) headline — `type.heading.md` |
| `Subtext` | Text | `"Description…"` | Illustration · Illustration Left | Supporting copy below heading — `type.body.md` |

### Bottom Sheet Header — `2993:8` (sub-component)

The Header is a **single** component (no variants); slots are toggled via booleans. Defaults match the most common case (title-only, no icon, no trailing action).

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Show Leading Icon` | Boolean | `false` | Reveals a 24 dp leading icon (instance-swappable) — used for back-arrow nav, branded icon, or contextual cue |
| `Show Title` | Boolean | `true` | Reveals the Title text — `type.heading.md`, `text.default` |
| `Show Subtitle` | Boolean | `false` | Reveals the Subtitle below Title — `type.body.sm`, `text.subtle` |
| `Show Trailing Action` | Boolean | `false` | Reveals a trailing label-only action (e.g. "Clear all") — `type.label.md`, `text.brand` |
| `Title` | Text | `"Title"` | The headline string |
| `Subtitle` | Text | `"Subtitle description"` | The supporting line |
| `Trailing Action Label` | Text | `"Action"` | The trailing action's text |
| `Leading Icon` | Instance swap | `arrow_back` (24 dp) | Swap to any 24 dp icon from the Foundation/icons library |

### Bubbled-up nested-instance properties

For list / picker / form variants, the nested children expose their own variants on the bottom-sheet instance properties panel:

| Slot | Bubbled props |
|---|---|
| Header (always present except Illustration variants) | All header booleans + text + leading-icon swap (above) |
| Action Bar → Button Group | `Count` (1 / 2 / 3) · `Layout` (Side by side / Stacked) · per-CTA Type swap (Primary / Secondary / Tertiary / Destructive / Pre-booking / Acknowledge) — see `wiom-cta` |
| List rows (Compact / Half / Expanded / Full) | Per-row `wiom-list-item` Type + booleans — see `wiom-list-item` |
| Illustration Block badge (Illustration · Illustration Left) | Glyph (instance swap). **Tone is not bubbled in Figma** — Figma ships Neutral (`bg.brand.subtle` + `icon.brand`); in code the `WiomBottomSheet` `illustrationTone` parameter resolves to a `WiomIconBadgeTone` |
| Form fields (Form variant) | Per-field `wiom-input-fields` Type / State / Has Helper / Has Trailing Icon |

### Code enum ↔ Figma variant string

| Kotlin | Figma `Size` value |
|---|---|
| `WiomBottomSheetSize.Compact` | `Compact` |
| `WiomBottomSheetSize.Half` | `Half` |
| `WiomBottomSheetSize.Expanded` | `Expanded` |
| `WiomBottomSheetSize.Full` | `Full` |
| `WiomBottomSheetSize.Illustration` | `Illustration` |
| `WiomBottomSheetSize.IllustrationLeft` | `Illustration Left` *(case-sensitive — capitalised, with space)* |
| `WiomBottomSheetSize.Share` | `Share` |
| `WiomBottomSheetSize.Form` | `Form` |

---

## 2. Anatomy

### List layout (Compact / Half / Expanded / Full · Form · Share)

```
┌──────────────────────── 360 dp wide · hugs height ─────────────────────────┐
│  [Scrim — overlay.scrim 50% · only when Is Modal = true]                         │
└──────────────────────────────────────────────────────────────────────────────┘
┌──────── Sheet Surface · bg.default · radius-xlarge top · shadow.md ─────────┐
│                                                                              │
│        ▬▬▬▬   ←  Handle (32 × 4 dp · stroke.strong fill · radius-full)       │
│                  ↑ wrapped in 8 dp top + 8 dp bottom padding                 │
│                                                                              │
│  [16 dp Top Safe Space — when Show Top Safe Space = true]                   │
│                                                                              │
│  Title (type.heading.md)                                                     │   ← Header
│  Subtitle (type.body.sm) — when Show Subtitle = true                         │     padding 8T 16R 16B 16L
│                                                                              │
│  ───────────────────────────── 1 dp · stroke.subtle ─────────────────────── │   ← Divider (when Title shown)
│                                                                              │
│  ●Lead    Primary text                                       ●chev          │   ← list rows · 48 dp min-height each
│           Secondary text                                                     │
│  ●Lead    Primary text                                       ●chev          │
│           Secondary text                                                     │
│                                                                              │
│  [48 dp space — Content's pb-48 · loose gap before action bar]              │
│                                                                              │
│  [px-16 px-16] ┌───── Cancel ─────┐ ┌────── Apply ──────┐  [16 dp Bottom]   │   ← Action Bar
│                └──────────────────┘ └───────────────────┘                    │     pb-16 px-16 · no top pad
│                                                                              │
│  [16 dp Bottom Safe Space — when Show Bottom Safe Space = true]             │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Illustration layout (Illustration · Illustration Left)

```
┌──────── Sheet Surface · bg.default · radius-xlarge top · shadow.md ─────────┐
│        ▬▬▬▬   ← Handle                                                       │
│                                                                              │
│  [16 dp Top Safe Space]                                                      │
│                                                                              │
│  ┌──── Illustration Block · pt-16 · pb-48 · px-16 · gap-24 ────────────┐    │
│  │                                                                      │    │
│  │                          ╭──────╮                                    │    │
│  │                          │  ●   │  ← WiomIconBadge Lg (96 dp)        │    │
│  │                          ╰──────╯     tone-swappable (per §5)        │    │
│  │                                                                      │    │
│  │                  Heading (type.heading.md, centered)                 │    │
│  │                  ↑ gap-12 ↓                                          │    │
│  │                  Subtext (type.body.md, text.subtle, centered)       │    │
│  │                                                                      │    │
│  └──────────────────────────────────────────────────────────────────────┘    │
│                                                                              │
│  [px-16 px-16] ┌────────── Pay now ──────────┐         [pb-16]              │
│                └─────────────────────────────┘                               │   ← Action Bar (Stacked default)
│                ┌────────── Cancel ───────────┐                               │     for Illustration variants
│                └─────────────────────────────┘                               │
└──────────────────────────────────────────────────────────────────────────────┘
```

`Illustration Left` swaps the Illustration Block to `Row` layout (badge left, text right, `Alignment.Start` instead of centered).

### Bottom Sheet Header anatomy

```
┌──────── Header · padding 8T · 16R · 16B · 16L · gap-12 ─────────────────────┐
│                                                                              │
│  ●Lead   Title (type.heading.md, text.default)                  Action      │
│  24 dp   ↓ space-4 ↓                                            (label.md   │
│  (when   Subtitle (type.body.sm, text.subtle)                    text.brand)│
│   shown) (when shown)                                           (when shown)│
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

| Slot | Value |
|---|---|
| Sheet width | 360 dp (in Figma) — `fillMaxWidth()` in code |
| Sheet shape | top corners `radius-xlarge` (24 dp) · bottom corners 0 |
| Sheet shadow | `shadow.md` (drop-shadow · y=2 · blur=6 · #00000026) — **no border** |
| Handle indicator | 32 × 4 dp · `stroke.strong` fill · `radius-full` · 8 dp top + 8 dp bottom padding |
| Top Safe Space | 16 dp (when shown) — V2 default `true` |
| Header outer padding | `8T · 16R · 16B · 16L` · slot gap `space-12` |
| Header text gap (title→subtitle) | `space-4` |
| Divider | 1 dp · `stroke.subtle` · full-width (only when Header shown) |
| List row content padding | inherits from `wiom-list-item` (16 dp all sides per row) |
| Content → Action Bar gap | `space-48` (Content's `pb-48`). **Not 24 dp** — V2 ships 48 dp |
| Action Bar | `pb-16 · px-16 · pt-0` (no top padding — Content's pb-48 is the gap) |
| Action Bar inner | `wiom-cta` Button Group (`Count` / `Layout` per recipe) — 328 dp wide, 12 dp inter-CTA gap |
| Bottom Safe Space | 16 dp (when shown) |
| Illustration Block | `pt-16 · pb-48 · px-16` · gap-24 between badge and text · gap-12 between heading and subtext |
| Illustration badge | 96 × 96 dp `WiomIconBadge.Lg` — tone via `WiomIconBadgeTone` (per `wiom-icon-badge`) |

---

## 3. Surface contract — sheet owns the surface

The bottom sheet **owns the surface** as a self-contained bottom-anchored panel: top-radius, drop shadow, scrim, drag handle, optional header + divider, content area, action bar gutters, safe spaces. Children render their own atoms; the sheet supplies the rest.

### What this means in code

- `Modifier.fillMaxWidth()` on the sheet root (`360 dp` in Figma; in code the device viewport drives it).
- **Top corners only:** `RoundedCornerShape(topStart = 24.dp, topEnd = 24.dp)`. Bottom corners are 0 — the sheet anchors flush to the screen edge.
- **`shadow.md` only — no border.** Foundations rule: shadow XOR border. The sheet has shadow; the surface contains the surface.
- **List rows are end-to-end inside the sheet.** Don't add horizontal padding around the list — `wiom-list-item` already owns its 16 dp inner gutter.
- **The Action Bar's Button Group is centered and 328 dp wide** (per `wiom-cta` Button Group). The 16 dp horizontal padding on Action Bar provides the gutter; the Button Group fills inside.
- **Drag handle is the dismiss affordance.** Don't add a close (X) button. The handle + scrim tap (Modal) + back press cover all dismiss paths.
- **No top border on Action Bar.** The 48 dp Content `pb-48` is the loose-gap separator. A border would be redundant clutter.

### Safe Space pairing (Figma `visible` has no NOT operator)

Figma's `visible` binding is direct-state only — `Show Top Safe Space` and `Show Bottom Safe Space` cannot auto-mirror `Show Title` / `Show Button Group`. They are **paired booleans** designers toggle together.

| Intent | Booleans |
|---|---|
| Default (with title + Button Group) | All defaults — `Show Top Safe Space = true`, the rest as listed |
| Hide title with breathing room | `Show Title = false` + keep `Show Top Safe Space = true` |
| Hide Button Group with safe area | `Show Button Group = false` + `Show Bottom Safe Space = true` |
| Bare list (no header, no CTAs) | `Show Title = false` + `Show Button Group = false` + both safe spaces `true` |

> Document the Top / Bottom Safe Space pairing on Figma Dev Mode handoff. In Compose, system insets cover this for free — see §7.

### Scrim ownership

`Is Modal = true` reveals the scrim. The scrim:
- Fills the viewport above the sheet (`overlay.scrim` = #161021 @ 50%).
- Tap-to-dismiss is the user contract — wire `onDismissRequest` for scrim taps.
- Does NOT receive shadow or radius — it's a flat colour pane.

`Is Modal = false` (Standard) — no scrim; background remains interactive. Use only for non-commit content (e.g. inline help, action sheet over a feed).

---

## 4. Sizes — when to use what

| Size | Sheet height (approx) | Picks |
|---|---|---|
| `Compact` | ~200 dp | ≤ 2 quick actions · single-row confirm |
| `Half` | ~370 dp | 3–4 options · short picker · filter sheet |
| `Expanded` | ~600 dp | 5+ options with CTAs · multi-select |
| `Full` | ~600 dp (scrollable) | Long lists · T&C · search-header pickers |
| `Illustration` | ~300 dp (hug) | Centered visual feedback (success / error / pay confirm / celebration) |
| `Illustration Left` | ~300 dp (hug) | Compact feedback with image left of text |
| `Share` | ~220 dp (hug) | System share · Wiom share affordance with horizontal targets |
| `Form` | ~400 dp (hug) | 1–3 inline inputs (PIN / short label / single-step input) |

### Pick the right surface — Bottom Sheet is one of five

Bottom Sheet is heavyweight (scrim + drag handle + tall surface). For Wiom users on budget Android, it's the right call for thumb-reach picker UX and trust-heavy commit moments — not the default for everything. Industry standard (Material 3) plus the Wiom context (mobile-first, bilingual, low cognitive load, trust-heavy payment moments) tilts each case to a specific surface:

| Moment | Surface | Why this surface |
|---|---|---|
| Non-essential acknowledgment — "Saved", "Copied", "Sent", "Settings updated" · 1–3 s auto-dismiss · optional Undo | **Toast** (`wiom-toast`) | Non-blocking; thumb stays put. Reserve for moments where missing the message causes no harm. **Don't use Toast for money / payment outcomes** — Wiom users (trust-heavy, low/mid-income) need a confirmable receipt, not a 3-second flash |
| Payment / recharge / refund outcome — "Recharge successful", "Payment failed" | **Bottom Sheet** · Illustration · Modal *(Positive / Critical tone)* — or **Full Screen** receipt for end-of-flow | Money moments need a confirmable surface with amount, plan, date, and an explicit "Done" / "Retry" CTA — not a transient toast |
| Inline error on a single input field | **Field error state** (`wiom-input-fields`) | Lives next to the cause — no surface change needed |
| Instant-apply binary setting / single-row toggle | **Inline list row** (`wiom-list-item` Switch) | Settings that take effect immediately don't warrant a modal interruption |
| Tooltip · contextual hint | **Tooltip** | Lightweight; dismissed on next tap |
| Short critical confirm — ≤ 1 line text · ≤ 2 short CTAs · no visual | **Dialog** (`wiom-dialog`) | Centered halt reads as "stop and decide" — fits "Discard changes?" |
| ≤ 4-option select with simple commit | **Dialog Selection** (`wiom-dialog`) | Forbids scroll; keeps the moment short. Bottom Sheet is overkill |
| Critical alert blocking all interaction | **Dialog** | Centered halt; no escape until dismissed |
| Picker triggered from a Dropdown field (always) | **Bottom Sheet** · Half / Expanded / Full | Bottom-anchored; thumb-reach friendly; canonical Material picker pattern |
| Action menu of 3+ contextual actions | **Bottom Sheet** · Compact / Half | Easier to tap than a centered popover; mobile-first |
| Multi-select with Apply / Cancel | **Bottom Sheet** · Expanded + Apply CTA | Apply-or-revert commit pattern; rows scroll |
| Confirm an action where the illustration is load-bearing or the subtext is multi-line | **Bottom Sheet** · Illustration · Modal | Wiom payment / delete moments — trust-heavy; illustration + body.md subtext carry weight a Dialog can't |
| Feedback that needs acknowledgment or an action — "Payment failed — Retry" | **Bottom Sheet** · Illustration · Modal | More than a Toast can carry; visual + multi-line subtext + CTA |
| Single-step input — PIN / short label tied to a field or button | **Bottom Sheet** · Form | Bottom-anchored; keeps the calling context behind the scrim |
| Long form · multi-step flow — signup · KYC · plan upgrade | **Full screen** | Bottom Sheet feels cramped on budget Android — full screen gives steps room, supports back-nav, and survives keyboard push-up |
| Detail · read page — T&C · plan details · FAQ | **Full screen** | Long scrollable content belongs on its own screen, not a sheet |
| Search experience with > 20 results | **Full screen** with search header | Sheet `Full` works up to ~20 rows; beyond that, full screen with system back |

### Decision logic — `Is Modal`

| Context | `Is Modal` |
|---|---|
| User may keep interacting with background content | `false` (Standard) |
| Requires user attention before continuing | `true` (Modal) |
| Payment / delete / consent (commit moment) | `true` (Modal — always) |
| Picker triggered from a field (commit moment) | `true` (Modal — always) |

---

## 5. Tokens

All tokens are **element-first**. Don't reference raw hex.

### Sheet surface

| Slot | Token |
|---|---|
| Sheet fill | `bg.default` (#FAF9FC) |
| Top corner radius | `radius-xlarge` (24 dp) |
| Bottom corner radius | 0 (anchors flush) |
| Shadow | `shadow.md` (drop-shadow · y=2 · blur=6 · #00000026) — no border |
| Scrim fill (`Is Modal = true`) | `overlay.scrim` (#161021 @ 50%) |
| Sheet width | `fillMaxWidth()` (360 dp in Figma) |

### Handle

| Slot | Token |
|---|---|
| Indicator | 32 × 4 dp · `stroke.strong` (#473F55) · `radius-full` |
| Handle outer padding | 8 dp top + 8 dp bottom |

### Header

| Slot | Token |
|---|---|
| Outer padding | `8T · 16R · 16B · 16L` |
| Slot gap (icon ↔ text ↔ trailing action) | `space-12` |
| Title style / colour | `type.heading.md` (20/28 Bold) · `text.default` (#161021) |
| Subtitle style / colour | `type.body.sm` (12/16) · `text.subtle` (#5C5570) |
| Title → Subtitle gap | `space-4` |
| Trailing action style / colour | `type.label.md` (14/20 SemiBold) · `text.brand` (#D9008D) — acts as a link |
| Leading icon | 24 dp · `icon.nonAction` (#898296) |

### Divider (only when Header shown)

| Slot | Token |
|---|---|
| Thickness | 1 dp |
| Fill | `stroke.subtle` (#D7D3E0) |
| Width | full-bleed (no horizontal margin) |

### Content / Action Bar gap

| Slot | Token |
|---|---|
| Content `pb` (Content → Action Bar) | `space-48` (48 dp) — V2 ship value |
| Action Bar padding | `pb-16 · px-16 · pt-0` |
| Action Bar Button Group width | 328 dp (per `wiom-cta` Button Group) |
| Action Bar Button Group gap | `space-12` (12 dp inter-CTA, inherited) |
| Action Bar background | transparent · **no top border** |

### Top / Bottom Safe Space

| Slot | Token |
|---|---|
| Top Safe Space height | 16 dp (when `Show Top Safe Space = true`) |
| Bottom Safe Space height | 16 dp (when `Show Bottom Safe Space = true`) |

### Illustration Block (Illustration · Illustration Left)

| Slot | Token |
|---|---|
| Block padding | `pt-16 · pb-48 · px-16` |
| Badge → Text gap | `space-24` |
| Heading style / colour | `type.heading.md` (20/28 Bold) · `text.default` |
| Subtext style / colour | `type.body.md` (14/20 Regular) · `text.subtle` |
| Heading → Subtext gap | `space-12` |
| Badge | `WiomIconBadge.Lg` (96 dp) — tone resolves to `bg.{tone}.subtle` + `icon.{tone}` (per `wiom-icon-badge` §5) |

### Illustration tones (semantic)

| Tone | Map (per `wiom-icon-badge`) | Use for |
|---|---|---|
| `Brand` (Figma default) | `bg.brand.subtle` + `icon.brand` | Tips · announcements · first-run celebrations · neutral-positive |
| `Positive` | `bg.positive.subtle` + `icon.positive` | Payment success · activation success · verification done |
| `Critical` | `bg.critical.subtle` + `icon.critical` | Payment failure · network error · permission denied · destructive confirmation |
| `Warning` | `bg.warning.subtle` + `icon.warning` | Low balance · expiring plan · permission required |
| `Info` | `bg.info.subtle` + `icon.info` | Explanatory feedback · neutral status changes |

> Old skill called the brand tone "Neutral" and used a `bg.selected` (#FFCCED) fill. V2 ship uses `bg.selected` literally on the Figma node — but the canonical tone in code is `Brand` (`bg.brand.subtle` + `icon.brand`), which renders the same pink family. Always go through `WiomIconBadge` instead of replicating the badge inline.

---

## 6. Universal rules

1. **Width is `fillMaxWidth()` · top corners only `radius-xlarge`.** No horizontal margin; bottom corners are always 0 — the sheet anchors flush to the screen edge.
2. **`shadow.md` only — never a border.** Foundations rule: shadow XOR border. Don't add `Modifier.border` to the sheet.
3. **Drag handle is the dismiss affordance.** Don't add a close (X) button. Handle + scrim tap (Modal) + back press cover all paths.
4. **No top border on Action Bar.** Content's 48 dp `pb` provides the loose gap; a border is redundant clutter.
5. **Action Bar always uses Button Group from `wiom-cta`.** Never hand-roll buttons inside the action bar — use the Button Group instance and configure `Count` / `Layout` per recipe.
6. **Header is composable.** Always instance `WiomBottomSheetHeader` — don't recreate the header markup inline. The sheet supplies title / subtitle / leading icon / trailing action through the Header's booleans.
7. **List rows are `wiom-list-item` instances.** Never re-implement rows. The list-item owns its 16 dp inner gutter, 48 dp min-height, and selection-control nesting.
8. **Illustration badge is `wiom-icon-badge` Lg.** Tone-swappable via `WiomIconBadgeTone`. Don't replicate a circle + glyph inline.
9. **Pair Safe Space with the absent neighbour.** When `Show Title = false` keep `Show Top Safe Space = true`. When `Show Button Group = false` flip `Show Bottom Safe Space = true`. Figma's `visible` binding can't auto-mirror.
10. **`Is Modal = true` for commit moments.** Pickers, payment confirms, deletes, consent — always Modal. Standard sheets are for non-commit, ambient content only.
11. **Illustration tones are semantic.** Match the message: Positive for wins, Critical for failures, Warning for permission required, Info for explanations, Brand for neutral / celebratory.
12. **Never nest a bottom sheet inside a bottom sheet.** If a flow needs a follow-up sheet, dismiss the first then present the second. Hand off via `onDismissRequest` + state.
13. **Don't put more than 7 rows in Expanded / Full without scroll.** Scrollable content lives in `Full`; `Expanded` is the upper bound for non-scrolling lists.
14. **Tokens only — element-first.** No raw hex. No legacy `colors.surface.*` / `colors.bg.brand.subtle` namespaces; use `Wiom.bg.default` / `Wiom.text.subtle` / `Wiom.stroke.subtle`.
15. **Use the lightest surface that fits the moment — but not lighter.** Bottom Sheet is heavyweight; Toast is the opposite extreme — and both can be misused. For Wiom users on budget Android, the rough hierarchy: **Toast** for non-essential acknowledgments only ("Saved", "Copied", "Sent"); **inline UI** for instant-apply settings; **Dialog** for short halts; **Bottom Sheet** for picker UX, action menus, multi-select commits, illustration-load-bearing confirms, single-step picker inputs; **Full Screen** for long forms, multi-step flows, detail pages, and end-of-flow money receipts. **Money moments never go to Toast** — recharges, refunds, payment failures, security alerts need a confirmable surface (Bottom Sheet Illustration or Full Screen receipt). The §4 table maps every common moment.

---

## 7. Compose API + code

### Public API

```kotlin
enum class WiomBottomSheetSize {
    Compact, Half, Expanded, Full,
    Illustration, IllustrationLeft, Share, Form,
}
```

The base `WiomBottomSheet` is slot-based; an `Illustration` overload wires the badge + heading + subtext for the centered / left-aligned illustration variants.

```kotlin
// Base — slot-based. Use the Illustration overload below for the illustration variants.
@Composable
fun WiomBottomSheet(
    onDismissRequest: () -> Unit,
    isModal: Boolean = true,                                 // commit-moment default
    showHandle: Boolean = true,
    size: WiomBottomSheetSize = WiomBottomSheetSize.Half,    // sheet body height policy
    header: @Composable (() -> Unit)? = null,                // typically WiomBottomSheetHeader { … }
    actionBar: @Composable (() -> Unit)? = null,             // typically WiomButtonGroup(…)
    content: @Composable ColumnScope.() -> Unit,
)

// Illustration / Illustration Left overload — synthesises the Illustration Block.
@Composable
fun WiomBottomSheet(
    onDismissRequest: () -> Unit,
    heading: String,
    subtext: String,
    @DrawableRes illustrationIconRes: Int,
    illustrationTone: WiomIconBadgeTone = WiomIconBadgeTone.Brand,
    leftAligned: Boolean = false,                             // false = Illustration · true = Illustration Left
    isModal: Boolean = true,
    showHandle: Boolean = true,
    actionBar: @Composable (() -> Unit)? = null,
)

// Header — single component, slot-toggled via booleans equivalent to its Figma counterpart.
@Composable
fun WiomBottomSheetHeader(
    title: String,
    subtitle: String? = null,
    @DrawableRes leadingIconRes: Int? = null,
    trailingActionLabel: String? = null,
    onTrailingActionClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
)
```

### Base `WiomBottomSheet` impl

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun WiomBottomSheet(
    onDismissRequest: () -> Unit,
    isModal: Boolean = true,
    showHandle: Boolean = true,
    size: WiomBottomSheetSize = WiomBottomSheetSize.Half,
    header: @Composable (() -> Unit)? = null,
    actionBar: @Composable (() -> Unit)? = null,
    content: @Composable ColumnScope.() -> Unit,
) {
    val sheetState = rememberModalBottomSheetState(skipPartiallyExpanded = true)

    ModalBottomSheet(
        onDismissRequest = onDismissRequest,
        sheetState = sheetState,
        containerColor = Wiom.bg.default,
        contentColor = Wiom.text.default,
        scrimColor = if (isModal) Wiom.bg.scrim else Color.Transparent,
        shape = RoundedCornerShape(topStart = 24.dp, topEnd = 24.dp),    // radius-xlarge top only
        dragHandle = if (showHandle) {
            { WiomBottomSheetHandle() }
        } else null,
        // shadow.md is supplied by ModalBottomSheet's elevation; do not add Modifier.shadow.
        windowInsets = WindowInsets(0),                                  // sheet handles its own bottom inset via Bottom Safe Space
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .heightConstraintFor(size)                               // see helper below
                .navigationBarsPadding(),                                // system inset → replaces Figma Bottom Safe Space
        ) {
            Spacer(Modifier.height(Wiom.space.s16))                      // Top Safe Space — V2 default true

            if (header != null) {
                header()
                HorizontalDivider(thickness = 1.dp, color = Wiom.stroke.subtle)
            }

            Column(
                modifier = Modifier
                    .weight(1f, fill = false)
                    .verticalScroll(rememberScrollState())
                    .padding(bottom = Wiom.space.s48),                   // Content → Action Bar loose gap (V2 ships 48 dp)
                content = content,
            )

            if (actionBar != null) {
                Box(
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(start = Wiom.space.s16, end = Wiom.space.s16, bottom = Wiom.space.s16),
                    contentAlignment = Alignment.Center,
                ) {
                    actionBar()                                          // typically WiomButtonGroup(…)
                }
            }
        }
    }
}

@Composable
private fun WiomBottomSheetHandle() {
    Box(
        modifier = Modifier
            .padding(top = Wiom.space.s8, bottom = Wiom.space.s8)
            .size(width = 32.dp, height = 4.dp)
            .background(Wiom.stroke.strong, CircleShape),
    )
}

private fun Modifier.heightConstraintFor(size: WiomBottomSheetSize): Modifier = when (size) {
    WiomBottomSheetSize.Compact          -> heightIn(min = 200.dp, max = 240.dp)
    WiomBottomSheetSize.Half             -> heightIn(min = 320.dp, max = 400.dp)
    WiomBottomSheetSize.Expanded         -> heightIn(min = 480.dp, max = 600.dp)
    WiomBottomSheetSize.Full             -> fillMaxHeight(0.92f)
    // Hug-content sizes — let the Column measure intrinsically.
    WiomBottomSheetSize.Illustration,
    WiomBottomSheetSize.IllustrationLeft,
    WiomBottomSheetSize.Share,
    WiomBottomSheetSize.Form             -> wrapContentHeight()
}
```

### `WiomBottomSheetHeader` impl

```kotlin
@Composable
fun WiomBottomSheetHeader(
    title: String,
    subtitle: String? = null,
    @DrawableRes leadingIconRes: Int? = null,
    trailingActionLabel: String? = null,
    onTrailingActionClick: (() -> Unit)? = null,
    modifier: Modifier = Modifier,
) {
    Row(
        modifier = modifier
            .fillMaxWidth()
            .padding(start = Wiom.space.s16, top = Wiom.space.s8, end = Wiom.space.s16, bottom = Wiom.space.s16),
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.spacedBy(Wiom.space.s12),
    ) {
        if (leadingIconRes != null) {
            Icon(
                painter = painterResource(leadingIconRes),
                contentDescription = null,
                tint = Wiom.icon.nonAction,
                modifier = Modifier.size(24.dp),
            )
        }
        Column(
            modifier = Modifier.weight(1f),
            verticalArrangement = Arrangement.spacedBy(Wiom.space.s4),
        ) {
            Text(title, style = Wiom.type.heading.md, color = Wiom.text.default)
            if (subtitle != null) {
                Text(subtitle, style = Wiom.type.body.sm, color = Wiom.text.subtle)
            }
        }
        if (trailingActionLabel != null && onTrailingActionClick != null) {
            Text(
                trailingActionLabel,
                style = Wiom.type.label.md,
                color = Wiom.text.brand,
                modifier = Modifier.clickable(role = Role.Button, onClick = onTrailingActionClick),
            )
        }
    }
}
```

### Illustration overload impl

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun WiomBottomSheet(
    onDismissRequest: () -> Unit,
    heading: String,
    subtext: String,
    @DrawableRes illustrationIconRes: Int,
    illustrationTone: WiomIconBadgeTone = WiomIconBadgeTone.Brand,
    leftAligned: Boolean = false,
    isModal: Boolean = true,
    showHandle: Boolean = true,
    actionBar: @Composable (() -> Unit)? = null,
) {
    WiomBottomSheet(
        onDismissRequest = onDismissRequest,
        isModal = isModal,
        showHandle = showHandle,
        size = if (leftAligned) WiomBottomSheetSize.IllustrationLeft else WiomBottomSheetSize.Illustration,
        header = null,
        actionBar = actionBar,
    ) {
        if (leftAligned) {
            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = Wiom.space.s16, top = Wiom.space.s16, end = Wiom.space.s16),
                horizontalArrangement = Arrangement.spacedBy(Wiom.space.s24),
                verticalAlignment = Alignment.CenterVertically,
            ) {
                WiomIconBadge(iconRes = illustrationIconRes, size = WiomIconBadgeSize.Lg, tone = illustrationTone)
                Column(
                    modifier = Modifier.weight(1f),
                    verticalArrangement = Arrangement.spacedBy(Wiom.space.s12),
                ) {
                    Text(heading,  style = Wiom.type.heading.md, color = Wiom.text.default)
                    Text(subtext,  style = Wiom.type.body.md,    color = Wiom.text.subtle)
                }
            }
        } else {
            Column(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(start = Wiom.space.s16, top = Wiom.space.s16, end = Wiom.space.s16),
                horizontalAlignment = Alignment.CenterHorizontally,
                verticalArrangement = Arrangement.spacedBy(Wiom.space.s24),
            ) {
                WiomIconBadge(iconRes = illustrationIconRes, size = WiomIconBadgeSize.Lg, tone = illustrationTone)
                Column(
                    horizontalAlignment = Alignment.CenterHorizontally,
                    verticalArrangement = Arrangement.spacedBy(Wiom.space.s12),
                ) {
                    Text(heading, style = Wiom.type.heading.md, color = Wiom.text.default, textAlign = TextAlign.Center)
                    Text(subtext, style = Wiom.type.body.md,    color = Wiom.text.subtle,  textAlign = TextAlign.Center)
                }
            }
        }
    }
}
```

### Common mistake → fix

```kotlin
// ❌ WRONG — Material ModalBottomSheet directly, raw colours, hand-rolled header,
//             custom drag handle, X close button, hand-rolled buttons inline.
ModalBottomSheet(
    onDismissRequest = onClose,
    containerColor = Color(0xFFFAF9FC),
    shape = RoundedCornerShape(24.dp),                       // wrong — bottom corners must be 0
) {
    Row {
        Text("Choose plan", fontSize = 20.sp, fontWeight = FontWeight.Bold)
        IconButton(onClick = onClose) { Icon(Icons.Default.Close, null) }   // banned — handle is dismiss
    }
    plans.forEach { Row { Text(it.name); Spacer(Modifier.weight(1f)); Icon(...) } }   // hand-rolled rows
    Row {
        Button(onClick = onApply) { Text("Apply") }                          // wrong — must use Button Group
    }
}

// ✅ RIGHT — WiomBottomSheet wires the surface; nested Wiom* components for header / rows / CTA.
WiomBottomSheet(
    onDismissRequest = onClose,
    isModal = true,
    size = WiomBottomSheetSize.Expanded,
    header = {
        WiomBottomSheetHeader(
            title = "Choose plan",
            subtitle = "Select up to 3",
        )
    },
    actionBar = {
        WiomButtonGroup(
            count = WiomButtonGroupCount.Two,
            layout = WiomButtonGroupLayout.SideBySide,
            primary  = { WiomCta(label = "Apply",  type = WiomCtaType.Primary,   onClick = onApply) },
            secondary = { WiomCta(label = "Cancel", type = WiomCtaType.Secondary, onClick = onClose) },
        )
    },
) {
    plans.forEach { plan ->
        WiomListItemCheckbox(
            primary = plan.name,
            secondary = "₹${plan.price} / month",
            selection = if (picked[plan.id] == true) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
            onSelectionChange = { picked[plan.id] = (it == WiomCheckboxSelection.Selected) },
        )
    }
}
```

---

## 8. Recipes

### Single-select picker (Radio · Half / Expanded)

The canonical replacement for the deprecated `wiom-dropdown` menu UX. Picker lists always open bottom-anchored.

```kotlin
val languages = remember { listOf("English", "हिन्दी", "मराठी", "தமிழ்") }
var pickedIndex by remember { mutableStateOf(0) }

if (sheetVisible) {
    WiomBottomSheet(
        onDismissRequest = { sheetVisible = false },          // scrim / drag / back = cancel
        isModal = true,                                        // commit moment
        size = WiomBottomSheetSize.Half,
        header = { WiomBottomSheetHeader(title = "Choose language") },
        actionBar = null,                                      // tap-row commits & dismisses
    ) {
        languages.forEachIndexed { i, lang ->
            WiomListItemRadio(
                primary = lang,
                radioSelected = i == pickedIndex,
                onRadioSelect = {
                    pickedIndex = i
                    sheetVisible = false                       // commit & dismiss
                },
            )
        }
    }
}
```

### Multi-select picker (Checkbox · Expanded + Done CTA)

```kotlin
val notifChannels = listOf("Push", "SMS", "Email", "WhatsApp", "Voice call")
val pickedDraft = remember { mutableStateMapOf<String, Boolean>() }   // pre-open snapshot
val pickedConfirmed = remember { mutableStateMapOf<String, Boolean>() }

if (sheetVisible) {
    WiomBottomSheet(
        onDismissRequest = {
            pickedDraft.clear()                                 // revert to pre-open on scrim / drag
            pickedDraft.putAll(pickedConfirmed)
            sheetVisible = false
        },
        isModal = true,
        size = WiomBottomSheetSize.Expanded,
        header = {
            WiomBottomSheetHeader(
                title = "Filter notifications",
                subtitle = "Select up to 3",
            )
        },
        actionBar = {
            WiomButtonGroup(
                count = WiomButtonGroupCount.One,
                primary = {
                    WiomCta(label = "Apply", type = WiomCtaType.Primary, onClick = {
                        pickedConfirmed.clear()
                        pickedConfirmed.putAll(pickedDraft)
                        sheetVisible = false
                    })
                },
            )
        },
    ) {
        notifChannels.forEach { channel ->
            WiomListItemCheckbox(
                primary = channel,
                selection = if (pickedDraft[channel] == true) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
                onSelectionChange = { pickedDraft[channel] = (it == WiomCheckboxSelection.Selected) },
            )
        }
    }
}
```

### Action menu (no selection state · Compact / Half)

```kotlin
if (menuVisible) {
    WiomBottomSheet(
        onDismissRequest = { menuVisible = false },
        isModal = true,
        size = WiomBottomSheetSize.Compact,
        header = { WiomBottomSheetHeader(title = "Choose action") },
        actionBar = null,
    ) {
        WiomListItem(primary = "Edit",       leadingIconRes = null, showTrailingChevron = false, onClick = { onEdit();      menuVisible = false })
        WiomListItem(primary = "Duplicate",  leadingIconRes = null, showTrailingChevron = false, onClick = { onDuplicate(); menuVisible = false })
        WiomListItem(primary = "Share",      leadingIconRes = null, showTrailingChevron = false, onClick = { onShare();     menuVisible = false })
        WiomListItem(primary = "Delete",     leadingIconRes = R.drawable.ic_delete_rounded, showTrailingChevron = false, onClick = { onDelete(); menuVisible = false })
    }
}
```

### Payment confirmation (Illustration · Modal)

```kotlin
if (confirmVisible) {
    WiomBottomSheet(
        onDismissRequest = { confirmVisible = false },
        heading = "Pay ₹299?",
        subtext = "Monthly recharge for Plan A — 1.5GB/day. Amount will be debited from your UPI account.",
        illustrationIconRes = R.drawable.ic_payments_rounded,
        illustrationTone = WiomIconBadgeTone.Brand,
        isModal = true,
        actionBar = {
            WiomButtonGroup(
                count = WiomButtonGroupCount.Two,
                layout = WiomButtonGroupLayout.Stacked,
                primary  = { WiomCta(label = "Pay now", type = WiomCtaType.Primary,   onClick = onPay) },
                secondary = { WiomCta(label = "Cancel",  type = WiomCtaType.Secondary, onClick = { confirmVisible = false }) },
            )
        },
    )
}
```

### Other Illustration tones at a glance

The Illustration overload is one API; tone + glyph + heading + CTA together signal the moment. Match the tone to the message (per §6 rule 11):

| Moment | `illustrationTone` | Glyph | Heading + CTA shape |
|---|---|---|---|
| Payment success | `Positive` | `ic_check_circle_rounded` | "Payment successful" · 1 Primary "Done" |
| Payment failure | `Critical` | `ic_error_rounded` | "Payment failed" · 2 side-by-side · Secondary "Cancel" + Primary "Retry" |
| Destructive confirm | `Critical` | `ic_delete_rounded` | "Delete account?" · 2 stacked · **Destructive** primary "Delete" + Secondary "Cancel" |
| Permission required | `Warning` | `ic_warning_rounded` | "Allow location?" · 2 stacked · Primary "Allow" + Secondary "Not now" |
| Explanatory feedback | `Info` | `ic_info_rounded` | "How auto-pay works" · 1 Primary "Got it" |

All five reuse the Illustration overload from §7 — only `heading`, `subtext`, `illustrationIconRes`, `illustrationTone`, and the `actionBar` Button Group composition change. **Destructive primary** swaps the CTA `type` from `Primary` to `Destructive` (per `wiom-cta`); the sheet itself is unchanged.

### Filter sheet (Half · Modal · Header + Trailing "Clear all")

```kotlin
WiomBottomSheet(
    onDismissRequest = onCancel,
    isModal = true,
    size = WiomBottomSheetSize.Half,
    header = {
        WiomBottomSheetHeader(
            title = "Filter plans",
            subtitle = "${activeCount} filters applied",
            trailingActionLabel = "Clear all",
            onTrailingActionClick = onClearAll,
        )
    },
    actionBar = {
        WiomButtonGroup(
            count = WiomButtonGroupCount.Two,
            layout = WiomButtonGroupLayout.SideBySide,
            primary  = { WiomCta(label = "Apply",  type = WiomCtaType.Primary,   onClick = onApply) },
            secondary = { WiomCta(label = "Cancel", type = WiomCtaType.Secondary, onClick = onCancel) },
        )
    },
) {
    filters.forEach { filter ->
        WiomListItemCheckbox(
            primary = filter.label,
            selection = if (filter.active) WiomCheckboxSelection.Selected else WiomCheckboxSelection.No,
            onSelectionChange = { filter.active = (it == WiomCheckboxSelection.Selected) },
        )
    }
}
```

### Search-header picker (Full · long lists)

For lists > 10 items, use `Full` and replace / prefix the Header's content with a `WiomInputField` Search variant. Rows filter live as the user types.

```kotlin
var query by remember { mutableStateOf("") }

WiomBottomSheet(
    onDismissRequest = onCancel,
    isModal = true,
    size = WiomBottomSheetSize.Full,
    header = {
        Column {
            WiomBottomSheetHeader(title = "Choose city")
            WiomInputField(
                type = WiomInputFieldType.Search,
                value = query,
                onValueChange = { query = it },
                placeholder = "Search 100+ cities…",
                modifier = Modifier.padding(horizontal = 16.dp, vertical = 8.dp),
            )
        }
    },
    actionBar = null,
) {
    cities.filter { it.contains(query, ignoreCase = true) }.forEach { city ->
        WiomListItemRadio(
            primary = city,
            radioSelected = city == picked,
            onRadioSelect = { picked = city; onCancel() },
        )
    }
}
```

---

## 9. Anti-patterns

(Universal rules in §6 cover the bans. Listed here are extras.)

- ❌ Adding a close (X) button on the sheet — drag handle + scrim + back press is the dismiss contract.
- ❌ Adding `Modifier.border` / `Modifier.shadow.xl` to the sheet surface — `shadow.md` only.
- ❌ Bottom corners with radius > 0 — sheet anchors flush to the screen edge.
- ❌ `Modifier.padding(bottom = …)` instead of `navigationBarsPadding()` for bottom safe area in Compose — system insets are the source of truth.
- ❌ Hand-rolled buttons inside the Action Bar — always `WiomButtonGroup`.
- ❌ Hand-rolled rows inside the body — always `WiomListItem` (or its type helpers).
- ❌ Hand-rolled circle + glyph for the Illustration badge — always `WiomIconBadge.Lg`.
- ❌ Hand-rolled header markup — always `WiomBottomSheetHeader`.
- ❌ `Is Modal = false` for picker / commit moments — pickers, payment, delete, consent must be Modal.
- ❌ Nesting a bottom sheet inside another bottom sheet — dismiss the first, then present the second.
- ❌ More than 7 rows in `Expanded` without scroll — use `Full` for scrollable lists.
- ❌ 3-side-by-side Button Group with a Primary CTA inside the Action Bar — the 101 dp slot collapses hierarchy. Use `Stacked` for `Count = 3` (per `wiom-cta`).
- ❌ Tone mismatch on Illustration — Positive tone with "Payment Failed" heading, Critical tone with "Welcome", etc.
- ❌ Pre-checking consent inside a sheet picker — checkboxes start unchecked (per `wiom-selection-controls`).

---

## 10. Figma traceability

File: `ZPErh7YU7omdbyy2o268Qo`. Bottom Sheet canvas: `3118:7650` (Section 1). Component set: `3022:424`. Header sub-component: `2993:8`.
Re-pull `get_variable_defs` on `3022:424` if foundations token names change.

| Node | Variant / component |
|---|---|
| `3022:424` | Bottom Sheet component set — 8 ship sizes |
| `3019:73` | Size = Compact |
| `3020:124` | Size = Half |
| `3020:227` | Size = Expanded |
| `3020:371` | Size = Full |
| `3021:403` | Size = Illustration (centered) |
| `3091:1379` | Size = Illustration Left |
| `3090:1365` | Size = Share |
| `3092:1391` | Size = Form |
| `2993:8` | Bottom Sheet Header (single component, slot booleans) |

**Description-vs-ship discrepancies** — if a stale doc says otherwise, trust ship values:

- The ship has **8 Size variants**, not 5. V2 added `Share`, `Illustration Left`, and `Form` to the original 5. Older skill drafts described 5 sizes; the Compose API surfaces all 8.
- **Content → Action Bar gap is `space-48` (48 dp), not 24 dp.** V2 ship measures `pb-48` on the Content frame and `pt-0` on the Action Bar — the Content's bottom padding is the loose gap. Older notes said `pt-24` on Action Bar.
- **`Show Top Safe Space` defaults to `true` in V2** (V1 was `false`). The 16 dp top spacer is now part of the default rhythm.
- **The Illustration badge is 96 dp (`WiomIconBadge.Lg`), not 120 dp.** Subtext is `type.body.md` (14/20), not `body.sm` — the Illustration variant pulls more visual weight. Block gaps are `space-24` (badge↔text) and `space-12` (heading↔subtext); older drafts said 16 / 8.
- **The Illustration Block's circle ships `bg.selected` (#FFCCED) on the Figma node** — but the canonical tone in code is `WiomIconBadgeTone.Brand` (`bg.brand.subtle` + `icon.brand`), which renders the same pink family. Always go through `WiomIconBadge`; don't replicate the badge inline.

---

## See also

- `wiom-design-foundations` — token source. Especially: `bg.default` / `overlay.scrim` / `bg.selected`, `text.default` / `text.subtle` / `text.brand`, `stroke.strong` / `stroke.subtle`, `icon.nonAction` / `icon.brand`, `space-4` / `space-8` / `space-12` / `space-16` / `space-24` / `space-48`, `radius-xlarge` (24 dp), `radius-full`, `shadow.md`, `type.heading.md` / `type.body.md` / `type.body.sm` / `type.label.md`.
- `wiom-list-item` — every row inside Compact / Half / Expanded / Full and inside picker / multi-select / action-menu sheets.
- `wiom-selection-controls` — `WiomCheckbox` / `WiomRadio` embedded in the picker / multi-select rows (via `wiom-list-item` helpers).
- `wiom-icon-badge` — the 96 dp Lg badge inside the Illustration / Illustration Left variants. Tone selection lives there (Brand / Positive / Critical / Warning / Info / Neutral).
- `wiom-cta` — Button Group inside the Action Bar (`Count` / `Layout` rules; 3-side-by-side primary ban).
- `wiom-input-fields` — the Form variant body (1–3 inline inputs); also the Dropdown trigger that opens this sheet.
- `wiom-dialog` — the alternate centered surface for ≤ 4-option pickers and very short selection flows. Dialogs forbid scrollable content; pickers with > 4 rows belong here.
- `wiom-compose-layout-rules` — Modifier discipline; `navigationBarsPadding()` not manual bottom padding; `Icons.Rounded.*` enforcement via `painterResource`.
