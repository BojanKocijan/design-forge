# Design Forge

**A Claude Code plugin that makes Claude work like a disciplined senior product engineer — for UX and frontend work.**

Design Forge is a set of binding *laws*, reusable *skills*, and shared *knowledge files* that govern every Claude Code session: how it scaffolds a React app, how it names branches and opens PRs, how it writes components, how it runs UX research, and how it hands work off to developers. Library-agnostic. No corporate toolchain required.

> Think of it as a constitution for your AI pair-programmer: announce before acting, branch + issue before code, never push to `main`, never merge for you, no inline styles, WCAG 2.2 AA, no bloated code, no hallucination.

---

## Table of contents

- [Why](#why)
- [Install](#install)
- [Commands](#commands)
- [How it works](#how-it-works)
- [Wiring a project](#wiring-a-project)
- [Project registry](#project-registry)
- [Updating](#updating)
- [Contributing](#contributing)
- [License](#license)

---

## Why

Out of the box, an AI assistant will happily push to `main`, invent APIs, over-engineer, and forget your conventions between sessions. Design Forge fixes that with **30 binding laws** Claude must follow, plus knowledge files that travel with you to every project:

- **Disciplined git** — pull the default branch, branch + issue before code, PRs only, Claude never merges, **immediate branch cleanup after every merge** (no prompt needed).
- **Pre-execution announcements** — Claude tells you what it's about to change (and its severity) before touching anything.
- **Arm / disarm** — suspend all laws for a session when you want unconstrained speed; hard-safety rails (never merge, no secrets, no PII) always survive.
- **Senior-engineer thinking** — YAGNI, edge-case analysis upfront, "verify before claiming" (no hallucination), reason out loud before executing.
- **Consistent frontend** — 4-file component folders, no inline styles, TypeScript, React Router architecture, accessibility baked in.
- **Whole workflows** — new-project scaffolding, UX research → deck, feature lifecycle, and a two-surface developer handoff.

---

## Install

Design Forge works **today** with Claude Code and the Claude CLI. A one-click install from the Claude/Cowork marketplace is **coming soon** (see [below](#coming-soon-marketplace)).

### Path A — Claude Code / Claude CLI (recommended, works today)

Installs the rules into Claude's **global memory** so every session on your machine inherits them automatically.

```bash
curl -fsSL https://raw.githubusercontent.com/BojanKocijan/design-forge/main/install.sh | bash
```

What it does:

1. Clones the repo to `~/.design-forge`
2. Injects `@~/.design-forge/CLAUDE.md` into `~/.claude/CLAUDE.md` (Claude's global memory)
3. Installs the `dforge-update` shell function for pulling the latest rules

Verify by opening any session — you should see a `Rules loaded: DESIGN_FORGE v2.0.0` confirmation line. Keep rules fresh anytime with `dforge-update`.

### Path B — Clone and use it yourself

Prefer to read, fork, or adapt the rules? Just clone the repo and point Claude's global memory at it:

```bash
git clone https://github.com/BojanKocijan/design-forge.git ~/.design-forge
# then add this line to ~/.claude/CLAUDE.md:
#   @~/.design-forge/CLAUDE.md
```

From here you own it — edit the laws, knowledge, and skills to fit your workflow.

### Path C — GitHub Copilot

Paste the contents of [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md) into your Copilot custom instructions (VS Code settings, JetBrains, or github.com → Copilot → custom instructions).

### Coming soon: marketplace

The repo already ships a valid plugin manifest (`.claude-plugin/plugin.json`) and marketplace descriptor (`.claude-plugin/marketplace.json`), so a one-click install from the **Claude / Cowork marketplace** will be available once Design Forge is submitted to and listed by Anthropic. Until then, use Path A or B above.

---

## Commands

Type these in any Claude Code session:

| Command | What it does |
|---|---|
| `new project` | Scaffold Vite + React + TypeScript. Asks platform, UI library, **app architecture** (routing / shell / data flow / auth), deployment, then builds it with CI + tests. |
| `update rules` | Pull the latest rules and reload them in this session |
| `check rules` | Print the loaded version + git status of the rules clone |
| `load rules` | Re-import the rules without pulling |
| `start feature` | 3-question triage → tracks an active feature in `PROJECT_KNOWLEDGE.md` |
| `pause feature` / `resume feature` / `finish feature` | Feature lifecycle management |
| `handoff <id>` | Generate a 13-section developer handoff doc + a tracking issue |
| `team` / `build feature` | Start the Lead-orchestrated team pipeline (plan → build → test → document → review) |
| `fullstack mode` | Activate the **Lead** (team orchestrator) |
| `frontend mode` | Frontend persona — UI (default) |
| `backend mode` | Backend persona — APIs, auth, DB, migrations, observability |
| `tester mode` | Tester persona — tests + a11y/coverage gate |
| `research mode` / `research mode full` | UX research → 6-slide outcome deck (or 12–18 slide full deck) |
| `analyst mode` | Product-analytics persona — Pendo, Amplitude, Mixpanel, PostHog, GA4, … via their MCP |
| `disarm` | Suspend all Design Forge laws for this session. Hard-safety rails survive: never merge · no secrets · no PII. A `⚠ DISARMED` banner appears on every response. |
| `arm` | Restore full governance. Prints `✓ ARMED` once and continues normally. Always armed at session start. |
| `dry run` / `auto git` | Toggle "print the git/gh commands for me to run" vs "Claude runs them" |
| `stop preview` / `start preview` | Control the background `npm run dev` |

---

## How it works

Design Forge has three layers:

### 1. Laws — [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md)

30 binding rules Claude must follow. Highlights:

- English-only · pre-execution announcement before any change
- Pull default branch → branch + issue → code → PR · **never push to the default branch, never merge for you**
- Check for existing PRs before starting (no duplicate work)
- **Immediate branch cleanup after every merge** — same response, no prompt
- No inline styles (4-file component folders) · Conventional Commits · secret scanning · PII-free mock data · WCAG 2.2 AA
- YAGNI · edge-case thinking · verify-before-claiming · reason-before-executing
- `dry run` mode — print git/gh commands for you to run instead of spending tokens
- `disarm` / `arm` — suspend or restore all laws for a session; three hard-safety rails (never merge · no secrets · no PII) always survive

### 2. Personas — a team that works one pipeline

Team roles compose into a single pipeline (**plan → build → test → document → review → human-merge**) via [`TEAM_WORKFLOW.md`](./knowledge/TEAM_WORKFLOW.md); start it with `team` / `build feature`.

| Persona | Scope | Activated by |
|---|---|---|
| **Frontend** *(default)* | UI, React, a11y, localStorage, mocked data | default / `frontend mode` |
| **Backend** | APIs, auth, DB, server logic, migrations, observability | `backend mode` |
| **Lead** | Orchestrates the team — scope, delegate, review, drive the PR | `team` / `fullstack mode` |
| **Tester** | Tests + axe/coverage gate; can block the PR | `tester mode` |
| **Design** / **Research** / **Analyst** | Supporting — Figma/critique · transcripts/RICE · product analytics (Pendo, Amplitude, Mixpanel, …) | `research mode` / `analyst mode` |

### 3. Knowledge — [`knowledge/`](./knowledge/)

Binding guides Claude reads before acting: `FRONTEND_GUIDE`, `PROJECT_SCAFFOLD`, `COMPONENT_PATTERNS`, `SKILLS`, `UX_RESEARCH_GUIDE`, `FULLSTACK_WORKFLOW`, `FEATURE_WORKFLOW`.

Full version history: [`RELEASES.md`](./RELEASES.md).

---

## Working with the team

**Default:** every session starts as **Frontend** — nothing auto-triggers the team. You opt in.

**Start the whole pipeline** for a real feature:

```
team            # or:  build feature: add CSV export to the invoices page
```

The **Lead** then runs it end-to-end — scope → build → test → document → review → PR — pausing for your go-ahead on multi-file edits and architectural calls (Law 2), and stopping at "PR open, CI green" for you to merge (Law 7). You don't hand-trigger each stage; the Lead drives the handoffs.

**Or call one role directly** — which agent for what:

| You want to… | Trigger | Role |
|---|---|---|
| Build / change UI | `frontend mode` *(default)* | Frontend |
| Build an API, DB, auth, migration | `backend mode` | Backend |
| Run a feature end-to-end with the team | `team` / `build feature` | Lead |
| Write/run tests, check a11y + acceptance | `tester mode` | Tester |
| Document a change (README/API/RELEASES) | part of building it | the role that built it (Lead-enforced) |
| Critique a design, Figma work | (design task) | Design |
| Analyse transcripts → deck | `research mode` | Research |
| Query product analytics | `analyst mode` | Analyst |

**The rules win.** Every agent obeys [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md) completely. If an agent thinks it should do something outside the rules — skip a gate, merge, delete a file, take a shortcut — it **stops and asks** rather than acting on its own (Law 29). The laws are not suggestions.

---

## Wiring a project

**You never wire the laws per project** — Path A and Path B load them globally for every session. A project only needs its own context in a one-line `CLAUDE.md`:

```
@./PROJECT_KNOWLEDGE.md
```

You don't type this by hand: `new project` writes it for you, and for an existing project Claude creates it automatically the first time you do code work.

> Don't add `@~/.design-forge/CLAUDE.md` to a project — it's redundant (laws are global) and breaks the repo on machines without Design Forge.

---

## Project registry

Design Forge can assign each of your projects a **locked localhost port** so running several `npm run dev` servers in parallel never clashes. Your personal registry lives in `projects.yaml`, which is **gitignored** — copy the template to start:

```bash
cp projects.example.yaml projects.yaml
```

Claude auto-registers new projects (issue → branch → PR) and locks the port in `vite.config.ts` with `strictPort: true`. Your project list stays private to your machine.

---

## Updating

- **Installed (Path A):** run `dforge-update`, or `git -C ~/.design-forge pull`
- **Cloned (Path B):** `git -C ~/.design-forge pull`
- In any session: type `update rules`

---

## Contributing

Issues and PRs welcome. Every change follows the project's own laws: branch + issue first, Conventional Commits, PRs only, CI (markdownlint) must be green. See [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md) for the full workflow.

---

## License

[MIT](./LICENSE) © Bojan Kocijan
