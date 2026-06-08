# Skill clarifications — surfaced during V2 Android rebuild

Findings collected while rebuilding `wiom-ds-android` against the V2 skill set
(locked SHA `ed110b6`). Each item is an edge case the current skill text doesn't
spell out — the V2 ship behaviour is correct, but the prose could be tighter so
the next consumer derives the right code on the first try.

| # | Skill | Section | Finding |
|---|---|---|---|
| 1 | `wiom-cta` | §9 (Compose API + code) | The example wraps the inner `Row` inside a `Surface`, which auto-sizes to its content. Real-world callers wrap the `Row` inside a `Box(modifier.fillMaxWidth())` instead — the `Arrangement.spacedBy(_, Alignment.CenterHorizontally)` only centers when the Row is wider than its content. The `Box` therefore needs `contentAlignment = Alignment.Center` (or the Row needs `fillMaxWidth()`). The Android v2 rebuild surfaced this as a real CTA-label-centering bug in v1.x. **Suggested fix:** add a one-line note to §9 / §7 universal rules calling out the constraint. |
| 2 | `wiom-input-fields` | §6 (OTP anatomy) | The boxed-OTP variant doesn't specify a typography token for the digit text. Implementations have to guess. Skill anatomy says "wrap = 12 V + 24 LH + 12 V = 48 dp" so any 16/24 token works (`type.label.lg` or `type.body.lg`). **Suggested fix:** name the token explicitly. The Android impl picked `type.label.lg`. |
| 3 | `wiom-input-fields` | §6 (OTP) | Recommended Compose pattern (single `BasicTextField` + `decorationBox` rendering N visual cells) isn't called out — designers / implementers reach for `N` separate fields with manual focus chaining, which is the wrong pattern. **Suggested fix:** add the canonical Compose snippet to §8 like the other helpers have. |
| 4 | `wiom-loader` | Typing dots | 10 dp vertical padding inside the dots cell is off the 4-dp grid. **Suggested fix:** specify `space-12` (or document the intentional off-grid choice). |
| 5 | `wiom-tabs-filters` | §1.5 (4+ pill tabs) | The "horizontally scrollable" fallback for 4+ pill tabs is described in prose but no example. The Android code allows up to 4 hard-capped (no scroll fallback). **Suggested fix:** either add the scroll-fallback example to the skill or tighten the cap to 3 in code (matching Figma `Count = 1/2/3`). |
| 6 | `wiom-progress-indicator` | §1.3 (Milestones glyphs) | Stage glyphs are direct-edit per instance in Figma (no `INSTANCE_SWAP` property). The Compose API takes them as `WiomMilestoneStage(label, icon)`, but the skill doesn't say what icon family they should come from. **Suggested fix:** name `Icons.Rounded.*` (or `painterResource(R.drawable.ic_*_rounded)` for brand-specific glyphs) explicitly. |

## How these were resolved on the Android side (v2.0.0)

| # | Code resolution |
|---|---|
| 1 | Outer `Box` in `WiomButton` now uses `contentAlignment = Alignment.Center`. |
| 2 | `WiomOtp` digit text uses `type.label.lg` (16/24 SemiBold). |
| 3 | `WiomOtp` ships single `BasicTextField` + `decorationBox` rendering N weighted boxes; active digit gets `stroke.brand.focus`. |
| 4 | `WiomDots` uses 4-dp-grid vertical padding (off-grid choice rejected; Android picks the safer canonical value). |
| 5 | Android v0.3.0 currently caps Pill Tab at 2..4 hard (no scroll fallback). To tighten to 1..3 in the next minor release to match the skill. |
| 6 | `WiomMilestoneStage(label, icon: WiomMilestoneIcon)` — caller passes either `WiomMilestoneIcon.vector(Icons.Rounded.X)` (preferred) or `WiomMilestoneIcon.resource(R.drawable.ic_*_rounded)` (fallback for brand glyphs not in Material3). |

## Skill amendment status (2026-05-20)

All six clarifications are now folded into the skill text. Resolutions:

| # | Skill | Applied amendment |
|---|---|---|
| 1 | `wiom-cta` | §7 universal rule 10 extended with the centering constraint; §9 inner row wrapped in `Box(fillMaxWidth, contentAlignment = Center)`. |
| 2 | `wiom-input-fields` | §6 OTP anatomy now names `type.label.lg` for the digit (with a callout against `type.body.lg`). |
| 3 | `wiom-input-fields` | §8 ships the canonical single-`BasicTextField` + `decorationBox` snippet with active-cell handling. |
| 4 | `wiom-loader` | Dots bubble padding corrected to 12 H · 12 V (`space-12`); on-grid. |
| 5 | `wiom-tabs-filters` | §1.4 / §1.5 tightened — Pill Track is hard-capped at 1..3, no scroll fallback. Android v0.3.0 (2..4) to tighten next release. |
| 6 | `wiom-progress-indicator` | `WiomMilestoneStage` icon now a sealed `WiomMilestoneIcon` (Vector / Resource). Material3 `Icons.Rounded.*` preferred; brand drawable fallback documented. |

## Reviewer notes

These are clarifications, not bug fixes. The V2 ship behaviour is what's correct
on the Wiom side — the Android rebuild now matches it. This document remains so
the next consumer (iOS, Web) doesn't re-derive these edge cases from scratch,
and so the history of why the prose now reads the way it does is preserved.
