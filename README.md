# Awesome Geekit Agent

[English](README.md) | [简体中文](README.zh-CN.md)

Guides for connecting coding assistants, desktop Agents, and open-source clients to Geekit, maintained by YRYQ.

## Get started

1. Create an API key on [Geekit](https://geekit.top/keys) and copy a real model ID from the [model catalog](https://geekit.top/pricing/models).
2. Configure your connection using [First API call](docs/en/getting-started.md).
3. Choose a tool below, configure it, and check the protocol, model, and tool calls in usage logs.

Online guides display the BaseURL for the site you visit. Repository examples use `https://geekit.top`. Set the root address for your environment before use. Open the [CN guides](https://geekit.top/docs/en/agents/) or [international guides](https://intl.geekit.top/docs/en/agents/).

## Tools

| Tool | Integration |
| --- | --- |
| [DeepSeek Harness](docs/en/agents/deepseek-harness.md) | Custom model API in the Web UI |
| [Claude Code](docs/en/agents/claude-code.md) | Anthropic Messages |
| [Codex CLI](docs/en/agents/codex-cli.md) | OpenAI Responses |
| [OpenCode](docs/en/agents/opencode.md) | Custom provider |
| [OpenClaw](docs/en/agents/openclaw.md) | Personal AI assistant |
| [Hermes Agent](docs/en/agents/hermes-agent.md) | Multi-protocol Agent |
| [Reasonix](docs/en/agents/reasonix.md) | Terminal and Studio |
| [WorkBuddy / CodeBuddy](docs/en/agents/workbuddy.md) | Desktop and local model configuration |
| [Qoder](docs/en/agents/qoder.md) | Custom models and BaseURL requirements |
| [CC Switch](docs/en/agents/cc-switch.md) | Graphical provider management |
| [LobsterAI](docs/en/agents/lobsterai.md) | Desktop Agent |

Qoder's public BYOK documentation uses a provider catalog. Connecting to Geekit also requires an editable BaseURL. Its guide records the documented fields and connection requirements.

## Troubleshooting and contributions

- Use [Error codes](docs/en/error-codes.md) to diagnose authentication, quota, parameter, and upstream failures.
- Follow the [contribution guidelines](CONTRIBUTING.md) to add tools, version updates, or redacted verification records.
- Include reproduction steps and the client version in [Issues](https://github.com/yirenyiqi/awesome-geekit-agent/issues).

## Sources and license

Guides follow official client documentation and Geekit API behavior. Each page links its sources. Initial tutorials were adapted from the [Geekit user documentation](https://geekit.top/docs/) under the same [AGPL-3.0 license](LICENSE).
