---
description: 在 Zoo Code 中配置 xAI 的 Grok 模型。使用 Grok-4、Grok-3、Grok-2 和视觉模型，享受大上下文窗口、推理能力和提示缓存功能。
keywords:
  - xAI
  - Grok
  - Zoo Code
  - AI models
  - reasoning AI
  - vision models
  - large context
  - Grok Code Fast
  - Grok-4
  - Grok-3
  - Grok-2
  - prompt caching
sidebar_label: xAI (Grok)
---

# 在 Zoo Code 中使用 xAI (Grok)

xAI 是 Grok 背后的公司，Grok 是一个以对话能力和大上下文窗口而闻名的大型语言模型。Grok 模型旨在提供有用、信息丰富且与上下文相关的响应。

**网站：**[https://x.ai/](https://x.ai/)

---

## 获取 API 密钥

1.  **注册/登录：**访问 [xAI 控制台](https://console.x.ai/)。创建账户或登录。
2.  **导航到 API 密钥：**在仪表板中进入 API 密钥部分。
3.  **创建密钥：**点击创建新的 API 密钥。为您的密钥起一个描述性名称（例如“Zoo Code”）。
4.  **复制密钥：****重要：**立即复制 API 密钥。您将无法再次查看它。请安全存储。

---

## 可用模型

Zoo Code 支持通过 xAI 的 API 提供的所有 Grok 模型。

有关完整、最新的模型列表和功能，请参阅 [xAI 文档](https://docs.x.ai/docs)。

---

## 在 Zoo Code 中配置

1.  **打开 Zoo Code 设置：**点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：**从“API 提供商”下拉菜单中选择“xAI”。
3.  **输入 API 密钥：**将您的 xAI API 密钥粘贴到“xAI API 密钥”字段中。
4.  **选择模型：**从“模型”下拉菜单中选择您所需的 Grok 模型。

---

## 推理能力

Grok 3 Mini 模型具有专门的推理能力，允许它们在“响应前思考”——这对于复杂的问题解决任务特别有用。

### 支持推理的模型

多个 Grok 模型具有推理能力。然而，只有 Grok 3 Mini 模型支持可配置的推理努力控制：

* `grok-3-mini` - 支持推理努力控制
* `grok-3-mini-fast` - 支持推理努力控制

其他模型（`grok-code-fast-1`、`grok-4.1-fast`、`grok-4`、`grok-3`、`grok-3-fast`）具备推理能力，但不暴露 `reasoning_effort` 参数。

### 控制推理努力

使用支持推理的模型时，您可以通过 `reasoning_effort` 参数控制模型的思考强度：

* `low`：最小思考时间，使用更少的 token 以快速响应
* `high`：最大思考时间，利用更多 token 解决复杂问题

对于应快速完成的简单查询，选择 `low`；对于响应延迟不那么重要的较难问题，选择 `high`。

### 关键特性

* **逐步问题解决**：模型在给出答案之前有条理地思考问题
* **数学与定量优势**：擅长数值挑战和逻辑谜题
* **推理轨迹访问**：模型的思考过程可通过响应完成对象中的 `reasoning_content` 字段获得

---

## 提示缓存

提示缓存适用于选定的 Grok 模型，包括 `grok-code-fast-1`、`grok-4`、`grok-3`、`grok-3-fast`、`grok-3-mini` 和 `grok-3-mini-fast`。此功能可以降低成本并提高响应时间。

---

## 定价

定价因模型而异。有关当前定价信息，请参阅 [xAI 文档](https://console.x.ai/)。