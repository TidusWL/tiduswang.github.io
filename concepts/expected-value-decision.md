---
title: 期望值决策
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [probability, decision, finance, reference]
sources: [scratch]
confidence: low
---

# 期望值决策

> **Stub 页**：仅作为概率博弈与金融知识体系的占位节点，详细内容待补充。

## 简要定义
**期望值（Expected Value, EV）**：随机变量所有可能取值按概率加权的平均值。
- 公式：`E(X) = Σ(Pi × Vi)`
- **核心决策原则**：长期重复下，选择 EV > 0 的决策

## 决策框架
1. 识别所有可能结果
2. 估算每种结果的概率
3. 计算期望值
4. 选择 EV > 0 的决策
5. 用凯利公式控制单次下注比例
6. 持续用贝叶斯更新概率

## 典型应用
- **德州扑克**：胜率 × 底池大小，决定是否跟注
- **体育博彩**：EV > 0 的盘口才投注
- **金融交易**：开盘溢价率 + 期望值框架判断次日走势
- **涨停股次日操作**：结合溢价率区间与量能

## 相关页面
- [[sports-betting-ev]] — 体育博彩期望值与凯利公式
- [[opening-premium-rate]] — 涨停股次日操作策略
- [[probability-in-gaming]] — 概率论在竞技博弈中的应用
- [[poker-probability]] — 德州扑克概率分析

## 待补充
- 多目标决策下的扩展（风险/收益权衡）
- 风险厌恶型决策者（效用理论）
