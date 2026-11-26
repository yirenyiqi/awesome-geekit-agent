# OpenClaw

OpenClaw 是可通过聊天渠道和 Skills 扩展的个人 AI 助手。Geekit 使用自定义模型提供商接入。

资料核对日期为 2026-11-26。

## 安装与准备

按官方安装说明完成安装与初始设置，已有安装可以运行 `openclaw onboard` 进入向导。安装说明见 [OpenClaw 入门](https://docs.openclaw.ai/start/getting-started)。

按[快速开始](../getting-started.md)获取 Geekit 令牌和支持工具调用的模型 ID。通过后台服务启动 OpenClaw 时，需要在该服务的环境配置中提供 `GEEKIT_API_KEY`，保存后重启服务。

## 配置模型

将以下配置合并到用户级 `~/.openclaw/openclaw.json`，把两处 `<Geekit model ID>` 替换为同一个真实模型 ID。

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

`apiKey` 引用环境变量，`baseUrl` 使用 `https://geekit.top/v1`。`models.providers.geekit.models` 注册可用模型，`agents.defaults.model.primary` 选择默认模型。

## 首次运行与验证

重启正在运行的 Gateway，然后检查模型并打开界面。

```bash
openclaw gateway restart
openclaw models list
openclaw dashboard
```

也可以运行 `openclaw tui` 使用终端界面。选择 Geekit 模型，发送一条简单消息，再执行一个工具任务。在 Geekit 调用日志中检查模型 ID、流式响应和 Token 用量。

切换协议时，需要同时调整 `api`、BaseURL 和所选模型。配置值见官方自定义 Provider 文档，路径规则见[Agent 接入](index.md)。失败时查看[错误码](../error-codes.md)。

## 参考资料

- [OpenClaw 自定义 Provider](https://docs.openclaw.ai/concepts/model-providers/custom-providers)
- [OpenClaw 模型配置](https://docs.openclaw.ai/concepts/models)
- [DeepSeek 的 OpenClaw 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/openclaw/)
