# Playbook — Cold Start (Unfamiliar Codebase)

You need to build a feature in a part of the codebase you have never worked in before. You do not know the architecture, conventions, or where to start. This playbook walks you from zero orientation through a merge-ready PR.

> **Prerequisite**: you have read [core-workflow.md](../core-workflow.md) and have Claude Code running inside the repo root.

## 1. When to use this playbook

- You are new to a subsystem or codebase section
- You are onboarding to a project and need to deliver a feature quickly
- You are about to start work in an area with unfamiliar patterns, dependencies, or test infrastructure
- You want the AI to orient itself before writing requirements or code

If you already know the codebase well, skip to the [greenfield playbook](greenfield.md) or [brownfield playbook](brownfield.md) instead.

## 2. Map the target subsystem

Run the map-codebase skill on the subsystem you will be working in:

```
/map-codebase api/
```

Or map the full repository if you are unsure which subsystem is relevant:

```
/map-codebase
```

The skill dispatches 4 parallel subagents to investigate architecture, conventions, dependencies, and test infrastructure. The output lands at `${DLC_ARTIFACT_ROOT:-ai_dlc_artifacts}/maps/YYYY-MM-DD-<name>.md`.

**Time**: 2-5 minutes depending on codebase size.

## 3. Review the map artifact

Open the generated map and scan for:

- **Entry points**: where would your feature plug in?
- **Conventions**: what patterns does this subsystem follow?
- **Dependencies**: what services and packages are already available?
- **Test infrastructure**: how will you test your feature?

This review takes 2-3 minutes and saves significant time in later phases by reducing Q&A cycles.

## 4. (Optional) Run product discovery for raw ideas

If your feature idea is still unformalized — you have a rough concept but no clear scope, user stories, or acceptance criteria — run product discovery first:

```
/discover I'm thinking about adding a recommendation engine
```

The skill supports 5 product frameworks. Choose the one that fits your scenario:

| Framework | When to choose |
|-----------|---------------|
| **JTBD + Lean Canvas** (default) | Validating customer needs and business model for new or evolving products |
| **SWOT + Kano** | Assessing competitive position and prioritizing features by satisfaction impact |
| **Porter's Five Forces + BMC** | Analyzing market dynamics and designing a comprehensive business model |
| **Opportunity Solution Tree** | Mapping outcomes to opportunities, solutions, and assumption tests |
| **Working Backward (PR/FAQ)** | Pressure-testing a concept by writing the customer-facing narrative first |

**Not sure which to pick?** Select "Help me choose" when prompted — the skill asks 3-5 diagnostic questions about your product stage, goals, and data availability, then recommends the best fit using a scoring algorithm. See the [product-discovery reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/product-discovery.md) for the full routing matrix.

The output is a structured discovery brief (`discovery.md`) with framework analysis, MoSCoW backlog, RAID log, and scope-cluster recommendations. The brief feeds directly into requirements analysis in the next step.

**Skip this step** if you already have a clear feature description, a product brief, or a GitHub issue with defined scope.

## 5. Start the feature with context

Run the new-feature skill with your feature description. It auto-detects the map you generated (and discovery brief if you ran Step 4):

```
/new-feature Add batch export endpoint to the api/ subsystem
```

The skill will:
1. Find and load the existing map (no need to regenerate)
2. Detect the discovery brief if one exists (no need to re-run)
3. Pass both as context into `/analyze-requirements`
4. Produce a PRD grounded in the actual codebase architecture and product thinking

If you want full end-to-end delivery in one command, say so explicitly:

```
/new-feature Build a batch export endpoint in api/; deliver end-to-end
```

## 6. Review the PRD, then deliver

After the PRD is produced, you have two paths:

**Path A — Review first, then deliver:**
1. Read `${DLC_ARTIFACT_ROOT:-ai_dlc_artifacts}/<slug>/requirements.prd.md`
2. Edit anything that does not match your intent
3. Run `/orchestrate-sdlc` to pick up from Phase 2a (scope assessment)

**Path B — Continuous delivery (autopilot/confident):**
The `/new-feature` skill offers to chain directly into `/orchestrate-sdlc`. Accept, and the system continues through tech design, implementation, PR, and stabilization without stopping.

## 7. Tips

### When to map the full repo vs a subsystem

- **Full repo**: When you do not know which subsystem your feature belongs to, or when the feature spans multiple subsystems.
- **Subsystem**: When you know where the feature lives but not how that subsystem works. Subsystem maps are faster and more focused.

### Reusing maps across features

Maps are persistent artifacts. If you mapped `api/` last week and are starting another feature there, the `/new-feature` skill will find and offer to reuse that map. Maps older than a few weeks may be stale if the codebase has changed significantly — regenerate in that case.

### Combining with other playbooks

- After the cold-start, subsequent features in the same area follow the [greenfield playbook](greenfield.md) (new features) or [brownfield playbook](brownfield.md) (changes to existing features). The map artifact remains available.
- For proof-of-concept work, see [poc.md](poc.md) — you can run `/map-codebase` before a POC to understand integration points.

## Next

- Ready to deliver the feature? Chain into `/orchestrate-sdlc`.
- Want to explore more before committing? Run `/map-codebase` on additional subsystems.
- Need to create tracking issues? See [create-issues skill reference](https://github.com/posterity-ventures/dlc-plugin/blob/main/docs/skills-guide/skills/create-issues.md).
