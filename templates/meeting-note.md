---
type: meeting
date: <% tp.date.now("YYYY-MM-DD") %>
time: <% tp.date.now("HH:mm") %>
participants: 
tags:
  - meeting
---

# 📅 会议：<% await tp.system.prompt("会议主题：") %>

## 📋 基本信息

| 项目 | 内容 |
|------|------|
| 日期 | <% tp.date.now("YYYY-MM-DD") %> |
| 时间 | <% tp.date.now("HH:mm") %> |
| 地点 | <% await tp.system.prompt("会议地点（可留空）：") || "线上" %> |
| 参与者 | <% tp.file.cursor(1) %> |

## 📋 会议议程

1. 
2. 
3. 

## 📝 会议记录

### 讨论要点

<% tp.file.cursor(2) %>

### 决策事项

- 

### 待定问题

- 

## ✅ 行动项目

| 任务 | 负责人 | 截止日期 | 状态 |
|------|--------|----------|------|
| | | | ⏳ |
| | | | ⏳ |
| | | | ⏳ |

## 📎 相关资料

- 

## 💡 备注

---

**下次会议**：
