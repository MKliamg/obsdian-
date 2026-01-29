# 贡献指南

感谢你对本项目的关注！我们欢迎任何形式的贡献，无论是修复错误、改进文档还是添加新功能。

## 📋 目录

- [行为准则](#行为准则)
- [如何贡献](#如何贡献)
- [提交规范](#提交规范)
- [文档规范](#文档规范)
- [问题反馈](#问题反馈)

## 行为准则

请尊重所有参与者，保持友好和建设性的交流氛围。

## 如何贡献

### 1. Fork 项目

点击页面右上角的 Fork 按钮，将项目复制到你的账户。

### 2. 克隆仓库

```bash
git clone https://github.com/你的用户名/obsdian-.git
cd obsdian-
```

### 3. 创建分支

```bash
git checkout -b feature/你的功能名称
```

### 4. 进行修改

编辑相关文件，确保符合项目规范。

### 5. 提交更改

```bash
git add .
git commit -m "feat: 添加新功能描述"
```

### 6. 推送分支

```bash
git push origin feature/你的功能名称
```

### 7. 创建 Pull Request

在 GitHub 上创建 Pull Request，描述你的更改内容。

## 提交规范

我们使用 [Conventional Commits](https://www.conventionalcommits.org/) 规范：

| 类型 | 描述 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 bug |
| `docs` | 文档更新 |
| `style` | 格式调整（不影响代码含义） |
| `refactor` | 代码重构 |
| `test` | 添加测试 |
| `chore` | 构建过程或辅助工具的变动 |

### 示例

```
feat: 添加 Dataview 插件教程
fix: 修复 Markdown 语法示例错误
docs: 更新安装指南
```

## 文档规范

### 文件命名

- 使用小写字母
- 使用连字符分隔单词
- 示例：`01-introduction.md`

### 标题层级

- H1（`#`）：仅用于文档标题
- H2（`##`）：主要章节
- H3（`###`）：子章节
- H4（`####`）：小节

### 代码块

使用正确的语言标记：

````markdown
```javascript
console.log("Hello");
```
````

### 图片

- 存放在 `resources/images/` 目录
- 使用相对路径引用
- 添加 alt 文本

```markdown
![描述文字](../resources/images/example.png)
```

## 问题反馈

### 报告 Bug

1. 搜索是否已有类似问题
2. 如果没有，创建新 Issue
3. 使用 Bug 报告模板
4. 提供详细的复现步骤

### 功能建议

1. 搜索是否已有类似建议
2. 创建新 Issue
3. 使用功能建议模板
4. 详细描述你的需求

## 🎉 感谢

感谢所有贡献者的付出！
