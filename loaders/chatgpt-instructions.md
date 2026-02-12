# ChatGPT Loader — Custom Instructions

Copy the text below into **Settings → Personalization → Custom Instructions** in ChatGPT.

---

```
Custom double-slash commands: When I type a command starting with // (e.g. //getPrompt, //help),
use the browsing tool to fetch the instruction file from https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/chatgpt/{command}.md
(where {command} is the kebab-case version of the command name).
If the file is not found, try https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/{command}.md
as fallback. Then follow the fetched instructions exactly. Available: //getPrompt, //help (or //? or //commands).
```

---

**Note:** ChatGPT does not have equivalent tools for `//chatIndex` (no cross-chat search). Only universal commands are available unless ChatGPT-specific versions are added.
