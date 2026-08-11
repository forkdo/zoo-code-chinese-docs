---
description: 在 Zoo Code 中配置 OpenRouter，通过单一 API 访问来自不同提供商的 100 多种语言模型，支持自动模型发现。
keywords:
  - zoo code
  - openrouter
  - ai provider
  - language models
  - api configuration
  - model selection
  - prompt caching
  - byok
sidebar_label: OpenRouter
---

# 在 Zoo Code 中使用 OpenRouter

OpenRouter 是一个 AI 平台，可通过单一 API 访问来自不同提供商的多种语言模型。这可以简化设置流程，并让您轻松尝试不同的模型。

**网站：** [https://openrouter.ai/](https://openrouter.ai/)

---

## Getting an API Key

1. **注册/登录：** 访问 [OpenRouter 网站](https://openrouter.ai/)，使用您的 Google 或 GitHub 账户登录。
2. **获取 API 密钥：** 转到 [密钥页面](https://openrouter.ai/keys)。您应该会看到一个列出的 API 密钥。如果没有，请创建一个新的密钥。
3. **复制密钥：** 复制该 API 密钥。

---

## 可用模型

Zoo Code 会自动从 OpenRouter 的 API 获取所有可用模型（来自各种提供商的 100 多种模型）。

如需查看完整且最新的模型列表（包含定价和功能信息），请访问 [OpenRouter 的模型页面](https://openrouter.ai/models)。

---

## 在 Zoo Code 中的配置

1. **打开 Zoo Code 设置：** 在 Zoo Code 面板中点击齿轮图标 (<Codicon name="gear" />)。
2. **选择提供商：** 从“API Provider”下拉菜单中选择“OpenRouter”。
3. **输入 API 密钥：** 将您的 OpenRouter API 密钥粘贴到“OpenRouter API Key”字段中。
4. **选择模型：** 从“Model”下拉菜单中选择您需要的模型。
5. **（可选）自定义基础 URL：** 如果您需要为 OpenRouter API 使用自定义基础 URL，请勾选“Use custom base URL”并输入 URL。大多数用户可留空此字段。

---

## 提示与注意事项

* **模型选择：** OpenRouter 提供多种模型。建议您尝试不同模型，以找到最适合您需求的选项。
* **定价：** OpenRouter 的费用基于所使用底层模型的定价。详情请参见 [OpenRouter 模型页面](https://openrouter.ai/models)。
* **Prompt 缓存：**
    * OpenRouter 会将缓存请求传递给支持该功能的底层模型。请查看 [OpenRouter 模型页面](https://openrouter.ai/models) 以了解哪些模型支持缓存。
    * 对于大多数模型，如果底层模型本身支持缓存，缓存将自动激活（类似于 Requesty 的工作方式）。
    * **支持 prompt 缓存的模型包括：**
        * Anthropic Claude Sonnet 3.5、3.7
        * Anthropic Claude Haiku 3.5
        * **Anthropic Claude Haiku 4.5**（新增）
        * Google Gemini 模型（需手动激活 - 请参见下文）
    * **通过 OpenRouter 使用 Gemini 模型的例外情况：** 由于通过 OpenRouter 访问 Google 缓存机制时可能出现响应延迟，因此针对 Gemini 模型需要手动激活步骤。
    * 如果您使用的是 **Gemini 模型** 且通过 OpenRouter 接入，**必须手动勾选**提供商设置中的“Enable Prompt Caching”选项以启用缓存。此复选框为临时解决方案。对于非 Gemini 模型，无需勾选此选项即可启用缓存。
* **自带密钥（BYOK）：** 如果您使用自己在底层服务中的密钥，OpenRouter 的费用将仅为正常费用的 5%。Zoo Code 会自动调整成本计算以反映这一优惠。
