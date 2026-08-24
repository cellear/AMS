# DOC — Protocol

Follow this file only when `CONFIG.md` lists `DOC` in `components`.

DOC is persistent project knowledge, organized by topic. It is not a session journal (that's HANDOFF) and not a sprint retro or tutorial-in-progress (that's LEARNINGS).

---

## What belongs here

Architecture decisions, conventions, deploy steps, runbooks, known issues, maps of the system — anything a future agent should treat as current truth without replaying handoffs.

Prefer updating an existing file over creating a new one.

When you update a file, add or update a line at the bottom:

```
Last updated: yyyy-mm-dd by [author]
```

---

## What does not belong here

- Chronological session notes — write a handoff
- Dated sprint retros or still-soft findings — write a learning; promote it here once it is a load-bearing convention
- Work in flight — sprint/story files, if `SPRINTS` is enabled

---

## Ownership

The Librarian persona (Lila, when staffed) owns this layer: organization, naming, and keeping files current. Any persona may update a doc when they learn something that belongs here.

---

## Independence

DOC does not require SPRINTS, LEARNINGS, or OFFICES. If those components are off, DOC still works: agents read relevant files at session start and update them during the session, as `AGENT.md` describes.
