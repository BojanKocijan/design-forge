---
name: ppt-template
description: Presentation template spec for .pptx generation — 16:9 canvas, configurable brand palette (user provides colors, or generic accessible default is used), system default fonts, standard layout names, research deck → layout map. Invoke when generating a .pptx or a slide outline intended for PowerPoint export.
---

# PPT Template

Full spec: [`knowledge/PPT_TEMPLATE.md`](../../knowledge/PPT_TEMPLATE.md).

## Canvas

16:9 widescreen — 13.33 in × 7.50 in. Do not change.

## Brand colors

**Always ask the user for brand colors first.** If they don't have them or say "use a default", use the generic accessible palette from `knowledge/PPT_TEMPLATE.md §2`.

Minimum needed from the user:
- Primary brand color (used for headers, accents)
- Background color (usually white)
- One secondary accent color

## Typography

Use the system default font (Arial or Calibri) unless the user specifies one. Apply consistently: titles 32–40pt bold, section headers 24–28pt bold, body 18–20pt regular.

## Standard layouts

| Layout | Use |
|---|---|
| Title Slide | Cover, section dividers |
| Title and Content | Standard slide with header + body |
| Two Content | Side-by-side blocks |
| Comparison | Two-column comparison |
| Title Only | Large visual or chart |
| Blank | Full-bleed image or custom |

## Research deck → layout map

| Slide type | Layout |
|---|---|
| Cover | Title Slide |
| Executive summary | Title and Content |
| Single finding | Title and Content |
| Finding + screenshot | Two Content |
| Prioritization matrix | Title Only |
| RICE table | Title and Content |
| Now/Next/Later | Title and Content |
