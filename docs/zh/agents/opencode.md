# OpenCode

OpenCode 提供终端与网页编程助手，可通过自定义 Provider 调用 Geekit。

资料核对日期为 2026-11-26。本页采用当前官方 `providers` / `package` / `settings` 配置结构。

## 安装与准备

从官方入口安装或升级 OpenCode，运行 `opencode --version` 确认版本。已有安装可执行 `opencode upgrade`。安装说明见 [OpenCode 下载](https://opencode.ai/download)。

按[快速开始](../getting-started.md)设置永久环境变量，选择支持 Chat Completions 与工具调用的 Geekit 模型。

## 添加 Geekit Provider

编辑用户级 `~/.config/opencode/opencode.json`，将以下内容合并到已有配置。把两处 `<Geekit model ID>` 替换为同一个真实模型 ID。

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

`env` 指定读取密钥的环境变量。`settings.baseURL` 使用 `https://geekit.top/v1`。运行 OpenCode 的进程需要能读取 `GEEKIT_API_KEY`。

如果已安装版本的配置使用 `provider` / `npm` / `options` 字段，请先按该版本的配置 schema 核对，或升级后使用本页格式。

## 选择模型与验证

```bash
cd /path/to/my-project
opencode
```

使用 `/models` 选择 `geekit/<Geekit model ID>`，让助手总结项目结构。在 Geekit 调用日志中确认请求命中 `/v1/chat/completions`，检查流式输出与工具调用。

需要 Responses 的模型可按当前官方包目录选择 `@opencode/ai/providers/openai-compatible/responses`，并核对该模型对 `/v1/responses` 的支持。遇到调用失败时查看[错误码](../error-codes.md)。

## 参考资料

- [OpenCode Provider 文档](https://opencode.ai/docs/providers/)
- [OpenCode 配置文档](https://opencode.ai/docs/config/)
- [DeepSeek 的 OpenCode 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/opencode/)
