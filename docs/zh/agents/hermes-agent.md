# Hermes Agent

Hermes Agent 的自定义 Provider 支持 Chat Completions、OpenAI Responses 和 Anthropic Messages。通用模型优先从 Chat Completions 开始；Codex 类模型使用 Responses，Claude 模型使用 Messages。

核验日期：2026-07-27

交付状态：站内镜像待核验

## 1. 配置永久环境变量

先在 [API Keys](https://geekit.top/keys) 生成令牌，并按[快速开始第一步](../getting-started.md)永久设置 `GEEKIT_API_KEY`。本页使用以下三个参数：

- **`<Geekit 地址>`**：`https://geekit.top`。
- **`GEEKIT_API_KEY`**：变量名保持不变，变量值替换为生成的真实 Geekit 令牌。
- **`<Geekit 模型 ID>`**：从[模型广场](https://geekit.top/pricing)选择当前可用的最新模型；Codex 类和 Claude 类模型都以模型广场显示的当前可用 ID 为准。

图形客户端不能直接读取环境变量时，只把 `GEEKIT_API_KEY` 的值填入客户端受保护的 API Key 字段，不要写入项目文件。

**站内镜像待核验**

Hermes Agent 当前版本暂未提供站内镜像。请只使用本页末尾的官方资料完成安装；本站核验发布者身份和安装包后才会开放下载命令。

## 前置条件

- 已按 [Hermes Agent 官方仓库](https://github.com/NousResearch/hermes-agent)的安装说明完成安装，并可正常启动。
- 已取得 Geekit 根地址、用户令牌和模型 ID。
- 模型能够可靠调用工具，且上下文长度满足 Agent 任务需要。

将令牌保存在用户级 `~/.hermes/.env`，不要写入项目目录或可分享配置：

```dotenv
GEEKIT_API_KEY=<生成的真实 Geekit 令牌>
```

限制该文件仅当前用户可读写：

```bash
chmod 600 ~/.hermes/.env
```

## 配置 Chat Completions

编辑 `~/.hermes/config.yaml`：

```yaml
custom_providers:
  - name: geekit
    base_url: https://geekit.top/v1
    key_env: GEEKIT_API_KEY
    api_mode: chat_completions
    model: <Geekit 模型 ID>

model:
  provider: custom:geekit
  default: <Geekit 模型 ID>
```

`name` 决定 `custom:geekit` 中的 Provider ID。`key_env` 只填写环境变量名，不要直接放入令牌。

## 选择其他协议

| 模型协议                | `api_mode`           | `base_url`                   |
| ----------------------- | -------------------- | ---------------------------- |
| OpenAI Chat Completions | `chat_completions`   | `https://geekit.top/v1` |
| OpenAI Responses        | `codex_responses`    | `https://geekit.top/v1` |
| Anthropic Messages      | `anthropic_messages` | `https://geekit.top`    |

Generic URL 自动检测通常会回落到 Chat Completions。Responses 和 Messages 应显式设置 `api_mode`，不要只修改模型名称。

## 验证

按原方式启动 Hermes Agent，先执行一个只读最小任务，然后确认：

1. Hermes 选择了 `custom:geekit` Provider。
2. Geekit 调用日志中的端点与 `api_mode` 一致。
3. 模型 ID、Token 和状态符合预期。
4. 工具调用能继续执行，而不是只输出工具名称或 JSON 文本。

主模型使用 Geekit 后，视觉理解、网页摘要等辅助任务仍可能走独立路由。若希望这些任务跟随主模型，检查对应辅助设置是否保持为官方建议的 `auto`；同时确认 Geekit 模型确实具备相应能力。

## 常见问题

### 返回 `401`

确认 `~/.hermes/.env` 中存在 `GEEKIT_API_KEY`，并重新启动 Hermes。不要通过终端回显或日志打印令牌进行排查。

### 返回 `404`

检查 Messages 是否误用了带 `/v1` 的 Base URL，或 Chat、Responses 是否漏掉 `/v1`。

### Responses 被当成 Chat

将 `api_mode` 明确设为 `codex_responses`。仅填写自定义 URL 不会自动选择 Responses。

## 恢复原 Provider

将 `model.provider` 和 `model.default` 改回原配置，重新启动 Hermes。确认不再使用 Geekit 后，再按需要移除 `custom_providers` 中的条目和用户级令牌变量。

## 官方资料

- [Hermes Agent Provider 文档](https://hermes-agent.nousresearch.com/docs/integrations/providers)
- [Hermes Agent 官方仓库](https://github.com/NousResearch/hermes-agent)
- [三种 API 模式实现](https://github.com/NousResearch/hermes-agent/blob/main/hermes_cli/main.py)
