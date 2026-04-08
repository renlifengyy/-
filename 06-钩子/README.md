# 模块 6：钩子（Hooks）

> 预计学习时间：1 小时
> 难度：中级
> 前置知识：模块 1-4

---

## 什么是钩子（Hooks）？

钩子是**事件驱动的自动化**机制。当特定事件发生时（比如会话启动、文件修改、命令执行），钩子会自动运行你定义的脚本或命令。

把它想象成 git hooks 的升级版——但作用范围更广，覆盖了 Claude Code 的整个工作流。

---

## 钩子事件类型

### 核心事件

| 事件 | 触发时机 | 典型用途 |
|------|----------|----------|
| `SessionStart` | 会话开始时 | 环境检查、依赖安装 |
| `PreToolUse` | 工具调用前 | 权限验证、安全检查 |
| `PostToolUse` | 工具调用后 | 格式化、日志记录 |
| `TaskCreated` | 子任务创建时 | 通知、日志 |
| `WorktreeCreate` | 创建工作树时 | 环境初始化 |

### 常见组合

```
SessionStart  →  检查环境是否就绪
PreToolUse    →  拦截危险操作
PostToolUse   →  自动格式化代码
```

---

## 配置方式

钩子在 `settings.json` 中配置：

**项目级**：`.claude/settings.json`
**用户级**：`~/.claude/settings.json`

### 基本结构

```json
{
  "hooks": {
    "事件名": [
      {
        "type": "command",
        "command": "要执行的命令"
      }
    ]
  }
}
```

---

## 实战示例

### 示例 1：会话启动时检查环境

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "command",
        "command": "echo '检查 Node.js 版本...' && node --version && echo '检查依赖...' && npm ls --depth=0 2>/dev/null || echo '提示: 请运行 npm install'"
      }
    ]
  }
}
```

### 示例 2：代码修改后自动格式化

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "npx prettier --write",
        "match_tool": "Edit"
      }
    ]
  }
}
```

当 Claude 使用 Edit 工具修改文件后，自动运行 Prettier 格式化。

### 示例 3：拦截危险的 bash 命令

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "type": "command",
        "command": "if echo \"$TOOL_INPUT\" | grep -qE 'rm -rf|drop table|force push'; then echo 'BLOCKED: 检测到危险命令' && exit 1; fi",
        "match_tool": "Bash"
      }
    ]
  }
}
```

### 示例 4：记录所有 bash 命令到日志

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "echo \"$(date '+%Y-%m-%d %H:%M:%S') - $TOOL_NAME: $TOOL_INPUT\" >> .claude/audit.log",
        "match_tool": "Bash"
      }
    ]
  }
}
```

### 示例 5：安全扫描钩子

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "type": "command",
        "command": "if echo \"$TOOL_INPUT\" | grep -qiE '(api.key|password|secret|token)\\s*='; then echo '⚠️ 警告: 可能包含敏感信息'; fi",
        "match_tool": "Edit"
      }
    ]
  }
}
```

---

## 钩子类型

| 类型 | 说明 | 示例 |
|------|------|------|
| `command` | 执行 shell 命令 | `"command": "npm run lint"` |
| `http` | 发送 HTTP 请求 | 通知 Webhook |
| `prompt` | 向 Claude 注入提示 | 添加额外上下文 |
| `agent` | 触发子代理 | 委托专门的处理 |

---

## 钩子的环境变量

钩子脚本中可以使用这些环境变量：

| 变量 | 说明 |
|------|------|
| `$TOOL_NAME` | 当前工具名称 |
| `$TOOL_INPUT` | 工具的输入内容 |
| `$SESSION_ID` | 当前会话 ID |

---

## 完整配置示例

一个综合的 `.claude/settings.json`：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "command",
        "command": "node --version && npm --version"
      }
    ],
    "PreToolUse": [
      {
        "type": "command",
        "command": "if echo \"$TOOL_INPUT\" | grep -qE 'rm -rf /'; then exit 1; fi",
        "match_tool": "Bash"
      }
    ],
    "PostToolUse": [
      {
        "type": "command",
        "command": "npx prettier --write",
        "match_tool": "Edit"
      },
      {
        "type": "command",
        "command": "echo \"$(date) $TOOL_NAME\" >> .claude/audit.log"
      }
    ]
  }
}
```

---

## 最佳实践

1. **先测试再启用** — 在脚本中先用 `echo` 验证逻辑
2. **保持轻量** — 钩子不应该耗时太长，会影响工作流速度
3. **用 match_tool 精确匹配** — 避免所有工具都触发
4. **安全钩子用 PreToolUse** — 在执行前拦截
5. **日志钩子用 PostToolUse** — 在执行后记录
6. **团队共享** — 项目级钩子放在 `.claude/settings.json` 并提交到 git

---

## 练习

1. 配置一个 `SessionStart` 钩子，检查项目环境
2. 配置一个 `PostToolUse` 钩子，在代码修改后自动格式化
3. 创建一个安全钩子，拦截包含敏感信息的代码修改

---

## 参考

- [原项目 06-hooks](https://github.com/luongnv89/claude-howto/tree/main/06-hooks)
- [← 上一模块：技能](../05-技能/README.md) | [下一模块：MCP 协议 →](../07-MCP协议/README.md)
