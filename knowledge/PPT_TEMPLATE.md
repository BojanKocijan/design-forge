# PowerPoint Template Guide — Design Forge

**Version:** 1.0.0
**Last Updated:** 2026-06-06
**Binding:** Yes — when the Research persona produces a `.pptx` or a slide-outline intended for export, slides must conform to this spec unless the user specifies their own brand.

---

## 1. Canvas

| Property | Value |
|---|---|
| Width | 13.33 in (33.9 cm) |
| Height | 7.50 in (19.1 cm) |
| Ratio | **16:9 widescreen** |

All standard layouts in modern `.potx` files are built for 16:9. Do not change canvas size.

---

## 2. Color Palette (generic default)

If the user has not specified a brand palette, use this accessible default:

| Role | Token name | Hex | Use |
|---|---|---|---|
| Text / dark | `text-primary` | `#1a1a2e` | Body text on light backgrounds |
| Background / white | `bg-primary` | `#ffffff` | Primary slide background |
| Brand navy | `brand-dark` | `#16213e` | Cover titles, section dividers |
| Brand accent | `brand-accent` | `#0f3460` | Primary brand color — highlights, icons, CTA |
| Brand blue | `brand-blue` | `#533483` | Links, callouts, secondary brand |
| Teal | `accent-teal` | `#0d7377` | Supporting accent — charts, icons |
| Amber | `accent-amber` | `#f4b942` | Warnings, highlights, roadmap "Now" tier |
| Light blue | `accent-light-blue` | `#14a0c0` | Data visualization, roadmap "Next" tier |

**User-specified brand:** If the user provides brand hex colors (or says "use my company's colors: #..."), use those instead. Ask for the primary color, background, and at least one accent color before generating the deck.

---

## 3. Typography

| Element | Font | Size | Weight |
|---|---|---|---|
| Slide title | System default (Arial or Calibri) | 32–40pt | Bold |
| Section header | System default | 24–28pt | Bold |
| Body text | System default | 18–20pt | Regular |
| Caption / footnote | System default | 12–14pt | Regular |

If the user specifies a font (e.g., "use Inter"), apply it consistently. If not specified, use the system default.

---

## 4. Layout names (commonly available in POTX files)

| Layout | Use |
|---|---|
| Title Slide | Cover slide, section dividers |
| Title and Content | Standard content slide with header + body |
| Two Content | Side-by-side content blocks |
| Comparison | Two-column comparison |
| Title Only | Large visual or chart slide |
| Blank | Full-bleed image or custom layout |
| Content with Caption | Image + explanatory text |
| Picture with Caption | Photo-forward layout |

When generating a deck outline, Claude specifies the layout for each slide by name.

---

## 5. Research deck → layout map

| Slide type | Recommended layout |
|---|---|
| Cover / study title | Title Slide |
| Executive summary (bullets) | Title and Content |
| Single finding with quote | Title and Content |
| Finding with screenshot + quote | Two Content |
| Prioritization matrix (2×2) | Title Only (full-bleed chart) |
| RICE table | Title and Content |
| Now / Next / Later roadmap | Title and Content |
| Limitations + next steps | Title and Content |

---

## 6. Chart guidance

For data visualization in slides:

- **Bar charts** — for comparing values across categories.
- **Line charts** — for trends over time.
- **Pie charts** — only when showing parts of a whole with ≤6 categories.
- **2×2 matrix** — for prioritization (Reach vs. Impact, Effort vs. Value).
- **Color:** use `brand-accent` for the primary series, `accent-teal` and `accent-amber` for secondary series. Never use more than 5 colors in one chart.

---

## Changelog

- **1.0.0 (2026-06-06)** — Initial Design Forge release. Adapted from Digital.ai UX PPT_TEMPLATE v1.1.0. Removed: Digital.ai brand colors and `Corporate deck 2026.potx` specifics, all 44 validated layout names (replaced with commonly available subset), Arial font requirement. Replaced with user-configurable brand palette (generic accessible default provided), system-default fonts, and instructions to accept user-specified brand colors. Kept: canvas spec (16:9), layout-name table, research deck → layout map, chart guidance.
