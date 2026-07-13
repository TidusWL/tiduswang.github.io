---
title: SharePoint 项目状态仪表盘方案
created: 2026-06-26
updated: 2026-06-26
type: concept
tags: [project, planning, architecture, team, tool, m365, sharepoint]
sources: []
confidence: medium
---

# SharePoint 项目状态仪表盘方案

> 用户需求（2026-06-26 沟通后定稿）：搭建一个**可复用模板**的项目状态仪表盘，通过 SharePoint + Teams 鉴权分享给项目成员，包含项目全局进度、计划任务状态、重要风险/话题、项目文档链接、团队成员联系方式 5 大模块。

## 📋 需求确认

| 维度 | 选择 | 理由 |
|------|------|------|
| **数据源** | Planner + Project Online 混合 | Planner 上手快（队员），Project Online 算进度（PM）|
| **更新频率** | 实时同步 | Power Automate 自动流 |
| **访问者** | 项目成员，Teams 鉴权 | 自动权限同步，离开 Teams 自动失权 |
| **复用** | 多项目共用模板 | PnP 导出 XML 模板，固定部分 + 可定制部分 |

## 🏗️ 核心架构

```
        Microsoft 365 租户
        ┌─────────────────────────────┐
        │  Microsoft 365 Group        │
        │  "项目 Phoenix 团队"          │
        │  ────────────────────────  │
        │  ├─ SharePoint 站点           │
        │  │  ├─ 项目仪表盘（主页）      │◀─── 模板从这里导出
        │  │  ├─ 文档库（项目文档）      │
        │  │  ├─ 项目动态列表（风险）    │
        │  │  └─ 站点成员（从 AD 同步）  │
        │  ├─ Planner "Phoenix 任务"    │
        │  ├─ Project Online "Phoenix"  │
        │  └─ Teams 频道 "Phoenix"      │
        └─────────────────────────────┘
                │
                │ (Power Automate 同步)
                ▼
        ┌─────────────────────────────┐
        │  自动数据流                   │
        │  ────────────────────────  │
        │  Planner → Project Online   │
        │  Planner → 仪表盘 Web Part   │
        │  Teams → 通知 PM             │
        └─────────────────────────────┘
```

## 🎨 仪表盘页面布局

| 位置 | Web Part | 数据源 | 模板固定/可定制 |
|------|----------|--------|----------------|
| 顶部 Hero | **Hero** | 项目名 + 阶段 + RAG 灯 | 🎨 Banner/名称可定制 |
| 左 1 | **Planner** | "我的任务"过滤视图 | 🔒 固定 |
| 左 2 | **进度环** | Project Online 嵌入 | 🔒 固定 |
| 右 1 | **Project Online** | Gantt 图表 | 🔒 固定 |
| 右 2 | **突出新闻** | 站点"项目动态"列表 | 🎨 风险标签可定制 |
| 底部 | **文档库** | SitePages Documents 精选 | 🔒 固定 |
| 底部 | **人员卡片** | 站点成员 | 🎨 自动从 M365 组拉 |

## 📦 模板的固定 vs 可定制部分

| 模块 | 固定/可定制 |
|------|------------|
| 页面布局 | 🔒 固定 |
| Web Part 配置 | 🔒 固定 |
| RAG 状态灯 | 🎨 按项目风险等级调 |
| 项目 Banner 图 | 🎨 每项目自己的图 |
| 项目名称/描述 | 🎨 每项目自己的文案 |
| 团队成员 | 🎨 自动从 M365 组拉 |
| Planner 桶名 | 🎨 阶段命名按项目调 |
| 风险分类标签 | 🎨 项目专属词汇 |

## 🔐 Teams 鉴权实现

| 用户类型 | 访问方式 |
|---------|---------|
| 项目成员 | Teams 频道直接看（pin） |
| PM（你） | SharePoint 站点所有者 |
| 外部协作者 | Azure B2B 邀请加 M365 组 |

**自动权限同步**：添加到 Teams 频道 → 自动成为站点成员；离开 → 自动失去访问权。不用手动维护权限列表。

## 📊 Power Automate 自动化流程（4 个推荐）

### 流程 1：任务状态变更通知
```
触发：Planner 任务标记为"已完成"
动作：往 Teams 频道发消息 "✅ [任务名] 由 [负责人] 完成"
```

### 流程 2：风险升级
```
触发：Planner 任务标记为"阻塞"
动作：① PM 私聊通知 ② 仪表盘"风险"列表自动添加条目
```

### 流程 3：每周状态报告
```
触发：每周一 9:00
动作：① 汇总上周完成/新增/阻塞任务 ② 邮件给利益相关者
```

### 流程 4：Planner ↔ Project Online 同步
```
触发：Planner 任务变更
动作：自动更新 Project Online 任务（任务名/日期/负责人）
```

## ⚠️ 关键建议

1. **从 Planner 开始，Project Online 慢慢加**
   - 第 1 个月：只用 Planner（队员上手快）
   - 第 2 个月：开始用 Project Online（PM 维护依赖）
   - 第 3 个月：建立 Power Automate 同步

2. **标准化项目阶段**
   模板固化 5 个阶段：启动 → 规划 → 执行 → 监控 → 收尾

3. **风险分级标准化**
   - 🔴 红色 = 阻塞项目进度，必须立即升级
   - 🟡 黄色 = 影响项目时间/范围，需要关注
   - 🟢 绿色 = 进度正常，无重大风险

4. **让队员 30 秒能更新状态**
   - Planner 移动端拖拽改状态
   - 不强制写详细描述（除非重要里程碑）
   - 详细描述留给周会

## 🚦 下一步行动（用户 2026-06-26 决定：先不做）

- [ ] 用户决定**暂不实施**，等后续项目启动再调用本文档
- [ ] 准备搭建时：先创建样板项目（Phoenix）→ 配置 Planner/Project → 搭建仪表盘页面 → PnP 导出模板 → 应用到新项目

## 🔗 相关链接

- [[sharepoint-dashboard-implementation]] — 具体实施步骤（PnP 脚本 + 浏览器操作）
- [[planner-vs-project-online]] — Planner 与 Project Online 选型对比
- [[power-automate-flows-recipes]] — Power Automate 常用流程模板
- [[microsoft-365-groups-permissions]] — M365 组与 SharePoint 权限模型