---
title: 概率博弈知识体系总览
created: 2026-05-26
updated: 2026-05-26
type: summary
tags: [probability, game-theory, summary]
sources: [synthesis]
---

# 概率博弈知识体系总览

## 领域概述

本领域覆盖概率论在竞技博弈和投资决策中的应用，从基础概率模型到高级模拟方法，形成完整的分析框架。

## 核心知识地图

### 基础方法
- [[probability-in-gaming]] — 概率论基础：古典概率、贝叶斯推断、期望值、分布模型
- [[monte-carlo-simulation]] — 蒙特卡洛模拟：通过随机抽样解决复杂概率问题

### 博弈场景
- [[poker-probability]] — 德州扑克：不完全信息博弈，胜率 vs 赔率决策
- [[blackjack-strategy]] — 21点：完全信息博弈 + 算牌获得统计优势
- [[rock-paper-scissors]] — 石头剪刀布：零和博弈的纳什均衡与人类行为偏差

### 投资决策
- [[sports-betting-ev]] — 体育博彩：期望值分析与凯利公式
- [[opening-premium-rate]] — A股交易：开盘溢价率与涨停股次日策略

## 核心思维框架

```
1. 识别所有可能结果
2. 估算每种结果的概率（基础频率 / 贝叶斯更新）
3. 计算期望值 EV = Σ(Pi × Vi)
4. 选择 EV > 0 的决策
5. 用凯利公式控制风险
6. 持续用贝叶斯更新概率
```

## 开放问题与争议

- **有效市场假说 vs 行为金融**：概率优势在多大程度上可战胜市场？
- **凯利公式的实用性**：真实场景中估计误差对凯利策略的影响
- **AI博弈中的纳什均衡**：深度强化学习能否收敛到纳什均衡？

## 推荐阅读顺序

1. 先读 [[probability-in-gaming]] 建立基础
2. 选一个感兴趣的场景深入（扑克 / 21点 / 体育博彩）
3. 用 [[monte-carlo-simulation]] 实践模拟方法
4. 读 [[sports-betting-ev]] 或 [[opening-premium-rate]] 了解实际投资应用
