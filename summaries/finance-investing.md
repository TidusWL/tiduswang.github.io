---
title: 投资领域总览
created: 2026-06-16
updated: 2026-06-16
type: summary
tags: [finance, summary, ashare, ai-investing]
sources: [ai-stock-analysis-framework, data-sources-ashare, watchlist-2026]
confidence: medium
---

# 投资领域总览

> 用户投资画像：纯 A 股 / 混合型（长中短线） / 东方财富 / AI 建议+用户决策

## 核心原则

1. **AI 不下单** — 合规边界，只生成分析报告
2. **混合型风格** — 报告分短/中/长线三个时间维度
3. **承认错误** — 预测错误要复盘归档到 [[watchlist-2026]] 已清仓区
4. **风险第一** — 每份报告必带风险提示

## 当前能力图（2026-06-16）

### ✅ 已就绪
- **分析框架**：[[ai-stock-analysis-framework]]（4 维度：基本面/技术/资金/情绪）
- **数据源**：[data-sources-ashare](data-sources-ashare)（东方财富 + akshare + 同花顺 + 雪球 + 巨潮）
- **AI 工具**：
  - Tavily web_search（新闻舆情）
  - AnyGen（生成 K 线图、研报 PDF、PPT）
  - LLM 综合分析（基本+技术+资金+情绪）

### ⏳ 待用户输入
- [[watchlist-2026]] — 自选股清单（**空**）

## 行业新热点（2026-06）

### 1. TradingAgents（开源多 agent 框架）
- 6 个 agent 模拟（基本面/情绪/技术/研究员/交易员/风控）辩论决策
- GitHub: https://github.com/tauricresearch/tradingagents
- 论文: https://tradingagents-ai.github.io
- 适合：技术派/研究派本地部署

### 2. 富途 Skill（富途 OpenAPI + AI 插件）
- 港股/美股实时行情 + 13 种订单类型
- 交易需手动输入密码（安全设计）
- 文档: https://www.futuhk.com/blog/detail-futu-skills-102-260475002
- 对你适用性：低（你是 A 股）

### 3. TickDB Skill
- A 股/港股/加密行情跨市场查询
- 需 Claude Code 等 ClawHub 环境
- 对你适用性：中（仅行情查询，没有分析）

### 4. 主流 AI 股票工具（2026 综述）
来源：TradingKey 综述 https://www.tradingkey.com/zh-hans/analysis/stocks/us-stock/261586691-ai-tools-stock-analysis-investment
- **Kavout**：美股 AI 选股（Kai 评分）
- **Danelfin**：3 个月跑赢标普 500 概率评分
- **TradingKey 股票评分**：34 项指标聚合
- **TrendSpider**：技术分析自动化
- **Seeking Alpha Premium**：虚拟分析师
- **Fiscal.ai / Composer / Magnifi**：AI 财报分析
- 多数是英文工具 + 美股为主，**对 A 股的覆盖有限**

### 5. 国信证券 6 月策略（仅供参考，非投资建议）
- 看好：腾讯/阿里/百度（港股）/美团
- 谨慎：京东/拼多多/快手
- 看淡：游戏/短视频
- 关注：AI Agent + 国产算力

## 开放问题

- 自选股清单何时确定？建议 5-15 只为宜
- 是否需要日级推送？建议**交易日晚上 20:00** + **周末复盘**
- 历史回测：是否对历史预测做 accuracy 统计？
- 是否需要"打脸复盘"机制来提升 LLM 提示词

## 关联页面

- [[watchlist-2026]]
- [[ai-stock-analysis-framework]]
- [[data-sources-ashare]]

## 操作日志

- 2026-06-16：建立投资领域框架，4 个页面就位，待用户填自选股
