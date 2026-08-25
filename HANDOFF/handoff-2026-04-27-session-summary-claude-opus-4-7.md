# Handoff: Multi-day session summary — agent-scrum, wizard, kanban board

**Date:** 2026-04-27
**Author:** claude-opus-4-7
**Status:** ⏳ In progress — Stanford talk Thu 2026-04-30

---

## Table of Contents — what's been done across this thread

1. **agent-scrum extracted and published** as a standalone repo at https://github.com/cellear/agent-scrum (sibling to agent-handoff). MIT-licensed, has README + AGENT.md + LICENSE + template/ skeleton. Linked into AMS as a git submodule.
2. **Pre-publish PII/sensitive-content scan** of the three referenced source projects (theme_machine, ddev-drush-tui, ddev-xdebug-tui). All clear.
3. **AMS README updated** with the scrum-personas image, moved from gitignored `PERSONAS/source/` into a tracked `images/` directory.
4. **Repo housekeeping** — `.gitignore` updated for `OUTGOING/`; agent-scrum converted from inline dir to submodule.
5. **Wizard designed** (`agent-scrum/wizard.md`, ~210 lines) — six-phase architect-led question script for bootstrapping new projects from a one-sentence goal. Committed and pushed; submodule pointer bumped in AMS.
6. **Event log schema designed** (`DOC/event-log-schema.md`) — append-only CSV capturing every state transition / field change for burndown, cycle time, and board-as-a-movie playback.
7. **Wizard test-driven** on `AMS-TEST/ams-test1/drupal-module-refactor/` — full architect run from Phase 1 (project framing) through partial Phase 5 (sprint 1 + sprint 2 stories drafted, sprints 3–4 not yet). Currently paused awaiting interim handoff request.
8. **Wizard improvements list** (`DOC/wizard-improvements.md`) — 11 items from the live run, prioritized.
9. **Static kanban board built** (`board/board.html` + `board/stories.csv`) — dark-themed dashboard with sprint/persona filters, epic color coding, persona/model/points on every card. CSV hand-extracted from wizard transcript; embedded as `file://` fallback so it opens with `open board.html`.

---

## Key locations

### In the AMS repo (this directory)

- `agent-scrum/` — submodule pointing at https://github.com/cellear/agent-scrum (commit e46b0d7)
- `board/board.html`, `board/stories.csv` — static kanban board demo
- `images/scrum-personas.jpeg` — banner image used in README
- `INTERFACE/daily-scrum.html` — existing polished standup-scene demo (NOT yet wired to stories.csv; that's an open question)
- `DOC/scrum-evidence.md` — original research on which projects evidenced Scrum (motivated agent-scrum extraction)
- `DOC/kanban-wizard-plan.md` — design doc for the wizard
- `DOC/event-log-schema.md` — event log column spec + retrofit rules for theme_machine
- `DOC/wizard-improvements.md` — 11-item improvement list from the live run
- `HANDOFF/` — session journals (this file is the latest)
- `Personas.md` — durable team roster (Architect, Priya, Cody, Lila, Eric, etc.)

### In the test project

- `AMS-TEST/ams-test1/drupal-module-refactor/AMS/` — fresh agent-scrum workspace
  - `Personas.md`, `agent-scrum/` (submodule), `CLAUDE.md`
  - HANDOFF/, DOC/, SPRINTS/, EPICS/, LEARNINGS/ — **none of these have been written yet** (Phase 6 has not run)
- The wizard run is captured in `/Users/lukemccormick/Sites/AMS/AMS-TEST/ams-testing-transcript-log.md`

---

## State of the live wizard run (at pause point)

**Project goal (Phase 1, locked):** A contrib module that reorganizes and curates Drupal's Extend page so site builders can manage already-installed modules without being overwhelmed by hundreds of irrelevant rows. Final UI uses icons and visual cues, not just text. Drupal 11.3, contrib (not core), local modules only.

**Team (Phase 2, locked):** Architect (Opus), Priya (PO, Sonnet), Cody (Coder, Sonnet), Derek (Designer, Sonnet), Ulla (UX — newly named, Sonnet), Lila (Librarian, Haiku), Quinn (QA, Sonnet).

**Epics (Phase 3, locked, all dated 260427):** taxonomy, ux, mockup, architecture, implementation, qa.

**Sprints (Phase 4, locked, 4 sprints):**
1. Decide what we're building (taxonomy + ux + mockup)
2. Decide how we're building it (architecture only)
3. Build it (implementation, partial qa)
4. Make sure it doesn't break Drupal (full qa, bugfix)

**Stories (Phase 5):**
- Sprint 1: 13 stories drafted, **revised** per user (Drupal CMS module list instead of just core; deprioritize spec simplified to per-user via user data API; override mechanism = config-sync YAML merge). 11 Sonnet, 2 Haiku, 34 points. Marked confirmed.
- Sprint 2: 5 stories drafted (4 Opus ADRs + 1 Sonnet skeleton story). 15 points. Marked planned.
- Sprints 3 & 4: not yet drafted. (For the kanban demo I drafted plausible stories in `board/stories.csv` so the visualization has data — the architect has not officially produced these.)

**User preferences captured during run:**
- Remind to scrutinize each epic-definition at the start of each epic
- Deliver stories sprint-by-sprint, not all at once
- End each sprint's story block with a next-sprint preview

**Phase 6 (commit + write files):** has NOT run. The architect needs to write the markdown filesystem to `drupal-module-refactor/AMS/`.

---

## Open work items, ordered by Stanford-talk relevance

1. **Decide on demo data state.** The kanban board currently shows everything in Backlog (accurate but visually flat for a demo). Options: (a) mock 2–3 stories into in-progress + 1–2 finished for the demo, or (b) actually do a tiny slice of Sprint 1 work (e.g., the two 1-pointers `tax-deprioritize-spec` and `tax-override-spec`) so the board reflects real progress.
2. **Daily-scrum view wiring.** `INTERFACE/daily-scrum.html` is polished demo material with its own visual identity. Decision pending: adapt a sibling `daily-scrum-live.html` to read the same `stories.csv`, or leave it static for the talk. Gated by item 1 — the standup view needs in-progress data to feel alive.
3. **Theme Machine retrofit.** The event-log schema's retrofit-rules section spells out how to derive events from `git log --diff-filter=AMRD --name-status` on `EXAMPLES/theme_machine/.handoff/WORK/EPICS/`. Not built yet. Goal: a CSV of theme_machine's actual story-state transitions, rendered through the same `board.html` to show "this is what a real project's history looks like." Strong demo candidate.
4. **Markdown→CSV pipeline.** Once Phase 6 actually runs in the test project, we'll want a script that reads the EPICS/*/0-backlog-*.md filenames and front-matter and emits stories.csv. This is what makes the board real (vs. hand-extracted).
5. **Wizard improvements (11 items in DOC/wizard-improvements.md).** Most important: per-phase HANDOFF checkpoints. None are talk-blockers.
6. **Daily-scrum adaptation, if pursued** — would create `INTERFACE/daily-scrum-live.html` keeping the original intact.

---

## Key decisions baked in (so a future agent doesn't re-litigate them)

- **agent-scrum is convention + protocol, not tooling.** No CLI, no parser. The wizard is just a prompt.
- **Push is the user's manual gate.** I commit; user pushes. Saved as memory at `~/.claude/projects/-Users-lukemccormick-Sites-AMS/memory/feedback_pushes.md`.
- **Agent-scrum lives as a sibling to agent-handoff**, not nested or merged. It's a layer on top of the handoff protocol.
- **State is encoded in story filename prefixes** (`0-backlog-`, `1-in-progress-`, `2-finished-`) and mirrored in front-matter — filename is source of truth for state.
- **Personas are durable; per-story model assignment is the dial.** Card always shows both.
- **Confidence flag** (`confirmed` vs `planned`) replaces the earlier "tentative sprint" idea — applied per-story instead of per-sprint.
- **Plan all stories to the end during the wizard** (per user — theme_machine is the existence proof). Don't be conservative about late-sprint specificity; just mark them `planned`.
- **Theme Machine retrofit only.** The other EXAMPLES projects don't have clean enough state-transition data; don't promise general retrofit.

---

## Files created or modified (across all related sessions)

### In agent-scrum repo (separate Git history at github.com/cellear/agent-scrum)
- README.md, AGENT.md, LICENSE, .gitignore — initial publish
- wizard.md — added in commit e46b0d7
- template/SPRINTS/sprint-1.md, template/EPICS/example-epic/{epic-definition.md, 0-backlog-example-story.md}, template/LEARNINGS/sprint-1.md

### In AMS repo
- .gitmodules, .gitignore, README.md, agent-scrum (submodule pointer)
- images/scrum-personas.jpeg
- DOC/scrum-evidence.md, DOC/kanban-wizard-plan.md, DOC/event-log-schema.md, DOC/wizard-improvements.md
- board/board.html, board/stories.csv
- HANDOFF/handoff-2026-04-22-*, handoff-2026-04-25-*, handoff-2026-04-26-*, this file

### Local agent memory
- ~/.claude/projects/-Users-lukemccormick-Sites-AMS/memory/MEMORY.md
- ~/.claude/projects/-Users-lukemccormick-Sites-AMS/memory/feedback_pushes.md

---

## How to recover from compaction

If context is lost mid-session, point a fresh agent at this file. Specifically:

1. Read this handoff
2. Read `DOC/wizard-improvements.md` for the next-iteration changes to wizard.md
3. Read `DOC/event-log-schema.md` if working on the retrofit
4. Look at `board/stories.csv` to see the test project's current planned story set
5. Ask the user where they want to pick up

---

*Last updated: 2026-04-27 by claude-opus-4-7*
