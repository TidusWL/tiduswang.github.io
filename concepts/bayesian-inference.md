---
title: 贝叶斯推断
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [probability, reference, algorithm]
sources: [scratch]
confidence: low
---

# 贝叶斯推断

> **Stub 页**：仅作为概率博弈知识体系的占位节点，详细内容待补充。

## 简要定义
**贝叶斯推断**（Bayesian Inference）：根据新观测数据，利用贝叶斯定理更新对未知参数的信念（先验 → 后验）。

核心公式：`P(A|B) = P(B|A)·P(A) / P(B)`

## 典型应用
- 医学检测：根据阳性结果反推真实患病概率
- 垃圾邮件过滤：不断学习新特征
- 博彩建模：根据赛果更新对球队/选手实力的估计

## 相关页面
- [[probability-in-gaming]] — 概率论在竞技博弈中的应用
- [[monte-carlo-simulation]] — 蒙特卡洛模拟方法

## 待补充
- 先验分布选择的影响
- 与频率学派的对比
- MCMC、变分推断等近似方法
