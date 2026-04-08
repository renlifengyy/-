# 模块 4：CLI 基础（CLI Basics）

> 预计学习时间：30 分钟
> 难度：入门

---

## 两种使用模式

Claude Code 有两种主要使用模式：

### 交互模式（Interactive Mode）

就是你平时用的模式——启动 Claude Code 后，在终端中对话：

```bash
claude
```

进入后直接输入你的需求，Claude 会交互式地回应。

### 打印模式（Print Mode）

单次执行，适合脚本和自动化：

```bash
claude -p "解释这个项目的目录结构"
```

执行完毕后直接退出，输出结果到标准输出。适合：
- Shell 脚本中调用
- CI/CD 管道
- 批处理任务

---

## 常用 CLI 参数

### 基础参数

```bash
# 启动交互模式
claude

# 单次执行（打印模式）
claude -p "你的问题"

# 恢复上次会话
claude --resume

# 指定模型
claude --model claude-sonnet-4-6

# 使用特定权限模式
claude --permission-mode auto
```

### 管道处理

Claude Code 可以接收管道输入：

```bash
# 让 Claude 解释一个文件
cat src/utils.ts | claude -p "解释这段代码"

# 分析 git diff
git diff | claude -p "总结一下这次改动"

# 检查日志
cat error.log | claude -p "分析错误原因并给出修复建议"

# 审查单个文件
cat src/auth.ts | claude -p "审查这段代码的安全性"
```

### JSON 输出

适合程序化处理结果：

```bash
# 输出 JSON 格式
claude -p "列出项目中所有的 TODO 注释" --output-format json
```

---

## 会话管理

### 恢复会话

```bash
# 恢复最近的会话
claude --resume

# 在交互模式中恢复
/resume
```

### 会话历史

Claude Code 会保存你的会话历史，你可以随时回到之前的对话继续工作。

---

## 实用组合技

### 批量处理文件

```bash
# 给多个文件添加类型注解
for file in src/utils/*.js; do
  claude -p "给这个文件添加 TypeScript 类型注解" < "$file" > "${file%.js}.ts"
done
```

### 与其他工具组合

```bash
# 分析测试覆盖率
npm test -- --coverage 2>&1 | claude -p "分析测试覆盖率报告，指出需要补充测试的地方"

# 代码复杂度分析
npx complexity-report src/ | claude -p "哪些函数复杂度过高？给出简化建议"
```

### 快速代码生成

```bash
# 生成单元测试
claude -p "为 src/utils/date.ts 生成完整的单元测试" > src/utils/date.test.ts

# 生成 API 文档
claude -p "根据 src/api/ 目录下的路由文件生成 API 文档" > docs/api.md
```

---

## 环境变量

一些有用的环境变量：

| 变量 | 说明 | 示例 |
|------|------|------|
| `CLAUDE_MODEL` | 默认模型 | `claude-sonnet-4-6` |
| `ANTHROPIC_API_KEY` | API 密钥 | `sk-ant-...` |

---

## 小贴士

1. **善用管道** — Claude Code 与 Unix 管道完美配合
2. **打印模式用于自动化** — 写脚本时用 `-p` 参数
3. **JSON 输出可编程** — 需要解析结果时用 `--output-format json`
4. **恢复会话省时间** — 用 `--resume` 继续之前的工作

---

## 练习

1. 用管道将一个文件发送给 Claude 分析
2. 写一个小脚本，用 Claude 打印模式批量处理文件
3. 试试用 `--resume` 恢复上次的会话
4. 将 `git log` 的输出传给 Claude，让它总结最近的开发活动

---

## 参考

- [原项目 10-cli](https://github.com/luongnv89/claude-howto/tree/main/10-cli)
- [← 上一模块：检查点](../03-检查点/README.md) | [下一模块：技能 →](../05-技能/README.md)
