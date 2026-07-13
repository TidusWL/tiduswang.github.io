# Wiki Log

> 所有 wiki 操作的按时间顺序记录。仅追加。
> 格式：`## [YYYY-MM-DD] action | subject`
> 操作：ingest, update, query, lint, create, archive, delete
> 超过 500 条时轮转：重命名为 log-YYYY.md，重新开始。

## [2026-05-21] create | Wiki initialized
- Domain: 个人综合知识库（技术/编程/AI、综合笔记、工作项目、其他）
- 结构创建完成：SCHEMA.md, index.md, log.md
- 目录结构：raw/{articles,papers,transcripts,assets}, entities/, concepts/, comparisons/, queries/

## [2026-05-23] create | opening-premium-rate
- 新增概念页：开盘溢价率与涨停股次日操作策略
- 来源：视频分享
- 标签：finance, stock, trading, tutorial
- 文件：concepts/opening-premium-rate.md

## [2026-05-26] lint | 首次 wiki 健康检查
- 运行完整的 wiki lint
- **总页面数**: 7
- **发现问题**: 11个
  - 🔴 孤立页面 3 个：rock-paper-scissors, blackjack-strategy, opening-premium-rate（无入链）
  - 🟡 缺失交叉引用：无
  - 🟠 缺少来源 1 个：probability-in-gaming.md（sources 为空）
  - 🟠 索引不同步 7 个：所有页面均未在 index.md 列出
- **修复**:
  - 更新 index.md，补全所有页面索引
  - probability-in-gaming.md 添加 sources: [scratch]
  - rock-paper-scicillin.md 添加互链 [[blackjack-strategy]]
  - probability-in-gaming.md 添加互链到 blackjack-strategy, poker-probability, rock-paper-scissors, sports-betting-ev

## [2026-05-26] update | SCHEMA.md
- 新增 Summary 页面类型定义（summaries/ 目录）
- 新增 Operations 章节（Ingest / Query / Lint）
- 新增 Lint 规则（孤立页面、过期声明、缺失交叉引用、矛盾检查、空来源、索引同步）

## [2026-05-26] create | probability-gaming-summary
- 新增领域总览页：概率博弈知识体系总览
- 链接到所有 7 个相关概念页
- 包含：领域概述、核心知识地图、思维框架、开放问题、推荐阅读顺序
- 文件：summaries/probability-gaming-summary.md

## [2026-05-26] create | summaries/ 目录
- 新建 summaries/ 目录，用于存放领域总览页

## [2026-05-26] lint | 修复后验证
- 修复后再次运行 lint，所有检查通过 ✅
- 孤立页面：0 → 全部修复
- 索引同步：全部修复
- 空来源：全部修复
- 总页面数：8（7 concept + 1 summary）

## [2026-06-03] ingest | Harness 架构体系
- 来源1：36氪《一文读懂Harness Engineering》→ raw/articles/harness-engineering-36kr-2026.md
- 来源2：腾讯云《一文讲透如何构建Harness——六大组件全解析》→ raw/articles/harness-six-components-tencent-2026.md
- 创建概念页：concepts/harness-architecture.md（Harness架构体系总览）
- 创建概念页：concepts/harness-context-reset.md（Context Reset机制）
- 创建概念页：concepts/mcp-protocol.md（MCP协议）
- 创建对比页：comparisons/harness-vs-framework.md（Harness vs Framework）
- 更新 index.md，新增 4 个页面索引
- 标签：ai, ml, system-design, framework, open-source, protocol, context-management, comparison
- 总页面数：12

## [2026-06-03] ingest | Markdown 语法
- 来源：roc-mountain.github.io/Markdown → raw/articles/markdown-syntax-reference.md
- 创建概念页：concepts/markdown-syntax.md（Markdown 格式常用写法完整参考）
- 更新 index.md
- 标签：programming, tool, reference, tutorial
- 总页面数：13

## [2026-06-09] ingest | AI-Augmented Scrum + Agentic 协议栈
- 来源：web_search 2026-06-09，5 篇相关资料综合
  - Agile Leadership Day India (2026-03) — AI-Augmented Scrum Framework
  - ResearchGate (2025-10) — AI-Augmented Scrum 学术论文
  - LeanWisdom (2025) — Gen AI for Scrum Masters
  - Refonte Learning (2026) — Scrum Master in 2026
  - 麦肯锡/Gartner/IBM/Forrester 2026 协议层预测
- 创建概念页：concepts/ai-augmented-scrum.md（人机混合团队 Scrum 框架）
- 创建概念页：concepts/agentic-ai-scrum-protocols.md（MCP/A2A/HITL 协议栈 + 多 Agent 架构）
- 互链：两篇页面互相引用，引用 [[mcp-protocol]] 和 [[harness-architecture]]
- 标签：ai, agent, framework, protocol, system-design, project, team
- 总页面数：14
- 来源标记：scratch（web_search 实时综合，未持久化到 raw/）

## [2026-06-09] lint | 修复历史 broken link + 补齐索引
- 上一轮 lint 发现 17 个问题（16 broken-link + 1 no-source），全部为历史遗留，非本次新页面引入
- **修复方案**：
  - harness-architecture.md 表格与"流程管控"行的 8 个 `[[harness-component-*|别名]]` → 改为普通文字（这些子组件本就在 harness-architecture 内详细描述）
  - harness-context-reset.md 的 [[agent-long-running]] 引用 → 创建 stub 页
  - mcp-protocol.md 的 [[harness-component-orchestration]] → 改为 `[[harness-architecture#编排--hooks|编排组件]]` 锚点
  - probability-in-gaming.md 三个无说明链接 → 补充说明
  - poker-probability.md / rock-paper-scissors.md / markdown-syntax.md 同类问题同步修复
- **新建 4 个 stub 概念页**：game-theory-nash-equilibrium, bayesian-inference, expected-value-decision, agent-long-running
- **补 sources**：opening-premium-rate.md 添加 `sources: [scratch]`，updated 2026-06-09
- **orphan 修复**：harness-architecture.md 增加 `[[markdown-syntax]]` 出链
- **总页面数**：14 → 18

## [2026-06-09] lint | 验证：全部通过
- 复跑 lint-script.py：Pages 19，Issues 0，**All checks passed ✅**
- 修复历史遗留的 17 个问题 + 引入新页面后 2 个小问题（误用 `[[xxx|alias]]`、跨文件夹 orphan 误判）→ 全部解决
- 未持久化 raw/ 来源，本轮修复均以 `scratch` 标记 + 简短说明文本替代

## [2026-06-16] ingest | 新建投资领域（A 股自选股分析）
- **来源**：用户咨询"股票投资技能新热点"+ 用户回答 4 维需求（纯 A 股 / 东财 / 混合型 / AI 建议+用户决策）
- **新增页面（4）**：
  - `entities/watchlist-2026.md` — 自选股清单（骨架，待用户填个股）
  - `concepts/ai-stock-analysis-framework.md` — 4 维度分析框架（基本面/技术/资金/情绪）
  - `concepts/data-sources-ashare.md` — A 股数据源测评
  - `summaries/finance-investing.md` — 投资领域总览
- **新增 skill（1）**：`~/.hermes/skills/finance/stock-watchlist/` — A 股自选股分析技能
- **工具盘点**（基于 2026-06-16 web search）：
  - 主流 AI 工具：TradingAgents（开源多 agent）、富途 Skill（港股）、TickDB、Tavily web_search、AnyGen
  - 国外工具对 A 股覆盖有限
  - 国信证券 6 月策略：看好腾讯/阿里/百度港股
- **下一步**：等用户填自选股清单 → 跑一次真实分析 → 评估是否要加 cron 定时推送
- **index.md 同步**：4 个新页面已加入；总页数 18 → 22

## [2026-06-16] update | 自选股清单首版填充 + 首次完整日报
- **来源**：用户提供自选股清单
  - 000725 京东方A（半导体显示/物联网）
  - 002564 天齐锂业（锂矿/新能源）
  - 002905 金逸影视（电影/院线）
  - 511130 博时上证30年期国债ETF（避险+久期博弈）
  - 600057 厦门象屿（供应链/物流）
  - 601601 中国太保（保险/红利）
- **wiki 同步**：`entities/watchlist-2026.md` 加入 6 只观察池标的 + 用户投资画像
- **首次 6 只票日报跑通**（耗时 104s，6 票 24 个 web_search 并行）：
  - 完整版：`/home/admin/a股日报_2026-06-16.md` (5586 字符)
  - 微信推送精华版：≤ 1100 字符 (iLink 限制)
- **关键发现**：
  - 厦门象屿：扣非暴增 735% + 主力 5 日净流入，估值修复空间最大（+52%）
  - 中国太保：NBV 提速但已反弹 6%，等回踩 31.60 加仓
  - 30 年国债 ETF：险资/机构配置盘，央行宽松预期支撑
- **用户尚未提供**：持仓成本、仓位、买入日期（按中性观察跑）
- **下一步**：等用户填持仓成本 → 加"持仓盈亏"列 → 推 cron 定时推送（交易日 20:00）

## [2026-06-16] fix | 002564 误识别修正
- **错误**：日报初次跑出把 002564 当作"天齐锂业"（实际是 002466），错把锂矿新能源数据填进去
- **修正**：002564 实际是 **天沃科技**（能源工程/环保装备/电力/化工/军工/船舶）
- **真实情况**（2026-06-16 重新分析）：
  - 业绩：2025 扣非 -355%，Q1 2026 净利 -82%
  - 股价：自 9.84 高点阴跌 -52%，现 4.75 接近 52 周低
  - 资金：累计诉讼 7.11 亿、资产负债率 91.63%
  - 评级：全观望
- **影响**：
  - 微信修正推送已发
  - `/home/admin/a股日报_2026-06-16.md` 已 patch
  - `wiki/entities/watchlist-2026.md` 观察池已更新
- **教训**：股票分析 skill 必须做**代码+公司名交叉验证**，不能依赖 web_search 关键词联想

## [2026-06-23] create | digital-transformation-monthly-report

- **用户需求**：每隔一段时间（每月）了解行业动态和技术数字化相关资讯
- **方案选择**：方案 A 极简版（手动触发），参考 [[news-briefing]] 早报的成功路径
- **触发关键词**：`跑下行业月报` / `生成月报` / `[月份]行业月报` / `行业动态月报` / `数字化月报`
- **首次运行**：预计 2026-07-01 前后

### 关键设计决策

1. **手动触发而非 cron**：
   - iLink cron 静默失败问题未解（见 [[weixin-delivery-debugging]] / MEMORY v5→v6）
   - 月报噪声容忍度低，不像早报是日常必需品
   - 触发一次跑一次更可控

2. **5 板块固定结构**（避免重复设计）：
   - 🌐 国际趋势动态（Gartner/IDC/McKinsey/BCG）
   - 🏛️ 政策与标准（工信部/发改委/信通院）
   - 🤖 AI 与大模型（行业大模型/Agent/应用）
   - 🏭 行业方案（制造/金融/零售/医疗）
   - 💼 市场格局（融资/收购/排名）

3. **v7 微信格式**（沿用早报规则）：
   - ≤2000 字符 + emoji 标题 + 加粗 + 短来源标签
   - 单条不拆分（用户 2026-06-16 永久禁令）

4. **wiki 沉淀**：
   - 工作流 → `concepts/digital-transformation-monthly-report.md`（永久规则）
   - 月报 → `summaries/digital-transformation-monthly-YYYY-MM.md`（每月一份）
   - 信源白名单内嵌到工作流页面（方便后续复用）

### 后续动作

- [ ] 2026-07-01 前后首次跑通 7 月月报
- [ ] 根据用户反馈决定是否升级到方案 B/C
- [ ] 监控 wiki 页面是否随时间膨胀（>200 行需拆分）

## [2026-06-26] create | sharepoint-project-dashboard

### 用户需求
搭建可复用项目状态仪表盘（5 大模块：进度/任务/风险/文档/团队），通过 SharePoint + Teams 鉴权分享。

### 决策记录
- **数据源**：Planner + Project Online 混合（Planner 日常，Project Online 算进度）
- **更新频率**：实时同步（Power Automate）
- **访问者**：项目成员，Teams 鉴权（自动权限同步）
- **复用**：多项目共用模板，固定部分（页面布局/Web Part 配置）+ 可定制部分（Banner/名称/标签）

### 新增页面（3 个）
- `concepts/sharepoint-project-dashboard.md` — 方案概览（架构 + 布局 + Power Automate 流程 + 关键建议）
- `comparisons/planner-vs-project-online.md` — 数据源选型对比（10 维度对比表 + 同步方案 A/B/C）
- `tutorials/sharepoint-dashboard-implementation.md` — 实施教程（6 步流程 + PnP 脚本 + 验收清单）

### 后续动作

- [ ] 用户决定**暂不实施**（2026-06-26），等后续项目启动再调用本文档
- [ ] 实施时按 tutorial 步骤：先创建样板项目 → 搭建页面 → PnP 导出模板 → 应用到新项目
- [ ] 准备阶段可能新增：[[power-automate-flows-recipes]] [[microsoft-365-groups-permissions]] 依赖页

<<<<<<< Updated upstream
## [2026-07-02] create | hermes-reinstall-guide
- 文件: `entities/hermes-reinstall-guide.md`（4639 bytes / 155 行）
- 内容: Hermes Agent 重装 + 还原完整指南（备份路径、4 阶段步骤、5 步验证、5 个易踩坑、一键验证脚本）
- 来源: 用户 2026-07-02 09:38 咨询重装服务器还原 Hermes 流程
- 配套操作: index.md 更新（Total pages: 28 → 29）+ 钉钉/微信推送完成
=======
## [2026-07-02] — 新增「创新创造力思维训练法」概念页

### 用户需求
训练创新创造力思维能力，希望沉淀为可复用的知识体系。

### 新增页面（1 个）
- `concepts/creative-thinking-training.md` — 三层级训练法：日常微习惯（SCAMPER/逆向思考/5Why/随机词）+ 刻意练习（跨领域输入/第一性原理/思维模型库/设计思维）+ 环境工程（散步思考/B 状态/多样性人际圈/环境变化）。含常见误区表 + 推荐起步路径。

### 设计考量
- 选 `concept` 类型：方法论体系，不是单一实体
- 标签 `thinking, life`：思维训练 + 自我提升
- 预留 2 个相关链接待后续补全：`first-principles-thinking`、`charlie-munger-mental-models`

### 后续动作
- [ ] 用户开始训练后，可考虑补一个"训练记录/打卡"页面追踪 30 天进度
- [ ] 后续如需深入第一性原理或思维模型库，分别建独立页

## [2026-07-06] — 新增「Hermes Doctor 修复 Runbook」教程页

### 用户需求
完成 `hermes doctor` 修复后，希望把本次链式修复的步骤、踩坑、备份建议沉淀为可复用 runbook。

### 新增页面（1 个）
- `tutorials/hermes-doctor-recovery.md` — 根因速查表 + 4 步修复流程（PATH 前置 / agentmemory MCP / memory provider 改内置 / config migrate）+ 4 个关键踩坑（`ln -sf` 自指环 / provider 非法值 / agentmemory 是 MCP 不是 provider / config.yaml 不可直接编辑）+ 备份建议（~/.hermes/node + ~/.bashrc 加入白名单）。

### 设计考量
- 选 `tutorial` 类型：可执行修复步骤的 runbook，与 "concept" 区分
- 标签 `devops, tool, reference`：运维修复 + 工具 + 参考手册三维度
- 根因速查表优先：用户下次再遇到 doctor 报错，先查表定位再执行步骤
- "关键踩坑"段记录本人在此次修复中犯的真实错误（symlink 自指环 / 用 `builtin` 当合法值），避免下次再踩

### 后续动作
- [ ] 用户后续手动备份 `/home/admin/` 时，把 `~/.hermes/node/` 和 `~/.bashrc` 加入白名单
- [ ] 评估是否启用 agentmemory MCP 7 工具（memory_save/recall/smart_search 等），决定启用后此页"副作用"段可标记 ✅ 完成

## [2026-07-06] — 整合 reinstall-guide 与 doctor-recovery

### 用户需求
阅读对比 `entities/hermes-reinstall-guide.md`（异机还原）和 `tutorials/hermes-doctor-recovery.md`（同机 doctor 修复）后，决定通过 cross-link 双向打通，不合并。

### 改动（4 处）
- `entities/hermes-reinstall-guide.md` 修复 frontmatter 不合规：`type: ops` → `entity`，`date` → `created/updated`，tags 从 `[hermes, reinstall, backup, migration]` → `[devops, tool, reference, backup]`（对齐 SCHEMA.md 标签体系）
- 同文件"5 个易踩坑"表新增第 6 行（还原后立刻跑 doctor 报错 → 链 doctor-recovery）
- 同文件"还原后遇到问题"段新增一行（`hermes doctor` 报错入口）
- `tutorials/hermes-doctor-recovery.md`"副作用与备份建议"段新增"场景区分" callout，明确何时用哪篇；相关段加反向链接

### 合并决策
- **不合并**：两文职责清晰分离（异机 vs 同机）。合并会丧失"何时该读哪篇"的引导。
- cross-link 让用户**从任意一篇自然发现另一篇**，比合并更可维护。

### 设计考量
- reinstall-guide 既是 wiki 页又是"还原指令文档"（首页 callout 说"让新 Hermes 读取并执行"）—— 修复 schema 时没动 body，因为 body 是执行指令不归 SCHEMA 管
- doctor-recovery 的"场景区分"用 blockquote + 第一人称口吻（"一次会话内 30 分钟搞定"），让用户能在 5 秒内判断该不该读

### 额外修复
- `log.md` 末尾残留 `>>>>>>> Stashed changes`（7-02 wiki-push autostash 冲突未清），本次顺手清掉。这是 git rebase 失败但文件未被恢复的脏 marker —— wiki-push 脚本"stash drop 后未恢复 working tree 状态"的行为缺点已观察到两次，待商定修法

### 后续动作
- [ ] 考虑修 wiki-push 脚本：autostash 失败时回退 working tree 而非保留冲突 marker
- [ ] 提醒：未来 reinstall-guide 应作为新机还原 first-read，doctor-recovery 作为还原后子任务
