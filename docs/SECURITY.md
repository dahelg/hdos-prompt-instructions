# Security Model

## Threat Analysis

This repository uses a **fetch-and-execute pattern**: an AI chatbot reads instructions from a public URL and follows them within the user's session. This creates a unique attack surface.

### Threat 1: Repository Compromise

**Risk:** If an attacker gains write access to `main`, they can modify command files to include malicious instructions.

**Impact:** The AI could be instructed to:
- Exfiltrate data from connected services (Gmail, Google Drive, Calendar)
- Output sensitive information from the user's context
- Perform actions on behalf of the user

**Mitigations:**
- Enable branch protection on `main` (require PR review, no force push)
- Enable 2FA on the repository owner's GitHub account
- Use signed commits where possible
- Regularly audit command file contents
- Keep the collaborator list minimal (ideally: owner only)

### Threat 2: Malicious Pull Requests

**Risk:** An external contributor submits a PR with hidden malicious instructions.

**Impact:** Same as Threat 1 if merged.

**Mitigations:**
- Never merge PRs without careful manual review of the full diff
- Pay special attention to instructions that reference external URLs, data exfiltration, or destructive actions
- Use GitHub's code review tools to flag suspicious patterns

### Threat 3: Prompt Injection via Command Content

**Risk:** A command file could contain instructions that override the AI's safety guidelines or manipulate it into ignoring user intent.

**Impact:** Unexpected AI behavior, potential data exposure.

**Mitigations:**
- Every command includes a mandatory `## Security Constraints` section
- Commands are designed to be additive (they define what TO do), not override-based
- Commands must never use phrases like "ignore previous instructions" or "disregard safety guidelines"

### Threat 4: Supply Chain via Loader

**Risk:** The loader text in the user's chatbot settings points to this repo. If the repo is deleted, transferred, or renamed, the URL breaks — or worse, someone else could claim the namespace.

**Impact:** Commands stop working, or a malicious actor serves content at the old URL.

**Mitigations:**
- Use a stable GitHub account with a strong password and 2FA
- Never transfer or rename the repository without updating all loaders
- Pin to a specific commit hash in high-security environments:
  `https://raw.githubusercontent.com/dahelg/hdos-prompt-instructions/{COMMIT_SHA}/commands/...`

### Threat 5: Information Leakage via Public Files

**Risk:** Command files or examples accidentally contain personal information.

**Impact:** Public exposure of names, email addresses, file paths, internal URLs, etc.

**Mitigations:**
- Pre-commit review checklist (see below)
- `.gitignore` blocks common secret file patterns
- CLAUDE.md explicitly prohibits personal data in command files
- All examples use placeholder data only

## Pre-Commit Checklist

Before committing any file, verify:

- [ ] No personal names, email addresses, or usernames
- [ ] No file paths referencing local machines
- [ ] No API keys, tokens, or credentials
- [ ] No internal URLs or IP addresses
- [ ] No instructions to send data to external services
- [ ] No instructions to override AI safety guidelines
- [ ] `## Security Constraints` section present in every command file
- [ ] All example data uses generic placeholders

## Reporting Vulnerabilities

If you discover a security issue in any command file, please open a GitHub issue or contact the repository owner directly. Do not submit a PR that demonstrates the vulnerability.
