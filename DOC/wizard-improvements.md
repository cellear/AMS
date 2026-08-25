# Wizard Improvements (post-2026-04-26 live run)

Captured during/after the first live wizard run on the drupal-module-refactor test project. To be folded into `agent-scrum/wizard.md` (then committed + pushed + submodule-bumped in AMS).

## Done

- ✅ **"Next session" handoff convention** (added 2026-04-27 to both `agent-scrum/wizard.md` Phase 6 and `agent-scrum/AGENT.md` Workflow). Every per-story handoff now ends with a ready-to-paste prompt for the next session, naming the model and tool. Codifies the chained-prompt pattern from ddev-drush-tui. Bumped agent-scrum to v1.1.

## High-priority — affects every run

1. **Checkpoint to HANDOFF/ after each phase completes**, not just at Phase 6. Context loss should be recoverable by design, not by remembering to ask. Each checkpoint is a partial handoff capturing decisions made in that phase.
1a. **Approvals between sprints, not between stories.** Currently each story handoff produces a "Next session" prompt that the user manually pastes. For multi-story sprints with confident plans, that's friction without payoff. Default behavior: chain stories within a sprint without human gating; only require human approval at sprint boundaries (sprint demo + acceptance). User can override per-story for risky work. Reduces per-story orchestration tax substantially.
1b. **Subagent dispatch as a first-class option.** Claude Code's `Agent` tool lets a session spawn a subagent in a different model context (e.g. a Sonnet session calling Haiku for a focused chunk). The protocol currently only documents session-per-story; should also document subagent dispatch as an alternative for stories that fit cleanly inside a parent session's context. Right tradeoff: smaller stories run as subagents (no setup tax); bigger stories or model-quality changes still warrant their own session. Worth comparing against ddev-xdebug-tui's actual usage to see how it played out there.
2. **Phase 1 — add "What is this *not*?" question.** The drupal-module-refactor run took three rounds to surface that the goal wasn't Project Browser's territory. A negative-space question would have caught it round one.
3. **Phase 2 — require a "skipped, with reasoning" section** in the proposed roster. The drupal-module-refactor architect produced this without being asked; it's load-bearing for showing the human what was considered and rejected.
4. **Phase 4 — architect self-check: if a sprint name needs a comma or "and," split it.** "Decide how we're building it, then build it" was the live example; the user caught it, the architect should have.
5. **Phase 5 — append a next-sprint preview to every sprint-stories block by default.** When the human asks for sprint-by-sprint delivery, they lose foresight; a preview at the end of each block compensates.
6. **Standardize "remind me to scrutinize each epic at sprint start" as a stock architect behavior**, not a memory the architect has to invent on the fly. Belongs in the wizard's architect-role section.

## Medium-priority — improves output quality

7. **Project preferences (delivery cadence, scrutiny reminders, etc.) should land in the project's `DOC/` not in agent-local memory.** When the user says "always do X going forward," it should survive a switch to a different agent or model. Wizard should write a `DOC/project-preferences.md` instead of (or in addition to) saving an agent memory.
8. **Sprint-1-stories output format** that emerged in the live run is better than the wizard prescribes: includes points totals, model distribution, critical path. Promote into wizard.md as the standard format.
9. **Token spend projection at Phase 6** — already in the wizard but worth verifying it actually produces the expected ballpark when Phase 6 runs.

## Low-priority — nice to have

10. **Phase 3 — encourage the architect to read source material in `INCOMING/` (or equivalent) before drafting Phase 1 questions.** The drupal run did this without being told; codify it.
11. **The architect should propose draft answers to Phase 1 questions, not just ask them.** This is implicit in the architect-role guidance ("propose answers when human seems unsure") but worth making explicit.

## Open: schema reference

The wizard references `event-log-schema.md` from `../DOC/` (AMS-internal), which won't exist in a downstream project. Two options:
- Copy the schema into agent-scrum (a `agent-scrum/DOC/event-log-schema.md`)
- Inline the column spec directly in `wizard.md`

Probably the first — keeps wizard.md focused, makes the schema a real protocol artifact.

---

*Last updated: 2026-04-27 by claude-opus-4-7*
