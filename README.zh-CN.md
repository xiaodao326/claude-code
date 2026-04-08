# Claude Code

![](https://img.shields.io/badge/Node.js-18%2B-brightgreen?style=flat-square) [![npm]](https://www.npmjs.com/package/@anthropic-ai/claude-code)

[npm]: https://img.shields.io/npm/v/@anthropic-ai/claude-code.svg?style=flat-square

Claude Code 是一款智能编码工具，运行在终端中，能够理解您的代码库，并通过自然语言命令帮助您更快地编码——执行日常任务、解释复杂代码、处理 git 工作流程。可在终端、IDE 中使用，或在 Github 上 @claude。

**了解更多信息，请参阅[官方文档](https://code.claude.com/docs/en/overview)**。

<img src="./demo.gif" />

## 快速开始

> [!NOTE]
> 通过 npm 安装已被弃用。请使用下面推荐的方法之一。

如需更多安装选项、卸载步骤和故障排除，请参阅[安装文档](https://code.claude.com/docs/en/setup)。

1. 安装 Claude Code：

    **MacOS/Linux（推荐）：**
    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```

    **Homebrew（MacOS/Linux）：**
    ```bash
    brew install --cask claude-code
    ```

    **Windows（推荐）：**
    ```powershell
    irm https://claude.ai/install.ps1 | iex
    ```

    **WinGet（Windows）：**
    ```powershell
    winget install Anthropic.ClaudeCode
    ```

    **NPM（已弃用）：**
    ```bash
    npm install -g @anthropic-ai/claude-code
    ```

2. 进入您的项目目录并运行 `claude`。

## 插件

本仓库包含多个 Claude Code 插件，通过自定义命令和代理扩展功能。有关可用插件的详细文档，请参阅 [plugins 目录](./plugins/README.md)。

## 报告 Bug

我们欢迎您的反馈。使用 `/bug` 命令在 Claude Code 中直接报告问题，或提交 [GitHub issue](https://github.com/anthropics/claude-code/issues)。

## 加入 Discord

加入 [Claude 开发者 Discord](https://anthropic.com/discord)，与其他使用 Claude Code 的开发者交流。获取帮助、分享反馈，并与社区讨论您的项目。

## 数据收集、使用和保留

当您使用 Claude Code 时，我们会收集反馈，其中包括使用数据（如代码接受或拒绝）、相关的对话数据，以及通过 `/bug` 命令提交的用户反馈。

### 我们如何使用您的数据

请参阅我们的[数据使用政策](https://code.claude.com/docs/en/data-usage)。

### 隐私保护措施

我们已实施多项保护措施来保护您的数据，包括对敏感信息的有限保留期、对用户会话数据的受限访问，以及明确禁止将反馈用于模型训练的政策。

欲了解完整详情，请查看我们的[商业服务条款](https://www.anthropic.com/legal/commercial-terms)和[隐私政策](https://www.anthropic.com/legal/privacy)。