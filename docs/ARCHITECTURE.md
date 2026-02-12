# Architecture

## Design Decisions

### Why fetch-on-demand instead of inline?

AI chatbot context windows are limited and expensive. Every token in a system prompt or user preferences is loaded into **every single message** — even when the user doesn't need it.

| Approach | Tokens per message | Tokens when using command |
|----------|-------------------|-------------------------|
| All commands inline | ~2,000+ always | ~2,000+ (already loaded) |
| Fetch-on-demand | ~50–80 (loader only) | ~50–80 + ~300–800 (fetched) |

Over a 50-message conversation, the inline approach wastes ~100,000 tokens on unused commands. The fetch approach uses ~4,000 tokens for the loader and only loads command definitions when actually invoked.

### Why GitHub raw URLs?

- Free, reliable, globally cached CDN
- Version-controlled with full audit trail
- Public visibility enables community contributions
- Raw URLs are stable as long as the repo exists
- No authentication required for public repos

### Why platform-specific directories?

Different chatbots have different capabilities:

- **Claude** has `recent_chats`, `conversation_search`, `web_fetch`, and file creation tools
- **ChatGPT** has browsing, DALL-E, and code interpreter
- **Gemini** has Google Search integration

A command like `//chatIndex` needs Claude's `recent_chats` tool — it simply can't work the same way in ChatGPT. Platform-specific directories allow optimized implementations while sharing the command interface.

### Why `//` instead of `/`?

Many chat platforms reserve `/` for built-in commands:

- **Discord** — `/giphy`, `/tts`, custom bot commands
- **Slack** — `/remind`, `/status`, `/call`
- **Claude Code** — `/compact`, `/clear`, `/help`
- **ChatGPT** — potential future use

The double-slash `//` prefix:
- Has no known collision with any chat platform
- Is easy to type on every keyboard layout without Shift
- Is intuitively readable as "extended command"
- Echoes comment syntax in programming languages (C, JS, Java) — fitting for a "meta" layer

### Why mandatory Security Constraints?

The fetch-and-execute pattern is powerful but dangerous. The security section in every command file serves as:

1. **Documentation** — makes the safety model explicit
2. **AI guardrail** — the AI reads these constraints before the instructions
3. **Review aid** — PRs without this section are automatically non-compliant

### Why camelCase commands but kebab-case files?

- `//chatIndex` is easier to type than `//chat-index` in a chat interface
- `chat-index.md` follows standard file naming conventions
- The mapping is explicit in each command's `## Syntax` section
