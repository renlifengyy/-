# 模块 2：记忆系统（Memory）

> 预计学习时间：45 分钟
> 难度：入门

---

## 什么是记忆系统？

Claude Code 默认情况下每次启动都是"全新的"——它不记得上次对话的内容。**记忆系统**让你可以持久化地告诉 Claude 关于项目的关键信息，比如编码规范、技术栈、团队约定等。

核心机制就是 **CLAUDE.md** 文件——Claude Code 每次启动时会自动读取它。

---

## 记忆文件的层级

记忆文件有多个层级，从高到低依次是：

```
组织级（最高优先级）
  ↓  由管理员通过 Anthropic Console 配置
项目级 CLAUDE.md
  ↓  放在项目根目录
目录级 CLAUDE.md
  ↓  放在特定子目录中
.claude/rules/ 目录
  ↓  模块化的规则文件
个人级 CLAUDE.md
  ↓  ~/.claude/CLAUDE.md
自动记忆
     Claude 自动记录的偏好
```

### 各层级说明

| 层级 | 位置 | 作用 | 谁可见 |
|------|------|------|--------|
| 组织级 | Anthropic Console | 公司统一政策 | 组织内所有人 |
| 项目级 | `./CLAUDE.md` | 项目规范和约定 | 所有团队成员 |
| 目录级 | `./src/api/CLAUDE.md` | 特定目录的规则 | 访问该目录时 |
| 规则目录 | `.claude/rules/*.md` | 模块化规则 | 所有团队成员 |
| 个人级 | `~/.claude/CLAUDE.md` | 个人偏好 | 仅自己 |
| 自动记忆 | 自动管理 | Claude 学到的偏好 | 仅自己 |

---

## 如何编写有效的 CLAUDE.md

### 基本结构模板

```markdown
# 项目名称

## 项目概述
- 简述项目是做什么的
- 主要技术栈

## 项目结构
- 关键目录说明

## 编码规范
- 命名约定
- 代码风格
- 提交信息格式

## 常用命令
- 构建、测试、部署命令

## 重要约定
- 团队特有的规则
```

### 实战：项目级 CLAUDE.md

以下是一个 Web 项目的 CLAUDE.md 示例：

```markdown
# MyApp

## 技术栈
- 前端: React 18 + TypeScript + Tailwind CSS
- 后端: Node.js + Express + PostgreSQL
- 测试: Jest + React Testing Library

## 项目结构
- src/components/ — React 组件
- src/api/ — API 路由
- src/utils/ — 工具函数
- src/types/ — TypeScript 类型定义

## 编码规范
- 使用函数组件和 Hooks，不用 class 组件
- 文件名使用 kebab-case，组件名使用 PascalCase
- API 响应统一用 { data, error, message } 格式
- 不要使用 any 类型

## 常用命令
- npm run dev — 启动开发服务器
- npm test — 运行测试
- npm run build — 构建生产版本
- npm run lint — 代码检查

## Git 规范
- 提交信息使用 Conventional Commits 格式
- feat: 新功能 / fix: 修复 / docs: 文档 / refactor: 重构
- PR 标题不超过 70 字符
```

### 实战：目录级 CLAUDE.md

在 `src/api/` 目录下放一个 CLAUDE.md：

```markdown
# API 目录规则

- 所有路由文件以 .routes.ts 结尾
- 每个路由文件必须有对应的 .test.ts 文件
- 使用 zod 验证请求参数
- 错误处理统一使用 AppError 类
- 所有数据库操作放在 services/ 目录中
```

### 实战：个人级偏好

`~/.claude/CLAUDE.md`：

```markdown
# 我的偏好

- 用中文回复我
- 代码注释用英文
- 优先使用函数式编程风格
- 给出修改建议时，同时给出修改前后的对比
- 不需要过度解释基础概念
```

---

## 自动记忆（Auto Memory）

Claude Code 会自动记录你在对话中表达的偏好。比如：

- 你说"以后用中文回复"，Claude 会记住
- 你说"我喜欢用 tab 而不是 space"，Claude 会记住

这些自动记忆存储在 `~/.claude/` 中，无需手动管理。

---

## 最佳实践

1. **项目级 CLAUDE.md 放在 git 中** — 让团队共享一致的规范
2. **保持简洁** — 不要写太多，只写 Claude 需要知道的关键信息
3. **定期更新** — 项目变化时及时更新记忆文件
4. **用 .claude/rules/ 模块化** — 规则多的时候，按主题拆分到不同文件
5. **个人偏好不要放项目级** — 个人习惯用 `~/.claude/CLAUDE.md`

---

## 练习

1. 为你当前的项目创建一个 CLAUDE.md 文件
2. 在某个子目录中创建目录级的 CLAUDE.md
3. 配置你的个人偏好文件 `~/.claude/CLAUDE.md`
4. 试试在对话中说"以后请用中文回复"，观察自动记忆功能

---

## 参考

- [原项目 02-memory](https://github.com/luongnv89/claude-howto/tree/main/02-memory)
- [← 上一模块：斜杠命令](../01-斜杠命令/README.md) | [下一模块：检查点 →](../03-检查点/README.md)
