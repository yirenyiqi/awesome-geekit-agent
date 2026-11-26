# DeepSeek Harness

DeepSeek Harness provides a Web UI, CLI, and SDK. A custom model API routes model requests through Geekit.

Sources checked on 2026-11-26.

## Install and start

Install the current version and start the Web UI following the official repository README. The startup command prints the address. See the [DeepSeek Harness repository](https://github.com/deepseek-harness/deepseek-harness).

Obtain a Geekit API key and model ID using [Quick start](../getting-started.md), then open the Harness Web UI.

## Add Geekit

Open Settings → Models → Add model provider and switch to Custom model API.

| Setting | Value |
| --- | --- |
| Provider ID | `geekit` |
| Display name | `Geekit` |
| Base URL | `https://geekit.top/v1` |
| API protocol | OpenAI Chat Completions |
| API key | Your Geekit key value |
| Model ID | Exact ID copied from the Geekit model list |

Choose Fetch available models, select models, and save. You can also add model IDs manually. For Codex-style models, create a separate `geekit-responses` provider using OpenAI Responses and the same `/v1` base URL.

Each provider uses one protocol. Anthropic Messages uses the Geekit root `https://geekit.top` and Gemini API uses `https://geekit.top/v1beta`. Confirm protocols and capabilities in the [model catalog](https://geekit.top/pricing).

## First run and verification

Add and select a project workspace in the Web UI, then select the Geekit model. Start a new session and ask for a summary of the repository's main packages.

Check that the endpoint in Geekit logs matches the selected protocol. Inspect streaming, tool calls, and usage. Saved model changes apply to the next request. Sessions that already sent requests retain their recorded session model.

For failures, consult [Error codes](../error-codes.md) and include the model and request ID in reports.

## References

- [DeepSeek Harness quick start](https://deepseek-harness.github.io/deepseek-harness/guide/quickstart)
- [DeepSeek Harness model configuration](https://deepseek-harness.github.io/deepseek-harness/guide/providers)
