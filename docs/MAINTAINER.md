# Maintainer Guide — Design Forge

## What this repo is

A personal Claude Code plugin for UX and frontend work. It is a library-agnostic fork of Digital.ai's internal `ux-claude-laws` plugin, stripped of all corporate-specific toolchain requirements.

## Repository structure

```
design-forge/
├── .claude-plugin/plugin.json   ← plugin manifest
├── .github/                     ← workflows + PR/issue templates
├── agents/                      ← subagent definitions (Frontend, Fullstack, Design, Research, Pendo)
├── docs/                        ← maintainer and contributor guides
├── knowledge/                   ← binding knowledge files (loaded into Claude context)
│   ├── FRONTEND_GUIDE.md
│   ├── PROJECT_SCAFFOLD.md
│   ├── SKILLS.md
│   ├── FEATURE_WORKFLOW.md
│   ├── FULLSTACK_WORKFLOW.md
│   ├── UX_RESEARCH_GUIDE.md
│   └── PPT_TEMPLATE.md
├── skills/                      ← skill definitions (plugin entry points)
│   ├── claude-laws/
│   ├── design-critique/
│   ├── design-resources/
│   ├── developer-handoff/
│   ├── feature-workflow/
│   ├── figma-craft/
│   ├── frontend-guide/
│   ├── fullstack-workflow/
│   ├── pendo-analyst/
│   ├── ppt-template/
│   ├── project-scaffold/
│   ├── scaffold-react-project/
│   ├── skills-matrix/
│   ├── ux-research-deck/
│   ├── ux-research-guide/
│   └── ux-writing/
├── CLAUDE.md                    ← session entry point (auto-imported by global memory)
├── CLAUDE_LAWS.md               ← the master laws (binding rules)
├── install.sh                   ← one-shot installer
└── README.md
```

## Versioning

This repo uses Conventional Commits. Version bumps in `CLAUDE_LAWS.md` header:

- `feat:` → minor bump (new law, new skill)
- `fix:` → patch bump (law clarification, bug fix)
- `chore:` → no bump (docs, tooling)
- `BREAKING CHANGE:` → major bump

## Making changes

1. Always branch from `main`: `git checkout -b feat/<description>`
2. Open a GitHub issue first.
3. Edit the relevant `CLAUDE_LAWS.md`, `knowledge/*.md`, or `skills/*/SKILL.md` file.
4. Bump the `**Version:**` header in the changed file.
5. Add a changelog entry to `CLAUDE_LAWS.md` if the law behavior changed.
6. Open a PR with `Closes #<issue>` in the body.
7. Merge via the GitHub UI after CI passes.

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md`.
2. Add the skill to `README.md`'s skills list.
3. If the skill has a corresponding knowledge file, add it to `knowledge/` and `@`-import it in `CLAUDE.md`.

## Updating `install.sh`

The installer clones the repo to `~/.design-forge` and injects the import into `~/.claude/CLAUDE.md`. If the clone path or markers change, update both `install.sh` and `README.md`.

## Running `dforge-update`

End users run this shell function (installed by `install.sh`) to pull the latest rules:

```bash
dforge-update
```

This pulls `~/.design-forge` and the updated `CLAUDE.md` import is picked up on the next Claude Code session.
