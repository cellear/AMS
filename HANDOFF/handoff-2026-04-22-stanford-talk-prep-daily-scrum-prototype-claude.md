# Handoff — 2026-04-22

**Task:** Stanford Webcamp talk prep + Daily Scrum interface prototype
**Author:** Claude (Sonnet 4.6)
**Session:** Cowork mode

---

## What Was Accomplished

### Talk Structure (presentation.md)
Defined and documented a three-part structure for the April 30 Stanford Webcamp talk "Agile for Agents: Managing Robots The Way We Manage Humans":

- **Part 1** — Origin story: failures managing WP4BD and BlueGreen simultaneously; tried Spec Kitty (powerful but too much effort for Luke's workflow), then Jira (familiar but still overhead), finally arrived at the one-page Handoff Protocol
- **Part 2** — AMS proper: Scrum for AI assistants, Handoff Protocol as communications layer, Personas as team structure layer, Sprint Planning & Task Triage (Opus evaluates/assigns tasks to cheapest capable model)
- **Part 3** — Landscape survey of other tools, framed by Luke's "simplify" philosophy (simplifydrupal.com, drupalisms working group). Can be trimmed to a single slide if time is short.

### Personas (Personas.md)
Significantly expanded and reformatted. Key decisions:
- Large pool of available personas; staff each sprint from the list as needed
- Multiple instances of same persona are valid (context management, not capacity)
- **Added:** Scrum Master, QA/Tester, Security Auditor, Researcher, Copywriter, Content Strategist
- **Kept separate:** Designer (visual, how it looks) vs. UX Specialist (experience, how it feels)
- **Marketing Manager** retained — AMS is explicitly not just for coding
- **Accessibility** folded into UX Specialist rather than standalone
- Luke added names and avatars directly: Priya (PO), Cody (Coder), Lila (Librarian/Doc), Eric (EA), Quinn (QA), Derek (Designer), Maya (Marketing), Stacey (Content Strategist)
- Scrum Master persona added (unnamed so far)

### Sprint Planning & Task Triage (named concept)
Established name for the pattern where a powerful model (Opus) evaluates and sizes tasks, then assigns each to the cheapest model capable of completing it (often Haiku). Framed as the Scrum Master + team assignment pattern. Token budget motivation: Luke is on $20/month Pro plan and frequently hits limits.

### Document Formatting
Reformatted all root .md files with proper Markdown: Overview.md, Tooling.md, Personas.md, session-description.md, presentation.md. Added cross-links, tables, and structured sections throughout.

### Tooling.md
Restructured into "Built Here" (with descriptions from subproject READMEs) and "Related Work" sections. Three placeholder rows remain for Spec Kitty, Rich Yaker's thing, and Lullabot's thing — need descriptions from Luke.

### Daily Scrum HTML Interface (daily-scrum.html)
Built an interactive comic book-style Daily Scrum interface:
- Four active personas (Priya, Cody, Lila, Eric) arranged around a conference table
- Avatars at 170×210px with transparent backgrounds from `PERSONAS/trans/`
- Click a persona → modal popup with Done / Next / Blockers buttons
- Click a button → content reveals at larger text size; active button fills with role color
- Halftone dot background, Bangers font, thick comic book borders throughout
- Standup data currently hardcoded as JS object — ready to be replaced by HANDOFF/-generated content

---

## Current State

The talk outline is solid. Personas list is well-developed. The Daily Scrum prototype is functional and visually on-brand.

---

## Open Questions / Blockers

- **Tooling.md placeholders** — Need Luke's descriptions for Spec Kitty, Rich Yaker's thing, and Lullabot's thing (Part 3 survey content)
- **Scrum Master name** — No name assigned yet for the Scrum Master persona
- **UX Specialist name** — No name assigned yet
- **Security Auditor name** — No name assigned yet
- **Researcher name** — No name assigned yet
- **Copywriter name** — No name assigned yet
- **Daily Scrum data source** — Standup content is placeholder; next step is building a skill that reads HANDOFF/ files and synthesizes Done/Next/Blockers per persona
- **Talk timing** — Eric flagged a talk run-of-show/timing breakdown as next action

---

## Repo Prep Decisions

- `PERSONAS/opaque/` excluded from repo (not needed)
- `PERSONAS/source/` excluded from repo (working files, not public yet)
- `PERSONAS/trans/` copied to `INTERFACE/avatars/` — avatars treated as interface assets; HTML paths updated accordingly; `PERSONAS/trans/` gitignored
- Sub-projects (`agent-handoff/`, `agent-handoff-plugin/`, `agent-project-tracker/`, `tracktime/`, `claude-fact-check-skill/`) excluded — each has its own repo, referenced in Tooling.md
- `INTERFACE/` v1 files and concept sketches (`daily-scrum-v1.html`, `office-v1.html`, `daily-scrum-v1 Large.jpeg`, `scrum-idea-1.jpg`) added to `.gitignore` — kept locally but not staged
- `presentation.md` and `session-description.md` excluded — talk-specific, not part of the framework
- `HANDOFF/` and `DOC/` excluded from repo

## Files Created or Modified

| File | Action |
|---|---|
| `presentation.md` | Created (from Luke's raw notes) |
| `Personas.md` | Major rewrite — expanded, formatted, named personas added by Luke |
| `Overview.md` | Created (was empty) |
| `Tooling.md` | Reformatted, expanded with subproject descriptions |
| `session-description.md` | Reformatted with headers and structured bullet list |
| `daily-scrum.html` | Created — interactive comic book Daily Scrum interface |

---

## Avatar Files

Located at `PERSONAS/trans/`:
- `priya-product-owner.png`
- `cody-coder.png`
- `Lila-librarian.png`
- `eric-EA.png`
- `maya-marketing.png`
- `derek-designer.png`
- `quinn-qa.png`
- `scrum-master.png`
- `stacey-strategist.png`
