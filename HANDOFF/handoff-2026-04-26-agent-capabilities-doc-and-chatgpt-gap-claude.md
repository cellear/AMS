# Handoff: Agent capabilities doc and ChatGPT filesystem gap

**Date:** 2026-04-26
**Author:** claude (Claude Opus 4.6)
**Status:** Complete

## What Was Attempted and Outcome

Documented a design gap in AMS: the protocol assumes agents can read files from the project directory, but a whole class of tools (ChatGPT, Claude.ai, Gemini web) can't. Created a DOC file and added a warning to the README.

Also created `AGENTS.md` (the OpenAI pointer file) which was missing.

## What Was Done

### `DOC/agent-capabilities.md` — new file
Categorizes AI tools into two groups:
- **Code-native tools** (Claude Code, Codex CLI, Cursor, GitHub Copilot, Gemini CLI) — run in the terminal, read instruction files automatically, can follow the full protocol
- **Chat-based tools** (ChatGPT, Claude.ai, Gemini web) — no filesystem access, human must bridge the context gap

The key insight: when a human uses a chat tool for part of the work, the handoff protocol breaks *silently*. The tool doesn't know it should read handoffs or write one. The human becomes responsible for continuity.

### `README.md` — added warning
Added a note in the Getting Started section alerting users to the chat-tool gap. Framed positively — these tools are valuable, but the human carries the protocol responsibility.

### `AGENTS.md` — new file
One-liner pointing to AGENT.md. Codex CLI reads this on startup. (Was missing; CLAUDE.md already existed.)

## What Worked, What Didn't

- The two-layer confusion surfaced naturally: AMS is both a project that *uses* the handoff protocol and a project *about* the handoff protocol. This came up because ChatGPT was working on the project without following the protocol — it couldn't.
- Naming: `AGENT.md` (singular) is the canonical protocol; `AGENTS.md`, `CLAUDE.md`, `.cursorrules` etc. are tool-specific pointer files. This is correct and intentional — one protocol, many shims.

## Current State

- No blockers
- `DOC/agent-capabilities.md` exists and covers the gap
- README has the warning
- All pointer files now exist: `CLAUDE.md`, `AGENTS.md`
- Still missing: `.cursorrules`, `.github/copilot-instructions.md`, `.gemini/styleguide.md` (can create when those tools are actually in use)

## Open Questions

- Should AMS provide a "chat tool starter kit" — a single markdown file a human can paste into ChatGPT/Claude.ai that gives enough context to approximate protocol compliance?
- Should handoffs written *by a human on behalf of a chat tool* use a different author convention? e.g. `handoff-2026-04-26-avatars-chatgpt-via-luke.md`

## Files Created or Modified

- `DOC/agent-capabilities.md` — new (agent filesystem access categories)
- `README.md` — added chat-tool warning in Getting Started
- `AGENTS.md` — new (OpenAI pointer to AGENT.md)
- `HANDOFF/handoff-2026-04-26-agent-capabilities-doc-and-chatgpt-gap-claude.md` — this file

## References

- `AGENT.md` — the protocol itself (v2.0)
- `DOC/agent-capabilities.md` — the new doc written this session
- Prior handoff: `HANDOFF/handoff-2026-04-26-kanban-wizard-and-event-log-design-claude-opus-4-7.md`

---

## Note for ChatGPT

If you're reading this, a human pasted it to you — which is itself an example of the problem this session addressed. You don't have access to this project's filesystem, so here's what you need to know:

**This project is AMS (Agent Management System)** — a framework for managing AI agents using Scrum-like practices. The repo lives at `/Users/lukemccormick/Sites/AMS/`. Key structure:

| Path | What it is |
|---|---|
| `AGENT.md` | The handoff protocol — instructions every agent follows |
| `HANDOFF/` | Session journals, one per work session, chronological |
| `DOC/` | Persistent reference docs by topic |
| `Personas.md` | AI team roles (Architect, Coder, Designer, etc.) |
| `INTERFACE/` | HTML prototypes (daily scrum board, office, floor plan) |
| `PERSONAS/` | Avatar images — **you created these** |
| `agent-handoff/` | Standalone handoff protocol repo (submodule) |
| `agent-scrum/` | Scrum layer repo (submodule) |
| `EXAMPLES/` | Past projects that informed AMS design |
| `OUTGOING/` | Material for the Stanford WebCamp talk (2026-04-30) |

**What happened this session:** We documented the fact that chat-based tools like you can't follow the protocol automatically because you can't read files from the repo. The human has to bridge the gap by pasting context to you and writing handoffs on your behalf. This isn't a criticism — you're great at things code-native tools aren't (you made the avatars). It's just a gap the protocol needs to acknowledge.

**If you're about to do work on this project**, ask Luke to paste you the most recent handoff or two from `HANDOFF/` and any relevant `DOC/` files. When you're done, ask him to save a handoff for you — or draft one and ask him to save it as `HANDOFF/handoff-[date]-[task]-chatgpt.md`.
