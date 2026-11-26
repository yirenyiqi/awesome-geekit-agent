# OpenCode

OpenCode provides terminal and web coding assistants. A custom provider connects it to Geekit.

Sources checked on 2026-11-26. This guide uses the current official `providers` / `package` / `settings` configuration format.

## Install and prepare

Install or update OpenCode from its official source and run `opencode --version`. Existing installations can run `opencode upgrade`. See [OpenCode downloads](https://opencode.ai/download).

Persist the environment variables from [Quick start](../getting-started.md) and select a Geekit model supporting Chat Completions and tool calls.

## Add the Geekit provider

Merge the following into the user-level `~/.config/opencode/opencode.json`. Replace both occurrences of `<Geekit model ID>` with the same real model ID.

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "geekit/<Geekit model ID>",
  "providers": {
    "geekit": {
      "name": "Geekit",
      "env": ["GEEKIT_API_KEY"],
      "package": "@opencode/ai/providers/openai-compatible",
      "settings": {
        "baseURL": "https://geekit.top/v1"
      },
      "models": {
        "<Geekit model ID>": {
          "name": "Geekit"
        }
      }
    }
  }
}
```

`env` identifies the credential environment variable. Set `settings.baseURL` to `https://geekit.top/v1`. The OpenCode process needs access to `GEEKIT_API_KEY`.

If your installed version uses `provider` / `npm` / `options`, consult that version's configuration schema or upgrade before using this format.

## Select a model and verify

```bash
cd /path/to/my-project
opencode
```

Choose `geekit/<Geekit model ID>` using `/models` and ask for a project summary. Confirm `/v1/chat/completions`, streaming output, and tool calls in Geekit usage logs.

For a Responses model, select `@opencode/ai/providers/openai-compatible/responses` from the current official package catalog and confirm the model supports `/v1/responses`. See [Error codes](../error-codes.md) for troubleshooting.

## References

- [OpenCode providers](https://opencode.ai/docs/providers/)
- [OpenCode configuration](https://opencode.ai/docs/config/)
- [DeepSeek OpenCode guide](https://api-docs.deepseek.com/quick_start/agent_integrations/opencode/)
