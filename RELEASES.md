# Design Forge Releases

---

## v2.6.0 — June 15, 2026

### Lazy-loaded knowledge — ~73% smaller session start
- Only the binding laws (`CLAUDE_LAWS.md`) load at session start now
- The 8 knowledge files load **on demand** — Claude reads each one only when its trigger/scope fires (e.g. `PROJECT_SCAFFOLD` on `new project`, `UX_RESEARCH_GUIDE` on `research mode`)
- Session-start context drops from ~40.4K to ~10.7K tokens; each session pulls in only the 1–2 files it actually uses
- No rules changed — only *when* they load

---

## v2.5.0 — June 15, 2026

### Documentation is the team's shared duty (no Docs agent)
- Removed the dedicated **Docs** role (`docs-writer.md`) and the `docs mode` trigger
- Each role now documents its own change as it builds; the **Lead enforces** the doc standards as a gate before review
- Pipeline stages: `planned → building → testing → in-review`; the documentation gate stays (undocumented change = not done), just owned by the team

---

## v2.4.0 — June 12, 2026

### An agent team that works one pipeline
- Personas now compose into a team that runs a single flow: **plan → build → test → document → review → human-merge** ([`TEAM_WORKFLOW.md`](./knowledge/TEAM_WORKFLOW.md))
- **New roles:** Lead (orchestrator), Backend (API/DB/server), **Tester** (writes + runs tests, axe/coverage gate, can block the PR), **Docs** (README/API docs, RELEASES, PROJECT_KNOWLEDGE, handoff)
- Frontend stays the UI builder; Design/Research/Analyst are supporting; `fullstack mode` now activates the Lead
- Two gates before a PR is review-ready: **Tester** (tests pass + axe clean + acceptance criteria met) and **Docs** (change is documented)
- Handoff rides one shared thread — the `PROJECT_KNOWLEDGE §11` Stage column + the PR body — so context isn't re-derived between roles
- New triggers: `team` / `build feature`, `backend mode`, `tester mode`, `docs mode`
- README gains a **"Working with the team"** how-to + a which-agent-for-what table

### Agents obey the rules (Law 29)
- Every role is fully bound by the laws; if it thinks it should do something the rules don't allow or cover, it **stops and asks** instead of acting on its own

### Stronger accessibility testing
- Beyond an axe scan, the Tester now explicitly verifies **tab order, visible focus states, keyboard-only operation, focus management/trap (modals + route change), roles + accessible names, and live regions** (`FULLSTACK_WORKFLOW §8.1`)

---

## v2.3.0 — June 12, 2026

### Deeper Fullstack engineering (FULLSTACK_WORKFLOW §6–§8)
- **Backend §6** — API contracts (contract-first, version breaking changes, contract tests); DB migrations (forward-only + reversible, expand → migrate → contract); observability (OpenTelemetry tracing, structured logs with request IDs, error tracking)
- **Frontend §7** — Core Web Vitals budgets, server-vs-client state (TanStack Query), loading/empty/error/success UI states + error boundaries, accessible forms
- **Testing §8 + Phase 5** — unit / integration / contract / E2E pyramid; RTL component tests, vitest-axe + axe-core/playwright zero-violation gate, MSW API mocking, Playwright E2E, meaningful coverage
- `agents/fullstack.md` references the full checklist

### AGENTS.md
- Added a root `AGENTS.md` shim pointing at the laws, so non-Claude agents (Cursor, Codex, Copilot, Aider, …) inherit Design Forge governance

---

## v2.2.0 — June 9, 2026

### Decks use your own PowerPoint template
- In research/deck mode, Claude asks you to share your `.pptx`/`.potx` template and builds the slides on top of it
- No built-in or third-party theme is ever used; if you have no template, you get a plain neutral 16:9 deck
- Removed the dangling `PPT_TEMPLATE.md` references (the file and `ppt-template/` skill never existed)

### Dependabot for scaffolded projects (Law 10)
- Every new project ships with `.github/dependabot.yml` — weekly update PRs for npm + GitHub Actions
- Applies to all platform tracks (React/Vite, React Native/Expo, Angular)
- This repo also gets a `dependabot.yml` (github-actions ecosystem)

### Stricter process discipline (Laws 2 + 7)
- Law 2: Claude announces what it understood and waits for explicit approval before executing — silence or "ok" is not approval
- Law 7: Claude never merges under any phrasing (no merge button, API, squash/rebase/fast-forward, or local merge); "merge it"/"ship it"/"done" = open/finish the PR and stop

### Update notifications (Law 28)
- At session start, Claude compares your loaded version against the remote and, if a newer one exists, shows: `Design Forge update available: v<x> → v<y>. Run \`update rules\` to pull and reload.`
- One global update reaches every consuming project (they share one `~/.design-forge`). Claude never auto-pulls — it notifies and waits for your go-ahead.
- Fixed the README law count (26 → 28)

---

## v2.1.0 — June 9, 2026

### Analyst persona (replaces Pendo mode)
- `pendo mode` → `analyst mode` — a tool-agnostic product-analytics persona
- Works with whichever analytics MCP is connected: Pendo, Amplitude, Mixpanel, PostHog, FullStory, Contentsquare (Heap/Hotjar), Adobe Analytics, Google Analytics 4, LogRocket, Statsig
- New `knowledge/ANALYTICS_GUIDE.md` with the supported-platform matrix, privacy rules, and Triangulated Insight Brief template — Pendo retained as the worked example
- Renamed `agents/pendo.md` → `agents/analyst.md` and `skills/pendo-analyst/` → `skills/analyst/`

---

## v2.0.0 — June 9, 2026 — Public release

### Public-ready repository
- New professional README explaining what Design Forge is and how anyone can use it
- `projects.yaml` is now gitignored — ships `projects.example.yaml` instead, so no personal project data is published
- `.claude-plugin/marketplace.json` added — the repo is installable via `/plugin marketplace add BojanKocijan/design-forge`

### New: `dry run` mode (Law 26)
- Toggle with `dry run` / `auto git`
- Claude prepares all edits, then prints a copy-paste terminal command block (commit/push/PR/issue) and offers to run it — instead of spending tokens executing git/gh itself
- Never overrides Law 7 (Claude never merges)

### New: plugin-standards compliance (Law 27)
- Codifies that the repo stays a valid, submittable Claude Code plugin
- Manifest + marketplace.json kept valid; skills as `skills/<name>/SKILL.md` with name+description; components at plugin root
- Version must stay in sync across plugin.json, marketplace.json, CLAUDE_LAWS header, and RELEASES on every release
- Quality/security gate (MIT license, README, no secrets, no personal data, CI green) before official-directory submission

### Plugin directory
- Tracking the submission of Design Forge to the official Claude Code plugin directory (`anthropics/claude-plugins-official`)

---

## v1.2.1 — June 9, 2026

### Proactive branch cleanup (Laws 9 + 25)
- Once a PR is merged, Claude deletes the branch (remote + local) without being asked
- Only deletes branches confirmed merged into `main` (`git merge-base --is-ancestor` check)
- Session-start checklist now sweeps orphaned merged branches
- Mirrored in FULLSTACK_WORKFLOW Phase 9

---

## v1.2.0 — June 8, 2026

### Session-start checklist (Law 25)
- Always pull `main` and check open PRs before any code work

### Automatic project wiring (Law 11)
- Laws load globally via `install.sh`; projects only link `@./PROJECT_KNOWLEDGE.md`
- Scaffold writes it for new projects; session-start step 4.6 auto-creates it for existing ones

---

## v1.1.0 — June 7, 2026

### New Laws (21–24): Senior-engineer thinking
- Law 21: No bloated code (YAGNI) — ask before over-engineering
- Law 22: Edge case thinking before code — identify 3-5 edge cases upfront
- Law 23: Verify before claiming — no hallucination, check the code first
- Law 24: Question and reason before executing — why, simpler way, best practice

### New law (5a): Check existing PRs
- Before branching, check if open/merged PR exists for same work
- Update existing PR instead of creating duplicate
- Prevents lost effort when resuming after quota limits

### Project Registry (Law 20): Auto-register new projects
- Detect unregistered project at session start
- Auto-open issue → branch → register in projects.yaml → PR workflow
- No manual registration step required
- Every project gets locked localhost port

**Documentation**: Auto-registration link in PROJECT_KNOWLEDGE.md template

---

## v1.0.0 — June 6, 2026

### Initial Design Forge release
- Library-agnostic from the start: shadcn/ui, MUI, Ant Design, Chakra UI, or local components
- `dforge-update` CLI, `~/.design-forge` clone dir
- 20 binding laws, 4-file component pattern, no-inline-styles rule, methodology (research, UX writing, handoff, feature workflow, fullstack workflow)
- Three install paths: plugin, install.sh (global memory), Copilot custom instructions
- Five personas: Frontend, Fullstack, Design, Research, Analyst
- Project registry with locked localhost ports per project
