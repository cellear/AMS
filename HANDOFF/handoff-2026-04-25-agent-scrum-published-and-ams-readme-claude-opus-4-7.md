# Handoff: Publish agent-scrum, link from AMS README

**Date:** 2026-04-25
**Author:** claude-opus-4-7
**Status:** ✅ Complete (pending push)

## What Was Attempted and Outcome

Two related goals: (1) publish the freshly-extracted `agent-scrum` protocol as a standalone GitHub repo, and (2) refresh the AMS README with the scrum personas image so the project's public face reflects the new piece. Both done.

## What Was Done

### Published agent-scrum as its own repo

- Cleaned up `agent-scrum/` for public release: added `LICENSE` (MIT), added `.gitignore` (`.DS_Store`), removed stray `.DS_Store` and a redundant `template/EPICS/.gitkeep`.
- Removed `DOC/evidence.md` from the published repo (the inventory was useful as research input, less so as user-facing documentation). Origin section in README still credits theme_machine + ddev-drush-tui + ddev-xdebug-tui.
- Initial commit signed `Co-Authored-By: Claude Opus 4.7`.
- Created and pushed to `https://github.com/cellear/agent-scrum` via `gh repo create ... --push`. Hit a brief race condition where the immediate post-create push 404'd; manual `git push -u origin main` succeeded a few seconds later. Known `gh` quirk, not a real problem.

### Pre-publish PII / sensitive-content scan

Ran a focused scan on the three repos referenced in `agent-scrum`'s Origin section (theme_machine, ddev-drush-tui, ddev-xdebug-tui) before publishing — looking for credentials, third-party PII, internal URLs, profanity, and venting. **All clear.** No remediation needed.

### AMS README — added scrum personas image

- Added `![AMS scrum personas]` image link near the top of `README.md`, just under the H1.
- Source file was named `scrum-peronas-v1.jpeg` (typo + obsolete v1 suffix). Renamed to `scrum-personas.jpeg`.
- Original location `PERSONAS/source/` is gitignored — image link would have 404'd on GitHub. Created new top-level `images/` directory and moved the jpeg there. README link updated to `images/scrum-personas.jpeg`. The `.png` source remains in `PERSONAS/source/` (gitignored, working asset).

### Repo housekeeping

- Added `OUTGOING/` to `.gitignore` (matches existing pattern for non-framework artifacts).
- Converted `agent-scrum/` from an inline directory into a proper git submodule pointing at the new GitHub repo. `.gitmodules` created. AMS now references agent-scrum as an external dependency rather than inlining its source.
- `INTERFACE/avatars.zip` left untracked per user direction.

## What Worked Well

- The PII scan via subagent was fast and gave the user confidence to publish without spending a session reviewing dozens of session logs by hand.
- The submodule conversion was clean — local `agent-scrum/` was already pushed, so `rm -rf` + `git submodule add` did the right thing without losing any work.

## Current State

- `agent-scrum` v1.0 live at https://github.com/cellear/agent-scrum
- AMS working tree has staged + unstaged changes ready for one commit (see Files below). User will push.
- `.git` config has new submodule registered.

## Open Questions

1. Should the `EXAMPLES/` directory (currently gitignored) be retained for talk prep but excluded from the public framework long-term, or move it into a `private/` subdir so the boundary is clearer?
2. Should `agent-handoff/`, `agent-handoff-plugin/`, etc. (also currently gitignored sibling repos) be converted to submodules too, for consistency with the new agent-scrum treatment? Or leave them as-is since they're orthogonal to AMS?
3. The README's "Scrum for AI assistants" tagline now has an actual scrum personas image above it — worth tweaking the opening prose to lean into the image, or leave it?

## Files Created or Modified

- `agent-scrum/LICENSE` — created (MIT)
- `agent-scrum/.gitignore` — created
- `agent-scrum/README.md` — removed two `evidence.md` references
- `agent-scrum/DOC/evidence.md` — deleted before publishing
- `agent-scrum/template/EPICS/.gitkeep` — deleted (redundant)
- `agent-scrum/` — pushed to GitHub as standalone repo
- `images/scrum-personas.jpeg` — created (moved from PERSONAS/source/, renamed)
- `PERSONAS/source/scrum-peronas-v1.{jpeg,png}` — renamed to `scrum-personas.{jpeg,png}`; the `.jpeg` then moved to `images/`
- `README.md` (AMS) — added image link below H1
- `.gitignore` (AMS) — added `OUTGOING/`
- `.gitmodules` — created (agent-scrum submodule)
- `agent-scrum` — now a submodule, not an inline directory
- `HANDOFF/handoff-2026-04-25-agent-scrum-extraction-claude-opus-4-7.md` — prior session handoff (kept)
- `HANDOFF/handoff-2026-04-25-agent-scrum-published-and-ams-readme-claude-opus-4-7.md` — this file

## References

- https://github.com/cellear/agent-scrum — newly published
- `HANDOFF/handoff-2026-04-25-agent-scrum-extraction-claude-opus-4-7.md` — earlier session that built agent-scrum
- `HANDOFF/handoff-2026-04-22-scrum-evidence-research-claude-sonnet-4-6.md` — original research that motivated the extraction
