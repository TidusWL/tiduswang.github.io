---
title: Harness vs Framework — 架构范式对比
created: 2026-06-03
updated: 2026-06-03
type: comparison
tags: [ai, framework, system-design, comparison]
sources: [raw/articles/harness-engineering-36kr-2026.md, raw/articles/harness-six-components-tencent-2026.md]
confidence: high
---

# Harness vs Framework — 架构范式对比

## 核心区别

| 维度 | Framework（框架） | Harness（约束工程） |
|---|---|---|
| **定位** | 工具箱 | 带装修的房子 / 操作系统 |
| **理念** | 提供工具，由开发者决策 | 内置最佳实践，开箱即用 |
| **主见程度** | 低（Unopinionated） | 高（Opinionated） |
| **复杂度** | 高（需要大量配置） | 低（约定优于配置） |
| **代表** | LangChain, LangGraph | DeerFlow 2.0, Cursor, Claude Code |

## 详细对比

| 维度 | LangChain/LangGraph | Harness 模式 |
|---|---|---|
| **并行能力** | 需手动实现 | 内置编排 |
| **身份隔离** | 需配置 | Profile 自动隔离 |
| **记忆管理** | 需接入外部存储 | AGENTS.md + 文件系统 |
| **凭证管理** | 需自行管理 | 集中密钥管理 |
| **验证机制** | 无内置 | Generator-Evaluator 对抗 |
| **学习成本** | 高 | 低 |

## LangGraph 是 Harness 吗？

**不是**。LangGraph 是编排框架，提供 DAG 构建能力，但不内置记忆、凭证隔离、验证机制等 Harness 核心组件。

## 结论

- **用 Framework**：需要完全自定义 Agent 行为
- **用 Harness**：需要快速构建生产级 Agent 系统


## 相关概念

- [[harness-architecture]] — Harness 架构体系总览
