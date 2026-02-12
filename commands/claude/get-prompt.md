# //getPrompt

> Summarize the current chat as a compact .md file for continuing in a new session.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data
- The output MUST only contain information already present in the current conversation

## Platform

Claude (claude.ai) — uses file creation and `present_files` tools for download.

## Syntax

```
//getPrompt
```

## Instructions

Generate a compact Markdown summary of the current conversation that enables seamless continuation in a new chat session.

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

**Delivery (Claude-specific):**

1. Create the file at `/mnt/user-data/outputs/chat-prompt.md`
2. Use the `present_files` tool to make it available for download
3. Do not output the full content in the chat — just confirm delivery with a one-line summary of what's in the file
