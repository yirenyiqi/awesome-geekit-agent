# LobsterAI

LobsterAI 是网易有道维护的开源桌面 Agent。Geekit 自定义地址优先使用 OpenAI Chat Completions；Claude 模型可以使用 Anthropic Messages。

核验日期：2026-07-27

交付状态：站内镜像待核验

## 1. 配置永久环境变量

先在 [API Keys](https://geekit.top/keys) 生成令牌，并按[快速开始第一步](../getting-started.md)永久设置 `GEEKIT_API_KEY`。本页使用以下三个参数：

- **`<Geekit 地址>`**：`https://geekit.top`。
- **`GEEKIT_API_KEY`**：变量名保持不变，变量值替换为生成的真实 Geekit 令牌。
- **`<Geekit 模型 ID>`**：从[模型广场](https://geekit.top/pricing)选择当前可用的最新模型；Codex 类和 Claude 类模型都以模型广场显示的当前可用 ID 为准。

图形客户端不能直接读取环境变量时，只把 `GEEKIT_API_KEY` 的值填入客户端受保护的 API Key 字段，不要写入项目文件。

**站内镜像待核验**

LobsterAI 当前版本暂未提供站内镜像。请只从本页末尾的官方仓库获取安装包；独立校验和与平台发布者身份全部核验后，本站才会开放下载命令。

## 前置条件

- 已从 [LobsterAI 官方仓库](https://github.com/netease-youdao/LobsterAI)安装或更新可使用自定义模型的版本。
- 已取得 Geekit 根地址、用户令牌和模型 ID。
- 已确认模型支持任务需要的工具、图片或推理能力。

## 使用 OpenAI Chat

在 LobsterAI 的模型或 Provider 设置中添加自定义 Provider：

| 字段          | 填写值                       |
| ------------- | ---------------------------- |
| Provider 名称 | `Geekit`                     |
| API 格式      | OpenAI                       |
| Base URL      | `https://geekit.top/v1` |
| API Key       | Geekit 用户令牌              |
| 模型          | Geekit 中真实可用的模型 ID   |

对非 OpenAI 官方域名的自定义 OpenAI URL，LobsterAI 通常映射为 Chat Completions。保存后应在 Geekit 日志中看到 `/v1/chat/completions`。

## 使用 Anthropic Messages

Claude 模型可以新增 Anthropic 格式 Provider：

| 字段     | 填写值                             |
| -------- | ---------------------------------- |
| API 格式 | Anthropic                          |
| Base URL | `https://geekit.top`          |
| API Key  | Geekit 用户令牌                    |
| 模型     | Geekit 中真实可用的 Claude 模型 ID |

Messages 的 Base URL 使用 Geekit 根地址，不要追加 `/v1`。

## 关于 Responses

自定义 Geekit URL 不会自动选择 Responses。只有当前客户端界面或高级配置明确允许选择 `openai-responses` 时才启用；否则继续使用 Chat Completions。

不要仅因为模型名称包含 Codex 或 GPT，就假设客户端已经改用 Responses。以 Geekit 调用日志中的实际端点为准。

## 验证

保存模型后，新建会话执行一个只读最小任务，并检查：

1. LobsterAI 当前模型为 Geekit Provider 下的模型。
2. Geekit 日志命中 `/v1/chat/completions` 或 `/v1/messages`。
3. 流式输出能正常结束。
4. 文件、浏览器或其他工具调用符合模型真实能力。

LobsterAI 是本地桌面客户端，但模型请求仍会发送到所填 Geekit 地址。敏感文件是否离开设备取决于具体任务、工具和模型请求内容，不能把“本地客户端”等同于“内容永不出机”。

## 常见问题

### 返回 `404`

OpenAI 格式使用 `/v1` Base URL；Anthropic 格式使用根地址。检查是否重复拼接 `/v1`。

### 图片或工具不可用

检查模型设置中的能力声明，并确认模型确实支持相应输入和工具协议。不要一次性开启所有能力。

### 修改后仍请求旧地址

重新选择模型并新建会话；必要时重启 LobsterAI，再通过 Geekit 日志确认实际请求地址。

## 官方资料

- [LobsterAI 官方仓库](https://github.com/netease-youdao/LobsterAI)
- [Provider 配置类型](https://github.com/netease-youdao/LobsterAI/blob/main/src/shared/providers/types.ts)
