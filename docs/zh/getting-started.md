# 首次调用 API

在 [API Keys](https://geekit.top/keys) 创建令牌，在[模型广场](https://geekit.top/pricing/models)选择支持 Chat Completions 的模型。以下示例向该模型发送一条消息。

## 配置永久环境变量

将下面三行写入个人设备的 `~/.zshrc` 或 `~/.bashrc`，替换令牌和模型 ID，再打开新终端。

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<你的 Geekit API Key>"
export GEEKIT_MODEL="<模型 ID>"
```

使用 PowerShell 时，将变量保存到当前用户，然后重新打开终端。

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<你的 Geekit API Key>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "<模型 ID>", "User")
```

## 发送消息

```bash
curl "$GEEKIT_BASE_URL/v1/chat/completions" \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"model\":\"$GEEKIT_MODEL\",\"messages\":[{\"role\":\"user\",\"content\":\"你好！\"}]}"
```

PowerShell 用户可直接运行：

```powershell
$body = @{ model = $env:GEEKIT_MODEL; messages = @(@{ role = "user"; content = "Hello!" }) } | ConvertTo-Json -Depth 4
Invoke-RestMethod "$env:GEEKIT_BASE_URL/v1/chat/completions" -Method Post -ContentType "application/json" -Headers @{ Authorization = "Bearer $env:GEEKIT_API_KEY" } -Body $body
```

从 `choices[0].message.content` 读取回复，从 `usage` 查看 Token 用量。若模型不可用，请用同一令牌查询[模型列表](https://geekit.top/docs/api/models)。要逐段接收回复，请参阅[流式响应与错误](https://geekit.top/docs/guides/streaming-and-errors)。
