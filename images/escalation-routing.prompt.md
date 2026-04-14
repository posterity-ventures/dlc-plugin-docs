# Prompt — escalation-routing.png

**Include** [_style.md](_style.md) preamble before the prompt body.

**Parameters**: `aspectRatio: 16:9`, `imageSize: 2K`

## Prompt body

A clean, modern technical diagram in flat vector illustration style showing how PR-stabilization failures are classified and routed. Dark-mode-friendly deep slate background (#1e293b). Soft blue (#60a5fa) for failure-class boxes and warm amber (#fbbf24) for the routing actions. Sans-serif typography, crisp labels.

A left column of six blue rounded rectangles, stacked vertically, each labeled exactly in order from top to bottom: "flaky_test", "merge_conflict", "ai_review_unresolved", "human_review", "security_scan", "design_delta".

A right column of four amber rounded rectangles, stacked vertically, each labeled exactly in order from top to bottom: "retry_ci", "auto_fix", "triage", "hard_pause".

Between the two columns, draw arrows from each left box to the appropriate right box:
- "flaky_test" arrow to "retry_ci"
- "merge_conflict" arrow to "auto_fix"
- "ai_review_unresolved" arrow to "triage"
- "human_review" arrow to "triage"
- "security_scan" arrow to "hard_pause"
- "design_delta" arrow to "hard_pause", with a second dashed amber arrow curving back to the left labeled exactly "route to Phase 2c"

Title at the top reads exactly "PR stabilization escalation routing".

Small caption at the bottom reads exactly "classifier labels each failure; orchestrator picks the handling path".

Minimal line icons inside each left box: a timer for flaky_test, a cross-arrows for merge_conflict, a robot for ai_review_unresolved, a person for human_review, a shield for security_scan, a drafting-triangle for design_delta. Clear hierarchy, consistent stroke width, no photo-realistic elements.
