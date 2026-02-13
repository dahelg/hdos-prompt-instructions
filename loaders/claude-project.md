# Claude Loader — Project Instructions

Copy the text below into your **Claude Project's Custom Instructions**.

---

```
Custom double-slash commands: When the user types a command starting with //,
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
present ALL URLs listed above and ask the user to send them back in one
message to unlock fetching for the session. Keep it brief, e.g.:
"📎 Please send these URLs so I can fetch them:
[all URLs]"
Once the URLs appear in a user message, fetch the requested command
directly without asking again.
```

---

**Tip:** Project-scoped loaders are preferred for commands that use
project-specific tools like `recent_chats`. Keep your global User
Preferences clean for actual preferences (style, language, behavior).
