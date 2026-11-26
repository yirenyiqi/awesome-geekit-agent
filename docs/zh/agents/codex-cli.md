# Codex CLI

Codex CLI 是 Geekit 首选接入方式之一。自定义 Provider 使用 OpenAI Responses 协议，适合代码理解、编辑、命令执行和长任务。

核验日期：2026-07-29

**需要桌面管理界面**

如果你使用 Geekit 客户端，并希望通过图形界面管理 Provider、会话、上下文窗口和增强功能，请查看 Geekit 客户端随附的使用说明。Codex CLI 用户继续按本页配置即可。

## 1. 配置永久环境变量

先在 [API Keys](https://geekit.top/keys) 页面生成令牌。`GEEKIT_API_KEY` 是固定的环境变量名，它的值必须替换为生成的真实 Geekit 令牌。默认模型优先使用[模型广场](https://geekit.top/pricing)当前可用的最新版本，本教程使用 `gpt-6-astra`。

将以下内容写入 `~/.zshrc`（Zsh）或 `~/.bashrc`（Bash）：

```bash
export GEEKIT_BASE_URL="https://geekit.top"
export GEEKIT_API_KEY="<生成的真实 Geekit 令牌>"
export GEEKIT_MODEL="gpt-6-astra"
```

保存后重新打开终端，或执行 `source ~/.zshrc` / `source ~/.bashrc`。PowerShell 使用用户级永久环境变量：

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", "https://geekit.top", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", "<生成的真实 Geekit 令牌>", "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", "gpt-6-astra", "User")
```

设置后关闭并重新打开 PowerShell。`GEEKIT_BASE_URL` 是 Geekit 根地址，不带 `/v1`。如果 `gpt-6-astra` 不在你的可用模型中，请将永久环境变量 `GEEKIT_MODEL` 和后续配置里的 `model` 一起替换为模型广场展示的最新模型 ID。

## 2. 安装 Codex CLI

如果可以访问 OpenAI 官网，也可以按 [OpenAI 官方 Codex CLI 安装教程](https://developers.openai.com/codex/cli/)完成安装或更新。

**Geekit 站内镜像安装**

无法直连官方发布源时，可以从 Geekit 的受保护软件下载站安装。下载请求必须携带第一步配置的 `GEEKIT_API_KEY`；脚本会选择当前平台的已核验版本，并验证签名和 SHA256。

macOS / Linux：

```bash
curl -fsSL \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  https://s3.geekit.top/install/codex/install.sh | sh
```

Windows PowerShell：

```powershell
$installer = Join-Path ([IO.Path]::GetTempPath()) ("geekit-codex-" + [Guid]::NewGuid() + ".ps1")
try {
  Invoke-RestMethod `
    -Headers @{ Authorization = "Bearer $env:GEEKIT_API_KEY" } `
    -Uri "https://s3.geekit.top/install/codex/install.ps1" `
    -OutFile $installer `
    -MaximumRedirection 0
  & $installer
}
finally {
  Remove-Item -LiteralPath $installer -ErrorAction SilentlyContinue
}
```

该方式可用于首次安装和更新。安装脚本和程序文件都从 `s3.geekit.top` 获取，不再跳转到国外下载站；候选版本未通过完整校验时，线上仍保留上一可用版本。

安装后执行 `codex --version` 确认命令可用。随后继续配置 Geekit Provider；使用 Geekit 时无需登录 OpenAI，也无需配置 OpenAI API Key。

开始配置前还需确认：

- 已取得 Geekit 根地址、用户令牌和支持 Responses 的模型 ID。
- 所选模型支持流式事件、工具调用和足够的上下文。

验证 Responses：

```bash
curl "$GEEKIT_BASE_URL/v1/responses" \
  -H "Authorization: Bearer $GEEKIT_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{
    \"model\": \"$GEEKIT_MODEL\",
    \"input\": \"只回复：连接成功\"
  }"
```

## 3. 配置 Provider

编辑用户级 `~/.codex/config.toml`：

```toml
model_provider = "geekit"
model = "gpt-6-astra"

[model_providers.geekit]
name = "Geekit"
base_url = "https://geekit.top/v1"
env_key = "GEEKIT_API_KEY"
wire_api = "responses"
```

配置中只记录环境变量名，不要写真实令牌。`geekit` 是自定义 Provider ID，不要覆盖 Codex 内置 Provider。

## 4. 启动与选择模型

使用配置中的默认模型启动：

```bash
codex
```

临时指定另一个 Geekit 模型：

```bash
codex -m "$GEEKIT_MODEL"
```

进入 Codex 后先执行一个只读最小任务，例如：

```text
读取当前目录的 README，只概括项目用途，不修改文件。
```

## 5. 验证连接

同时检查：

1. Codex 能收到完整或流式响应。
2. 它可以读取工作区并在授权后调用工具。
3. Geekit 调用日志出现 `/v1/responses` 请求。
4. 日志中的模型、Token 和状态符合预期。
5. 若当前客户端版本实际发起服务端压缩，请求没有出现 `/v1/responses/compact` 路由错误；没有该请求不算失败。

只有普通文本成功但工具调用失败时，优先更换明确支持 Responses 工具调用的模型。

## 常见问题

### 请求发到 Chat Completions

检查 `wire_api = "responses"`。当前 Codex 自定义 Provider 应使用 Responses，不要配置已移除的 Chat wire API。

### 返回 `401`

确认变量名与 `env_key` 完全一致：

```bash
test -n "$GEEKIT_API_KEY" && echo "GEEKIT_API_KEY 已设置"
```

不要直接打印变量值。

### 返回 `404`

`base_url` 应为 `https://geekit.top/v1`。Codex 会继续拼接 `/responses`，不要把完整 `/v1/responses` 填到 `base_url`。

### 工具或流式事件异常

确认模型原生支持 Responses。仅支持 Chat Completions 的模型即使能返回文本，也不保证兼容 Codex 的事件、推理和工具流程。

## WebSocket 与服务端压缩

自定义 Provider 默认使用 HTTP/SSE。不要在 Geekit 和模型完成兼容测试前启用 Responses WebSocket。当前 Codex 自定义 Provider 不一定会调用远程 `/v1/responses/compact`，因此不能把该端点是否出现作为连接成功条件；它只供明确实现服务端压缩流程的客户端使用。

## 恢复官方 Provider

退出 Codex，移除或注释顶层的 `model_provider = "geekit"` 与自定义默认模型选择，保留 `[model_providers.geekit]` 备用也可以。随后按 OpenAI 官方登录流程选择默认 Provider。

Zsh / Bash 用户从 shell 配置文件中删除 `GEEKIT_BASE_URL`、`GEEKIT_API_KEY` 和 `GEEKIT_MODEL` 三行，再重新打开终端。需要同时清理当前终端时执行：

```bash
unset GEEKIT_BASE_URL
unset GEEKIT_API_KEY
unset GEEKIT_MODEL
```

PowerShell 用户删除用户级永久变量：

```powershell
[Environment]::SetEnvironmentVariable("GEEKIT_BASE_URL", $null, "User")
[Environment]::SetEnvironmentVariable("GEEKIT_API_KEY", $null, "User")
[Environment]::SetEnvironmentVariable("GEEKIT_MODEL", $null, "User")
```

完成后重新打开终端。

## 官方资料

- [Codex CLI 安装与使用](https://developers.openai.com/codex/cli/)
- [Codex 高级配置](https://developers.openai.com/codex/config-file/config-advanced)
- [Codex 基础配置](https://developers.openai.com/codex/config-file/config-basic)
- [Codex Provider 协议实现](https://github.com/openai/codex/blob/main/codex-rs/model-provider-info/src/lib.rs)
- [Codex 配置 Schema](https://github.com/openai/codex/blob/main/codex-rs/core/config.schema.json)
