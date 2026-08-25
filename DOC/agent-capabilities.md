# Agent Capabilities: Filesystem Access

## The core assumption

AMS assumes agents can read and write files in the project directory. The protocol relies on this — AGENT.md, CLAUDE.md, AGENTS.md, and .cursorrules all work because the tool reads them automatically on startup, then follows them to AGENT.md, then reads HANDOFF/ and DOC/.

## Which tools can do this

**In-terminal / code-native tools** run inside the repo and have filesystem access:

- Claude Code / Cowork (reads CLAUDE.md)
- OpenAI Codex CLI (reads AGENTS.md)
- Cursor (reads .cursorrules)
- GitHub Copilot (reads .github/copilot-instructions.md)
- Gemini CLI (reads .gemini/styleguide.md)

These tools can follow the full protocol without human intervention.

**Chat-based / desktop tools** don't have automatic filesystem access:

- ChatGPT (web and desktop app)
- Claude.ai (web)
- Gemini (web)

These tools are useful — ChatGPT desktop created the persona avatars for this project, for example — but they won't discover AGENT.md on their own. The human has to paste or upload the relevant context manually.

## Why this matters for AMS

If a human collaborator uses a chat-based tool for part of the work (visual design, brainstorming, research), the handoff protocol breaks silently. The tool doesn't know to read prior handoffs, doesn't know the conventions, and won't write a handoff when the session ends. The human becomes responsible for bridging that gap — either by feeding the tool the right context, or by writing the handoff themselves.

This is a known gap in the protocol, not a bug in any particular tool. AMS should document it clearly so humans know when they're the ones responsible for continuity.

Last updated: 2026-04-26 by claude
