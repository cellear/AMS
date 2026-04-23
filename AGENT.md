# AMS — Agent Protocol

Follow this protocol at the start and end of every session.

---

## Finding Your Directories

AMS uses a small set of directories to store session journals and reference docs. Before doing anything else, locate them:

1. Look for an `AMS/` directory in the project root. If not found, look for `.ams/`.
2. Inside that directory, read `config.md` (if it exists) to find the configured names for the handoff and doc directories.
3. Look for those directories inside `AMS/` first. If they're not there, check the project root.

**Defaults** (used if no `config.md` exists):

| Directory | Default name | Purpose |
|---|---|---|
| Handoff | `HANDOFF` | Session journals, chronological |
| Doc | `DOC` | Reference docs, persistent by topic |

---

## Starting a Session

1. Read the most recent files in the handoff directory (newest first — 2 or 3 is usually enough)
2. Read relevant files in the doc directory
3. Summarize current project state for the user
4. Ask what to work on

---

## During a Session

When you learn something persistent about the project — architecture decisions, conventions, deploy steps, known issues — update or create a relevant file in the doc directory. Prefer updating existing files over creating new ones.

When updating a doc file, add or update a `Last updated: yyyy-mm-dd by [author]` line at the bottom.

---

## Ending a Session

Write a handoff document before the session ends.

**Filename:** `[handoff-dir]/handoff-[yyyy-mm-dd]-[short-description]-[author].md`

No spaces in filenames. Examples:

- `HANDOFF/handoff-2026-01-20-auth-bug-claude.md`
- `HANDOFF/handoff-2026-03-15-homepage-redesign-gemini.md`

**Include:**

- What was attempted and the outcome
- What worked, what didn't
- Current state and any blockers
- Open questions
- Files created or modified

---

## Commit Messages

Use multi-line commit messages: a short summary line, a blank line, then a body with bullet points covering what changed and why.

Every commit made by an AI assistant **must** include a `Co-Authored-By` trailer identifying the model:

    Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
    Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
    Co-Authored-By: Gemini CLI <noreply@google.com>
    Co-Authored-By: Codex <noreply@openai.com>
    Co-Authored-By: Cursor Composer <noreply@cursor.com>

---

## First-Time Setup

If this project doesn't have a tool-specific instruction file yet, create the appropriate one pointing to this file:

| Tool | File to create | Contents |
|---|---|---|
| Claude Code / Cowork | `CLAUDE.md` | `Read and follow AMS/AGENT.md` |
| OpenAI Codex | `AGENTS.md` | `Read and follow AMS/AGENT.md` |
| Cursor | `.cursorrules` | `Read and follow AMS/AGENT.md` |
| GitHub Copilot | `.github/copilot-instructions.md` | `Read and follow AMS/AGENT.md` |
| Gemini CLI | `.gemini/styleguide.md` | `Read and follow AMS/AGENT.md` |

Adjust the path if you've renamed `AMS/` to something else.

---

## Version History

- **2.0** (2026-04-23) — Moved to `AMS/` directory convention; added configurable directory names via `config.md`; fallback to project root for `HANDOFF/` and `DOC/`
- **1.1** (2026-02-16) — Split into README + AGENT.md; added tool-specific setup; simplified DOC guidance
- **1.0** (2026-01-25) — Initial protocol: HANDOFF/ and DOC/ directories, session workflow, naming convention
