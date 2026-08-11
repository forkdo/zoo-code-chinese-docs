---
sidebar_label: Google Gemini
description: 在 Zoo Code 中使用 Google 的 Gemini AI 模型。为您的开发工作流配置 Gemini Flash、Pro 和实验性模型。
keywords:
  - google gemini
  - gemini ai
  - roo code
  - api provider
  - gemini flash
  - gemini pro
  - google ai
  - gemini models
  - ai studio
---

# 在 Zoo Code 中使用 Google Gemini

Zoo Code 通过 Google AI Gemini API 支持 Google 的 Gemini 模型系列。

**官方网站:** [https://ai.google.dev/](https://ai.google.dev/)

---

## 获取 API 密钥

1.  **访问 Google AI Studio:** 导航至 [https://ai.google.dev/](https://ai.google.dev/)。
2.  **登录:** 使用您的 Google 账户登录。
3.  **创建 API 密钥:** 点击左侧菜单中的"创建 API 密钥"。
4.  **复制 API 密钥:** 复制生成的 API 密钥。

---

## 可用模型

Zoo Code 支持通过 Google API 提供的所有 Gemini 模型，并自动跟踪 Google 最新的稳定版本发布。

有关完整且最新的模型列表和功能，请参阅 [Google 的 Gemini 模型文档](https://ai.google.dev/models/gemini)。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置:** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商:** 从"API 提供商"下拉菜单中选择"Google Gemini"。
3.  **输入 API 密钥:** 将您的 Gemini API 密钥粘贴到"Gemini API 密钥"字段中。
4.  **选择模型:** 从"模型"下拉菜单中选择您所需的 Gemini 模型。

默认情况下，Zoo Code 会在您的提供商支持的情况下选择一个稳定的 Pro 模型，温度设置为 **1.0**。这使得建议更具表现力和自然性，同时仍能保持任务相关性。如果您需要高度确定性的输出（例如，在 CI 中进行代码生成），您可以将温度降低至 `0.0`。

---

## 高级功能

### URL 上下文

Gemini 模型现在可以通过 URL 上下文直接访问和分析网页内容。此功能允许 Zoo 做到：

- 实时读取和理解网页
- 分析来自 URL 的文档
- 审查在线代码仓库
- 访问网站的最新信息

#### 启用 URL 上下文

1. 打开 Zoo Code 设置
2. 导航至 Gemini 提供商设置
3. 启用"URL 上下文"选项
4. 保存您的设置

#### 使用示例

```
请分析 https://example.com/api-docs 处的文档，并根据 API 规范创建一个 TypeScript 客户端库。
```

### Google 搜索 grounding

启用 Google 搜索 grounding 功能，以通过实时搜索结果增强 Gemini 的响应。这提供了：

- 来自网络搜索的最新信息
- 事实核查能力
- 当前事件意识
- 技术查询的准确性增强

#### 启用搜索 grounding

1. 打开 Zoo Code 设置
2. 导航至 Gemini 提供商设置
3. 启用"Google 搜索 grounding"选项
4. 保存您的设置

#### 使用示例

```
2025 年 React Server Components 的最新最佳实践是什么？请搜索最新信息。
```

### 组合使用

这两个功能可以一起使用，以实现强大的工作流：

```
搜索最新的 Node.js 安全漏洞，然后分析我的 package.json 文件，看看我是否受到影响。同时检查官方 Node.js 安全页面以获取建议。
```

---

## 提示和注意事项

*   **定价:**  Gemini API 的使用费用基于输入和输出 token 数量。URL 上下文和搜索 grounding 可能会产生额外费用。某些实验性模型可免费使用。有关详细信息，请参阅 [Gemini 定价页面](https://ai.google.dev/pricing)。
*   **模型选择:** 根据您的需求选择模型：
    - **Flash 模型:** 对于大多数任务来说更快且更具成本效益
    - **Pro 模型:** 更适合复杂推理和分析
    - **Thinking 模型:** 最适合需要逐步推理的任务（需要推理预算）
    - **实验性模型:** 最新功能，可能免费但稳定性较差
*   **上下文窗口:** 大多数 Gemini 模型支持高达 1,048,576 个 token 的大型上下文窗口，允许进行广泛的代码分析和文档处理。
*   **速率限制:** URL 上下文和搜索 grounding 功能可能有单独的速率限制。监控您的使用情况，以避免达到限制。
*   **隐私:** 使用 URL 上下文时，请注意访问私有或敏感的 URL。确保您有权分析内容。
*   **搜索质量:** Google 搜索 grounding 功能最适合使用具体、格式良好的查询。请明确说明您需要什么信息。
