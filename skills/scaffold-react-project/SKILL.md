---
name: scaffold-react-project
description: Scaffold a new React + TypeScript project with library choice (shadcn/ui, MUI v6, Ant Design, Chakra UI, local styled-components, or user-specified) and deployment choice (Netlify recommended, GitHub Pages, or none). Sets up Vite + TypeScript + the chosen library + styled-components base layer + 4-file component pattern + ESLint + tsc + Vitest + Playwright + deployment config. Invoke when the user says "new project", "scaffold a project", "create a React app", "set up a new repo", or equivalent. Do NOT invoke for adding features to an existing project.
---

# Scaffold React Project

This skill implements the `new project` trigger per [`knowledge/PROJECT_SCAFFOLD.md`](../../knowledge/PROJECT_SCAFFOLD.md). The full runbook lives there; this SKILL.md is the plugin-path entry point.

## Step 0 — GitHub auth gate

```bash
gh auth status
```

If not authenticated, run `gh auth login`. Required scopes: `repo`, `workflow`.

## Step 1 — Ask project name

Any kebab-case name. No prefix restrictions.

## Step 2 — Ask UI library choice

Run **one `AskUserQuestion` call** with these options:

```ts
AskUserQuestion({
  questions: [
    {
      header: "UI library",
      question: "Which UI library would you like to use for this project?",
      options: [
        {
          label: "shadcn/ui",
          description: "Radix primitives + Tailwind CSS. Copy-paste components, fully customizable. Best for custom designs."
        },
        {
          label: "Material UI (MUI v6)",
          description: "Google Material Design. Comprehensive component set with theming. Best for data-heavy apps."
        },
        {
          label: "Ant Design",
          description: "Enterprise-grade, batteries-included. Rich component set. Best for admin/B2B apps."
        },
        {
          label: "Chakra UI",
          description: "Accessible, composable, style-prop-driven. Best for rapid accessible UI."
        },
        {
          label: "No library — local components only",
          description: "Pure styled-components with a custom theme object. Maximum control, more initial work."
        },
        {
          label: "Other — I'll type the name",
          description: "Claude will research the library on npm, find its install command, and set it up correctly."
        }
      ],
      multiSelect: false
    }
  ]
});
```

Wait for the answer before any file creation.

## Step 3 — Install base stack

Always:

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
```

Then library-specific install (see [`knowledge/PROJECT_SCAFFOLD.md §3`](../../knowledge/PROJECT_SCAFFOLD.md)):

### shadcn/ui

```bash
npm install styled-components @types/styled-components
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npx shadcn@latest init
```

Add path alias to `vite.config.ts` and `tsconfig.json`.

### MUI v6

```bash
npm install @mui/material @emotion/react @emotion/styled
npm install styled-components @types/styled-components
```

Root wrapper: `ThemeProvider` from `@mui/material/styles` + `CssBaseline`.

### Ant Design

```bash
npm install antd
npm install styled-components @types/styled-components
```

Root wrapper: `ConfigProvider` from `antd`.

### Chakra UI

```bash
npm install @chakra-ui/react
npm install styled-components @types/styled-components
```

Root wrapper: `ChakraProvider` from `@chakra-ui/react`.

### No library — local only

```bash
npm install styled-components @types/styled-components
```

Create `src/theme.ts` with a custom theme object (colors, spacing, typography, radii).

Root wrapper: `ThemeProvider` from `styled-components`.

### Other library

1. Run `npm info <library-name> description version` to find the package.
2. Check the library's README/docs for install command and required peer deps.
3. Find the root provider/wrapper pattern from the docs.
4. Report findings to the user and ask for confirmation before installing.
5. Proceed with install + setup using the same pattern as above.

## Step 4 — Create folder skeleton

```
src/
├── components/
│   └── ExampleCard/
│       ├── ExampleCard.tsx
│       ├── ExampleCard.styles.ts
│       ├── ExampleCard.types.ts
│       ├── ExampleCard.test.tsx
│       └── index.ts
├── pages/
│   └── Home/
│       ├── Home.tsx
│       ├── Home.styles.ts
│       ├── Home.types.ts
│       └── index.ts
├── hooks/
├── utils/
├── assets/
├── data/
├── services/
│   └── storage.ts
├── App.tsx
└── main.tsx
tests/
└── e2e/
    └── smoke.spec.ts
```

## Step 5 — Install dev tools

```bash
npm install -D \
  eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser \
  eslint-plugin-react eslint-plugin-react-hooks \
  prettier eslint-config-prettier \
  vitest @vitest/ui jsdom @testing-library/react @testing-library/jest-dom vitest-axe \
  @playwright/test @axe-core/playwright
```

## Step 6 — Write config files

Create:
- `.eslintrc.cjs`
- `.prettierrc`
- `tsconfig.json`
- `vite.config.ts`
- `vitest.config.ts`
- `playwright.config.ts`

Add scripts to `package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext .ts,.tsx",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "test:e2e": "playwright test",
    "ci": "npm run lint && npm run typecheck && npm run test && npm run build"
  }
}
```

## Step 7 — CI + Pages workflows

Create `.github/workflows/ci.yml` (lint + typecheck + test + build + e2e). For deployment:
- **Netlify**: add `netlify.toml` to the project root (see `§4b` in `PROJECT_SCAFFOLD.md`). No pages workflow needed — Netlify handles deployment and PR previews automatically.
- **GitHub Pages**: add `.github/workflows/pages.yml` (see `§4a` in `PROJECT_SCAFFOLD.md`).
- **None**: skip deployment config entirely.

See full workflow YAML in [`knowledge/PROJECT_SCAFFOLD.md §4`](../../knowledge/PROJECT_SCAFFOLD.md).

## Step 8 — Project files

Create:
- `PROJECT_KNOWLEDGE.md` from the template in [`knowledge/PROJECT_SCAFFOLD.md §7`](../../knowledge/PROJECT_SCAFFOLD.md)
- Local `CLAUDE.md` containing `@./PROJECT_KNOWLEDGE.md`

## Step 9 — First commit

```bash
git init
git add -A
git commit -m "chore: scaffold <name> project with <library>"
gh repo create <name> --public --source=. --push
```

## Step 10 — Spawn preview

```bash
npm run dev &
```

Capture the `Local:` URL from Vite's output. From this response onward, every reply ends with:

```
Preview: http://localhost:5173/  ·  status: up
```

## Step 11 — Confirm

Report to the user:
- Project name + chosen library
- GitHub repo URL
- Preview URL
- Next step: `start feature` when ready to begin work
