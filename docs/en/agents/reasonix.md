# Reasonix

Reasonix provides terminal and Studio desktop coding assistants. Current versions support custom OpenAI-compatible providers.

Sources checked on 2026-11-26 against the current official CLI and configuration-path documentation.

## Install and prepare

Install the CLI or Studio for your platform from the official repository releases. Run `reasonix --version` to confirm the CLI is available. See the [Reasonix repository](https://github.com/esengine/reasonix).

Obtain the Geekit root URL, API key, and a model supporting Chat Completions and tool calls using [Quick start](../getting-started.md).

## Configure the CLI

```bash
reasonix setup
```

Add an OpenAI-compatible provider in the provider manager.

| Setting | Value |
| --- | --- |
| Provider | `geekit` |
| Base URL | `https://geekit.top/v1` |
| API key environment variable name | `GEEKIT_API_KEY` |
| API key | Your generated Geekit key value |
| Default model | Exact ID copied from the Geekit model list |

Save the key through the wizard, test the connection, refresh the model list, select a default model, and choose Save and exit.

Current Reasonix stores provider credentials in the user-level Reasonix home `.env`. Defaults are `~/.reasonix/.env` on macOS / Linux and `%APPDATA%\reasonix\.env` on Windows. The wizard writes this file. CLI and desktop share configuration and credentials.

## Configure Studio

Add an OpenAI-compatible service in Studio provider settings. If the field is labeled API address, enter the complete request URL `https://geekit.top/v1/chat/completions`. Current desktop versions save this field as the exact request URL.

## First run and verification

```bash
cd /path/to/my-project
reasonix
```

Select the Geekit model using `/model` and inspect settings with `/status`. Ask for a project summary, then check `/v1/chat/completions`, the model ID, tokens, and tool calls in Geekit logs.

If discovery returns an empty list, add the model ID manually. For authentication failures, save credentials again using the wizard and consult [Error codes](../error-codes.md).

## References

- [Reasonix CLI reference](https://github.com/esengine/reasonix/blob/main/docs/CLI.md)
- [Reasonix configuration and credential paths](https://github.com/esengine/reasonix/blob/main/docs/CONFIG_PATHS.md)
- [DeepSeek Reasonix guide](https://api-docs.deepseek.com/quick_start/agent_integrations/reasonix/)
