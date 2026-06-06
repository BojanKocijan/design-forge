---
name: fullstack-workflow
description: Fullstack developer PR flow — 10 phases (verify env → describe → pre-execution announcement → branch+issue → pair-programming loop → pre-PR checks → PR → review → merge → post-merge), surface support matrix (Claude Code in VS Code/JetBrains/CLI/desktop, Claude.ai web), refuse list, edge cases (hotfix, stacked PRs, revert, draft PR). Invoke when the user activates "fullstack mode", ships production code, modifies CI workflows, or makes architectural decisions.
---

# Fullstack Workflow

Full runbook: [`knowledge/FULLSTACK_WORKFLOW.md`](../../knowledge/FULLSTACK_WORKFLOW.md).

## Activation

```
User: fullstack mode

Claude: Fullstack persona active. I'll announce every change before making it,
wait for confirmation on multi-file edits, and treat every commit as production code.
What are we solving?
```

## The 10 phases

0. Verify environment (node, npm, git, gh auth)
1. Describe the work — wait for go-ahead
2. Pre-execution announcement (Understanding / Updating / Severity / Data layer / Affected / Code change / Branch / Issue)
3. Branch + issue: `git checkout main && git pull && git checkout -b feat/<description>`
4. Pair-programming loop (announce each step → confirm multi-file → make change → run test → report)
5. Pre-PR checks: `npm run lint && npm run typecheck && npm run test && npm run build && npm run test:e2e`
6. Open PR with `gh pr create`
7. Human review
8. **Merge = human action** (Law 7 — Claude never runs `gh pr merge`)
9. Post-merge: `git checkout main && git pull`; close issue if not auto-closed
10. Update `PROJECT_KNOWLEDGE.md` with new components / decisions

## Refuse list

- Silent multi-file edits
- Force-push to `main`
- Skip pre-commit hooks (`--no-verify`)
- Commit credentials / `.env`
- Real PII in mock data
- Delete files without approval
- Skip failing axe assertions
- `style={{}}` in `.tsx`
- Run a merge command
