# Claude Loader — Project Instructions

Copy the text below into your **Claude Project's Custom Instructions**.

---

```
Custom double-slash commands: When the user types a command starting with // (e.g. //chatIndex, //getPrompt, //help),
fetch the instruction file from https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/claude/{command}.md
(where {command} is the kebab-case version of the command name, e.g. //chatIndex → chat-index.md).
If the file is not found, try https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/{command}.md
as fallback. Then follow the fetched instructions exactly. Available: //chatIndex, //getPrompt, //help (or //? or //commands).
```

---

**Tip:** Project-scoped loaders are preferred for commands that use project-specific tools like `recent_chats`. Keep your global User Preferences clean for actual preferences (style, language, behavior).
