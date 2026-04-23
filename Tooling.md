# AMS Tooling

Tools and projects in the AMS ecosystem — both things Luke has built and related work by others.

---

## Built Here

| Tool | Description |
|---|---|
| **[Agent Handoff Protocol](agent-handoff/)** | The core convention: `AGENT.md`, `HANDOFF/`, and `DOC/` directories give agents persistent memory across sessions |
| **[Agent Handoff Plugin](agent-handoff-plugin/)** | Claude Code plugin wrapping the protocol; adds `/handoff` command for easy setup and session capture |
| **[Agent Project Tracker](agent-project-tracker/)** | Private template for tracking a portfolio of agent-enabled projects across a machine or organization |
| **[TrackTime](tracktime/)** | Claude Code skill for logging billable hours to CSV; no external services required |

---

## Related Work

Tools by others solving similar problems — surveyed as part of the "Agile for Agents" talk (Part 3).

| Tool | Notes |
|---|---|
| **[Spec Kitty](https://github.com/Priivacy-ai/spec-kitty)** | Robert Douglass's open-source CLI for spec-driven AI development — transforms requirements into implementation through an automated lifecycle: spec, planning, task breakdown, agent execution, review, and merge, with kanban visibility across parallel workstreams. Powerful but higher overhead; the Handoff Protocol was built as a simpler alternative after trying Spec Kitty. |
| **[Zora](https://github.com/ryaker/zora)** | Rich Yaker's agent context system |
| **[Dalia](https://dalia-drupal.com/)** | Lullabot's AI agent framework for Drupal |
