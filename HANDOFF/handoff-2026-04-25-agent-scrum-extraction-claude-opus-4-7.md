# Handoff: Extract agent-scrum protocol

**Date:** 2026-04-25
**Author:** claude-opus-4-7
**Status:** ✅ Complete

## What Was Attempted and Outcome

The user asked for a new sibling project that extracts the Scrum/Kanban patterns evidenced across `EXAMPLES/theme_machine`, `EXAMPLES/ddev-xdebug-tui`, and `EXAMPLES/ddev-drush-tui`. The prior session's research (`DOC/scrum-evidence.md`) had already inventoried these patterns and identified theme_machine's `DOC/handoff-epics-protocol.md` as the most evolved source — its own footer noted it "may be extracted as a standalone project if it proves useful." This session is that extraction.

Created `agent-scrum/` as a top-level sibling to `agent-handoff/`. The user clarified that `agent-project-tracker/` is **not** a generic project tool — it's a private tool for managing projects that already use the handoff protocol — so it was the wrong home.

## What Was Done

Created `agent-scrum/`:

- **`AGENT.md`** — generalized protocol covering Sprints, Epics with state-prefixed Kanban filenames (`0-backlog-`, `1-in-progress-`, `2-finished-`), and Learnings (retros). Distilled from theme_machine's `handoff-epics-protocol.md` plus the simpler `SPRINTS/`+`LEARNINGS/` shape from the ddev TUI projects.
- **`README.md`** — pitch, layout diagram, "pick the parts you need" guidance distinguishing sprint-only / epics-only / both modes.
- **`DOC/evidence.md`** — citations of the three source projects, what converged, what this protocol is *not* (not a tool; not Spec Kitty/pipeline-driven planning).
- **`template/`** — placeholder skeleton: `SPRINTS/sprint-1.md`, `EPICS/example-epic/epic-definition.md`, `EPICS/example-epic/0-backlog-example-story.md`, `LEARNINGS/sprint-1.md`.

Design choice: layered on top of `agent-handoff`, not bundled with it. Handoffs capture *what happened in this session*; scrum captures *what work exists, what state it's in, what we learned*. They reference each other but ship independently.

## What Worked Well

- The prior session's `DOC/scrum-evidence.md` did most of the analytical work — this session was largely synthesis + writing.
- theme_machine's `handoff-epics-protocol.md` was already nearly publishable; the main lift was generalizing the directory paths (`.handoff/WORK/EPICS/` → `EPICS/`) and adding the Sprints + Learnings dimensions from the ddev projects.
- The state-prefixed filename pattern (`0-backlog-`, `1-in-progress-`, `2-finished-`) is the strongest contribution beyond textbook Scrum and translated cleanly into the new project.

## Current State

- `agent-scrum/` exists and is self-contained.
- Not yet a git repo of its own; lives inside the AMS working tree alongside `agent-handoff/` and the other top-level pieces.
- `AGENT.md` references a future GitHub URL (`https://github.com/cellear/agent-scrum`) that doesn't exist yet — purely aspirational, mirrors how `agent-handoff/AGENT.md` does it.
- No tests, no tooling, no CLI. Intentional — this is a convention, like agent-handoff.

## Open Questions

1. Should `agent-scrum/` get its own GitHub repo (mirroring `agent-handoff`), or stay nested in AMS for now?
2. Should `Overview.md` and `Tooling.md` at the AMS root be updated to mention `agent-scrum`?
3. Should the protocol mention the `_workspace/` convention seen in theme_machine, or is that project-specific noise?
4. Worth adding a one-liner skill or slash command to scaffold a new epic dir + initial backlog stories? (Out of scope for this session.)

## Files Created or Modified

- `agent-scrum/README.md` — created
- `agent-scrum/AGENT.md` — created
- `agent-scrum/DOC/evidence.md` — created
- `agent-scrum/template/SPRINTS/sprint-1.md` — created
- `agent-scrum/template/EPICS/example-epic/epic-definition.md` — created
- `agent-scrum/template/EPICS/example-epic/0-backlog-example-story.md` — created
- `agent-scrum/template/LEARNINGS/sprint-1.md` — created
- `HANDOFF/handoff-2026-04-25-agent-scrum-extraction-claude-opus-4-7.md` — this file

## References

- `DOC/scrum-evidence.md` — inventory that motivated this extraction
- `HANDOFF/handoff-2026-04-22-scrum-evidence-research-claude-sonnet-4-6.md` — prior session that produced the evidence doc
- `EXAMPLES/theme_machine/DOC/handoff-epics-protocol.md` — primary source for the epic/story pattern
- `EXAMPLES/ddev-drush-tui/SPRINTS/sprint-1.md` — exemplar of the Sprint file format
- `EXAMPLES/ddev-drush-tui/LEARNINGS/sprint-1.md` — exemplar of the Learnings format
- `agent-handoff/AGENT.md` — sibling protocol this one layers on
