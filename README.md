# Design Forge

A personal Claude Code plugin for UX and frontend work. Library-agnostic, no corporate toolchain required.

Design Forge gives Claude a set of binding laws, skills, and knowledge files that govern every session — from scaffolding a new React project to running UX research to generating a developer handoff document.

---

## What's included

### Skills

| Skill | What it does |
|---|---|
| `claude-laws` | Master binding laws — pre-execution announcement, branch+issue, English-only, no direct push, Conventional Commits, secret scanning, PII rules |
| `scaffold-react-project` | Full `new project` scaffold with UI library choice: shadcn/ui, MUI, Ant Design, Chakra UI, local, or user-specified |
| `frontend-guide` | React standards — 4-file component pattern, no-inline-styles, TypeScript, WCAG 2.2 AA |
| `figma-craft` | Figma construction — Auto Layout, constraints, variables, variants, pixel-perfect checklist, Dev Mode handoff |
| `design-critique` | 8-step design critique with severity levels and concrete suggestions |
| `design-resources` | Curated resources — NN/g, Laws of UX, platform guidelines, accessibility, inspiration galleries |
| `developer-handoff` | Two-surface handoff: `docs/handoffs/<id>.md` (13 sections) + tracking issue in dev repo |
| `feature-workflow` | Designer feature lifecycle — start/pause/resume/finish, any issue tracker |
| `ux-research-guide` | Transcript analysis, RICE + MoSCoW, JTBD, PII redaction |
| `ux-research-deck` | 6-slide outcome deck or 12–18 slide full research deck |
| `ux-writing` | 10 UX writing rules + copy templates |
| `fullstack-workflow` | 10-phase PR runbook for production code |
| `skills-matrix` | Competency standards for layout, design, React, a11y, git, motion, error handling |
| `ppt-template` | Presentation template spec — 16:9, configurable brand colors |
| `pendo-analyst` | Pendo analytics — adoption, NPS, guides, Triangulated Insight Briefs |
| `project-scaffold` | Entry point for the full scaffold runbook |

---

## Install

**One-shot:**

```bash
curl -fsSL https://raw.githubusercontent.com/bojankocijan/design-forge/main/install.sh | bash
```

This:

1. Clones the rules repo to `~/.design-forge`
2. Injects a `@`-import into `~/.claude/CLAUDE.md` (Claude's global memory)
3. Installs the `dforge-update` shell function in your `.zshrc` / `.bashrc`

**Verify** by opening any Claude Code session. You should see:

```
Rules loaded: DESIGN_FORGE v1.0.0
Project: <repo-name>
Persona: Frontend
GitHub: <username>
Ready.
```

---

## Manual install

```bash
git clone https://github.com/bojankocijan/design-forge.git ~/.design-forge
```

Add to `~/.claude/CLAUDE.md`:

```
@~/.design-forge/CLAUDE.md
```

---

## Usage

### Trigger phrases

| Phrase | What happens |
|---|---|
| `update rules` | Runs `dforge-update`, re-imports rules, reprints confirmation |
| `check rules` | Prints loaded version + git log |
| `new project` | Asks UI library choice, then scaffolds a full React project |
| `fullstack mode` | Activates Fullstack persona (10-phase PR runbook) |
| `frontend mode` | Returns to Frontend persona (default) |
| `research mode` | Activates Research persona (6-slide outcome deck) |
| `research mode full` | Research persona with 12–18 slide full deck |
| `pendo mode` | Activates Pendo Analyst persona |
| `start feature` | 3-question triage → writes §11 Active feature to PROJECT_KNOWLEDGE.md |
| `pause feature` | Moves active feature to paused list |
| `resume feature` | Lists paused features, restores chosen one |
| `finish feature` | Closes feature, runs handoff if ready |
| `handoff <id>` | Generates 13-section handoff doc + tracking issue in dev repo |
| `stop preview` | Stops `npm run dev` |
| `start preview` | Restarts `npm run dev` |

### Keeping rules fresh

```bash
dforge-update
```

---

## New project

When you say "new project", Claude will:

1. Ask what kind of project (UX prototype or Fullstack)
2. Ask for the project name (any kebab-case)
3. Ask which UI library to use:
   - **shadcn/ui** — Radix primitives + Tailwind
   - **Material UI (MUI v6)** — Google Material Design
   - **Ant Design** — enterprise-grade
   - **Chakra UI** — accessible, composable
   - **No library** — local styled-components only
   - **Other** — Claude researches and sets it up

Then scaffold: Vite + TypeScript + React + chosen library + CI (ESLint, tsc, Vitest, Playwright) + GitHub Pages preview.

---

## Laws overview

The 19 laws in `CLAUDE_LAWS.md` cover:

- English-only responses
- Pre-execution announcement before any code change
- Branch + issue before code
- No direct push to `main` — PRs only
- No file deletion without approval
- No inline styles in `.tsx` — 4-file component folder pattern
- Conventional Commits
- Secret scanning before every commit
- PII-free mock data
- WCAG 2.2 AA on every component
- Localhost preview running throughout the session
- Design fidelity — only implement what's in the design

---

## License

MIT — see [LICENSE](./LICENSE).
