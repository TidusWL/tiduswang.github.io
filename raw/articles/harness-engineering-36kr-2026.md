---
source_url: https://eu.36kr.com/zh/p/3749464991187458
ingested: 2026-06-03
sha256: to-be-computed
---

# 一文读懂 Harness Engineering：让 AI 不再离经叛道的"壳"

> 来源：36氪 | 作者：Yousa 博阳 | 收录日期：2026-06-03

## 核心概念

**Harness（约束工程）** 是围绕大语言模型建立的工业级管理制度。模型是引擎，Prompt 是方向盘，Harness 是让整辆车能安全、连续运行的控制体系。

**关键数据**：LangChain 实验显示，仅更换更精巧的Harness架构，同一大模型在 Terminal Bench 2.0 上的通过率从 **52.8% 提升至 66.5%**。

## Harness 三层架构

### 第一层：流程管控

四种失败模式：提前交卷、环境盲区、虚标完成、失忆实习生综合征。

Anthropic 解决方案：JSON 物理锁、三步唤醒仪式、Git 存档与回滚、Context Reset（上下文重置）。

OpenAI 解决方案（Repo-as-truth）：仓库是唯一现实、可执行自动化检查、AGENTS.md 约100行、Doc-gardening Agent。

核心区别：Anthropic 管流程（行为），OpenAI 管环境（认知）。

### 第二层：并发控制

Cursor 方案：Planner-Worker-Judge 三层架构 + DAG 引擎。
Anthropic 方案（16个Claude并行写C编译器）：GCC 标准答案参照 + 二分查找定位 Bug。

### 第三层：验证机制

Anthropic 的 Generator-Evaluator 对抗机制：评估者被要求"try to break it"。
Cursor 的 8 通道并行盲审：同一代码差异打乱顺序，8个独立 Bugbot 各自发现 Bug。

## 从加法到减法

Harness 的每一个组件，都编码了一条关于模型做不到什么的假设。当假设不再成立，组件就该走了。

Claude Code 源码中发现的补偿面迁移：KAIROS（常驻后台守护程序）、YOLO Classifier（风险标签自适应调节）、Hooks（8个关键节点插槽）。

## 核心结论

- Harness 里每个方块存在的理由都不是"它能做什么"，而是"模型做不到什么"
- 护城河不在厚度，在迁移速度
- 通往简单的路，必须经过复杂
