You are working **on** AMS itself, not in a project that has AMS installed.

- The framework protocol is `kit/AGENT.md`. Read and follow it.
- `kit/` is the payload — the directory that becomes `AMS/` in a consuming project.
  Anything outside `kit/` is repo-role and never ships.
- Write session handoffs to `HANDOFF/` in this repo root.

To install AMS into another project, read `INSTALL-AMS.md`.
