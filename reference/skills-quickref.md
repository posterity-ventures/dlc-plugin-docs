# Skills Quick Reference

Every skill in `.claude/skills/` on one page. For the full workflow of any skill, follow its link to the skills-guide.

| Skill | Purpose | Example trigger | Typical duration |
|-------|---------|-----------------|------------------|
| [orchestrate-sdlc](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/orchestrator.md) | Top-level SDLC dispatcher. Routes through sdlc-intake / sdlc-implement / sdlc-deliver / sdlc-finalize | `/orchestrate-sdlc <feature>` | end-to-end (30 min – several hours) |
| [sdlc-intake](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/sdlc-intake.md) | Phases 1–2c: requirements, scope assessment, pre-design reviews, tech design | dispatched by orchestrate-sdlc | 5–30 min |
| [sdlc-implement](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/sdlc-implement.md) | Phase 3 epic loop: plan → parallel coding subagents → tests → coverage gate → polish → commit | dispatched by orchestrate-sdlc | 10–60 min per epic |
| [sdlc-deliver](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/sdlc-deliver.md) | Phases 4–6: PR creation, review, stabilization, conditional E2E | dispatched by orchestrate-sdlc | 10–60 min |
| [sdlc-finalize](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/sdlc-finalize.md) | Phases 7–8 wrapper around finalize-sdlc: cleanup, post-merge, issue close | dispatched by orchestrate-sdlc | 5–20 min |
| [product-discovery](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/product-discovery.md) | Structured discovery for raw product ideas: runs a chosen framework (JTBD + Lean Canvas, SWOT + Kano, Porter's Five Forces + BMC, Opportunity Solution Tree, Working Backward/PR-FAQ) and produces a `discovery.md` brief that feeds `/analyze-requirements` | `/discover <idea>` or `/product-discovery` | 10–30 min |
| [analyze-requirements](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/analyze-requirements.md) | Turn a plain-language brief into a full PRD with acceptance criteria | `/analyze-requirements`, or first phase of orchestrate-sdlc | 2–10 min |
| [create-issues](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/create-issues.md) | Decompose a PRD into GitHub issues (epic + children) | `/create-issues` | 1–5 min |
| [produce-tech-design](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/produce-tech-design.md) | Write a tech design with decisions, work-item DAG, epic breakdown | Phase 2c of orchestrate-sdlc | 3–10 min |
| [review-security](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-security.md) | Pre-design security review triggered by auth/PII/endpoint/infra changes | Phase 2b of orchestrate-sdlc when trigger matches | 2–8 min |
| [review-ux](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-ux.md) | Pre-design UX review triggered by UI/user-workflow changes; captures live screenshots | Phase 2b of orchestrate-sdlc when trigger matches | 3–10 min |
| [review-a11y](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-a11y.md) | Accessibility review (WCAG 2.1 AA): keyboard nav, screen reader, color contrast, focus, ARIA, semantic HTML. Uses Playwright + axe-core | Phase 2b of orchestrate-sdlc when UI triggers match | 3–10 min |
| [review-performance](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-performance.md) | Static performance review: N+1 queries, hot-path complexity, render blockers, bundle size, caching, async misuse | Phase 2b of orchestrate-sdlc when perf-sensitive triggers match | 2–8 min |
| [plan-implementation](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/plan-implementation.md) | Turn a tech design's work items into an executable plan for the epic loop | inside sdlc-implement | 1–3 min |
| [build-unit-tests](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/build-unit-tests.md) | Discover stack and conventions, write tests via parallel test-writer agents, measure coverage delta against 60% gate | inside sdlc-implement | 2–15 min |
| [build-e2e-tests](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/build-e2e-tests.md) | Plan and run Playwright E2E tests for UI changes, loop with stabilize-pr | Phase 6 of orchestrate-sdlc | 10–45 min |
| [align-pre-commit](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/align-pre-commit.md) | Lint, format, and polish before committing an epic | end of each epic in sdlc-implement | 1–3 min |
| [prepare-pr](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/prepare-pr.md) | Sync branch with main, draft PR description, open PR with `Closes #N` | Phase 4 of orchestrate-sdlc | 1–2 min |
| [review-pr](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-pr.md) | Interactive / human-invoked PR review across 7 dimensions | `/review-pr <PR>` | 3–10 min |
| [review-pr-ci](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-pr-ci.md) | Unattended, comment-only PR review in GitHub Actions (Claude-backed) | `claude-review` label on a PR | 5–15 min (30 min timeout) |
| [review-pr-codex-ci](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-pr-codex-ci.md) | Unattended, comment-only PR review in GitHub Actions (Codex-backed, structured summary) | `codex-review` label on a PR | 5–20 min (45 min timeout) |
| [stabilize-pr](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/stabilize-pr.md) | Triage CI failures + reviewer findings + external comments, auto-fix high-confidence issues, loop until merge-ready | Phase 5 of orchestrate-sdlc | 5–30 min |
| [babysit-pr](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/babysit-pr.md) | Long-running review-stabilize loop with no-progress counter for PRs that need multiple passes | `/babysit-pr <PR>` | 10 min – hours |
| [hotfix](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/hotfix.md) | Minimal-diff hotfix branched from prod with 2 hard-pause gates; supports revert mode | `/hotfix <description>` | 10–30 min |
| [finalize-sdlc](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/finalize-sdlc.md) | Phase 7–8 cleanup, post-merge deploy wait, smoke tests, issue closeout, epic checklist update | end of orchestrate-sdlc | 5–20 min |
| [maintain-docs](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/maintain-docs.md) | SDLC-integrated / standalone / changelog modes for documentation upkeep; drift detection | `/maintain-docs`, or invoked by finalize-sdlc | 2–15 min |
| [image-generation](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/image-generation.md) | Enhance image prompts using Subject-Context-Style structure, delegate to mcp-image MCP | `/image-generation <description>` | 10–60 sec |
| [map-codebase](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/map-codebase.md) | Fast codebase exploration via parallel explore-fast subagents — maps architecture, conventions, dependencies, tests for a subsystem | `/map-codebase <subsystem>` or `/map` | 2–10 min |
| [new-feature](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/new-feature.md) | Cold-start front door: optionally pre-loads a codebase map, runs analyze-requirements, can chain into orchestrate-sdlc for full delivery | `/new-feature <description>` | 5–20 min (before chaining) |

## Alias commands

Shorthand wrappers that delegate to canonical skills. All arguments pass through unchanged.

| Alias | Delegates to | Note |
|-------|--------------|------|
| `/run` | `orchestrate-sdlc` | Full SDLC, accepts issue number / description / "resume" |
| `/next` | `orchestrate-sdlc` | Resume from `state.md` (= `/run resume`) |
| `/reqs` | `analyze-requirements` | Requirements-only path |
| `/design` | `produce-tech-design` | Tech-design-only path |
| `/plan` | `plan-implementation` | Epic implementation plan |
| `/discover` | `product-discovery` | Discovery brief from raw idea |
| `/issues` | `create-issues` | Decompose a PRD into GitHub issues |
| `/map` | `map-codebase` | Map a subsystem |
| `/review` | `review-pr` | Ad-hoc PR review |
| `/ship` | `prepare-pr` | Open the PR |
| `/stabilize` | `stabilize-pr` | Drive CI + review to green |
| `/babysit` | `babysit-pr` | Long-running PR watcher |
| `/finalize` | `finalize-sdlc` | Post-merge close-out |
| `/verify` | sdlc-deliver (Phase 6.5) | UAT screenshot checkpoint |
| `/help` | — | In-session cheatsheet — verb → canonical skill, SDLC phase, common workflows |

## How to pick a skill

- **Start a feature**: `/orchestrate-sdlc` (or its alias `/run`) almost always. It routes to everything else.
- **Resume a feature**: `/next` (or `/run resume`) reads `state.md` and picks up from the last completed phase.
- **Raw product idea, no clear scope yet**: `/discover` (= `/product-discovery`). Produces a `discovery.md` brief that feeds `/analyze-requirements`.
- **Just need a PRD**: `/analyze-requirements` (or `/reqs`) by itself.
- **Just need issues from an existing PRD**: `/create-issues` (or `/issues`) by itself.
- **Unfamiliar codebase**: `/map-codebase <subsystem>` first, then `/new-feature <description>` to chain into requirements.
- **Incident**: `/hotfix`, not `/orchestrate-sdlc`.
- **Ad-hoc PR review**: `/review-pr`. For CI-triggered review, use the labels.
- **PR sitting in CI hell**: `/babysit-pr` to run the loop without supervising it minute-by-minute.
- **Generate an image**: `/image-generation`.
- **Post-merge cleanup on an old branch**: `/finalize-sdlc <slug>`.

## Model selection

Most skills use the strongest available model by default. Three exceptions worth knowing:

- **babysit-pr** splits: opus for triage, haiku for polling and minor fixes. See [babysit-pr reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/babysit-pr.md).
- **review-pr-codex-ci** runs on `gpt-5.4` inside a Codex container, not Claude. See [review-pr-codex-ci reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/review-pr-codex-ci.md).
- **image-generation** calls the `mcp-image` server (Gemini-backed) — the skill itself is just prompt rewriting.

## Related

- [Skill Catalog (full)](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/README.md)
- [Agents Quick Reference](agents-quickref.md)
- [Cheatsheet](cheatsheet.md)
- [Glossary](glossary.md)
