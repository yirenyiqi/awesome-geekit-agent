# OpenClaw

OpenClaw is a personal AI assistant extensible through chat channels and Skills. Connect Geekit using a custom model provider.

Sources checked on 2026-11-26.

## Install and prepare

Complete installation and initial setup using the official instructions. Existing installations can run `openclaw onboard`. See [OpenClaw getting started](https://docs.openclaw.ai/start/getting-started).

Obtain a Geekit key and a tool-capable model ID using [Quick start](../getting-started.md). When running OpenClaw as a background service, provide `GEEKIT_API_KEY` in that service's environment configuration and restart it.

## Configure the model

Merge this configuration into the user-level `~/.openclaw/openclaw.json`. Replace both occurrences of `<Geekit model ID>` with the same real model ID.

```json
{
  "agents": {
    "defaults": {
      "model": { "primary": "geekit/<Geekit model ID>" }
    }
  },
  "models": {
    "mode": "merge",
    "providers": {
      "geekit": {
        "baseUrl": "https://geekit.top/v1",
        "apiKey": "${GEEKIT_API_KEY}",
        "api": "openai-completions",
        "models": [
          { "id": "<Geekit model ID>", "name": "Geekit" }
        ]
      }
    }
  }
}
```

`apiKey` references the environment variable. Set `baseUrl` to `https://geekit.top/v1`. `models.providers.geekit.models` registers the model and `agents.defaults.model.primary` selects it by default.

## First run and verification

Restart the running Gateway, inspect the models, and open the interface.

```bash
openclaw gateway restart
openclaw models list
openclaw dashboard
```

Alternatively, run `openclaw tui` for the terminal interface. Select the Geekit model, send a simple message, and try a tool task. Check the model ID, streaming response, and token usage in Geekit logs.

Changing protocols requires matching the `api`, BaseURL, and model settings. See the official custom-provider documentation for supported values and [Agent setup](index.md) for endpoint rules. Use [Error codes](../error-codes.md) to diagnose failures.

## References

- [OpenClaw custom providers](https://docs.openclaw.ai/concepts/model-providers/custom-providers)
- [OpenClaw models](https://docs.openclaw.ai/concepts/models)
- [DeepSeek OpenClaw guide](https://api-docs.deepseek.com/quick_start/agent_integrations/openclaw/)
