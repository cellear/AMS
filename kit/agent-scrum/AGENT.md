# Agent Scrum Protocol

A lightweight Scrum + Kanban layer for AI-assisted projects. Pairs with the agent handoff protocol — handoffs are chronological session journals; scrum tracks the work itself.

## Directories

Use whichever subset fits the project:

- `SPRINTS/` — One file per sprint: goal, stories, acceptance criteria, demo checkpoint
- `EPICS/` — One subdirectory per epic with state-prefixed story files
- `LEARNINGS/` — Per-sprint retros (one file per sprint, e.g. `sprint-1.md`)

Create these alongside `HANDOFF/` and `DOC/` (from the handoff protocol).

## Sprints

**File:** `SPRINTS/sprint-{n}.md`

Each sprint file should include:

- **Sprint Goal** — One-sentence outcome that proves the sprint succeeded
- **Stories** — List of story sections (`### S{n}-{m} · {title} · [ ]`), each with owner, scope, acceptance criteria
- **Demo checkpoint** — What a human runs/sees to accept the sprint
- **Acceptance** — Status, date, reviewer (filled in when the sprint is accepted)
- **Decisions made this sprint** — Optional; conventions or tradeoffs that bind future work
- **Deferred to later sprints** — Optional; what was pulled out of scope

Stories use checkbox status in their heading (`[ ]` → `[x]`). Acceptance criteria are checklists.

## Epics

**Directory:** `EPICS/epic{YYMMDD}-{slug}/` — e.g. `epic0228-theme-catalog`

Each epic directory contains:

- `epic-definition.md` — Goal, scope, approach, risks, definition of done, **backlog order**
- One file per story, named with a **state prefix** (see below)

### Story state prefix

| Prefix | State | Meaning |
|--------|-------|---------|
| `0-` | backlog | Not started |
| `1-` | in-progress | Currently being worked on |
| `2-` | finished | Done |
| (extend as needed) | blocked, cancelled | |

**Format:** `{prefix}-{state-name}-{task-slug}.md`

Examples:
- `0-backlog-api-discovery.md`
- `1-in-progress-build-scraper.md`
- `2-finished-catalog-schema.md`

**Renaming a file changes its state.** The prefix drives sort order in `ls`, making workflow visible at a glance. No metadata file to keep in sync.

### Story content

Each story file should include:

- **Story ID** — short slug (matches the filename suffix)
- **Epic** — parent epic ID
- **Status** — redundant with filename, useful inside the file
- **Points** — optional (small/medium/large or numeric)
- **Description** — what the story is
- **Tasks** — checklist of work items
- **Acceptance criteria** — definition of done
- **Dependencies** — optional; other stories that must complete first
- **References** — optional; DOC files, prior handoffs

### Backlog order

Story filenames do **not** imply priority. Maintain a `## Backlog order` section in `epic-definition.md` listing story IDs in priority order. The product owner edits this when reordering.

```markdown
## Backlog order
1. api-discovery
2. build-scraper
3. download-screenshots
4. validate-catalog
5. document-process
```

(Alternatives: numeric prefix in filename, or `Priority:` field in story content. The `epic-definition.md` list is recommended — easiest to reorder.)

## Learnings (Retros)

**File:** `LEARNINGS/sprint-{n}.md`

A bullet list of dated, durable findings from the sprint — gotchas, environment quirks, conventions that emerged. Phrased so a future agent or teammate can act on them without re-deriving.

```markdown
- 2026-03-19: On this machine, `ddev describe` uses `--json-output` instead of `--json`.
- 2026-03-19: Drush JSON output has an inconsistent `definition.arguments` field — handle with `json.RawMessage`.
```

Keep it short. If a learning becomes a load-bearing convention, promote it to `DOC/`.

## Workflow

1. **Create sprint** — `SPRINTS/sprint-{n}.md` with goal and stories
2. **(Optional) Create epic** — `EPICS/epic{YYMMDD}-{slug}/epic-definition.md` for cross-sprint work
3. **Add stories** — `0-backlog-*.md` in the epic dir; or as story sections in the sprint file
4. **Set backlog order** — update `Backlog order` in epic definition
5. **Pick up work** — rename `0-backlog-*` → `1-in-progress-*`
6. **Complete** — rename `1-in-progress-*` → `2-finished-*`; check the box in the sprint file
7. **Handoff** — session handoffs (`HANDOFF/handoff-*.md`) reference the sprint and any stories touched, and end with a **"Next session" block** (see below)
8. **Sprint demo + acceptance** — human reviews demo checkpoint; fill in `## Acceptance` block
9. **Retro** — write `LEARNINGS/sprint-{n}.md`

## The "Next session" handoff convention

Every per-story session handoff ends with a ready-to-paste prompt for the next session. This makes the chain self-propagating: each session hands the human a tight prompt and tells them which model/tool to run it in, so orchestration is always pre-chewed. The human's job between sessions is reduced to: close one terminal, open the named tool with the named model, paste.

This pattern emerged organically in earlier projects (e.g. ddev-drush-tui's sprint handoffs ended with "next-assistant prompt for Sprint N"). Codified here.

### Format

```markdown
## Next session

**Next story:** `{story_id}`
**Persona:** {persona-name} ({role})
**Model:** {suggested model from story front-matter}
**Tool:** {Claude Code | Codex CLI | Gemini CLI | Cursor Composer | …}

**Paste this prompt:**

> You are {Persona}, the {role} on this project. Read `DOC/project-preferences.md` for standing user preferences, then read `EPICS/{epic_id}/epic-definition.md` and scrutinize it (per the user's standing epic-kickoff preference, if this is the first story of the epic). Then do this story: `EPICS/{epic_id}/0-backlog-{story_id}.md`. When done:
>
> 1. Rename the story file from `0-backlog-{story_id}.md` to `2-finished-{story_id}.md`
> 2. Append events to `.scrum/events.csv`: state→in-progress at session start, state→finished at session end. Use `actor: {Persona}`, `source: manual`.
> 3. Write a session handoff to `HANDOFF/` that ends with a "Next session" block following this same template, naming the next story per the sprint's backlog order.
```

### What "Next session" points to, by context

| Context | Next session block points to |
|---|---|
| First story of project | (Emitted by the wizard at Phase 6) |
| Mid-sprint story handoff | Next story in the sprint's backlog order |
| Last story of a sprint | Sprint demo + acceptance review |
| Last story of the final sprint | Closing project handoff |
| Project complete | Block reads: "Project complete; no next session." |

### Picking the model and tool

The story's front-matter `model` field is the suggestion. The agent writing the handoff translates that into a concrete tool/model combination:

- `claude-opus-4-7`, `claude-sonnet-4-6`, `claude-haiku-4-5` → Claude Code, with `/model {name}` mid-session or `claude --model {name}` to start fresh
- `gpt-5`, `o-{n}` → Codex CLI (model native to the tool)
- `gemini-2.5`, etc. → Gemini CLI
- `cursor-*` → Cursor Composer

If the story's `model` doesn't fit the team's available tools, the agent picks the closest plausible substitute and notes it in the next-session block.

## Relation to other protocols

- **Handoff (`HANDOFF/`)** — Session journals, chronological. Reference the active sprint/epic/story.
- **DOC (`DOC/`)** — Persistent reference docs. Promote durable conventions out of `LEARNINGS/` when they stabilize.
- **Scrum (this protocol)** — Work organization: what's planned, in flight, done, learned.

## Version History

Source: https://github.com/cellear/agent-scrum

- **1.1** (2026-04-27) — Added the "Next session" handoff convention (chained per-story prompts with model/tool). Codifies the pattern from ddev-drush-tui sprint handoffs.
- **1.0** (2026-04-25) — Initial extraction from theme_machine, ddev-xdebug-tui, ddev-drush-tui
