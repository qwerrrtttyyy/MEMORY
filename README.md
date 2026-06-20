# MEMORY — Agent Memory Backup

This repository contains a backup of agent memory and accumulated context for cross-session continuity.

## Contents

| File/Directory | Description |
|----------------|-------------|
| `mcp_memory_graph.json` | Exported knowledge graph from the `mcp_memory` MCP server. Contains entities, observations, and relations for active projects and workflows. |
| `memory/MEMORY.md` | Long-term hot memory: external agent routing rule, skills optimization state, and current main task. |
| `memory/2026-06-20.md` | Diary entry for 2026-06-20 with decisions, data, files, links, next steps, and issues. |
| `test-prompts/` | Generated test prompts for the 18 skills pending optimization. |
| `auto-optimize-results.tsv` | Optimization results tracker for the Darwin skill optimization process. |

## Key Context

- **Main task**: OrdPaw — add batch/multi-select download support, configurable storage, space limits, Browser/Server distributed storage, and refine New UI/UX performance/design.
- **Paused task**: Optimize all 18 skills under `/data/user/skills/*/SKILL.md` using the Darwin skill optimizer.
- **Workflow rule**: When invoking external agents (Qwen / WorkBuddy / OpenCode / Z.ai), consolidate context into a single file and let the user route.

## Last Updated

2026-06-20
