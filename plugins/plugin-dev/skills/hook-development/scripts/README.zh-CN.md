# 钩子开发实用脚本

这些脚本帮助在部署前验证、测试和 lint 钩子实现。

## validate-hook-schema.sh

验证 `hooks.json` 配置文件的正确结构和常见问题。

**用法：**
```bash
./validate-hook-schema.sh path/to/hooks.json
```

**检查：**
- 有效 JSON 语法
- 必需字段存在
- 有效钩子事件名称
- 正确钩子类型（command/prompt）
- 有效范围内的超时值
- 硬编码路径检测
- Prompt 钩子事件兼容性

**示例：**
```bash
cd my-plugin
./validate-hook-schema.sh hooks/hooks.json
```

## test-hook.sh

在部署到 Claude Code 前用样本输入测试单个钩子脚本。

**用法：**
```bash
./test-hook.sh [options] <hook-script> <test-input.json>
```

**选项：**
- `-v, --verbose` - 显示详细执行信息
- `-t, --timeout N` - 设置超时秒数（默认：60）
- `--create-sample <event-type>` - 生成样本测试输入

**示例：**
```bash
# 创建样本测试输入
./test-hook.sh --create-sample PreToolUse > test-input.json

# 测试钩子脚本
./test-hook.sh my-hook.sh test-input.json

# 带详细输出和自定义超时测试
./test-hook.sh -v -t 30 my-hook.sh test-input.json
```

**功能：**
- 设置适当环境变量（CLAUDE_PROJECT_DIR、CLAUDE_PLUGIN_ROOT）
- 测量执行时间
- 验证输出 JSON
- 显示退出代码及其含义
- 捕获环境文件输出

## hook-linter.sh

检查钩子脚本中的常见问题和最佳实践违规。

**用法：**
```bash
./hook-linter.sh <hook-script.sh> [hook-script2.sh ...]
```

**检查：**
- Shebang 存在
- `set -euo pipefail` 使用
- Stdin 输入读取
- 正确错误处理
- 变量引用（注入预防）
- 退出代码使用
- 硬编码路径
- 长运行代码检测
- 错误输出到 stderr
- 输入验证

**示例：**
```bash
# Lint 单个脚本
./hook-linter.sh ../examples/validate-write.sh

# Lint 多个脚本
./hook-linter.sh ../examples/*.sh
```

## 典型工作流程

1. **编写钩子脚本**
   ```bash
   vim my-plugin/scripts/my-hook.sh
   ```

2. **Lint 脚本**
   ```bash
   ./hook-linter.sh my-plugin/scripts/my-hook.sh
   ```

3. **创建测试输入**
   ```bash
   ./test-hook.sh --create-sample PreToolUse > test-input.json
   # 按需编辑 test-input.json
   ```

4. **测试钩子**
   ```bash
   ./test-hook.sh -v my-plugin/scripts/my-hook.sh test-input.json
   ```

5. **添加到 hooks.json**
   ```bash
   # 编辑 my-plugin/hooks/hooks.json
   ```

6. **验证配置**
   ```bash
   ./validate-hook-schema.sh my-plugin/hooks/hooks.json
   ```

7. **在 Claude Code 中测试**
   ```bash
   claude --debug
   ```

## 提示

- 部署前始终测试钩子以避免破坏用户工作流程
- 使用详细模式（`-v`）调试钩子行为
- 检查 linter 输出中的安全和最佳实践问题
- 任何更改后验证 hooks.json
- 为各种场景创建不同测试输入（安全操作、危险操作、边缘情况）

## 常见问题

### 钩子不执行

检查：
- 脚本有 shebang（`#!/bin/bash`）
- 脚本可执行（`chmod +x`）
- hooks.json 中路径正确（使用 `${CLAUDE_PLUGIN_ROOT}`）

### 钩子超时

- 在 hooks.json 中减少超时
- 优化钩子脚本性能
- 移除长运行操作

### 钩子静默失败

- 检查退出代码（应为 0 或 2）
- 确保错误到 stderr（`>&2`）
- 验证 JSON 输出结构

### 注入漏洞

- 始终引用变量：`"$variable"`
- 使用 `set -euo pipefail`
- 验证所有输入字段
- 运行 linter 捕获问题