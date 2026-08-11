---
description: 在 Zoo Code 中配置 Requesty，以访问 150+ 个大型语言模型（LLM），并享受成本优化、统一计费和全面的使用情况跟踪功能。
keywords:
  - zoo code
  - requesty
  - ai provider
  - language models
  - api configuration
  - cost optimization
  - usage tracking
  - prompt caching
sidebar_label: Requesty
---

# 在 Zoo Code 中使用 Requesty

Zoo Code 支持通过 [Requesty](https://www.requesty.ai/) AI 平台访问模型。Requesty 提供了一个简单且优化的 API，用于与 150+ 个大型语言模型（LLM）进行交互。

**网站：** [https://www.requesty.ai/](https://www.requesty.ai/)

---

## 获取 API 密钥

1.  **注册/登录：** 访问 [Requesty 网站](https://www.requesty.ai/) 并创建账户或登录。
2.  **获取 API 密钥：** 您可以在 Requesty 仪表板的 [API 管理](https://app.requesty.ai/manage-api) 部分获取 API 密钥。

---

## 可用模型

Zoo Code 会自动从 Requesty 的 API 获取所有可用模型（来自多个提供商的 150+ 个模型）。

有关包含定价的完整、最新模型列表，请参阅 [Requesty 的模型列表](https://app.requesty.ai/router/list)。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“Requesty”。
3.  **输入 API 密钥：** 将您的 Requesty API 密钥粘贴到“Requesty API 密钥”字段中。
4.  **选择模型：** 从“模型”下拉菜单中选择您所需的模型。

---

## 提示和注意事项

- **优化：** Requesty 提供了一系列实时成本优化功能，以降低您的成本。
- **统一且简化的计费：** 通过单个 [API 密钥](https://app.requesty.ai/manage-api) 无限制访问所有提供商和模型，自动余额充值等功能。
- **成本跟踪：** 通过 [成本仪表板](https://app.requesty.ai/cost-management) 或 [Requesty VS.code 扩展](https://marketplace.visualstudio.com/items?itemName=Requesty.requesty) 跟踪每个模型、编程语言、更改文件等的成本。
- **统计和日志：** 查看您的 [编码统计仪表板](https://app.requesty.ai/usage-stats) 或浏览您的 [LLM 交互日志](https://app.requesty.ai/logs)。
- **故障转移策略：** 当提供商出现故障时，使用故障转移策略确保您的 LLM 继续为您服务。
* **提示缓存：** 某些提供商支持提示缓存。[搜索支持缓存的模型](https://app.requesty.ai/router/list)。

---

## 相关资源

- [Requesty Youtube 频道](https://www.youtube.com/@requestyAI)：
- [Requesty Discord](https://requesty.ai/discord)