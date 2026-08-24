# AMS

This directory is an installed **AMS kit** — the Agent Management System, a lightweight
framework for managing AI agents with defined roles, structured handoffs, and just enough
process to stay on track.

**Start with [AGENT.md](AGENT.md).** That is the session protocol: what to read when a session
begins, what to record while it runs, and what to write before it ends.

| File | What it's for |
|---|---|
| [AGENT.md](AGENT.md) | Session protocol. Agents read this first. |
| [CONFIG.md](CONFIG.md) | Which components are enabled here, and where they live |
| [INSTALL.md](INSTALL.md) | The setup wizard. Re-run it to add components or staff offices. |
| [Personas.md](Personas.md) | Persona roster |
| `*/PROTOCOL.md` | One per component — followed only when `CONFIG.md` lists that component |

Everything except `HANDOFF` is optional. `CONFIG.md` is the source of truth for what this
project actually uses.

Upstream, issues, and updates: https://github.com/cellear/AMS
