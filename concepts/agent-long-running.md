---
title: 长程 Agent 任务管理
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [ai, agent, context-management]
sources: [scratch]
confidence: low
---

# 长程 Agent 任务管理

> **Stub 页**：详细策略待补充，先作为 Harness 与 AI-Augmented Scrum 知识网络的占位节点。

## 为什么是问题
AI Agent 在长程任务中面临三大挑战：
- **上下文窗口限制**：历史消息撑爆窗口 → 注意力涣散
- **错误累积效应**：5 步任务每步 90% 准确率 → 整体仅 59%
- **状态一致性**：跨 Session 重启后丢失进度

## 已知解决思路
- **Context Reset**（Anthropic） — 彻底清空上下文，用结构化交接文件延续任务
- **AGENTS.md 记忆机制** — 关键知识持久化到文件系统
- **Harness 流程管控** — 三步唤醒仪式（pwd → git log → progress.txt）
- **Sprint Contract** — 短周期原子化任务
- **多 Agent 协作** — 拆分长程任务给多个 Agent 分工

## 相关页面
- [[harness-architecture]] — Harness 架构体系
- [[harness-context-reset]] — Context Reset 上下文重置机制
- [[ai-augmented-scrum]] — AI-Augmented Scrum 框架

## 待补充
- 各种长程策略的实测对比
- 错误检测与回滚机制
- 与 2026 年多 Agent 编排架构的衔接
