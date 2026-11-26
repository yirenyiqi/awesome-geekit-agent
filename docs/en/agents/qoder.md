# Qoder

Qoder provides an IDE, CLI, and JetBrains plugin. DeepSeek's integration guide uses Qoder's built-in DeepSeek BYOK entry.

Sources checked on 2026-11-26.

## Install

Install the IDE, CLI, or JetBrains plugin and sign in. After following the official CLI installation instructions, run `qodercli --version`. See [Qoder downloads](https://qoder.com/download).

## Official custom-model entry points

| Client | Steps |
| --- | --- |
| Qoder IDE | Qoder IDE Settings → Models → Add model → Select provider and model → Enter that provider's API key |
| Qoder CLI | `/model` → Custom → Add custom model → Choose provider, type, and model |
| JetBrains plugin | Qoder panel settings → Plugin settings → Add model |

Custom models are available in the personal edition. Provider choices depend on the current client and account catalog. Validate and save in the wizard, then select the model in a conversation.

## Geekit connection requirements

Geekit requires editable BaseURL, API key, and model ID fields. Current official Qoder documentation describes a provider-catalog BYOK flow with provider, model, and key fields.

If your client exposes an arbitrary BaseURL field, use the following values and verify the actual endpoint in usage logs.

| Field | Geekit value |
| --- | --- |
| OpenAI-compatible BaseURL | `https://geekit.top/v1` |
| API key | A key generated on [API Keys](https://geekit.top/keys) |
| Model ID | Exact ID from the [model catalog](https://geekit.top/pricing) |

Contribute Qoder instructions with the client version, BaseURL field, and redacted request evidence to [Awesome Geekit Agent](https://github.com/yirenyiqi/awesome-geekit-agent).

## First run and troubleshooting

Open a project and select the model in the IDE or plugin. For the CLI, run `qodercli` in the project directory and use `/model`. Ask for a project summary, then check provider usage and request logs. For Geekit failures, see [Error codes](../error-codes.md).

## References

- [Qoder custom models](https://docs.qoder.com/user-guide/chat/custom-models)
- [Qoder CLI custom models](https://docs.qoder.com/cli/custom-models)
- [DeepSeek Qoder guide](https://api-docs.deepseek.com/quick_start/agent_integrations/qoder/)
