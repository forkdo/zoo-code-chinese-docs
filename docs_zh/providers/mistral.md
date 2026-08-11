---
sidebar_label: Mistral AI
description: 配置 Zoo Code 使用 Mistral AI 模型，包括用于代码生成的 Codestral，支持函数调用和视觉功能。
keywords:
  - Mistral AI
  - Codestral
  - Zoo Code
  - AI models
  - code generation
  - Pixtral
  - Ministral
  - function calling
  - La Plateforme
---

# 在 Zoo Code 中使用 Mistral AI

Zoo Code 支持通过 Mistral AI API 访问模型，包括标准 Mistral 模型和专为代码设计的 Codestral 模型。

**官网:** [https://mistral.ai/](https://mistral.ai/)

---

## 获取 API 密钥

1.  **注册/登录:** 访问 [Mistral Platform](https://console.mistral.ai/)。创建账户或登录。您可能需要完成验证流程。
2.  **创建 API 密钥:**  
    - [La Plateforme API Key](https://console.mistral.ai/api-keys/) 和/或 
    - [Codestral API Key](https://console.mistral.ai/codestral)

---

## 可用模型

Zoo Code 支持 Mistral AI API 提供的所有模型。

有关完整且最新的模型列表和功能，请参阅 [Mistral 的模型文档](https://docs.mistral.ai/getting-started/models/models_overview/)。

---

## 在 Zoo Code 中配置

1.  **打开 Zoo Code 设置:** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商:** 从“API Provider”下拉菜单中选择“Mistral”。
3.  **输入 API 密钥:** 如果您使用的是 `mistral` 模型，请将您的 Mistral API 密钥粘贴到“Mistral API Key”字段中。如果您打算使用 `codestral-latest`，请参阅下面的“Codestral”部分。
4.  **选择模型:** 从“Model”下拉菜单中选择您所需的模型。

---

## 使用 Codestral

[Codestral](https://docs.mistral.ai/capabilities/code_generation/) 是专为代码生成和交互设计的模型。
只有 Codestral 可以使用不同的端点（默认：codestral.mistral.ai）。
对于 La Plateforme API 密钥，请将 **Codestral Base Url** 更改为：https://api.mistral.ai

要使用 Codestral：

1.  **选择“Mistral”作为 API 提供商。**
2.  **选择一个 Codestral 模型**
3.  **输入您的 Codestral (codestral.mistral.ai) 或 La Plateforme (api.mistral.ai) API 密钥。**