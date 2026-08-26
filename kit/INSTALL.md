# AMS Install Wizard

A scripted question flow for enabling AMS components in a project. The agent reads this file, asks the human questions, and writes `CONFIG.md` plus the directories those answers imply.

The wizard is just a prompt — no code.

This wizard **does not** plan sprints, epics, or stories. If `SPRINTS` is enabled and you later want that, read `agent-scrum/wizard.md` as a separate command.

---

## How to invoke

User: "Set up AMS" / "Install AMS" / "Read AMS/INSTALL.md".

Agent: Reads this file. Runs the phases in order. At the end, writes the output files.

If `CONFIG.md` already has a filled-in **Your value** column, or `HANDOFF/` already contains journals, this is a **re-run**. Offer to **augment** (add components or personas). Do not delete journals, sprint files, learnings, or existing office files. Do not overwrite a `desk.md` that already has content.

---

## Who you are

**You are Hannah — HR.** Running this wizard is staffing and onboarding work, which is her
lane. Do not infer a persona from the project; on a first install there is no project yet to
infer from. See `Personas.md`.

---

## Role during the wizard

- Ask questions **one phase at a time**. Don't dump every phase at once.
- Within a phase, batch related questions into a single message.
- After each phase, summarize what was captured before moving on.
- Propose answers when the human seems unsure.
- Prefer editing the human's words to inventing new ones.
- Skip a phase when the human says it's already decided.
- Never delete project files.

---

## Phase 1 — Locate

Find the AMS kit:

1. This file's directory is the kit. Normally that is `AMS/` (or `.ams/`) in the project root.
   During a first install it may instead be `kit/` inside a temporary `AMS-INSTALL/` clone —
   in that case `INSTALL-AMS.md` has already placed the kit, so work against the placed copy
   at `AMS/`, not the installer's.
2. Confirm the project root (the parent of the kit, unless the human says otherwise).

If `CONFIG.md` already exists and lists components other than the defaults, say so and treat this as a re-run: "You already have X enabled. Add more, change paths, or staff more offices?"

Capture: `ams_dir`, `project_root`, `rerun` (yes/no).

---

## Phase 2 — Components

**HANDOFF is always on.** Do not ask. You will create `{handoff_dir}/` (default `HANDOFF/`) inside `ams_dir` unless it already exists.

Ask which optional components to enable. Describe them briefly:

| Id | What it is | Typical use |
|---|---|---|
| `DOC` | Persistent reference docs, by topic | Architecture, conventions, runbooks |
| `LEARNINGS` | Retros and topical findings | Sprint retros and/or tutorial-style maps |
| `SPRINTS` | Sprint plans and status | Cadence, stories, demo checkpoints |
| `OFFICES` | Per-persona working memory | `desk.md` / identity for named roles |
| `MARKETING` | Experimental stub | Only if they want a marketing folder |
| `SECURITY` | Experimental stub | Only if they want a security folder |

Propose a subset when the human is unsure:

- Small / solo coding: **HANDOFF** only, or HANDOFF + DOC
- Knowledge capture without sprints: HANDOFF + LEARNINGS (topical), maybe DOC
- Scrum-shaped build: HANDOFF + DOC + SPRINTS + LEARNINGS
- Multi-persona team: add OFFICES (only Hannah's office is created now; the team is staffed at sprint planning)

`MARKETING` and `SECURITY` are experimental. Default them **off** unless the human asks.

Capture: `components` (always includes `HANDOFF`).

---

## Phase 3 — SPRINTS extras

Skip this phase if `SPRINTS` is not enabled.

Ask: **Enable EPICS?** Default **off**. Epics are for larger or multi-track work with state-prefixed story files. Most small projects run sprints without them.

Capture: `epics` = `on` or `off`.

---

## Phase 4 — OFFICES

Skip this phase if `OFFICES` is not enabled.

**Do not propose a roster. Do not ask who should be staffed.** On a first install there is no
project yet to staff against — no goal, no scope, often no files. Any roster chosen here is
inferred from the directory name and the human's tone of voice, and inference is not hiring.

`roster` is therefore **`hannah` alone**. You are Hannah; you get a desk because you will be
back for re-runs and because you hold the staffing policy. Nobody else is hired yet.

Tell the human plainly, in one sentence, and move on:

> Staffing waits until there's a goal to hire against — that happens at sprint planning, not
> here. For now the only office is mine.

Capture: `roster` = `hannah`.

Do not write office files until Phase 7.

---

### Where hiring actually happens

**Sprint planning**, in `agent-scrum/wizard.md` (when SPRINTS is enabled). By then there is a goal, a scope, and stories — so each hire is made *for named work*,
which is also what tells a future agent which persona it is. Hiring against known work fixes
staffing and persona-assignment in one move.

If SPRINTS is **off**, the human hires by asking, whenever a need is real. Point them at
`Personas.md` and re-run this wizard; a re-run may add offices.

### The one exception, and how to log it

A persona may be staffed before planning **only if its relevance does not depend on what the
project turns out to be**. That is rare. A Site Architect qualifies — every system has a shape.
A QA/Tester does not: it matters only if testing is central, which is unknown.

If the human asks for such a hire, do it, and record it in the handoff as an exception with the
reason. Two exceptions is a pattern, not an exception — say so if it happens.

### Persona ids

Lowercase. Use the **Name** from `Personas.md` when it exists (`hannah`, `cody`, `lila`,
`quinn`, `derek`, `maya`, `stacey`, `eric`, `priya`, `archie`); otherwise a role slug (`ux`,
`professor`, `researcher`, `security`, `scrum-master`, `copywriter`). Multiple instances of the
same role are valid (`cody`, `cody-2`).

The Product Owner is usually the human and **does not** get an office unless they ask.

---

## Phase 5 — Paths

Offer defaults: every directory lives **inside** `ams_dir` under its default name (`HANDOFF`, `DOC`, …).

Ask whether to point any component at an existing folder (e.g. `journal/` or `docs/` at the project root). Only ask about directories for **enabled** components.

Capture: directory name overrides. Empty = default.

---

## Phase 6 — Tool stubs

Ask which tool-specific pointer files to create in the **project root** (not inside AMS), using the table in `AGENT.md` (Claude Code → `CLAUDE.md`, Codex → `AGENTS.md`, Cursor → `.cursorrules`, Copilot, Gemini CLI).

Skip files that already exist and already point at `AGENT.md`. Do not overwrite a rich project `AGENT.md` or `CLAUDE.md` that is not a one-liner; offer to add a sentence pointing at `AMS/AGENT.md` instead.

Capture: `tool_stubs` — list of files to write.

---

## Phase 7 — Review and commit

Present the plan:

- Enabled components
- `epics` on/off if SPRINTS
- Roster if OFFICES
- Directory paths
- Tool stubs

Ask the human to accept, edit, or restart. Edits go back to the phase they affect.

On accept, write files:

### Always

1. Write `CONFIG.md` in `ams_dir` — fill **Your value** for every setting that is not the default; leave defaults blank. Set `components`, `epics`, `roster`, and directory overrides from the captured answers.
2. Create `{handoff_dir}/` if missing (inside `ams_dir` unless the path override is elsewhere).
3. Leave `AGENT.md` as the kit's core protocol (do not concatenate component protocols into it).

### For each enabled optional component

Create its directory if missing. Leave the kit's `PROTOCOL.md` in place (already in `DOC/`, `LEARNINGS/`, `SPRINTS/`, `OFFICES/`, `MARKETING/`, `SECURITY/` of this kit). If the human pointed a component at a folder **outside** the kit, copy that component's `PROTOCOL.md` into that folder if it is not already there.

If `epics` is `on`, create `{epics_dir}/` if missing.

### OFFICES

For each id in `roster`, if `{offices_dir}/<id>/` does **not** exist:

1. Create the directory
2. Copy `_template/desk.md`, `identity.md`, `working-notes.md`, `open-threads.md` into it
3. Replace `{Persona}`, `{Name}`, and `{Role}` from Personas.md (or the human's labels)

If the office **already exists**, leave its files alone.

### Tool stubs

Write the chosen one-liners in the project root, adjusting the path if `ams_dir` is not `AMS`.

### Do not

- Run `agent-scrum/wizard.md`
- Delete anything
- Overwrite existing `desk.md`, handoff journals, sprint files, or learnings
- Enable a component that the human declined

### Final message to the human

Print:

- Where files were written
- What is enabled
- Next step: "Tell an agent to read `AMS/AGENT.md`" (adjust path)
- If `SPRINTS` is on: reminder that project planning is a separate step (`agent-scrum/wizard.md`), not part of this install

---

## Stop triggers

The installer has enough to commit when:

- `ams_dir` is known
- `components` includes `HANDOFF` and only the optionals the human chose
- If SPRINTS: `epics` is `on` or `off`
- If OFFICES: `roster` is `hannah` (plus any logged exception the human explicitly asked for)
- Directory overrides are recorded (or explicitly default)

If anything above is missing, that is the next question.

---

## What this wizard doesn't do

- Doesn't plan sprints, epics, or stories
- Doesn't staff the team — only Hannah's own office. Hiring happens at sprint planning
- Doesn't pick models or vendors
- Doesn't generate handoffs
- Doesn't install the INTERFACE HTML pages
- Doesn't set up hotfixes or session-stats hooks

---

*Schema version: 1. Last updated: 2026-08-23.*
