# Management Techniques Inventory: EXAMPLES/ Projects

## Purpose

This document inventories the EXAMPLES/ projects to identify which agent management techniques each one evidenced. It was produced as research input for:

- The Stanford WebCamp talk *Agile for Agents: Managing Robots the Way We Manage Humans* (April 30, 2026)
- Designing the Scrum layer of AMS, which is currently missing

## Per-Project Inventory

Sorted by management sophistication (most structured first).

### theme_machine — **Scrum + Epic/Story State Protocol**
- `.handoff/WORK/EPICS/` with state-prefixed files (`0-backlog`, `1-in-progress`, `2-finished`)
- `_workspace/SPRINTS/` + `_workspace/LEARNINGS/`
- `DOC/handoff-epics-protocol.md` defines a "lightweight scrum-style protocol for managing epics and stories"
- 40+ handoff logs, multi-model coordination (Claude, Cursor, Codex)
- **Most sophisticated example** — hybrid Scrum + Kanban-style state columns

### ddev-xdebug-tui — **Scrum + Agent Handoff**
- `.agent-handoff/SPRINTS/` (5 sprints), `.agent-handoff/HANDOFF/` (8 logs), `LEARNINGS/`
- `DEVELOPMENT_PROCESS.md`: "four sprints, each with an explicit plan document, user stories with acceptance criteria, and demo checkpoints"
- Handoffs are agent-readable, structured

### ddev-drush-tui — **Scrum + Agent Handoff**
- `SPRINTS/` (4 files), `HANDOFF/` (11 logs named by sprint, e.g. `handoff-2026-03-20-sprint2-claude.md`), `LEARNINGS/sprint-1.md`, `DOC/`
- Same pattern as ddev-xdebug-tui — sub-task handoffs within each sprint

### bluegreen — **Scrum-lite (Burndown only)**
- `DOCS/BURNDOWN.md` with explicit progress: "9/15 Tasks Complete (60%)"
- `DOCS/PLAN.md`, `DOCS/HANDOFF.md`, phase-based structure
- No sprints or retros — burndown + phases only

### sd-contentful — **Spec Kitty (Pipeline-driven, not Scrum)**
- `.kittify/missions/` + `.claude/commands/`, `.codex/prompts/`, `.cursor/commands/`
- `docs/001-contentful-migration/`: research → spec → plan → data-model → tasks → checklists
- Multi-IDE agent coordination via CLI templates
- Waterfall-adjacent, not iterative

### wp2bd — **Waterfall + Handoff**
- `DOCS/HANDOFF.md` (narrative architectural doc, not per-session), `DOCS/IMPLEMENTATION-TICKETS.md`, `DOCS/Project Plan_*.md`
- Phase 1/2/3 structure, no sprints
- Extensive `CLAUDE.md`, `DEBUGGING/` discovery logs

### backdrop-admin-ios — **Ad-hoc + Documentation**
- `RECENT_WORK.md` (date-ordered narrative log), `WORKING_ON.md`
- `docs/architecture/` — 15 feature-scoped architecture files
- No sprints, no backlog — solo-contributor workflow with persistent docs

### backdrop-blue-green — **Minimal / Spec-only**
- `SPECS/Backdrop-BlueGreen-Proposal.md`, `DOCS/CHANGES.md`, PR template
- No active management artifacts

### backdrop-umami — **Ad-hoc Session Logs**
- Just `session.md` + one `claude-conversation-2025-11-06-*.md`

### kiza-photo — **None / Undocumented**
- README only

## Summary Table

| Project | Technique | Sprints | Handoff | Backlog/States | Retros |
|---|---|---|---|---|---|
| theme_machine | Scrum + Epic/Story states | ✅ | ✅ (40+) | ✅ state dirs | implicit |
| ddev-xdebug-tui | Scrum + Handoff | ✅ (5) | ✅ (8) | in sprints | ✅ LEARNINGS |
| ddev-drush-tui | Scrum + Handoff | ✅ (4) | ✅ (11) | in sprints | ✅ LEARNINGS |
| bluegreen | Scrum-lite (burndown) | phases | ✅ | ✅ burndown | — |
| sd-contentful | Spec Kitty pipeline | — | — | tasks/checklists | — |
| wp2bd | Waterfall + Handoff | phases | ✅ | tickets | — |
| backdrop-admin-ios | Ad-hoc + arch docs | — | — | WORKING_ON | — |
| backdrop-blue-green | Spec-only | — | — | — | — |
| backdrop-umami | Session log | — | — | — | — |
| kiza-photo | None | — | — | — | — |

## Takeaways for AMS

1. **Scrum is already field-tested in 4 projects** — not just ddev-xdebug-tui and ddev-drush-tui. theme_machine is the most evolved example (epic/story states are a genuine contribution beyond textbook Scrum).
2. **Handoff + Sprints co-occur.** Every Scrum project also has structured handoffs — the two protocols should ship together in AMS.
3. **`LEARNINGS/` = retros.** Already being used under a different name. Worth aliasing or renaming in AMS docs.
4. **The state-prefixed directory pattern** (`0-backlog/`, `1-in-progress/`, `2-finished/` in theme_machine) is a lightweight Kanban that emerged organically. Strong candidate for promotion into AMS.
5. **Spec Kitty (sd-contentful)** is pipeline/waterfall, not iterative — useful counter-example in the talk: multi-agent coordination ≠ Scrum.

## Spot-Check Commands

```bash
ls EXAMPLES/theme_machine/.handoff/WORK/EPICS/
ls EXAMPLES/ddev-xdebug-tui/.agent-handoff/SPRINTS/
cat EXAMPLES/bluegreen/DOCS/BURNDOWN.md
```

---

*Last updated: 2026-04-22 by claude-sonnet-4-6*
