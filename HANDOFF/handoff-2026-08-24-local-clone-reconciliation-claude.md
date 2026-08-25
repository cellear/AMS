# Reconciling the local clone with the kit/ split

claude-opus-5 (1M context) · 2026-08-24 · session 2 of the day

Companion to `handoff-2026-08-24-kit-split-claude.md`. That session worked in
`~/Sites/WELCOMEROBOTOVERLORDS/AMS-DIST` and never saw the primary working clone at
`~/Sites/AMS/AMS`. This session worked in the primary clone and found what the other one
could not have known about.

---

## What was attempted

The human asked for an audit before any cleanup: "we're trying to fix this repo and
probably preserve some things that only exist in this local clone." The files were dated
around the Stanford Webcamp talk (2026-04-30), a period when they were busy, and they
suspected uncommitted work.

That suspicion was correct. Four months of work existed only on that one disk.

## Outcome

`origin/main` is now `46dbbcc`, pushed. Five commits on top of `b77af4f`:

    46dbbcc  Shrink the README persona image to 43 KB
    5a10c1a  Add the April 2026 handoffs and design docs
    b183b13  Merge origin/kit-split: reconcile diverged main with the kit/ split
    76b8294  README: note that chat-based tools can't discover AGENT.md
    b02ebcd  Bump agent-scrum to v1.1; ignore AMS-TEST/ working directory

`b02ebcd` is dated Apr 27 — it had been sitting unpushed since before the talk.

---

## Corrections to the kit-split handoff

Not criticism — that session could not have seen any of this. But the record should be
straight, because two of these were live defects on the published repo.

**1. "This is handoff #1" is wrong — it was #9.**

Eight handoffs from Apr 22–27 already existed in `HANDOFF/`, plus six `DOC/` files. Both
directories were in `.gitignore` at the time, so they never appeared as untracked and were
invisible to `git status`. They are now committed in `5a10c1a`. AMS had been recording its
own development since April; the record simply was not tracked.

The most substantial is `DOC/project-history.md` — a chronological audit of 14 projects
and the technique each used, from bluegreen (Oct 2025) through tracktime (Apr 2026).

**2. The published kit shipped agent-scrum without the handoff convention.**

`origin/main` and `origin/kit-split` both pointed at `e46b0d7`. The local Apr 27 commit had
bumped it to `40f1942` (v1.1, the "Next session" convention). The merge restored `40f1942`.

**3. The kit-split `.gitignore` rewrite dropped the `AMS-TEST/` rule.**

`AMS-TEST/` is 670 MB / 70,610 files of test installations locally. Without that rule a
`git add -A` in the primary clone would have offered all of it. The rule is back, at line
44, and does not conflict with the "no component-directory rules" principle — it is a
working-directory exclusion, not a component.

**4. `AGENTS.md` was left pointing at a file the split moved.**

It said "Read and follow AGENT.md in this project's root directory," but the split moved
that to `kit/AGENT.md`. Untracked, so kit-split never saw it. Now mirrors `CLAUDE.md`.
**Still untracked** — see open questions.

---

## What worked

The merge was clean. `git merge origin/kit-split` produced zero conflicts; `.gitignore` and
`README.md` both auto-merged correctly. Every claim above was verified after the fact
rather than assumed — the submodule pointer, the ignore rule, and the README paragraph were
each checked individually, and all three had survived.

The push was a plain fast-forward. No force, no rewritten SHAs.

## What didn't

**The submodule came up empty after the merge.** `kit/agent-scrum` had the right gitlink but
no working tree, because the path moved from `agent-scrum/` to `kit/agent-scrum/` and the
old directory still held the checkout. This is exactly the failure the kit-split commit
message warned about. Fixed with `git submodule update --init kit/agent-scrum`, then the
orphaned `agent-scrum/` was removed after verifying its contents were identical and its
working tree clean.

Anyone else who pulls this merge will hit the same thing. `git submodule update --init` is
the fix.

**`--depth 1` was recommended, then measured, and it did nothing.** With no history churn,
full and shallow clones were both 2.9 MB. After the README image was replaced there *is*
churn, and it now saves ~330 KB. The recommendation was wrong when first made and is right
now, for a different reason than originally given.

---

## Current state

**Repo.** `main` == `origin/main` == `46dbbcc`. Working tree has four untracked items and
nothing else.

**Sizes, measured with real clones rather than estimated:**

| | |
|---|---|
| full clone | 2.6 MB |
| shallow clone (`--depth 1`) | 2.3 MB |
| tracked content | 1.5 MB — of which 1,248 KB images, 190 KB text |
| all blob versions in history | 1,511 KB — 85% images |

The framework itself is ~190 KB of text. Everything else a clone carries is pictures.

**Demo material moved out** of the repo to `~/Sites/AMS/AMS-DEMOS/` (138 MB, deliberately
not a git repo, outside the repo so it can never be committed). It has its own `README.md`
recording provenance. Contents: the 13 MB scrum dashboard, `board/`, the Apr 30 talk
material and practice recording, and the v2–v6 build zips.

**Backup** of everything as found: `~/Sites/AMS/AMS-BACKUP-2026-08-24` (1.3 GB, includes
`.git`). Untouched.

---

## Blockers

None. Nothing is waiting on anything.

## Still open, in rough priority order

**1. The two follow-ups this repo queued on 2026-08-24 are both still undone.** Verified
just now:

    ~/.claude/skills/handoff/         Mar 26 21:53   <- unchanged, Mode 1 still unguarded
    ~/.claude/skills/handoff-install/ Aug 24 11:10   <- new, has the preflight
    ~/.claude/skills/ams-install/     does not exist

`handoff-install` was created instead, and it does carry the preflight checks. But
`/handoff` itself is untouched: its Mode 1 test is still "no `AGENT.md` in project root,"
which is false in an AMS project because AGENT.md lives at `AMS/AGENT.md`. Running
`/handoff` there still stands up a competing root protocol. The original note said "there
is a demo on Friday and /handoff is muscle memory" — that Friday is 2026-08-28.

**2. `.gitignore` has a case-insensitivity trap.** The `EXAMPLES/` rule (line 25, meant for
the 419 MB root directory) also matches lowercase `examples/` on macOS. It was silently
hiding `INTERFACE/AMS-Scrum-Dashboard/examples/`, which contains a working CSV-to-board
converter. That directory has moved to AMS-DEMOS, so nothing is hidden right now, but the
rule will do it again to any future lowercase `examples/`. Same class of bug the kit/ split
was written to fix.

**3. Trim pass, deferred by choice.** Decide whether `INTERFACE/` (9 avatars + 3 HTML,
954 KB) leaves the repo like the rest of the demo material. If it does, a full clone drops
to roughly 0.5 MB. Related: add `--depth 1 --shallow-submodules` to the documented clone in
`INSTALL-AMS.md`.

**4. Untracked leftovers.** `AGENTS.md` (fixed, worth tracking — it is what Codex and
Cursor read), `INTERFACE/samples-and-screenshots/`, and two files in `images/`:
`scrum-personas-old.jpeg` (the 373 KB original) and `scrum-personas.-smalljpeg` (note the
malformed extension).

**5. Branch cleanup.** `origin/kit-split` and `origin/v3-components` are both fully merged
ancestors of `main` and can be deleted.

**6. History rewrite — considered and declined, for now.** Removing the old image version
and the avatars from history would take a full clone to ~0.7 MB. Conditions are unusually
good (12 commits, one author, every branch merged), but the human decided ~1.9 MB did not
justify it. It stays possible indefinitely; what changes is blast radius, since a rewrite
means a force-push and anyone holding a clone must re-clone. If it ever happens, the moment
is before AMS has an audience, bundled with the `INTERFACE/` decision.

---

## Files created or modified

**Committed:** `README.md` (chat-tools note), `images/scrum-personas.jpeg` (373 KB → 43 KB,
900x600), `.gitignore` (merge), `kit/agent-scrum` → `40f1942`, and 14 recovered files —
`HANDOFF/handoff-2026-04-*.md` (8) and `DOC/*.md` (6).

**Modified, uncommitted:** `AGENTS.md`.

**Moved out of the repo:** `INTERFACE/AMS-Scrum-Dashboard/`, `INTERFACE/avatars.zip`,
`INTERFACE/daily-scrum-drupal.html`, `board/`, `PRESENTATION-PRACTICE/`, `OUTGOING/`,
`presentation.md`, `session-description.md` — all now under `~/Sites/AMS/AMS-DEMOS/`.

**Created outside the repo:** `~/Sites/AMS/AMS-DEMOS/README.md`,
`~/Sites/AMS/AMS-BACKUP-2026-08-24/`.

---

## Notes for whoever reads this in AMS-DIST

`~/Sites/WELCOMEROBOTOVERLORDS/AMS-DIST` is on branch `kit-split` at `b77af4f`. That branch
is now merged into `main` and five commits behind. Fetch and switch to `main` before doing
anything else, and run `git submodule update --init` afterward or `kit/agent-scrum` will be
empty.

Two findings from the dashboard that are recorded in `AMS-DEMOS/README.md` rather than here,
since they concern demo material: the standup interface has two datasets that diverged (the
Apr 27 Drupal retarget landed only in the modular version and was never re-bundled, so no
single-file Drupal standalone exists), and `board/stories.csv` uses a richer schema than
`examples/kanban.csv` — including a per-story `model` column, which is a distinctly AMS
idea and a candidate for the SPRINTS component.

---

## Prompt for Next Assistant

```
Read AMS-DIST/kit/AGENT.md, then AMS-DIST/HANDOFF/handoff-2026-08-24-local-clone-reconciliation-claude.md
(fetch first — it is new), then handoff-2026-08-24-kit-split-claude.md for your own prior context.

You are in ~/Sites/WELCOMEROBOTOVERLORDS/AMS-DIST, on branch kit-split at b77af4f. That
branch has been merged into main and pushed. Before anything else:

    git fetch origin
    git checkout main
    git pull
    git submodule update --init      # kit/agent-scrum is empty without this

main is now 46dbbcc. Five commits landed on top of your work, from the primary clone at
~/Sites/AMS/AMS, which you never had access to. Three things you could not have known:

- Eight April handoffs and six DOC/ files existed but were gitignored, so "Add first
  handoff... This is handoff #1" was actually #9. They are committed now.
- The agent-scrum submodule was stale on your branch (e46b0d7); it is now 40f1942 (v1.1).
- Your .gitignore rewrite dropped the AMS-TEST/ rule, which excludes 670 MB of test
  installs in the primary clone. It is restored at line 44.

The urgent item you queued on 2026-08-24 is STILL NOT DONE, and the Friday demo is
2026-08-28. Verified state of ~/.claude/skills/:

    handoff/          Mar 26 21:53   unchanged — Mode 1 still unguarded
    handoff-install/  Aug 24 11:10   new, has the preflight checks
    ams-install/      does not exist

handoff-install covers install with proper preflight, but /handoff itself was never fixed.
Its Mode 1 test is still "no AGENT.md in project root" — false in an AMS project, where
AGENT.md lives at AMS/AGENT.md. Running /handoff in an AMS project still installs a
competing root AGENT.md plus root HANDOFF/ and DOC/.

Do this first:

1. Fix ~/.claude/skills/handoff/SKILL.md. Mode 1 must also treat AMS/AGENT.md or
   .ams/AGENT.md as "already set up" and, in that case, write the handoff into the
   AMS-configured handoff directory rather than installing anything. Consider whether
   Mode 1 should now just defer to handoff-install, since that skill already exists and
   duplicated install logic will drift.

2. Decide whether ams-install is still wanted as originally specified, or whether
   handoff-install supersedes it. If still wanted: five steps, no install logic — refuse
   if AMS/ or .ams/ exists; refuse if AMS-INSTALL/ exists; git clone --depth 1
   --recurse-submodules https://github.com/cellear/AMS.git AMS-INSTALL; append
   /AMS-INSTALL to .git/info/exclude; then read AMS-INSTALL/INSTALL-AMS.md and follow it.
   INSTALL-AMS.md must stay the single source of truth or the Claude path and the
   tool-agnostic path will drift.

Constraints: skills live in ~/.claude/skills/ and cannot be part of an AMS PR. Do not
re-add component-directory rules to .gitignore — AMS uses those names at root for its own
journal. Write a handoff to HANDOFF/ when you finish.

Lower priority, all documented in the handoff: the EXAMPLES/ gitignore rule matches
lowercase examples/ on macOS and has already hidden a real file once; INTERFACE/ (954 KB)
may leave the repo like the rest of the demo material; origin/kit-split and
origin/v3-components are merged and can be deleted.
```
