# Chat Domain Tokens

Chat screens use their own tokens, namespaced `chat.*`. These are **scoped** — only valid inside chat UI. Never apply to product screens (agents inferring "system bubble grey" for a card background is the mistake this namespace prevents).

---

## Why chat uses Gray, not Neutral

Chat bubbles must look like pure grays — Neutral's warm mauve undertone reads as tinted next to user-generated content and competes with the pure-white user bubble. Don't unify chat with product neutrals.

---

## Tokens

4 tokens cover the essential chat surfaces. Extend component-side for timestamps, read receipts, etc.

| Token | Hex | Primitive | Use |
|---|---|---|---|
| `chat.bg.userBubble` | #FFFFFF | Gray_50 | Customer's own message (right-aligned) |
| `chat.bg.canvas` | #F5F6F7 | Gray_100 | Chat screen background |
| `chat.bg.systemBubble` | #EBEDF0 | Gray_200 | System/agent message (left-aligned) + reply overlay bg |
| `chat.stroke.replyAccent` | #B6B9BF | Gray_300 | 2dp left-edge stroke on reply overlay |

---

## Layout

```
┌─────────────────────────────────┐
│                                 │  ← chat.bg.canvas (#F5F6F7)
│  ┌───────────────────┐          │
│  │ Hi, I have a...   │          │  ← chat.bg.systemBubble (#EBEDF0)
│  └───────────────────┘          │
│                                 │
│          ┌───────────────────┐  │
│          │ Sure, I can help  │  │  ← chat.bg.userBubble (#FFFFFF)
│          └───────────────────┘  │
│                                 │
│  ┌───────────────────┐          │
│  │ ▎Replied to:      │          │  ← chat.bg.systemBubble + replyAccent
│  │   Sure, I can...  │          │
│  │                   │          │
│  │ Great. Here's...  │          │
│  └───────────────────┘          │
│                                 │
└─────────────────────────────────┘
```

---

## Text inside chat bubbles

Use core text tokens — chat doesn't need its own text palette.
- Message body: `text.default` (#161021)
- Timestamp / read-receipt: `text.subtle` (#5C5570)
- System-message prefix ("Wiom Support"): `text.subtle`

---

## Extensions (component-level, not in foundation)

These aren't in the token set — handle them in the chat component itself:
- **Typing indicator dots**: Use Gray_400 (#9FA1A7) with animation
- **Read receipt checkmarks**: `icon.brand` (#D9008D) when read, `icon.nonAction` (#898296) when delivered
- **Media message backgrounds**: `chat.bg.systemBubble` (messages with images) or `chat.bg.userBubble`
- **Timestamp divider ("Today", "Yesterday")**: `text.subtle` centered, no background

If chat adds more distinct bubble types (e.g., bot-generated, voice-note), add to this token family rather than reusing product tokens.

---

## Anti-patterns

- **Using `bg.subtle` (Neutral_100) as chat canvas**: It's warm mauve — the purity of Gray matters in chat
- **Using `chat.bg.userBubble` on a product card**: `chat.*` is scoped to chat screens only
- **White text on outgoing bubbles**: Body text is always `text.default`. The white fill of the user bubble is the bubble background, not a "white-label CTA" pattern
- **Adding brand pink to chat bubbles**: Chat is a neutral utility surface. Brand pink should appear only in interactive elements (action buttons in attachments, links) — not in bubble fills
