# Switch — Component Build Spec

## 1. Component purpose

The Switch is a binary toggle control in the Wiom Design System. It allows users to turn a setting on or off with immediate effect. It is a core form component used wherever an instant-apply binary choice is needed — such as toggling notifications, enabling features, or controlling privacy settings.

Sources for all token values:
- `tokens/v1-color-proposal.md`
- `tokens/v1-typography-proposal.md`
- `tokens/v1-spacing-proposal.md`
- `tokens/v1-radius-proposal.md`
- `tokens/v1-stroke-proposal.md`
- `tokens/v1-shadow-proposal.md`

---

## 2. Where to use it

- Settings toggles (notifications, dark mode, data sharing)
- Feature enable/disable (biometric login, auto-pay, SMS alerts)
- Privacy and permission toggles (location access, analytics consent)
- Instant-apply preferences (language, sound, vibration)
- Partner dashboard feature flags (enable/disable partner features)
- Any screen where the user needs to flip a binary setting with immediate effect

---

## 3. Where NOT to use it

- Form options that require a submit action — use Checkbox component
- Mutually exclusive choice from a set — use Radio component
- Multiple selections from a list — use Checkbox component
- Selecting items from a dropdown — use Select component
- Actions that need confirmation before applying — use Checkbox with a submit button
- Navigation or action triggering — use Button component

---

## 4. Anatomy

```
┌─ Switch ────────────────────────────────────────────┐
│                                                       │
│  ┌──────────────────┐                                 │
│  │  Track            │   Label text                   │
│  │  ┌──────┐        │   Helper text (optional)        │
│  │  │Thumb │        │                                 │
│  │  └──────┘        │                                 │
│  └──────────────────┘                                 │
│                                                       │
└───────────────────────────────────────────────────────┘
```

| Element | Required? | Notes |
|---|---|---|
| Track | **Yes** | 52 x 32 px pill container. Background changes between Off (neutral) and On (brand) states. |
| Thumb | **Yes** | 24 x 24 px circle. Slides left (Off) or right (On) within the track. Always white. |
| Label | **Yes** | Text describing what the toggle controls. Always required for accessibility. |
| Helper text | Optional | Supporting guidance below the label |

Rules:
- Label is always required. A switch without visible text is not allowed in V1.
- The entire row (track + label) is the click/tap target.
- Helper text, when present, aligns with the label text, not with the track.
- The thumb position is the primary affordance for selection state — left = Off, right = On.

---

## 5. Selection states

| Selection state | Track appearance | Thumb position |
|---|---|---|
| **Off** | Neutral fill with border | 4 px from left edge of track |
| **On** | Brand primary fill, no border | 4 px from right edge of track |

Notes:
- Unlike Checkbox, Switch has no indeterminate state. Selection is always binary: Off or On.
- Switches apply their effect immediately — there is no separate submit action.
- The filled state rule applies: when On (filled with `color/brand/primary`), the track border is removed. Fill IS the boundary.

---

## 6. Interaction states

| State | Description | Visual change |
|---|---|---|
| **Default** | Resting state | Base appearance per selection state |
| **Disabled** | Not interactive | Reduced opacity, no pointer events |

**Excluded from V1:** Hover and Focused states are not built. Wiom is a mobile-first product — touch interactions don't produce hover, and keyboard focus states are deferred to a later accessibility phase. Error state is also excluded — switches are instant-apply binary toggles, not form inputs requiring validation.

---

## 7. Size options

| Size | Track dimensions | Thumb dimensions | Label typography | Status |
|---|---|---|---|---|
| **Medium** | 52 x 32 px | 24 x 24 px | `type/body/md` (14 px / 20 px / Regular) | **Build for V1** |

V1 builds Medium only. Small and Large sizes can be added later if product screens require them.

---

## 8. Label rules

| Rule | Detail |
|---|---|
| Position | Right of the track, vertically centered with the track |
| Typography | `type/body/md` (14 px / 20 px line height / Regular 400) |
| Color (default) | `color/text/primary` #161021 |
| Color (disabled) | `color/text/disabled` #A7A1B2 |
| Required | Always present. No switch without visible label text. |
| Case | Sentence case ("Enable notifications", not "ENABLE NOTIFICATIONS") |
| Length | Keep concise. One line preferred. Multi-line labels are allowed but should be uncommon. |
| Wrapping | Allowed. Label wraps naturally. Track stays vertically centered. |
| Hindi labels | Must work at the same component size. Test bilingual lengths. |

**Why Body, not Label:**
Switch text describes what the toggle controls: "Enable notifications", "Auto-pay", "Share location data". This is reading text, not compact UI control text like a button label. Regular weight is more readable for potentially longer content. Consistent with Checkbox and Radio, which also use `type/body/md` for user-facing content text.

---

## 9. Helper text rules

| Rule | Detail |
|---|---|
| Position | Below the label, aligned with label start |
| Typography | `type/body/sm` (12 px / 16 px / Regular 400) |
| Color (default) | `color/text/secondary` #352D42 |
| Color (disabled) | `color/text/disabled` #A7A1B2 |
| Purpose | Additional guidance or context for the toggle |
| Examples | "You can change this later in settings" |
| Visibility | Hidden by default, toggled via boolean property |
| Max lines | 1-2 lines |

Note: Switch has no error text. Switches are binary toggles with instant apply — there is no form validation scenario. This differs from Checkbox and Radio, which both support error states.

---

## 10. Spacing rules

### Internal spacing

| Property | Value | Token | Note |
|---|---|---|---|
| Track to content gap (horizontal) | 8 px | `space/8` | Auto-layout horizontal gap |
| Label to helper text gap (vertical) | 4 px | `space/4` | Content frame vertical gap |
| Thumb inset (all sides) | 4 px | — | Absolute position within track |
| Track width | 52 px | — | Fixed dimension |
| Track height | 32 px | — | Fixed dimension |
| Thumb diameter | 24 px | — | Ellipse within track |

### External spacing (guidance, not built into the component)

| Context | Recommended spacing | Token |
|---|---|---|
| Between stacked switches | 12 px | `space/12` |
| Section gap (switch group to next section) | 24 px | `space/24` |
| Touch target (minimum) | 48 x 48 px | — (parent handles) |

### Touch target note

The track is 32 px tall, which is below the recommended 48 px minimum touch target. The parent layout should provide adequate tappable height. Unlike Checkbox and Radio (which have a 20 px indicator), the Switch track is larger and closer to touch compliance, but still needs layout-level padding for comfortable tapping.

---

## 11. Color rules

### Off track — all interaction states

| Element | Default | Disabled |
|---|---|---|
| Track fill | `color/surface/muted` #E8E4F0 | `color/surface/subtle` #F1EDF7 |
| Track stroke color | `color/border/strong` #665E75 | `color/border/strong` at 40% opacity |
| Track stroke width | `stroke/2` (2 px) | `stroke/2` (2 px) |

### On track — all interaction states

| Element | Default | Disabled |
|---|---|---|
| Track fill | `color/brand/primary` #D9008D | `color/brand/primary` at 40% opacity |
| Track stroke | None (filled = no border) | None |
| Track opacity | 1.0 | 0.4 (on track frame) |

### Thumb — all states

| Element | Default | Disabled |
|---|---|---|
| Thumb fill | White #FFFFFF | White #FFFFFF |
| Thumb size | 24 x 24 px | 24 x 24 px |
| Position (Off) | 4 px from left | 4 px from left |
| Position (On) | 4 px from right | 4 px from right |

### Label and helper text colors

| Element | Default | Disabled |
|---|---|---|
| Label | `color/text/primary` #161021 | `color/text/disabled` #A7A1B2 |
| Helper text | `color/text/secondary` #352D42 | `color/text/disabled` #A7A1B2 |

### Why `color/surface/muted` for the Off track

The Off track needs to be clearly visible on `color/surface/base` (#FAF9FC) backgrounds. `color/surface/muted` (#E8E4F0) provides sufficient contrast while remaining neutral. Combined with `color/border/strong` stroke, the Off state is clearly distinguishable from the surrounding surface, even on budget Android screens in outdoor light.

### Why white (#FFFFFF) for the thumb

The thumb must provide maximum contrast against both the neutral Off track and the branded On track. White achieves this for both states. Using a token like `color/text/inverse` (#FAF9FC) would also work but the Figma build uses pure white for the cleanest contrast.

---

## 12. Typography rules

| Element | Token | Font | Size | Line height | Weight |
|---|---|---|---|---|---|
| Label | `type/body/md` | Noto Sans | 14 px | 20 px | Regular (400) |
| Helper text | `type/body/sm` | Noto Sans | 12 px | 16 px | Regular (400) |

---

## 13. Radius, stroke, and shadow

### Radius

| Property | Token | Value | Applied to |
|---|---|---|---|
| Track corner radius | `radius/full` | 9999 px | Track (pill shape) |
| Thumb corner radius | `radius/full` | 9999 px | Thumb (circle via ellipse) |

**Why `radius/full`:** The track must be a pill shape and the thumb must be circular. `radius/full` (9999 px) applied to a rectangular frame produces a pill; applied to a square frame produces a circle. This follows Material Design 3, iOS, and all top-10 design systems for switch controls.

### Stroke

| Property | State | Token | Value |
|---|---|---|---|
| Track stroke (Off) | Default | `stroke/2` | 2 px, inside |
| Track stroke (Off) | Disabled | `stroke/2` | 2 px, inside (at 40% opacity) |
| Track stroke (On) | All states | None | Filled = no border |

**Filled state rule:** When the switch is On (filled with `color/brand/primary`), the border is removed. The fill IS the boundary. Consistent with checked checkbox and filled button patterns.

### Shadow

| Property | Token | Value |
|---|---|---|
| Track shadow | None | No elevation shadow |
| Thumb shadow | None | No thumb shadow in V1 |

Switch is an inline form control, not an elevated surface. No shadow is applied. Consistent with Checkbox, Radio, Button, and Input Field in V1.

---

## 14. Figma component structure

### Component name

```
Switch
```

### Layer structure — Off / Default

```
Switch                                   <- component root, auto-layout horizontal
|-- align: center (track vertically centered with label)
|-- gap: 8 px (space/8)
|-- width: hug contents (resizable by designer)
|-- height: hug contents
|
|-- Track                               <- frame, 52 x 32, fixed size, NO auto-layout
|   |-- fill: color/surface/muted (#E8E4F0)
|   |-- stroke: 2 px, inside, color/border/strong (#665E75)
|   |-- radius: 9999 px (radius/full)
|   |
|   +-- Thumb                           <- ellipse, 24 x 24, absolute position
|       |-- fill: #FFFFFF (white)
|       +-- positioned: 4 px from left edge, vertically centered
|
+-- Content                             <- auto-layout vertical
    |-- gap: 4 px (space/4)
    |-- resizing: fill horizontal, hug vertical
    |
    |-- Label                           <- text layer
    |   |-- text style: type/body/md
    |   |-- fill: color/text/primary (#161021)
    |   |-- resizing: fill horizontal, hug vertical
    |   +-- text: "Label"
    |
    +-- HelperText                      <- text layer, hidden by boolean
        |-- text style: type/body/sm
        |-- fill: color/text/secondary (#352D42)
        |-- resizing: fill horizontal, hug vertical
        +-- text: "Helper text"
```

### Layer changes — On / Default

- Track fill -> `color/brand/primary` (#D9008D)
- Track stroke -> none (removed)
- Thumb position -> 4 px from right edge, vertically centered

### Layer changes — Off / Disabled

- Track fill -> `color/surface/subtle` (#F1EDF7)
- Track stroke color -> `color/border/strong` at 40% opacity
- Thumb fill -> #FFFFFF (unchanged)
- Label fill -> `color/text/disabled` (#A7A1B2)
- HelperText fill -> `color/text/disabled` (#A7A1B2)

### Layer changes — On / Disabled

- Track fill -> `color/brand/primary` at 40% opacity (0.4 frame opacity on track)
- Track stroke -> none
- Thumb fill -> #FFFFFF (unchanged)
- Label fill -> `color/text/disabled` (#A7A1B2)
- HelperText fill -> `color/text/disabled` (#A7A1B2)

---

## 15. Layer and frame naming

| Layer | Name | Notes |
|---|---|---|
| Component root | `Switch` | Auto-layout horizontal, center alignment |
| Track container | `Track` | 52 x 32 fixed frame, radius/full, NO auto-layout |
| Thumb circle | `Thumb` | 24 x 24 ellipse, absolute position within track |
| Content wrapper | `Content` | Auto-layout vertical, holds Label and HelperText |
| Label text | `Label` | |
| Helper text | `HelperText` | Hidden by boolean property |

**Figma component naming in the assets panel:**

```
Switch
```

Selection and state are handled via Figma variant properties, not separate named components. The switch appears as a single component entry in the assets panel.

---

## 16. Properties and variants to create in Figma

| Property | Values | Type | Default |
|---|---|---|---|
| Selection | Off, On | Variant | Off |
| State | Default, Disabled | Variant | Default |
| Show Helper Text | true, false | Boolean | false |
| Label | (editable) | Text | "Label" |
| Helper Text | (editable) | Text | "Helper text" |

### Total variant count

2 selection states x 2 interaction states = **4 variants**

This is the smallest variant set among the form controls (Checkbox: 15, Radio: 10, Switch: 4). The simplicity reflects the component's mobile-first, instant-apply nature.

### Build order recommendation

1. Build Off x Default first as the base variant
2. Add On x Default (change track fill, remove stroke, move thumb)
3. Add Off x Disabled (reduce opacity, change colors)
4. Add On x Disabled (reduce track opacity)
5. Add helper text boolean toggle last

---

## 17. Cross-axis alignment

The switch uses **CENTER** cross-axis alignment, not MIN (top). This is a deliberate difference from Checkbox and Radio:

| Component | Cross-axis | Reason |
|---|---|---|
| Checkbox | MIN (top) | 20 px indicator matches 20 px text line height |
| Radio | MIN (top) | 20 px indicator matches 20 px text line height |
| Switch | CENTER | 32 px track is taller than 20 px text — centering is natural |

The 32 px track height exceeds the 20 px label line height. Top-aligning would leave the track hanging below the label. Centering vertically balances the visual weight.

---

## 18. What should be excluded from V1

- **Hover state** — mobile-first product, no web hover needed in V1
- **Focused state** — no keyboard focus ring needed for mobile V1. Deferred to accessibility phase
- **Error state** — switches are binary toggles with instant apply, no form validation
- **Size variants (Small / Large)** — single size covers all V1 use cases. Deferred to V2
- **Loading / indeterminate state** — not needed for V1 toggle patterns
- **Icon in thumb** — decorative icons (check, X) deferred. Thumb position is sufficient affordance
- **Animation / transition specs** — handled by platform implementation, not Figma component
- **RTL (right-to-left) layout** — deferred to internationalization phase
- **Dark mode or inverse surface variants** — separate phase
- **Platform-specific variants (web vs app)** — not differentiated in V1
- **Accessibility annotations** — separate phase
- **Toggle-on/off labels (e.g. "ON" / "OFF" text inside track)** — thumb position is sufficient

---

## 19. Design decisions

1. **Track uses `radius/full` (9999 px) for pill shape**
   Consistent with Material Design 3, iOS, and all top-10 design systems for switch controls.

2. **Filled = no border rule applied**
   On state removes track stroke. Fill IS the boundary. Matches checkbox, radio, and button patterns.

3. **Off track uses `color/surface/muted` fill + `color/border/strong` stroke**
   Provides clear visibility on `color/surface/base` backgrounds and budget Android screens.

4. **Thumb is white (#FFFFFF) in all states**
   Maximum contrast on both neutral and branded track fills.

5. **Disabled On state uses frame opacity 0.4**
   Matches checkbox checked + disabled pattern exactly.

6. **Disabled Off state uses stroke opacity 0.4 + `color/surface/subtle` fill**
   Matches checkbox unchecked + disabled pattern.

7. **No Error state**
   Switches are instant-apply binary toggles, not form inputs requiring validation. Consistent with M3, iOS, Polaris, and Primer.

8. **Track size 52 x 32 px with 24 x 24 px thumb**
   Follows M3 proportions, provides comfortable touch area on mobile.

9. **Label uses `type/body/md` (not label token)**
   Describes what the toggle controls (content), not a UI control label. Consistent with Checkbox and Radio.

10. **Cross-axis CENTER alignment**
    Vertically centers 32 px track with label text, unlike Checkbox/Radio which use MIN (20 px indicator matches 20 px text).

---

## 20. Open questions

1. **Should the thumb have a drop shadow?**
   Some design systems (Material Design 3, iOS) add a subtle shadow to the thumb for depth. This spec omits it for V1 to match the flat inline control approach used by Checkbox and Radio. Should a thumb shadow be added for visual polish?

2. **Hindi switch labels.**
   Same question as Checkbox and Radio. Devanagari characters are taller and wider. Does the track stay correctly vertically centered when Hindi labels wrap?

3. **Touch target sizing.**
   The track is 32 px tall. Should the component include built-in vertical padding for a 48 px touch target, or should this remain a layout concern? Same question as Checkbox and Radio.

4. **Should the Off track use `color/surface/subtle` instead of `color/surface/muted`?**
   The current spec uses `color/surface/muted` (#E8E4F0) for the Off track fill. `color/surface/subtle` (#F1EDF7) is lighter and may be too faint. Needs visual testing on budget Android screens.

5. **Thumb position in Disabled state.**
   Should the disabled thumb remain at the same position (Off=left, On=right), or should there be any visual differentiation beyond opacity? Current spec keeps position unchanged.
