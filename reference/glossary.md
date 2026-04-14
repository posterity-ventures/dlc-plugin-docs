# Glossary

Every term used in this guide, defined in one sentence. When in doubt about jargon, `Ctrl-F` here first.

## A

**Acceptance criteria** — concrete, testable statements in a PRD that define when a feature is "done."

**Agent (subagent)** — a lightweight child agent the main agent dispatches for focused work; runs in its own context and returns a single message.

**AI-DLC** — AI-Driven Development Lifecycle; the system of skills, agents, hooks, and rules that automates the SDLC in this repo.

**analyze-requirements** — the skill that turns a plain-language brief into a full PRD. See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/analyze-requirements.md).

**Artifact** — a structured document under `${DLC_ARTIFACT_ROOT:-ai_dlc_artifacts}/<slug>/` (PRD, design, plan, state, reviews, telemetry) that forms a feature's audit trail.

**Artifact directory** — `${DLC_ARTIFACT_ROOT:-ai_dlc_artifacts}/<slug>/`; the per-feature scoped directory where all artifacts live.

**Autopilot** — an interaction mode where the orchestrator runs without pausing except at hard-pause gates; used for overnight and trusted work.

## B

**babysit-pr** — a skill that runs the review-stabilize loop on a PR with a no-progress counter for long-running stabilization. See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/babysit-pr.md).

**Branching (GitLab Flow)** — `feature/* → main → staging → prod`; `main` is the integration branch and `staging`/`prod` receive downstream merges only.

**Brownfield** — working inside existing code (bug fixes, small features, legacy modules); see [playbook](../playbooks/brownfield.md).

**build-e2e-tests** — the skill that plans and runs Playwright E2E tests for UI changes in Phase 6.

**build-unit-tests** — the skill that writes and measures unit test coverage against the 60% gate in Phase 3.

## C

**Canary deploy** — a deploy to a small subset of production traffic before full rollout; the modernization playbook uses this to catch regressions early.

**Checkpoint** — a point in a skill's workflow where it pauses and asks the user to approve or redirect; behavior depends on the interaction mode.

**claude-review / codex-review labels** — GitHub PR labels that trigger `review-pr-ci` / `review-pr-codex-ci` respectively via workflow dispatch.

**Confident mode** — the default interaction mode; the orchestrator pauses at major phase boundaries only.

**Coverage gate** — the 60% unit-test coverage threshold enforced by `build-unit-tests` at the end of each epic.

**create-issues** — the skill that decomposes a PRD into GitHub issues (epic + children). See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/create-issues.md).

## D

**Decisions Log** — a section in `state.md` where skills record non-trivial decisions, tagged `AUTOPILOT DECISION` when made without user confirmation.

**Delta (design delta)** — a targeted update to a tech design when implementation reveals a new constraint, routed through the dispatcher back to Phase 2c.

**Dispatcher** — the routing layer of `orchestrate-sdlc`; reads `state.md`, picks a sub-skill, invokes it, re-routes on return.

**Dossier** — the pre-generated PR context JSON (`/tmp/pr-context.json` or `.codex-review/pr-context.json`) consumed by CI review skills.

## E

**E2E testing** — end-to-end testing, specifically Playwright browser automation; Phase 6 of the SDLC, conditional on UI changes.

**Epic (issue)** — a GitHub issue that tracks a large initiative and contains a checklist of child issue links; ticked by `finalize-sdlc`.

**Epic (SDLC)** — a self-contained increment of implementation work in Phase 3; each epic ends with a commit and push.

**Escalation** — a sub-skill surfacing a blocker by writing `escalation-context.md` and returning `blocked` to the dispatcher for re-routing.

**Escalation counter** — a counter in `state.md` that forces a hard-pause when it reaches 2 in autopilot mode.

## F

**Feature branch** — a branch named `feat/<slug>` / `fix/<slug>` / `chore/<slug>` / `refactor/<slug>` created by `sdlc-intake` for a single SDLC run.

**finalize-sdlc** — the Phase 7–8 skill that cleans up transient artifacts, waits for deploy, runs smoke tests, closes the linked issue, updates the epic checklist. See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/finalize-sdlc.md).

**Flux** — the GitOps reconciler used in this repo to deploy Kubernetes workloads by updating `image-tags.env`; see [CLAUDE.local.md](../../../../CLAUDE.local.md#deployment).

## G

**Greenfield** — starting from a PRD with no existing code; see [playbook](../playbooks/greenfield.md).

## H

**Hard-pause gate** — a point the orchestrator will not pass without explicit user confirmation, regardless of interaction mode (direct pushes to protected branches, hotfix confirmations, Phase 8 deploy).

**Haiku** — the fast/cheap Claude tier used by read-only exploration agents (`explore-fast`, `shell-runner`) and babysit-pr polling.

**Hook** — a shell command the Claude Code harness runs automatically on an event (`PreToolUse`, `PostToolUse`, `SubagentStart`, `SubagentStop`); configured in `.claude/settings.json`.

**Hotfix** — an out-of-band fix branched from `prod` with minimal diff and two hard-pause gates; use `/hotfix` instead of `/orchestrate-sdlc` for incidents.

## I

**Image generation** — the skill that wraps the `mcp-image` MCP server with a Subject-Context-Style prompt enhancement pattern.

**Interaction mode** — one of `interactive`, `confident`, `autopilot`; controls how often the orchestrator pauses for user input.

**Interactive mode** — an interaction mode where the orchestrator pauses at every checkpoint and invites discussion.

## L

**Linked issue** — a GitHub issue passed to `orchestrate-sdlc` that anchors the SDLC run; `prepare-pr` adds `Closes #<n>` to the PR description.

## M

**maintain-docs** — the skill that handles documentation upkeep in SDLC-integrated, standalone, or changelog modes. See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/maintain-docs.md).

**MCP (Model Context Protocol)** — the protocol Claude Code uses to connect to external tool servers (context7, mcp-image); servers are configured under `mcpServers` in `.claude/settings.json`.

**Merge-ready** — a PR that has passed `review-pr` + `stabilize-pr` and is waiting on a human to click merge.

**Mermaid** — the diagramming syntax used as a fallback when image generation is unavailable (see Epic 3 of this docs refresh).

**Modernization** — large refactors, dependency migrations, or cross-cutting rewrites; see [playbook](../playbooks/modernization.md).

## O

**Opus** — the strongest Claude tier; used by the orchestrator and skills that require high judgment (coding-agent, review).

**orchestrate-sdlc** — the top-level SDLC dispatcher skill invoked as `/orchestrate-sdlc`. See [reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/orchestrator.md).

## P

**Parallel-change (expand/migrate/contract)** — a migration pattern where new code is added alongside old, callers are migrated one at a time, then old is deleted; used in the modernization playbook.

**Phase** — one of the 8 SDLC phases (1–8); `state.md` records the current phase and phase status table.

**plan-implementation** — the skill that turns tech-design work items into an executable epic plan.

**POC (proof of concept)** — a lightweight SDLC path that skips heavy gates; see [playbook](../playbooks/poc.md).

**prepare-pr** — the Phase 4 skill that syncs the branch with main, drafts a PR description, and opens the PR with `Closes #<n>`.

**PRD (Product Requirements Document)** — the `requirements.prd.md` artifact; the source of truth for what a feature does.

**Preview environment** — a per-PR or shared preview deploy used for UAT by product stakeholders.

**produce-tech-design** — the Phase 2c skill that writes a tech design with decisions, work-item DAG, and epic breakdown.

**Promotion PR** — a PR that promotes code from `main` → `staging` or `staging` → `prod`; must use a merge commit (never squash) per [branching rule](https://github.com/posterity-ventures/dlc-plugin/blob/main/rules/branching.md).

## Q

**Q&A protocol** — the shared checkpoint protocol used by skills when asking the user questions; see `.claude/skills/_shared/qa-protocol.md`.

## R

**Resume-safe** — a property of the orchestrator: if a session is interrupted mid-run, the next run picks up cleanly by reading `state.md`.

**review-pr** — the interactive / human-invoked PR review skill across 7 dimensions.

**review-pr-ci** — the unattended Claude-backed CI review skill triggered by the `claude-review` label.

**review-pr-codex-ci** — the unattended Codex-backed CI review skill triggered by the `codex-review` label; emits a structured summary with a `<!-- codex-review -->` marker.

**review-security** — the Phase 2b skill that produces a security review report before tech design when auth/PII/infra triggers match.

**review-ux** — the Phase 2b skill that captures live screenshots and produces a UX review report before tech design when UI triggers match.

**Revert mode (hotfix)** — a hotfix mode where the fix is a revert of a specific commit rather than new code.

**Rule** — a short markdown file in `.claude/rules/` that auto-loads into every conversation as durable project policy.

## S

**Scope assessment** — Phase 2a: a short evaluation of whether security/UX review triggers match; never silently skipped.

**sdlc-deliver** — the sub-skill that handles Phases 4–6 (PR creation, review, stabilization, conditional E2E).

**sdlc-finalize** — the sub-skill that wraps `finalize-sdlc` for Phases 7–8.

**sdlc-implement** — the sub-skill that runs the Phase 3 epic loop.

**sdlc-intake** — the sub-skill that handles Phases 1–2c (requirements, scope, pre-design reviews, tech design).

**Skill** — a structured AI workflow defined in `.claude/skills/<name>/SKILL.md`; the unit the agent actually runs.

**Skill invocation** — reading a skill's `SKILL.md` and following its workflow start to finish.

**Slug** — a short kebab-case identifier for a feature (e.g., `ai-dlc-docs-refresh`); used for the artifact directory and branch name.

**Smoke tests** — a small set of post-deploy checks run by `finalize-sdlc` in Phase 8.

**Sonnet** — the mid-tier Claude model; used by specialized subagents (doc-writer, reviewer, test-writer).

**stabilize-pr** — the Phase 5 skill that triages CI failures, reviewer findings, and external comments in a loop until the PR is merge-ready.

**state.md** — the single source of truth for resume; records phase, mode, slug, branch, worktree, phase status table, decisions log.

**Strangler fig** — a modernization pattern where a new subsystem shadows the old until traffic fully migrates.

**Subject–Context–Style** — the prompt structure used by the `image-generation` skill (what / where-when / how).

## T

**task-scope rule** — the rule that enforces "do exactly what was asked, confirm before extras"; see [rule](https://github.com/posterity-ventures/dlc-plugin/blob/main/rules/task-scope.md).

**Tech design** — the `designs/tech-design.md` artifact produced by `produce-tech-design` in Phase 2c.

**Telemetry** — the append-only `telemetry.jsonl` log of events (tool calls, subagent dispatches, hook fires, phase transitions) for each SDLC run.

**Trigger (review trigger)** — a condition in the diff or PRD that causes `sdlc-intake` to mandate a Phase 2b review (auth changes trigger security review, UI changes trigger UX review).

## U

**UAT (User Acceptance Testing)** — product-side verification that the feature behaves as intended, run in the preview environment, not locally.

## W

**Work item** — a leaf node in the tech design's DAG; each work item becomes a coding subagent task in Phase 3.

**Work type** — the conventional-commit type prefix for a branch: `feat` / `fix` / `chore` / `refactor`.

**Worktree** — a `git worktree`-isolated checkout of the feature branch at `.worktrees/<work-type>/<slug>`; created by `sdlc-intake` so multiple SDLC runs can proceed in parallel without stepping on each other.

**Worktree-safety rule** — the rule that bans force-checkout and enforces worktree isolation; see [rule](https://github.com/posterity-ventures/dlc-plugin/blob/main/rules/worktree-safety.md).

## Related

- [Skills Quick Reference](skills-quickref.md)
- [Agents Quick Reference](agents-quickref.md)
- [Cheatsheet](cheatsheet.md)
- [skills-guide concepts](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/concepts.md) — deeper architectural treatment
