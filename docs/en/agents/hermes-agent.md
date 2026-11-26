# Hermes Agent

Hermes Agent custom providers support Chat Completions, OpenAI Responses, and Anthropic Messages. Start with Chat Completions for general models, Responses for Codex models, and Messages for Claude models.

Verification date: 2026-07-27

Delivery status: Geekit mirror pending verification

## 1. Configure persistent environment variables

Generate a key on [API Keys](https://geekit.top/keys), then persist `GEEKIT_API_KEY` using [step 1 of Quick start](../getting-started.md). This page uses:

- **`<Geekit address>`**: `https://geekit.top`.
- **`GEEKIT_API_KEY`**: keep the variable name and replace its value with your real Geekit key.
- **`<Geekit model ID>`**: choose a current model from the [model catalog](https://geekit.top/pricing). Use the currently available IDs shown there for both Codex and Claude models.

If a graphical client cannot read environment variables, paste only the value of `GEEKIT_API_KEY` into its protected API Key field. Do not store it in project files.

**Geekit mirror pending verification**

A Geekit mirror is not yet available for the current Hermes Agent version. Install using only the official sources below. Download commands will be published after the publisher identity and installer are verified.

## Prerequisites

- Install Hermes Agent using its [official repository](https://github.com/NousResearch/hermes-agent) and confirm it starts.
- Obtain the Geekit root address, user key, and model ID.
- Choose a model with reliable tool calls and enough context for Agent tasks.

Store the key in the user-level `~/.hermes/.env`, outside project directories and shareable configuration:

```dotenv
GEEKIT_API_KEY=<your real Geekit API key>
```

Restrict the file to the current user:

```bash
chmod 600 ~/.hermes/.env
```

## Configure Chat Completions

Edit `~/.hermes/config.yaml`:

```yaml
custom_providers:
  - name: geekit
    base_url: https://geekit.top/v1
    key_env: GEEKIT_API_KEY
    api_mode: chat_completions
    model: <Geekit model ID>

model:
  provider: custom:geekit
  default: <Geekit model ID>
```

`name` determines the provider ID in `custom:geekit`. Put only the environment variable name in `key_env`, not the key itself.

## Choose another protocol

| Model protocol | `api_mode` | `base_url` |
| --- | --- | --- |
| OpenAI Chat Completions | `chat_completions` | `https://geekit.top/v1` |
| OpenAI Responses | `codex_responses` | `https://geekit.top/v1` |
| Anthropic Messages | `anthropic_messages` | `https://geekit.top` |

Generic URL detection usually falls back to Chat Completions. Set `api_mode` explicitly for Responses and Messages instead of changing only the model name.

## Verify

Start Hermes Agent as usual and run a minimal read-only task. Confirm:

1. Hermes selects the `custom:geekit` provider.
2. The endpoint in Geekit logs matches `api_mode`.
3. The model ID, tokens, and status match expectations.
4. Tool calls continue execution instead of merely printing tool names or JSON text.

Even with Geekit as the primary model, auxiliary tasks such as vision or webpage summaries may use separate routes. To have them follow the primary model, check whether their settings use the officially recommended `auto` value, and confirm the Geekit model supports those capabilities.

## Troubleshooting

### `401` responses

Confirm `GEEKIT_API_KEY` exists in `~/.hermes/.env`, then restart Hermes. Do not print the key in the terminal or logs while debugging.

### `404` responses

Check whether Messages incorrectly uses a Base URL ending in `/v1`, or Chat/Responses is missing `/v1`.

### Responses treated as Chat

Set `api_mode` explicitly to `codex_responses`. A custom URL alone does not select Responses.

## Restore the original provider

Restore `model.provider` and `model.default`, then restart Hermes. Once it no longer uses Geekit, remove the custom-provider entry and user-level key variable if desired.

## Official sources

- [Hermes Agent provider documentation](https://hermes-agent.nousresearch.com/docs/integrations/providers)
- [Hermes Agent repository](https://github.com/NousResearch/hermes-agent)
- [Implementation of the three API modes](https://github.com/NousResearch/hermes-agent/blob/main/hermes_cli/main.py)
