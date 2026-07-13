---
title: Hermes Doctor 修复 Runbook
created: 2026-07-06
updated: 2026-07-06
type: tutorial
tags: [devops, tool, reference]
---

# Hermes Doctor 修复 Runbook

`hermes doctor` 报 issue 时的实际修复步骤。2026-07-06 在 `o9cq80y1m6JP` 用户的
机器上验证过完整链路（3 条 issue 全部清零）。

## 触发场景

`hermes doctor` 报告以下任意一条：

- `Reinstall entry point: cd <site-packages> && source venv/bin/activate && pip install -e '.[all]'`
  路径跑不通（指向不存在的 site-packages / venv）
- `agentmemory plugin not found run: hermes memory setup`
- `Run 'hermes doctor --fix' or 'hermes setup' to migrate config`

## 根因速查

| 报错 | 真因 |
|---|---|
| 路径错位的"Reinstall entry point" | `~/.pyenv/shims/hermes` 抢在 venv 入口前面，doctor 跟随 `sys.executable` 解析，生成坏路径 |
| `agentmemory plugin not found` | 配置 `memory.provider: agentmemory` 是 MCP server 配置残留，不是合法 memory provider 名；doctor 对任何不识别的 provider 名都报 "plugin not found" |
| Config migration 32→33 | Hermes schema 升级滞后，`hermes doctor --fix` 一句搞定 |

## 修复步骤

### 1. PATH 前置 venv（一次解决路径错位）

```bash
echo 'export PATH="/home/admin/.hermes/hermes-agent/venv/bin:$PATH"' >> ~/.bashrc
export PATH="/home/admin/.hermes/hermes-agent/venv/bin:$PATH"

# 验证
which hermes
# 应输出: /home/admin/.hermes/hermes-agent/venv/bin/hermes

hermes --version | head -2
```

新 shell 才会加载 `~/.bashrc`，当前会话已生效就够。

### 2. agentmemory MCP 装回（仅在确需 7 个 memory_* 工具时）

如果配置残留 `mcp_servers.agentmemory` 但二进制丢了：

```bash
# 装到用户空间（避免全局权限问题）
mkdir -p ~/.hermes/node/bin
npm install -g --prefix /home/admin/.hermes/node @agentmemory/agentmemory

# 建 shim 指向 bin 字段（package.json 指定 dist/cli.mjs）
cat > ~/.hermes/node/bin/agentmemory <<'EOF'
#!/usr/bin/env bash
exec node /home/admin/.hermes/node/lib/node_modules/@agentmemory/agentmemory/dist/cli.mjs "$@"
EOF
chmod +x ~/.hermes/node/bin/agentmemory

# 验证 MCP
hermes mcp test agentmemory
# 预期: Connected (~200ms), 7 tools discovered
```

### 3. 把 memory provider 改回"内置"

`memory.provider` 必须是合法 provider 名（honcho / mem0 / openviking 等）或**空字符串**。
`builtin` 也会被 doctor 报"plugin not found"。

```bash
hermes config set memory.provider ""
hermes memory status
# 预期: Provider: (none — built-in only)
```

### 4. config migration

```bash
hermes doctor --fix
hermes doctor
# 预期: All checks passed! 🎉
```

## 关键踩坑（避免重复）

1. **`ln -sf X X` 是 symlink 自指环** — 不要假定路径不存在就先创建 symlink。先 `ls -la`。
   真文件 → `rm` 后可以从 npm 重装；symlink 自指环 → `rm` 是解药，但要确认原是 symlink 才行。
2. **`memory.provider: builtin` 不是合法值** — doctor 对任何不识别名字都报 "plugin not found"。
   内置默认是空串 `""`（"none — built-in only"）。
3. **`memory.provider: agentmemory` 是 MCP server 配置的副作用，不是 provider 配置** — agentmemory
   是个 MCP 服务器（提供 7 个 memory_* 工具），不归 `hermes memory setup` picker 管。
4. **改 `~/.hermes/config.yaml` 会被 agent 拒绝**（security-sensitive 文件），必须用 `hermes config set <key> <value>`。

## 副作用与备份建议

本次修复在文件系统留下的痕迹：

- `~/.bashrc` 多了 1 行 PATH export（永久）
- `~/.hermes/node/` 多了 ~228 个 npm 包（agentmemory v0.9.27）
- `~/.hermes/node/bin/agentmemory` bash shim
- `~/.hermes/config.yaml` 第 399 行 `provider` 改为空串

下次还原 ~/.hermes 时，这 4 项如果不在备份范围内会再次出现同样问题。建议备份脚本把
`~/.hermes/node/` 和 `~/.bashrc` 加进白名单（或至少保留 shim）。

> **场景区分**：本文是同机 doctor 报错后的修复（一次会话内 30 分钟搞定）。如果是异机迁移 /
> 灾难还原的全流程，见 `[[hermes-reinstall-guide]]`（4 阶段从装包到验证 5 件套）。

## 相关

- 微信/钉钉 home channel 故障 → `[[weixin-silent-no-send]]`
- 异机还原流程 → `[[hermes-reinstall-guide]]`
- backup 后遗症模式 → 详见 `MEMORY.md` §铁律（2026-07-02 还原踩坑）

## 后续动作（可选）

- [ ] 把 `~/.hermes/node/` 和 `~/.bashrc` 加入备份脚本白名单
- [ ] 评估是否启用 agentmemory MCP 7 tool（memory_save/recall/smart_search 等）
