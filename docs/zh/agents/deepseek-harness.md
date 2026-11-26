# DeepSeek Harness

DeepSeek Harness 提供 Web UI、CLI 与 SDK。通过自定义模型 API，可将模型请求发送至 Geekit。

资料核对日期为 2026-11-26。

## 安装与启动

按官方仓库 README 安装当前版本并启动 Web UI。启动命令会打印访问地址。安装入口见 [DeepSeek Harness 官方仓库](https://github.com/deepseek-harness/deepseek-harness)。

按[快速开始](../getting-started.md)获取 Geekit API Key 和模型 ID，然后打开 Harness Web UI。

## 添加 Geekit

进入设置 → 模型 → 添加模型提供商，切换到自定义模型 API。

| 项目 | 填写值 |
| --- | --- |
| Provider ID | `geekit` |
| 显示名称 | `Geekit` |
| 基础 URL | `https://geekit.top/v1` |
| API 协议 | OpenAI Chat Completions |
| API Key | Geekit 令牌值 |
| 模型 ID | 从 Geekit 模型列表复制的真实 ID |

点击获取可用模型，选择需要的模型后保存。也可以手动添加模型 ID。需要 Codex 类模型时，可单独建立 `geekit-responses` Provider，选择 OpenAI Responses 并使用相同的 `/v1` 基础地址。

一个 Provider 对应一种协议。Anthropic Messages 使用 Geekit 根地址 `https://geekit.top`，Gemini API 使用 `https://geekit.top/v1beta`。模型协议与能力可在[模型广场](https://geekit.top/pricing)确认。

## 首次运行与验证

在 Web UI 添加并选择项目工作区，然后在模型选择器中选择 Geekit 模型。新建会话，请助手总结仓库的主要模块。

在 Geekit 调用日志中确认请求路径与配置的协议一致，检查流式输出、工具调用和用量。模型配置保存后从下一次请求生效，已发送过请求的会话沿用其会话模型记录。

出现错误时查看[错误码](../error-codes.md)，并在反馈中提供模型与请求 ID。

## 参考资料

- [DeepSeek Harness 快速开始](https://deepseek-harness.github.io/deepseek-harness/guide/quickstart)
- [DeepSeek Harness 模型配置](https://deepseek-harness.github.io/deepseek-harness/guide/providers)
