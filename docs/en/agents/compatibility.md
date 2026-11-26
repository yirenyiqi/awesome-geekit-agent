# Other Agent compatibility

This page records the limits of products that do not yet have a full tutorial. Geekit publishes reproducible setup steps only when official sources confirm the custom-provider fields, request protocol, and required user permissions.

Verification date: 2026-07-27

## Status overview

| Product | Current finding | Recommendation |
| --- | --- | --- |
| ZCode | Supports OpenAI- or Anthropic-compatible providers, but the OpenAI subprotocol is unspecified | Experimental use only |
| StepClaw | No stable official configuration contract for third-party providers found | Do not connect yet |
| Hosted Coze | Arbitrary Base URLs for ordinary users are unconfirmed | No direct-connection tutorial |
| Coze Studio | Platform administrators can configure model services | Contact your instance administrator |
| Dify, FastGPT | Support compatible model services through workspace or platform administration | Outside the ordinary-user setup path |
| DeepThink, AgentScope | Self-hosted platforms or development frameworks | Reserved for future developer documentation |

## ZCode

ZCode's official documentation confirms support for custom OpenAI- or Anthropic-compatible providers. Fields include provider name, Base URL, and API key, with automatic model loading or manual model entry.

The documentation currently says only “OpenAI compatible” and does not specify Chat Completions or Responses. Geekit therefore does not yet provide a full tutorial. To experiment yourself:

1. Use a test key instead of a long-lived or high-quota key.
2. Start with `https://geekit.top/v1` for the OpenAI-compatible URL.
3. Manually choose a model confirmed to support Chat Completions.
4. Use Geekit logs to verify the actual endpoint, streaming result, and tool calls.

Stop if logs show unsupported endpoints or malformed tool calls. Do not guess a production configuration by repeatedly changing paths.

## StepClaw

StepFun provides entry points related to StepClaw, but as of the verification date, no stable, officially maintained user documentation was found confirming arbitrary third-party Base URLs, API keys, and protocol selection.

This site therefore does not rely on secondary claims of native support for arbitrary OpenAI-compatible APIs, Ollama, or all Chinese model APIs, and does not provide unverified form instructions. Guidance will be added after an official configuration contract or real-client verification is available.

## Coze and Coze Studio

The open-source Coze Studio allows platform administrators to manage model services. This is a capability of self-hosted administration; it does not establish that ordinary hosted Coze users can add an arbitrary Geekit Base URL.

- Ordinary Coze users: no direct Geekit tutorial is available yet.
- Users of an existing Coze Studio instance: ask the instance administrator to add models.
- Coze Studio administrators: configuration belongs in administrator documentation and is not covered in these public user guides.

## Self-hosted platforms and development frameworks

Dify, FastGPT, DeepThink, and AgentScope support compatible model services in different ways, but setup typically involves workspace owners, platform administrators, container networking, or code development. They are not ready-to-use end-user Agents, so these public guides do not cover their setup.

If your organization already configures these platforms centrally, ask the administrator for available model names and the access URL. Ordinary users should not need upstream credentials or administrator provider settings.

## Evaluating a new client

A new client must satisfy all of the following:

1. Official support allows ordinary users to enter a custom Base URL and API key.
2. The documented protocol is Chat Completions, Responses, or Messages.
3. The client can specify a model ID that actually exists in Geekit.
4. Client tests and Geekit request logs can verify the connection together.

Treat “OpenAI compatible” without a specified subprotocol as experimental.

## Official sources

- [ZCode official website](https://zcode.z.ai/cn)
- [ZCode model configuration](https://zcode.z.ai/cn/docs/configuration)
- [StepFun chat](https://chat.stepfun.com)
- [StepFun official website](https://www.stepfun.com)
- [StepFun platform](https://platform.stepfun.com)
- [Coze Studio repository](https://github.com/coze-dev/coze-studio)
- [Dify repository](https://github.com/langgenius/dify)
- [FastGPT repository](https://github.com/labring/FastGPT)
- [DeepThink repository](https://github.com/AIGeniusInstitute/deepthink)
- [AgentScope repository](https://github.com/agentscope-ai/agentscope)
