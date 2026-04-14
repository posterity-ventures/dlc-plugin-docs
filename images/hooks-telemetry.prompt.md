# Prompt — hooks-telemetry.png

**Include** [_style.md](_style.md) preamble before the prompt body.

**Parameters**: `aspectRatio: 16:9`, `imageSize: 2K`

## Prompt body

A clean, modern technical diagram in flat vector illustration style showing how hooks and telemetry plug into the Claude Code tool-call lifecycle. Title banner at the top spelled exactly: "Claude Code tool-call lifecycle". Dark-mode-friendly deep slate background (#1e293b). Soft blue (#60a5fa) for the main flow and warm amber (#fbbf24) for the hook insertion points and telemetry sink. Sans-serif typography, crisp labels. A horizontal main flow left to right with four boxes labeled exactly: "Agent decides tool call", "PreToolUse", "Tool executes", "PostToolUse". Between steps 1 and 2 and between 3 and 4, amber arrows branch off downward to smaller boxes labeled exactly "hook shell command" representing the hook firing. From the PostToolUse box, a dashed amber arrow points down to a bottom box labeled exactly "${DLC_ARTIFACT_ROOT:-.dlc}/slug/telemetry.jsonl" representing the append-only event log. Small caption at the bottom reads "Each hook event → one JSONL line". Minimal line icons — a robot for agent, a shield for PreToolUse, a gear for execute, a checkmark for PostToolUse, a terminal for hook, a document for telemetry. Clear hierarchy, consistent stroke width, no photo-realistic elements.
