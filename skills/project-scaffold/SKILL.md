---
name: project-scaffold
description: Scaffold a new React + TypeScript project — triggers the UI library choice question (shadcn/ui, MUI, Ant Design, Chakra UI, local, or user-specified), then builds Vite + TypeScript + chosen library + styled-components base layer + 4-file component pattern + CI + GitHub Pages preview. Also creates PROJECT_KNOWLEDGE.md and local CLAUDE.md. This is the entry-point skill; the detailed runbook is in knowledge/PROJECT_SCAFFOLD.md and the step-by-step procedure is in skills/scaffold-react-project/SKILL.md.
---

# Project Scaffold

## When to invoke

User says "new project", "scaffold a project", "create a React app", "set up a new repo", or equivalent.

## Summary

This skill delegates to the full scaffold procedure in [`skills/scaffold-react-project/SKILL.md`](../scaffold-react-project/SKILL.md) and the detailed runbook in [`knowledge/PROJECT_SCAFFOLD.md`](../../knowledge/PROJECT_SCAFFOLD.md).

**The key difference from the source plugin:** there is no restricted project name prefix, no registration in any external registry, and GitHub Pages is open (no password) — this is a personal plugin for individual work.

## At a glance

1. GitHub auth gate (`gh auth status`)
2. Ask project name (any kebab-case)
3. Ask UI library: shadcn/ui | MUI v6 | Ant Design | Chakra UI | No library | Other
4. Scaffold: Vite + TypeScript + React + chosen library + styled-components
5. Full folder skeleton: `components/` `pages/` `hooks/` `utils/` `assets/` `data/` `services/`
6. 4-file folder pattern per component (Law 12)
7. CI: ESLint + tsc + Vitest + Playwright
8. GitHub Pages preview (no password)
9. `PROJECT_KNOWLEDGE.md` + local `CLAUDE.md`
10. First commit on a branch + PR
11. Spawn `npm run dev` → `Preview:` footer

After scaffold: user triggers `start feature` when ready to begin work.
