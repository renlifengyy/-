# Claude Code 学习笔记

> 基于 [luongnv89/claude-howto](https://github.com/luongnv89/claude-howto) 项目的中文学习指南
>
> 从零开始，一步步掌握 Claude Code 的全部功能

---

## 这是什么？

Claude Code 是 Anthropic 推出的官方 CLI 工具，可以直接在终端中与 Claude 协作编程。它不仅是一个聊天工具，更是一个可以读写文件、执行命令、管理项目的智能开发助手。

本笔记将帮助你系统学习 Claude Code 的核心功能，从最基础的命令到高级的自动化工作流。

---

## 学习路线图

### 入门级（约 2.5 小时）

| 模块 | 主题 | 预计时长 | 说明 |
|------|------|----------|------|
| [01-斜杠命令](01-斜杠命令/README.md) | Slash Commands | 30 分钟 | 最常用的交互方式，快速上手 |
| [02-记忆系统](02-记忆系统/README.md) | Memory | 45 分钟 | 让 Claude 记住你的项目规范 |
| [03-检查点](03-检查点/README.md) | Checkpoints | 45 分钟 | 安全实验，随时回退 |
| [04-CLI基础](04-CLI基础/README.md) | CLI Basics | 30 分钟 | 命令行的各种用法 |

### 中级（约 4.5 小时）

| 模块 | 主题 | 预计时长 | 说明 |
|------|------|----------|------|
| [05-技能](05-技能/README.md) | Skills | 1 小时 | 自动触发的可复用能力 |
| [06-钩子](06-钩子/README.md) | Hooks | 1 小时 | 事件驱动的自动化 |
| [07-MCP协议](07-MCP协议/README.md) | MCP | 1 小时 | 连接外部工具和服务 |
| [08-子代理](08-子代理/README.md) | Subagents | 1.5 小时 | 委托专业化的子任务 |

### 高级（约 4-5 小时）

| 模块 | 主题 | 预计时长 | 说明 |
|------|------|----------|------|
| [09-高级功能](09-高级功能/README.md) | Advanced | 2-3 小时 | 规划模式、权限、自动化 |
| [10-插件](10-插件/README.md) | Plugins | 2 小时 | 打包和分发完整方案 |

**总计：约 11-13 小时**（可根据自身水平跳过已掌握的部分）

---

## 快速开始

如果你只有 15 分钟，做这些就能立刻受益：

```bash
# 1. 创建你的第一个自定义命令
mkdir -p .claude/commands
cat > .claude/commands/optimize.md << 'EOF'
分析当前代码，找出可以优化的地方：
1. 性能瓶颈
2. 代码重复
3. 可读性改进
给出具体的修改建议和代码示例。
EOF

# 2. 在 Claude Code 中使用
# 输入 /optimize 即可触发
```

如果你有 1 小时，建议完成模块 1 和模块 2。

---

## 学习建议

1. **按顺序学习** — 模块之间有依赖关系，后面的模块会用到前面的知识
2. **动手实践** — 每个模块都有可以直接复制使用的示例，建议在自己的项目中试用
3. **善用检查点** — 学到模块 3 后，在尝试新功能前先创建检查点，这样可以放心实验
4. **参考原项目** — 每个模块都附有原项目的链接，想深入了解可以查看英文原文

---

## 参考资源

- [claude-howto 原项目](https://github.com/luongnv89/claude-howto) — 本笔记的参考来源
- [Claude Code 官方文档](https://docs.anthropic.com/en/docs/claude-code) — Anthropic 官方文档
- [Claude Code GitHub](https://github.com/anthropics/claude-code) — Claude Code 源代码
