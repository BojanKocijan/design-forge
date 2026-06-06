---
name: skills-matrix
description: Engineering + design competency matrix for Design Forge — layout (Flexbox/Grid/container queries/mobile-first), design (visual hierarchy, typography, color, spacing), UX writing (10 rules), React (state, performance, testing), WCAG 2.2 AA, forms, git hygiene, motion, error handling, performance, developer handoff (13-section template). Invoke when the user asks about best practices, competency standards, or "how should I handle X" in any of these areas.
---

# Skills Matrix

Full reference: [`knowledge/SKILLS.md`](../../knowledge/SKILLS.md).

## Quick reference

### Layout
- Flexbox for 1D, CSS Grid for 2D, container queries for component-responsive layouts.
- Mobile-first. Gaps not margins between flex children. No fixed widths except for inherently fixed UI.

### Design critique (8 steps)
Visual hierarchy → Typography → Spacing → Color → UI component consistency → Accessibility → Responsive → Content quality.

Each finding: numbered, severity (**cosmetic / minor / major / catastrophe**), concrete suggestion.

### UX writing (10 rules)
Sentence case · Active voice · Action-first buttons · Specific not generic · No jargon · Empty states next-action · Error messages name problem + fix · Confirmation dialogs use action label · Loading states specific · Tooltips supplementary.

### React
- State: `useState` / `useReducer` (local), Context / Zustand (shared), TanStack Query (server).
- Performance: `useMemo` / `useCallback`, lazy routes, virtualize long lists.
- Testing: colocated `.test.tsx` with vitest-axe axe assertion on every component.

### WCAG 2.2 AA
4.5:1 text contrast · 3:1 large text + UI components · keyboard accessible · visible focus · aria-label on icon-only buttons.

### Git hygiene
Conventional Commits · branch naming `feat/` `fix/` `refactor/` · PRs only · one logical change per commit · PR description includes why + how to test + screenshots.

### Developer handoff (13 sections)
Summary · Figma links · JTBD · Acceptance criteria · UI components used · Interaction spec · Responsive behavior · Accessibility notes · Analytics (optional) · Data layer · Open questions · Implementation hints · Definition of done.
