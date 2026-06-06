# Design Forge

A personal Claude Code plugin for UX and frontend work. Library-agnostic, no corporate toolchain required.

Design Forge gives Claude a set of binding laws, skills, and knowledge files that govern every session — from scaffolding a new React project to running UX research to generating a developer handoff document.

---

## Install

Three ways to use Design Forge. Pick the one that fits your setup.

---

### Path A — Claude Code plugin (Cowork marketplace)

The easiest path if you use [Claude Code](https://claude.ai/code) with the Cowork plugin system.

1. Open Claude Code
2. Run `/cowork install design-forge` (or find **Design Forge** in the Cowork marketplace)
3. Done — rules and skills load automatically in every session

No cloning, no shell config. Plugin updates when you run `/cowork update design-forge`.

---

### Path B — Global install via `install.sh`

Best for Claude Code CLI, desktop app, or VS Code / JetBrains extension users.

```bash
curl -fsSL https://raw.githubusercontent.com/BojanKocijan/design-forge/main/install.sh | bash
```

> **Note:** The repo is private. You need to be authenticated with `gh auth login` before running this, or clone manually (see Path B manual below).

**What it does:**
1. Clones the rules repo to `~/.design-forge`
2. Injects `@~/.design-forge/CLAUDE.md` into `~/.claude/CLAUDE.md` (Claude's global memory)
3. Installs the `dforge-update` shell function in your `.zshrc` / `.bashrc`

**Verify** by opening any Claude Code session — you should see:

```
Rules loaded: DESIGN_FORGE v1.0.0
Project: <repo-name>
Persona: Frontend
GitHub: <username>
Ready.
```

**Keep rules fresh:**

```bash
dforge-update
```

**Manual clone (alternative):**

```bash
git clone https://github.com/BojanKocijan/design-forge.git ~/.design-forge
```

Then add to `~/.claude/CLAUDE.md`:

```
@~/.design-forge/CLAUDE.md
```

---

### Path C — GitHub Copilot custom instructions

Use Design Forge rules inside GitHub Copilot (VS Code, JetBrains, or github.com).

**VS Code:**
1. Copy the contents of [`knowledge/FRONTEND_GUIDE.md`](./knowledge/FRONTEND_GUIDE.md) and [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md)
2. Open VS Code settings → search `github.copilot.chat.codeGeneration.instructions`
3. Add a file entry pointing to a local copy, or paste the content directly

**github.com (Copilot Chat):**
1. Go to [github.com/settings/copilot/custom_instructions](https://github.com/settings/copilot/custom_instructions)
2. Paste the contents of `CLAUDE_LAWS.md` — this governs Copilot in every repo you touch

**JetBrains:**
1. Settings → GitHub Copilot → Chat → Custom Instructions
2. Paste the contents of `CLAUDE_LAWS.md`

---

## Wiring a project to Design Forge

Once installed, wire any project by adding two lines to its `CLAUDE.md`:

```
@~/.design-forge/CLAUDE.md
@./PROJECT_KNOWLEDGE.md
```

This loads Design Forge rules first, then your project-specific context on top.

---

## Project registry

All projects governed by Design Forge are registered in [`projects.yaml`](./projects.yaml). Each project gets a **locked localhost port** so multiple projects never clash.

When you start a new project, Claude registers it in `projects.yaml` via the standard issue + PR flow, then locks that port in `vite.config.ts` with `strictPort: true`.

Current registry:

| Project | Repo | Port | Status |
|---|---|---|---|
| remodo | BojanKocijan/ReMoDo | 5173 | active |

---

## What's included

### Skills

| Skill | What it does |
|---|---|
| `claude-laws` | Master binding laws — pre-execution announcement, branch+issue, English-only, no direct push, Conventional Commits, secret scanning, PII rules |
| `scaffold-react-project` | Full `new project` scaffold with UI library choice (shadcn/ui, MUI, Ant Design, Chakra UI, local, or custom) and deployment choice (Netlify, GitHub Pages, or none) |
| `frontend-guide` | React standards — 4-file component pattern, no-inline-styles, TypeScript, WCAG 2.2 AA |
| `figma-craft` | Figma construction — Auto Layout, constraints, variables, variants, pixel-perfect checklist, Dev Mode handoff |
| `design-critique` | 8-step design critique with severity levels and concrete suggestions |
| `design-resources` | Curated resources — NN/g, Laws of UX, platform guidelines, accessibility, inspiration galleries |
| `developer-handoff` | Two-surface handoff: `docs/handoffs/<id>.md` (13 sections) + tracking issue in configured GitHub Issues repo |
| `feature-workflow` | Designer feature lifecycle — start/pause/resume/finish, any issue tracker |
| `ux-research-guide` | Transcript analysis, RICE + MoSCoW, JTBD, PII redaction |
| `ux-research-deck` | 6-slide outcome deck or 12–18 slide full research deck |
| `ux-writing` | 10 UX writing rules + copy templates |
| `fullstack-workflow` | 10-phase PR runbook for production code |
| `skills-matrix` | Competency standards for layout, design, React, a11y, git, motion, error handling |
| `pendo-analyst` | Pendo analytics — adoption, NPS, guides, Triangulated Insight Briefs |
| `project-scaffold` | Entry point for the full scaffold runbook |

### Agents

| Agent | Scope | Activated by |
|---|---|---|
| **Frontend** *(default)* | UI, React, Tailwind, components, a11y, localStorage, mocked data | Default |
| **Fullstack** | Everything Frontend + Supabase, APIs, auth, CI/CD, deployment | `fullstack mode` |
| **Design** | Figma, critique, tokens, handoff | Figma / design tasks |
| **Research** | Transcript analysis, JTBD, RICE + MoSCoW, decks | `research mode` |
| **Pendo Analyst** | Product analytics via Pendo MCP | `pendo mode` |

---

## Trigger phrases

| Phrase | What happens |
|---|---|
| `update rules` | Runs `dforge-update`, re-imports rules, reprints confirmation |
| `check rules` | Prints loaded version + git log |
| `load rules` | Re-imports rules without pulling |
| `new project` | Asks project type → UI library → deployment → GitHub Issues repo, then scaffolds |
| `fullstack mode` | Activates Fullstack persona |
| `frontend mode` | Returns to Frontend persona |
| `research mode` | Activates Research persona (6-slide outcome deck) |
| `research mode full` | Research persona with 12–18 slide full deck |
| `pendo mode` | Activates Pendo Analyst persona |
| `start feature` | 3-question triage → writes active feature to `PROJECT_KNOWLEDGE.md §12` |
| `pause feature` | Moves active feature to paused list |
| `resume feature` | Lists paused features, restores chosen one |
| `finish feature` | Closes feature, runs handoff if ready |
| `handoff <id>` | Generates 13-section handoff doc + tracking issue in configured GitHub Issues repo |
| `stop preview` | Stops `npm run dev` |
| `start preview` | Restarts `npm run dev` on the project's locked port |

---

## Laws overview

The 20 laws in [`CLAUDE_LAWS.md`](./CLAUDE_LAWS.md) cover:

- English-only responses
- Pre-execution announcement before any code change (Understanding / Updating / Severity / Data layer / Affected / Code change / Branch / Issue)
- Branch + issue before any code
- No direct push to `main` — PRs only, with required PR summary format
- No file deletion without approval
- No inline styles in `.tsx` — 4-file component folder pattern
- Conventional Commits
- Secret scanning before every commit
- PII-free mock data
- WCAG 2.2 AA on every component
- Localhost preview on a **locked port per project** throughout the session
- Every project registered in `projects.yaml`
- Design fidelity — only implement what's in the design

---

## License

MIT — see [LICENSE](./LICENSE).
