# Onboarding Guide — Images

This directory contains the visual diagrams embedded throughout the onboarding guide, plus their source prompts so each image can be regenerated deterministically.

## Contents

| File | Used by | Notes |
|------|---------|-------|
| [_style.md](_style.md) | all prompts | Shared style preamble that every `.prompt.md` references |
| [sdlc-lifecycle.png](sdlc-lifecycle.png) | [README.md](../README.md) | AI-DLC 8-phase lifecycle, horizontal |
| [skill-agent-hook-rule.png](skill-agent-hook-rule.png) | [README.md](../README.md) | Four building blocks and how they interact |
| [artifacts-lifecycle.png](artifacts-lifecycle.png) | [core-workflow.md](../core-workflow.md) | `${DLC_ARTIFACT_ROOT:-.dlc}/<slug>/` layout with permanent vs transient |
| [gitlab-flow.png](gitlab-flow.png) | [cheatsheet.md](../reference/cheatsheet.md) | GitLab Flow — feature → main → staging → prod (default preset, shown alone on the cheatsheet for emphasis) |
| [git-flows-comparison.png](git-flows-comparison.png) | [core-workflow.md §9](../core-workflow.md#9-branching-model-configuration--read-this-if-youre-adopting-claude-in-another-repo) | 2×2 grid of all four supported presets: GitLab Flow, GitHub Flow, Gitflow, Trunk-based |
| [interaction-modes.png](interaction-modes.png) | [core-workflow.md §4](../core-workflow.md#4-interaction-modes), [cheatsheet.md](../reference/cheatsheet.md) | Checkpoint cadence per mode (interactive / confident / autopilot) with Decisions Log |
| [product-discovery-flow.png](product-discovery-flow.png) | [playbooks/discovery.md](../playbooks/discovery.md) | 7-phase discovery flow: Framing → Research → Framework Passes → Concerns+RAID → Scope Clustering → Synthesis → Handoff |
| [discovery-framework-modes.png](discovery-framework-modes.png) | [playbooks/discovery.md](../playbooks/discovery.md) | Six framework paths grid (JTBD+Lean Canvas default, SWOT+Kano, Porter+BMC, OST, Working Backward, Help me choose) |
| [hotfix-flow.png](hotfix-flow.png) | [troubleshooting.md](../playbooks/troubleshooting.md#hotfix-path) | Hotfix path with 2 hard-pause gates and back-merge to staging |
| [escalation-routing.png](escalation-routing.png) | [troubleshooting.md](../playbooks/troubleshooting.md#escalation-paths) | PR stabilization escalation classifier → retry_ci / auto_fix / triage / hard_pause / route-to-2c |
| [worktree-isolation.png](worktree-isolation.png) | [core-workflow.md](../core-workflow.md) | Shared `.git/` + independent worktree checkouts |
| [product-dev-handoff.png](product-dev-handoff.png) | [product-dev-collaboration.md](../playbooks/product-dev-collaboration.md) | Swimlane handoff between Product and Dev |
| [hooks-telemetry.png](hooks-telemetry.png) | [cheatsheet.md](../reference/cheatsheet.md) | Tool-call lifecycle with hook insertion and telemetry sink |
| [playbook-greenfield.png](playbook-greenfield.png) | [greenfield.md](../playbooks/greenfield.md) | Vertical 7-step flow |
| [playbook-poc.png](playbook-poc.png) | [poc.md](../playbooks/poc.md) | POC with three-way outcome decision |
| [playbook-brownfield.png](playbook-brownfield.png) | [brownfield.md](../playbooks/brownfield.md) | Test-first 7-step flow |
| [playbook-modernization.png](playbook-modernization.png) | [modernization.md](../playbooks/modernization.md) | Parallel-change 3-lane pattern |
| [playbook-troubleshooting.png](playbook-troubleshooting.png) | [troubleshooting.md](../playbooks/troubleshooting.md) | Decision tree across failure categories |

Each `.png` has a sibling `.prompt.md` file with the exact prompt used, so the image set stays regenerable and auditable.

## Visual style

All images share the same visual identity defined in [_style.md](_style.md):

- **Palette**: deep slate background (#1e293b), soft blue (#60a5fa) for primary flow, warm amber (#fbbf24) for emphasis — and nothing else
- **Typography**: sans-serif, clear hierarchy, crisp labels
- **Iconography**: minimal line icons only, no photo-realism
- **Composition**: left-to-right (or top-to-bottom for tall diagrams)
- **Labels**: spelled exactly in the prompt (image models misspell labels often)

When regenerating or adding a new image, copy the preamble from `_style.md` into the new `.prompt.md` first, then append the image-specific content. This is how cross-image consistency is maintained.

## How the images were generated

The onboarding guide diagrams are generated via the `image-generation` skill, which wraps the `mcp-image` MCP server (backed by Gemini). See the [image-generation skill reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/image-generation.md) and [mcp.md](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/mcp.md) for server configuration.

Invocation pattern:

```
/image-generation <read prompt from .prompt.md file verbatim>
```

Parameters set inline in the tool call:

| Parameter | Value | Reason |
|-----------|-------|--------|
| `aspectRatio` | `16:9` (wide diagrams) or `9:16` (tall playbook flows) | Fit inline in markdown |
| `imageSize` | `2K` | Crisp at zoom; not wasteful |
| `fileName` | `<name>.png` (no path) | Server decides directory |

## Regenerating a single image

```
cat .claude/docs/onboarding-guide/images/<name>.prompt.md
# Then invoke /image-generation with the body of that file
```

The generator writes to `${IMAGE_OUTPUT_DIR}/<fileName>` where `IMAGE_OUTPUT_DIR` comes from `.claude/settings.json`'s `mcp-image` env. Verify the output landed in this directory before committing.

## Regenerating the entire set

All images in this directory can be regenerated in a single Claude Code session by asking the agent to "regenerate all images in `images/` from their `.prompt.md` files using `/image-generation`." The generation is deterministic with respect to the prompt — the same prompt should produce visually similar output each time, though not pixel-identical (generative models have inherent variance).

## Path handling note

The `mcp-image` server writes to its configured output directory (`IMAGE_OUTPUT_DIR` env var, defaulting to `./output/` relative to the Claude Code working directory). If regenerated files land somewhere unexpected, check `IMAGE_OUTPUT_DIR` in `.claude/settings.json`'s `mcp-image` env block, move the files into place, and commit from this repo.

## Extension note

Generated files are labeled `.png` but the `mcp-image` server actually outputs JPEG. Markdown renderers and browsers render them correctly regardless of extension mismatch, so no rename is required.
