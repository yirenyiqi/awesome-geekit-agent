# WorkBuddy / CodeBuddy

Tencent WorkBuddy supports adding models through “Custom API”. Prefer standard OpenAI Chat Completions for Geekit. Enable custom protocol mode only when the client requires a complete endpoint.

Sources checked on 2026-11-26.

Delivery status: official source available in China

## 1. Configure persistent environment variables

Generate a key on [API Keys](https://geekit.top/keys), then persist `GEEKIT_API_KEY` using [step 1 of Quick start](../getting-started.md). This page uses:

- **`<Geekit address>`**: `https://geekit.top`.
- **`GEEKIT_API_KEY`**: keep the variable name and replace its value with your real Geekit key.
- **`<Geekit model ID>`**: choose a current model from the [model catalog](https://geekit.top/pricing). Use the currently available IDs shown there for both Codex and Claude models.

If a graphical client cannot read environment variables, paste only the value of `GEEKIT_API_KEY` into its protected API Key field. Do not store it in project files.

**Official source available in China**

Tencent WorkBuddy's official download is directly accessible in China, so Geekit does not redistribute its installer. Install or update from the official Tencent sources below.

## Prerequisites

- Install or update a client supporting Custom API from the [official WorkBuddy site](https://copilot.tencent.com/work), then sign in.
- Obtain the Geekit root address, user key, and model ID.
- Choose a model supporting Chat Completions and the tools your task requires.

## Standard configuration

Add a Custom API in WorkBuddy's model settings. Keep “Custom protocol” disabled and enter:

| Field | Value |
| --- | --- |
| URL | `https://geekit.top/v1` |
| API Key | Your Geekit user key |
| Model name | An actual available Geekit model ID |

Standard mode appends the Chat Completions path. Before saving, check that the address ends with exactly one `/v1`.

## Use a complete endpoint

If your WorkBuddy version explicitly requires a complete request URL, enable Custom protocol and enter:

```text
https://geekit.top/v1/chat/completions
```

Custom protocol mode requests this exact URL without appending a path. Do not enter the complete endpoint in standard mode, where `/chat/completions` may be duplicated.

## WorkBuddy / CodeBuddy local model configuration

Install and sign in to the client, then open a project once. Edit user-level `~/.codebuddy/models.json`, or `C:\Users\<username>\.codebuddy\models.json` on Windows. For a project-specific configuration, use `<project>/.codebuddy/models.json`.

Set the persistent user variable `GEEKIT_API_KEY` following [Quick start](../getting-started.md). Merge this JSON into existing configuration and replace both `<Geekit model ID>` values with the same real model ID supporting tool calls.

```json
{
  "models": [
    {
      "id": "<Geekit model ID>",
      "name": "Geekit",
      "vendor": "Geekit",
      "url": "https://geekit.top/v1/chat/completions",
      "apiKey": "${GEEKIT_API_KEY}",
      "maxInputTokens": 32768,
      "maxOutputTokens": 4096,
      "supportsToolCall": true,
      "supportsImages": false
    }
  ],
  "availableModels": ["<Geekit model ID>"]
}
```

The example caps input at 32768 tokens and output at 4096 tokens with image input disabled. Adjust these fields to match the context, output, and image capabilities listed in the model catalog.

Save as UTF-8 without a BOM. Fully quit and reopen the client, then choose Geekit in the model selector. Desktop clients need to inherit `GEEKIT_API_KEY`. If the variable appears literally, launch the client from a terminal that has loaded it, or save the key through the client's API key settings.

## Verify the connection

Run WorkBuddy's connection check, then start a new conversation with a minimal read-only task. Confirm in Geekit logs:

1. The request reaches `/v1/chat/completions`.
2. The model ID matches the WorkBuddy configuration.
3. The status and token counts are normal.
4. Streaming, tool calls, and file tasks work within the model's actual capabilities.

WorkBuddy's official pages do not provide a streaming and tool-call compatibility matrix for every model. A successful connection verifies the protocol and credentials; it does not establish that every model supports every desktop task.

## Troubleshooting

### `404` responses

Match the Custom protocol toggle to the URL: disabled uses the `/v1` Base URL; enabled uses the full `/v1/chat/completions` endpoint.

### `401` responses

Paste your Geekit user key again and remove leading or trailing spaces. Do not include it in screenshots, project files, or support requests.

### The model is missing from the list

WorkBuddy may not synchronize Geekit's model list automatically. Enter the exact model ID returned by `/v1/models`.

## Official sources

- [Tencent WorkBuddy model configuration](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Model)
- [Tencent WorkBuddy product page](https://cloud.tencent.com/product/workbuddy)
- [Official WorkBuddy site](https://copilot.tencent.com/work)

- [DeepSeek WorkBuddy / CodeBuddy guide](https://api-docs.deepseek.com/quick_start/agent_integrations/workbuddy/)
