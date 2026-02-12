# //getPrompt

> Summarize the current chat as a compact .md file for continuing in a new session.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data
- The output MUST only contain information already present in the current conversation

## Syntax

```
//getPrompt
```

## Instructions

When the user types `//getPrompt`, generate a compact Markdown summary of the current conversation. The output should enable seamless continuation in a new chat session.

**Include only:**

1. **Context** — What is this conversation about? (1–2 sentences)
2. **Key Decisions** — Decisions made during this chat (bullet list, keep brief)
3. **Current State** — What has been built/done so far?
4. **Open Items** — What still needs to be done? What's blocked?
5. **Technical Details** — File paths, versions, config values, or commands that would be tedious to re-derive
6. **Important Constraints** — Any rules or boundaries established during the chat

**Exclude:**

- Greetings, small talk, or meta-discussion about the summary itself
- Information the AI can derive from project files or common knowledge
- Redundant section headers like "Summary" or "Chat Export"
- Timestamps or message attribution

**Format:**

- Plain Markdown, no YAML frontmatter
- Use `##` headers for the sections above, but only if the section has content
- Bullet points for lists, inline code for technical values
- Target length: 200–600 words (scale with conversation complexity)

**Delivery:**

- If the platform supports file creation (e.g., Claude), create a `.md` file and present it for download
- Otherwise, output the Markdown directly in the chat as a code block
