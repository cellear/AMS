You are working **on** AMS itself, not in a project that has AMS installed.

- The framework protocol is `kit/AGENT.md`. Read and follow it.
- `kit/` is the payload — the directory that becomes `AMS/` in a consuming project.
  Anything outside `kit/` is repo-role and never ships.
- Write session handoffs to `HANDOFF/` in this repo root.

## `kit/agent-scrum/` is vendored — never edit it here

It is a verbatim copy of [`cellear/agent-scrum`](https://github.com/cellear/agent-scrum),
currently commit `40f1942` (2026-04-27). It was a git submodule until 2026-08-25; it is now
plain files.

**Why plain files.** The submodule was invisible to AMS users by design — install flattens
`kit/` into `AMS/` as files either way — so its only beneficiary was this repo's own
maintenance, while every cost landed on users: a `--recurse-submodules` flag that silently
yields an empty `agent-scrum/` and no sprint planning when forgotten, and a stale `.git`
pointer that `cp -R` copies into every install, breaking git inside that directory invisibly.
Nine markdown files, three commits in four months. Not a dependency worth that machinery.

**The sync is one-way.** Edit `cellear/agent-scrum`, never this copy. To re-vendor:

    git clone https://github.com/cellear/agent-scrum /tmp/as
    rm -rf /tmp/as/.git && rm -rf kit/agent-scrum
    cp -R /tmp/as kit/agent-scrum

Then update the commit stamp in `kit/agent-scrum/README.md` and in this file. A patch applied
here and not upstream forks the protocol with nothing to detect it.

To install AMS into another project, read `INSTALL-AMS.md`.
