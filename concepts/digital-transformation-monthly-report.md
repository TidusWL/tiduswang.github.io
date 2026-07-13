---
title: 数字化行业月报工作流
created: 2026-06-23
updated: 2026-06-23
type: concept
tags: [workflow, monthly, digital-transformation, industry-research, reporting]
sources: [news-briefing skill, weixin-delivery-debugging skill]
confidence: medium
---

# 数字化行业月报工作流

> 用户需求（2026-06-23 确立）：每隔一段时间（每月）了解行业动态和技术数字化相关资讯。**方案 A（极简版）**：手动触发，按固定流程跑。

## 触发条件

**触发关键词**（任一即可）：
- `跑下行业月报` / `生成月报` / `生成行业月报`
- `[月份]行业月报`（如 `7月行业月报` / `8月行业月报`）
- `行业动态月报` / `数字化月报` / `行业月报`

**触发窗口**：无限制（不像早报卡 8:00-8:55；月初任意时间都可以）

**首次启动**：2026 年 7 月 1 日前后

## 工作流（5 步，30-60 秒）

### 步骤 1：web_search 5 个固定查询（并行）

固定 query 模板（必须带"月报"或时间词，避免历史稳态热门）：

| # | 板块 | 固定 query |
|---|---|---|
| 1 | 国际趋势 | `数字化转型 最新 报告 月度 中国` |
| 2 | 政策标准 | `工信部 OR 发改委 OR 信通院 最新发布 月度` |
| 3 | AI 大模型 | `中国 AI 行业 最新 月度 OR 行业大模型 最新发布` |
| 4 | 行业方案 | `制造业 OR 金融业 OR 零售业 数字化 最新 月度` |
| 5 | 市场格局 | `中国 SaaS OR 工业互联网 厂商 融资 收购 月度` |

**时效性硬规则**：每条结果必须来自过去 30 天，> 60 天丢弃。

### 步骤 2：去重 + 筛选

- 每个 query 选 2-3 条最有价值的（事实陈述 > 情绪化标题）
- 跨板块去重（同一事件可能跨多个 query 命中）
- 优先：白皮书发布 > 头部厂商动作 > 政策落地 > 行业排名变化

### 步骤 3：生成 wiki 快照

**文件名**：`summaries/digital-transformation-monthly-YYYY-MM.md`

**frontmatter 模板**：
```yaml
---
title: 数字化行业月报 YYYY年M月
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: summary
tags: [digital-transformation, monthly-report, industry-research, summary]
sources: [digital-transformation-monthly-report]
period: YYYY-MM
period_months_covered: 1
---

# 数字化行业月报 |YYYY年M月

## 一、国际趋势动态
1. **标题** — 摘要 [来源](url)
2. ...

## 二、政策与标准
1. **标题** — 摘要 [来源](url)
2. ...

## 三、AI 与大模型
1. **标题** — 摘要 [来源](url)
2. ...

## 四、行业方案（制造/金融/零售/医疗）
1. **标题** — 摘要 [来源](url)
2. ...

## 五、市场格局（融资/收购/排名）
1. **标题** — 摘要 [来源](url]
2. ...

## 对你的项目启示
（1-2 段，结合用户的 PM 角色和已知自选股清单）

## 交叉引用
- 政策/标准：[[...]]
- AI/技术：[[harness-architecture]] [[mcp-protocol]] [[agent-long-running]]
- 框架方法：[[ai-augmented-scrum]] [[agentic-ai-scrum-protocols]]
```

### 步骤 4：推送微信（v7 格式）

**触发模式**：手动触发 → 自动走 final response（gateway 推送 weixin）

**格式硬约束**（沿用 [[news-briefing]] skill v7 规则）：
- 单条消息 ≤ 2000 字符（Hermes 硬上限，留 300-400 缓冲）
- 无 SPLIT 标记（用户已禁止分条，2026-06-16 规则）
- emoji 板块标题 + 加粗新闻标题
- 短来源标签（如 `[信通院]`、`[Gartner]`、`[36氪]`）而非 `[来源]`

**微信模板**：
```
📊 行业月报 |2026年M月

### 🌐 国际趋势动态
1. **标题** — 摘要 [Gartner](url)
2. ...

### 🏛️ 政策与标准
1. **标题** — 摘要 [信通院](url)
2. ...

### 🤖 AI 与大模型
1. **标题** — 摘要 [36氪](url)
2. ...

### 🏭 行业方案
1. **标题** — 摘要 [IDC](url)
2. ...

### 💼 市场格局
1. **标题** — 摘要 [艾瑞](url)
2. ...

📊 月报共 N 条 | 1xxx 字符 | wiki 已同步
```

### 步骤 5：交叉引用 + log 更新

- 新发现的概念/厂商 → 在 `concepts/` 或 `entities/` 建/更新页面
- `index.md` 加入新 summary 链接（更新 `Total pages` 计数和 `Last updated`）
- `log.md` 追加一行：
  ```
  ## [YYYY-MM-DD] summary | 数字化行业月报 YYYY-MM
  - 触发：[用户的触发关键词]
  - wiki 写入：summaries/digital-transformation-monthly-YYYY-MM.md
  - 微信推送：[N] 字符
  - 交叉引用：[列出涉及的 wiki 概念]
  ```

## 关键资源池（信源白名单）

### 国际机构（中文友好）
- [Gartner 中国](https://www.gartner.com/cn/publications) — 战略/MQ/Hype Cycle
- [IDC 中国](https://www.idc.com/cn) — 市场份额/Tracker
- [McKinsey Greater China](https://www.mckinsey.com/cn) — 行业洞察
- [BCG 中国](https://www.bcg.com/zh-cn/) — 转型实践
- [PwC Strategy&](https://www.strategyand.pwc.com/cn/zh.html) — 思略特
- [Accenture China](https://www.accenture.cn) — 技术展望

### 国内研究/咨询
- [艾瑞咨询 / 艾瑞数智](https://www.idigital.com.cn/report) — 行业研究最全
- [赛迪顾问 CCID](http://www.ccidconsulting.com) — 政策/国产化
- [头豹研究院](https://www.frostchina.com) + [沙利文](https://www.frost.com) — 投融资视角
- [甲子光年](https://www.jazzyear.com) — To B 深度
- [亿邦动力](https://www.ebrun.com) — 电商/产业互联网

### 官方/标准
- [国家中小企业公共服务示范平台 ndtpc.com](https://ndtpc.com) — 国家级数字化转型促进中心
- [中国信通院 CAICT](http://www.caict.ac.cn) — 标准/白皮书
- [国家发改委](https://www.ndrc.gov.cn) — 政策原文

### 行业媒体（深度报道）
- 36氪 / 虎嗅 / 钛媒体 / 雷峰网 / DoNews

### 行业方案库
- [阿里云](https://developer.aliyun.com) / [腾讯云](https://cloud.tencent.com) / [华为云](https://support.huaweicloud.com) 行业方案
- [树根互联](https://www.rootcloud.com) — 工业互联网
- [用友](https://www.yonyou.com) / [金蝶](https://www.kingdee.com) — ERP/财务数字化

## 当前不做的（边界）

- ❌ **不会自动跑** — 必须用户触发（避免噪声；也避开了 [[weixin-delivery-debugging]] 的 iLink cron 静默问题）
- ❌ **不会备份信源原文** — 除非用户显式要求（避免本地存储膨胀）
- ❌ **不会改成日报/周报** — 频率固定月度（日报已用 [[news-briefing]] 覆盖）
- ❌ **不会自动打分排序** — 信任 Tavily 检索相关性，由 LLM 二次筛选

## 何时升级到方案 B/C

| 触发条件 | 升级到 |
|---|---|
| 用户连续 2 个月都说"内容不错但希望更全" | **B 标准版**（装 blogwatcher + 订阅 RSS） |
| 用户连续 3 个月都忘触发 | **C 进阶版**（cron 自动跑，前提：iLink cron 静默修复） |
| 用户希望加新信源/新板块 | 修改本页面"信源白名单" |

## 相关页面

- [[news-briefing]] — 日报 skill（每天 8:00-8:55 触发）
- [[weixin-delivery-debugging]] — 微信投递故障排查（手动触发模式下不需要走 cron）
- [[finance-investing]] — 类似的手动触发月报/日报参考（A股自选股日报）
- [[ai-stock-analysis-framework]] — 数据驱动决策框架（月报可借鉴）
- [[harness-architecture]] — AI Agent 架构（与月报中"AI 行业"板块强相关）
- [[mcp-protocol]] — AI 工具标准化（与"行业方案"板块强相关）