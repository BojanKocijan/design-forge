# Project Scaffold — Design Forge React App

**Version:** 1.0.0
**Last Updated:** 2026-06-06
**Applies to:** Every new React project under Design Forge governance
**Binding:** Yes — this file is a law (see [`CLAUDE_LAWS.md`](../CLAUDE_LAWS.md) Laws 7, 10, 11, 12, 13, 14, 15, 18). Claude must follow it whenever the user says **"new project"** (or equivalent).

---

## 0. GitHub authentication gate (always first)

Before a single file is created, Claude verifies the user is authenticated to GitHub.

```bash
gh auth status
```

If not authenticated:

```bash
gh auth login
```

Required scopes: `repo` (create repo, push, open issues + PRs), `workflow` (push `.github/workflows/`).

After authentication, confirm identity:

```bash
gh api user --jq '"Authenticated as: " + .login'
```

---

## 1. When the user says "new project"

**Step 0 — Ask which kind of project.**

> *"Two project types are supported:*
> 1. **UX project** (default) — mockups, prototypes, UI exploration. Uses the scaffold below. Frontend persona stays default.
> 2. **Fullstack project** — production code, real backend, real DB. Activates the Fullstack persona. Same governance (CI, branch protection) but stack is per-project (Claude will ask for backend choice).
>
> *Which one?"*

Wait for the answer.

**Step 1 — Ask for project name.**

Any kebab-case name (e.g. `my-dashboard`, `analytics-prototype`). Not restricted to any prefix.

**Step 2 — Ask for UI library choice.** Run an `AskUserQuestion` call:

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
          description: "Google Material Design. Comprehensive component library with theming. Best for data-heavy apps."
        },
        {
          label: "Ant Design",
          description: "Enterprise-grade, batteries-included. Rich component set, opinionated design. Best for admin/B2B."
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
          description: "Claude will research the library, find its install command, and set it up correctly."
        }
      ],
      multiSelect: false
    }
  ]
});
```

Wait for the answer before proceeding.

**Step 3 — Ask where GitHub Issues should be created.**

> *"Which GitHub repo should Claude open issues in for this project? This is where branch-linked issues, feature tracking, and handoff tickets will go.*
> *Format: `owner/repo` (e.g. `bojankocijan/my-dashboard`). Press Enter to use the current project repo (default)."*

Store the answer in `PROJECT_KNOWLEDGE.md §9 GitHub Issues repo`. If the user presses Enter or says "this repo" / "same repo", default to the current project repo. Claude will use `gh issue create --repo <owner/repo>` for all issue creation.

**Step 4 — Ask for deployment target.** Run an `AskUserQuestion` call:

```ts
AskUserQuestion({
  questions: [
    {
      header: "Deployment",
      question: "Where should this project be deployed for preview?",
      options: [
        {
          label: "Netlify",
          description: "Recommended. Continuous deployment from main, instant preview URLs per PR, free tier. Requires a Netlify account."
        },
        {
          label: "GitHub Pages",
          description: "No extra account needed. Deploys from main via GitHub Actions. Single URL, no per-PR previews."
        },
        {
          label: "None — I'll set up deployment later",
          description: "Skip deployment setup. Claude will scaffold the app only; you wire deployment yourself."
        }
      ],
      multiSelect: false
    }
  ]
});
```

- **Netlify**: add a `netlify.toml` to the project root and a `netlify.yml` CI workflow. No `pages.yml` workflow. See §4b.
- **GitHub Pages**: add `.github/workflows/pages.yml` as before. See §4a.
- **None**: skip all deployment config.

Store the choice in `PROJECT_KNOWLEDGE.md §10 Deployment`.

---

## 2. Scaffold procedure (all library choices)

### Base stack (always)

1. Vite + TypeScript + React
2. styled-components (base styling layer; replaced by Tailwind if the chosen library requires it)
3. ESLint + TypeScript ESLint + Prettier
4. Vitest + React Testing Library + vitest-axe (unit + component + a11y)
5. Playwright + @axe-core/playwright (E2E + full-page accessibility)
6. Deployment: per user choice (Netlify / GitHub Pages / none)

### Full folder skeleton

```
<project-name>/
├── src/
│   ├── components/
│   │   └── ExampleCard/
│   │       ├── ExampleCard.tsx
│   │       ├── ExampleCard.styles.ts
│   │       ├── ExampleCard.types.ts
│   │       ├── ExampleCard.test.tsx
│   │       └── index.ts
│   ├── pages/
│   │   └── Home/
│   │       ├── Home.tsx
│   │       ├── Home.styles.ts
│   │       ├── Home.types.ts
│   │       └── index.ts
│   ├── hooks/
│   ├── utils/
│   ├── assets/
│   ├── data/
│   ├── services/
│   │   └── storage.ts
│   ├── App.tsx
│   └── main.tsx
├── tests/
│   └── e2e/
│       └── smoke.spec.ts
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── pages.yml          ← GitHub Pages only
├── netlify.toml               ← Netlify only
├── .eslintrc.cjs
├── .prettierrc
├── tsconfig.json
├── vite.config.ts
├── vitest.config.ts
├── playwright.config.ts
├── PROJECT_KNOWLEDGE.md
└── CLAUDE.md
```

---

## 3. Library-specific setup

### shadcn/ui

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
npm install styled-components @types/styled-components

# Tailwind (required for shadcn/ui)
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

# shadcn/ui
npx shadcn@latest init
# answer prompts: TypeScript yes, path aliases yes
```

`vite.config.ts` path alias:

```ts
import path from 'path';
export default {
  resolve: { alias: { '@': path.resolve(__dirname, './src') } }
}
```

App wrapper — no global provider needed; use `cn()` utility from `lib/utils.ts` (auto-generated by shadcn init).

For Tailwind, use utility classes in `.tsx`. For custom overrides, use styled-components in `.styles.ts`.

---

### Material UI (MUI v6)

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
npm install @mui/material @emotion/react @emotion/styled
npm install styled-components @types/styled-components
```

Root wrapper:

```tsx
import { ThemeProvider, createTheme } from '@mui/material/styles';
import CssBaseline from '@mui/material/CssBaseline';

const theme = createTheme({
  // customize here
});

function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* app content */}
    </ThemeProvider>
  );
}
```

---

### Ant Design

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
npm install antd
npm install styled-components @types/styled-components
```

Root wrapper:

```tsx
import { ConfigProvider } from 'antd';

function App() {
  return (
    <ConfigProvider theme={{ token: { /* customize */ } }}>
      {/* app content */}
    </ConfigProvider>
  );
}
```

---

### Chakra UI

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
npm install @chakra-ui/react
npm install styled-components @types/styled-components
```

Root wrapper:

```tsx
import { ChakraProvider } from '@chakra-ui/react';

function App() {
  return (
    <ChakraProvider>
      {/* app content */}
    </ChakraProvider>
  );
}
```

---

### No library — local components only

```bash
npm create vite@latest <name> -- --template react-ts
cd <name>
npm install
npm install styled-components @types/styled-components
```

Create `src/theme.ts` with a custom theme object:

```ts
export const theme = {
  colors: {
    primary: '#3b82f6',
    background: '#ffffff',
    surface: '#f8fafc',
    text: '#0f172a',
    textSecondary: '#64748b',
    border: '#e2e8f0',
    error: '#ef4444',
    success: '#22c55e',
  },
  spacing: {
    1: '4px', 2: '8px', 3: '12px', 4: '16px',
    5: '20px', 6: '24px', 8: '32px', 10: '40px', 12: '48px',
  },
  typography: {
    fontFamily: 'Inter, system-ui, sans-serif',
    sizes: { sm: '0.875rem', md: '1rem', lg: '1.125rem', xl: '1.25rem' },
    weights: { regular: 400, medium: 500, bold: 700 },
  },
  radii: { sm: '4px', md: '8px', lg: '12px', full: '9999px' },
};

export type Theme = typeof theme;
```

---

### Other library

When the user types a library name not in the preset list:

1. Search npm for the library: `npm info <library> description version`
2. Find its official install command and required peer deps.
3. Find its root provider/wrapper component from docs.
4. Set it up following the same pattern as above (root wrapper, custom theme if available).
5. Report what was found and ask for confirmation before installing.

---

## 4. CI workflows

### `.github/workflows/ci.yml`

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:

jobs:
  ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm run test
      - run: npm run build
      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium
      - run: npm run test:e2e
```

### §4a — GitHub Pages: `.github/workflows/pages.yml`

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run build
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
      - id: deployment
        uses: actions/deploy-pages@v4
```

Enable GitHub Pages in the repo settings: **Settings → Pages → Source: GitHub Actions**.

---

### §4b — Netlify: `netlify.toml` + deploy setup

**`netlify.toml`** in the project root:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

The `[[redirects]]` rule is required for client-side routing (React Router, etc.) — without it, refreshing any non-root URL returns a 404.

**Setup steps (one-time, done by the user):**
1. Go to [app.netlify.com](https://app.netlify.com) → **Add new site → Import an existing project**
2. Connect to GitHub, pick the repo
3. Build command: `npm run build` · Publish directory: `dist` (Netlify auto-detects from `netlify.toml`)
4. Click **Deploy site**

Netlify automatically creates **preview deployments for every PR** — no extra workflow needed. The main branch deploys to the site's production URL.

**No CI workflow file needed for deployment** — Netlify handles it. The existing `ci.yml` (lint + typecheck + tests) still runs on GitHub Actions independently.

---

## 5. `package.json` scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "typecheck": "tsc --noEmit",
    "test": "vitest run",
    "test:watch": "vitest",
    "test:e2e": "playwright test",
    "ci": "npm run lint && npm run typecheck && npm run test && npm run build"
  }
}
```

---

## 6. Post-scaffold steps

After the scaffold lands:

1. **Spawn `npm run dev` in the background** (Law 18) and capture Vite's "Local:" URL. From this response onward, every reply ends with `Preview: <url> · status: up`.
2. **Create `PROJECT_KNOWLEDGE.md`** from the template (see §7).
3. **Create local `CLAUDE.md`** with `@./PROJECT_KNOWLEDGE.md`.
4. Commit on a branch: `chore: scaffold <name> project`.
5. Push and open the first PR.
6. **After scaffold, the user triggers `start feature`** — scaffolding does NOT auto-start a feature.

---

## 7. `PROJECT_KNOWLEDGE.md` template

```markdown
# PROJECT_KNOWLEDGE — <project-name>

## 1. Project purpose
<one-line description>

## 2. Target users
<who this is for>

## 3. Project-specific components
<!-- Components not in the chosen UI library -->
| Component | Path | Purpose |
|---|---|---|

## 4. Local wrappers / interim overrides
<!-- Overrides of library defaults until upstream fixes land -->
| Wrapper | Path | Why | Ticket |
|---|---|---|---|

## 5. Architectural decisions
| Date | Decision | Why |
|---|---|---|

## 6. Open questions / known issues
- [ ] <question or issue>

## 7. Upstream PRs opened from this project
| Date | Repo | PR | Status |
|---|---|---|---|

## 8. Data layer
- Default: mocks + localStorage
- DB: none (until upgraded)

## 9. GitHub Issues repo
<!-- Where Claude opens issues: owner/repo format. Defaults to this repo. -->
none yet

## 10. Deployment
<!-- Netlify | GitHub Pages | none -->
none yet

## 11. Handoffs shipped
| Date | ID | Title | PR |
|---|---|---|---|

## 12. Active feature
| ID | Title | Status | Branch | JTBD |
|---|---|---|---|---|

### Paused features
| ID | Title | Status | Branch | JTBD |
|---|---|---|---|---|

## 12. Feature audit log
| ID | Title | Handed off | Notes |
|---|---|---|---|
```

---

## Changelog

- **1.0.0 (2026-06-06)** — Initial Design Forge release. Adapted from Digital.ai UX PROJECT_SCAFFOLD v1.21.0. Replaced locked dot-components stack with library-agnostic approach: user chooses shadcn/ui, MUI, Ant Design, Chakra UI, local, or other at scaffold time. Removed: digital-ai org registration, projects.yaml, password-protected Pages (now open), Copilot instructions, CODEOWNERS, Agility integration, cross-project patterns registry. Kept: Vite + TypeScript + React base, 4-file component pattern, CI (ESLint + tsc + Vitest + Playwright), GitHub Pages, PROJECT_KNOWLEDGE.md, local CLAUDE.md, branch-protection requirements, feature workflow, Law 18 localhost preview.
