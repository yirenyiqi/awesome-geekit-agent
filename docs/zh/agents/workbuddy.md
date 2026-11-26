# WorkBuddy / CodeBuddy

Tencent WorkBuddy 支持通过“自定义 API”添加模型。接入 Geekit 时优先使用标准 OpenAI Chat Completions；只有当前客户端要求完整端点时，才启用自定义协议。

资料核对日期为 2026-11-26。

交付状态：国内官方源

## 1. 配置永久环境变量

先在 [API Keys](https://geekit.top/keys) 生成令牌，并按[快速开始第一步](../getting-started.md)永久设置 `GEEKIT_API_KEY`。本页使用以下三个参数：

- **`<Geekit 地址>`**：`https://geekit.top`。
- **`GEEKIT_API_KEY`**：变量名保持不变，变量值替换为生成的真实 Geekit 令牌。
- **`<Geekit 模型 ID>`**：从[模型广场](https://geekit.top/pricing)选择当前可用的最新模型；Codex 类和 Claude 类模型都以模型广场显示的当前可用 ID 为准。

图形客户端不能直接读取环境变量时，只把 `GEEKIT_API_KEY` 的值填入客户端受保护的 API Key 字段，不要写入项目文件。

**国内官方源**

Tencent WorkBuddy 可直接访问国内官方入口，本站不重复分发安装包。请从本页末尾的腾讯官方资料安装或更新。

## 前置条件

- 已从 [WorkBuddy 官方入口](https://copilot.tencent.com/work)安装或更新支持自定义 API 的客户端并登录。
- 已取得 Geekit 根地址、用户令牌和模型 ID。
- 所选模型支持 Chat Completions 和任务需要的工具能力。

## 标准配置

在 WorkBuddy 的模型设置中添加“自定义 API”，保持“自定义协议”关闭，并填写：

| 字段    | 填写值                       |
| ------- | ---------------------------- |
| URL     | `https://geekit.top/v1` |
| API Key | Geekit 用户令牌              |
| 模型名  | Geekit 中真实可用的模型 ID   |

标准模式会在基础地址后补全 Chat Completions 路径。保存前检查地址末尾只有一个 `/v1`。

## 使用完整端点

如果当前 WorkBuddy 版本明确要求完整请求地址，开启“自定义协议”，并填写：

```text
https://geekit.top/v1/chat/completions
```

自定义协议会直接请求所填 URL，不再自动补全路径。不要在标准模式中填写完整端点，否则可能出现重复的 `/chat/completions`。

## WorkBuddy / CodeBuddy 本地模型配置

先安装并登录客户端，至少打开一次项目目录，然后编辑用户级 `~/.codebuddy/models.json`。Windows 对应 `C:\Users\<用户名>\.codebuddy\models.json`。项目专用配置可放在 `<项目目录>/.codebuddy/models.json`。

按[快速开始](../getting-started.md)设置用户级永久变量 `GEEKIT_API_KEY`，将以下 JSON 合并到现有配置，并将两处 `<Geekit model ID>` 替换为同一个支持工具调用的真实模型 ID。

```json
{
  "models": [
    {
      "id": "<Geekit model ID>",
      "name": "Geekit",
      "vendor": "Geekit",
      "url": "https://geekit.top/v1/chat/completions",
      "apiKey": "${GEEKIT_API_KEY}",
      "maxInputTokens": 32768,
      "maxOutputTokens": 4096,
      "supportsToolCall": true,
      "supportsImages": false
    }
  ],
  "availableModels": ["<Geekit model ID>"]
}
```

示例使用 32768 输入与 4096 输出 Token 上限，并关闭图片输入。根据模型广场公布的上下文、输出与图片能力调整这些字段。

文件保存为 UTF-8 无 BOM，完全退出并重新启动客户端，在模型选择器中选择 Geekit。桌面端需要继承 `GEEKIT_API_KEY`，环境变量引用仍以字面量显示时，从已加载变量的终端重新启动客户端，或通过客户端的 API Key 设置保存令牌。

## 验证连接

先使用 WorkBuddy 自带的连接检查，再新建会话执行一个只读最小任务。随后在 Geekit 调用日志中确认：

1. 请求命中 `/v1/chat/completions`。
2. 模型 ID 与 WorkBuddy 配置一致。
3. 返回状态和 Token 统计正常。
4. 流式输出、工具调用和文件任务符合模型实际能力。

WorkBuddy 官方页面没有给出所有模型的流式与工具调用兼容矩阵。连接成功只说明协议和凭证可用，不代表每个模型都能完成全部桌面任务。

## 常见问题

### 返回 `404`

检查“自定义协议”开关与 URL 是否匹配：关闭时填 `/v1` 基础地址，开启时填完整 `/v1/chat/completions`。

### 返回 `401`

重新粘贴 Geekit 用户令牌，确认没有前后空格。不要把令牌放进截图、项目文件或工单正文。

### 模型列表中找不到目标模型

WorkBuddy 可能不会自动同步 Geekit 模型列表。直接填写 `/v1/models` 返回的准确模型 ID。

## 官方资料

- [Tencent WorkBuddy 模型配置](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Model)
- [腾讯 WorkBuddy 产品页](https://cloud.tencent.com/product/workbuddy)
- [WorkBuddy 官方入口](https://copilot.tencent.com/work)

- [DeepSeek 的 WorkBuddy / CodeBuddy 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/workbuddy/)
