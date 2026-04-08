# 模块 7：MCP 协议（Model Context Protocol）

> 预计学习时间：1 小时
> 难度：中级
> 前置知识：模块 1-4

---

## 什么是 MCP？

MCP（Model Context Protocol）是一个开放协议，让 Claude Code 可以连接**外部工具和服务**。通过 MCP，Claude 可以：

- 操作 GitHub（创建 PR、查看 issues）
- 查询数据库
- 发送 Slack 消息
- 读写 Google Docs
- 连接任何提供 MCP Server 的服务

**简单理解**：MCP 就像 Claude 的"插头"，不同的 MCP Server 就像不同的"插座"，插上就能使用对应的外部能力。

---

## 核心概念

```
Claude Code（客户端）
    ↕ MCP 协议
MCP Server（服务端）
    ↕ API 调用
外部服务（GitHub, DB, Slack...）
```

- **MCP Client**：Claude Code 本身，内置了 MCP 客户端
- **MCP Server**：提供特定工具的服务程序
- **Tools**：MCP Server 暴露给 Claude 的具体功能

---

## 配置 MCP Server

在 `.claude/settings.json` 或 `~/.claude/settings.json` 中配置：

```json
{
  "mcpServers": {
    "服务名": {
      "command": "启动命令",
      "args": ["参数列表"],
      "env": {
        "环境变量": "值"
      }
    }
  }
}
```

---

## 常见 MCP Server 配置

### 1. GitHub MCP Server

操作 GitHub 仓库、PR、Issues：

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "ghp_你的token"
      }
    }
  }
}
```

配置后 Claude 可以：
- 创建和审查 Pull Request
- 查看和管理 Issues
- 浏览仓库文件和提交历史

### 2. 数据库 MCP Server

查询和管理数据库：

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost:5432/mydb"
      }
    }
  }
}
```

配置后 Claude 可以：
- 查询数据库表结构
- 执行 SQL 查询
- 分析数据

### 3. 文件系统 MCP Server

扩展文件操作能力：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/允许访问的目录"]
    }
  }
}
```

### 4. Slack MCP Server

```json
{
  "mcpServers": {
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-slack"],
      "env": {
        "SLACK_TOKEN": "xoxb-你的token"
      }
    }
  }
}
```

---

## 多 MCP Server 组合

可以同时配置多个 MCP Server：

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_xxx" }
    },
    "database": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "postgresql://..." }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-slack"],
      "env": { "SLACK_TOKEN": "xoxb-xxx" }
    }
  }
}
```

这样 Claude 就能同时操作 GitHub、数据库和 Slack！

---

## MCP vs 其他功能的区别

| 需求 | 用什么 |
|------|--------|
| 执行本地 shell 命令 | Bash 工具（内置） |
| 读写本地文件 | Read/Write 工具（内置） |
| 调用外部 API/服务 | MCP Server |
| 委托 AI 子任务 | Subagents（下一模块） |
| 自动化工作流 | Hooks |

**经验法则**：如果需要连接外部服务或获取实时数据，用 MCP。

---

## 安全注意事项

1. **不要硬编码 Token** — 使用环境变量或 `.env` 文件
2. **最小权限原则** — Token 只授予必要的权限
3. **项目级配置不要包含 Token** — Token 放在用户级配置或环境变量中
4. **.gitignore 中排除敏感文件** — 确保不会提交 Token

```bash
# .gitignore
.env
.claude/settings.local.json
```

---

## 实战场景

### 场景：自动化 PR 审查

配置 GitHub MCP 后：

```
你: 帮我审查 PR #42 的代码
Claude: [通过 MCP 读取 PR 内容]
       这个 PR 有以下几个问题：
       1. 缺少错误处理...
       2. 有一个潜在的 SQL 注入...
       [通过 MCP 在 PR 上留评论]
```

### 场景：数据分析

配置数据库 MCP 后：

```
你: 查看一下上个月的用户注册趋势
Claude: [通过 MCP 查询数据库]
       上个月共注册 1,234 名新用户...
       [生成分析报告]
```

---

## 练习

1. 配置一个 GitHub MCP Server（需要 GitHub Token）
2. 使用 MCP 工具让 Claude 查看你的 GitHub 仓库
3. 尝试配置多个 MCP Server 并让它们协同工作

---

## 参考

- [原项目 05-mcp](https://github.com/luongnv89/claude-howto/tree/main/05-mcp)
- [MCP 官方规范](https://modelcontextprotocol.io/)
- [← 上一模块：钩子](../06-钩子/README.md) | [下一模块：子代理 →](../08-子代理/README.md)
