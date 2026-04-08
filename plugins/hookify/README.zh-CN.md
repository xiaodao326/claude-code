# Hookify 插件

轻松创建自定义钩子，通过分析对话模式或显式指令来防止不良行为。

## 概述

hookify 插件使创建钩子变得简单，无需编辑复杂的 `hooks.json` 文件。相反，您创建轻量级 markdown 配置文件，定义要监视的模式和匹配时显示的消息。

**关键功能：**
- 🎯 自动分析对话以找到不良行为
- 📝 带有 YAML frontmatter 的简单 markdown 配置文件
- 🔍 用于强大规则的 Regex 模式匹配
- 🚀 不需要编码 - 只需描述行为
- 🔄 无需重启即可轻松启用/禁用

## 快速开始

### 1. 创建您的第一个规则

```bash
/hookify 当我使用 rm -rf 命令时警告我
```

这分析您的请求并创建 `.claude/hookify.warn-rm.local.md`。

### 2. 立即测试

**无需重启！** 规则在下一次工具使用时立即生效。

请求 Claude 运行应该触发规则的命令：
```
运行 rm -rf /tmp/test
```

您应该立即看到警告消息！

## 用法

### 主命令：/hookify

**带参数：**
```
/hookify 不要在 TypeScript 文件中使用 console.log
```
从您的显式指令创建规则。

**不带参数：**
```
/hookify
```
分析最近对话以找到您纠正过或感到沮丧的行为。

### 辅助命令

**列出所有规则：**
```
/hookify:list
```

**交互式配置规则：**
```
/hookify:configure
```
通过交互界面启用/禁用现有规则。

**获取帮助：**
```
/hookify:help
```

## 规则配置格式

### 简单规则（单一模式）

`.claude/hookify.dangerous-rm.local.md`：
```markdown
---
name: block-dangerous-rm
enabled: true
event: bash
pattern: rm\s+-rf
action: block
---

⚠️ **检测到危险的 rm 命令！**

此命令可能删除重要文件。请：
- 验证路径正确
- 考虑使用更安全的方法
- 确保您有备份
```

**Action 字段：**
- `warn`：显示警告但允许操作（默认）
- `block`：阻止操作执行（PreToolUse）或停止会话（Stop 事件）

### 高级规则（多条件）

`.claude/hookify.sensitive-files.local.md`：
```markdown
---
name: warn-sensitive-files
enabled: true
event: file
action: warn
conditions:
  - field: file_path
    operator: regex_match
    pattern: \.env$|credentials|secrets
  - field: new_text
    operator: contains
    pattern: KEY
---

🔐 **检测到敏感文件编辑！**

确保凭证未硬编码且文件在 .gitignore 中。
```

**所有条件必须匹配** 规则才会触发。

## 事件类型

- **`bash`**：在 Bash 工具命令时触发
- **`file`**：在 Edit、Write、MultiEdit 工具时触发
- **`stop`**：在 Claude 想停止时触发（用于完成检查）
- **`prompt`**：在用户提交提示时触发
- **`all`**：在所有事件时触发

## 模式语法

使用 Python regex 语法：

| 模式 | 匹配 | 示例 |
|------|------|------|
| `rm\s+-rf` | rm -rf | rm -rf /tmp |
| `console\.log\(` | console.log( | console.log("test") |
| `(eval\|exec)\(` | eval( 或 exec( | eval("code") |
| `\.env$` | 以 .env 结尾的文件 | .env, .env.local |
| `chmod\s+777` | chmod 777 | chmod 777 file.txt |

**提示：**
- 使用 `\s` 表示空白
- 转义特殊字符：`\.` 表示字面点
- 使用 `|` 表示 OR：`(foo|bar)`
- 使用 `.*` 匹配任何内容
- 为危险操作设置 `action: block`
- 为信息性警告设置 `action: warn`（或省略）

## 示例

### 示例 1：阻止危险命令

```markdown
---
name: block-destructive-ops
enabled: true
event: bash
pattern: rm\s+-rf|dd\s+if=|mkfs|format
action: block
---

🛑 **检测到破坏性操作！**

此命令可能导致数据丢失。为安全阻止操作。
请验证确切路径并使用更安全的方法。
```

**此规则阻止操作** - Claude 将不允许执行这些命令。

### 示例 2：警告调试代码

```markdown
---
name: warn-debug-code
enabled: true
event: file
pattern: console\.log\(|debugger;|print\(
action: warn
---

🐛 **检测到调试代码**

提交前记得删除调试语句。
```

**此规则警告但允许** - Claude 看到消息但仍可继续。

### 示例 3：停止前要求运行测试

```markdown
---
name: require-tests-run
enabled: false
event: stop
action: block
conditions:
  - field: transcript
    operator: not_contains
    pattern: npm test|pytest|cargo test
---

**会话记录中未检测到测试！**

停止前，请运行测试以验证您的变更正确工作。
```

**此规则阻止 Claude 停止** 如果会话记录中没有测试命令。仅在您想严格执行时启用。

## 高级用法

### 多条件

同时检查多个字段：

```markdown
---
name: api-key-in-typescript
enabled: true
event: file
conditions:
  - field: file_path
    operator: regex_match
    pattern: \.tsx?$
  - field: new_text
    operator: regex_match
    pattern: (API_KEY|SECRET|TOKEN)\s*=\s*["']
---

🔐 **TypeScript 中硬编码凭证！**

使用环境变量代替硬编码值。
```

### 操作符参考

- `regex_match`：模式必须匹配（最常用）
- `contains`：字符串必须包含模式
- `equals`：精确字符串匹配
- `not_contains`：字符串必须不包含模式
- `starts_with`：字符串以模式开始
- `ends_with`：字符串以模式结束

### 字段参考

**对于 bash 事件：**
- `command`：bash 命令字符串

**对于 file 事件：**
- `file_path`：正在编辑的文件路径
- `new_text`：正在添加的新内容（Edit、Write）
- `old_text`：正在替换的旧内容（仅 Edit）
- `content`：文件内容（仅 Write）

**对于 prompt 事件：**
- `user_prompt`：用户提交的提示文本

**对于 stop 事件：**
- 在会话状态上使用通用匹配

## 管理

### 启用/禁用规则

**临时禁用：**
编辑 `.local.md` 文件并设置 `enabled: false`

**重新启用：**
设置 `enabled: true`

**或使用交互工具：**
```
/hookify:configure
```

### 删除规则

简单地删除 `.local.md` 文件：
```bash
rm .claude/hookify.my-rule.local.md
```

### 查看所有规则

```
/hookify:list
```

## 安装

此插件是 Claude Code Marketplace 的一部分。安装 marketplace 时应自动发现。

**手动测试：**
```bash
cc --plugin-dir /path/to/hookify
```

## 要求

- Python 3.7+
- 无外部依赖（仅使用 stdlib）

## 故障排除

**规则未触发：**
1. 检查规则文件存在于 `.claude/` 目录（项目根目录，而非插件目录）
2. 验证 frontmatter 中 `enabled: true`
3. 单独测试 regex 模式
4. 规则应立即生效 - 无需重启
5. 尝试 `/hookify:list` 查看规则是否加载

**导入错误：**
- 确保 Python 3 可用：`python3 --version`
- 检查 hookify 插件已安装

**模式不匹配：**
- 测试 regex：`python3 -c "import re; print(re.search(r'pattern', 'text'))"`
- 在 YAML 中使用未引用模式以避免转义问题
- 从简单开始，然后添加复杂性

**钩子似乎慢：**
- 保持模式简单（避免复杂 regex）
- 使用特定事件类型（bash、file）而非 "all"
- 限制活动规则数量

## 贡献

发现了有用的规则模式？考虑通过 PR 分享示例文件！

## 未来增强

- 严重性级别（错误/警告/信息区分）
- 规则模板库
- 交互模式构建器
- 钩子测试工具
- JSON 格式支持（除 markdown 外）

## 许可证

MIT License