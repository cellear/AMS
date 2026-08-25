# Kanban Wizard: Onboarding Flow + Data Model

A plan for how a user starts a new project in agent-scrum: an architect-led wizard turns "we're building X" into a fully-populated Kanban board with epics, sprints, stories, and persona/model assignments.

---

## What this is

`agent-scrum/` (the published protocol) defines the *filesystem convention* — `SPRINTS/`, `EPICS/`, state-prefixed story files. This doc plans the *UX layer on top*: how a project gets bootstrapped from a one-sentence goal to a populated board.

The architect persona (Opus avatar) runs the wizard. The output is two things:
1. A CSV file the Kanban board UI reads
2. The corresponding markdown files in the agent-scrum directory layout

The CSV and the markdown stay in sync — either is the source of truth depending on context.

---

## Wizard flow

The architect asks questions until they have enough to write the plan, then writes it.

### Phase 1 — Project framing

1. **What are we building?** (one-sentence goal)
2. **Who's it for / what does success look like?** (helps shape epics that aren't just feature dumps)
3. **Any hard constraints?** (deadline, platform, stack, budget, must-use-X)

### Phase 2 — Team selection

Architect proposes a roster from `Personas.md`. User accepts/edits.

- Always include: **Product Owner** (Priya), **Architect** (Opus avatar — that's the wizard runner)
- Default working team: **Coder** (Cody), **Librarian** (Lila)
- Add by project shape: **QA** (Quinn) for anything user-facing, **Designer** (Derek) for UI, **Researcher** for unknowns, **Professor** for sprints likely to produce learnings
- Multiple instances OK (two Coders for parallel workstreams)

Output: a `team` array in the project metadata. Each persona has a default model, but per-story model assignment overrides it.

### Phase 3 — Epic decomposition

Architect proposes the full epic list. User edits.

For each epic: name, slug, one-paragraph goal, definition of done (checklist).

This is where the architect commits to the shape of the project. Theme Machine's experience says this is doable upfront for goal-driven projects.

### Phase 4 — Sprint plotting

Architect proposes sprint count and per-sprint goals, mapping epics to sprints (an epic can span multiple sprints; a sprint can touch multiple epics).

For each sprint: goal, demo checkpoint (concrete: "human runs X, sees Y").

### Phase 5 — Story decomposition (all the way to the goal)

Architect drafts every story for every sprint. Per `theme_machine` precedent — over-planning isn't fatal because the demo+acceptance gates between sprints catch drift.

For each story:
- Title + slug
- Description (1–2 sentences)
- Acceptance criteria (checklist)
- Points (1/3/5 or s/m/l)
- Suggested persona
- Suggested model
- Confidence: `confirmed` (sprint 1, user signed off) vs `planned` (later sprints, written in good faith)
- Dependencies (other story slugs)

### Phase 6 — Review + commit

Architect presents the full plan: goal, team, epics, sprints, stories. User reviews, edits, accepts.

On accept:
- Write the CSV
- Write the markdown filesystem (`SPRINTS/sprint-N.md`, `EPICS/epic{date}-{slug}/...`)
- Initialize the Kanban board

---

## Stop trigger

The architect knows it has enough when it can write, without hand-waving:
- Goal sentence
- Team roster
- Full epic list with goals + DoD
- Sprint goals + demo checkpoints
- Sprint 1 stories with acceptance criteria

Later-sprint stories with `planned` confidence are allowed to be lighter — the architect can flag uncertainty rather than fabricate.

If the architect stalls on any of the above, that's the next question to ask the user.

---

## Data model

### Story (the atomic unit)

```yaml
---
story_id: catalog-schema
epic: epic0228-theme-catalog
sprint: 2
state: 0-backlog            # 0-backlog | 1-in-progress | 2-finished | blocked
confidence: planned         # confirmed | planned
points: 3
persona: Cody               # which persona will pick this up
model: claude-sonnet-4-6    # suggested model for this story
depends_on: [api-discovery]
---

# Story: Catalog schema

[description, tasks, acceptance criteria — the existing story body format]
```

The `state`, `confidence`, `persona`, and `model` fields are the new front-matter additions. The story filename still encodes state (`0-backlog-catalog-schema.md`) for `ls` visibility — front-matter and filename stay in sync.

### CSV format

One row per story. Columns:

```
story_id, epic_id, epic_name, sprint, state, confidence, title, points,
persona, model, depends_on, acceptance_criteria
```

`acceptance_criteria` is a single cell with bullets — readable in a spreadsheet, parseable by the board.

The CSV is generated from the markdown on demand and the markdown is regenerated from the CSV on board edits. One source of truth at a time, switchable.

### Project metadata

A separate `project.yaml` (or top of the CSV) holds the goal, team, and epic list — things that aren't per-story.

---

## Kanban card

Each card surfaces what a teammate (or the human) needs to glance at:

```
┌─────────────────────────────────────┐
│ S2-3 · Build catalog scraper        │
│ 🧑 Cody (Coder)  ·  3 pts            │
│ 🤖 Sonnet 4.6                        │
│ 📁 epic0228-theme-catalog            │
│ 🏷  planned                          │
└─────────────────────────────────────┘
```

Editable fields on the card: persona, model, points, confidence. Editing a card writes back to the markdown.

Columns: `Backlog`, `In Progress`, `Finished`, `Blocked`. Moving a card across columns renames the file (`0-backlog-*` → `1-in-progress-*`), keeping the filesystem and board in lock-step.

---

## What's an MVP?

Build in this order — each step is independently useful:

1. **Wizard, prose-only** — the architect runs the question script in chat and writes the markdown files directly. No CSV, no board UI yet. Validates the question flow on a real project.
2. **CSV export** — once the markdown is reliable, generate a CSV from it. Validates the data model.
3. **Static board view** — render the CSV as an HTML Kanban board (read-only). Validates the card design.
4. **Editable board** — moving cards / editing fields writes back to markdown. The full loop.
5. **Slash commands** — `/scrum-init`, `/sprint-new`, `/story-move` for the mechanical bits the wizard doesn't cover.

The published `agent-scrum` v1.0 is the foundation for step 1 already — the protocol document tells an agent how to write the files. The wizard is just an opinionated front-end for that.

---

## Open questions

1. **Where does the wizard live?** A skill (`/scrum-init`) inside `agent-scrum`? A standalone HTML+JS interface? A Claude Project? The `INTERFACE/daily-scrum.html` exists — is it heading toward being the board UI?
2. **CSV vs YAML for the project metadata?** CSV is human-friendly in a spreadsheet, YAML is friendlier for the goal/team/epic list. Probably YAML for project, CSV for stories.
3. **Story-to-sprint mapping** — does a story know its sprint via front-matter (clean), or via being listed in the sprint file (matches theme_machine's pattern but causes duplication)? Probably front-matter as source of truth, sprint files generated.
4. **Handling re-plans mid-project** — when sprint 3 reveals epic 5 was wrong, what's the workflow for revising? Re-run the wizard scoped to the affected epic? Edit in place?
5. **Cost projection** — given persona+model assignments on every story, can the wizard show a projected token spend at review time? Useful, but maybe v2.

---

## What this doesn't change

- The published `agent-scrum` v1.0 stays as-is. It's the convention layer; this wizard is a UX layer that produces conformant output.
- Existing manual usage (writing sprint files by hand) still works. The wizard is for new projects starting from scratch.

---

*Last updated: 2026-04-26 by claude-opus-4-7*
