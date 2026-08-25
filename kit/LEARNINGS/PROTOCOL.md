# LEARNINGS — Protocol

Follow this file only when `CONFIG.md` lists `LEARNINGS` in `components`.

LEARNINGS captures transferable findings so a future agent or teammate can act on them without re-deriving. Two shapes share this directory.

---

## Topical files (always allowed)

Numbered or named maps of a subject — how a subsystem works, a pattern worth repeating, an environment quirk that is not (yet) a project convention.

**Filenames:** `01-slug.md` or `slug.md`. If you use numbers, keep a short reading-order note in `README.md` or at the top of the first file.

Write them as maps, not textbooks: enough to orient, with pointers into the code. They should be usable as the starting context for a fresh session on that topic.

---

## Sprint retros (when SPRINTS is enabled)

**File:** `sprint-{n}.md`

A short, dated bullet list of durable findings from that sprint — gotchas, environment quirks, conventions that emerged.

```markdown
# Sprint {n} Learnings

- 2026-03-19: On this machine, `ddev describe` uses `--json-output` instead of `--json`.
- 2026-03-19: Drush JSON output has an inconsistent `definition.arguments` field — handle with `json.RawMessage`.
```

Keep it short. Phrased so a future agent can act on it.

If `SPRINTS` is not enabled, skip this shape. Use topical files instead (a single rolling `log.md` is fine for a small project).

---

## Promote to DOC

If a learning becomes a load-bearing convention — something every session should treat as current truth — and `DOC` is enabled, promote it to a DOC file and leave a one-line pointer in the learning.

If `DOC` is off, leave the finding in LEARNINGS (or in the handoff if LEARNINGS is also the wrong home).

---

## Independence

LEARNINGS does not require SPRINTS. Topical learnings are valid on a project that never runs sprints. LEARNINGS does not require DOC; promotion is optional.

HANDOFF remains the session journal. Do not dump a whole session into LEARNINGS — extract the durable bit.
