# Hot Memory

## 2026-06-20 — Skills 全量优化完成
**Context**: 完成 18 个 skills 的基线评估 + hill-climb 优化。
**Result**: 平均分从 72.7 提升至 78.7（+6.0）。0 回滚。全部 5 个薄弱 skill 脱离危险区。
**Key improvement**: 全部 skills 新增 keywords + tags 触发词字段，解决 Trae 不自动调用 Skills 的问题。
**Details**: [auto-optimize-results.tsv](/workspace/auto-optimize-results.tsv), branch `auto-optimize/20260620-2200`

## 2026-06-20 — 外部 Agent 路由规则
**Rule**: 当需要 Qwen / WorkBuddy / OpenCode / Z.ai 时，整合上下文到单一文件，由用户自动路由。
**Skill created**: agent-router 技能已创建到 `/data/user/skills/agent-router/SKILL.md`，含路由决策矩阵、4 阶段工作流、交接文件模板。

## 2026-06-20 — 安装的外部技能
**Installed**:
- **caveman** (7 skills): 减少 AI 输出 75% token
- **superpowers** (14 skills): TDD、子 Agent 驱动、代码审查等
- **Not installed** (too heavy): claude-mem, everything-claude-code

## 2026-06-20 — Skills 优化前状态（历史）
**State before optimization**:
- 18 skills under `/data/user/skills/*/SKILL.md`.
- Git initialized in `/data/user/skills`, branch `auto-optimize/20260620-0015`.
- Test prompts generated in `/workspace/test-prompts/*.json` (2-3 per skill).
- Baseline average: 72.7/100.
- **已完成：基线评估 + 全量优化循环 + Phase 3 汇总报告**.

## 2026-06-20 — OrdPaw Main Task
**Task**: Add download manager support (batch/multi-select for AI conversations, programs, files, Skills, MCP, Scripts, source code). Configurable storage location & space limits. Distributed storage (Browser, Server). Fix New UI/UX animation performance.
**Status**: Awaiting project source files via git clone.
**Next step**: Obtain OrdPaw repository URL and clone into /workspace/ordpaw.
