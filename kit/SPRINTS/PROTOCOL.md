# SPRINTS — Protocol

Follow this file only when `CONFIG.md` lists `SPRINTS` in `components`.

Sprints organize the work: what is planned, in flight, done, and accepted. Handoffs remain the session journals; they should reference the sprint and story they touched.

Sprint planning (breaking a goal into epics, sprints, and stories) is a **separate** command: read `agent-scrum/wizard.md`. Do not run it as part of `INSTALL.md`.

---

## Sprints

**File:** `{sprints_dir}/sprint-{n}.md`

Each sprint file should include:

- **Sprint Goal** — One-sentence outcome that proves the sprint succeeded
- **Stories** — List of story sections (`### S{n}-{m} · {title} · [ ]`), each with owner, scope, acceptance criteria
- **Demo checkpoint** — What a human runs/sees to accept the sprint
- **Acceptance** — Status, date, reviewer (filled in when the sprint is accepted)
- **Decisions made this sprint** — Optional; conventions or tradeoffs that bind future work
- **Deferred to later sprints** — Optional; what was pulled out of scope

Stories use checkbox status in their heading (`[ ]` → `[x]`). Acceptance criteria are checklists.

A starter file lives in `agent-scrum/template/SPRINTS/sprint-1.md`.

---

## Epics (optional)

Use epics only when `CONFIG.md` has `epics` set to `on`. Default is off. Field projects often run sprints without epics.

**Directory:** `{epics_dir}/epic{YYMMDD}-{slug}/` — e.g. `epic0228-theme-catalog`

Each epic directory contains:

- `epic-definition.md` — Goal, scope, approach, risks, definition of done, **backlog order**
- One file per story, named with a **state prefix**

### Story state prefix

| Prefix | State | Meaning |
|---|---|---|
| `0-` | backlog | Not started |
| `1-` | in-progress | Currently being worked on |
| `2-` | finished | Done |
| (extend as needed) | blocked, cancelled | |

**Format:** `{prefix}-{state-name}-{task-slug}.md`

Examples: `0-backlog-api-discovery.md`, `1-in-progress-build-scraper.md`, `2-finished-catalog-schema.md`

**Renaming a file changes its state.** The prefix drives sort order in `ls`. No metadata file to keep in sync.

Story filenames do **not** imply priority. Maintain a `## Backlog order` section in `epic-definition.md` listing story IDs in priority order.

Each story file should include: Story ID, Epic, Status, optional Points, Description, Tasks, Acceptance criteria, optional Dependencies and References.

---

## Workflow

1. **Create sprint** — `sprint-{n}.md` with goal and stories
2. **(Optional)** Create an epic for work that crosses sprint boundaries
3. **Add stories** — as sections in the sprint file; or as `0-backlog-*.md` in the epic dir when epics are on
4. **Pick up work** — if using epics, rename `0-backlog-*` → `1-in-progress-*`
5. **Complete** — rename to `2-finished-*` if using epics; check the box in the sprint file
6. **Handoff** — session journals in HANDOFF reference the sprint and any stories touched
7. **Sprint demo + acceptance** — human reviews the demo checkpoint; fill in `## Acceptance`
8. **Retro** — if `LEARNINGS` is enabled, write `LEARNINGS/sprint-{n}.md`

---

## Relation to other components

- **HANDOFF** (required) — Session journals. Reference the active sprint/story.
- **DOC** (optional) — Persistent reference. Do not assume it exists.
- **LEARNINGS** (optional) — Retros live there when enabled; skip the retro file if LEARNINGS is off.
- **OFFICES** (optional) — Persona working memory. Sprint files remain authoritative over `desk.md`.

---

## Version History

Lifted from [agent-scrum](https://github.com/cellear/agent-scrum) into AMS as an optional component.

- **1.0** (2026-04-25) — Initial extraction from theme_machine, ddev-xdebug-tui, ddev-drush-tui
- **1.1** (2026-08-23) — Optional under AMS CONFIG; EPICS gated by `epics: on`; LEARNINGS/DOC no longer assumed
