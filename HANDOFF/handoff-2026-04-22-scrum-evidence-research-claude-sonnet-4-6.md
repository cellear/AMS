# Handoff: Scrum Evidence Research + AMS Handoff Setup

**Date:** 2026-04-22
**Author:** claude-sonnet-4-6
**Status:** ✅ Complete

## What Was Attempted and Outcome

The user asked for an inventory of management techniques used across the EXAMPLES/ projects, to support the Stanford WebCamp talk *Agile for Agents: Managing Robots the Way We Manage Humans* (April 30, 2026). All 10 projects were surveyed. Results documented. Additionally, the AMS root project was set up with the agent handoff protocol for the first time.

## What Was Done

### Research

Surveyed all 10 projects in `EXAMPLES/`:
- Examined DOC/, HANDOFF/, SPRINTS/, and other management artifacts in each
- Classified each project by management technique
- Key finding: **4 projects (not 2) evidence Scrum** — theme_machine is the most sophisticated, using a hybrid Scrum + Kanban epic/story state system

Full findings in `DOC/scrum-evidence.md`.

### First-Time AMS Handoff Setup

AMS root had no handoff infrastructure. Created:
- `AGENT.md` — standard agent handoff protocol (v1.1)
- `CLAUDE.md` — points to AGENT.md
- `DOC/` directory
- `HANDOFF/` directory

## What Worked Well

- EXAMPLES survey was comprehensive; the Explore agent found artifacts not previously recalled (theme_machine's epic/story state system, bluegreen's burndown chart)
- Standard handoff protocol from `agent-handoff/AGENT.md` applied cleanly to AMS root

## Current State

- `DOC/scrum-evidence.md` — complete, ready to use as talk reference and AMS design input
- AMS root handoff protocol — bootstrapped, first session documented
- **Missing from AMS:** no Scrum layer yet. The scrum-evidence doc is the research input for building it.

## Open Questions

1. Should `LEARNINGS/` (as used in ddev-xdebug-tui, ddev-drush-tui) be formalized in AMS as the retro convention, or renamed to `RETROS/`?
2. Should the state-prefixed directory pattern from theme_machine (`0-backlog/`, `1-in-progress/`, `2-finished/`) be adopted as the AMS Kanban pattern?
3. Should theme_machine's `handoff-epics-protocol.md` be pulled into AMS as a starting point for the Scrum layer?
4. The AMS `Overview.md` doesn't list `tracktime` or the fact-check skill in its AMS Tools table — is that intentional?

## Files Created or Modified

- `AGENT.md` — created (standard handoff protocol v1.1)
- `CLAUDE.md` — created
- `DOC/scrum-evidence.md` — created (management techniques inventory)
- `HANDOFF/` — created (this directory)
- `HANDOFF/handoff-2026-04-22-scrum-evidence-research-claude-sonnet-4-6.md` — this file

## References

- `DOC/scrum-evidence.md` — full inventory with summary table
- `EXAMPLES/theme_machine/DOC/handoff-epics-protocol.md` — most evolved Scrum pattern found
- `agent-handoff/AGENT.md` — source of the handoff protocol used here
- Stanford WebCamp session: https://webcamp.stanford.edu/session/agile-for-agents-managing-robots-the-way-we-manage-humans
