---
sidebar_label: Z AI
description: 在 Zoo Code 中配置 Z AI 模型。通过区域感知路由访问 GLM 系列模型，支持国际用户和中国大陆用户。
keywords:
  - z ai
  - zai
  - zhipu ai
  - glm models
  - zoo code
  - api provider
  - china ai
  - international ai
  - openai compatible
---

# 在 Zoo Code 中使用 Z AI

Z AI (Zhipu AI) 提供先进的 GLM 系列语言模型。该提供商提供区域感知路由，为国际用户和中国大陆用户提供独立的端点。

**网站:** [https://z.ai/model-api](https://z.ai/model-api) (国际) | [https://open.bigmodel.cn/](https://open.bigmodel.cn/) (中国大陆)

---

## 获取 API 密钥

### 国际用户

1. **注册/登录:** 访问 [https://z.ai/model-api](https://z.ai/model-api)。创建账户或登录。
2. **导航至 API 密钥:** 访问您的账户仪表板，找到 API 密钥部分。
3. **创建密钥:** 为您的应用程序生成一个新的 API 密钥。
4. **复制密钥:** **重要:** 立即复制 API 密钥并安全存储。

### 中国大陆用户

1. **注册/登录:** 访问 [https://open.bigmodel.cn/](https://open.bigmodel.cn/)。创建账户或登录。
2. **导航至 API 密钥:** 访问您的账户仪表板，找到 API 密钥部分。
3. **创建密钥:** 为您的应用程序生成一个新的 API 密钥。
4. **复制密钥:** **重要:** 立即复制 API 密钥并安全存储。

---

## 可用模型

Zoo Code 会根据您选择的区域自动从 Z AI 的 API 获取所有可用模型。

有关完整、最新的模型列表和规格，请参阅官方提供商文档：
- **国际:** [Z AI 模型文档](https://z.ai/model-api)
- **中国大陆:** [BigModel 文档](https://open.bigmodel.cn/)

---

## 在 Zoo Code 中的配置

1. **打开 Zoo Code 设置:** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2. **选择提供商:** 从“API 提供商”下拉菜单中选择“Z AI”。
3. **选择区域:** 选择您的区域：
   - “国际”（默认）用于全球访问
   - “中国”用于中国大陆访问
4. **输入 API 密钥:** 将您的 Z AI API 密钥粘贴到“Z AI API 密钥”字段中。
5. **选择模型:** 从“模型”下拉菜单中选择您所需的模型。可用模型取决于您选择的区域。

### 默认值和行为
- **自动基础 URL:** 所选区域会自动确定 API 端点：
  - 国际 → `https://api.z.ai/api/paas/v4`
  - 中国 → `https://open.bigmodel.cn/api/paas/v4`
- **动态模型:** 更改区域会自动更新模型目录和目标端点。
- **无需手动设置基础 URL:** 您通常无需配置自定义基础 URL。

---

## 提示和注意事项

* **区域选择:** 区域设置决定了 API 端点和可用模型：
  - 国际: 使用 `https://api.z.ai/api/paas/v4`
  - 中国: 使用 `https://open.bigmodel.cn/api/paas/v4`
* **自动基础 URL:** 基础 URL 会根据您的区域自动选择；在典型设置中无需手动覆盖。
* **OpenAI 兼容性:** Z AI 使用与 OpenAI 兼容的 API，提供流式响应和使用情况报告。
* **模型选择:** 模型会根据您选择的区域自动过滤，以确保兼容性。
* **需要 API 密钥:** 所有请求都需要有效的 API 密钥。确保您已从相应的区域平台获取。
* **定价:** 请查看各自的区域网站以获取当前定价信息。