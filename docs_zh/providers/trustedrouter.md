---
description: 在 Zoo Code 中通过兼容 OpenRouter 的提供方流程配置 TrustedRouter。
keywords:
  - zoo code
  - trustedrouter
  - 兼容 openrouter
  - ai 提供方
  - 语言模型
  - api 配置
sidebar_label: TrustedRouter
---

# 在 Zoo Code 中使用 TrustedRouter

TrustedRouter 提供兼容 OpenAI 的 API 用于模型路由。在 Zoo Code 中，选择 OpenRouter 提供方并设置自定义 base URL 即可连接它。

**网站：** [https://trustedrouter.com/](https://trustedrouter.com/)

---

## 获取 API 密钥

1. **登录：** 前往 [TrustedRouter 控制台](https://trustedrouter.com/console/api-keys)。
2. **创建 API 密钥：** 为 Zoo Code 创建一个新的密钥。
3. **复制密钥：** 妥善保管。原始密钥仅显示一次。

---

## 在 Zoo Code 中的配置

1. **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标（<Codicon name="gear" />）。
2. **选择提供方：** 从"API Provider"下拉菜单中选择"OpenRouter"。
3. **输入 API 密钥：** 将你的 TrustedRouter API 密钥粘贴到"OpenRouter API Key"字段。
4. **启用自定义 Base URL：** 勾选"Use custom base URL"并输入：

   ```text
   https://api.trustedrouter.com/v1
   ```

5. **选择模型：** 输入一个 TrustedRouter 模型 ID，例如：

   ```text
   trustedrouter/auto
   trustedrouter/zdr
   trustedrouter/e2e
   trustedrouter/synth
   ```

---

## 提示与说明

- **模型 ID：** TrustedRouter 支持路由别名，如 `trustedrouter/auto`、`trustedrouter/zdr` 和 `trustedrouter/e2e`，以及其模型目录中的直接模型 ID。
- **定价：** 用量由 TrustedRouter 根据所选的下游模型和路由计费。
- **验证：** TrustedRouter 在 [https://trust.trustedrouter.com/](https://trust.trustedrouter.com/) 发布网关证明详情。
