# 插件配置指南

## 目录
- [Dataview 配置](#dataview-配置)
- [Templater 配置](#templater-配置)
- [Calendar 配置](#calendar-配置)
- [Tasks 配置](#tasks-配置)
- [QuickAdd 配置](#quickadd-配置)
- [Obsidian Git 配置](#obsidian-git-配置)

---

## Dataview 配置

### 推荐设置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 启用 JavaScript 查询 | ✅ 开启 | 允许使用 DataviewJS |
| 启用内联查询 | ✅ 开启 | 允许行内查询 |
| 日期格式 | `yyyy-MM-dd` | 统一日期格式 |
| 刷新间隔 | 2500ms | 自动刷新间隔 |

### 完整配置步骤

1. 安装 Dataview 插件
2. 进入设置 → Dataview
3. 启用 JavaScript 查询
4. 启用内联查询
5. 设置日期格式

### 常用查询模板

```markdown
<!-- 保存这些查询以便复用 -->

# 最近修改的笔记
```dataview
LIST
SORT file.mtime DESC
LIMIT 10
```

# 所有未完成任务
```dataview
TASK
WHERE !completed
SORT file.name ASC
```
```

---

## Templater 配置

### 推荐设置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 模板文件夹位置 | `模板/` | 存放模板的文件夹 |
| 语法高亮 | ✅ 开启 | 编辑器中高亮模板语法 |
| 自动跳转到光标 | ✅ 开启 | 插入模板后跳到光标位置 |
| 触发文件创建 | ✅ 开启 | 新建文件时触发模板 |

### 文件夹模板设置

在 Templater 设置中配置"文件夹模板"：

| 文件夹 | 模板 |
|--------|------|
| `日记/` | `模板/日记模板.md` |
| `项目/` | `模板/项目模板.md` |
| `读书笔记/` | `模板/书籍模板.md` |
| `会议/` | `模板/会议模板.md` |

### 创建模板文件夹

```
模板/
├── 日记模板.md
├── 周报模板.md
├── 项目模板.md
├── 书籍模板.md
├── 会议模板.md
└── 人物模板.md
```

---

## Calendar 配置

### 推荐设置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 周的开始 | 周一 | 一周从周一开始 |
| 显示周数 | ✅ 开启 | 显示周数便于创建周报 |
| 日期格式 | `YYYY-MM-DD` | 与日记文件名匹配 |

### 与日记插件集成

确保 Calendar 和核心日记插件使用相同的：
- 日期格式
- 笔记存放位置
- 模板位置

### 与 Periodic Notes 集成

安装 Periodic Notes 插件后：
1. 在 Calendar 设置中启用周报链接
2. 配置周报格式：`YYYY-[W]WW`
3. 点击周数可创建周报

---

## Tasks 配置

### 推荐设置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 全局筛选器 | 留空 | 搜索所有笔记 |
| 自动完成日期 | ✅ 开启 | 记录完成时间 |
| 日期格式 | `YYYY-MM-DD` | 统一日期格式 |

### 任务格式

```markdown
# 基本任务
- [ ] 普通任务

# 带日期任务
- [ ] 任务 📅 2024-01-20

# 带优先级任务
- [ ] 高优先级任务 ⏫
- [ ] 中优先级任务 🔼
- [ ] 低优先级任务 🔽

# 重复任务
- [ ] 每周任务 🔁 every week
```

### 常用查询

```markdown
# 今日待办
```tasks
not done
due today
```

# 本周任务
```tasks
not done
due before next week
due after last week
```

# 高优先级任务
```tasks
not done
priority is high
```
```

---

## QuickAdd 配置

### 基础设置

1. 安装 QuickAdd 插件
2. 进入设置 → QuickAdd
3. 创建"宏"或"捕获"

### 创建快速捕获

**配置"快速想法"捕获**：

1. 点击"管理宏"
2. 添加新项目："快速想法"
3. 类型：捕获
4. 目标文件：`0-收件箱/{{DATE}}.md`
5. 内容模板：
   ```
   - {{VALUE}} - {{TIME}}
   ```

### 创建模板宏

**配置"新建项目"宏**：

1. 添加新项目："新建项目"
2. 类型：模板
3. 模板路径：`模板/项目模板.md`
4. 新文件位置：`1-项目/`
5. 文件名：从模板或提示获取

### 设置快捷键

1. 进入设置 → 快捷键
2. 搜索 "QuickAdd"
3. 为常用命令设置快捷键

---

## Obsidian Git 配置

### 前提条件

1. 安装 Git
2. 有 GitHub/GitLab 账号
3. 配置 SSH 密钥或个人访问令牌

### 初始设置

```bash
# 在知识库目录中
cd 你的笔记库
git init
git remote add origin https://github.com/用户名/仓库名.git
```

### 插件设置

| 设置项 | 推荐值 | 说明 |
|--------|--------|------|
| 自动拉取间隔 | 5 分钟 | 定期拉取更新 |
| 自动提交间隔 | 10 分钟 | 定期提交更改 |
| 自动推送 | ✅ 开启 | 提交后自动推送 |
| 提交信息 | `vault backup: {{date}}` | 自动提交信息 |

### .gitignore 配置

创建 `.gitignore` 文件：

```gitignore
# Obsidian
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/plugins/recent-files-obsidian/data.json

# 系统文件
.DS_Store
Thumbs.db

# 临时文件
*.tmp
```

### 常见问题

**问题：合并冲突**
```bash
# 查看冲突文件
git status

# 手动解决后
git add .
git commit -m "resolve conflict"
git push
```

**问题：大文件无法推送**
```bash
# 使用 Git LFS 处理大文件
git lfs install
git lfs track "*.pdf"
git add .gitattributes
```

---

## 配置备份

### 导出设置

备份 `.obsidian` 文件夹：
- `appearance.json` - 外观设置
- `community-plugins.json` - 插件列表
- `hotkeys.json` - 快捷键
- `plugins/` - 插件数据

### 配置同步

如果使用云盘同步，`.obsidian` 文件夹会自动同步所有设置。

---

## 相关资源

- [推荐插件列表](recommended-plugins.md)
- [Dataview 教程](../advanced-techniques/01-dataview.md)
- [Templater 教程](../advanced-techniques/02-templater.md)
