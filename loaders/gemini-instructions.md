# Gemini Loader — Custom Instructions

Copy the text below into your **Gem's instructions** or **Gemini system prompt**.

---

```
Custom double-slash commands: When the user types a command starting with // (e.g. //getPrompt, //help),
fetch the instruction file from https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/gemini/{command}.md
(where {command} is the kebab-case version of the command name).
If the file is not found, try https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/main/commands/universal/{command}.md
as fallback. Then follow the fetched instructions exactly. Available: //getPrompt, //help (or //? or //commands).
```

---

**Note:** Gemini's URL fetch capabilities may vary. Test with `//help` first to verify the fetch-and-execute pattern works in your setup.
