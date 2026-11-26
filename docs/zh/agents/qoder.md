# Qoder

Qoder 提供 IDE、CLI 与 JetBrains 插件。DeepSeek 的接入教程使用 Qoder 内置的 DeepSeek BYOK 入口。

资料核对日期为 2026-11-26。

## 安装

根据使用习惯安装 IDE、CLI 或 JetBrains 插件，完成账号登录。CLI 可以按官方安装说明安装后执行 `qodercli --version`。安装入口见 [Qoder 下载](https://qoder.com/download)。

## 官方自定义模型入口

| 客户端 | 操作路径 |
| --- | --- |
| Qoder IDE | Qoder IDE 设置 → 模型 → 添加模型 → 选择供应商与模型 → 填写该供应商的 API Key |
| Qoder CLI | `/model` → Custom → Add custom model → 按向导选择 Provider、类型和模型 |
| JetBrains 插件 | Qoder 面板设置 → 插件设置 → 添加模型 |

自定义模型面向个人版，可选供应商以当前客户端和账号显示的目录为准。通过向导验证连接并保存，在会话模型选择器中选择已添加模型。

## Geekit 接入条件

Geekit 接入需要客户端同时提供自定义 BaseURL、API Key 与模型 ID。当前 Qoder 官方文档展示的 BYOK 流程按服务商目录选择模型，文档公开的字段包括供应商、模型和密钥。

如当前客户端已提供任意 BaseURL 输入框，可按下表配置，并通过调用日志确认实际请求路径。

| 字段 | Geekit 值 |
| --- | --- |
| OpenAI 兼容 BaseURL | `https://geekit.top/v1` |
| API Key | 在 [API Keys](https://geekit.top/keys) 生成的令牌 |
| 模型 ID | 从[模型广场](https://geekit.top/pricing)复制的真实 ID |

欢迎向 [Awesome Geekit Agent](https://github.com/yirenyiqi/awesome-geekit-agent)贡献带客户端版本、BaseURL 字段与脱敏调用记录的 Qoder 接入步骤。

## 首次运行与排错

IDE 或插件中打开项目并选择模型。CLI 进入项目目录后运行 `qodercli`，通过 `/model` 切换模型。先执行项目摘要任务，再检查服务商用量与调用日志。Geekit 调用失败可查阅[错误码](../error-codes.md)。

## 参考资料

- [Qoder 自定义模型](https://docs.qoder.com/zh/user-guide/chat/custom-models)
- [Qoder CLI 自定义模型](https://docs.qoder.com/zh/cli/custom-models)
- [DeepSeek 的 Qoder 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/qoder/)
