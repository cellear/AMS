# AMS Configuration

Customize directory names to fit your project. Change any value in the table below — your agent will use these names instead of the defaults.

| Setting | Default | Your value |
|---|---|---|
| `ams_dir` | `AMS` | |
| `handoff_dir` | `HANDOFF` | |
| `doc_dir` | `DOC` | |

## Notes

- `ams_dir` is the folder containing this file. Rename it to `.ams` to keep it hidden, or to any name that fits your project.
- `handoff_dir` and `doc_dir` can live inside `ams_dir` or directly in your project root — your agent checks both.
- Leave the "Your value" column empty to use the default.

## Example: using existing folders

If your project already has a `docs/` folder and a `journal/` folder you'd like AMS to use:

| Setting | Default | Your value |
|---|---|---|
| `ams_dir` | `AMS` | `AMS` |
| `handoff_dir` | `HANDOFF` | `journal` |
| `doc_dir` | `DOC` | `docs` |
