# 其他 Agent 兼容性

本页记录尚未进入正式教程的产品边界。只有官方资料能够确认自定义 Provider 字段、请求协议和用户权限时，Geekit 才会发布可复现的接入步骤。

核验日期：2026-07-27

## 状态总览

| 产品                  | 当前结论                                                    | 用户建议             |
| --------------------- | ----------------------------------------------------------- | -------------------- |
| ZCode                 | 支持 OpenAI 或 Anthropic 兼容供应商，但 OpenAI 子协议未明确 | 仅实验性试用         |
| StepClaw              | 未找到稳定的官方第三方 Provider 配置契约                    | 暂不接入             |
| Coze 托管版           | 未证实普通用户可填写任意 Base URL                           | 不提供直连教程       |
| Coze Studio           | 可由平台管理员配置模型服务                                  | 联系实例管理员       |
| Dify、FastGPT         | 支持兼容模型服务，但属于工作区或平台管理                    | 不进入用户主线       |
| DeepThink、AgentScope | 自托管平台或开发框架                                        | 后续开发者文档再讨论 |

## ZCode

ZCode 官方文档确认可以添加兼容 OpenAI 或 Anthropic 协议的自定义供应商，字段包括供应商名称、Base URL 和 API Key，也可以自动加载或手动添加模型。

当前官方资料只写“OpenAI 兼容”，没有明确固定使用 Chat Completions 还是 Responses。因此 Geekit 暂不发布正式步骤。需要自行试用时：

1. 使用测试令牌，不使用高额度或长期令牌。
2. OpenAI 兼容入口可先填写 `https://geekit.top/v1`。
3. 手动选择一个已确认支持 Chat Completions 的模型。
4. 通过 Geekit 日志确认实际请求端点、流式结果和工具调用。

如果日志命中未支持的端点或工具调用结构异常，应停止使用，不要靠反复修改路径猜测生产配置。

## StepClaw

阶跃 AI 已提供 StepClaw 相关入口，但截至核验日期，没有找到由官方稳定维护、能够确认任意第三方 Base URL、API Key 和协议选择的用户文档。

因此本站不采信“原生支持任意 OpenAI 兼容接口、Ollama 或所有国产 API”等二手说法，也不提供不可复现的表单步骤。待官方发布配置契约或完成真实客户端验证后再补充。

## Coze 与 Coze Studio

Coze Studio 开源版支持由平台管理员管理模型服务。这属于自托管实例的管理能力，不等于 Coze 托管版普通用户可以添加任意 Geekit Base URL。

- 普通 Coze 用户：暂不提供 Geekit 直连教程。
- 已有 Coze Studio 实例的普通用户：联系实例管理员添加模型。
- Coze Studio 管理员：属于管理端文档范围，不在公开用户文档展示配置步骤。

## 自托管平台与开发框架

Dify、FastGPT、DeepThink 和 AgentScope 都能以不同方式接入兼容模型服务，但配置通常涉及工作区所有者、平台管理员、容器网络或代码开发。它们不属于开箱即用的用户侧 Agent，本轮公开文档不展开。

如果你的组织已经统一配置这些平台，普通用户只需向管理员索取可用模型名称和使用入口，不应接触上游密钥或管理端 Provider 配置。

## 判断新客户端是否可接入

新增客户端至少需要同时满足：

1. 官方允许普通用户填写自定义 Base URL 和 API Key。
2. 官方明确请求协议是 Chat Completions、Responses 或 Messages。
3. 可以指定 Geekit 中实际存在的模型 ID。
4. 能通过客户端测试和 Geekit 调用日志交叉验证。

只写“OpenAI 兼容”但没有子协议说明时，应先按实验性接入处理。

## 官方资料

- [ZCode 官方站](https://zcode.z.ai/cn)
- [ZCode 连接模型文档](https://zcode.z.ai/cn/docs/configuration)
- [阶跃 AI](https://chat.stepfun.com)
- [阶跃星辰官网](https://www.stepfun.com)
- [阶跃星辰开放平台](https://platform.stepfun.com)
- [Coze Studio 官方仓库](https://github.com/coze-dev/coze-studio)
- [Dify 官方仓库](https://github.com/langgenius/dify)
- [FastGPT 官方仓库](https://github.com/labring/FastGPT)
- [DeepThink 官方仓库](https://github.com/AIGeniusInstitute/deepthink)
- [AgentScope 官方仓库](https://github.com/agentscope-ai/agentscope)
