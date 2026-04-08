# 插件开发工具包

一个用于开发 Claude Code 插件的综合工具包，提供关于钩子、MCP 集成、插件结构和市场发布的专家指导。

## 概述

plugin-dev 工具包提供七个专业技能帮助您构建高质量 Claude Code 插件：

1. **钩子开发** - 高级钩子 API 和事件驱动自动化
2. **MCP 集成** - Model Context Protocol 服务器集成
3. **插件结构** - 插件组织和 manifest 配置
4. **插件设置** - 使用 .claude/plugin-name.local.md 文件的配置模式
5. **命令开发** - 创建带有 frontmatter 和参数的斜杠命令
6. **代理开发** - 创建带有 AI 辅助生成的自主代理
7. **技能开发** - 创建带有渐进式披露和强触发的技能

每个技能遵循最佳实践和渐进式披露：精益核心文档、详细参考、工作示例和实用脚本。

## 引导工作流程命令

### /plugin-dev:create-plugin

一个全面的、端到端工作流程命令，用于从零创建插件，类似 feature-dev 工作流程。

**8 阶段过程：**
1. **发现** - 理解插件目的和需求
2. **组件规划** - 确定需要的技能、命令、代理、钩子、MCP
3. **详细设计** - 指定每个组件并解决模糊之处
4. **结构创建** - 设置目录和 manifest
5. **组件实现** - 使用 AI 辅助代理创建每个组件
6. **验证** - 运行 plugin-validator 和组件特定检查
7. **测试** - 验证插件在 Claude Code 中工作
8. **文档** - 最终化 README 并准备分发

**功能：**
- 每阶段询问澄清问题
- 自动加载相关技能
- 使用 agent-creator 进行 AI 辅助代理生成
- 运行验证工具（validate-agent.sh、validate-hook-schema.sh 等）
- 遵循 plugin-dev 自己的验证模式
- 引导测试和验证

**用法：**
```bash
/plugin-dev:create-plugin [可选描述]

# 示例：
/plugin-dev:create-plugin
/plugin-dev:create-plugin 一个用于管理数据库迁移的插件
```

使用此工作流程进行结构化、高质量插件开发，从概念到完成。

## 技能

### 1. 钩子开发

**触发短语：** "create a hook"、"add a PreToolUse hook"、"validate tool use"、"implement prompt-based hooks"、"${CLAUDE_PLUGIN_ROOT}"、"block dangerous commands"

**涵盖内容：**
- 带有 LLM 决策的提示式钩子（推荐）
- 用于确定性验证的命令钩子
- 所有钩子事件：PreToolUse、PostToolUse、Stop、SubagentStop、SessionStart、SessionEnd、UserPromptSubmit、PreCompact、Notification
- 钩子输出格式和 JSON schemas
- 安全最佳实践和输入验证
- ${CLAUDE_PLUGIN_ROOT} 用于可移植路径

**资源：**
- 核心 SKILL.md（1,619 字）
- 3 个示例钩子脚本（validate-write、validate-bash、load-context）
- 3 个参考文档：patterns、migration、advanced techniques
- 3 个实用脚本：validate-hook-schema.sh、test-hook.sh、hook-linter.sh

**何时使用：** 创建事件驱动自动化、验证操作或在插件中执行策略。

### 2. MCP 集成

**触发短语：** "add MCP server"、"integrate MCP"、"configure .mcp.json"、"Model Context Protocol"、"stdio/SSE/HTTP server"、"connect external service"

**涵盖内容：**
- MCP 服务器配置（.mcp.json vs plugin.json）
- 所有服务器类型：stdio（本地）、SSE（托管/OAuth）、HTTP（REST）、WebSocket（实时）
- 环境变量展开（${CLAUDE_PLUGIN_ROOT}、用户变量）
- MCP 工具命名和在命令/代理中使用
- 认证模式：OAuth、令牌、环境变量
- 集成模式和性能优化

**资源：**
- 核心 SKILL.md（1,666 字）
- 3 个示例配置（stdio、SSE、HTTP）
- 3 个参考文档：server-types（~3,200w）、authentication（~2,800w）、tool-usage（~2,600w）

**何时使用：** 将外部服务、API、数据库或工具集成到插件中。

### 3. 插件结构

**触发短语：** "plugin structure"、"plugin.json manifest"、"auto-discovery"、"component organization"、"plugin directory layout"

**涵盖内容：**
- 标准插件目录结构和自动发现
- plugin.json manifest 格式和所有字段
- 组件组织（commands、agents、skills、hooks）
- ${CLAUDE_PLUGIN_ROOT} 全面使用
- 文件命名约定和最佳实践
- 最小、标准和高级插件模式

**资源：**
- 核心 SKILL.md（1,619 字）
- 3 个示例结构（minimal、standard、advanced）
- 2 个参考文档：component-patterns、manifest-reference

**何时使用：** 开始新插件、组织组件或配置插件 manifest。

### 4. 插件设置

**触发短语：** "plugin settings"、"store plugin configuration"、".local.md files"、"plugin state files"、"read YAML frontmatter"、"per-project plugin settings"

**涵盖内容：**
- 用于配置的 .claude/plugin-name.local.md 模式
- YAML frontmatter + markdown body 结构
- bash 脚本的解析技术（sed、awk、grep 模式）
- 临时活动钩子（标志文件和快速退出）
- 来自 multi-agent-swarm 和 ralph-wiggum 插件的实际示例
- 原子文件更新和验证
- Gitignore 和生命周期管理

**资源：**
- 核心 SKILL.md（1,623 字）
- 3 个示例（read-settings hook、create-settings command、templates）
- 2 个参考文档：parsing-techniques、real-world-examples
- 2 个实用脚本：validate-settings.sh、parse-frontmatter.sh

**何时使用：** 使插件可配置、存储项目状态或实现用户偏好。

### 5. 命令开发

**触发短语：** "create a slash command"、"add a command"、"command frontmatter"、"define command arguments"、"organize commands"

**涵盖内容：**
- 斜杠命令结构和 markdown 格式
- YAML frontmatter 字段（description、argument-hint、allowed-tools）
- 动态参数和文件引用
- 用于上下文的 Bash 执行
- 命令组织和命名空间
- 命令开发最佳实践

**资源：**
- 核心 SKILL.md（1,535 字）
- 示例和参考文档
- 命令组织模式

**何时使用：** 创建斜杠命令、定义命令参数或组织插件命令。

### 6. 代理开发

**触发短语：** "create an agent"、"add an agent"、"write a subagent"、"agent frontmatter"、"when to use description"、"agent examples"、"autonomous agent"

**涵盖内容：**
- 代理文件结构（YAML frontmatter + system prompt）
- 所有 frontmatter 字段（name、description、model、color、tools）
- 带有 <example> 块用于可靠触发的描述格式
- System prompt 设计模式（analysis、generation、validation、orchestration）
- 使用 Claude Code 验证提示的 AI 辅助代理生成
- 验证规则和最佳实践
- 完整的生产就绪代理示例

**资源：**
- 核心 SKILL.md（1,438 字）
- 2 个示例：agent-creation-prompt（AI 辅助工作流程）、complete-agent-examples（4 个完整代理）
- 3 个参考文档：agent-creation-system-prompt（来自 Claude Code）、system-prompt-design（~4,000w）、triggering-examples（~2,500w）
- 1 个实用脚本：validate-agent.sh

**何时使用：** 创建自主代理、定义代理行为或实现 AI 辅助代理生成。

### 7. 技能开发

**触发短语：** "create a skill"、"add a skill to plugin"、"write a new skill"、"improve skill description"、"organize skill content"

**涵盖内容：**
- 技能结构（带有 YAML frontmatter 的 SKILL.md）
- 渐进式披露原则（metadata → SKILL.md → resources）
- 带有特定短语的强触发描述
- 写作风格（祈使/不定式形式、第三人称）
- bundled resources 组织（references/、examples/、scripts/）
- 技能创建工作流程
- 基于 skill-creator 方法论，适配 Claude Code 插件

**资源：**
- 核心 SKILL.md（1,232 字）
- 参考：skill-creator 方法论、plugin-dev 模式
- 示例：学习 plugin-dev 自己的技能作为模板

**何时使用：** 为插件创建新技能或改进现有技能质量。

## 安装

从 claude-code-marketplace 安装：

```bash
/plugin install plugin-dev@claude-code-marketplace
```

或用于开发，直接使用：

```bash
cc --plugin-dir /path/to/plugin-dev
```

## 快速开始

### 创建您的第一个插件

1. **规划插件结构：**
   - 问："有命令和 MCP 集成的插件最佳目录结构是什么？"
   - plugin-structure 技能将引导您

2. **添加 MCP 集成（如需要）：**
   - 问："如何为数据库访问添加 MCP 服务器？"
   - mcp-integration 技能提供示例和模式

3. **实现钩子（如需要）：**
   - 问："创建一个验证文件写入的 PreToolUse 钩子"
   - hook-development 技能提供工作示例和工具

## 开发工作流程

plugin-dev 工具包支持您整个插件开发生命周期：

```
┌─────────────────────┐
│  设计结构           │  → plugin-structure 技能
│  (manifest, layout) │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  添加组件           │
│  (commands, agents, │  → 所有技能提供指导
│   skills, hooks)    │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  集成服务           │  → mcp-integration 技能
│  (MCP servers)      │
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  添加自动化         │  → hook-development 技能
│  (hooks, validation)│     + 实用脚本
└──────────┬──────────┘
           │
┌──────────▼──────────┐
│  测试与验证         │  → hook-development 工具
│                     │     validate-hook-schema.sh
└──────────┬──────────┘     test-hook.sh
           │                 hook-linter.sh
```

## 功能

### 渐进式披露

每个技能使用三层披露系统：
1. **元数据**（始终加载）：带有强触发的简洁描述
2. **核心 SKILL.md**（触发时）：必要 API 参考（~1,500-2,000 字）
3. **References/Examples**（按需）：详细指南、模式和可工作代码

这保持 Claude Code 上下文聚焦，同时按需提供深层知识。

### 实用脚本

hook-development 技能包含生产就绪工具：

```bash
# 验证 hooks.json 结构
./validate-hook-schema.sh hooks/hooks.json

# 部署前测试钩子
./test-hook.sh my-hook.sh test-input.json

# 为最佳实践 lint 钩子脚本
./hook-linter.sh my-hook.sh
```

### 工作示例

每个技能提供工作示例：
- **钩子开发**：3 个完整钩子脚本（bash、write validation、context loading）
- **MCP 集成**：3 个服务器配置（stdio、SSE、HTTP）
- **插件结构**：3 个插件布局（minimal、standard、advanced）
- **插件设置**：3 个示例（read-settings hook、create-settings command、templates）
- **命令开发**：10 个完整命令示例（review、test、deploy、docs 等）

## 文档标准

所有技能遵循一致标准：
- 第三人称描述（"This skill should be used when..."）
- 用于可靠加载的强触发短语
- 全文使用祈使/不定式形式
- 基于官方 Claude Code 文档
- 安全优先方法与最佳实践

## 总内容

- **核心技能**：~11,065 字跨 7 个 SKILL.md 文件
- **参考文档**：~10,000+ 字详细指南
- **示例**：12+ 工作示例（hook scripts、MCP configs、plugin layouts、settings files）
- **实用工具**：6 个生产就绪验证/测试/解析脚本

## 使用案例

### 构建数据库插件

```
1. "有 MCP 集成的插件结构是什么？"
   → plugin-structure 技能提供布局

2. "如何为 PostgreSQL 配置 stdio MCP 服务器？"
   → mcp-integration 技能展示配置

3. "添加一个 Stop 钩子确保连接正确关闭"
   → hook-development 技能提供模式
```

### 创建验证插件

```
1. "创建验证所有文件写入安全的钩子"
   → hook-development 技能带示例

2. "部署前测试我的钩子"
   → 使用 validate-hook-schema.sh 和 test-hook.sh

3. "组织我的钩子和配置文件"
   → plugin-structure 技能展示最佳实践
```

### 集成外部服务

```
1. "添加带 OAuth 的 Asana MCP 服务器"
   → mcp-integration 技能覆盖 SSE servers

2. "在我的命令中使用 Asana 工具"
   → mcp-integration tool-usage 参考

3. "组织带有命令和 MCP 的插件"
   → plugin-structure 技能提供模式
```

## 最佳实践

所有技能强调：

✅ **安全优先**
- 钩子中的输入验证
- MCP 服务器使用 HTTPS/WSS
- 凭证使用环境变量
- 最小权限原则

✅ **可移植性**
- 全程使用 ${CLAUDE_PLUGIN_ROOT}
- 仅相对路径
- 环境变量替换

✅ **测试**
- 部署前验证配置
- 用样本输入测试钩子
- 使用 debug 模式（`claude --debug`）

✅ **文档**
- 清晰 README 文件
- 文档化的环境变量
- 使用示例

## 贡献

此插件是 claude-code-marketplace 的一部分。要贡献改进：

1. Fork marketplace 仓库
2. 对 plugin-dev/ 进行更改
3. 用 `cc --plugin-dir` 本地测试
4. 按照 marketplace-publishing 指导创建 PR

## 版本

0.1.0 - 初始发布，包含七个综合技能和三个验证代理

## 作者

Daisy Hollman (daisy@anthropic.com)

## 许可证

MIT License - 详见仓库

---

**注意：** 此工具包设计用于帮助您构建高质量插件。当您询问相关问题时，技能自动加载，在您需要时提供专家指导。