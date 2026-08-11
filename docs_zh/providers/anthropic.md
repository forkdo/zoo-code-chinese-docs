---
sidebar_label: Anthropic
description: 在 Zoo Code 中配置 Anthropic 的 Claude AI 模型。使用 Claude Opus、Sonnet 和 Haiku 模型，支持提示缓存和大型上下文窗口。
keywords:
  - anthropic
  - claude
  - claude ai
  - roo code
  - api provider
  - claude opus
  - claude sonnet
  - claude haiku
  - prompt caching
  - ai models
---

# 在 Zoo Code 中使用 Anthropic

Anthropic 是一家专注于 AI 安全和研究的公司，致力于构建可靠、可解释且可控的 AI 系统。他们的 Claude 模型以其强大的推理能力、实用性和诚实性而闻名。

**官网：** [https://www.anthropic.com/](https://www.anthropic.com/)

---

## Getting an API Key

1.  **注册/登录：** 访问 [Anthropic 控制台](https://console.anthropic.com/)。创建账户或登录。
2.  **导航到 API 密钥：** 进入 [API 密钥](https://console.anthropic.com/settings/keys) 部分。
3.  **创建密钥：** 点击“创建密钥”。为您的密钥起一个描述性名称（例如“Zoo Code”）。
4.  **复制密钥：**  **重要：** 请*立即*复制 API 密钥。您将无法再次查看它。请妥善保管。

---

## 可用模型

Zoo Code 支持通过 Anthropic API 提供的所有 Claude 模型。

有关完整且最新的模型列表和功能，请参阅 [Anthropic 的模型文档](https://docs.anthropic.com/en/docs/about-claude/models)。

---

## 在 Zoo Code 中配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“Anthropic”。
3.  **输入 API 密钥：** 将您的 Anthropic API 密钥粘贴到“Anthropic API 密钥”字段中。
4.  **选择模型：** 从“模型”下拉菜单中选择您所需的 Claude 模型。
5.  **（可选）自定义基础 URL：** 如果您需要使用 Anthropic API 的自定义基础 URL，请勾选“使用自定义基础 URL”并输入 URL。大多数人无需调整此设置。

---

## 提示和注意事项

*   **提示缓存：** Claude 模型支持[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)，可以显著降低重复提示的成本和延迟。
*   **上下文窗口：** Claude 模型拥有大型上下文窗口（200,000 个 token），允许您在提示中包含大量代码和上下文。
*   **定价：** 有关最新定价信息，请参阅 [Anthropic 定价](https://www.anthropic.com/pricing) 页面。
*   **速率限制：** Anthropic 根据[使用层级](https://docs.anthropic.com/en/api/rate-limits#requirements-to-advance-tier)设置了严格的速率限制。如果您频繁遇到速率限制，请考虑联系 Anthropic 销售团队或通过其他提供商（如 [OpenRouter](/providers/openrouter) 或 [Requesty](/providers/requesty)）访问 Claude。
