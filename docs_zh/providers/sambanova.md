---
sidebar_label: SambaNova
description: 在 Zoo Code 中配置 SambaNova 的高速 AI 模型。体验企业级推理性能与多样化的模型选择所带来的卓越表现。
keywords:
  - sambanova
  - sambanova ai
  - zoo code
  - api provider
  - high-speed inference
  - enterprise ai
  - llm provider
  - fast inference
---

# 在 Zoo Code 中使用 SambaNova

SambaNova 专注于通过其 SambaCloud 门户利用可重构数据流单元（RDUs）为大型语言模型提供高速推理，从而实现对支持模型的快速响应。

**官网：** [https://cloud.sambanova.ai/](https://cloud.sambanova.ai/)

---

## 获取 API 密钥

要在 Zoo Code 中使用 SambaNova，您需要从 [SambaCloud](https://cloud.sambanova.ai?utm_source=zoocode&utm_medium=external&utm_campaign=cloud_signup) 获取一个 API 密钥。注册后，请在左侧边栏中进入 API Keys 部分，创建并复制您的 SambaCloud API 密钥。

---

## 可用模型

Zoo Code 会自动从 SambaNova API 获取所有可用模型。

有关完整且最新的模型列表及其功能，请参阅 [SambaCloud 的支持模型文档](https://docs.sambanova.ai/cloud/docs/get-started/supported-models)。

---

## 在 Zoo Code 中的配置

1. **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2. **选择提供商：** 从“API Provider”下拉菜单中选择 “SambaNova”。
3. **输入 API 密钥：** 将您的 SambaNova API 密钥粘贴到“SambaNova API Key”字段中。
4. **选择模型：** 从“Model”下拉菜单中选择您所需的模型。
5. **（可选）自定义基础 URL：** 如果使用私有部署，请勾选“Use custom base URL”并输入您的端点 URL。