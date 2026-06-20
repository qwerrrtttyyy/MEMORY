# Hot Memory

## 2026-06-20 — External Agent Routing Workflow
**Context**: User clarified available external agents and the request pattern.
**Rule**: When needing Qwen / WorkBuddy / OpenCode / Z.ai, consolidate all context, current task state, test prompts, and the specific request into a single file, then ask the user to route. Do not specify which agent to call; let the user auto-route.
**Applies to**: Any multi-agent task requiring external capabilities.

## 2026-06-20 — Skills Optimization State
**Context**: User asked to optimize all skills, then clarified it is not the current main task but should be remembered.
**State**:
- 18 skills under `/data/user/skills/*/SKILL.md`.
- Git initialized in `/data/user/skills`, branch `auto-optimize/20260620-0015`.
- Test prompts generated in `/workspace/test-prompts/*.json` (2-3 per skill).
- Results TSV at `/workspace/auto-optimize-results.tsv`.
- Paused before Phase 1 baseline evaluation (structure + effect dimensions).
**Next step when resumed**: Confirm test prompts, run baseline evaluation, then hill-climb optimize from lowest score.

## 2026-06-20 — OrdPaw Main Task
**Context**: User defined the current main development task.
**Task**: Add OrdPaw support for downloading AI conversations, AI-generated programs, files, Skills, MCP, Scripts, OrdPaw source code, etc., with batch and multi-select support. OrdPaw should support configurable storage location and space limits, and distributed storage (Browser, Server). Also fix New UI/UX animation performance and refine visual design.
**Status**: Awaiting project source files via git clone.
**Next step**: Obtain OrdPaw repository URL and clone into /workspace/ordpaw.
