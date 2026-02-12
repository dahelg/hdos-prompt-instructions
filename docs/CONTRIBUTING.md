# Contributing

## Adding a New Command

1. Create a branch: `feat/{command-name}`
2. Add the command file in the appropriate directory:
   - `commands/universal/` for platform-agnostic commands
   - `commands/{platform}/` for platform-specific overrides
3. Follow the command file format (see CLAUDE.md)
4. Include the mandatory `## Security Constraints` section
5. Test the command manually in the target chatbot
6. Run through the pre-commit checklist (see docs/SECURITY.md)
7. Open a PR against `main`

## Command Naming

- Filenames: `kebab-case.md` (e.g., `chat-index.md`)
- Double-slash command: `camelCase` (e.g., `//chatIndex`)
- The mapping is defined in the command file's `## Syntax` section

## What Makes a Good Command

- **Single purpose** — one command does one thing
- **Self-contained** — no dependencies on other commands
- **Platform-aware** — uses native tools where available, falls back gracefully
- **Safe** — read-only, no side effects, no data exfiltration
- **Documented** — clear syntax, options, and examples
