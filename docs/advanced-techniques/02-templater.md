# Templater 插件

## 目录
- [什么是 Templater](#什么是-templater)
- [安装与配置](#安装与配置)
- [基础语法](#基础语法)
- [常用函数](#常用函数)
- [模板示例](#模板示例)
- [自动化应用](#自动化应用)
- [最佳实践](#最佳实践)
- [常见问题](#常见问题)

---

## 什么是 Templater

Templater 是一个强大的模板插件，它允许你创建**动态模板**，可以自动填充日期、执行代码、获取用户输入等。

### 核心能力

| 功能 | 描述 |
|------|------|
| 📅 **日期时间** | 自动插入各种格式的日期时间 |
| 📝 **动态内容** | 根据条件生成不同内容 |
| 💬 **用户交互** | 弹窗输入、选择列表 |
| 🔄 **自动化** | 创建文件时自动应用模板 |
| 🔧 **系统命令** | 执行系统命令获取输出 |

### Templater vs 核心模板插件

| 特性 | 核心模板 | Templater |
|------|----------|-----------|
| 静态文本 | ✅ | ✅ |
| 动态日期 | ⚠️ 基础 | ✅ 强大 |
| 用户输入 | ❌ | ✅ |
| 条件逻辑 | ❌ | ✅ |
| 循环 | ❌ | ✅ |
| JavaScript | ❌ | ✅ |
| 自动应用 | ❌ | ✅ |

---

## 安装与配置

### 安装

1. 设置 → 社区插件 → 浏览
2. 搜索 "Templater"
3. 安装并启用

### 基础配置

设置 → Templater：

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 模板文件夹位置 | `模板/` | 存放模板的文件夹 |
| 语法高亮 | 开启 | 在编辑器中高亮模板语法 |
| 自动跳转到光标 | 开启 | 插入模板后跳到 `<% tp.file.cursor() %>` |
| 触发文件创建 | 开启 | 新建文件时触发模板 |

### 文件夹模板设置

为特定文件夹设置自动应用的模板：

```
文件夹        →  模板
日记/         →  模板/日记模板.md
项目/         →  模板/项目模板.md
读书笔记/     →  模板/书籍模板.md
```

---

## 基础语法

### 语法格式

```markdown
<%* 执行代码（无输出） *%>
<% 执行代码并输出 %>
<%- 输出原始内容（不转义） -%>
```

### 执行与输出

```markdown
// 有输出（显示结果）
<% tp.date.now() %>

// 无输出（只执行，常用于变量赋值）
<%* let myVar = "hello" *%>

// 使用变量
<% myVar %>
```

### 光标位置

```markdown
完成模板后光标会停在这里：<% tp.file.cursor() %>

多个光标位置（按Tab切换）：
标题：<% tp.file.cursor(1) %>
内容：<% tp.file.cursor(2) %>
```

---

## 常用函数

### 日期时间 - tp.date

| 函数 | 输出示例 | 说明 |
|------|----------|------|
| `tp.date.now()` | `2024-01-15` | 当前日期 |
| `tp.date.now("YYYY-MM-DD")` | `2024-01-15` | 自定义格式 |
| `tp.date.now("dddd")` | `星期一` | 星期几 |
| `tp.date.now("YYYY-MM-DD", 7)` | `2024-01-22` | 7天后 |
| `tp.date.now("YYYY-MM-DD", -1)` | `2024-01-14` | 昨天 |
| `tp.date.now("YYYY-[W]WW")` | `2024-W03` | 周数 |
| `tp.date.weekday("YYYY-MM-DD", 0)` | 本周一 | 本周某天 |

**日期格式代码**：

| 代码 | 说明 | 示例 |
|------|------|------|
| YYYY | 年（4位） | 2024 |
| MM | 月（2位） | 01 |
| DD | 日（2位） | 15 |
| HH | 时（24小时） | 14 |
| mm | 分 | 30 |
| ss | 秒 | 45 |
| dddd | 星期全称 | 星期一 |
| ddd | 星期缩写 | 周一 |

### 文件操作 - tp.file

| 函数 | 说明 |
|------|------|
| `tp.file.title` | 当前文件名（不含扩展名） |
| `tp.file.path()` | 文件完整路径 |
| `tp.file.folder()` | 文件所在文件夹 |
| `tp.file.creation_date()` | 创建日期 |
| `tp.file.last_modified_date()` | 修改日期 |
| `tp.file.cursor()` | 插入光标位置 |
| `tp.file.cursor_append("text")` | 光标位置追加文本 |
| `tp.file.rename("新名称")` | 重命名文件 |
| `tp.file.move("新路径")` | 移动文件 |
| `tp.file.include("[[笔记]]")` | 插入另一个笔记的内容 |

### 用户输入 - tp.system

| 函数 | 说明 |
|------|------|
| `tp.system.prompt("提示")` | 弹出输入框 |
| `tp.system.suggester(选项, 值)` | 弹出选择列表 |
| `tp.system.clipboard()` | 获取剪贴板内容 |

**示例**：

```markdown
<%* 
let title = await tp.system.prompt("请输入标题：")
let type = await tp.system.suggester(
  ["📚 书籍", "📄 文章", "🎬 视频"],
  ["book", "article", "video"]
)
*%>

# <% title %>
类型：<% type %>
```

### 前端 Frontmatter - tp.frontmatter

```markdown
<%* 
// 读取 frontmatter
let author = tp.frontmatter.author
let tags = tp.frontmatter.tags
*%>
```

### Web 请求 - tp.web

```markdown
<%*
// 获取每日格言
let quote = await tp.web.daily_quote()
*%>

> <% quote.content %>
> — <% quote.author %>
```

---

## 模板示例

### 1. 每日笔记模板

```markdown
---
date: <% tp.date.now("YYYY-MM-DD") %>
day: <% tp.date.now("dddd") %>
week: <% tp.date.now("WW") %>
---

# <% tp.date.now("YYYY-MM-DD dddd") %>

## 📋 今日任务
- [ ] 

## 📝 笔记

## 🌟 今日感想

## 📊 习惯追踪
- [ ] 运动
- [ ] 阅读
- [ ] 冥想

---
**昨天**：[[<% tp.date.now("YYYY-MM-DD", -1) %>]]
**明天**：[[<% tp.date.now("YYYY-MM-DD", 1) %>]]
```

### 2. 会议笔记模板

```markdown
---
date: <% tp.date.now("YYYY-MM-DD") %>
type: meeting
participants: 
---

# 📅 会议：<% await tp.system.prompt("会议主题：") %>

**时间**：<% tp.date.now("YYYY-MM-DD HH:mm") %>
**地点**：<% await tp.system.prompt("会议地点：") %>
**参与者**：<% tp.file.cursor(1) %>

## 📋 议程

## 📝 会议记录

## ✅ 行动项目

| 任务 | 负责人 | 截止日期 |
|------|--------|----------|
| <% tp.file.cursor(2) %> | | |

## 📎 相关资料
```

### 3. 书籍笔记模板

```markdown
---
type: book
title: "<% await tp.system.prompt("书名：") %>"
author: "<% await tp.system.prompt("作者：") %>"
rating: 
status: <%* 
let status = await tp.system.suggester(
  ["📖 阅读中", "✅ 已完成", "📚 待读", "⏸️ 暂停"],
  ["阅读中", "已完成", "待读", "暂停"]
)
*%><% status %>
date-started: <% tp.date.now("YYYY-MM-DD") %>
date-finished: 
---

# 📚 <% tp.frontmatter.title %>

**作者**：<% tp.frontmatter.author %>
**开始日期**：<% tp.date.now("YYYY-MM-DD") %>

## 📖 简介

<% tp.file.cursor() %>

## 💡 主要观点

## ✍️ 读书笔记

## 📝 精彩摘录

> 

## 🎯 行动要点

## 📊 评分

⭐⭐⭐⭐⭐

## 🔗 相关

```

### 4. 项目模板

```markdown
---
type: project
status: active
created: <% tp.date.now("YYYY-MM-DD") %>
deadline: 
---

# 🎯 项目：<% await tp.system.prompt("项目名称：") %>

## 📋 项目概述

### 目标

<% tp.file.cursor(1) %>

### 成功标准

- [ ] 

## 📅 里程碑

| 里程碑 | 截止日期 | 状态 |
|--------|----------|------|
| <% tp.file.cursor(2) %> | | ⏳ |

## ✅ 任务列表

### 待办

- [ ] 

### 进行中

### 已完成

## 📝 笔记

## 📎 相关资源

## 📊 项目日志

### <% tp.date.now("YYYY-MM-DD") %>
- 项目启动

```

### 5. 人物笔记模板

```markdown
---
type: person
name: "<% await tp.system.prompt("姓名：") %>"
organization: 
role: 
contact: 
met: <% tp.date.now("YYYY-MM-DD") %>
---

# 👤 <% tp.frontmatter.name %>

## 📋 基本信息

- **组织**：<% tp.file.cursor(1) %>
- **职位**：
- **联系方式**：

## 🤝 如何认识

## 📝 交流记录

### <% tp.date.now("YYYY-MM-DD") %>
<% tp.file.cursor(2) %>

## 💡 备注

## 🔗 相关

```

---

## 自动化应用

### 文件夹自动模板

设置 → Templater → 文件夹模板：

```
日记/           →  模板/日记模板.md
会议/           →  模板/会议模板.md
读书笔记/书籍/  →  模板/书籍模板.md
```

当在这些文件夹中创建新文件时，会自动应用对应模板。

### 自动重命名

```markdown
<%*
// 在模板开头使用
let title = await tp.system.prompt("笔记标题：")
await tp.file.rename(title)
*%>
```

### 自动移动

```markdown
<%*
let folder = await tp.system.suggester(
  ["📚 读书", "📝 笔记", "🎯 项目"],
  ["读书笔记/", "笔记/", "项目/"]
)
await tp.file.move(folder + tp.file.title)
*%>
```

### 按快捷键触发

设置 → 快捷键 → 搜索 "Templater"

为常用模板设置快捷键：
- 日记模板：`Alt + D`
- 快速笔记：`Alt + N`

---

## 最佳实践

### 1. 模板组织

```
模板/
├── 核心/
│   ├── 日记.md
│   ├── 周报.md
│   └── 月报.md
├── 笔记/
│   ├── 书籍.md
│   ├── 文章.md
│   └── 课程.md
├── 工作/
│   ├── 会议.md
│   ├── 项目.md
│   └── 1on1.md
└── 片段/
    ├── 任务区块.md
    └── 元数据.md
```

### 2. 使用模板片段

创建可复用的片段：

```markdown
<!-- 片段：任务区块.md -->
## ✅ 任务

- [ ] <% tp.file.cursor() %>
```

在其他模板中引用：

```markdown
<% tp.file.include("[[模板/片段/任务区块]]") %>
```

### 3. 渐进式复杂化

```markdown
// 从简单开始
# <% tp.date.now("YYYY-MM-DD") %>

// 逐渐添加功能
# <% tp.date.now("YYYY-MM-DD dddd") %>
<%* if (tp.date.now("d") === "1") { *%>
📅 本周开始！
<%* } *%>
```

### 4. 条件模板

```markdown
<%*
let type = await tp.system.suggester(
  ["工作", "个人"],
  ["work", "personal"]
)
*%>

<%* if (type === "work") { *%>
## 工作相关内容
<%* } else { *%>
## 个人相关内容
<%* } *%>
```

---

## 常见问题

### Q1: 模板不工作？

检查：
1. 模板文件夹设置正确
2. 语法无误（`<%` 和 `%>` 配对）
3. 文件在模板文件夹中

### Q2: 如何调试模板？

```markdown
<%*
console.log("调试信息", variable)
// 在开发者控制台查看（Ctrl/Cmd + Shift + I）
*%>
```

### Q3: async/await 报错？

用户交互函数需要使用 `await`：

```markdown
<%*
// 正确
let input = await tp.system.prompt("输入：")

// 错误
let input = tp.system.prompt("输入：")
*%>
```

### Q4: 如何获取 frontmatter？

```markdown
<%*
// 方法1：直接访问
let author = tp.frontmatter.author

// 方法2：安全访问（防止 undefined）
let author = tp.frontmatter?.author || "未知"
*%>
```

---

## 下一步

掌握了 Templater 后，学习 [Daily Notes 工作流](03-daily-notes.md)，将模板与每日笔记系统结合使用！

---

## 相关资源

- [Templater 官方文档](https://silentvoid13.github.io/Templater/)
- [Templater GitHub](https://github.com/SilentVoid13/Templater)
- [Moment.js 日期格式](https://momentjs.com/docs/#/displaying/format/)
