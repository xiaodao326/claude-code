# 命令开发技能

创建 Claude Code 斜杠命令的综合指导，包括文件格式、frontmatter 选项、动态参数和最佳实践。

## 概述

此技能提供关于以下的知识：
- 斜杠命令文件格式和结构
- YAML frontmatter 配置字段
- 动态参数（$ARGUMENTS、$1、$2 等）
- 使用 @ 语法的文件引用
- 使用 !` 语法的 Bash 执行
- 命令组织和命名空间
- 命令开发最佳实践
- 插件特定功能（${CLAUDE_PLUGIN_ROOT}、插件模式）
- 与插件组件集成（代理、技能、钩子）
- 验证模式和错误处理

## 技能结构

### SKILL.md（~2,470 字）

核心技能内容涵盖：

**基础：**
- 命令基础和位置
- 文件格式（带可选 frontmatter 的 Markdown）
- YAML frontmatter 字段概述
- 动态参数（$ARGUMENTS 和位置参数）
- 文件引用（@ 语法）
- Bash 执行（!` 语法）
- 命令组织模式
- 最佳实践和常见模式
- 故障排除

**插件特定：**
- ${CLAUDE_PLUGIN_ROOT} 环境变量
- 插件命令发现和组织
- 插件命令模式（配置、模板、多脚本）
- 与插件组件集成（代理、技能、钩子）
- 验证模式（参数、文件、资源、错误处理）

### 参考

详细文档：

- **frontmatter-reference.md**：完整的 YAML frontmatter 字段规范
  - 所有字段描述带类型和默认值
  - 何时使用每个字段
  - 示例和最佳实践
  - 验证和常见错误

- **plugin-features-reference.md**：插件特定命令功能
  - 插件命令发现和组织
  - ${CLAUDE_PLUGIN_ROOT} 环境变量使用
  - 插件命令模式（配置、模板、多脚本）
  - 与插件代理、技能和钩子集成
  - 验证模式和错误处理

### 示例

实用命令示例：

- **simple-commands.md**：10 个完整命令示例
  - 代码审查命令
  - 测试命令
  - 部署命令
  - 文档生成器
  - Git 集成命令
  - 分析和研究命令

- **plugin-commands.md**：10 个插件特定命令示例
  - 带脚本的简单插件命令
  - 多脚本工作流程
  - 模板生成
  - 配置驱动部署
  - 代理和技能集成
  - 多组件工作流程
  - 验证输入命令
  - 环境感知命令

## 此技能触发时机

当用户执行以下操作时 Claude Code 激活此技能：
- 请求 "创建斜杠命令" 或 "添加命令"
- 需要 "编写自定义命令"
- 想要 "定义命令参数"
- 询问 "命令 frontmatter" 或 YAML 配置
- 需要 "组织命令" 或使用命名空间
- 想要创建带文件引用的命令
- 询问 "命令中的 bash 执行"
- 需要命令开发最佳实践

## 渐进式披露

技能使用渐进式披露：

1. **SKILL.md**（~2,470 字）：核心概念、常见模式和插件功能概述
2. **参考**（~13,500 字总计）：详细规范
   - frontmatter-reference.md（~1,200 字）
   - plugin-features-reference.md（~1,800 字）
   - interactive-commands.md（~2,500 字）
   - advanced-workflows.md（~1,700 字）
   - testing-strategies.md（~2,200 字）
   - documentation-patterns.md（~2,000 字）
   - marketplace-considerations.md（~2,200 字）
3. **示例**（~6,000 字总计）：完整工作命令示例
   - simple-commands.md
   - plugin-commands.md

Claude 根据任务按需加载参考和示例。

## 命令基础快速参考

### 文件格式

```markdown
---
description: 简短描述
argument-hint: [arg1] [arg2]
allowed-tools: Read, Bash(git:*)
---

命令提示内容，包含：
- 参数：$1, $2, 或 $ARGUMENTS
- 文件：@path/to/file
- Bash：!`command here`
```

### 位置

- **项目**：`.claude/commands/`（与团队共享）
- **个人**：`~/.claude/commands/`（您的命令）
- **插件**：`plugin-name/commands/`（插件特定）

### 关键功能

**动态参数：**
- `$ARGUMENTS` - 所有参数作为单个字符串
- `$1`、`$2`、`$3` - 位置参数

**文件引用：**
- `@path/to/file` - 包含文件内容

**Bash 执行：**
- `!`command`` - 执行并包含输出

## Frontmatter 字段快速参考

| 字段 | 用途 | 示例 |
|------|------|------|
| `description` | /help 的简短描述 | `"审查代码问题"` |
| `allowed-tools` | 限制工具访问 | `Read, Bash(git:*)` |
| `model` | 指定模型 | `sonnet`、`opus`、`haiku` |
| `argument-hint` | 文档参数 | `[pr-number] [priority]` |
| `disable-model-invocation` | 手动命令 | `true` |

## 常见模式

### 简单审查命令

```markdown
---
description: 审查代码问题
---

审查此代码的质量和潜在 bug。
```

### 带参数的命令

```markdown
---
description: 部署到环境
argument-hint: [environment] [version]
---

部署到 $1 环境，使用版本 $2
```

### 带文件引用的命令

```markdown
---
description: 文档文件
argument-hint: [file-path]
---

为 @$1 生成文档
```

### 带 Bash 执行的命令

```markdown
---
description: 显示 Git 状态
allowed-tools: Bash(git:*)
---

当前状态：!`git status`
最近提交：!`git log --oneline -5`
```

## 开发工作流程

1. **设计命令：**
   - 定义目的和范围
   - 确定所需参数
   - 识别需要的工具

2. **创建文件：**
   - 选择适当位置
   - 创建带命令名的 `.md` 文件
   - 编写基本提示

3. **添加 frontmatter：**
   - 从最小开始（仅描述）
   - 按需添加字段（allowed-tools 等）
   - 用 argument-hint 文档参数

4. **测试命令：**
   - 用 `/command-name` 调用
   - 验证参数工作
   - 检查 bash 执行
   - 测试文件引用

5. **细化：**
   - 改善提示清晰度
   - 处理边缘情况
   - 在注释中添加示例
   - 文档需求

## 最佳实践摘要

1. **单一职责**：一个命令，一个清晰目的
2. **清晰描述**：在 `/help` 中可发现
3. **文档参数**：始终使用 argument-hint
4. **最小工具**：使用最严格的 allowed-tools
5. **彻底测试**：验证所有功能工作
6. **添加注释**：解释复杂逻辑
7. **处理错误**：考虑缺少参数/文件

## 状态

**已完成增强：**
- ✓ 插件命令模式（${CLAUDE_PLUGIN_ROOT}、发现、组织）
- ✓ 集成模式（代理、技能、钩子协调）
- ✓ 验证模式（输入、文件、资源验证、错误处理）

**剩余增强（进行中）：**
- 高级工作流程（多步命令序列）
- 测试策略（如何有效测试命令）
- 文档模式（命令文档最佳实践）
- 市场考虑（发布和分发）

## 维护

要更新此技能：
1. 保持 SKILL.md 聚焦核心基础
2. 将详细规范移至 references/
3. 为不同用例添加新 examples/
4. 添加新字段时更新 frontmatter
5. 全程确保祈使/不定式形式
6. 测试示例与当前 Claude Code 工作

## 版本历史

**v0.1.0**（2025-01-15）：
- 带基本命令基础的初始发布
- Frontmatter 字段参考
- 10 个简单命令示例
- 准备添加插件特定模式