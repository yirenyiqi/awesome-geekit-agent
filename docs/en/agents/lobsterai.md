# LobsterAI

LobsterAI is an open-source desktop Agent maintained by NetEase Youdao. Prefer OpenAI Chat Completions for a custom Geekit URL. Claude models can use Anthropic Messages.

Verification date: 2026-07-27

Delivery status: Geekit mirror pending verification

## 1. Configure persistent environment variables

Generate a key on [API Keys](https://geekit.top/keys), then persist `GEEKIT_API_KEY` using [step 1 of Quick start](../getting-started.md). This page uses:

- **`<Geekit address>`**: `https://geekit.top`.
- **`GEEKIT_API_KEY`**: keep the variable name and replace its value with your real Geekit key.
- **`<Geekit model ID>`**: choose a current model from the [model catalog](https://geekit.top/pricing). Use the currently available IDs shown there for both Codex and Claude models.

If a graphical client cannot read environment variables, paste only the value of `GEEKIT_API_KEY` into its protected API Key field. Do not store it in project files.

**Geekit mirror pending verification**

A Geekit mirror is not yet available for the current LobsterAI version. Download only from the official repository below. Geekit will publish download commands after independent checksums and platform publisher identities are verified.

## Prerequisites

- Install or update a version supporting custom models from the [official LobsterAI repository](https://github.com/netease-youdao/LobsterAI).
- Obtain the Geekit root address, user key, and model ID.
- Confirm the model supports the tools, images, or reasoning required for your task.

## Use OpenAI Chat

Add a custom provider in LobsterAI's model or provider settings:

| Field | Value |
| --- | --- |
| Provider name | `Geekit` |
| API format | OpenAI |
| Base URL | `https://geekit.top/v1` |
| API Key | Your Geekit user key |
| Model | An actual available Geekit model ID |

For custom OpenAI URLs outside the official OpenAI domain, LobsterAI usually selects Chat Completions. After saving, check for `/v1/chat/completions` in Geekit logs.

## Use Anthropic Messages

For Claude models, add a provider using Anthropic format:

| Field | Value |
| --- | --- |
| API format | Anthropic |
| Base URL | `https://geekit.top` |
| API Key | Your Geekit user key |
| Model | An actual available Claude model ID in Geekit |

Messages uses the Geekit root address. Do not append `/v1`.

## Responses support

A custom Geekit URL does not automatically select Responses. Enable it only if the current client UI or advanced configuration explicitly offers `openai-responses`; otherwise, continue using Chat Completions.

A model name containing Codex or GPT does not mean the client switched to Responses. Check the actual endpoint in Geekit logs.

## Verify

Save the model, start a new conversation, and run a minimal read-only task. Check:

1. The active LobsterAI model belongs to the Geekit provider.
2. Geekit logs show `/v1/chat/completions` or `/v1/messages`.
3. Streaming finishes normally.
4. File, browser, and other tool calls match the model's actual capabilities.

LobsterAI runs locally, but model requests still go to the configured Geekit address. Whether sensitive files leave the device depends on the task, tools, and request content. A local client does not guarantee that content stays on the device.

## Troubleshooting

### `404` responses

OpenAI format uses the `/v1` Base URL; Anthropic format uses the root address. Check for a duplicated `/v1`.

### Images or tools are unavailable

Check the capabilities declared in the model settings and confirm the model supports the corresponding inputs and tool protocol. Do not enable every capability at once.

### Requests still use the old address

Select the model again and start a new conversation. Restart LobsterAI if needed, then confirm the actual request address through Geekit logs.

## Official sources

- [LobsterAI repository](https://github.com/netease-youdao/LobsterAI)
- [Provider configuration types](https://github.com/netease-youdao/LobsterAI/blob/main/src/shared/providers/types.ts)
