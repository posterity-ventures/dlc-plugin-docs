# Onboarding Guide — Images

This directory contains the visual diagrams embedded throughout the onboarding guide, plus their source prompts so each image can be regenerated deterministically.

## Contents

| File | Used by | Notes |
|------|---------|-------|
| [_style.md](_style.md) | all prompts | Shared style preamble that every `.prompt.md` references |
| [sdlc-lifecycle.png](sdlc-lifecycle.png) | [README.md](../README.md) | AI-DLC 8-phase lifecycle, horizontal |
| [skill-agent-hook-rule.png](skill-agent-hook-rule.png) | [README.md](../README.md) | Four building blocks and how they interact |
| [artifacts-lifecycle.png](artifacts-lifecycle.png) | [core-workflow.md](../core-workflow.md) | `${DLC_ARTIFACT_ROOT:-ai_dlc_artifacts}/<slug>/` layout with permanent vs transient |
| [gitlab-flow.png](gitlab-flow.png) | [cheatsheet.md](../reference/cheatsheet.md) | Branching model — feature → main → staging → prod |
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

The onboarding guide diagrams are generated via the `image-generation` skill, which wraps the `mcp-image` MCP server (backed by Gemini). See the [image-generation skill reference](../../skills-guide/skills/image-generation.md) and [mcp.md](../../skills-guide/mcp.md) for server configuration.

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

All 12 images can be regenerated in a single Claude Code session by asking the agent to "regenerate all images in `.claude/docs/onboarding-guide/images/` from their `.prompt.md` files using `/image-generation`." The generation is deterministic with respect to the prompt — the same prompt should produce visually similar output each time, though not pixel-identical (generative models have inherent variance).

## Path handling note

At the time of initial generation, the `mcp-image` server in this environment had an unresolved `${PROJECT_DIR}` variable in its internal output path template, causing images to land at `/workspaces/k2_mvp/${PROJECT_DIR}/tmp/<fileName>` literally. The workaround used at generation time was a symlink:

```bash
mkdir -p '/workspaces/k2_mvp/${PROJECT_DIR}'
ln -sfn /workspaces/k2_mvp/.worktrees/chore/ai-dlc-docs-refresh/.claude/docs/onboarding-guide/images \
  '/workspaces/k2_mvp/${PROJECT_DIR}/tmp'
```

If the server config is ever fixed upstream, this symlink becomes unnecessary. If regeneration lands files in the wrong place, re-apply the symlink or adjust the `mcp-image` server's `IMAGE_OUTPUT_DIR` env var and restart Claude Code.

## Extension note

Generated files are labeled `.png` but the `mcp-image` server actually outputs JPEG. Markdown renderers and browsers render them correctly regardless of extension mismatch, so no rename is required.
