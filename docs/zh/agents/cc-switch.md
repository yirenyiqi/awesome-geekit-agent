# CC Switch

CC Switch 是 Claude Code、Codex 和 Hermes 的 Provider 配置管理器，不是独立执行任务的 Agent。使用 Geekit 时，优先让目标客户端直接连接匹配的协议；只有协议确实不匹配时，才启用 CC Switch 的本地转换。

核验日期：2026-07-27

交付状态：站内镜像待核验

## 1. 配置永久环境变量

先在 [API Keys](https://geekit.top/keys) 生成令牌，并按[快速开始第一步](../getting-started.md)永久设置 `GEEKIT_API_KEY`。本页使用以下三个参数：

- **`<Geekit 地址>`**：`https://geekit.top`。
- **`GEEKIT_API_KEY`**：变量名保持不变，变量值替换为生成的真实 Geekit 令牌。
- **`<Geekit 模型 ID>`**：从[模型广场](https://geekit.top/pricing)选择当前可用的最新模型；Codex 类和 Claude 类模型都以模型广场显示的当前可用 ID 为准。

图形客户端不能直接读取环境变量时，只把 `GEEKIT_API_KEY` 的值填入客户端受保护的 API Key 字段，不要写入项目文件。

**站内镜像待核验**

CC Switch 当前版本暂未提供站内镜像。请只通过本页末尾的官方仓库安装或更新；发布者身份和各平台安装包完成核验后，本站才会显示下载命令。

先按 [CC Switch 官方仓库](https://github.com/farion1231/cc-switch)的安装说明完成安装或更新，再配置需要接管的目标客户端。

## 选择目标客户端

先确定要由 CC Switch 管理哪一个客户端，再按对应协议添加 Geekit Provider：

| 目标客户端   | 协议               | 地址                                |
| ------------ | ------------------ | ----------------------------------- |
| Codex CLI    | OpenAI Responses   | `https://geekit.top/v1`        |
| Claude Code  | Anthropic Messages | `https://geekit.top`           |
| Hermes Agent | 取决于 `api_mode`  | 参见 [Hermes Agent](hermes-agent.md) |

模型 ID 必须来自 Geekit 的 `/v1/models`，不要直接照搬客户端内置的模型名称。

## 添加 Geekit Provider

在 CC Switch 的 Provider 管理中创建自定义 Provider，并填写：

- 名称：`Geekit`
- API Key：Geekit 用户令牌
- Base URL：按上表选择
- 模型：Geekit 中真实可用的模型 ID

保存后，将该 Provider 应用到 Codex、Claude Code 或其他目标客户端。切换 Provider 后，关闭已有 CLI 会话并重新启动，避免进程继续使用旧环境变量或旧配置。

CC Switch 可以统一管理配置，但令牌仍属于敏感信息。不要假设所有版本都会对本地保存的 API Key 做静态加密；设备需要启用磁盘加密、账户锁屏和合理的文件权限。

## 什么时候启用本地转换

以下情况才需要协议转换：

- 客户端只发送 Anthropic Messages，但 Geekit 中准备使用的模型只适合 Responses。
- 客户端只发送 Responses，但目标模型只适合 Anthropic Messages。
- 已确认直接连接失败原因是协议不匹配，而不是地址、令牌或模型 ID 错误。

启用转换后，CC Switch 会成为本地请求链路的一部分。此时必须：

1. 保持 CC Switch 持续运行。
2. 开启总路由和目标应用的接管或路由选项。
3. 重新启动目标 CLI 会话。
4. 分别检查 CC Switch 路由记录和 Geekit 调用日志。

协议原本一致时不要开启转换。直接连接链路更短，也更容易定位流式事件、工具调用和错误响应。

## 验证

先在目标客户端执行一个最小只读任务，然后确认：

1. CC Switch 显示当前应用正在使用 Geekit Provider。
2. Geekit 调用日志中出现预期端点，例如 `/v1/responses` 或 `/v1/messages`。
3. 日志中的模型 ID 与 CC Switch 配置一致。
4. 流式输出和工具调用都能完成。

如果 `/v1/models` 自动发现失败，可以手动填写模型 ID；这不代表推理端点不可用。

## 恢复原配置

在 CC Switch 中切回原 Provider，并重新启动对应 CLI。若启用了本地转换，同时关闭总路由和应用接管，确认客户端不再指向 CC Switch 的本地地址。

## 官方资料

- [CC Switch 官方仓库](https://github.com/farion1231/cc-switch)
- [添加供应商](https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/zh/2-providers/2.1-add.md)
- [配置文件说明](https://github.com/farion1231/cc-switch/blob/main/docs/user-manual/zh/5-faq/5.1-config-files.md)
- [Claude 转 Responses 指南](https://github.com/farion1231/cc-switch/blob/main/docs/guides/claude-codex-routing-guide-zh.md)
- [Codex 转 Messages 指南](https://github.com/farion1231/cc-switch/blob/main/docs/guides/codex-claude-routing-guide-zh.md)
