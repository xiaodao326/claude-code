# 插件结构技能

Claude Code 插件架构、目录布局和最佳实践的综合指导。

## 概述

此技能提供关于以下的详细知识：
- 插件目录结构和组织
- `plugin.json` manifest 配置
- 组件组织（commands、agents、skills、hooks）
- 自动发现机制
- 使用 `${CLAUDE_PLUGIN_ROOT}` 的可移植路径引用
- 文件命名约定

## 技能结构

### SKILL.md（1,619 字）

核心技能内容涵盖：
- 目录结构概述
- Plugin manifest（plugin.json）字段
- 组件组织模式
- ${CLAUDE_PLUGIN_ROOT} 使用
- 文件命名约定
- 自动发现机制
- 最佳实践
- 常见模式
- 故障排除

### 参考

深入详细文档：

- **manifest-reference.md**：完整 `plugin.json` 字段参考
  - 所有字段描述和示例
  - 路径解析规则
  - 验证指导
  - 最小 vs 完整 manifest 示例

- **component-patterns.md**：高级组织模式
  - 组件生命周期（发现、激活）
  - 命令组织模式
  - 代理组织模式
  - 技能组织模式
  - 钩子组织模式
  - 脚本组织模式
  - 跨组件模式
  - 可扩展性最佳实践

### 示例

三个完整插件示例：

- **minimal-plugin.md**：最简单的插件
  - 单一命令
  - 最小 manifest
  - 何时使用此模式

- **standard-plugin.md**：结构良好的生产插件
  - 多组件（commands、agents、skills、hooks）
  - 带元数据的完整 manifest
  - 丰富的技能结构
  - 组件间集成

- **advanced-plugin.md**：企业级插件
  - 多级组织
  - MCP 服务器集成
  - 共享库
  - 配置管理
  - 安全自动化
  - 监控集成

## 此技能触发时机

当用户执行以下操作时 Claude Code 激活此技能：
- 请求 "创建插件" 或 "脚手架插件"
- 需要 "理解插件结构"
- 想要 "组织插件组件"
- 需要 "设置 plugin.json"
- 询问 "${CLAUDE_PLUGIN_ROOT}" 使用
- 想要 "添加 commands/agents/skills/hooks"
- 需要 "配置自动发现" 帮助
- 询问插件架构或最佳实践

## 渐进式披露

技能使用渐进式披露管理上下文：

1. **SKILL.md**（~1600 字）：核心概念和工作流程
2. **参考**（~6000 字）：详细字段参考和模式
3. **示例**（~8000 字）：完整工作示例

Claude 根据任务按需加载参考和示例。

## 相关技能

此技能与以下配合良好：
- **hook-development**：用于创建插件钩子
- **mcp-integration**：用于集成 MCP 服务器（可用时）
- **marketplace-publishing**：用于发布插件（可用时）

## 维护

要更新此技能：
1. 保持 SKILL.md 精简并聚焦核心概念
2. 将详细信息移至 references/
3. 为常见模式添加新 examples/
4. 更新 SKILL.md frontmatter 中的版本
5. 确保所有文档使用祈使/不定式形式