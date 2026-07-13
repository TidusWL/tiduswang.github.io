# Wiki Schema

## Domain
个人综合知识库，覆盖以下领域：
- **技术/编程/AI** — 软件开发、人工智能、系统设计
- **综合笔记** — 读书笔记、思考碎片、生活记录
- **工作项目** — 项目管理、会议记录、决策追踪
- **其他** — 任何值得记录的内容

## Conventions
- 文件名：小写、连字符、无空格（如 `transformer-architecture.md`）
- 每个 wiki 页面以 YAML frontmatter 开头
- 使用 `[[wikilinks]]` 链接页面（每页至少 2 个出站链接）
- 更新页面时始终更新 `updated` 日期
- 新页面必须加入 `index.md`
- 所有操作必须记录到 `log.md`
- 来源追溯：多来源合成的页面用 `^[raw/articles/source.md]` 标记

## Frontmatter
```yaml
---
title: 页面标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [来自下方标签体系]
sources: [raw/articles/source-name.md]
confidence: high | medium | low  # 可选
contested: true                  # 可选，存在矛盾时
contradictions: [other-page]     # 可选
---
```

## Tag Taxonomy

### 技术/编程/AI
`programming`, `ai`, `ml`, `system-design`, `algorithm`, `backend`, `frontend`, `devops`, `database`, `security`, `open-source`, `framework`, `language`, `tool`, `agent`, `context-management`, `protocol`, `comparison`

### 综合笔记
`book`, `reading`, `thinking`, `life`, `health`, `finance`, `travel`, `food`, `hobby`, `review`

### 工作项目
`project`, `meeting`, `decision`, `planning`, `retrospective`, `requirement`, `architecture`, `team`

### 通用
`person`, `company`, `comparison`, `timeline`, `controversy`, `prediction`, `tutorial`, `reference`

规则：页面上的标签必须来自此体系。需要新标签时先添加到这里。

## Page Thresholds
- **创建页面**：实体/概念出现在 2+ 来源中，或对某一来源至关重要
- **追加到已有页面**：来源提及已覆盖的内容
- **不创建页面**：仅顺带提及、细节不重要、超出领域范围
- **拆分页面**：超过 ~200 行时拆分
- **归档页面**：内容完全被替代时移至 `_archive/`

## Entity Pages
每个实体一页，包含：概述、关键事实和日期、与其他实体的关系、来源引用。

## Concept Pages
每个概念一页，包含：定义/解释、当前认知状态、开放问题或辩论、相关概念链接。

## Comparison Pages
并排分析，包含：比较对象和原因、比较维度（表格优先）、结论/综合、来源。

## Summary Pages

每个领域/主题一个 summary 页：
- 放在 `summaries/` 目录
- 作为该领域的入口页，链接到相关 entity/concept/comparison
- 用一段话概括领域现状和核心认知
- 列出关键开放问题和争议
- frontmatter type 为 `summary`

## Update Policy
新信息与现有内容冲突时：
1. 检查日期 — 新来源通常替代旧来源
2. 如果确实矛盾，标注双方立场和日期
3. 在 frontmatter 中标记矛盾
4. 在 lint 报告中标记供用户审查

## Operations

### Ingest
- 将新资料放入 `raw/` 对应子目录
- LLM 阅读资料，提取关键信息
- 创建/更新相关 entity、concept、summary 页面
- 更新 `index.md`
- 在 `log.md` 追加记录
- 单个资料可能影响 10-15 个页面

### Query
- 向 wiki 提问
- LLM 搜索相关页面，综合回答并标注引用
- 好的回答可以归档为新页面

### Lint（定期健康检查）
- **孤立页面**：检测无入链的页面（`[[wikilinks]]` 中没有被其他页面引用）
- **过期声明**：标记 `updated` 超过 30 天且可能被新信息替代的页面
- **缺失交叉引用**：页面内容中提到的概念/实体但没有 `[[wikilink]]` 链接
- **矛盾检查**：`contested: true` 的页面是否已解决
- **空来源**：`sources` 字段为空或过时的页面
- **索引同步**：`index.md` 是否与实际页面一致
- 输出 lint 报告，列出所有问题和建议修复

### Lint 规则
- 每次 ingest 后自动运行轻量 lint（仅检查变更相关的页面）
- 每周运行一次完整 lint
- lint 结果记录到 `log.md`
- 严重问题（矛盾、孤立）标记供用户审查
