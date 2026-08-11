---
sidebar_label: OpenAI
description: 将 Zoo Code 连接到 OpenAI 官方 API，以访问具有高级功能和详细程度控制的 GPT 和推理模型。
keywords:
  - OpenAI
  - GPT 模型
  - 推理模型
  - Zoo Code
  - AI 集成
  - API 密钥
  - 官方 OpenAI API
  - 详细程度
  - 推理努力
---

# 在 Zoo Code 中使用 OpenAI

Zoo Code 支持通过 OpenAI 官方 API 直接访问模型，包括最新的 GPT-5 系列，具有推理努力控制和详细程度设置等高级功能。

:::info 想要使用 ChatGPT Plus/Pro 订阅？
使用 **OpenAI – ChatGPT Plus/Pro** 提供商通过 OAuth 登录（无需 API 密钥）：[OpenAI – ChatGPT Plus/Pro](/providers/openai-chatgpt-plus-pro)。
:::

**网站：** [https://openai.com/](https://openai.com/)

---

## 获取 API 密钥

1.  **注册/登录：** 访问 [OpenAI 平台](https://platform.openai.com/)。创建账户或登录。
2.  **导航到 API 密钥：** 进入 [API 密钥](https://platform.openai.com/api-keys) 页面。
3.  **创建密钥：** 点击“创建新的秘密密钥”。为您的密钥指定一个描述性名称（例如，“Zoo Code”）。
4.  **复制密钥：** **重要：** 立即复制 API 密钥。您将无法再次查看它。请安全存储。

---

## 可用模型

Zoo Code 支持通过 OpenAI API 提供的所有模型。

有关完整、最新的模型列表和功能，请参阅 [OpenAI 的模型文档](https://platform.openai.com/docs/models)。

---

## 在 Zoo Code 中的配置

### 设置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“OpenAI”。
3.  **输入 API 密钥：** 将您的 OpenAI API 密钥粘贴到“OpenAI API 密钥”字段中。
4.  **选择模型：** 从“模型”下拉菜单中选择您所需的模型。
5.  **（可选）基础 URL：** 如果您需要使用自定义基础 URL，请输入 URL。大多数人无需调整此设置。

---

## 高级功能

### 推理努力控制

对于支持推理的模型（GPT-5、o1、o3、o4 系列），您可以控制模型思考的深度：

**GPT-5 模型：**
- `minimal` - 具有基本推理的最快响应
- `low` - 具有轻量推理的快速响应
- `medium`（默认）- 平衡的推理和响应时间
- `high` - 针对复杂问题的深度推理

**o1/o3/o4 模型：**
- `low` - 最小思考时间
- `medium` - 平衡方法
- `high` - 针对复杂问题的最大思考

某些模型具有无法更改的预设推理级别。

### 详细程度控制

GPT-5 模型和选定的其他模型可用，详细程度控制响应的详细级别：

- `low` - 简洁、直接的响应
- `medium`（默认）- 平衡的详细程度
- `high` - 全面、详细的响应

### 温度设置

温度控制输出的随机性（0.0 到 2.0）：

- **GPT-5 模型：** 默认 1.0，以实现平衡的创造力
- **其他模型：** 默认 0.0，以实现确定性输出
- **注意：** o1/o3 推理模型不可用

### 对话连续性 (GPT-5)

GPT-5 模型通过响应 ID 高效地维护对话上下文，在保留上下文的同时减少令牌使用量。这会自动发生 - 无需配置。

---

## 提示和注意事项

*   **定价：** 有关当前模型成本和折扣（包括提示缓存）的信息，请参阅 [OpenAI 定价](https://openai.com/pricing) 页面。
*   **Azure OpenAI 服务：** 如果您想使用 Azure OpenAI 服务，请参阅我们的 [OpenAI 兼容](/providers/openai-compatible) 提供商部分。
*   **上下文优化：** 对于 GPT-5-Codex，通过在请求之间保持一致的上下文来利用提示缓存，以显著降低成本。