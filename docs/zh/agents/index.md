# Agent 接入

Geekit 可以作为支持自定义 Provider 的 CLI、桌面 Agent 和开源客户端的统一模型入口。优先推荐 Codex CLI；其他客户端应按其原生协议配置。

资料核对日期为 2026-11-26。

**Geekit 中转接入：适用于无法直连官方模型服务的网络环境**

客户端本体可按对应的官方教程安装或更新；已核验的 Codex CLI 也可使用本站受保护镜像。安装完成后，按本章教程把 Base URL、API Key 和模型 ID 配置为 Geekit；后续模型请求将发送到 `https://geekit.top`，无需直接连接 OpenAI 或 Anthropic API，也不需要把上游密钥交给客户端。

除已核验的 Codex CLI 站内镜像外，其他客户端是否有站内安装包，请以各自页面的交付状态为准。官方安装地址在当前网络不可达时，不要从不明网盘下载可执行文件。

**第一步：先获取并配置连接参数**

- **`<Geekit 地址>`**：填写 `https://geekit.top`。
- **`GEEKIT_API_KEY`**：变量名不要修改，变量值替换为生成的真实 Geekit 令牌，例如 `sk-...`。[生成 API Key](https://geekit.top/keys)。
- **`<Geekit 模型 ID>`**：使用账户当前可用的最新模型；Codex 推荐 `gpt-6-astra`，Claude 推荐 `claude-fable-5`。[查看可用模型](https://geekit.top/pricing)。

将以下内容写入 `~/.zshrc`（Zsh）或 `~/.bashrc`（Bash）：

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<生成的真实 Geekit 令牌>"
export GEEKIT_MODEL="<Geekit 模型 ID>"
```

保存后重新打开终端，或执行 `source ~/.zshrc` / `source ~/.bashrc`。PowerShell 使用用户级永久环境变量：

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<生成的真实 Geekit 令牌>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "<Geekit 模型 ID>", "User")
```

PowerShell 设置后需要关闭并重新打开。完整说明见[快速开始](../getting-started.md)。

## 接入工具

| 客户端 | Geekit 协议 | 配置方式 |
| --- | --- | --- |
| [DeepSeek Harness](deepseek-harness.md) | Chat、Responses、Messages 或 Gemini | Web UI 自定义模型 API |
| [Claude Code](claude-code.md) | Anthropic Messages | 环境变量 |
| [Codex CLI](codex-cli.md) | OpenAI Responses | `~/.codex/config.toml` |
| [OpenCode](opencode.md) | Chat Completions 或 Responses | 自定义 Provider |
| [OpenClaw](openclaw.md) | Chat、Responses 或 Messages | `~/.openclaw/openclaw.json` |
| [Hermes Agent](hermes-agent.md) | Chat、Responses 或 Messages | 自定义 Provider |
| [Reasonix](reasonix.md) | Chat Completions | CLI 向导或 Studio 设置 |
| [WorkBuddy / CodeBuddy](workbuddy.md) | Chat Completions | 模型设置或 `.codebuddy/models.json` |
| [Qoder](qoder.md) | 按客户端提供的自定义模型入口配置 | 查看当前版本的 BaseURL 支持情况 |
| [CC Switch](cc-switch.md) | 管理 Responses、Messages、Chat | 图形化 Provider 管理 |
| [LobsterAI](lobsterai.md) | Chat 或 Messages | 桌面端自定义 Provider |

本章覆盖 DeepSeek 文档“接入 Agent 工具”中的工具，并提供 Geekit 的地址、鉴权与模型配置。每个教程列出官方来源和验证步骤，实际兼容性可通过客户端任务与 Geekit 调用日志确认。

Qoder 的公开 BYOK 文档采用供应商目录，Geekit 接入还需要客户端提供自定义 BaseURL。对应页面说明当前可核对的字段与接入条件。

欢迎通过 [Awesome Geekit Agent](https://github.com/yirenyiqi/awesome-geekit-agent)补充教程和兼容性记录。提交步骤见[贡献你的 Agent 接入](contributing.md)。

## 配置前检查

1. 确认 `GEEKIT_API_KEY` 已替换为 [API Keys](https://geekit.top/keys) 页面生成的真实令牌。
2. 从[模型广场](https://geekit.top/pricing)选择最新模型，或调用 `/v1/models` 复制真实模型 ID。
3. 确认客户端要求根地址、`/v1` Base URL，还是完整端点。
4. 优先通过用户级永久环境变量或系统密钥管理器提供令牌。
5. 使用一个最小任务验证流式响应、工具调用和 Geekit 日志。

## Base URL 对照

| 协议                    | 填写值                                        |
| ----------------------- | --------------------------------------------- |
| OpenAI Chat / Responses | `https://geekit.top/v1`                  |
| Anthropic Messages      | `https://geekit.top`                     |
| Chat 完整端点           | `https://geekit.top/v1/chat/completions` |
| Responses 完整端点      | `https://geekit.top/v1/responses`        |
| Messages 完整端点       | `https://geekit.top/v1/messages`         |

地址重复出现 `/v1/v1`、客户端请求根路径或返回 `404` 时，优先检查这一项。

## 兼容性边界

[ZCode、StepClaw、Coze 及自托管平台](compatibility.md)的公开证据不完整，或配置属于管理员能力。相关页面只说明已确认的边界，不提供无法复现的操作步骤。

## 安全要求

- 不要把真实令牌写入项目仓库或共享配置。
- 不要在截图、录屏、日志和工单中显示令牌。
- 本地 Agent 仍会把模型请求发往所配置的 Geekit 服务；“本地客户端”不等于“内容从不离开设备”。
- 配置第三方工具前确认其密钥存储方式，不要假设所有客户端都加密保存 API Key。

## 官方资料

每个教程末尾列出对应产品的一手资料。客户端更新可能改变字段或界面；超过核验日期较久时，请先对照官方文档再修改配置。
