---
description: 在 Zoo Code 中配置 Kenari，通过一个兼容 OpenAI 的网关以印尼盾（Rupiah）计费访问 Claude、GPT、DeepSeek、GLM、Kimi 等模型。
keywords:
  - zoo code
  - kenari
  - ai 提供方
  - 兼容 openai
  - 印尼盾
  - 印度尼西亚
  - 语言模型
  - api 配置
sidebar_label: Kenari
---

# 在 Zoo Code 中使用 Kenari

Zoo Code 支持通过 [Kenari](https://kenari.id) 访问模型，这是一个印度尼西亚的、兼容 OpenAI 的 AI 网关，以印尼盾（IDR）计费。一个 `kn-` API 密钥即可通过单一端点访问 Claude、GPT、DeepSeek、GLM、Kimi 等模型。

**网站：** [https://kenari.id](https://kenari.id)

---

## 获取 API 密钥

1.  **注册/登录：** 前往 [kenari.id](https://kenari.id) 创建账户或登录。
2.  **创建密钥：** 在仪表板中打开 **API keys** 并点击 **Create key**。密钥以 `kn-` 为前缀，且仅显示一次，请立即复制。一个密钥适用于所有模型。

---

## 可用模型

Zoo Code 会自动从 Kenari 公开的 `/v1/models` 端点获取可用模型，因此列表会保持最新，无需更新扩展。

价格以印尼盾列在 [Kenari 目录](https://kenari.id/docs) 中，而非显示在 Zoo Code 内部，因为 Kenari 以印尼盾计费，而扩展显示的是美元价格。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标（<Codicon name="gear" />）。
2.  **选择提供方：** 从"API Provider"下拉菜单中选择"Kenari"。
3.  **输入 API 密钥：** 将你的 `kn-` 密钥粘贴到"Kenari API Key"字段。
4.  **选择模型：** 从"Model"下拉菜单中选择你想要的模型。

---

## 提示与说明

- **一个密钥，多种模型：** 单个 `kn-` 密钥通过同一端点即可访问 Claude、GPT、DeepSeek、GLM、Kimi 等模型。
- **印尼盾计费：** 用量以印尼盾计量和计费，无需外币信用卡。
- **兼容 OpenAI 和 Anthropic：** Kenari 同时提供 OpenAI 的 `/chat/completions` 和 Anthropic 的 `/messages` 接口形态，因此现有工具无需改动即可使用。

---

## 相关资源

- [Kenari 文档](https://kenari.id/docs)
- [OpenAPI 规范](https://kenari.id/openapi.json)
