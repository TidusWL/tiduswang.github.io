---
title: MCP — Model Context Protocol
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, protocol, tool-integration, open-source, framework]
sources: [raw/articles/harness-six-components-tencent-2026.md]
confidence: high
---

# MCP — Model Context Protocol

## 什么是 MCP？

**MCP（Model Context Protocol）** 是 Anthropic 推出的开放协议，定义了 AI 模型与外部工具的标准化交互方式。

> 比喻：**MCP = AI 世界的 USB 接口**

## 为什么需要 MCP？

在 Harness 体系中，MCP 解决了模型"知识过时"和"无法操控外部工具"的硬伤：

- 统一工具接入标准，避免每个工具单独适配
- 支持实时监控、代码仓库、数据库等外部系统连接
- 实现"修复生产Bug"级别的综合任务

## MCP 在 Harness 中的位置

MCP 是 Harness 六大组件中**Web搜索 + MCP** 组件的核心部分：

- Web搜索：获取实时信息和知识
- MCP：连接外部工具和数据源

典型协同流程：
1. Web搜索搜索错误信息 → 找到相关讨论
2. MCP连接监控系统 → 获取实时指标
3. MCP连接代码仓库 → 查看最近变更
4. 综合分析 → 定位根因 → 生成修复方案

## 技术特点

- 标准化接口
- 支持多模型接入
- Nous 维护了官方 MCP 服务器目录

## 相关概念

- [[harness-architecture]] — Harness 架构体系总览
- **编排组件** — 见 [[harness-architecture]] 的"编排 + Hooks"小节
