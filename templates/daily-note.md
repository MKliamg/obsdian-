---
date: <% tp.date.now("YYYY-MM-DD") %>
day: <% tp.date.now("dddd") %>
week: <% tp.date.now("WW") %>
---

# 📅 <% tp.date.now("YYYY年MM月DD日 dddd") %>

## ☀️ 早间规划

### 🎯 今日三件事
1. 
2. 
3. 

### 📋 任务清单
- [ ] 

## 📝 日间记录

<% tp.file.cursor() %>

## 🌙 晚间回顾

### ✅ 完成了什么

### 🤔 学到了什么

### 💪 明日改进

### 🙏 今日感恩
1. 
2. 
3. 

## 📊 习惯追踪

| 习惯 | 完成 |
|------|------|
| 🏃 运动 | ⬜ |
| 📚 阅读 | ⬜ |
| 🧘 冥想 | ⬜ |
| 💧 喝水 | ⬜ |

---

**导航**：[[<% tp.date.now("YYYY-MM-DD", -1) %>|← 昨天]] | [[<% tp.date.now("YYYY-[W]WW") %>|📅 本周]] | [[<% tp.date.now("YYYY-MM-DD", 1) %>|明天 →]]
