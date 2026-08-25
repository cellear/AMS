# Handoff — kit/ split and INSTALL-AMS.md bootstrap

*Claude · claude-opus-5[1m] · branch `kit-split` · no personas configured (HANDOFF only)*

First handoff in this repo. Until this branch, AMS could not record its own development:
`.gitignore` matched `HANDOFF/`, so any journal written here was untracked.

## What was attempted

Separate the two roles AMS was making one set of files play — the repo you develop, and the
kit you drop into a project — and give acquisition/placement an owner.

## Outcome

Done and verified. 17/17 assertions pass on a clean end-to-end install.

### What worked

- **The `.gitignore` bug is real and is fixed.** Before: copy the kit into a project, write a
  handoff, `git add -A`, and `AMS/HANDOFF/*`, `AMS/DOC/*`, `AMS/SPRINTS/*`, `AMS/LEARNINGS/*`
  were all silently dropped by the kit's own ignore rules. After: all four are tracked.
- **`kit/` as an allowlist.** `cp -R kit/ ../AMS/` is the entire placement step. Verified that
  `.git`, `.gitignore`, `README.md`, `Overview.md`, `Tooling.md`, `INTERFACE/`, `images/`, and
  `INSTALL-AMS.md` all stay behind.
- **The move was cheap, as predicted.** A path audit beforehand showed nearly every internal
  reference is kit-internal and relative (`AGENT.md`→`CONFIG.md`, `INSTALL.md`→`Personas.md`,
  `SPRINTS/PROTOCOL.md`→`agent-scrum/wizard.md`), so they survived untouched. Only root-level
  docs needed link rewrites. Git recorded all 12 moves as renames at 96–100% similarity.
- **Submodule.** Initialized *before* `git mv`, so `.gitmodules` rewrote cleanly to
  `kit/agent-scrum` and `wizard.md` now ships instead of an empty directory.
- **Non-git projects.** Install works; the bootstrap is told to say so and continue rather
  than running `git init` uninvited.

### What didn't

- **First verification run was invalid.** It cloned the branch before the work was committed,
  so it tested the old layout; several assertions passed spuriously. Re-run after committing,
  with the copy step failing hard instead of continuing. Lesson: a test harness that clones
  must clone committed state, and a failed setup step must abort, not degrade into false
  passes.

## Deviation from plan

`INSTALL-AMS.md` does **not** write the root `CLAUDE.md`/`AGENTS.md` tool stubs, though the
plan said it would. `INSTALL.md` Phase 6 already asks which stubs to create and where; having
the bootstrap do it too would be the first crack of exactly the drift the split exists to
prevent. The bootstrap places, excludes, and hands off — nothing more.

## Current state

Branch `kit-split`, two commits, pushed. No blockers. Not merged — Luke opens the PR.

## Open questions

- Should the AMS repo enable `DOC` and `LEARNINGS` for itself, or stay HANDOFF-only? Root is
  now free to track them.
- `INTERFACE/` stays repo-role because README says it is "not part of install". Worth
  revisiting — an installed project might want the office/floorplan pages.
- Branding (AMS vs. a new name) deliberately deferred past Friday's demo. No structural cost
  either way now.

## Files created or modified

- **Moved to `kit/`** — `AGENT.md`, `INSTALL.md`, `CONFIG.md`, `Personas.md`, `DOC/`,
  `LEARNINGS/`, `SPRINTS/`, `OFFICES/`, `MARKETING/`, `SECURITY/`, `agent-scrum` (submodule)
- **New** — `INSTALL-AMS.md`, `kit/README.md`, this handoff
- **Modified** — `.gitignore` (component rules removed), `CLAUDE.md` (now "working on AMS"),
  `README.md` (install flow + `kit/` paths), `Overview.md`, `Tooling.md` (4 dead links fixed,
  2 private repos marked), `.gitmodules`, `kit/AGENT.md` (v3.1), `kit/INSTALL.md` (Phase 1)

## Prompt for Next Assistant

Two things are queued, both outside this repo, and the first is urgent.

```
Read AMS-DIST/kit/AGENT.md and AMS-DIST/HANDOFF/handoff-2026-08-24-kit-split-claude.md.

The kit/ split has merged. Two follow-ups, both in ~/.claude/skills/, neither of which
could be part of the AMS PR:

1. URGENT — fix ~/.claude/skills/handoff/SKILL.md. Its Mode 1 test is "AGENT.md does NOT
   exist in the project root". In an AMS project that is false (AGENT.md lives at
   AMS/AGENT.md), so /handoff installs a competing root AGENT.md plus root HANDOFF/ and
   DOC/, standing up a second system alongside AMS. Make Mode 1 also treat AMS/AGENT.md
   or .ams/AGENT.md as "already set up", and in that case write the handoff into the
   AMS-configured handoff directory. There is a demo on Friday and /handoff is muscle
   memory.

2. Create ~/.claude/skills/ams-install/SKILL.md. Five steps only, no install logic:
   refuse if AMS/ or .ams/ exists (point at AMS/INSTALL.md for re-runs); refuse if
   AMS-INSTALL/ exists; git clone --depth 1 --recurse-submodules
   https://github.com/cellear/AMS.git AMS-INSTALL; append /AMS-INSTALL to
   .git/info/exclude; then read AMS-INSTALL/INSTALL-AMS.md and follow it. Frontmatter:
   name ams-install, disable-model-invocation: true, allowed-tools Bash/Read/Write/Edit/Glob.
   The skill must contain no install logic — INSTALL-AMS.md is the single source of truth,
   or the Claude path and the tool-agnostic path will drift.

Constraints: the repo must stay installable with no skill at all — a Gemini or Cursor user
follows README.md. Verify by installing into a scratch project and confirming AMS/HANDOFF
files are tracked by git. Write a handoff when done.
```
