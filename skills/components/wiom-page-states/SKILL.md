---
name: wiom-page-states
description: >
  Wiom Page States: what a screen's content area shows when the whole
  page is loading, empty, or its main call failed (error with retry).
  Use when a screen waits for its first data, has no data, or its main
  call failed. Not for inline waits (wiom-loader) or a failed section
  (wiom-inline-banner). Trigger on: empty state, error screen, retry,
  no data, full-screen loading, first load, offline page.
---

# Wiom Page States

What a screen shows in its content area when the whole page is loading, empty or failed. The top bar stays, so back always works. Same rules on iOS (`wiom-ios-page-states`), owner decision 27 Nov 2026.

## 1. Which state

| Situation | State |
|---|---|
| First load, layout known | **Loading**: `WiomSkeleton` of that layout |
| First load, layout unknown or very short | **Loading**: `WiomSpinner` Lg, centred |
| App launch or an explicit branded wait | Brand loader (`wiom-loader`) |
| Data loaded, nothing to show | **Empty** |
| The page's main call failed | **Error with retry** |
| One section failed, the rest is fine | `wiom-inline-banner` in that section |
| Offline, cached data still visible | Persistent Warning toast (`wiom-toast`) |

## 2. Anatomy (empty and error)

A centred column in the content area, inside the 16 dp gutter.

| Slot | Value | Token |
|---|---|---|
| Badge | Icon Badge **Lg** (96 dp circle, 48 dp glyph) until illustrations exist (D61) | `wiom-icon-badge`; Neutral for empty, Critical for error |
| Badge to text | | `space-24` |
| Title | Centred, wraps | `type.heading.md` · `text.default` |
| Title to body | | `space-8` |
| Body | Centred, wraps | `type.body.md` · `text.subtle` |
| Text to CTA | | `space-32` |
| CTA | Empty: optional Primary that fixes the emptiness. Error: Primary "दोबारा कोशिश करें" / "Try again" | `wiom-cta` |

## 3. Rules

1. **Keep the top bar.** Never replace the whole screen.
2. **Skeleton first** when the layout is known; a spinner only when it isn't.
3. **Error always has a way forward:** Try again, plus back. Phone offline uses the app's line: "इंटरनेट से कनेक्ट करके दोबारा कोशिश करें" / "No internet connection on your phone." No error codes.
4. **Try again** re-runs the same call and shows Loading again; it never navigates. No auto-retry.
5. **Empty is not an error.** Neutral tone, and a next step when there is one.
6. Text wraps; the column scrolls at large font scale.

## 4. Compose

```kotlin
sealed interface PageState {
    data object Loading : PageState
    data class Empty(val title: String, val body: String, val cta: String? = null, val onCta: (() -> Unit)? = null) : PageState
    data class Error(val title: String, val body: String, val retryLabel: String, val onRetry: () -> Unit) : PageState
}

@Composable
fun WiomPageState(state: PageState, icon: ImageVector, modifier: Modifier = Modifier) {
    when (state) {
        PageState.Loading -> Box(modifier.fillMaxSize(), contentAlignment = Alignment.Center) {
            WiomSpinner(size = WiomSpinnerSize.Lg)
        }
        is PageState.Empty, is PageState.Error -> Column(
            modifier = modifier.fillMaxSize().verticalScroll(rememberScrollState()).padding(horizontal = Wiom.space.s16),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.Center,
        ) {
            val isError = state is PageState.Error
            WiomIconBadge(
                icon = icon,
                size = WiomIconBadgeSize.Lg,
                tone = if (isError) WiomIconBadgeTone.Critical else WiomIconBadgeTone.Neutral,
            )
            Spacer(Modifier.height(Wiom.space.s24))
            val (title, body) = when (state) { is PageState.Error -> state.title to state.body; is PageState.Empty -> state.title to state.body; else -> "" to "" }
            Text(title, style = Wiom.type.heading.md, color = Wiom.text.default, textAlign = TextAlign.Center)
            Spacer(Modifier.height(Wiom.space.s8))
            Text(body, style = Wiom.type.body.md, color = Wiom.text.subtle, textAlign = TextAlign.Center)
            val cta = when (state) { is PageState.Error -> state.retryLabel to state.onRetry; is PageState.Empty -> state.cta?.let { it to state.onCta!! }; else -> null }
            if (cta != null) {
                Spacer(Modifier.height(Wiom.space.s32))
                WiomButton(text = cta.first, type = WiomButtonType.Primary, onClick = cta.second, modifier = Modifier.fillMaxWidth())
            }
        }
    }
}
```

The `retryLabel` comes from the string holders ("दोबारा कोशिश करें" / "Try again"); never a literal in the composable.

## 5. Anti-patterns

- ❌ Replacing the top bar with the page state.
- ❌ A spinner when the layout is known (use a skeleton).
- ❌ Error codes or "Something went wrong" with no way forward.
- ❌ Auto-retry loops.

## 6. Open

- No page-state component in Figma yet; illustrations may replace the Icon Badge Lg later.
