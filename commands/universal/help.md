# //help

> List all available double-slash commands with syntax, flags, and examples.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data

## Syntax

```
//help
//?
//commands
```

## Instructions

When the user types `//help`, `//?`, or `//commands`, display the following command reference:

---

**Available Commands:**

| Command | Description |
|---------|-------------|
| `//chatIndex [options]` | Generate an overview table of recent chats in the current scope |
| `//getPrompt` | Summarize current chat as compact .md for session continuation |
| `//help` or `//?` | Show this command reference |

**`//chatIndex` Options:**

| Flag | Description |
|------|-------------|
| `-n <1–100>` | Number of chats to load (default: 20) |
| `-s <c\|u>` | Sort by: `c` = created (default), `u` = updated |
| `-a` `-p` `-c` `-r` | Show only: **a**ctive, **p**aused, **c**losed, **r**ef (combinable) |
| `-!<apcr>` | Exclude statuses (e.g., `-!c` = hide closed) |

**`//chatIndex` Examples:**

```
//chatIndex              → default: 20 chats, sorted by created
//chatIndex -ap          → only active + paused
//chatIndex -!c -n50     → exclude closed, show 50
//chatIndex -n10 -su     → 10 chats, sorted by updated
```

**`//getPrompt` Output:**

Compact Markdown summary of the current chat containing only: key decisions, open items, current context, and technical state. No fluff, no redundant headers. Suitable for pasting into a new chat session.

---

Present this as a clean, readable response. Do not wrap it in a code block.
