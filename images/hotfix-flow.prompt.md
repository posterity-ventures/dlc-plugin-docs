# Prompt — hotfix-flow.png

**Include** [_style.md](_style.md) preamble before the prompt body.

**Parameters**: `aspectRatio: 16:9`, `imageSize: 2K`

## Prompt body

A clean, modern technical diagram in flat vector illustration style showing the incident-driven hotfix path with its two hard-pause gates. Dark-mode-friendly deep slate background (#1e293b). Soft blue (#60a5fa) for the main flow and warm amber (#fbbf24) for hard-pause gates and the dual-target merge callout. Sans-serif typography, crisp labels.

A horizontal main flow left to right with six rounded blue rectangles labeled exactly in order: "Incident", "Branch from prod", "Minimal-diff fix", "Open PR", "Stabilize", "Merge to prod".

Above the transition between "Branch from prod" and "Minimal-diff fix" place an amber diamond-shaped marker labeled exactly "Hard pause: scope gate".

Above the transition between "Open PR" and "Stabilize" place an amber diamond-shaped marker labeled exactly "Hard pause: review gate".

From the final box "Merge to prod", a second amber arrow curves downward to a separate rectangle labeled exactly "Back-merge to staging" showing that prod fixes are mirrored downstream so environments stay consistent.

Title at the top reads exactly "Hotfix path with hard-pause gates".

Small caption at the bottom reads exactly "minimal diff, two gates, dual-target merge".

Minimal line icons in each box: a warning triangle for Incident, a branching-arrow for Branch from prod, a bandage for Minimal-diff fix, a pull-request icon for Open PR, a wrench for Stabilize, a checkmark for Merge to prod, and a circular arrow for Back-merge to staging. Clear hierarchy, consistent stroke width, no photo-realistic elements.
