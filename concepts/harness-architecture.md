---
title: Harness 架构体系
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, ml, system-design, framework, open-source]
sources: [raw/articles/harness-engineering-36kr-2026.md, raw/articles/harness-six-components-tencent-2026.md]
confidence: high
---

# Harness 架构体系

> **核心公式：Agent = Model（大脑）+ Harness（身体/操作系统）**

## 什么是 Harness？

**Harness**（原意"马具"）是围绕 AI 模型构建的**运行时控制与约束工程体系**。如果说大模型是引擎，Prompt 是方向盘，Harness 就是让整辆车安全、连续运行的变速箱、制动器和仪表盘。

**关键数据**：LangChain 实验显示，仅更换更精巧的 Harness 架构，同一大模型在 Terminal Bench 2.0 上的通过率从 **52.8% → 66.5%**。

## 裸模型的四大硬伤

模型就像一颗强大的引擎，但引擎本身不是汽车：

|| 硬伤 | 缺失能力 | Harness 对应组件 |
||---|---|---|
|| 无跨会话记忆 | 长期记忆 | 文件系统 + 记忆（AGENTS.md） |
|| 不能执行代码 | 行动能力 | Bash + 沙箱 |
|| 知识过时 | 感知能力 | Web搜索 + MCP |
|| 无工作环境 | 环境操控 | 文件系统 + 上下文工程 + 编排 |

## 六大核心组件

### 1. 文件系统

Harness 最基础的原语，提供三大核心能力：

- **工作空间与中间结果存储** — 中间产物写入文件，按需加载，突破上下文窗口限制
- **Agent 协作基础** — 多 Agent 通过文件系统实现异步协作（"共享白板"）
- **版本追踪与错误回滚** — 与 Git 集成，赋予 Agent 试错能力

设计原则：目录结构清晰、文件粒度适中（一个文件对应一个职责单元）、元数据丰富。

### 2. Bash + 沙箱

让 AI 从"说"到"做"的关键组件：

- **自我验证循环**：写 → 跑 → 看 → 修 → 再来
- 实际测试：具备自我验证循环的 Agent 任务完成率比"一次性生成"高出 **40%-60%**
- **沙箱安全机制**：资源限制（CPU/内存/磁盘）、网络隔离、文件系统隔离、超时自动终止
- **沙箱技术选型**：Docker 容器（主流）、gVisor/Firecracker（轻量虚拟化）、WASM（浏览器端）、Nix（声明式环境）

### 3. 记忆（AGENTS.md）

> **核心洞察：上下文注入 = 不改权重给模型加知识**

- **写入阶段**：Agent 将工作中产生的有价值知识写入 AGENTS.md
- **存储阶段**：存放在项目文件系统中
- **注入阶段**：下次启动时自动读取并注入上下文

最佳实践：层次化存放、结构化书写、定期清理、双向可编辑（人类可直接修改）。

### 4. Web搜索 + MCP

- **Web搜索**突破知识"时间牢笼"，关键问题：查询构建、结果筛选、内容提取、信息整合
- **MCP（Model Context Protocol）** = AI 世界的 USB 接口，Anthropic 推出的开放协议

### 5. 上下文工程

对抗 **Context Rot（上下文腐烂）** 的系统方法：

- **四大表现**：信噪比下降、矛盾信息累积、Token 浪费、推理质量退化
- **核心策略**：压缩、工具输出卸载、Skills 渐进加载、分层上下文结构
- 分层：核心层（始终保留）→ 工作层（按需更新）→ 历史层（逐渐压缩）

### 6. 编排 + Hooks

- **编排解决**：子 Agent 调度、任务分发、模型路由（简单任务用小模型，复杂任务用大模型）、结果聚合
- **模式演进**：线性管道 → DAG 编排 → 动态编排 → 层级编排
- **Hooks**：在关键节点插入检查（Lint、续接、格式约束、安全过滤、成本控制）
- **核心价值**：用确定性规则约束概率性输出——"概率性生成 + 确定性校验"

## 三层架构（Anthropic/OpenAI 实践）

| 层级 | 解决问题 | 典型方案 |
|---|---|---|
| **流程管控** | 模型记忆如金鱼 | JSON物理锁、三步唤醒仪式、Git存档回滚、Context Reset |
| **并发控制** | 多Agent混乱 | Planner-Worker门控、Git锁隔离、DAG引擎 |
| **验证机制** | 模型盲目自信 | Generator-Evaluator对抗、"try to break it"、多通道盲审 |

## 从加法到减法

Harness 里每个方块存在的理由都不是"它能做什么"，而是**"模型做不到什么"**。

随着模型能力变强，Harness 应该从加法转向减法：
- Context Reset 可拆（新模型上下文管理已足够强）
- Sprint Contract 可拆（新模型能自己把控节奏）
- Evaluator 从每轮对抗改为最后一轮 QA

## 关键结论

> 模型决定了 Agent 能力的下限，Harness 决定了 Agent 能力的上限。

- **护城河不在厚度，在补偿面迁移速度** — 知道下一寸该加什么，上一寸该拆什么
- **调试 Agent 时先检查 Harness**：文件系统完善吗？沙箱安全吗？记忆机制工作吗？上下文腐烂了吗？
- **如果你不是模型本身，那你就是 Harness 的一部分**

## 相关概念
- [[harness-context-reset]] — Context Reset 上下文重置机制
- [[mcp-protocol]] — MCP 协议详解
- [[harness-vs-framework]] — Harness vs Framework 的区别
- [[markdown-syntax]] — Markdown 语法参考（AGENTS.md 记忆机制的基础格式）
