# CLAUDE.md — hdos-prompt-instructions

## What This Project Is

A public repository of reusable, version-controlled prompt commands for AI chatbots (Claude, ChatGPT, Gemini, etc.). Commands are fetched on-demand via URL to save context window space. All commands use the `//` (double-slash) prefix to avoid collisions with platform-native slash commands.

## Project Structure

```
commands/
├── universal/      # Platform-agnostic commands (fallback)
├── claude/         # Claude-specific (uses Claude tools like recent_chats, web_fetch)
├── chatgpt/        # ChatGPT-specific overrides
└── gemini/         # Gemini-specific overrides

loaders/            # Minimal text snippets users paste into their chatbot settings
docs/               # Security model, contributing guide, architecture
examples/           # Sample outputs
```

## Command Resolution

Platform-specific (`commands/{platform}/`) takes precedence over `commands/universal/`.

## Command Prefix

All commands use `//` (double-slash), e.g. `//chatIndex`, `//getPrompt`, `//help`.
This avoids collisions with `/` commands in Discord, Slack, Claude Code, and other platforms.

## Critical Rules

### Security (NON-NEGOTIABLE)

1. **No personal data** in any command file — no names, emails, paths, account IDs
2. **No external data transmission** — commands must NEVER instruct an AI to POST, send, or transmit data to any URL, API, webhook, or external service
3. **No destructive actions** — commands must NEVER delete, modify, or overwrite user data
4. **No credential access** — commands must NEVER attempt to read, display, or reference API keys, tokens, passwords, or secrets
5. **Read-only output** — commands generate text output only; they do not create files, modify settings, or change state
6. **No cross-command dependencies** — each command must be fully self-contained
7. **No embedded URLs** — commands must not instruct the AI to fetch additional URLs beyond what the loader already defines

### Command File Format

Every command file MUST include:

```markdown
# //command-name

> One-line description.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data

## Syntax
## Options (if any)
## Instructions
```

The `## Security Constraints` section is mandatory and must appear before `## Instructions`.

### Code Style

- Markdown files only (no executable code)
- Command names use kebab-case: `chat-index.md`, `get-prompt.md`
- Filenames match the command name without the prefix: `//chatIndex` → `chat-index.md`
- Line length: soft limit 100 characters
- Use fenced code blocks for examples
- Write in English

### Branching

- `main` is protected — no direct pushes
- Feature branches: `feat/{command-name}` or `fix/{command-name}`
- All changes via PR

## License

MIT — see LICENSE. This means anyone can use, modify, and distribute these commands.
Because this is public and MIT-licensed, assume every file will be read by strangers.
Never commit anything you wouldn't want on a billboard.
