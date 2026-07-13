---
title: Context Reset — Harness 上下文重置机制
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, system-design, context-management, agent]
sources: [raw/articles/harness-engineering-36kr-2026.md]
confidence: high
---

# Context Reset — Harness 上下文重置机制

## 什么是 Context Reset？

**Context Reset（上下文重置）** 是 Anthropic 在长程 Agent 任务中提出的一种 Harness 技术：当历史消息撑爆上下文窗口时，Harness 彻底清空 Agent 的上下文，启动一个全新的 Agent，仅通过一份**结构化的交接文件**将前一轮的状态和下一步任务传递过去。

> "不是压缩记忆，是直接换一条新金鱼，只给它一张写好的交接单。"

## 为什么需要 Context Reset？

传统上下文压缩（Compaction）的局限：
- 即使压缩了历史，模型在超长上下文里仍然会焦虑、丢失连贯性
- 注意力分散，"大海捞针"问题严重

Context Reset 更激进但更有效：彻底清空，给一张白纸，让模型重新集中注意力。

## 实现要点

- **交接文件**：结构化的任务状态描述，包含已完成工作、当前状态、下一步指令
- **触发条件**：上下文窗口接近满载时自动触发
- **跨 Session 延续**：每轮 Session 强制执行 `pwd` → `git log` → `progress.txt` 三步唤醒仪式

## 演进趋势

随着模型上下文管理能力的增强（如 Opus 4.6），Context Reset 已不再是必要组件。Anthropic 的实际经验表明：**Harness 的每个组件都应该在模型能力提升时重新评估是否需要保留**。

## 相关概念

- [[harness-architecture]] — Harness 架构体系总览
- [[agent-long-running]] — 长程 Agent 任务管理（待扩展）
