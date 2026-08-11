---
title: Baseten
sidebar_label: Baseten
description: 了解如何在 Zoo Code 中配置和使用 Baseten 的模型 API。通过企业级性能、可靠性和有竞争力的价格访问前沿开源模型。
keywords:
  - Baseten
  - Model APIs
  - open-source models
  - DeepSeek
  - Kimi K2
  - Qwen
  - Zoo Code
  - AI integration
  - API key
  - enterprise inference
---

# 在 Zoo Code 中使用 Baseten

Baseten 提供专为生产应用（而不仅仅是实验）设计的按需前沿模型 API。这些 API 基于 Baseten 推理栈构建，为来自 OpenAI、DeepSeek、Moonshot AI 和阿里云的领先开源模型提供优化的推理服务。

**官网：** [https://www.baseten.co/products/model-apis/](https://www.baseten.co/products/model-apis/)

---

## 获取 API 密钥

1. **注册/登录：** 访问 [Baseten](https://www.baseten.co/) 并创建账户或登录。

2. **导航至 API 密钥：** 进入您的仪表板，访问 [https://app.baseten.co/settings/api_keys](https://app.baseten.co/settings/api_keys) 的 API 密钥部分。

3. **创建密钥：** 生成一个新的 API 密钥。为其起一个描述性名称（例如 "Zoo Code"）。

4. **复制密钥：** 立即复制 API 密钥并安全存储。

---

## 可用模型

Zoo Code 支持通过 Baseten 模型 API 提供的所有模型。

有关完整且最新的模型列表和定价信息，请参阅 [Baseten 模型 API 页面](https://www.baseten.co/products/model-apis/)。

---

## 在 Zoo Code 中配置

1. **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。

2. **选择提供商：** 从 "API Provider" 下拉菜单中选择 "Baseten"。

3. **输入 API 密钥：** 将您的 Baseten API 密钥粘贴到 "Baseten API Key" 字段中。

4. **选择模型：** 从 "Model" 下拉菜单中选择您所需的模型。

:::warning Kimi K2 思维模型
要使用 `moonshotai/Kimi-K2-Thinking` 模型，您必须在 Zoo Code 设置中启用原生工具调用。此设置允许 Zoo Code 通过模型的原生工具处理器调用工具，这是该推理模型正常运行所必需的。
:::

---

## 提示和注意事项

- **定价：** 有关当前定价信息，请参阅 [Baseten 模型 API 页面](https://www.baseten.co/products/model-apis/)。
