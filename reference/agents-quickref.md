# Agents Quick Reference

Subagents are lightweight child agents the main agent dispatches for focused work. They run in isolation, with their own context window, and return a single message back when done. This page covers both **repo-local agents** (defined in `.claude/agents/*.md` in this repo) and **platform agents** (provided by the Claude Code runtime).

See [agents reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/agents.md) for the full architectural picture.

## Repo-local agents (`.claude/agents/`)

These are the agents the orchestrator and skills in this repo dispatch by name. They are declared in `.claude/agents/*.md` with frontmatter that controls model and tool access.

| Agent | Model | Tools | Dispatch guidance |
|-------|-------|-------|-------------------|
| **coding-agent** | inherit (strongest) | all | General-purpose coding — implement a feature slice, fix a bug, refactor. Used heavily by `sdlc-implement` (up to 4 in parallel per epic). |
| **doc-writer** | sonnet | all | Write or update documentation. Used by `maintain-docs`, Epic 2 of `ai-dlc-docs-refresh`, and any skill that produces markdown. |
| **explore-fast** | haiku | Read, Grep, Glob, Bash (read-only) | Fast, read-only exploration — find files, extract conventions, map dependencies. No writes. Use when you need to search before deciding. |
| **reviewer** | sonnet | Read, Grep, Glob, Bash (read-only) | Structured code review — evidence-backed findings with severity. Used by `review-pr`, `review-pr-ci`, `review-pr-codex-ci`. Cannot modify files. |
| **shell-runner** | haiku | Bash, Read, Grep (read-only for files) | Run commands, tests, CI jobs, git operations. Use for execution-heavy tasks. Does not edit files. |
| **test-writer** | sonnet | all | Discover test conventions and write tests that match. Used by `build-unit-tests` (up to 3 in parallel). |

### Dispatch patterns

- **Fan-out**: dispatch 3–4 agents in parallel in a single tool call. `sdlc-implement` does this for coding subagents; `build-unit-tests` does it for test-writers. The orchestrator waits for all to return before proceeding.
- **Read before write**: always dispatch `explore-fast` to gather context before dispatching `coding-agent` to change things. This prevents the "edit without understanding" failure mode.
- **Isolated review**: the reviewer agent is dispatched with **no implementation context** during Phase 5 stabilization. This is intentional — the reviewer should see the PR as an outsider would.

## Platform agents (Claude Code runtime)

These agents are provided by Claude Code itself and available via the `Agent` tool even without a repo-local definition. They are generally more general-purpose than repo-local agents.

| Agent | Typical use |
|-------|-------------|
| **general-purpose** | Open-ended research that spans the codebase and doesn't match a specialized agent |
| **Explore** | Fast file discovery and pattern search — the platform analog of `explore-fast`. Supports thoroughness levels (`quick`, `medium`, `very thorough`) |
| **Plan** | Software architect agent for designing implementation plans before writing code |
| **statusline-setup** | Configures the Claude Code status line — rarely needed directly |
| **claude-code-guide** | Answers questions about Claude Code itself, the Agent SDK, and the Anthropic API |

Prefer repo-local agents (`coding-agent`, `test-writer`, etc.) when they match the task, because they carry project-specific conventions in their system prompts. Fall back to platform agents when the task is generic or when no repo-local agent fits.

## Tool permission scoping

Agent frontmatter can restrict which tools an agent may use. For example:

```yaml
---
name: explore-fast
tools: Read, Grep, Glob, Bash
disallowedTools: Write, Edit
model: haiku
---
```

Read-only agents (`explore-fast`, `reviewer`, `shell-runner`) cannot modify files no matter what the main agent asks. This is a safety rail — if you see a task that should not mutate state, dispatch a read-only agent.

**Known quirk**: subagent `Write` permission may be denied by the harness permission layer even when the agent definition allows it. When this happens, the main agent pattern is for the subagent to **return drafted content** in its final message; the main agent then performs the actual write. Epic 1 of this docs refresh used exactly this workaround when doc-writer subagents were denied Write.

## Model selection

The model is declared in agent frontmatter via the `model:` field:

- `inherit` — use whatever model the parent is running (usually the strongest available)
- `opus` / `sonnet` / `haiku` — pin to a specific tier
- unspecified — inherits from parent

Rationale:

- **coding-agent uses `inherit`** because code editing is the single most judgment-heavy task and benefits most from the strongest model.
- **doc-writer, reviewer, test-writer use `sonnet`** — they need good reasoning but the tasks are well-scoped and sonnet is fast enough for parallel dispatch.
- **explore-fast, shell-runner use `haiku`** — fast, cheap, read-only. Parallel dispatch is the main value.

## How agents differ from skills

A **skill** is a structured workflow (a file with phases and rules) that the current agent follows step by step. A **subagent** is a child agent that you delegate a task to — it runs in its own context and returns a result.

You invoke a skill with a slash command or a natural-language trigger. You dispatch a subagent with the `Agent` tool. Skills and agents are composable: a skill can dispatch subagents, and a subagent can invoke skills.

See [skills-guide concepts](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/concepts.md) for the full architectural distinction.

## Related

- [agents reference (full)](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/agents.md)
- [Skills Quick Reference](skills-quickref.md)
- [Cheatsheet](cheatsheet.md)
- [Glossary](glossary.md)
