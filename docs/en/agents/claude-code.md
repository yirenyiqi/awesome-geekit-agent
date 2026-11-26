# Claude Code

Claude Code is Anthropic's terminal coding assistant. Connect it to Geekit through Anthropic Messages.

Sources checked on 2026-11-26. These settings follow official documentation and Geekit API conventions. Check the model catalog and usage logs for actual model capabilities.

## Install and prepare

Install or update Claude Code using its official instructions and run `claude --version`. Existing installations can proceed directly to configuration. See [Claude Code installation](https://code.claude.com/docs/en/setup).

Persist `GEEKIT_BASE_URL`, `GEEKIT_API_KEY`, and `GEEKIT_MODEL` following [Quick start](../getting-started.md). Select a model supporting Messages and tool calls from the [model catalog](https://geekit.top/pricing).

## Configure Geekit

Add these settings to `~/.zshrc` or `~/.bashrc`, save, and open a new terminal.

```bash
export ANTHROPIC_BASE_URL="$GEEKIT_BASE_URL"
export ANTHROPIC_AUTH_TOKEN="$GEEKIT_API_KEY"
export ANTHROPIC_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_OPUS_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_SONNET_MODEL="$GEEKIT_MODEL"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="$GEEKIT_MODEL"
export CLAUDE_CODE_SUBAGENT_MODEL="$GEEKIT_MODEL"
```

Load the `GEEKIT_*` variables before these settings. [CC Switch](cc-switch.md) also supports provider management.

On Windows, persist the variables in PowerShell and then open a new terminal.

```powershell
[Environment]::SetEnvironmentVariable("ANTHROPIC_BASE_URL", $env:GEEKIT_BASE_URL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_AUTH_TOKEN", $env:GEEKIT_API_KEY, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_OPUS_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_SONNET_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("ANTHROPIC_DEFAULT_HAIKU_MODEL", $env:GEEKIT_MODEL, "User")
[Environment]::SetEnvironmentVariable("CLAUDE_CODE_SUBAGENT_MODEL", $env:GEEKIT_MODEL, "User")
```

Set `ANTHROPIC_BASE_URL` to the Geekit root `https://geekit.top`. Claude Code requests `/v1/messages`. These settings use the selected Geekit model ID for the default and subagent models. You can assign other models your account can access individually.

## First run and verification

```bash
cd /path/to/my-project
claude
```

Ask for a summary of the project structure. Check the model in `/status`, then confirm the model ID, response status, token usage, and tool calls in Geekit usage logs.

Built-in search, images, and long-context features depend on the selected model and upstream API. Configure capabilities using the model catalog. See [Error codes](../error-codes.md) for `401`, `403`, `404`, or `429` failures.

## References

- [Claude Code gateway configuration](https://code.claude.com/docs/en/llm-gateway)
- [Claude Code model configuration](https://code.claude.com/docs/en/model-config)
- [DeepSeek Claude Code guide](https://api-docs.deepseek.com/quick_start/agent_integrations/claude_code/)
