---
name: claude-laws
description: Master binding laws for ANY Design Forge work — pre-execution announcement, branch+issue first, English-only, no direct push to main, no file deletion, Conventional Commits, secret scanning, mock data PII rules, write-access discipline, and the knowledge-file hierarchy that determines which other skill to invoke. AUTO-LOAD on every Design Forge session — these laws override everything else and apply universally to frontend, fullstack, design, research, and analyst personas. Whenever the user is working in any repository governed by design-forge, this skill must be in context.
---

<!-- markdownlint-disable-file MD025 MD041 -->

# Master Claude Laws — Design Forge

**Version:** 1.0.0
**Last Updated:** 2026-06-06
**Rules Repo:** https://github.com/bojankocijan/design-forge

See the full laws in [`CLAUDE_LAWS.md`](../../CLAUDE_LAWS.md) at the repo root.

This skill is the plugin-path mirror of `CLAUDE_LAWS.md` so plugin-mode Claude has the master governance layer in context. The content below is a condensed summary — for the authoritative text, read `CLAUDE_LAWS.md`.

---

## Prime Directives

1. **English only.**
2. **No code executes without disclosure** (pre-execution announcement).
3. **Rules repo is consulted first.**
4. **All knowledge files are binding.**
5. **Pull `main`, then branch + issue before code.**
6. **Ask to clarify, not to iterate.**
7. **No direct push to `main`. Merge is always a human action.**
8. **No file deletion** without explicit human approval.
9. **Close issues, link to PRs, delete branch after merge.**
10. **Every new project ships with CI and tests** (ESLint, tsc, Vitest, Playwright, GitHub Pages).
11. **Every new project has a living `PROJECT_KNOWLEDGE.md` and local `CLAUDE.md`.**
12. **No inline styles in component files.** 4-file folder pattern: `Component.tsx` + `Component.styles.ts` + `Component.types.ts` + `index.ts`.
13. **Conventional Commits** on every commit.
14. **Scan staged diff for secrets before every commit.**
15. **Mock data must contain no real PII.**
16. **Use current user's own credentials.**
17. **Triage-first** — one `AskUserQuestion` with up to 4 questions before non-trivial UI.
18. **Localhost preview must stay running** — every response ends with `Preview: <url> · status: <...>`.
19. **Design fidelity** — only add visual elements explicitly present in the design.

## Session-start confirmation format

```
Rules loaded: DESIGN_FORGE v1.0.0
Project: <repo-name>
Persona: <Frontend | Fullstack | Design | Research | Analyst>
GitHub: <username | unauthenticated>
Knowledge: PROJECT_KNOWLEDGE.md — <one-line summary>   ← omit if absent
Feature: <id · title · status>   ← omit if none
Ready.
```
