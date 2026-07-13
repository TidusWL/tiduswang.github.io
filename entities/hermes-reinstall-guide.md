---
type: entity
title: Hermes Agent 重装 + 还原指南
created: 2026-07-02
updated: 2026-07-06
tags: [devops, tool, reference, backup]
source: 2026-07-01 备份 ~/hermes-migration-20260701_094823.tar.gz
---

# Hermes Agent 重装 + 还原指南

> 本文档由小Tbot 生成 (2026-07-02)。  
> 用途：重装服务器后，**让新 Hermes Agent 读取并执行本文档完成还原**。

---

## 📦 备份信息

| 项 | 值 |
|---|---|
| 路径 | `~/hermes-migration-20260701_094823.tar.gz` |
| 大小 | 85.1 MB / 1402 文件 |
| 时间 | 2026-07-01 09:48 |
| 包含 | `.hermes/` 全部（config/env/auth/memory/skills/cron/state.db）+ `wiki/` |

⚠️ **备份不包含 Hermes 二进制**（`~/.local/bin/hermes`）—— 那是 pip 包，还原前必须先 `pip install`。

---

## 🚀 新服务器 4 阶段

### 阶段 1：装基础环境

```bash
# 1.1 系统包
yum install -y python3.11 python3.11-pip          # CentOS/Alma/Rocky
# 或
apt install -y python3.11 python3.11-venv          # Ubuntu/Debian

# 1.2 装 hermes-agent（必须 Python 3.11+）
python3.11 -m pip install --user hermes-agent

# 1.3 验证
which hermes
hermes --version        # 应看到 v0.17.0+
```

### 阶段 2：传备份文件

```bash
# 2.1 传输（任选一种）
scp old-server:/home/admin/hermes-migration-20260701_094823.tar.gz ~/

# 2.2 校验完整性
ls -lh ~/hermes-migration-20260701_094823.tar.gz
tar tzf ~/hermes-migration-20260701_094823.tar.gz | wc -l   # 应是 1402
```

### 阶段 3：解压还原

```bash
cd ~
tar xzf hermes-migration-20260701_094823.tar.gz

# 关键文件就位检查
ls -la ~/.hermes/config.yaml ~/.hermes/auth.json ~/.hermes/state.db
ls -la ~/wiki/ | head -5
```

### 阶段 4：权限修复 + 启动

```bash
chmod 600 ~/.hermes/auth.json ~/.hermes/.env ~/.hermes/.env.gateway
chmod 700 ~/.hermes/
hermes gateway start
hermes status
```

---

## ✅ 还原后必验证 5 件事

| # | 验证 | 命令 | 期望 |
|---|------|------|------|
| 1 | memory 还原 | `cat ~/.hermes/memories/MEMORY.md \| head -10` | 看到索引行 |
| 2 | skills 还原 | `ls ~/.hermes/skills/ \| wc -l` | ~80 个 |
| 3 | cron 还原 | `hermes cron list` | 看到 `9d2bbccf7920` 早报任务 |
| 4 | wiki 还原 | `ls ~/wiki/ \| head` | 看到 SCHEMA/index/log 等目录 |
| 5 | 用户身份 | `grep -A1 身份 ~/.hermes/memories/USER.md` | 看到「企业内部技术解决方案项目经理」 |

---

## ⚠️ 5 个易踩坑

| 坑 | 后果 | 解决 |
|---|------|------|
| **Python 版本错** | hermes 启动报错 / MCP 工具挂 | 必须 `python3.11+`，别用系统 3.6 |
| **没先装包就解压** | `~/.local/bin/hermes` 是空的 | **先装 hermes-agent，再解压** |
| **auth.json 权限过宽** | 启动警告 / 安全问题 | `chmod 600` |
| **gateway 端口被占** | start 失败 | `lsof -i :8080` 查谁占着，先 kill |
| **Tavily API key 失效** | 早报 cron 跑失败 | `.env` 里 `TAVILY_API_KEY` 还在就没事 |
| **还原后立刻 `hermes doctor` 报错** | PATH 错位 / agentmemory 缺失 / config migration | 详见 `[[hermes-doctor-recovery]]` 第 1-3 步（PATH 前置 + 重装 agentmemory）|

---

## 📋 执行顺序口诀

1. **现在**：异地备份 `scp` 一份到别处（别只留一份）
2. **新机**：先 `pip install hermes-agent` → 再 `tar xzf`
3. **解压后**：`chmod` → `hermes gateway start` → 跑 5 步验证
4. **确认正常**：`hermes cron list` 看 `9d2bbccf7920` 还在

🔔 **铁律：先装包，再解压**（顺序反了 `~/.local/bin/hermes` 是空的）

---

## 🔍 自动执行脚本（可选）

如果想一键跑完所有验证，把这段保存为 `~/verify-hermes-restore.sh`：

```bash
#!/usr/bin/env bash
set -e
echo "=== Hermes 还原验证 ==="

echo "[1/5] memory..."
test -f ~/.hermes/memories/MEMORY.md && echo "  ✓ MEMORY.md" || echo "  ✗ MISSING"

echo "[2/5] skills 数量..."
n=$(ls ~/.hermes/skills/ 2>/dev/null | wc -l)
echo "  skills: $n 个"
[ "$n" -ge 50 ] && echo "  ✓ 充足" || echo "  ⚠ 偏少"

echo "[3/5] cron 任务..."
hermes cron list 2>/dev/null | grep -q "9d2bbccf7920" && echo "  ✓ 早报 cron 在" || echo "  ✗ 早报 cron 丢失"

echo "[4/5] wiki..."
test -d ~/wiki && echo "  ✓ ~/wiki 存在" || echo "  ✗ wiki 丢失"

echo "[5/5] 用户身份..."
grep -q "企业内部技术解决方案项目经理" ~/.hermes/memories/USER.md && echo "  ✓ 身份正确" || echo "  ✗ 身份未还原"

echo "=== 完成 ==="
```

---

## 📞 还原后遇到问题

- gateway 起不来 → `hermes logs --tail 50`
- `hermes doctor` 报错（路径错位 / agentmemory plugin / config migration）→ 见 `[[hermes-doctor-recovery]]`
- 早报 cron 跑失败 → `hermes cron run 9d2bbccf7920 --dry-run`
- 找不到 skills → `hermes skills list`
- memory 没还原 → 检查 `~/.hermes/memories/MEMORY.md` 字符数（应该 > 2000）

---

_本文档由小Tbot 在 2026-07-02 09:38 生成，基于当天用户咨询。_
