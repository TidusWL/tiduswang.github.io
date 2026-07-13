---
title: SharePoint 项目仪表盘实施教程
created: 2026-06-26
updated: 2026-06-26
type: tutorial
tags: [tutorial, project, sharepoint, pnp, power-automate, m365]
sources: [sharepoint-project-dashboard.md]
confidence: medium
---

# SharePoint 项目仪表盘实施教程

> 配套 [[sharepoint-project-dashboard]] 使用。按此步骤可在 **3-4 小时** 内搭建一个可复用的项目仪表盘模板。

## 📋 前置条件

| 条件 | 说明 |
|------|------|
| M365 租户 | 含 SharePoint Online + Planner + Project Online |
| PowerShell 7+ | 安装 `Install-Module -Name PnP.PowerShell` |
| 用户权限 | SharePoint 管理员或租户管理员 |
| 项目名称 | 准备好第 1 个样板项目名（如 "Phoenix"）|

## 🚀 实施步骤

### Step 1：创建样板项目（30 分钟）

#### 1.1 创建 M365 组 + 站点
- Teams → 新建团队 → 命名"项目 Phoenix"
- 自动生成：SharePoint 站点 + Planner + Project Online 候选

#### 1.2 配置 Planner
- 桶：待办 / 进行中 / 已完成 / 阻塞 / 已搁置
- 标签：阶段1 / 阶段2 / 紧急 / 客户相关 / 内部
- 视图：按桶分 / 按负责人分 / 按到期日分

#### 1.3 配置 Project Online（如用）
- 任务列表（从 Planner 同步）
- 关键路径自动算
- 基线（baseline）保存每周一

### Step 2：搭建仪表盘页面（30 分钟）

#### 2.1 新建页面
- 站点内容 → 新建 → 页面
- 模板：选"空白"或"项目状态"
- 命名：`项目仪表盘`

#### 2.2 添加 Web Part（顺序）

| 位置 | 操作 |
|------|------|
| 顶部 | 插入 → Hero → 填项目名 + 阶段 + RAG 灯 |
| 左 1 | 插入 → Planner → 选"项目任务"计划 → 选"我的任务"视图 |
| 左 2 | 插入 → 嵌入 → Project Online URL |
| 右 1 | 插入 → Project Web App → Gantt |
| 右 2 | 插入 → 突出新闻 → 关联"项目动态"列表 |
| 底部 | 插入 → 文档库 → SitePages Documents |
| 底部 | 插入 → 人员 → 显示站点成员 |

#### 2.3 配置 Banner
- 页面设置 → 顶部图像 → 上传项目 Banner
- 描述：项目名称 + 阶段 + 当前 RAG 状态

### Step 3：PnP 导出模板（20 分钟）

```powershell
# 安装 PnP PowerShell（首次）
Install-Module -Name PnP.PowerShell

# 连接样板站点
Connect-PnPOnline -Url "https://contoso.sharepoint.com/sites/Phoenix" -Interactive

# 导出为 XML 模板（保留页面 + Web Part 结构 + 列表）
Get-PnPProvisioningTemplate -Out "项目仪表盘模板.xml" `
    -Handlers Lists,Pages,WebParts `
    -PersistSchemaVersion:$true
```

模板文件结构：
```
项目仪表盘模板.xml
├── 列表定义（项目动态、文档库）
├── 页面定义（项目仪表盘）
├── Web Part 配置（5 个）
└── 导航结构
```

### Step 4：应用到新项目（15 分钟）

#### 4.1 创建新 M365 组
- Teams → 新建团队 → 命名"项目 Atlas"

#### 4.2 应用模板
```powershell
# 连接新站点
Connect-PnPOnline -Url "https://contoso.sharepoint.com/sites/Atlas" -Interactive

# 应用模板
Apply-PnPProvisioningTemplate -Path "项目仪表盘模板.xml"
```

#### 4.3 自定义可调部分
```powershell
# 改项目名称
Set-PnPWeb -Title "项目 Atlas - 仪表盘"

# 改 Banner 图（手动上传或脚本）
Add-PnPFile -Path ".\atlas-banner.png" -Folder "SiteAssets"

# 调整 Planner 桶名（如 Atlas 有自己的阶段命名）
# （需在 Planner UI 手动调整，或用 Planner REST API）
```

### Step 5：Teams 集成（10 分钟）

#### 5.1 Pin 仪表盘到频道
- Teams → 频道 → "+" → 网站 → 选"项目 Atlas - 仪表盘"
- 设置为频道顶部标签

#### 5.2 配置权限
- 频道成员 → 自动成为站点成员
- 离开频道 → 自动失去访问权（无需手动维护）

### Step 6：Power Automate 流程（30 分钟）

参考 [[power-automate-flows-recipes]] 的 4 个流程：
- 任务状态变更通知
- 风险升级
- 每周状态报告
- Planner ↔ Project Online 同步

## ✅ 验收清单

部署完检查这些：

- [ ] 站点能正常访问
- [ ] 仪表盘页面 5 个 Web Part 都显示正常
- [ ] Planner 任务拖拽改状态后，仪表盘实时刷新
- [ ] Teams 频道顶部能直接看到仪表盘
- [ ] PnP 模板 XML 文件保存到共享位置（如 SharePoint 文档库）
- [ ] Power Automate 4 个流程都启用并测试过

## 🐛 常见问题

| 问题 | 解决 |
|------|------|
| PnP 模板应用到新站点报权限错 | 新站点所有者用 `-Interactive` 重连 |
| Planner Web Part 显示空 | 检查站点成员对 Planner 的访问权限 |
| Project Online Gantt 不显示 | 浏览器需支持 iframe，部分老 IE 不行 |
| Teams 看不到 pin 的仪表盘 | 频道所有者重新 pin，或检查 Tab 权限 |
| 自定义 Banner 图过大 | 建议 ≤ 500KB，1920×240 px |

## 🎁 Bonus 玩法

| 需求 | 实现 |
|------|------|
| 多项目横向对比 | Hub 站点聚合所有项目仪表盘 |
| 客户定制简化版 | SharePoint 页面版本 + 客户权限组 |
| AI 自动生成周报 | Copilot for M365 + Planner 数据 |
| 历史项目归档 | SharePoint 站点模板 + Archive |

## 🔗 相关链接

- [[sharepoint-project-dashboard]] — 方案概览
- [[planner-vs-project-online]] — 数据源选型
- [[power-automate-flows-recipes]] — Power Automate 流程
- [[microsoft-365-groups-permissions]] — 权限模型