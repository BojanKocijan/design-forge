# Design Forge Releases

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
