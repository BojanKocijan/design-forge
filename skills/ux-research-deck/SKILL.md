---
name: ux-research-deck
description: Generate a UX research presentation deck outline — 6-slide outcome deck (default) or 12–18 slide full research deck. Uses the project's brand colors if specified, otherwise the Design Forge generic palette. Can render a real .pptx via the pptx skill. Invoke after research synthesis is complete and the user wants to create slides or a deck outline.
---

# UX Research Deck

## When to invoke

After research synthesis (themes, RICE scores, MoSCoW tiers are known) and the user asks to "create slides", "make a deck", "build the presentation", or "export to PowerPoint".

## Deck formats

### 6-slide outcome deck (default)

| Slide | Layout | Content |
|---|---|---|
| 1 | Title Slide | Study name · date range · participant count · research question |
| 2 | Title and Content | Executive summary: 3–4 bullet findings, one recommended action each |
| 3 | Two Content | Finding 1: theme + evidence count + quote + action + screenshot/graph |
| 4 | Two Content | Finding 2 |
| 5 | Two Content | Finding 3 |
| 6 | Title and Content | Now / Next / Later roadmap: Must → Should → Could |

Evidence appendix (full RICE math, quote ledger, audit log) → separate `<study-id>-evidence.md` linked from slide 1.

### Full 12–18 slide research deck (`research mode full`)

| Slides | Content |
|---|---|
| 1 | Cover (study name, date, researcher) |
| 2 | Research questions |
| 3 | Methodology (session format, participant criteria, dates) |
| 4 | Participants table (roles, seniority, usage — no PII) |
| 5 | Executive summary (3 findings, 3 actions) |
| 6–13 | One slide per finding (theme · evidence count · quote · RICE · action · screenshot) |
| 14 | Prioritization matrix (Reach × Impact 2×2) |
| 15 | RICE table |
| 16 | MoSCoW roadmap |
| 17 | Limitations + next steps |
| 18 | Appendix pointer → `<study-id>-evidence.md` |

## Quality gates before delivering

1. Every non-obvious claim has a citation: `(call_id @ timestamp)`.
2. No raw PII in any slide text or chart label.
3. Counter-evidence represented (at least one mention of dissenting data).
4. RICE scores explained, not just printed.
5. Recommended actions are specific (not "improve UX").

## Rendering a real .pptx

If the user wants an actual file (not just an outline), invoke the `pptx` skill after producing the outline. Pass:

- The slide outline (layout + content per slide)
- Brand colors (from the user or the `PPT_TEMPLATE.md` generic palette)
- Font choice (system default if not specified)
