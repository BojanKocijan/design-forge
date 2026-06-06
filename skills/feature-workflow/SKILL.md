---
name: feature-workflow
description: Designer-facing feature lifecycle — 3-question triage at "start feature", bootstrap vs steady-state modes (any issue tracker: GitHub, Linear, Jira, or synthetic bootstrap ID), 5 states with transitions (in-design → in-mockup → in-prototype → in-review → ready-for-handoff → handed-off), one-active-at-a-time rule with explicit pause/resume exception path. Drives PROJECT_KNOWLEDGE.md §11 Active feature and §12 Feature audit log. Invoke on "start feature", "pause feature", "resume feature", "finish feature", or any equivalent paraphrase.
---

# Feature Workflow

Full runbook: [`knowledge/FEATURE_WORKFLOW.md`](../../knowledge/FEATURE_WORKFLOW.md).

## Trigger phrases

| Phrase | Action |
|---|---|
| `start feature` | Run 3-question triage; write §11 Active feature row |
| `pause feature` | Move §11 Active to §11 Paused; clear Active |
| `resume feature` | List paused features; user picks; restore to Active |
| `finish feature` | If `ready-for-handoff`, run `handoff <id>`; archive to §12; clear §11 Active |

## The 3-question triage

**Q1.** Is this a new feature, improvement, or refactor?
→ Sets branch prefix: `feat/`, `fix/`, `refactor/`

**Q2.** Do you have an issue ID for this work (GitHub #, Linear, Jira, etc.)?
→ If yes: note the ID as-is. If no: generate `<PROJECT>-BOOT-<NNN>`.

**Q3.** One-line JTBD: "As a <user>, I want to <action> so that <outcome>."

Write to `PROJECT_KNOWLEDGE.md §11 Active feature` immediately.

## States

`in-design` → `in-mockup` → `in-prototype` → `in-review` → `ready-for-handoff` → `handed-off`

Claude updates the status row whenever a transition is implied or requested.

## One active feature at a time

If a feature is already active and not `handed-off`, Claude asks to pause it before starting a new one.
