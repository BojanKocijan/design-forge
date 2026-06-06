---
name: frontend-guide
description: Frontend coding standards for React + TypeScript projects under Design Forge governance — 4-file component pattern (Component.tsx + Component.styles.ts + Component.types.ts + index.ts), no-inline-styles rule, styled-components as base styling layer, library-agnostic (shadcn/ui | MUI | Ant Design | Chakra UI | local), WCAG 2.2 AA, mock data defaults, Conventional Commits. Invoke for any React or frontend code task, component creation, styling question, or a11y review.
---

# Frontend Guide

Full guide: [`knowledge/FRONTEND_GUIDE.md`](../../knowledge/FRONTEND_GUIDE.md).

## Key rules (binding)

### 4-file component pattern (Law 12)

Every component lives in its own folder:

```
ComponentName/
├── ComponentName.tsx        # logic + JSX only — no CSS, no styled.* here
├── ComponentName.styles.ts  # every styled-component lives here
├── ComponentName.types.ts   # prop interfaces + related types
└── index.ts                 # barrel re-export
```

### No inline styles

**Banned in `*.tsx` files:**

- `style={{}}` — even for "just one value"
- `css\`...\`` template literals in the component file
- `styled.div\`...\`` declarations in the component file
- CSS module imports
- Tailwind classes used as ad-hoc styling (exception: shadcn/ui projects where Tailwind is the chosen styling system — then utility classes in `.tsx` are allowed, but styled-component overrides still live in `.styles.ts`)

### UI library

**UI library:** chosen at scaffold time (shadcn/ui | MUI | Ant Design | Chakra UI | local styled-components). Claude never switches the library mid-project without explicit approval.

Use the library's components for primitives — buttons, inputs, selects, dialogs, cards. Never roll your own if the library has it.

### Theme tokens only

All colors, spacing, font sizes from the project's theme. Never raw hex / px / font names.

### WCAG 2.2 AA

Every component must pass: 4.5:1 contrast for text, visible focus states, keyboard-accessible, `aria-label` on icon-only buttons.

### Mock data — PII-free (Law 15)

All mock data in `src/data/` must use fictional values: `alice@example.com`, `+1 555-0100`, `Acme Corp`, `1 Fictional Lane`.

### Data layer

Default = mocks + `localStorage`. Graduating to a real DB requires the `Data layer:` line in the pre-execution announcement.
