# Handoff: Kanban wizard + event log design

**Date:** 2026-04-26
**Author:** claude-opus-4-7
**Status:** ✅ Design phase complete; implementation pending

## What Was Attempted and Outcome

Continued the agent-scrum work into the UX layer. Two design docs produced; no implementation yet. User has a live wizard practice run scheduled today and wants to retrofit theme_machine before the Stanford talk Thursday (2026-04-30).

## What Was Done

### Wizard UX design (`DOC/kanban-wizard-plan.md`)

Settled on a six-phase architect-led wizard that turns "we're building X" into a populated Kanban board:

1. **Project framing** — goal, audience, constraints
2. **Team selection** — pick personas from `Personas.md` (Architect + PO always; Coder/Librarian default; QA/Designer/Researcher/Professor by project shape)
3. **Epic decomposition** — full epic list upfront (goal-driven projects know their epics)
4. **Sprint plotting** — per-sprint goals + demo checkpoints
5. **Story decomposition** — every story for every sprint, all the way to the goal
6. **Review + commit** — write CSV + markdown filesystem

Key decisions made along the way:
- **Plan all stories to the goal** (not just sprint 1) — user pushed back on my initial conservatism, citing theme_machine precedent. Demo+acceptance gates between sprints catch drift in practice.
- **Confidence flag on stories** — `confirmed` (sprint 1, user signed off) vs `planned` (later sprints, written in good faith). Same specificity, honest about confidence.
- **Persona vs model** — persona is the stable role (Cody the Coder); model is the per-story dial. Both shown on the Kanban card. Editing the model on a card writes back to story front-matter.
- **Card design** — title, persona, model, points, epic, confidence flag.
- **MVP build order** — prose wizard first → CSV export → static board → editable board → slash commands. Each step independently useful.

### Retrofit-from-handoffs idea

User proposed: install agent-scrum into existing handoff projects, have the architect reverse-engineer stories from handoffs to populate a Kanban board. Analyzed feasibility:

- **Theme Machine** — ✅ works well; it actually used the state-prefix protocol, so `git log` on `.handoff/WORK/EPICS/` *is* the event log. 40+ handoffs name the work.
- **ddev TUI projects** — ⚠️ partial; sprints exist but stories are markdown sections, not files with state transitions
- **backdrop-* / wp2bd** — ❌ no scrum; would require fabricating story boundaries

**Decision:** Scope the retrofit to theme_machine only — for the Stanford talk demo. Don't promise general retrofit capability.

### Event log schema (`DOC/event-log-schema.md`)

User chose to start with the event log before any wizard implementation. Schema produced:

- **Append-only CSV** — corrections via new events, never edits
- **Single source of truth** for live boards, wizards, and retrofits — `source` column distinguishes them
- **State transitions are `story_updated` events with `field=state`** — not a separate event_type, keeps the enum small, makes per-story history a single filter
- **Columns:** event_id, timestamp, event_type, story_id, epic_id, sprint, field, from_value, to_value, actor, source, note
- **Retrofit rules** — explicit walk of `git log --diff-filter=AMRD --name-status` on EPICS dirs, including handling theme_machine's habit of skipping `1-in-progress` when a story completed in one session

## What Worked Well

- The "be too conservative → user pushes back → land on better answer" rhythm worked. Confidence flag on stories is a better solution than skipping later-sprint detail.
- Scoping the retrofit to theme_machine specifically (rather than promising it for any handoff project) keeps the talk demo honest and the build feasible.
- Event log before wizard implementation is the right ordering — it's the data substrate everything else writes to.

## Current State

- Two design docs in `DOC/` (gitignored, working docs)
- No code written yet
- agent-scrum v1.0 unchanged — these docs describe a UX layer on top
- User about to run a live wizard practice session (today)

## Open Questions

Carried forward from the design docs (don't repeat them all here):

- **Where does the wizard live?** Skill in agent-scrum? Standalone HTML+JS? Ties into `INTERFACE/daily-scrum.html`?
- **Where does `events.csv` physically live?** Project root, `.scrum/`, or inside `agent-scrum/`? Leaning `.scrum/events.csv`.
- **Single-field events vs batched edits?** Probably single-field; group at query time.
- **What does the live practice run reveal?** Real signal on whether the question script flows naturally — will inform the next session's revisions.

## Files Created or Modified

- `DOC/kanban-wizard-plan.md` — created (wizard flow, data model, MVP plan)
- `DOC/event-log-schema.md` — created (event log CSV schema, retrofit rules)
- `HANDOFF/handoff-2026-04-26-kanban-wizard-and-event-log-design-claude-opus-4-7.md` — this file

## References

- `HANDOFF/handoff-2026-04-25-agent-scrum-published-and-ams-readme-claude-opus-4-7.md` — prior handoff (publish day)
- `HANDOFF/handoff-2026-04-25-agent-scrum-extraction-claude-opus-4-7.md` — agent-scrum extraction
- `HANDOFF/handoff-2026-04-22-scrum-evidence-research-claude-sonnet-4-6.md` — original research
- `Personas.md` — team roster the wizard pulls from
- https://github.com/cellear/agent-scrum — published protocol
- Stanford WebCamp Thursday 2026-04-30 — the deadline driving the retrofit
