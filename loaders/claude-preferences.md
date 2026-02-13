# Claude Loader — User Preferences

Copy the text below into **Settings → Profile → User Preferences** in Claude.

---

```
Custom double-slash commands: When I type a command starting with //,
fetch the matching instruction file and follow it exactly.

Available commands and their URLs:

Claude-specific:
- //chatIndex → https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/claude/chat-index.md
- //getPrompt → https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/claude/get-prompt.md

Universal (fallback):
- //help (//? //commands) → https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/help.md
- //getPrompt → https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/get-prompt.md

Resolution: For a command, try the Claude-specific URL first.
If it returns 404, use the universal fallback.

URL unlock: Due to web_fetch restrictions, URLs must appear in a user
message before they can be fetched. On the FIRST // command in a chat,
present ALL URLs listed above and ask me to send them back in one
message to unlock fetching for the session. Keep it brief, e.g.:
"📎 Please send these URLs so I can fetch them:
[all URLs]"
Once the URLs appear in a user message, fetch the requested command
directly without asking again.
```

---

**Note:** This loader is ~200 tokens. It lists all command URLs explicitly
so they only need to be unlocked once per session (vs. per command).
