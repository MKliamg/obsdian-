# Dataview 查询示例

本文件包含常用的 Dataview 查询代码，可以直接复制使用。

---

## 📋 基础查询

### 最近修改的笔记

````markdown
```dataview
TABLE file.mtime AS "修改时间"
FROM ""
SORT file.mtime DESC
LIMIT 10
```
````

### 最近创建的笔记

````markdown
```dataview
TABLE file.ctime AS "创建时间"
FROM ""
SORT file.ctime DESC
LIMIT 10
```
````

### 按标签列出笔记

````markdown
```dataview
LIST
FROM #标签名
SORT file.name ASC
```
````

### 按文件夹列出笔记

````markdown
```dataview
LIST
FROM "文件夹名"
SORT file.name ASC
```
````

---

## ✅ 任务查询

### 所有未完成任务

````markdown
```dataview
TASK
FROM ""
WHERE !completed
SORT file.name ASC
```
````

### 今日待办

````markdown
```dataview
TASK
FROM ""
WHERE !completed AND contains(text, date(today))
```
````

### 本周任务

````markdown
```dataview
TASK
FROM "日记"
WHERE file.day >= date(today) - dur(7 days)
GROUP BY file.link
```
````

### 带截止日期的任务

````markdown
```dataview
TASK
FROM ""
WHERE !completed AND due
SORT due ASC
```
````

---

## 📚 项目管理

### 活跃项目列表

````markdown
```dataview
TABLE 
  status AS "状态",
  deadline AS "截止日期"
FROM "项目"
WHERE status = "active"
SORT deadline ASC
```
````

### 项目进度追踪

````markdown
```dataview
TABLE WITHOUT ID
  file.link AS "项目",
  length(file.tasks.where(t => t.completed)) AS "已完成",
  length(file.tasks) AS "总数",
  round(length(file.tasks.where(t => t.completed)) / length(file.tasks) * 100) + "%" AS "进度"
FROM "项目"
WHERE length(file.tasks) > 0
SORT file.name ASC
```
````

---

## 📖 读书追踪

### 阅读中的书籍

````markdown
```dataview
TABLE 
  author AS "作者",
  rating AS "评分",
  file.ctime AS "开始日期"
FROM #book
WHERE status = "阅读中"
SORT file.ctime DESC
```
````

### 按评分排序的书籍

````markdown
```dataview
TABLE 
  author AS "作者",
  rating AS "评分"
FROM #book
WHERE rating
SORT rating DESC
```
````

### 年度阅读统计

````markdown
```dataview
TABLE WITHOUT ID
  length(rows) AS "数量"
FROM #book
WHERE status = "已完成" AND file.ctime >= date(2024-01-01)
GROUP BY dateformat(file.ctime, "yyyy-MM") AS "月份"
```
````

---

## 📅 日记相关

### 本周日记链接

````markdown
```dataview
LIST
FROM "日记"
WHERE file.day >= date(today) - dur(7 days)
SORT file.day DESC
```
````

### 日历视图

````markdown
```dataview
CALENDAR file.day
FROM "日记"
```
````

### 日记字数统计

````markdown
```dataview
TABLE file.size AS "字符数"
FROM "日记"
WHERE file.day >= date(today) - dur(30 days)
SORT file.day DESC
```
````

---

## 🏷️ 标签统计

### 标签使用统计

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

---

## 🔗 链接分析

### 链入最多的笔记

````markdown
```dataview
TABLE length(file.inlinks) AS "被链接次数"
FROM ""
SORT length(file.inlinks) DESC
LIMIT 10
```
````

### 孤立笔记（无链接）

````markdown
```dataview
LIST
FROM ""
WHERE length(file.inlinks) = 0 AND length(file.outlinks) = 0
```
````

---

## 📊 统计信息

### 知识库统计

````markdown
```dataviewjs
let pages = dv.pages()
let tasks = pages.file.tasks.array().flat()

dv.paragraph(`📚 笔记总数：${pages.length}`)
dv.paragraph(`✅ 任务总数：${tasks.length}`)
dv.paragraph(`☑️ 已完成：${tasks.filter(t => t.completed).length}`)
dv.paragraph(`⬜ 未完成：${tasks.filter(t => !t.completed).length}`)
```
````

### 每月创建笔记数

````markdown
```dataview
TABLE WITHOUT ID
  length(rows) AS "数量"
FROM ""
WHERE file.ctime >= date(2024-01-01)
GROUP BY dateformat(file.ctime, "yyyy-MM") AS "月份"
SORT rows[0].file.ctime DESC
```
````

---

## 💡 使用提示

1. 复制代码块（包括 ``` 标记）
2. 粘贴到你的 Obsidian 笔记中
3. 根据需要修改：
   - 文件夹路径
   - 标签名称
   - 字段名称
   - 日期范围
4. 查看结果

如需更多示例，请参考 [Dataview 官方文档](https://blacksmithgu.github.io/obsidian-dataview/)。
