---
name: agent-router
description: 外部 Agent 路由分发技能。当用户请求更适合由其他 AI Agent（Qwen/通义千问、WorkBuddy、OpenCode、Z.ai、Claude Code 等）处理时，自动检测并执行路由：整合上下文到单一文件，生成路由指令，完成安全交接。Triggers when user asks for capabilities that belong to another agent, or when cross-agent handoff is needed.
keywords:
  - 外部Agent
  - Agent路由
  - 路由分发
  - 跨Agent协作
  - 任务交接
  - agent routing
  - handoff
  - task routing
  - cross-agent
  - Qwen
  - WorkBuddy
  - OpenCode
  - 通义千问
  - agent handoff
  - 路由
  - 外部模型
tags:
  - 路由
  - 跨Agent
  - 交接
  - routing
  - handoff
  - orchestration
---

# Agent Router - 外部 Agent 路由分发

> 当用户请求更适合由其他 AI Agent 处理时，自动路由到正确的目标。

核心理念：**不替代 → 不阻塞 → 完整交接**

---

## 设计哲学

1. **识别边界** — 识别当前 AI 的能力边界，把不适合的问题交给更合适的 Agent
2. **最小干扰** — 只在确实需要路由时介入，不过度推测
3. **完整上下文** — 打包所有必要信息（用户意图、对话历程、已有产出），确保目标 Agent 即开即用
4. **可追溯** — 每次路由都记录原因、目标、交接内容，支持复盘

---

## 路由决策矩阵

| 用户请求特征 | 目标 Agent | 路由原因 |
|-------------|-----------|---------|
| 中文通用知识/创作/问答 | **Qwen（通义千问）** | 中文语料更丰富 |
| 工作流、OA、企业系统操作 | **WorkBuddy** | 企业工作台集成 |
| 深度代码开发、大规模重构 | **OpenCode** | 专注代码工程 |
| 深度研究/多轮推理/复杂分析 | **Z.ai** | 深度推理能力强 |
| 命令行/终端操作指导 | **Claude Code** | 命令行原生支持 |
| 上述 Agent 之间的协作接力 | **按需路由** | 跨 Agent 工作流 |

---

## 工作流

### Phase 1: 识别路由需求

```
输入：用户原始请求
动作：
  1. 分析请求是否在当前 AI 能力范围内
  2. 如果当前 AI 能高质量完成 → 不需要路由，直接处理（本 skill 不介入）
  3. 如果请求更适合其他 Agent → 进入 Phase 2
  4. 如果请求需要多个 Agent 协作 → 标记为多阶段路由
输出：路由决策（目标 Agent / 路由原因 / 紧急程度）
```

[CHECKPOINT] 确认路由决策 — 向用户展示"建议路由到 [Agent名称]，原因是 [具体原因]"，等待用户确认。

### Phase 2: 打包上下文

```
输入：原始请求 + 当前对话上下文
动作：
  1. 提取关键信息：
     - 用户原始问题
     - 已完成的上下文/产出物
     - 关键文件/代码/数据引用
     - 用户偏好（语言、风格、约束）
  2. 生成交接文件到 /workspace/handoff-{timestamp}.md：
     - 文件头：目标 Agent、优先级、路由时间
     - 上下文摘要：用户意图、已做的工作、关键产出物路径
     - 任务描述：清晰告诉目标 Agent 需要做什么
     - 约束说明：格式要求、参考风格、已知限制
     - 附件清单：相关文件的绝对路径列表
  3. 验证交接文件完整性
输出：交接文件（handoff-*.md）
```

### Phase 3: 执行路由

```
输入：交接文件 + 用户确认
动作：
  1. 如果是 CLI 可用目标（如 Claude Code）：
     - 直接生成启动命令 + 交接文件路径
  2. 如果是 API/平台目标（如 Qwen、WorkBuddy）：
     - 提供交接文件内容展示，指导用户手动粘贴或导入
  3. 如果是文件传递目标（如 OpenCode）：
     - 展示交接文件路径，指导目标 Agent 读取
  4. 跨 Agent 工作流：
     - 生成阶段路由计划（Phase 1 → Phase 2 → ...）
输出：路由指令 + 交接文件
```

[CHECKPOINT] 确认执行方式 — 确认用户偏好哪种路由方式（CLI 命令 / 手动粘贴 / 文件传递）。

### Phase 4: 跟进与闭环

```
输入：路由执行结果（可选）
动作：
  1. 如果用户返回汇报结果：
     - 将结果追加到交接文件
     - 记录路由到 auto-route-log.tsv
  2. 如果需要二次路由：
     - 回到 Phase 1，继续下一跳
  3. 如果是跨 Agent 工作流的最后一步：
     - 生成完整报告
输出：路由日志 / 最终报告
```

---

## Checkpoints

| # | 位置 | 操作 | 等待条件 |
|---|------|------|---------|
| 1 | Phase 1 → Phase 2 | 展示路由决策给用户 | 用户确认"是，路由到 X" |
| 2 | Phase 3 执行前 | 确认路由执行方式 | 用户选择 CLI/手动/文件 |
| 3 | Phase 4 二次路由前 | 确认是否需要继续 | 用户说"继续"或"停止" |
| 4 | 跨 Agent 接力 | 每跳之前展示完整计划 | 用户确认阶段计划 |

---

## 边界条件

### 不需要路由的场景
- 用户只是提及另一个 Agent 的名字，但问题仍在当前能力范围内
- 用户询问"应该用哪个 Agent"这类元问题（应直接回答，不路由）
- 当前 AI 可以调用 MCP/Skill 来完成（先用内部能力，失败再路由）

### 路由降级策略
- **目标 Agent 不可用**：展示交接文件内容，建议用户手动保存并在目标环境中打开
- **交接文件生成失败**：退化为"直接展示关键信息摘要 + 建议用户手动整理"
- **用户拒绝路由**：回到当前 AI 处理模式，尽最大能力完成
- **跨 Agent 链路断裂**：保存中间产物路径，告知用户当前进度

### 路由安全规则
- 不在交接文件中包含敏感凭据（API Key、Token、密码）
- 用户确认前不执行任何路由操作
- 路由日志保留 30 天（/workspace/auto-route-log.tsv）

---

## 交接文件模板

```markdown
# Agent Handoff

## Meta
- Target Agent: [Agent 名称]
- Priority: [高/中/低]
- Routed At: [时间戳]
- Routed By: agent-router

## Context Summary
[2-3 句话描述用户意图和背景]

## Task Description
[清晰描述目标 Agent 需要完成的任务]

## Constraints
- 语言要求：[中文/英文/双语]
- 输出格式：[markdown/代码/文档]
- 参考风格：[详细描述]
- 已知限制：[描述]

## Related Files
- [文件路径 1]
- [文件路径 2]

## Previous Work
[已经完成的工作或产出物描述]
```

---

## 路由日志格式

文件位置：`/workspace/auto-route-log.tsv`

```tsv
timestamp	target_agent	reason	status	handoff_file
2026-06-20T10:00	Qwen	中文问答	completed	/workspace/handoff-20260620-1000.md
```