# Reasonix

Reasonix 提供终端与 Studio 桌面编程助手，当前版本支持自定义 OpenAI 兼容 Provider。

资料核对日期为 2026-11-26。本页按当前官方 CLI 与配置路径文档整理。

## 安装与准备

从官方仓库 Releases 安装适合当前平台的 CLI 或 Studio，执行 `reasonix --version` 确认可用。下载与安装说明见 [Reasonix 官方仓库](https://github.com/esengine/reasonix)。

按[快速开始](../getting-started.md)获取 Geekit 根地址、API Key 和支持 Chat Completions 与工具调用的模型 ID。

## 配置 CLI

```bash
reasonix setup
```

在 Provider 管理向导中添加 OpenAI-compatible Provider。

| 项目 | 填写值 |
| --- | --- |
| Provider | `geekit` |
| Base URL | `https://geekit.top/v1` |
| API Key 环境变量名 | `GEEKIT_API_KEY` |
| API Key | 生成的 Geekit 令牌值 |
| 默认模型 | 从 Geekit 模型列表复制的真实 ID |

通过向导保存密钥并测试连接，刷新模型列表，选择默认模型，最后选择 Save and exit。

当前 Reasonix 将 Provider 凭据统一保存到用户级 Reasonix home 中的 `.env`。macOS / Linux 默认是 `~/.reasonix/.env`，Windows 默认是 `%APPDATA%\reasonix\.env`。向导会写入该文件。CLI 与桌面端共享配置和凭据。

## 配置 Studio

在 Studio 的 Provider 设置中添加 OpenAI 兼容服务。如果字段显示 API address，填写完整请求地址 `https://geekit.top/v1/chat/completions`。当前桌面端将该字段作为精确请求 URL 保存。

## 首次运行与验证

```bash
cd /path/to/my-project
reasonix
```

使用 `/model` 选择 Geekit 模型，以 `/status` 检查当前设置，执行一个项目摘要任务。在 Geekit 日志中核对 `/v1/chat/completions`、模型 ID、Token 和工具调用。

模型列表为空时可手动添加模型 ID。鉴权失败时使用向导重新保存凭据，再查阅[错误码](../error-codes.md)。

## 参考资料

- [Reasonix CLI 参考](https://github.com/esengine/reasonix/blob/main/docs/CLI.md)
- [Reasonix 配置与凭据路径](https://github.com/esengine/reasonix/blob/main/docs/CONFIG_PATHS.md)
- [DeepSeek 的 Reasonix 接入说明](https://api-docs.deepseek.com/zh-cn/quick_start/agent_integrations/reasonix/)
