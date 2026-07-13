---
title: Markdown 格式常用写法
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [programming, tool, reference, tutorial]
sources: [raw/articles/markdown-syntax-reference.md]
confidence: high
---

# Markdown 格式常用写法

> **Markdown** 是一种轻量级标记语言，由 John Gruber 于 2004 年创建。语法简洁（常用标记不超过10个），可导出 HTML/PDF/.md 格式。

## 标题

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

> 行尾可加 `#`（数量不必与开头一致）。

## 段落与换行

- **段落**：行之间空一行
- **换行**：行末加两个以上空格再回车

```markdown
第一行（末尾两个空格）  
第二行

新段落（中间空行）
```

## 强调

| 效果 | 语法 |
|---|---|
| *倾斜* | `*文本*` 或 `_文本_` |
| **加粗** | `**文本**` 或 `__文本__` |
| ***倾斜加粗*** | `***文本***` |
| ~~删除线~~ | `~~文本~~` |

## 列表

### 无序列表

```markdown
* 项目一
* 项目二
  * 子项目（缩进2-3空格）
+ 也可用加号
- 或用减号
```

### 有序列表

```markdown
1. 第一项
2. 第二项
3. 第三项
```

### 任务列表

```markdown
- [x] 已完成
- [ ] 未完成
```

## 引用

```markdown
> 这是一段引用
>
> > 嵌套引用
```

## 代码

### 行内代码

```markdown
使用 `git commit` 提交
```

### 代码块

````markdown
```python
def hello():
    print("Hello!")
```
````

> 指定语言可实现语法高亮。

## 链接

```markdown
[链接文字](URL "可选提示文字")

[链接文字][标记]
[标记]: URL "提示文字"
```

## 图片

```markdown
![替代文本](图片URL "可选标题")
```

## 表格

```markdown
| 左对齐 | 居中 | 右对齐 |
|:-------|:----:|-------:|
| 内容   | 内容 | 内容   |
```

- `:` 在左侧 = 左对齐
- `:` 在右侧 = 右对齐
- `:` 在两侧 = 居中

## 分隔线

```markdown
---
***
___
```

## 转义字符

在特殊符号前加 `\`：

```markdown
\* 不是列表
\# 不是标题
```

支持转义的符号：`` \ ` * _ { } [ ] ( ) # + - . ! ``

## HTML 支持

可直接使用 HTML 标签：

```markdown
<kbd>Ctrl</kbd> + <kbd>C</kbd>
<details>
<summary>点击展开</summary>
折叠内容
</details>
```

## 编辑器推荐

| 工具 | 平台 | 特点 |
|---|---|---|
| **Typora** | 桌面 | 简洁优雅，所见即所得 |
| **VSCode** | 桌面 | + Markdown 插件，功能丰富 |
| **Obsidian** | 桌面 | 知识库管理，支持双向 wikilinks 链接 |
| **markdown.com.cn** | 在线 | 在线编辑体验 |

## 注意事项

1. 所有语法符号请在**英文输入法**下输入
2. 所有语法都可**嵌套使用**（列表套引用、引用内用标题等）
3. 不同平台对 GFM 扩展支持不一，建议以**标准语法**为主
4. 优先使用有语义的标签，而不是纯格式标签

## 相关概念
- [[harness-architecture]] — Harness 架构体系（使用 Markdown 的 AGENTS.md 记忆机制）
- [[mcp-protocol]] — MCP 协议文档（通常用 Markdown 编写）
- [[ai-augmented-scrum]] — AI-Augmented Scrum 框架（产物交付大量依赖 Markdown 文档）
