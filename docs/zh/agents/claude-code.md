# Claude Code

Claude Code 是 Anthropic 的终端编程助手，通过 Anthropic Messages 协议接入 Geekit。

资料核对日期为 2026-11-26。以下配置依据官方文档与 Geekit 接口约定整理，实际模型能力以模型广场及调用日志为准。

## 安装与准备

按官方安装说明安装或更新 Claude Code，执行 `claude --version` 确认可用。已有安装可直接进入配置步骤。安装入口见 [Claude Code 安装文档](https://code.claude.com/docs/en/setup)。

在[快速开始](../getting-started.md)配置永久环境变量 `GEEKIT_BASE_URL`、`GEEKIT_API_KEY` 和 `GEEKIT_MODEL`，并从[模型广场](https://geekit.top/pricing)选择支持 Messages 与工具调用的模型。

## 配置 Geekit

在 `~/.zshrc` 或 `~/.bashrc` 中加入以下设置，保存后重新打开终端。

```bash
export ANTHROPIC_BASE_URL="$GEEKIT_BASE_URL"
export ANTHROPIC_AUTH_TOKEN="$GEEKIT_API_KEY"
export ANTHROPIC_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_OPUS_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_SONNET_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="$GEEKIT_MODEL"
export CLAUDE_CODE_SUBAGENT_MODEL="$GEEKIT_MODEL"
```

先加载 `GEEKIT_*` 变量，再加载上面的设置。也可以使用 [CC Switch](cc-switch.md)管理 Provider。

Windows PowerShell 使用用户级永久变量，执行后重新打开终端。

```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", $env:GEEKIT_BASE_URL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", $env:GEEKIT_API_KEY, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_OPUS_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_SONNET_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_HAIKU_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("CLAUDE_CODE_SUBAGENT_MODEL", $env:GEEKIT_MODEL, "User")
```

`ANTHROPIC_BASE_URL` 填写 Geekit 根地址 `https://geekit.top`。Claude Code 会请求 `/v1/messages`。默认模型与子代理模型在这里统一使用所选 Geekit 模型 ID，也可分别设置账户有权调用的模型。

## 首次运行与验证

```bash
cd /path/to/my-project
claude
```

先让助手总结项目结构，检查 `/status` 中的模型，再在 Geekit 调用日志中确认模型 ID、响应状态、Token 用量和工具调用。

客户端内置的搜索、图片与长上下文能力取决于所选模型及上游接口。按模型广场公布的能力配置参数。遇到 `401`、`403`、`404` 或 `429` 时可查阅[错误码](../error-codes.md)。

## 参考资料

- [Claude Code 网关配置](https://code.claude.com/docs/en/llm-gateway)
- [Claude Code 模型配置](https://code.claude.com/docs/en/model-config)
- [DeepSeek 的 Claude Code 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/claude_code/)
