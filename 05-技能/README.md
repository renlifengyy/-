# 模块 5：技能（Skills）

> 预计学习时间：1 小时
> 难度：中级
> 前置知识：模块 1（斜杠命令）、模块 2（记忆系统）

---

## 什么是技能（Skills）？

技能是一种**自动触发**的可复用能力。与斜杠命令不同，你不需要手动输入命令——当 Claude 检测到特定的关键词或场景时，会自动激活对应的技能。

### 技能 vs 斜杠命令

| 特性 | 斜杠命令 | 技能 |
|------|----------|------|
| 触发方式 | 手动输入 `/命令名` | 自动检测触发 |
| 配置 | 纯 Markdown | Markdown + YAML frontmatter |
| 使用场景 | 主动调用 | 被动自动匹配 |
| 存放位置 | `.claude/commands/` | `.claude/skills/` 或 `~/.claude/skills/` |

---

## 内置技能

Claude Code 自带 5 个内置技能：

| 技能 | 触发条件 | 功能 |
|------|----------|------|
| `/simplify` | 代码修改后 | 审查已修改的代码，精简优化 |
| `/batch` | 需要批处理 | 在多个文件上执行相同操作 |
| `/debug` | 调试场景 | 系统化的调试工作流 |
| `/loop` | 周期性任务 | 按设定间隔重复执行 |
| `/claude-api` | 使用 Anthropic SDK | 引导正确使用 Claude API |

---

## 创建自定义技能

### 技能文件结构

技能文件是带有 YAML frontmatter 的 Markdown 文件：

```markdown
---
name: code-review
description: 全面的代码审查
triggers:
  - "review"
  - "审查"
  - "代码审查"
---

对当前代码进行全面审查，检查以下方面：

1. **代码质量**：命名、结构、可读性
2. **潜在 Bug**：空指针、边界条件、类型错误
3. **安全性**：注入风险、敏感数据暴露
4. **性能**：不必要的循环、重复计算
5. **测试覆盖**：是否有对应的测试

输出格式：
- 🔴 严重问题（必须修复）
- 🟡 建议改进（推荐修复）
- 🟢 良好实践（值得肯定）
```

### 存放位置

```
项目级：.claude/skills/技能名/skill.md
用户级：~/.claude/skills/技能名/skill.md
```

### 实战示例 1：品牌风格检查

`.claude/skills/brand-voice/skill.md`：

```markdown
---
name: brand-voice
description: 检查文案是否符合品牌风格
triggers:
  - "品牌"
  - "文案"
  - "风格检查"
---

检查提供的文案内容，确保符合以下品牌标准：

## 语气
- 专业但不刻板
- 友好但不随意
- 简洁有力

## 用词规范
- 使用"我们"而不是"本公司"
- 避免使用行业黑话
- 数字用阿拉伯数字

## 格式
- 段落不超过 3 行
- 要有明确的行动号召（CTA）
```

### 实战示例 2：文档生成器

`.claude/skills/doc-generator/skill.md`：

```markdown
---
name: doc-generator
description: 自动生成 API 文档
triggers:
  - "生成文档"
  - "API 文档"
  - "generate docs"
---

分析指定的代码文件，自动生成文档：

1. 提取所有公开的函数/方法/接口
2. 为每个生成：
   - 功能描述
   - 参数说明（类型、是否必需、默认值）
   - 返回值说明
   - 使用示例
3. 生成 Markdown 格式的文档
4. 包含目录索引
```

### 实战示例 3：带辅助文件的技能

技能可以包含多个文件：

```
.claude/skills/code-review/
├── skill.md           # 主技能文件
├── checklist.md       # 审查清单
└── examples.md        # 好/坏代码示例
```

在 `skill.md` 中引用：
```markdown
参考 @checklist.md 中的审查清单和 @examples.md 中的代码示例。
```

---

## YAML Frontmatter 配置

```yaml
---
name: skill-name           # 技能名称
description: 描述文字       # 简短描述
triggers:                   # 触发关键词列表
  - "关键词1"
  - "关键词2"
---
```

---

## 最佳实践

1. **触发词要具体** — 太泛的触发词会导致误触发
2. **一个技能做一件事** — 保持单一职责
3. **提供清晰的输出格式** — 让结果结构化、易于阅读
4. **项目级技能放 git** — 团队共享，保持一致
5. **先用斜杠命令验证** — 确认效果后再改为自动触发的技能

---

## 练习

1. 创建一个代码审查技能，放在 `.claude/skills/` 中
2. 试用内置的 `/simplify` 技能，修改一段代码后触发
3. 创建一个带辅助文件的技能（如代码审查清单）

---

## 参考

- [原项目 03-skills](https://github.com/luongnv89/claude-howto/tree/main/03-skills)
- [← 上一模块：CLI 基础](../04-CLI基础/README.md) | [下一模块：钩子 →](../06-钩子/README.md)
