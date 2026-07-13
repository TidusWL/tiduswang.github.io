---
title: A 股数据源测评
created: 2026-06-16
updated: 2026-06-16
type: concept
tags: [finance, data-source, ashare, tool]
sources: [web-search-2026-06-16, user-knowledge]
confidence: medium
---

# A 股数据源测评

> 用于 [[watchlist-2026]] 分析。用户的券商是东方财富，
> 优先使用东财免费数据 + 第三方公开 API。

## 评估维度

- ✅ 可用性（不需付费即可拉到）
- ✅ 实时性（延迟 ≤ 15 分钟）
- ✅ 历史深度（≥ 3 年日线 / 5 年财务）
- ✅ 覆盖度（A 股全市场 + 北交所）
- ⚠️ 稳定性（频次限制 / IP 风控）
- ⚠️ 合规性（爬虫风险）

## 数据源清单

### 1️⃣ 东方财富（efinance / 东财网页）
| 维度 | 评分 |
|---|---|
| 可用性 | 🟢 免费，公开网页可爬 |
| 实时性 | 🟢 实时 |
| 历史深度 | 🟢 5+ 年 K 线、10+ 年财务 |
| 覆盖度 | 🟢 A 股全 + 港股通 |
| 稳定性 | 🟡 反爬中等 |
| 合规性 | 🟡 仅供个人研究，不要重发 |

**用法**：
- F10 个股页：`https://emweb.securities.eastmoney.com/PC_HSF10/CompanySurvey/Index?type=web&code=SH600519`
- 资金流向：`https://data.eastmoney.com/zjlx/detail.html`
- 研报：`https://data.eastmoney.com/report/singlestock.html`

**风险**：
- 个别页面有 Cookie 反爬
- 移动端 mweb 限制更少

### 2️⃣ akshare（开源 Python 库）
| 维度 | 评分 |
|---|---|
| 可用性 | 🟢 pip install akshare 即用 |
| 实时性 | 🟡 15 分钟延迟（多数接口） |
| 历史深度 | 🟢 完整 |
| 覆盖度 | 🟢 A 股 + 港股 + 美股 + 期货 + 基金 + 宏观 |
| 稳定性 | 🟡 部分接口偶发失效 |
| 合规性 | 🟢 明确开源协议，但底层数据源是上交所/深交所/东财等 |

**用法**：
```python
import akshare as ak
df = ak.stock_zh_a_hist(symbol="600519", period="daily", start_date="20240101")
```

**优点**：直接 DataFrame，最方便做技术分析
**风险**：AkShare 维护者单人项目，接口稳定性看运气

### 3️⃣ 同花顺 iFinD / 问财
| 维度 | 评分 |
|---|---|
| 可用性 | 🟡 网页版免费，API 收费 |
| 实时性 | 🟢 实时 |
| 历史深度 | 🟢 5+ 年 |
| 覆盖度 | 🟢 全市场 |
| 稳定性 | 🟢 商业级 |
| 合规性 | 🟢 |

**用法**：
- 问财 NLP 查询：`https://www.iwencai.com/unifiedwap/Index?code=600519`
- 研报中心：`https://basic.10jqka.com.cn/603533/event.html`

**优点**：自然语言查询最方便
**风险**：API 收费高（年费 5k+），免费版限制大

### 4️⃣ 雪球
| 维度 | 评分 |
|---|---|
| 可用性 | 🟡 网页免费，爬虫需登录 |
| 实时性 | 🟢 实时（15 分钟延迟版） |
| 历史深度 | 🟡 财务深度一般 |
| 覆盖度 | 🟡 A 股 + 港股 + 美股 |
| 稳定性 | 🟡 反爬严 |
| 合规性 | 🟡 雪球 ToS 严格 |

**优点**：散户情绪数据最丰富（讨论热度、关注变化）
**风险**：ToS 限制，不建议大规模爬

### 5️⃣ 巨潮资讯网（cninfo）
| 维度 | 评分 |
|---|---|
| 可用性 | 🟢 官方公告，免费 |
| 实时性 | 🟢 T+0 |
| 历史深度 | 🟢 全部历史公告 |
| 覆盖度 | 🟢 A 股全 |
| 稳定性 | 🟢 官方源 |
| 合规性 | 🟢 官方源 |

**用法**：`https://www.cninfo.com.cn/new/disclosure/stock?stockCode=600519`

**优点**：官方公告，权威
**风险**：数据需解析 HTML

## 推荐组合（按使用场景）

| 场景 | 推荐数据源 |
|---|---|
| 实时行情看盘 | 东方财富网页 |
| K 线 + 技术分析 | akshare（DataFrame）|
| 财务三表 | 巨潮资讯 + akshare 财务接口 |
| 资金流向 | 东方财富资金流页 |
| 研报 + 目标价 | 同花顺 + 东方财富研报中心 |
| 散户情绪 | 雪球热帖（手工观察） |
| 公告事件 | 巨潮资讯 |
| 新闻舆情 | Tavily web_search |

## 关联页面

- [[watchlist-2026]]
- [[ai-stock-analysis-framework]]
- [[finance-investing]]
