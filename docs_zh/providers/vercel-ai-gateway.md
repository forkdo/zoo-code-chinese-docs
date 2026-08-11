---
description: 在 Zoo Code 中配置 Vercel AI Gateway，通过统一接口稳定访问来自多个提供商的 100 多种语言模型。
keywords:
  - zoo code
  - vercel ai gateway
  - ai provider
  - language models
  - api configuration
  - model selection
  - prompt caching
  - usage tracking
  - byok
sidebar_label: Vercel AI Gateway
---

# 在 Zoo Code 中使用 Vercel AI Gateway

AI Gateway 提供了一个统一的 API，通过单一端点访问数百种模型。它使您能够设置预算、监控使用情况、负载均衡请求以及管理故障转移。

实用链接：
- 团队仪表板：https://vercel.com/d?to=%2F%5Bteam%5D%2F%7E%2Fai
- 模型目录：https://vercel.com/ai-gateway/models
- 文档：https://vercel.com/docs/ai-gateway

---

## 获取 API 密钥

身份验证需要一个 API 密钥。

1.  **注册/登录：** 访问 [Vercel 网站](https://vercel.com/) 并登录。
2.  **获取 API 密钥：** 进入 AI Gateway 选项卡中的 [API 密钥页面](https://vercel.com/d?to=%2F%5Bteam%5D%2F%7E%2Fai%2Fapi-keys&title=AI+Gateway+API+Key)。创建一个新密钥。
3.  **复制密钥：** 复制 API 密钥。

---

## 可用模型

Zoo Code 会自动从 Vercel AI Gateway 的 API 获取所有可用模型（来自多个提供商的数百种模型）。

有关包含功能的完整、最新模型目录，请参阅 [Vercel AI Gateway 模型页面](https://vercel.com/ai-gateway/models)。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“Vercel AI Gateway”。
3.  **输入 API 密钥：** 将您的 Vercel AI Gateway API 密钥粘贴到“Vercel AI Gateway API 密钥”字段中。
4.  **选择模型：** 从“模型”下拉菜单中选择您所需的模型。

---

## 提示缓存
Vercel AI Gateway 支持对选定的模型（包括 Anthropic Claude 和 OpenAI GPT 模型）进行自动提示缓存。这通过缓存频繁使用的提示来降低成本。

---

## 提示和注意事项

* **模型选择：** Vercel AI Gateway 提供广泛的模型选择。请尝试找到最适合您需求的模型。
* **定价：** Vercel AI Gateway 根据底层模型的定价收费，包括缓存提示的成本。有关详细信息，请参阅 [Vercel AI Gateway 模型页面](https://vercel.com/ai-gateway/models)。
* **温度：** 默认温度为 `0.7`，每个模型均可配置。
* **自带密钥 (BYOK)：** 如果您决定使用自己底层服务的密钥，Vercel AI Gateway **不收取额外费用**。
* **更多信息：** Vercel 不添加速率限制。上游提供商可能会添加。新账户每 30 天可获得 $5 的信用额度，直到首次付款。