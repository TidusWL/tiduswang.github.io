---
title: AI-Augmented Scrum 框架
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [ai, agent, framework, project, team]
sources: [scratch]
confidence: medium
---

# AI-Augmented Scrum 框架

## 概述
当 Scrum 团队中 **50% 的"开发者"是 AI Agent** 时，传统 Scrum Guide 的规则开始失效——Agent 不睡觉、不疲劳、不会因重复任务倦怠，但**缺乏业务上下文**。AI-Augmented Scrum 是 2025-2026 年间为应对这一现实演化出的新框架范式，核心思路不是"用 AI 跑 Scrum"，而是"重构 Scrum 流程以容纳人机混合团队"。

## 背景与触发条件
- 传统 Scrum 假设 5-9 人、跨职能、全员同步
- 当 AI Coding Agent（Claude Code / Cursor / Bolt）、文档 Agent、测试 Agent 进入团队后，**Sprint Backlog 中出现可由 Agent 异步完成的任务**
- 触发阈值的经验法则：**AI Agent 占比 ≥ 30%**，需要开始重构仪式

## 框架核心变化

### 1. Product Owner 角色强化
- 仍是 Scrum Team 中唯一对"业务价值"负主责的角色
- 需要将**业务上下文显式化**（AI Agent 没有隐性的领域知识）
- 产出：可机器读取的 Acceptance Criteria（含边界条件、异常路径、DoD）

### 2. Sprint Backlog 重构
- 任务**原子化**：拆到 AI Agent 可以在一次 Context 内完成
- 每个任务附带：明确的输入/输出契约、验证命令、失败回滚条件
- 引入 **"AI 可接"标签**（如 `ai-ready`）和 **"需要人类判断"标签**（如 `human-required`）

### 3. Daily Scrum 异步化
- **AI Agent 不需要开口汇报**——它们以异步状态报告形式输出（PR 状态、测试通过率、Token 消耗）
- 人类成员**仍保留 15 分钟同步会议**，但议程变为"人类之间 + 人类对 Agent 的引导"
- 时长从 15 分钟压缩到 5-8 分钟

### 4. Sprint Review 扩展
- 传统：演示完成的功能
- 新增：**演示 AI Agent 自主完成的工作**（如自动生成的测试覆盖率、批量重构的 PR 集）
- 利益相关者需要理解"哪些是 AI 做的、哪些是人做的"——透明度是信任基础

### 5. Retrospective 增列
- 新增维度：**AI 协作健康度**
  - Agent 误解需求的频率
  - 人类花在"纠正 AI 输出"上的时间占比
  - 上下文交接失败次数（Agent 切换任务时的信息丢失）
  - Token 成本 / 业务价值 比

## AI Scrum Master 新职责
传统 SM 的仪式协调 + 障碍清除职责之外，新增三大能力：

1. **数据驱动决策**：把 velocity、burndown、cycle time 翻译成**业务结果**（不是"我们完成了 30 个故事点"，而是"我们为客户节省了 X 小时"）
2. **仪式自动化**：自动生成 standup 摘要、retro 情绪分析、Sprint 报告初稿
3. **预测性洞察**：基于历史数据预警 Sprint 是否能完不成，自动识别瓶颈（哪个 AI Agent 卡住哪个任务）

## 配套培训与认证
- **AI 4 Agile BootCamp**（Stefan Wolpers，2025-09 发布）——目前最体系化的 PM+AI 方法论课程
- 强调 **ethics + clarity + purpose** 三原则，明确反对"用 AI 替代 PM 决策"
- 目标受众：Product Owner、PM、Scrum Master、Agile Coach

## 相关概念
- [[mcp-protocol]] — AI Agent 调用工具的协议底座
- [[agentic-ai-scrum-protocols]] — 多 Agent 协作与人机混合的协议层（MCP/A2A/HITL）
- [[harness-architecture]] — AI Agent 运行时控制框架，与 Scrum 流程的 Agent 治理相通

## 开放问题
- **可解释性 vs 速度**：AI 决策要可追溯（学术要求）vs 业务要求快速试错（实操要求）如何平衡？
- **Backlog 原子化的粒度**：拆太细增加管理成本，拆太粗 AI 完不成，**经验值是多少？**
- **AI 贡献的归属**：当 AI 生成 95% 的代码（YC W25 数据）时，**Sprint Velocity 怎么计算？还合理吗？**
- **Retrospective 的"AI 摩擦"维度**：会不会变成新的形式主义？

## 实践建议（PM 视角）

| 阶段 | 动作 | 时间窗 |
|------|------|--------|
| 试点 | 在现有 Scrum 引入 1 个 AI Agent（如文档 Agent、代码 Agent） | 立即 |
| 拆分 | 重构 Backlog，任务拆得更原子化、可验证 | 1-2 月 |
| 规范 | 引入 Human-in-the-loop 规范：哪些决策 AI 可做、哪些必须人审 | 3-6 月 |
| 演进 | Retro 增加 "AI 协作健康度" 维度 | 持续 |

## 来源与参考
- Agile Leadership Day India (2026-03) — "How to Run Scrum When Half Your Team is AI Agents"
- ResearchGate (2025-10) — "AI-Augmented Scrum: A Unified, Explainable Framework for Agile Software Development"
- LeanWisdom (2025) — "Generative AI for Scrum Masters: Revolutionizing Agile Leadership by 2025"
- Refonte Learning (2026) — "Scrum Master in 2026: New Strategies, AI-Driven Agile Leadership"
