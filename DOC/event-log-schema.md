# Event Log Schema

A timestamped log of every change to a Kanban board. Enables burndown charts, cycle-time metrics, and board-as-a-movie playback. Same schema serves both new projects (board emits events live) and retrofits (events derived from git history).

---

## File

`events.csv` — lives next to the project's `stories.csv` (location TBD; probably project root or `.scrum/`).

One row per event. Append-only — events are never edited or deleted; corrections happen by emitting a new event.

---

## Columns

| Column | Type | Required | Description |
|---|---|---|---|
| `event_id` | string | yes | Unique ID (uuid or `evt-{n}` counter) |
| `timestamp` | ISO 8601 | yes | `2026-04-26T14:32:00-07:00` |
| `event_type` | enum | yes | See below |
| `story_id` | string | conditional | Required for story events; empty for project/sprint events |
| `epic_id` | string | conditional | Required for epic + story events |
| `sprint` | int | conditional | Sprint number, when applicable |
| `from_value` | string | conditional | Previous value (for change events); empty for create |
| `to_value` | string | conditional | New value (for change/create events); empty for delete |
| `field` | string | conditional | Which field changed (for `update` events): `state`, `persona`, `model`, `points`, `confidence`, `acceptance_criteria`, etc. |
| `actor` | string | yes | Who made the change — persona name, model id, or human handle |
| `source` | enum | yes | How the event was captured: `wizard`, `board`, `manual`, `git-retrofit` |
| `note` | string | no | Free-text context (e.g. handoff filename, commit sha, "blocked by upstream API change") |

### `event_type` values

| Value | Meaning |
|---|---|
| `project_created` | Wizard finished; initial state captured |
| `epic_created` | New epic added to the project |
| `epic_updated` | Epic field changed (goal, DoD, etc.) |
| `epic_deleted` | Epic removed from the plan |
| `sprint_created` | Sprint added |
| `sprint_started` | Sprint kicked off |
| `sprint_accepted` | Sprint demo passed; sprint closed |
| `story_created` | Story added |
| `story_updated` | Story field changed (use `field` column to identify which) |
| `story_deleted` | Story removed |

State transitions (`backlog → in-progress`, etc.) are `story_updated` events with `field=state`. Keeping them as one event_type avoids combinatorial explosion in the enum and makes "show me everything that changed about this story" a single filter.

---

## Examples

### Story moves from backlog to in-progress

```csv
event_id,timestamp,event_type,story_id,epic_id,sprint,field,from_value,to_value,actor,source,note
evt-0042,2026-03-19T09:14:00-07:00,story_updated,catalog-schema,epic0228-theme-catalog,2,state,0-backlog,1-in-progress,Cody,board,
```

### Wizard creates a story

```csv
evt-0007,2026-04-26T11:02:00-07:00,story_created,api-discovery,epic0228-theme-catalog,1,,,0-backlog,Architect,wizard,initial plan
```

### Persona reassigned mid-sprint

```csv
evt-0061,2026-03-22T16:40:00-07:00,story_updated,build-scraper,epic0228-theme-catalog,2,persona,Cody,Quinn,human,board,Cody hit a wall on the JSON parsing
```

### Retrofitted from theme_machine git history

```csv
evt-r-019,2026-02-28T13:55:00-08:00,story_updated,api-discovery,epic0228-theme-catalog,,state,0-backlog,2-finished,cursor,git-retrofit,commit:7a3c9d1
```

(Note `from_value=0-backlog, to_value=2-finished` — theme_machine sometimes skipped `1-in-progress` because work happened entirely in one session. The retrofit honors that rather than fabricating a missing transition.)

---

## Derived data

The CSV is the source of truth; useful aggregates are computed views:

- **Burndown** — count of stories by state per day, plotted over time
- **Cycle time** — for each finished story, time between first `1-in-progress` and `2-finished` events
- **Board-at-T** — replay events in order until the chosen timestamp; the resulting state is the board snapshot
- **Actor activity** — events grouped by `actor` per day (who's working on what, when)

---

## Retrofit rules (theme_machine)

For the talk demo, events come from git history. The script:

1. `git log --follow --diff-filter=AMRD --name-status -- '.handoff/WORK/EPICS/**'` for each story file
2. **Add** (`A`) of `0-backlog-foo.md` → `story_created` event, `to_value=0-backlog`
3. **Rename** (`R`) of `{prefix1}-{name}-foo.md` → `{prefix2}-{name}-foo.md` → `story_updated` event with `field=state`, `from_value={prefix1}-{name}`, `to_value={prefix2}-{name}`
4. **Modify** (`M`) of an existing story → `story_updated` event with `field=body` (or skip — content edits aren't board-visible)
5. **Delete** (`D`) → `story_deleted`
6. `actor` = commit author username (or `Co-Authored-By` trailer when present and the human is the committer)
7. `source` = `git-retrofit` for all events
8. `note` = `commit:{short-sha}`

Sprint events are harder — they aren't encoded in the EPICS dir. Derive `sprint_started`/`sprint_accepted` from `_workspace/SPRINTS/sprint-N.md` content (look for the `## Acceptance` block date) as a separate pass.

---

## Open questions

1. **One event per field-change, or batch fields edited in one save?** Single-field events are simpler to query; batched events better reflect human intent ("I edited persona AND model in one card-edit"). Probably single-field — group by timestamp+actor at query time.
2. **Where does the file live?** `events.csv` at project root (visible), `.scrum/events.csv` (hidden), or inside an `agent-scrum/` subdirectory? Likely `.scrum/events.csv` so the protocol pieces cluster.
3. **Schema versioning** — add a `schema_version` column or track in a sibling `events.meta.yaml`? Probably the latter; cleaner.
4. **Confidence transitions** — when a story moves from `planned` → `confirmed` at sprint start, that's a `story_updated` with `field=confidence`. Worth flagging because it's a *kind* of state transition the board may want to render differently.

---

*Last updated: 2026-04-26 by claude-opus-4-7*
