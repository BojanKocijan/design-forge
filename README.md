# Design Forge

Personal Claude Code plugin for UX and frontend work — library-agnostic, no corporate toolchain.

Governance laws + skills for scaffolding React projects, running UX research, writing design critiques, and shipping production code with discipline.

---

## Install

**Three ways to use:**

| Path | For | Command |
|---|---|---|
| **Plugin** | Cowork | `/cowork install design-forge` |
| **Global** | Claude Code CLI / VS Code / JetBrains | `curl -fsSL https://raw.githubusercontent.com/bojankocijan/design-forge/main/install.sh \| bash` |
| **Copilot** | GitHub Copilot in any IDE | Paste [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md) into Copilot custom instructions |

---

## Quick Reference

| Command | What it does |
|---|---|
| `update rules` | Pull latest from GitHub, reload in this session |
| `dforge-update` | Shell function: refresh rules + Copilot config |
| `new project` | Scaffold Vite + React + TypeScript + chosen library |
| `fullstack mode` | Activate Fullstack persona (backend + Supabase + CI) |
| `research mode` | Activate Research persona (RICE + MoSCoW + PPT) |
| `start feature` | Begin feature triage + tracking |
| `handoff <id>` | Generate developer handoff doc + tracking issue |

---

## What's Governed

- **24 laws** covering: branch + issue before code, pre-execution announcements, no merging without human approval, WCAG 2.2 AA, no bloated code, edge-case thinking, hallucination prevention
- **5 personas:** Frontend (default), Fullstack, Design, Research, Pendo Analyst
- **Project registry:** locked localhost ports, auto-registration, knowledge base links
- **Full details:** [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md), [`knowledge/`](./knowledge/), [`README_FULL.md`](./README_FULL.md)

---

## Wire a Project

**You don't wire laws per project — the global install does it once.** `install.sh` injects `@~/.design-forge/CLAUDE.md` into your global `~/.claude/CLAUDE.md`, so every session inherits the laws automatically.

A project only needs its own context, linked from a one-line `CLAUDE.md`:

```
@./PROJECT_KNOWLEDGE.md
```

You never type this by hand — `new project` writes it at scaffold time, and for an existing project Claude creates it automatically the first time you do code work. The project also auto-registers with a locked port.

> **Don't** add `@~/.design-forge/CLAUDE.md` to a project — it's redundant (laws are already global) and breaks if the repo is opened on a machine without Design Forge.

---

## Releases

See [`RELEASES.md`](./RELEASES.md) for version history and what changed.
