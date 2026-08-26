# Agent Scrum Protocol

> **Vendored copy.** This directory is a verbatim copy of
> [`cellear/agent-scrum`](https://github.com/cellear/agent-scrum) at commit `40f1942`
> (2026-04-27), carried inside AMS as plain files rather than a git submodule.
>
> **Do not edit anything in this directory.** The sync is one-way: change
> `cellear/agent-scrum`, then re-vendor. Editing here forks the protocol silently.
> See AMS's `CLAUDE.md` for the sync procedure.

A lightweight Scrum + Kanban convention for AI-assisted projects. No tools, no lock-in — just markdown files and directory naming.

Designed to layer on top of [`agent-handoff`](../agent-handoff/): handoffs capture *what happened in this session*, scrum captures *what work exists, what state it's in, and what we learned*.

## How It Works

```
your-project/
├── AGENT.md          # Handoff + scrum instructions (your AI reads this)
├── HANDOFF/          # Session journals (chronological)        ← agent-handoff
├── DOC/              # Reference docs (persistent, by topic)   ← agent-handoff
├── SPRINTS/          # One file per sprint: goal, stories, demo, acceptance
├── EPICS/            # One subdir per epic with state-prefixed story files
│   └── epic{YYMMDD}-{slug}/
│       ├── epic-definition.md
│       ├── 0-backlog-{task}.md
│       ├── 1-in-progress-{task}.md
│       └── 2-finished-{task}.md
└── LEARNINGS/        # Per-sprint retros
```

Pick the parts you need:

- **Sprints only** — `SPRINTS/` + `LEARNINGS/`. Good for small linear projects. *(Pattern: `ddev-drush-tui`, `ddev-xdebug-tui`.)*
- **Epics + Kanban states** — `EPICS/` with `0-backlog-`, `1-in-progress-`, `2-finished-` filename prefixes. Good for larger or multi-track work. *(Pattern: `theme_machine`.)*
- **Both** — Sprints set the cadence; epics organize the long-lived work that crosses sprint boundaries.

The state prefix in story filenames makes workflow visible at a glance in any file listing — moving a story between states is just a rename.

## Quick Start

1. Copy [`AGENT.md`](AGENT.md) into your project root (or merge into an existing `AGENT.md`).
2. Copy whichever directories you want from [`template/`](template/).
3. Tell your agent to read `AGENT.md`.

## Why This Works

- **No tools required** — Just markdown files and directory names
- **State is visible in `ls`** — `0-backlog-*`, `1-in-progress-*`, `2-finished-*` self-sort
- **Renames = state transitions** — No metadata file to keep in sync
- **Pairs with handoffs** — Session journals reference the epic/story they touched
- **Field-tested** — Extracted from real projects

## Origin

Extracted from patterns that emerged organically across several projects — most fully developed in [Theme Machine](https://github.com/cellear/theme_machine), with simpler variants in [`ddev-drush-tui`](https://github.com/cellear/ddev-drush-tui) and [`ddev-xdebug-tui`](https://github.com/cellear/ddev-xdebug-tui).

## License

MIT — Use it however you want.
