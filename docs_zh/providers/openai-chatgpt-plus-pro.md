---
sidebar_label: ChatGPT Plus/Pro
title: ChatGPT Plus/Pro
description: 使用您的 ChatGPT Plus/Pro 订阅在 Zoo Code 中调用 OpenAI 模型（通过 OAuth 登录，无需 API 密钥）。
keywords:
  - OpenAI Codex
  - ChatGPT Plus
  - ChatGPT Pro
  - Zoo Code
  - OAuth
  - no api key
  - subscription
---

<div style={{ position: 'relative', paddingBottom: '56.25%', height: 0, overflow: 'hidden' }}>
  <iframe
    src="https://www.youtube.com/embed/c1IXRMl5i0g?rel=0&modestbranding=1"
    title="OpenAI – ChatGPT Plus/Pro provider setup"
    style={{
      position: 'absolute',
      top: 0,
      left: 0,
      width: '100%',
      height: '100%',
    }}
    frameBorder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowFullScreen
  ></iframe>
</div>

---

## 快速入门：将您的订阅连接到 Zoo Code

1. 打开 Zoo Code 设置（点击 Zoo Code 面板中的齿轮图标 <Codicon name="gear" />）。
2. 在 **API Provider** 中选择 **OpenAI – ChatGPT Plus/Pro**。
3. 点击 **Sign in to OpenAI Codex**。
4. 在浏览器中完成登录流程。
5. 返回 Zoo Code 设置，从下拉菜单中选择一个模型。
6. 保存设置。

## 提示与注意事项

- **需要订阅：** 您必须拥有有效的 ChatGPT Plus 或 Pro 订阅。此提供程序不支持免费的 ChatGPT 账户。有关更多信息，请参阅 [OpenAI 的 ChatGPT 计划](https://openai.com/chatgpt/pricing)。
- **无额外 API 费用：** 通过此提供程序的使用将计入您的 ChatGPT 订阅额度，不会产生额外的 API 费用。
- **退出登录：** 要断开连接，请在提供程序设置中使用“Sign Out”按钮。

## 您无法做的事（及原因）

- **无法使用任意的 OpenAI API 模型。** 此提供程序仅支持 Zoo 的 Codex 模型目录中列出的模型。
- **无法通过设置导出功能导出或迁移登录状态。** OAuth 令牌存储在 VS Code SecretStorage 中，该内容不会包含在 Zoo 的设置导出中。
---