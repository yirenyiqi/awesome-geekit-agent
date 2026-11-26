# CC Switch

CC Switch manages provider configuration for Claude Code, Codex, and Hermes; it does not execute Agent tasks itself. Prefer connecting each client directly to its matching Geekit protocol. Enable local conversion only when the protocols differ.

Verification date: 2026-07-27

Delivery status: Geekit mirror pending verification

## 1. Configure persistent environment variables

Generate a key on [API Keys](https://geekit.top/keys), then persist `GEEKIT_API_KEY` using [step 1 of Quick start](../getting-started.md). This page uses:

- **`<Geekit address>`**: `https://geekit.top`.
- **`GEEKIT_API_KEY`**: keep the variable name and replace its value with your real Geekit key.
- **`<Geekit model ID>`**: choose a current model from the [model catalog](https://geekit.top/pricing). Use the currently available IDs shown there for both Codex and Claude models.

If a graphical client cannot read environment variables, paste only the value of `GEEKIT_API_KEY` into its protected API Key field. Do not store it in project files.

**Geekit mirror pending verification**

A Geekit mirror is not yet available for the current CC Switch version. Install or update only from the official repository linked below. Download commands will be published after the publisher identity and platform installers are verified.

Install or update using the [official CC Switch repository](https://github.com/farion1231/cc-switch), then configure the client you want it to manage.

## Choose the target client

Select a client first, then add a Geekit provider for its protocol:

| Target client | Protocol | Address |
| --- | --- | --- |
| Codex CLI | OpenAI Responses | `https://geekit.top/v1` |
| Claude Code | Anthropic Messages | `https://geekit.top` |
| Hermes Agent | Depends on `api_mode` | See [Hermes Agent](hermes-agent.md) |

Use model IDs from Geekit's `/v1/models`, rather than copying names from the client's built-in model list.

## Add a Geekit provider

Create a custom provider in CC Switch's provider manager:

- Name: `Geekit`
- API Key: your Geekit user key
- Base URL: use the table above
- Model: an actual available Geekit model ID

Save and apply the provider to Codex, Claude Code, or the target client. Close and restart existing CLI sessions after switching providers so processes do not retain old environment variables or configuration.

CC Switch centralizes configuration, but keys remain sensitive. Do not assume every version encrypts stored API keys at rest. Enable disk encryption, screen locking, and appropriate file permissions on the device.

## When to enable local conversion

Protocol conversion is needed only when:

- The client sends only Anthropic Messages, but the target Geekit model requires Responses.
- The client sends only Responses, but the target model requires Anthropic Messages.
- You have confirmed that direct connection fails because of a protocol mismatch, rather than an incorrect address, key, or model ID.

With conversion enabled, CC Switch becomes part of the local request path. You must:

1. Keep CC Switch running.
2. Enable global routing and the target application's takeover or routing option.
3. Restart the target CLI session.
4. Check both CC Switch routing records and Geekit request logs.

Leave conversion disabled when protocols already match. A direct connection has fewer steps and makes streaming, tool-call, and error diagnosis easier.

## Verify

Run a minimal read-only task in the target client, then confirm:

1. CC Switch shows that the application is using the Geekit provider.
2. Geekit logs contain the expected endpoint, such as `/v1/responses` or `/v1/messages`.
3. The logged model ID matches the CC Switch configuration.
4. Streaming and tool calls both finish successfully.

If automatic discovery through `/v1/models` fails, enter the model ID manually. Discovery failure does not mean the inference endpoint is unavailable.

## Restore the original configuration

Switch back to the original provider in CC Switch and restart the CLI. If local conversion was enabled, also disable global routing and application takeover, then confirm the client no longer points to CC Switch's local address.

## Official sources

- [CC Switch repository](https://github.com/farion1231/cc-switch)
- [Adding providers (Chinese)](https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/zh/2-providers/2.1-add.md)
- [Configuration files (Chinese)](https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/zh/5-faq/5.1-config-files.md)
- [Claude to Responses routing (Chinese)](https://github.com/farion1231/cc-switch/blob/main/docs/guides/claude-codex-routing-guide-zh.md)
- [Codex to Messages routing (Chinese)](https://github.com/farion1231/cc-switch/blob/main/docs/guides/codex-claude-routing-guide-zh.md)
