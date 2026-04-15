# Wiom Design System — Bottom Sheet Component Skill

You are generating a design or UI that includes a bottom sheet for the Wiom product ecosystem. You MUST use ONLY the approved Wiom bottom sheet specification below. Do NOT deviate from the defined anatomy, token assignments, variant rules, or sizing. Every bottom sheet in your output must conform exactly to this system.

## Context

Wiom serves lower- and middle-income Indian households. Bottom sheet decisions must account for:
- **Mobile-first** — budget Android devices, outdoor use, small screens
- **Trust-heavy** — payment confirmations, policy details, consent flows are critical path
- **Bilingual** — Hindi and English content, Devanagari characters are taller/wider at same size
- **Low cognitive load** — clear hierarchy, scannable list items, obvious dismiss affordance
- **Contextual actions** — option menus, quick settings, supplementary info without full navigation

---

## Component Overview

```
PURPOSE:  Surface anchored to screen bottom for supplementary content and contextual actions.
WIDTH:    360px (standard Android viewport). Fills screen width.
TYPES:    Standard (persistent, no scrim) and Modal (blocks background with scrim overlay).
LAYOUTS:  List-based (Compact/Half/Expanded/Full) and Illustration-based (with/without CTA).
SHADOW:   shadow/md (Standard) or shadow/xl (Modal) — never both shadow and border.
NOTE:     Hover and Focused states are excluded from V1 — mobile-first product.
```

---

## Anatomy — List Layout

```
┌─────────────────────────────────────────────────┐
│                 ── Handle ──                     │  ← 32×4px pill, centered
│                                                  │
│  Title                                           │  ← type/heading/md, left-aligned
│─────────────────────────────────────────────────│  ← 1px divider, full width
│                                                  │
│  [icon]  Label                            ›     │  ← list item row
│          Description                             │
│  ─────────────────────────────────────────      │  ← inset divider (76px from left)
│  [icon]  Label                            ›     │
│          Description                             │
│                                                  │
│─────────────────────────────────────────────────│  ← 1px top border on action bar
│  ┌─ Secondary ─┐  ┌─── Primary ───┐            │  ← CTA component instances
│  └─────────────┘  └───────────────┘            │
└─────────────────────────────────────────────────┘
```

| Element | Required | Spec |
|---------|----------|------|
| **Drag Handle** | Yes | 32×4px pill, `radius/full`, `color/border/strong`. Primary dismiss affordance. |
| **Header** | Yes | Title text left-aligned, `type/heading/md` (20sp Bold 28sp LH). |
| **Divider** | Yes | 1px `color/border/subtle`, full width below header. |
| **List Items** | Yes (list layout) | 40px icon circle + label + description + chevron_right. |
| **Inset Dividers** | Yes | 1px `color/border/subtle`, inset 76px from left (aligned to text start). |
| **Action Bar** | Conditional | CTA component instances. Present in Expanded, Full, and Illustration CTA variants. |

---

## Anatomy — Illustration Layout

```
┌─────────────────────────────────────────────────┐
│                 ── Handle ──                     │  ← 32×4px pill, centered
│                                                  │
│              ┌──────────┐                        │
│              │  120×120  │                        │  ← illustration circle, centered
│              │   icon    │                        │
│              └──────────┘                        │
│                                                  │
│               Heading                            │  ← type/heading/md, centered
│         Description subtext                      │  ← type/body/md, centered
│                                                  │
│─────────────────────────────────────────────────│
│  ┌─ Secondary ─┐  ┌─── Primary ───┐            │  ← optional CTA bar
│  └─────────────┘  └───────────────┘            │
└─────────────────────────────────────────────────┘
```

| Element | Required | Spec |
|---------|----------|------|
| **Illustration circle** | Yes | 120×120px, `color/brand/primary-tint` fill, `radius/full`. Contains 48dp icon (`icon/lg`). |
| **Icon** | Yes | Instance from icon library (Material Symbols Rounded), `color/brand/primary` fill override. |
| **Heading** | Yes | `type/heading/md`, `color/text/primary`, center-aligned. |
| **Subtext** | Yes | `type/body/md`, `color/text/secondary`, center-aligned. |

---

## Variant Properties (Figma)

| Property | Values | Type | Default |
|----------|--------|------|---------|
| Type | Standard, Modal | Variant | Standard |
| Size | Compact, Half, Expanded, Full, Illustration, Illustration CTA | Variant | Compact |
| Title | (editable) | Text | "Title" |
| Heading | (editable) | Text | "Heading" |
| Subtext | (editable) | Text | "Description text goes here..." |
| Show Secondary CTA | true, false | Boolean | true |

**Total variants:** 2 types × 6 sizes = 12

---

## Size Presets

| Size | Height | Content | Action Bar | Use Case |
|------|--------|---------|------------|----------|
| **Compact** | 240px | Handle + Header + 2 items | No | Quick peek, simple option menu |
| **Half** | 400px | Handle + Header + 4 items | No | Standard option list, settings |
| **Expanded** | 600px | Handle + Header + 7 items | Yes | Full content, confirmations |
| **Full** | 756px | Handle + Header + 7 items | Yes | Complex flows, long scrollable content |
| **Illustration** | Auto (hug) | Handle + illustration + heading + subtext | No | Info, success, error states |
| **Illustration CTA** | Auto (hug) | Handle + illustration + heading + subtext | Yes | Confirmations with visual context |

---

## Type Behavior

| Type | Scrim | Shadow | Dismiss | Background |
|------|-------|--------|---------|------------|
| **Standard** | None | `shadow/md` effect style | Swipe down on handle | Interactive — user can still interact with content behind |
| **Modal** | `color/overlay/default` (rgba 22,16,33 @ 50%) | `shadow/xl` effect style | Swipe down on handle OR tap scrim | Blocked — scrim prevents background interaction |

---

## Complete Token Reference

### Sheet surface tokens

| Property | Token | Value |
|----------|-------|-------|
| Fill | `color/surface/base` | #FAF9FC |
| Top corners | `radius/xlarge` | 24px |
| Bottom corners | `radius/none` | 0px |
| Shadow (Standard) | `shadow/md` effect style | 0 2px 6px rgba(0,0,0,0.15) |
| Shadow (Modal sheet) | `shadow/xl` effect style | 0 8px 24px rgba(0,0,0,0.15) |
| Scrim (Modal only) | `color/overlay/default` | rgba(22,16,33,0.50) |

### Drag handle tokens

| Property | Token | Value |
|----------|-------|-------|
| Size | — | 32×4px |
| Radius | `radius/full` | 9999px |
| Fill | `color/border/strong` | #665E75 |
| Container padding | `space/8` top and bottom | 8px |

### Header tokens

| Property | Token | Value |
|----------|-------|-------|
| Title style | `type/heading/md` | 20sp / Bold 700 / 28sp LH |
| Title color | `color/text/primary` | #161021 |
| Padding left | `space/24` | 24px |
| Padding right | `space/16` | 16px |
| Padding top | `space/4` | 4px |
| Padding bottom | `space/12` | 12px |

### List item tokens

| Property | Token | Value |
|----------|-------|-------|
| Icon container | 40×40px (`space/40`), `radius/full`, `color/brand/primary-tint` fill |
| Icon instance | From icon library, `color/brand/primary` fill override |
| Icon glyph size | `icon/sm` | 20dp (placeholder) |
| Label style | `type/label/md` | 14sp / SemiBold 600 / 20sp LH |
| Label color | `color/text/primary` | #161021 |
| Description style | `type/body/sm` | 12sp / Regular 400 / 16sp LH |
| Description color | `color/text/secondary` | #5C5570 |
| Chevron icon | `chevron_right` from icon library, `icon/md` (24dp), `color/text/disabled` fill |
| Row padding | `space/12` vertical, `space/24` horizontal |
| Row item spacing | `space/12` | 12px |
| Inset divider | 1px `color/border/subtle`, left inset 76px (24px padding + 40px icon + 12px gap) |

### Action bar tokens

| Property | Token | Value |
|----------|-------|-------|
| Top border | 1px `color/border/subtle` |
| Padding | `space/24` horizontal, `space/16` vertical |
| CTA gap | `space/12` | 12px |
| Buttons | CTA component instances (Primary + Secondary from design system) |

### Illustration tokens

| Property | Token | Value |
|----------|-------|-------|
| Circle size | 120×120px |
| Circle fill | `color/brand/primary-tint` | #FFE5F6 |
| Circle radius | `radius/full` | 9999px |
| Icon size | `icon/lg` | 48dp |
| Icon color | `color/brand/primary` | #D9008D |
| Heading style | `type/heading/md` | 20sp / Bold 700 / 28sp LH, center-aligned |
| Heading color | `color/text/primary` | #161021 |
| Subtext style | `type/body/md` | 14sp / Regular 400 / 20sp LH, center-aligned |
| Subtext color | `color/text/secondary` | #5C5570 |
| Heading-to-subtext gap | `space/8` | 8px |
| Text area padding | `space/24` horizontal, `space/16` top, `space/24` bottom |

---

## CTA Configuration

| Config | Show Secondary CTA | Result |
|--------|-------------------|--------|
| **Dual CTA** | true (default) | Secondary (outlined) + Primary (filled) side by side, each FILL width |
| **Single CTA** | false | Primary CTA fills full width. Use for simple confirmations. |

**Rule:** CTAs are instances of the existing CTA component from the design system (Component Set on "Components / CTA, Tabs, Filters, Chips" page). Never recreate buttons — always use CTA instances.

---

## Decision Trees

### When to use Bottom Sheet

```
User needs supplementary content without leaving the current screen?
  → Bottom Sheet

User needs to pick from a list of contextual actions?
  → Bottom Sheet (list layout, Compact or Half)

User needs to confirm an action with visual context?
  → Bottom Sheet (Illustration CTA layout)

User needs to see success/error/info feedback with detail?
  → Bottom Sheet (Illustration layout)

User needs to complete a short form or flow?
  → Bottom Sheet (Expanded or Full, with action bar)
```

### When NOT to use Bottom Sheet

```
Content requires immediate attention and blocks all interaction?
  → Dialog (not Bottom Sheet)

Content is a full page or complex multi-step flow?
  → New screen (not Bottom Sheet)

Content is a simple toast-style notification?
  → Snackbar / Toast (not Bottom Sheet)

Content is a tooltip or contextual hint?
  → Tooltip (not Bottom Sheet)

Content needs to persist across navigation?
  → Persistent UI element (not Bottom Sheet)
```

### Which type?

```
User may continue interacting with background content?
  → Standard

Sheet requires user attention before continuing?
  → Modal

Sheet shows critical confirmation (payment, delete, consent)?
  → Modal
```

### Which size?

```
2 or fewer list items, quick action?
  → Compact (240px)

3-4 list items, option menu?
  → Half (400px)

5+ list items with action buttons?
  → Expanded (600px)

Long scrollable content or complex flow?
  → Full (756px)

Visual feedback (success, error, info)?
  → Illustration or Illustration CTA
```

---

## Wiom Use Cases

### Policy quick actions

```
Screen: Policy details, Home screen long-press
Title:  "Policy Options"
Items:  View Policy Details (policy icon)
        Make a Payment (currency_rupee icon)
        Contact Support (support_agent icon)
        Download Documents (download icon)
Size:   Half (4 items)
Type:   Standard
```

### Payment confirmation

```
Screen: Recharge checkout
Title:  "Confirm Payment"
Layout: Illustration CTA
Icon:   currency_rupee (48dp in 120px circle)
Heading: "Pay Rs 299?"
Subtext: "Monthly recharge for Plan A — 1.5GB/day. Amount will be debited from your UPI account."
CTA:    Single Primary — "Pay Now" (Show Secondary CTA = false)
Type:   Modal
```

### Success state

```
Screen: Post-payment, Post-activation
Layout: Illustration (no CTA)
Icon:   check_circle or task_alt (48dp in 120px circle)
Heading: "Payment Successful"
Subtext: "Rs 299 has been charged. Your plan is now active until 15 May 2026."
Type:   Modal
Rule:   Auto-dismiss after 3 seconds or tap scrim to close.
```

### Error state

```
Screen: Payment failure, Network error
Layout: Illustration CTA
Icon:   error or warning (48dp in 120px circle)
Heading: "Payment Failed"
Subtext: "We couldn't process your payment. Please check your UPI app and try again."
CTA:    Dual — "Cancel" + "Retry"
Type:   Modal
```

### Settings menu

```
Screen: Account, Profile
Title:  "Account Settings"
Items:  Account Settings (settings icon)
        Help & FAQ (quiz icon)
        Rate this App (star icon)
Size:   Compact (3 items, no action bar)
Type:   Standard
```

---

## Figma Component Structure

```
Bottom Sheet (Standard)            ← root component, auto-layout vertical
├── Drag Handle                    ← frame, centered, space/8 padding
│   └── Indicator                  ← 32×4px rectangle, radius/full, color/border/strong
├── Header                         ← frame, horizontal, space/24 left padding
│   └── Title                      ← text, type/heading/md, color/text/primary
├── Divider                        ← 1px rectangle, color/border/subtle, full width
├── Content                        ← frame, vertical, FILL sizing, clipsContent
│   ├── Item / Label               ← frame, horizontal, space/12 gap
│   │   ├── Icon Container         ← 40×40, radius/full, color/brand/primary-tint
│   │   │   └── [icon instance]    ← from icon library, color/brand/primary
│   │   ├── Text                   ← frame, vertical
│   │   │   ├── Label              ← type/label/md, color/text/primary
│   │   │   └── Description        ← type/body/sm, color/text/secondary
│   │   └── chevron_right          ← icon instance, icon/md, color/text/disabled
│   └── Divider                    ← inset 76px, 1px, color/border/subtle
└── Action Bar (conditional)       ← frame, horizontal, space/12 gap
    ├── CTA / Secondary            ← instance of CTA component (boolean toggle)
    └── CTA / Primary              ← instance of CTA component
```

```
Bottom Sheet (Modal)               ← root component, auto-layout vertical, primaryAxisAlignItems MAX
├── [Scrim]                        ← root fill: color/overlay/default
└── Sheet Surface                  ← frame, same structure as Standard
    ├── Drag Handle
    ├── Header
    ├── Divider
    ├── Content
    └── Action Bar (conditional)
```

---

## Interaction Behavior

| Behavior | Standard | Modal |
|----------|----------|-------|
| **Dismiss** | Swipe down on drag handle | Swipe down on handle OR tap scrim |
| **Animation** | Spring animation, slides down | Spring animation with scrim fade |
| **Snap points** | Snaps to nearest size preset based on drag velocity | Same |
| **Scroll** | Content area scrolls independently, header/actions fixed | Same |
| **Background** | Interactive — user can interact behind sheet | Blocked by scrim |
| **Back button** | Dismisses sheet | Dismisses sheet |

---

## Accessibility

| Requirement | Implementation |
|-------------|---------------|
| Focus management | Focus moves into sheet on open, returns to trigger on close |
| Focus trap (Modal) | Tab/swipe navigation stays within sheet, cannot escape to background |
| Dismiss alternatives | Provide button alternative for swipe-down gesture |
| Touch targets | All interactive elements min 48×48px |
| Screen reader | Announce sheet as dialog/panel with title |
| Reduce motion | Skip spring animation, use instant show/hide |

---

## Strict Rules

### DO
- Use ONLY the tokens listed in this skill — no hardcoded values
- Always include a drag handle — it's the primary dismiss affordance
- Use `shadow/md` for Standard, `shadow/xl` for Modal — applied via effect styles
- Use `radius/xlarge` (24px) for top corners only — bottom corners always `radius/none`
- Use CTA component instances — never recreate buttons inside the sheet
- Use icon instances from the icon library — never raw vectors
- Bind all colors to Figma variables from the Color collection
- Bind all spacing to Figma variables from the Spacing collection
- Bind all radii to Figma variables from the Radius collection
- Apply text styles from the file's local text styles
- Use `color/overlay/default` for Modal scrim — not a custom opacity
- Keep list item dividers inset to text start (76px from left)
- Use `space/12` gap between CTA buttons in action bar
- Use `space/24` horizontal content padding consistently
- Test with Hindi text — Devanagari wraps differently

### DO NOT
- Do NOT add a close button — drag handle is the dismiss affordance
- Do NOT use border AND shadow on the sheet surface — shadow alone provides depth
- Do NOT use `shadow/xl` on Standard type — only Modal sheets get xl shadow
- Do NOT nest a bottom sheet inside another bottom sheet
- Do NOT use bottom sheet for critical alerts requiring immediate action — use Dialog
- Do NOT add more than 7 list items without scrollable content area
- Do NOT use custom button frames — always use CTA component instances
- Do NOT use custom icon shapes — always use icon library instances
- Do NOT hardcode hex colors — bind to Figma color variables
- Do NOT hardcode spacing values — bind to Figma spacing variables
- Do NOT put bottom radius on the sheet — it anchors to screen bottom

### WHEN IN DOUBT
- Type should be Modal for confirmations and critical actions
- Size should be Half for standard option menus (most common)
- Use Illustration layout for feedback states (success, error, info)
- Single Primary CTA for simple confirmations, dual for cancel/confirm flows
- Content padding is `space/24` horizontal — consistent across all variants
- Shadow is `shadow/md` for Standard — unless Modal, then `shadow/xl`

---

## Anti-Pattern Checklist

Before submitting any design or code with bottom sheets, verify NONE of these are present:

- [ ] Hardcoded hex values instead of Figma variable bindings
- [ ] Close button present (removed in V1 — drag handle is dismiss affordance)
- [ ] Border AND shadow on sheet surface (pick shadow only)
- [ ] Bottom corners with radius (should always be 0px / radius/none)
- [ ] Custom button frames instead of CTA component instances
- [ ] Raw vector icons instead of icon library instances
- [ ] Missing drag handle (always required)
- [ ] Standard type with shadow/xl (should be shadow/md)
- [ ] Modal type without scrim overlay
- [ ] Full-bleed dividers inside content area (should be inset to 76px)
- [ ] Icon containers not 40×40px circle
- [ ] Spacing values not from the Spacing variable collection
- [ ] Text not using local text styles
- [ ] Nested bottom sheet (never valid)
- [ ] More than 7 list items without scrollable content
- [ ] Missing title in header

---

## Quick Token Lookup

```
SURFACE:     color/surface/base (#FAF9FC)
CORNERS:     radius/xlarge (24px) top only, radius/none bottom
SHADOW:      shadow/md (Standard) / shadow/xl (Modal) — effect styles
SCRIM:       color/overlay/default (rgba 22,16,33,0.50) — Modal only
HANDLE:      32×4px, radius/full, color/border/strong (#665E75), space/8 padding
TITLE:       type/heading/md (20sp Bold 28sp LH), color/text/primary, space/24 left
DIVIDER:     1px color/border/subtle, full width (header) or inset 76px (items)
ICON CIRCLE: 40×40px (space/40), radius/full, color/brand/primary-tint
ICON GLYPH:  icon library instance, color/brand/primary fill
LABEL:       type/label/md (14sp SemiBold 20sp LH), color/text/primary
DESCRIPTION: type/body/sm (12sp Regular 16sp LH), color/text/secondary
CHEVRON:     chevron_right instance, icon/md (24dp), color/text/disabled
ACTION BAR:  1px top border color/border/subtle, space/24 h-pad, space/16 v-pad, space/12 gap
CTA:         Instances from CTA component set. Secondary toggle via boolean property.
ILLUSTRATION:120px circle, color/brand/primary-tint, icon/lg (48dp), color/brand/primary
HEADING:     type/heading/md, color/text/primary, center
SUBTEXT:     type/body/md, color/text/secondary, center
```

---

Apply this bottom sheet specification to the user's design request. Use token names in your output (not just values) so the design is traceable back to the Wiom Design System. Reference variant properties (Type, Size, Show Secondary CTA) when describing bottom sheet instances.
