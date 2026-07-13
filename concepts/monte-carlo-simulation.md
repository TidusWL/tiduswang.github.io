---
title: 蒙特卡洛模拟在博弈中的应用
created: 2026-05-21
updated: 2026-05-21
type: concept
tags: [probability, simulation, tutorial, reference]
sources: [concepts/probability-in-gaming.md]
confidence: high
---

# 蒙特卡洛模拟在博弈中的应用

蒙特卡洛模拟通过大量随机抽样来近似复杂概率问题的解，在博弈分析中非常实用。

## 核心思想

当解析解难以计算时：
1. 建立概率模型
2. 进行大量随机试验
3. 统计结果频率 → 近似概率

## 实例：足球世界杯夺冠概率

```python
import random
from collections import Counter

def simulate_match(team_a_strength, team_b_strength):
    """泊松分布模拟进球"""
    goals_a = random.poissonvariate(team_a_strength * 1.5)
    goals_b = random.poissonvariate(team_b_strength * 1.2)
    
    if goals_a > goals_b: return 'A'
    elif goals_b > goals_a: return 'B'
    else: return 'draw'

def simulate_tournament():
    """模拟完整锦标赛"""
    teams = {
        '巴西': 1.8, '法国': 1.7, '阿根廷': 1.6,
        '德国': 1.5, '西班牙': 1.5, '英格兰': 1.4
    }
    # 小组赛 → 淘汰赛
    # ... 完整赛程模拟
    return winner

# 模拟 10 万次
wins = Counter()
for _ in range(100000):
    winner = simulate_tournament()
    wins[winner] += 1

for team, count in wins.most_common():
    print(f"{team}: {count/1000:.1f}%")
```

## 应用场景

| 场景 | 模拟内容 |
|------|---------|
| 体育赛事 | 比赛结果、夺冠概率 |
| 扑克 | 手牌胜率、EV 计算 |
| 投资组合 | 收益分布、风险度量 |
| 项目管理 | 工期估算、风险评估 |
| 游戏平衡 | 角色强度、胜率分析 |

## 精度与效率

- 精度 ∝ √N（N 为模拟次数）
- 10,000 次 → 误差约 1%
- 100,000 次 → 误差约 0.3%
- 1,000,000 次 → 误差约 0.1%

## 方差缩减技术

| 技术 | 原理 |
|------|------|
| 对偶变量 | 使用负相关的随机数 |
| 控制变量 | 利用已知期望的变量 |
| 重要性采样 | 聚焦关键区域 |
| 分层抽样 | 分层覆盖样本空间 |

## 相关页面
- [[probability-in-gaming]]
- [[sports-betting-ev]]
