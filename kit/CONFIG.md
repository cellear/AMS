# AMS Configuration

This file is the source of truth for which AMS components are enabled and where they live. Leave **Your value** empty to use the default. `INSTALL.md` writes this file; you can also edit it by hand.

Filenames in this kit are uppercase (`CONFIG.md`, `AGENT.md`, `PROTOCOL.md`) so they stand apart from project code. If you have a `config.md` from an older kit, rename it.

---

## Components

| Setting | Default | Your value |
|---|---|---|
| `components` | `HANDOFF` | |
| `epics` | `off` | |
| `roster` | | |

`components` is a comma-separated list of component ids. **`HANDOFF` is always required** and is included even if omitted.

Optional ids: `DOC`, `LEARNINGS`, `SPRINTS`, `OFFICES`, `MARKETING`, `SECURITY`.

`epics` is `on` or `off`. Meaningful only when `SPRINTS` is enabled.

`roster` is a comma-separated list of persona ids (e.g. `cody, lila, quinn`). Meaningful only when `OFFICES` is enabled. The human Product Owner usually has no office.

Follow a component's `PROTOCOL.md` **only** when that component appears in `components`.

---

## Directories

| Setting | Default | Your value |
|---|---|---|
| `ams_dir` | `AMS` | |
| `handoff_dir` | `HANDOFF` | |
| `doc_dir` | `DOC` | |
| `learnings_dir` | `LEARNINGS` | |
| `sprints_dir` | `SPRINTS` | |
| `epics_dir` | `EPICS` | |
| `offices_dir` | `OFFICES` | |
| `marketing_dir` | `MARKETING` | |
| `security_dir` | `SECURITY` | |

## Notes

- `ams_dir` is the folder containing this file. Rename it to `.ams` to keep it hidden, or to any name that fits your project.
- Each directory can live inside `ams_dir` or directly in the project root — the agent checks CONFIG first, then `AMS/<name>`, then the project root.
- Leave the **Your value** column empty to use the default.

## Example: using existing folders

If your project already has a `docs/` folder and a `journal/` folder you'd like AMS to use:

| Setting | Default | Your value |
|---|---|---|
| `ams_dir` | `AMS` | `AMS` |
| `handoff_dir` | `HANDOFF` | `journal` |
| `doc_dir` | `DOC` | `docs` |
