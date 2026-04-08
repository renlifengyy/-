# 模块 10：插件（Plugins）

> 预计学习时间：2 小时
> 难度：高级
> 前置知识：模块 1-9（尤其是模块 1, 5, 6, 7, 8）

---

## 什么是插件？

插件是 Claude Code 中最高级的组织形式——它把**斜杠命令、技能、钩子、MCP Server、子代理**打包成一个完整的解决方案。

**类比**：
- 斜杠命令 = 单个工具（螺丝刀）
- 技能 = 自动触发的工具（电动螺丝刀）
- 插件 = 完整的工具箱（包含多种工具 + 使用说明）

---

## 插件的组成

一个插件可以包含以下任意组合：

```
my-plugin/
├── plugin.json          # 插件配置文件
├── commands/            # 斜杠命令
│   ├── review.md
│   └── deploy.md
├── skills/              # 技能
│   └── code-check/
│       └── skill.md
├── agents/              # 子代理
│   ├── reviewer.md
│   └── tester.md
├── hooks/               # 钩子脚本
│   ├── pre-commit.sh
│   └── format.sh
└── mcp/                 # MCP 配置
    └── config.json
```

---

## 插件配置文件

`plugin.json`：

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "完整的 PR 审查工作流",
  "author": "your-name",
  "components": {
    "commands": ["commands/"],
    "skills": ["skills/"],
    "agents": ["agents/"],
    "hooks": "hooks/",
    "mcp": "mcp/config.json"
  }
}
```

---

## 实战：创建 PR 审查插件

### 第 1 步：创建目录结构

```bash
mkdir -p .claude/plugins/pr-review/{commands,skills,agents,hooks}
```

### 第 2 步：创建斜杠命令

`.claude/plugins/pr-review/commands/review-pr.md`：

```markdown
对当前分支的 PR 进行全面审查：

1. 查看所有文件变更
2. 检查代码质量、安全性和性能
3. 验证测试覆盖率
4. 生成审查报告

$ARGUMENTS
```

### 第 3 步：创建子代理

`.claude/plugins/pr-review/agents/reviewer.md`：

```markdown
---
name: pr-reviewer
description: PR 代码审查专家
tools:
  - Read
  - Grep
  - Glob
---

你是一个 PR 审查专家。请审查代码变更，关注：
- 代码正确性和逻辑
- 安全漏洞
- 性能问题
- 编码规范合规性

输出结构化的审查报告。
```

### 第 4 步：创建钩子

`.claude/plugins/pr-review/hooks/check-pr-size.sh`：

```bash
#!/bin/bash
# 检查 PR 大小，超过 500 行发出警告
CHANGED_LINES=$(git diff --stat HEAD~1 | tail -1 | awk '{print $4}')
if [ "$CHANGED_LINES" -gt 500 ]; then
  echo "⚠️ PR 超过 500 行变更，建议拆分"
fi
```

### 第 5 步：配置插件

`.claude/plugins/pr-review/plugin.json`：

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "PR 审查自动化插件",
  "components": {
    "commands": ["commands/"],
    "agents": ["agents/"]
  }
}
```

---

## 更多插件示例

### DevOps 自动化插件

```
devops-automation/
├── plugin.json
├── commands/
│   ├── deploy.md         # 部署命令
│   ├── rollback.md       # 回滚命令
│   └── health-check.md   # 健康检查
├── hooks/
│   ├── pre-deploy.sh     # 部署前检查
│   └── post-deploy.sh    # 部署后通知
└── mcp/
    └── config.json       # 连接 CI/CD 服务
```

### 文档生成插件

```
documentation/
├── plugin.json
├── commands/
│   ├── generate-api-docs.md    # API 文档生成
│   └── generate-changelog.md   # 变更日志生成
├── skills/
│   └── doc-style/
│       └── skill.md            # 文档风格检查
└── agents/
    └── doc-writer.md           # 文档编写子代理
```

---

## 安装和使用插件

### 从项目安装

插件放在 `.claude/plugins/` 目录中，Claude Code 会自动识别。

### 从远程安装

```bash
# 克隆插件到本地
git clone https://github.com/someone/claude-plugin-xxx .claude/plugins/xxx
```

### 使用

安装后，插件中的命令、技能等自动可用：

```
你: /review-pr       # 使用插件中的命令
你: 审查这段代码       # 触发插件中的技能
```

---

## 团队分发

### 通过 Git 分发

```bash
# 将插件目录加入版本控制
git add .claude/plugins/my-plugin/
git commit -m "feat: add my-plugin"
```

团队成员拉取代码后自动获得插件。

### 通过独立仓库分发

```bash
# 将插件作为 git submodule
git submodule add https://github.com/team/claude-plugins .claude/plugins
```

---

## 插件设计最佳实践

1. **单一职责** — 一个插件解决一类问题（PR 审查、文档生成、部署自动化）
2. **完整自包含** — 插件内的组件能独立工作
3. **渐进增强** — 即使只安装部分组件也能用
4. **清晰文档** — 在 plugin.json 中写好描述
5. **版本管理** — 使用语义化版本号

---

## 各功能的关系总结

学完所有 10 个模块，你已经掌握了 Claude Code 的完整能力体系：

```
用户
 ↓ 斜杠命令（手动触发）
 ↓ 技能（自动触发）
Claude Code
 ├── 记忆系统（知道你的项目）
 ├── 钩子（事件自动化）
 ├── MCP（连接外部服务）
 ├── 子代理（委托专业任务）
 ├── 高级功能（规划、权限、思考）
 └── 检查点（安全回退）
 ↓
插件 = 以上所有的打包组合
```

---

## 练习

1. 创建一个简单的插件，包含 1 个命令 + 1 个子代理
2. 为你的团队设计一个完整的 PR 审查插件
3. 尝试将你之前创建的命令和技能组织成一个插件

---

## 参考

- [原项目 07-plugins](https://github.com/luongnv89/claude-howto/tree/main/07-plugins)
- [← 上一模块：高级功能](../09-高级功能/README.md)

---

## 恭喜！🎉

你已经完成了全部 10 个模块的学习！现在你掌握了：

- ✅ 斜杠命令 — 快速交互
- ✅ 记忆系统 — 持久化项目上下文
- ✅ 检查点 — 安全实验
- ✅ CLI 基础 — 命令行高效使用
- ✅ 技能 — 自动触发的能力
- ✅ 钩子 — 事件驱动自动化
- ✅ MCP 协议 — 连接外部服务
- ✅ 子代理 — 专业化任务委托
- ✅ 高级功能 — 规划、权限、自动化
- ✅ 插件 — 完整解决方案打包

**下一步**：在你的实际项目中开始使用这些功能吧！从最简单的斜杠命令和 CLAUDE.md 开始，逐步添加更多功能。

[回到主页 →](../README.md)
