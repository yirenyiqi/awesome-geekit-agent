# Agent setup

Geekit provides a shared model endpoint for CLIs, desktop Agents, and open-source clients that support custom providers. Codex CLI is the recommended starting point. Configure other clients using their native protocol.

Sources checked on 2026-11-26.

**Connecting through Geekit when official model services are unreachable**

Install or update clients using their official instructions. The verified Codex CLI is also available from Geekit's protected mirror. Then configure the Base URL, API key, and model ID as described in this section. Model requests go to `https://geekit.top`, without requiring a direct connection to the OpenAI or Anthropic API or giving upstream credentials to the client.

Apart from the verified Codex CLI mirror, check each client's delivery status for installer availability. If official downloads are unreachable on your network, do not download executables from untrusted file-sharing sites.

**First, obtain and configure your connection settings**

- **`<Geekit address>`**: use `https://geekit.top`.
- **`GEEKIT_API_KEY`**: keep this variable name and replace its value with your real Geekit key, such as `sk-...`. [Generate an API key](https://geekit.top/keys).
- **`<Geekit model ID>`**: use the latest model available to your account. The recommended models are `gpt-6-astra` for Codex and `claude-fable-5` for Claude. [View available models](https://geekit.top/pricing).

Add the following to `~/.zshrc` (Zsh) or `~/.bashrc` (Bash):

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<your real Geekit API key>"
export GEEKIT_MODEL="<Geekit model ID>"
```

Reopen your terminal or run `source ~/.zshrc` / `source ~/.bashrc`. In PowerShell, set persistent user environment variables:

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<your real Geekit API key>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "<Geekit model ID>", "User")
```

Close and reopen PowerShell afterward. See [Quick start](../getting-started.md) for details.

## Integration guides

| Client | Geekit protocol | Configuration |
| --- | --- | --- |
| [DeepSeek Harness](deepseek-harness.md) | Chat, Responses, Messages, or Gemini | Custom model API in the Web UI |
| [Claude Code](claude-code.md) | Anthropic Messages | Environment variables |
| [Codex CLI](codex-cli.md) | OpenAI Responses | `~/.codex/config.toml` |
| [OpenCode](opencode.md) | Chat Completions or Responses | Custom provider |
| [OpenClaw](openclaw.md) | Chat, Responses, or Messages | `~/.openclaw/openclaw.json` |
| [Hermes Agent](hermes-agent.md) | Chat, Responses, or Messages | Custom provider |
| [Reasonix](reasonix.md) | Chat Completions | CLI wizard or Studio settings |
| [WorkBuddy / CodeBuddy](workbuddy.md) | Chat Completions | Model settings or `.codebuddy/models.json` |
| [Qoder](qoder.md) | Use the current client's custom-model entry point | Check BaseURL support for the installed version |
| [CC Switch](cc-switch.md) | Manages Responses, Messages, and Chat | Graphical provider management |
| [LobsterAI](lobsterai.md) | Chat or Messages | Desktop custom provider |

This section covers the tools in DeepSeek's Agent integrations documentation with Geekit addresses, authentication, and model settings. Each guide includes official sources and verification steps. Check compatibility using a client task and Geekit usage logs.

Qoder's public BYOK documentation uses a provider catalog. Connecting to Geekit also requires an editable BaseURL. Its guide describes the documented fields and connection requirements.

Contribute tutorials and compatibility records through [Awesome Geekit Agent](https://github.com/yirenyiqi/awesome-geekit-agent). See [Contribute an Agent integration](contributing.md) for submission steps.

## Before configuring a client

1. Replace `GEEKIT_API_KEY` with a real key generated on the [API Keys](https://geekit.top/keys) page.
2. Choose a current model from the [model catalog](https://geekit.top/pricing), or copy its exact ID from `/v1/models`.
3. Check whether the client expects a root address, a `/v1` Base URL, or a complete endpoint.
4. Prefer persistent user environment variables or an operating-system credential manager for the key.
5. Run a minimal task to verify streaming, tool calls, and Geekit logs.

## Base URL reference

| Protocol | Value |
| --- | --- |
| OpenAI Chat / Responses | `https://geekit.top/v1` |
| Anthropic Messages | `https://geekit.top` |
| Complete Chat endpoint | `https://geekit.top/v1/chat/completions` |
| Complete Responses endpoint | `https://geekit.top/v1/responses` |
| Complete Messages endpoint | `https://geekit.top/v1/messages` |

Check these values first if the URL contains `/v1/v1`, the client requests the root path, or requests return `404`.

## Compatibility limits

Public evidence for [ZCode, StepClaw, Coze, and self-hosted platforms](compatibility.md) is incomplete, or configuration requires administrator access. That page explains confirmed limits without providing steps that cannot be reproduced.

## Security

- Do not put real keys in project repositories or shared configuration.
- Do not expose keys in screenshots, recordings, logs, or support requests.
- A local Agent still sends model requests to the configured Geekit service. Running the client locally does not guarantee that content stays on the device.
- Check how third-party tools store credentials. Do not assume every client encrypts saved API keys.

## Official sources

Each tutorial links to primary sources for the product. Client updates may change fields or screens; if the verification date is old, check the official documentation before changing configuration.
