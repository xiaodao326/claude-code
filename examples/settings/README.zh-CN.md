# 设置示例

Claude Code 设置文件示例，主要用于组织范围的部署。将这些作为起点使用——根据您的需求进行调整。

这些文件可以应用于[设置层级](https://code.claude.com/docs/en/settings#settings-files)的任何级别，不过某些属性仅在企业设置中指定时才生效（例如 `strictKnownMarketplaces`、`allowManagedHooksOnly`、`allowManagedPermissionRulesOnly`）。

## 配置示例

> [!WARNING]
> 这些示例是社区维护的代码片段，可能不被支持或不正确。您对自己设置配置的正确性负责。

| 设置 | [`settings-lax.json`](./settings-lax.json) | [`settings-strict.json`](./settings-strict.json) | [`settings-bash-sandbox.json`](./settings-bash-sandbox.json) |
|------|:---:|:---:|:---:|
| 禁用 `--dangerously-skip-permissions` | ✅ | ✅ | |
| 阻止插件市场 | ✅ | ✅ | |
| 阻止用户和项目定义的权限 `allow`/`ask`/`deny` | | ✅ | ✅ |
| 阻止用户和项目定义的钩子 | | ✅ | |
| 拒绝 Web 获取和搜索工具 | | ✅ | |
| Bash 工具需要审批 | | ✅ | |
| Bash 工具必须在沙箱内运行 | | | ✅ |

## 提示

- 考虑合并上述示例的代码片段以达到您想要的配置
- 设置文件必须是有效的 JSON
- 在将配置文件部署到您的组织之前，请先在本地测试，应用到 `managed-settings.json`、`settings.json` 或 `settings.local.json`
- `sandbox` 属性仅适用于 `Bash` 工具；它不适用于其他工具（如 Read、Write、WebSearch、WebFetch、MCP）、钩子或内部命令

## 完整文档

有关所有可用托管设置的完整文档，请参阅 https://code.claude.com/docs/en/settings。