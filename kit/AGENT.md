# AMS — Agent Protocol

Follow this protocol at the start and end of every session.

HANDOFF is required. Everything else is optional and is followed only when `CONFIG.md` lists it.

If this project has not been set up yet, read `INSTALL.md` instead and run that wizard.

---

## Finding Your Directories

1. Look for an `AMS/` directory in the project root. If not found, look for `.ams/`.
2. Inside that directory, read `CONFIG.md`.
3. Read the `components` setting. Treat `HANDOFF` as enabled even if omitted.
4. Resolve each configured directory: use **Your value** if set, otherwise the default. Look inside `AMS/` first; if the directory is not there, check the project root.

**Defaults:**

| Directory | Default name | Purpose | Required |
|---|---|---|---|
| Handoff | `HANDOFF` | Session journals, chronological | yes |
| Doc | `DOC` | Reference docs, persistent by topic | no |
| Learnings | `LEARNINGS` | Retros and topical findings | no |
| Sprints | `SPRINTS` | Sprint plans and status | no |
| Epics | `EPICS` | Cross-sprint work (only if `epics: on`) | no |
| Offices | `OFFICES` | Per-persona working memory | no |
| Marketing | `MARKETING` | Experimental | no |
| Security | `SECURITY` | Experimental | no |

---

## Enabled components

After reading CONFIG, follow each enabled component's protocol:

| Component | Protocol |
|---|---|
| `HANDOFF` | This file (the rest of it) |
| `DOC` | `{doc_dir}/PROTOCOL.md` |
| `LEARNINGS` | `{learnings_dir}/PROTOCOL.md` |
| `SPRINTS` | `{sprints_dir}/PROTOCOL.md` |
| `OFFICES` | `{offices_dir}/PROTOCOL.md` |
| `MARKETING` | `{marketing_dir}/PROTOCOL.md` |
| `SECURITY` | `{security_dir}/PROTOCOL.md` |

Do **not** follow a `PROTOCOL.md` unless that component is in `components`. Do not create or update DOC files if `DOC` is off. Do not assume sprints, offices, or learnings exist.

---

## Starting a Session

1. If `OFFICES` is enabled and `OFFICES/<your-persona>/` exists, read it first — start with `desk.md`.
2. Read the most recent files in the handoff directory (newest first — 2 or 3 is usually enough).
3. If `DOC` is enabled, read relevant files in the doc directory.
4. If `SPRINTS` is enabled, glance at the current sprint file when the work is sprint-shaped.
5. If `LEARNINGS` is enabled, read a relevant learning only when it bears on this session.
6. Summarize current project state for the user.
7. Ask what to work on.

---

## During a Session

When you learn something persistent about the project — architecture decisions, conventions, deploy steps, known issues:

- If `DOC` is enabled, update or create a relevant file in the doc directory. Prefer updating existing files. Add or update a `Last updated: yyyy-mm-dd by [author]` line at the bottom.
- Else if `LEARNINGS` is enabled, add it as a topical learning (see `{learnings_dir}/PROTOCOL.md`).
- Else put it in the session handoff.

---

## Ending a Session

Write a handoff document before the session ends.

**Filename:** `[handoff-dir]/handoff-[yyyy-mm-dd]-[short-description]-[author].md`

No spaces in filenames. Examples:

- `HANDOFF/handoff-2026-01-20-auth-bug-claude.md`
- `HANDOFF/handoff-2026-03-15-homepage-redesign-gemini.md`

**Include:**

- A header line: persona / model / story when known — e.g. `Cody · claude-sonnet-4.6 · S1-2`
- What was attempted and the outcome
- What worked, what didn't
- Current state and any blockers
- Open questions
- Files created or modified
- If `SPRINTS` is enabled, the sprint and story ids this session touched

**Prompt for Next Assistant.** The handoff file should include a `## Prompt for Next Assistant` section: a ready-to-paste prompt for the next session. Then **tell the human directly in your final chat message** (not only in the handoff file):

1. What is next and who should do it (persona and/or model), with a launch command when you know it
2. The exact prompt to paste, **as a literal code block in the chat message itself**

Pointing the human at a file ("paste the prompt at the end of the handoff") does not satisfy this: the human should never have to open a file to continue the pipeline.

The prompt should:

- State what to work on next
- List files to read first (`AMS/AGENT.md`, this handoff, the current sprint file if any, relevant DOC files)
- Summarize what is already done
- Remind the next agent of key constraints and to write a handoff when finished

**Offices.** If `OFFICES` is enabled and you maintain `OFFICES/<your-persona>/`, update it before ending — at minimum, overwrite `desk.md` with where things stand now. The handoff is the historical record; the office is what the persona (or a human returning after a break) reads first to reorient.

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

To choose components and write `CONFIG.md`, read `INSTALL.md`.

---

## Version History

- **3.1** (2026-08-24) — Repo split into payload (`kit/`) and repo-role files; `INSTALL-AMS.md`
  bootstrap places the kit and hides the installer; kit no longer ships a `.gitignore`, so
  projects track their own handoffs, docs, sprints, and learnings
- **3.0** (2026-08-23) — Components are independent; HANDOFF required; DOC/LEARNINGS/SPRINTS/OFFICES optional via `CONFIG.md`; next-assistant paste prompt; install wizard in `INSTALL.md`
- **2.0** (2026-04-23) — Moved to `AMS/` directory convention; added configurable directory names via `config.md`; fallback to project root for `HANDOFF/` and `DOC/`
- **1.1** (2026-02-16) — Split into README + AGENT.md; added tool-specific setup; simplified DOC guidance
- **1.0** (2026-01-25) — Initial protocol: HANDOFF/ and DOC/ directories, session workflow, naming convention
