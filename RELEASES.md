# Design Forge Releases

---

## v1.1.0 — June 7, 2026

**New Laws (21–24): Senior-engineer thinking**
- Law 21: No bloated code (YAGNI) — ask before over-engineering
- Law 22: Edge case thinking before code — identify 3-5 edge cases upfront
- Law 23: Verify before claiming — no hallucination, check the code first
- Law 24: Question and reason before executing — why, simpler way, best practice

**New law (5a): Check existing PRs**
- Before branching, check if open/merged PR exists for same work
- Update existing PR instead of creating duplicate
- Prevents lost effort when resuming after quota limits

**Project Registry (Law 20)**: Auto-register new projects
- Detect unregistered project at session start
- Auto-open issue → branch → register in projects.yaml → PR workflow
- No manual registration step required
- Every project gets locked localhost port

**Documentation**: Auto-registration link in PROJECT_KNOWLEDGE.md template

---

## v1.0.0 — June 6, 2026

**Initial Design Forge release**
- Forked from Digital.ai UX Claude Laws v0.56.0
- Stripped all dot-components specifics (DotThemeProvider, DotIcon, DotIllustration, Code Connect, design tokens, Agility integration)
- Made UI-library-agnostic: shadcn/ui, MUI, Ant Design, Chakra UI, or local components
- Renamed: `dforge-update` CLI, `~/.design-forge` clone dir
- Kept: 20 binding laws, 4-file component pattern, no-inline-styles rule, methodology (research, UX writing, handoff, feature workflow, fullstack workflow)
- Three install paths: plugin, install.sh (global memory), Copilot custom instructions
- Five personas: Frontend, Fullstack, Design, Research, Pendo Analyst
- Project registry with locked localhost ports per project
