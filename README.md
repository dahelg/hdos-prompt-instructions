# hdos-prompt-instructions

Reusable, version-controlled prompt commands for AI chatbots.

Instead of bloating your chatbot's system prompt with dozens of instructions that consume context window on every message, this repo lets you **fetch commands on demand** — only when you actually use them.

Commands use the `//` (double-slash) prefix to avoid collisions with built-in slash commands in any chat platform.

## How It Works

```
You type:  //chatIndex -ap
    ↓
Chatbot reads your loader (tiny, always loaded)
    ↓
Chatbot fetches the command file from this repo via URL
    ↓
Chatbot follows the instructions in that file
    ↓
Context window is only used for THIS turn
```

## Supported Platforms

| Platform | Loader Location | Fetch Method |
|----------|----------------|--------------|
| **Claude** (claude.ai) | User Preferences / Project Instructions | `web_fetch` tool |
| **ChatGPT** | Custom Instructions / GPT Instructions | browsing tool |
| **Gemini** | Gems / Custom Instructions | URL fetch |
| **Other** | System prompt or equivalent | varies |

## Quick Start

### 1. Copy the loader into your chatbot

Go to `loaders/` and pick the loader for your platform. Copy its content into your chatbot's settings.

**Example for Claude (User Preferences):**

```
When I type a double-slash command (e.g. //chatIndex, //getPrompt, //help),
fetch the command definition from
https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/{command-name}.md
and follow its instructions exactly. If a platform-specific version exists under commands/claude/,
prefer that. Fall back to commands/universal/ otherwise. Available: //help
```

### 2. Use commands

```
//help              → lists all available commands
//chatIndex         → generates a chat overview table
//chatIndex -ap     → only active + paused chats
//getPrompt         → summarizes chat for session transfer
```

## Project Structure

```
hdos-prompt-instructions/
├── commands/
│   ├── universal/          # Platform-agnostic commands
│   │   ├── help.md
│   │   └── get-prompt.md
│   ├── claude/             # Claude-specific commands (use Claude tools)
│   │   ├── chat-index.md
│   │   └── get-prompt.md
│   ├── chatgpt/            # ChatGPT-specific overrides
│   └── gemini/             # Gemini-specific overrides
├── loaders/
│   ├── claude-preferences.md     # Minimal loader for Claude User Preferences
│   ├── claude-project.md         # Loader for Claude Project Instructions
│   ├── chatgpt-instructions.md   # Loader for ChatGPT Custom Instructions
│   └── gemini-instructions.md    # Loader for Gemini
├── docs/
│   ├── SECURITY.md         # Security model and threat analysis
│   ├── CONTRIBUTING.md     # How to add commands safely
│   └── ARCHITECTURE.md     # Design decisions
├── examples/
│   └── demo-output.md      # Example outputs from commands
├── CLAUDE.md               # Claude Code project context
├── LICENSE                  # MIT
└── README.md
```

## Command Resolution Order

When you invoke `//commandName`, the chatbot should resolve in this order:

1. `commands/{platform}/{command-name}.md` (platform-specific)
2. `commands/universal/{command-name}.md` (fallback)

This allows platform-specific commands to leverage native tools (e.g., Claude's `recent_chats` tool) while sharing logic where possible.

## Why `//` Instead of `/`?

Many chat platforms reserve `/` for built-in commands (e.g., Discord, Slack, Claude Code). The double-slash `//` prefix:

- Avoids collisions with any known chat platform's built-in commands
- Is easy to type on every keyboard layout (DE, US, UK) without Shift
- Is intuitively readable as "extended command"
- Semantically echoes comments in programming languages — a fitting "meta-level" marker

## Writing Commands

Each command file follows a standard format:

```markdown
# //commandName

> One-line description of what this command does.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data

## Syntax

//commandName [options]

## Options

-n <number>   Description of flag
-v            Description of flag

## Instructions

(The actual instructions for the AI to follow)
```

### Rules for Command Authors

1. **No personal data** — commands must be generic and reusable by anyone
2. **No external calls** — commands must never instruct the AI to POST data anywhere
3. **No destructive actions** — commands must never delete, modify, or overwrite user data
4. **Explicit security block** — every command MUST include the Security Constraints section
5. **Idempotent** — running a command twice should produce the same result
6. **Self-contained** — commands must not depend on other commands

## Security

See [docs/SECURITY.md](docs/SECURITY.md) for the full threat model.

**Key points:**

- This repo uses a **fetch-and-execute pattern** — the AI reads instructions from a URL and follows them. This means whoever controls this repo controls what the AI does in your session.
- Only the repo owner should have write access to `main`.
- **Enable branch protection** on `main` (require PR reviews, no force push).
- Commands are designed to be **read-only** — they generate output but never modify external state.
- Never put secrets, account names, personal paths, or credentials in command files.

## FAQ

**Does this actually save context window?**
Yes. A typical loader is ~50–80 tokens. A full command definition is ~300–800 tokens. Without this pattern, all commands would be loaded into every single message (~2,000+ tokens). With this pattern, only the loader is always present, and command definitions are fetched on-demand.

**What if GitHub is down?**
The command will fail gracefully. The AI should inform you that it couldn't fetch the command and suggest you try again.

**Can I use this for private commands?**
Yes, but you'd need a private repo and a way for your AI to authenticate. For Claude, `web_fetch` only works with public URLs. Consider keeping sensitive commands directly in your Project Instructions instead.

**Can someone inject malicious instructions via PR?**
Not if you don't merge them. Enable branch protection and review all PRs carefully. See [docs/SECURITY.md](docs/SECURITY.md).

## License

MIT — see [LICENSE](LICENSE)

## Author

Created by [Helge Drews](https://github.com/dahelg) as a personal productivity tool and shared with the community.
