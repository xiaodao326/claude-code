# Ralph Wiggum 插件

在 Claude Code 中实现迭代、自引用 AI 开发循环的 Ralph Wiggum 技术。

## 什么是 Ralph？

Ralph 是一种基于持续 AI 代理循环的开发方法论。正如 Geoffrey Huntley 描述的：**"Ralph is a Bash loop"** - 一个简单的 `while true`，反复向 AI 代理提供提示文件，允许它迭代改进其工作直到完成。

此技术以《辛普森一家》中的 Ralph Wiggum 命名，体现了尽管遇到挫折也要坚持迭代的哲学。

### 核心概念

此插件使用 **Stop 钩子**实现 Ralph，拦截 Claude 的退出尝试：

```bash
# 您运行一次：
/ralph-loop "您的任务描述" --completion-promise "DONE"

# 然后 Claude Code 自动：
# 1. 处理任务
# 2. 尝试退出
# 3. Stop 钩子阻止退出
# 4. Stop 钩子再次提供相同的提示
# 5. 重复直到完成
```

循环在 **您当前会话内**发生 - 您不需要外部 bash 循环。`hooks/stop-hook.sh` 中的 Stop 钩子通过阻止正常会话退出创建自引用反馈循环。

这创建了一个 **自引用反馈循环**，其中：
- 提示在迭代之间永不改变
- Claude 的之前工作保留在文件中
- 每次迭代看到修改的文件和 git 历史
- Claude 通过读取自己在文件中的过去工作自主改进

## 快速开始

```bash
/ralph-loop "构建待办事项的 REST API。需求：CRUD 操作、输入验证、测试。完成时输出 <promise>COMPLETE</promise>。" --completion-promise "COMPLETE" --max-iterations 50
```

Claude 将：
- 迭代实现 API
- 运行测试并看到失败
- 基于测试输出修复 bug
- 迭代直到满足所有需求
- 完成时输出完成承诺

## 命令

### /ralph-loop

在您当前会话中启动 Ralph 循环。

**用法：**
```bash
/ralph-loop "<prompt>" --max-iterations <n> --completion-promise "<text>"
```

**选项：**
- `--max-iterations <n>` - N 次迭代后停止（默认：无限制）
- `--completion-promise <text>` - 表示完成的短语

### /cancel-ralph

取消活动的 Ralph 循环。

**用法：**
```bash
/cancel-ralph
```

## 提示编写最佳实践

### 1. 清晰完成标准

❌ 差："构建待办 API 并让它好用。"

✅ 好：
```markdown
构建待办事项的 REST API。

完成时：
- 所有 CRUD 端点工作
- 输入验证到位
- 测试通过（覆盖率 > 80%）
- README 包含 API 文档
- 输出：<promise>COMPLETE</promise>
```

### 2. 增量目标

❌ 差："创建完整的电商平台。"

✅ 好：
```markdown
阶段 1：用户认证（JWT、测试）
阶段 2：产品目录（列表/搜索、测试）
阶段 3：购物车（添加/删除、测试）

所有阶段完成时输出 <promise>COMPLETE</promise>。
```

### 3. 自纠正

❌ 差："为功能 X 编写代码。"

✅ 好：
```markdown
按 TDD 实现功能 X：
1. 编写失败测试
2. 实现功能
3. 运行测试
4. 如有任何失败，调试并修复
5. 如需要重构
6. 重复直到全绿
7. 输出：<promise>COMPLETE</promise>
```

### 4. 逃逸舱

始终使用 `--max-iterations` 作为安全网，防止不可能任务上的无限循环：

```bash
# 推荐：始终设置合理的迭代限制
/ralph-loop "尝试实现功能 X" --max-iterations 20

# 在您的提示中，包含如果卡住该怎么办：
# "15 次迭代后，如果未完成：
#  - 文档化什么阻碍进展
#  - 列出尝试了什么
#  - 建议替代方法"
```

**注意**：`--completion-promise` 使用精确字符串匹配，因此您不能用它用于多个完成条件（如 "SUCCESS" vs "BLOCKED"）。始终依赖 `--max-iterations` 作为您的主要安全机制。

## 理念

Ralph 体现几个关键原则：

### 1. 迭代 > 完美
不要期望第一次就完美。让循环细化工作。

### 2. 失败是数据
"确定性差"意味着失败是可预测和信息丰富的。使用它们来调整提示。

### 3. 操作者技能重要
成功取决于编写好提示，不仅仅是有好模型。

### 4. 坚持获胜
继续尝试直到成功。循环自动处理重试逻辑。

## 何时使用 Ralph

**适用于：**
- 带有清晰成功标准的定义良好任务
- 需要迭代和细化的任务（如让测试通过）
- 您可以走开的绿地项目
- 带有自动验证的任务（测试、linter）

**不适用于：**
- 需要人类判断或设计决策的任务
- 一次性操作
- 成功标准不清楚的任务
- 生产调试（使用针对性调试代替）

## 真实世界结果

- 在 Y Combinator 黑客马拉松测试中一夜成功生成 6 个仓库
- 一个 $50k 合同以 $297 API 成本完成
- 使用此方法在 3 个月内创建了整个编程语言（"cursed"）

## 了解更多

- 原始技术：https://ghuntley.com/ralph/
- Ralph Orchestrator：https://github.com/mikeyobrien/ralph-orchestrator

## 获取帮助

在 Claude Code 中运行 `/help` 获取详细命令参考和示例。