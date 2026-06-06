---
name: ux-research-guide
description: UX research methodology — transcript ingestion (upload/paste/SharePoint), PII redaction, per-call summaries, thematic clustering, RICE + MoSCoW prioritization, JTBD framing, 6-slide outcome deck (default) and 12–18 slide full research deck. Invoke on "research mode", "analyze transcripts", "research synthesis", or any qualitative research request.
---

# UX Research Guide

Full methodology: [`knowledge/UX_RESEARCH_GUIDE.md`](../../knowledge/UX_RESEARCH_GUIDE.md).

## Activation

`research mode` → 6-slide outcome deck (default)
`research mode full` → 12–18 slide full research deck

## Default cadence

1. Confirm scope (date range, participants, research question).
2. Ask how transcripts arrive (upload / paste / SharePoint).
3. Produce per-call summaries (PII-redacted, ≤200 words each).
4. Cluster into themes (≥3 calls = a theme).
5. Compute RICE per recommendation.
6. Generate deck outline → optionally render `.pptx` via the `pptx` skill.
7. End with open questions + suggested follow-up research.

## PII redaction (non-negotiable)

| Type | Token |
|---|---|
| Name | `[REDACTED:NAME]` |
| Email | `[REDACTED:EMAIL]` |
| Phone | `[REDACTED:PHONE]` |
| Company | `[REDACTED:COMPANY]` |
| ID | `[REDACTED:ID]` |

Aggregate by default: "3 enterprise admins said X" not "[REDACTED:NAME] said X".

## RICE scoring

`RICE = (Reach × Impact × Confidence) / Effort`

- Reach: % of users affected per quarter
- Impact: 0.25 / 0.5 / 1 / 2 / 3
- Confidence: 50% / 80% / 100%
- Effort: person-weeks

## 6-slide outcome deck structure

1. Title + scope
2. One-page summary (3–4 findings)
3. Finding 1 (quote + action + screenshot)
4. Finding 2
5. Finding 3
6. Now / Next / Later roadmap
