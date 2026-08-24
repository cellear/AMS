# OFFICES — Protocol

Follow this file only when `CONFIG.md` lists `OFFICES` in `components`.

Each persona may maintain a persistent office at `{offices_dir}/<persona>/` — a small set of files that survive across the fresh-session model, so a persona resuming after a break (or a human clicking in) can see identity, current state, and history without replaying every past handoff.

The staffed roster lives in `CONFIG.md` (`roster`), not in `AGENT.md`. Persona descriptions live in `Personas.md`.

---

## Opt-in

Not every persona has an office. Check before assuming. If `{offices_dir}/<your-persona>/` does not exist, proceed as normal. Offices are built as they prove useful, not mandatory scaffolding on day one.

The human Product Owner usually has no office.

---

## Convention, when an office exists

| File | Purpose | Update pattern |
|---|---|---|
| `desk.md` | Single-glance current state — read **first** on session start | **Overwritten** at every session end |
| `identity.md` | Role, lane, voice — elaboration of the roster / Personas.md row | Refined over time |
| `working-notes.md` | Standing preferences about how this persona's work is done | Persistent observations |
| `open-threads.md` | Unresolved carry-forward items in this persona's lane | Updated as threads open/close |
| Role-specific logs | e.g. `ceremony-log.md`, `build-log.md`, `design-log.md` | Append chronologically |

Starter copies of the four core files live in `_template/`. Use lowercase persona ids (`cody`, `lila`, `quinn`).

---

## Session workflow

**Start:** If your office exists, read `desk.md` first, then continue with `AGENT.md` (recent handoffs, then other enabled components).

**End:** Overwrite `desk.md` with where things stand now. Update `open-threads.md` and other office files as needed. Then write the handoff. The handoff is the historical record; the office is what the persona reads first to reorient.

---

## Source of truth

Offices are personal working memory, not the source of truth. `HANDOFF/` and sprint files (if `SPRINTS` is enabled) remain authoritative for what actually happened. If an office ever conflicts with a handoff or sprint file, the handoff/sprint wins.

---

## Independence

OFFICES requires HANDOFF (as all AMS components do). It does not require DOC or SPRINTS. A project can staff offices and still run without sprints.
