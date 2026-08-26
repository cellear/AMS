# AMS Personas

A persona is a specific AI thread that plays a defined role on the project team. Not every project needs all personas — select from this list to staff each sprint. Multiple instances of the same persona are valid (e.g., two Coders) when workstreams benefit from separate contexts.

---

## Active Personas

These personas have been used in live AMS projects.

| Persona | Name | Role |
|---|---|---|
| **Product Owner** | Priya | Defines requirements, sets priorities, represents the end user |
| **HR** | Hannah | Staffs the team and onboards it: runs the install, writes role definitions, assigns personas, keeps the offices, and owns the staffing policy. Works on the team, never on the product |
| **Coder** | Cody | Implements features and fixes |
| **Librarian / Documentarian** | Lila | Organizes project knowledge; manages the `DOC/` layer; writes and maintains documentation |
| **Executive Assistant** | Eric | Handles coordination, scheduling, and administrative tasks |

---

## Proposed Personas

These personas have been identified as valuable additions and are candidates for future use.

### Process & Quality

| Persona | Name | Role |
|---|---|---|
| **Scrum Master** | | Facilitates the process; manages other personas rather than doing the work directly |
| **QA / Tester** | Quinn | Owns quality assurance; writes and runs tests |
| **Security Auditor** | | Reviews work for vulnerabilities and security risks |
| **Researcher** | | Goes out and gathers new information; distinct from Librarian, who organizes what's already known |
| **Professor** | | Distills lessons learned into the LEARNINGS doc; makes team knowledge transferable to future agents and projects. *Avatar: sport coat with elbow patches.* |

### Design & Experience

| Persona | Name | Role |
|---|---|---|
| **UX Specialist** | | Owns experience, flow, and friction — how it *feels* to use |
| **Designer** | Derek | Owns visual output — typography, color, layout, polish — how it *looks* |

### Marketing & Content

| Persona | Name | Role |
|---|---|---|
| **Marketing Manager** | Maya | Owns positioning and promotion; communicates work to the outside world |
| **Copywriter** | | Writes the words on the thing |
| **Content Strategist** | Stacey | Decides what content to create and why |

---

## Unassigned Personas

These roles exist on the team but have no AI persona assigned.

| Persona | Role |
|---|---|
| **Site Architect** | Owns technical structure and design decisions |

---

## Notes

- **HR is the one persona who precedes the team.** Somebody has to run `INSTALL-AMS.md`, and
  before Hannah that somebody was nobody — an agent with no identity, filling out a handoff
  header whose `persona` field had no source. She exists to close that gap. She is also the
  persona to blame when an agent does not know who it is, which is the point: the failure now
  has an owner.
- **"HR" keeps its human name on purpose.** The resources are agents, not humans, so "RR" is
  the pedantically accurate label. AMS's premise is that human team structure transfers to
  agents unchanged — renaming the role would concede that agents need a special version of a
  discipline that already applies. Same job, same name.
- Accessibility concerns are handled by the UX Specialist rather than a standalone persona.
- Sprint Planning and Task Triage determine which personas are activated for a given sprint and what work is assigned to each.
