# Awesome Geekit Agent

[English](README.md) | [简体中文](README.zh-CN.md)

通过 Geekit 接入编程助手、桌面 Agent 与开源客户端的教程合集，由 YRYQ 维护。

## 开始使用

1. 在 [Geekit](https://geekit.top/keys) 创建 API Key，从[模型广场](https://geekit.top/pricing/models)复制真实模型 ID。
2. 按[首次调用 API](docs/zh/getting-started.md)配置连接参数。
3. 选择下表中的工具，完成配置后通过调用日志检查协议、模型与工具调用。

在线教程会按访问站点自动显示 BaseURL。仓库示例使用 `https://geekit.top`，使用前请按你的环境填写根地址。在线入口见[国内站教程](https://geekit.top/docs/agents/)和[国际站教程](https://intl.geekit.top/docs/agents/)。

## 工具目录

| 工具 | 接入内容 |
| --- | --- |
| [DeepSeek Harness](docs/zh/agents/deepseek-harness.md) | Web UI 自定义模型 API |
| [Claude Code](docs/zh/agents/claude-code.md) | Anthropic Messages |
| [Codex CLI](docs/zh/agents/codex-cli.md) | OpenAI Responses |
| [OpenCode](docs/zh/agents/opencode.md) | 自定义 Provider |
| [OpenClaw](docs/zh/agents/openclaw.md) | 个人 AI 助手 |
| [Hermes Agent](docs/zh/agents/hermes-agent.md) | 多协议 Agent |
| [Reasonix](docs/zh/agents/reasonix.md) | 终端与 Studio |
| [WorkBuddy / CodeBuddy](docs/zh/agents/workbuddy.md) | 桌面端与本地模型配置 |
| [Qoder](docs/zh/agents/qoder.md) | 自定义模型与 BaseURL 接入条件 |
| [CC Switch](docs/zh/agents/cc-switch.md) | 图形化 Provider 管理 |
| [LobsterAI](docs/zh/agents/lobsterai.md) | 桌面 Agent |

Qoder 的公开 BYOK 文档使用供应商目录，接入 Geekit 还需要客户端提供自定义 BaseURL。对应教程记录当前公开字段与接入条件。

## 排错与贡献

- 通过[错误码](docs/zh/error-codes.md)排查鉴权、额度、参数和上游故障。
- 按[贡献指南](CONTRIBUTING.zh-CN.md)补充新工具、版本变化或脱敏验证记录。
- 在 [Issues](https://github.com/yirenyiqi/awesome-geekit-agent/issues)提交复现步骤与客户端版本。

## 资料与许可

教程以客户端官方资料和 Geekit 接口行为为依据，来源链接保留在各页末尾。初始教程整理自 [Geekit 用户文档](https://geekit.top/docs/)，沿用 [AGPL-3.0 许可](LICENSE)。
