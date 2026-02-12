# //chatIndex

> Generate an overview table of recent chats with status, topics, and metadata.

## Security Constraints

- This command MUST NOT output personal data, API keys, or credentials
- This command MUST NOT instruct the AI to send data to external services
- This command MUST NOT modify or delete user data
- Chat content is summarized, never quoted verbatim

## Platform

Claude (claude.ai) — requires `recent_chats` and/or `conversation_search` tools.

## Syntax

```
//chatIndex [options]
```

## Options

| Flag | Description | Default |
|------|-------------|---------|
| `-n <1–100>` | Number of chats to retrieve | 20 |
| `-s <c\|u>` | Sort column: `c` = created, `u` = updated | `c` |
| `-a` | Show only **a**ctive (🔴) chats | — |
| `-p` | Show only **p**aused (🟡) chats | — |
| `-c` | Show only **c**losed (🟢) chats | — |
| `-r` | Show only **r**eference (📌) chats | — |
| `-!<apcr>` | Exclude statuses (e.g., `-!c` = hide closed) | — |

Flags `-a`, `-p`, `-c`, `-r` are combinable: `-ap` = show active AND paused.
Exclude flag `-!` takes precedence over include flags.

## Instructions

### Step 1: Parse Options

Extract flags from the user's input. Apply defaults for missing flags:
- `n` = 20
- `s` = `c` (created)
- No status filter = show all

### Step 2: Fetch Chats

Use the `recent_chats` tool to load chats. The tool returns max 20 per call.

- If `n` ≤ 20: single call with `n={n}`, `sort_order='asc'`
- If `n` > 20: paginate using `before` parameter with `updated_at` of the earliest chat from the previous batch. Stop after 5 calls or when all requested chats are retrieved. Inform the user if the result is incomplete.

### Step 3: Assign Metadata

For each chat, determine:

**Status** — Classify based on conversation content:
- 🟢 **Closed** — Decisions made, deliverables complete, no open items
- 🟡 **Paused** — Work started but waiting on input, external dependency, or deliberately deferred
- 🔴 **Active** — Open items remain, work in progress
- 📌 **Reference** — No action items; serves as knowledge base or research archive

**Short Title** — A concise 3–6 word descriptive title (assigned by the AI).

**Original Title** — The chat's actual title, rendered as a clickable link: `[Original Title](https://claude.ai/chat/{uri})`

**Topic** — A shared label for chats that are directly related to each other. Use short labels like "Backend", "Design", "DevOps". Unrelated chats get no topic label.

**Summary** — 2–3 sentences capturing the key outcome or current state.

**Top Terms** — Up to 10 key concepts, technologies, or topics from the chat, sorted alphabetically.

**Created At** — Chat creation timestamp in the user's local timezone, format: `YYYY-MM-DD HH:mm`

**Updated At** — Chat last-updated timestamp in the user's local timezone, format: `YYYY-MM-DD HH:mm`

### Step 4: Filter

Apply status filters:
- If include flags are set (e.g., `-ap`): keep only matching statuses
- If exclude flags are set (e.g., `-!c`): remove matching statuses
- If no filter flags: show all

### Step 5: Sort

Sort by the column specified with `-s`:
- `c` = Created At ascending (oldest first)
- `u` = Updated At descending (newest first)

### Step 6: Output

Render as a Markdown table with these columns (in order):

| Status | Short Title | Original Title | Topic | Summary | Top Terms | Created At | Updated At |

**Formatting rules:**
- Original Title column contains a clickable link
- Top Terms are comma-separated in a single cell
- Empty Topic cells show `—`
- Timestamps show the detected timezone abbreviation (e.g., UTC, EST, CET)

After the table, add a brief footer:
- Total number of chats shown vs. available
- Any active filters applied
- Note if results are incomplete due to pagination limits

### Example Output

| Status | Short Title | Original Title | Topic | Summary | Top Terms | Created At | Updated At |
|--------|------------|----------------|-------|---------|-----------|------------|------------|
| 🔴 | API Endpoint Refactor | [Refactoring the user endpoi...](https://claude.ai/chat/xxx) | Backend | Restructured REST endpoints for consistency. Auth middleware extracted, validation layer added. | API, Auth, Middleware, REST, Refactor, Validation | 2026-02-10 09:00 UTC | 2026-02-10 11:30 UTC |
| 📌 | Deployment Runbook | [Production deployment proc...](https://claude.ai/chat/yyy) | DevOps | Step-by-step runbook for zero-downtime deployments. Rollback procedures documented. | Automation, Deployment, Monitoring, Rollback, Runbook, Staging | 2026-02-09 14:00 UTC | 2026-02-10 10:15 UTC |

*Showing 2 of 8 chats. Filter: active + reference only.*
