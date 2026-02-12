# Claude Loader — User Preferences

Copy the text below into **Settings → Profile → User Preferences** in Claude.

---

```
Custom double-slash commands: When I type a command starting with // (e.g. //chatIndex, //getPrompt, //help),
fetch the instruction file from https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/claude/{command}.md
(where {command} is the kebab-case version of the command name, e.g. //chatIndex → chat-index.md).
If the file is not found, try https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/{command}.md
as fallback. Then follow the fetched instructions exactly. Available: //chatIndex, //getPrompt, //help (or //? or //commands).
```

---

**Note:** This loader is ~70 tokens. It replaces ~500+ tokens of inline command definitions.
