# Dataview 插件

## 目录
- [什么是 Dataview](#什么是-dataview)
- [安装与入门](#安装与入门)
- [基础查询](#基础查询)
- [进阶查询](#进阶查询)
- [Inline 查询](#inline-查询)
- [DataviewJS](#dataviewjs)
- [实用示例](#实用示例)
- [最佳实践](#最佳实践)
- [常见问题](#常见问题)

---

## 什么是 Dataview

Dataview 是 Obsidian 最强大的插件之一，它允许你将笔记库当作**数据库**来查询和展示信息。

### 核心能力

| 功能 | 描述 |
|------|------|
| 📊 **数据查询** | 从笔记中提取和聚合数据 |
| 📋 **自动列表** | 自动生成笔记列表 |
| 📈 **表格展示** | 以表格形式展示信息 |
| 🔄 **实时更新** | 查询结果随笔记变化自动更新 |

### 应用场景

- 📚 书籍阅读追踪
- ✅ 任务管理看板
- 📅 日程和会议汇总
- 📖 知识库索引
- 📊 项目进度追踪

---

## 安装与入门

### 安装 Dataview

1. 设置 → 社区插件 → 浏览
2. 搜索 "Dataview"
3. 安装并启用

### 基本设置

推荐设置（设置 → Dataview）：

| 设置 | 推荐值 | 说明 |
|------|--------|------|
| 启用 JavaScript 查询 | 开启 | 允许使用 DataviewJS |
| 日期格式 | `yyyy-MM-dd` | 统一日期格式 |
| 启用内联查询 | 开启 | 允许行内查询 |

### 数据来源

Dataview 可以从以下位置获取数据：

#### 1. YAML Frontmatter

```yaml
---
title: 我的笔记
tags: 
  - 教程
  - Obsidian
status: 进行中
rating: 4
date: 2024-01-15
---
```

#### 2. 内联字段

```markdown
正文中也可以定义字段：
作者:: 张三
评分:: ⭐⭐⭐⭐
完成日期:: 2024-01-15
```

#### 3. 隐式字段

Dataview 自动提供的字段：

| 字段 | 描述 |
|------|------|
| `file.name` | 文件名 |
| `file.path` | 文件路径 |
| `file.size` | 文件大小 |
| `file.ctime` | 创建时间 |
| `file.mtime` | 修改时间 |
| `file.day` | 日期笔记的日期 |
| `file.tags` | 所有标签 |
| `file.inlinks` | 链入的笔记 |
| `file.outlinks` | 链出的笔记 |
| `file.tasks` | 笔记中的任务 |

---

## 基础查询

### 查询类型

| 类型 | 语法 | 用途 |
|------|------|------|
| LIST | `LIST` | 显示笔记列表 |
| TABLE | `TABLE` | 显示表格 |
| TASK | `TASK` | 显示任务列表 |
| CALENDAR | `CALENDAR` | 显示日历视图 |

### LIST 查询

````markdown
```dataview
LIST
FROM "文件夹名"
WHERE status = "进行中"
SORT file.name ASC
```
````

**示例：列出所有读书笔记**

````markdown
```dataview
LIST
FROM "读书笔记"
SORT file.ctime DESC
```
````

### TABLE 查询

````markdown
```dataview
TABLE 字段1, 字段2, 字段3
FROM "来源"
WHERE 条件
SORT 排序字段 DESC
```
````

**示例：书籍追踪表**

````markdown
```dataview
TABLE 
  作者 AS "作者",
  rating AS "评分",
  status AS "状态",
  file.ctime AS "添加日期"
FROM "读书笔记"
WHERE type = "书籍"
SORT rating DESC
```
````

### TASK 查询

````markdown
```dataview
TASK
FROM "项目"
WHERE !completed
SORT file.name ASC
```
````

**示例：本周待办**

````markdown
```dataview
TASK
FROM "日记"
WHERE !completed AND file.day >= date(today) - dur(7 days)
```
````

### CALENDAR 查询

````markdown
```dataview
CALENDAR file.day
FROM "日记"
```
````

---

## 进阶查询

### FROM 来源

```dataview
// 从文件夹
FROM "文件夹名"

// 从标签
FROM #标签名

// 从链接到特定笔记的文件
FROM [[笔记名]]

// 排除特定来源
FROM "项目" AND -"项目/归档"

// 组合条件
FROM #工作 OR #学习
```

### WHERE 条件

```dataview
// 基本比较
WHERE rating >= 4
WHERE status = "完成"
WHERE author != "未知"

// 包含检查
WHERE contains(tags, "重要")
WHERE contains(file.name, "会议")

// 日期比较
WHERE date >= date(2024-01-01)
WHERE file.mtime >= date(today) - dur(7 days)

// 存在性检查
WHERE author
WHERE !completed

// 组合条件
WHERE rating >= 4 AND status = "完成"
WHERE type = "书籍" OR type = "文章"
```

### SORT 排序

```dataview
// 升序
SORT file.name ASC

// 降序
SORT rating DESC

// 多重排序
SORT status ASC, rating DESC

// 按日期排序
SORT file.ctime DESC
```

### GROUP BY 分组

````markdown
```dataview
TABLE rows.file.link AS "笔记"
FROM "读书笔记"
GROUP BY status
```
````

### FLATTEN 展开

将数组字段展开为多行：

````markdown
```dataview
TABLE author
FROM "读书笔记"
FLATTEN author
```
````

### LIMIT 限制

```dataview
// 只显示前 10 个结果
LIMIT 10
```

---

## Inline 查询

在正文中直接嵌入查询结果：

```markdown
今天是 `= date(today)`

这个库共有 `= length(dv.pages())` 篇笔记

本周创建了 `= length(dv.pages().where(p => p.file.ctime >= dv.date("today") - dv.duration("7 days")))` 篇笔记
```

### 常用内联查询

| 需求 | 查询 |
|------|------|
| 当前日期 | `` `= date(today)` `` |
| 笔记总数 | `` `= length(dv.pages())` `` |
| 文件名 | `` `= this.file.name` `` |
| 创建时间 | `` `= this.file.ctime` `` |
| 字数统计 | `` `= this.file.size` `` |

---

## DataviewJS

DataviewJS 提供更强大的编程能力：

````markdown
```dataviewjs
// 获取所有笔记
let pages = dv.pages()

// 过滤
let books = dv.pages('#book')
  .where(p => p.rating >= 4)
  .sort(p => p.rating, 'desc')

// 显示表格
dv.table(
  ["书名", "作者", "评分"],
  books.map(p => [p.file.link, p.author, p.rating])
)
```
````

### 常用 DataviewJS 方法

| 方法 | 描述 |
|------|------|
| `dv.pages()` | 获取所有页面 |
| `dv.pages("#tag")` | 按标签获取 |
| `dv.pages('"folder"')` | 按文件夹获取 |
| `dv.page("笔记名")` | 获取单个页面 |
| `dv.list()` | 显示列表 |
| `dv.table()` | 显示表格 |
| `dv.taskList()` | 显示任务 |
| `dv.paragraph()` | 显示段落 |
| `dv.header()` | 显示标题 |
| `dv.date("today")` | 当前日期 |
| `dv.duration("7 days")` | 时间长度 |

### 进阶示例

**带进度条的项目列表**：

````markdown
```dataviewjs
let projects = dv.pages("#project")

for (let project of projects) {
  let tasks = project.file.tasks
  let completed = tasks.filter(t => t.completed).length
  let total = tasks.length
  let percent = total > 0 ? Math.round(completed / total * 100) : 0
  
  dv.paragraph(`**${project.file.link}**`)
  dv.paragraph(`进度: ${completed}/${total} (${percent}%)`)
  dv.paragraph(`${"█".repeat(percent/10)}${"░".repeat(10-percent/10)}`)
  dv.paragraph("---")
}
```
````

---

## 实用示例

### 1. 最近修改的笔记

````markdown
```dataview
TABLE file.mtime AS "修改时间"
FROM ""
WHERE file.name != this.file.name
SORT file.mtime DESC
LIMIT 10
```
````

### 2. 未完成的阅读

````markdown
```dataview
TABLE 
  author AS "作者",
  progress AS "进度",
  file.mtime AS "上次阅读"
FROM #book
WHERE status != "已完成"
SORT file.mtime DESC
```
````

### 3. 本周任务汇总

````markdown
```dataview
TASK
FROM "日记"
WHERE file.day >= date(today) - dur(7 days)
GROUP BY file.link
```
````

### 4. 标签云统计

````markdown
```dataviewjs
let tags = new Map()
dv.pages().forEach(p => {
  if (p.file.tags) {
    p.file.tags.forEach(t => {
      tags.set(t, (tags.get(t) || 0) + 1)
    })
  }
})

let sorted = [...tags.entries()]
  .sort((a, b) => b[1] - a[1])
  .slice(0, 20)

dv.table(["标签", "数量"], sorted)
```
````

### 5. 每日笔记链接

````markdown
```dataview
LIST
FROM "日记"
WHERE file.day >= date(today) - dur(7 days)
SORT file.day DESC
```
````

### 6. 项目仪表板

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "项目",
  status AS "状态",
  length(file.tasks.where(t => t.completed)) + "/" + length(file.tasks) AS "进度",
  deadline AS "截止日期"
FROM #project
WHERE status != "已归档"
SORT deadline ASC
```
````

---

## 最佳实践

### 1. 统一元数据格式

```yaml
---
type: book
title: 书名
author: 作者
status: 阅读中
rating: 
date-started: 2024-01-01
date-finished: 
---
```

### 2. 使用别名让表格更美观

```dataview
TABLE 
  author AS "📝 作者",
  rating AS "⭐ 评分",
  status AS "📊 状态"
```

### 3. 避免过于复杂的查询

```dataview
// 好：简单直接
TABLE status, rating
FROM #book
WHERE rating >= 4

// 避免：过度复杂的嵌套
```

### 4. 创建查询模板

将常用查询保存为模板，需要时直接插入。

### 5. 组织查询笔记

创建专门的"仪表板"笔记来放置 Dataview 查询：

```
仪表板/
├── 项目看板.md
├── 阅读追踪.md
├── 任务总览.md
└── 知识统计.md
```

---

## 常见问题

### Q1: 查询结果为空？

检查：
1. FROM 路径是否正确
2. WHERE 条件是否太严格
3. 字段名是否拼写正确
4. 笔记中是否有对应的元数据

### Q2: 查询速度很慢？

优化方法：
1. 缩小搜索范围（使用更具体的 FROM）
2. 减少不必要的字段
3. 避免在大型库中使用复杂的 DataviewJS
4. 使用 LIMIT 限制结果数量

### Q3: 日期格式不正确？

确保使用标准格式：
```yaml
date: 2024-01-15
```

并在 Dataview 设置中配置正确的日期格式。

### Q4: 如何调试查询？

1. 简化查询，逐步添加条件
2. 使用 `dv.paragraph()` 输出中间结果
3. 检查 Obsidian 开发者控制台

---

## 下一步

Dataview 配合 [Templater 插件](02-templater.md) 使用效果更佳！学习如何创建动态模板来自动填充元数据。

---

## 相关资源

- [Dataview 官方文档](https://blacksmithgu.github.io/obsidian-dataview/)
- [Dataview GitHub](https://github.com/blacksmithgu/obsidian-dataview)
- [Dataview 示例库](https://s-blu.github.io/obsidian_dataview_example_vault/)
