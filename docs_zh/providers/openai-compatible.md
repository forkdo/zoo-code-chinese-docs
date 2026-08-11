---
sidebar_label: 兼容 OpenAI
description: 将 Zoo Code 与任何兼容 OpenAI API 标准的提供商（包括 Perplexity、Together AI、Anyscale 以及自定义端点）配合使用。
keywords:
  - 兼容 OpenAI
  - Zoo Code
  - API 集成
  - 自定义端点
  - Together AI
  - Perplexity
  - Anyscale
  - 模型配置
---

# 在 Zoo Code 中使用兼容 OpenAI 的提供商

Zoo Code 支持众多提供与 OpenAI API 标准兼容的 API 的 AI 模型提供商。这意味着您可以使用 *除* OpenAI 之外的提供商提供的模型，同时仍使用熟悉的 API 接口。这包括以下提供商：

*   **本地模型**：通过 Ollama 和 LM Studio 等工具运行的模型（在单独的部分中介绍）。
*   **云提供商**：如 Perplexity、Together AI、Anyscale、Modelis 等。
*   **任何其他提供商**：提供兼容 OpenAI API 端点的提供商。

本文档重点介绍如何设置 *除* 官方 OpenAI API 之外的提供商（官方 OpenAI API 有自己[专用的配置页面](/providers/openai)）。

---

## 通用配置

使用兼容 OpenAI 的提供商的关键在于配置两个主要设置：

1.  **基础 URL：** 这是提供商的 API 端点。它 *不会* 是 `https://api.openai.com/v1`（这是官方 OpenAI API 的地址）。
2.  **API 密钥：** 这是您从提供商处获得的密钥。
3.  **模型 ID：** 这是特定模型的名称。

您可以在 Zoo Code 设置面板中找到这些设置（点击 <Codicon name="gear" /> 图标）：

*   **API 提供商：** 选择“兼容 OpenAI”。
*   **基础 URL：** 输入您选择的提供商提供的基础 URL。**这至关重要。**
*   **API 密钥：** 输入您的 API 密钥。
*   **模型：** 选择一个模型。
*   **模型配置：** 这允许您自定义模型的高级配置
    - 最大输出令牌数
    - 上下文窗口
    - 图像支持
    - 计算机使用
    - 输入价格
    - 输出价格

---

## 原生工具调用

Zoo Code **仅**使用原生工具调用。这是唯一支持的工具协议——不存在基于 XML 的回退方案。

从高层次来看：

- **工具定义**使用 OpenAI 的原生工具架构发送给模型。
- **工具调用**作为专用工具事件流式传输回来，包括工具名称、参数和元数据。
- **工具参数**会逐步流式传输，这减少了模型决定使用工具与 Zoo Code 执行工具之间的延迟。

### 要求

要使原生工具调用正常工作，您选择的模型**必须**支持兼容 OpenAI 的工具调用。如果某个模型不支持原生工具调用，则无法在 Zoo Code 中使用。

### 示例：简单的原生工具流程

以下是使用 OpenAI 原生端点时，文件读取工具可能如何暴露的简化示例：

```json
{
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "read_file",
        "description": "从工作区读取带行号的文件。",
        "parameters": {
          "type": "object",
          "properties": {
            "path": { "type": "string", "description": "相对文件路径" },
            "start_line": { "type": "integer", "nullable": true },
            "end_line": { "type": "integer", "nullable": true }
          },
          "required": ["path"]
        }
      }
    }
  ]
}
```

当模型决定使用 `read_file` 时，Zoo Code 会在任务时间线中显示**流式传输的工具事件**：

- 一个原生*工具调用*事件，其中包含正在生成的工具名称和参数
- 相应的*工具结果*事件，显示文件内容以及任何截断或行范围信息

这使您可以获得关于正在使用哪些工具以及使用什么参数的更低延迟反馈。

### 限制

- **模型支持：** 并非所有模型都支持原生工具调用。如果模型不支持工具，则无法在 Zoo Code 中使用。请查阅您的提供商的文档，以确认您要使用的模型支持工具调用。
- **提供商特性：** 一些兼容 OpenAI 的提供商仅部分实现了原生工具 API。如果您遇到工具调用错误，请验证您的提供商是否完全支持兼容 OpenAI 的函数调用。

有关 Zoo Code 中工具工作原理的更深入概述，请参阅[工具使用概述](/advanced-usage/available-tools/tool-use-overview)。

---

## Azure OpenAI

Azure OpenAI 使用特定于部署的 URL 格式。使用以下值配置兼容 OpenAI 的提供商：

| 字段 | 值 |
| --- | --- |
| 基础 URL | `https://<resource>.openai.azure.com/openai` |
| API 密钥 | 来自 Azure AI Studio 的密钥 1 或密钥 2 |
| Azure 部署名称 | 您的部署名称，例如 `my-gpt4o-deployment` |
| 使用 Azure | 勾选，或从 `azure.com` URL 自动检测 |
| Azure API 版本 | 留空以使用 `2024-08-01-preview`，或输入特定版本 |

Azure AI Studio 中显示的资源端点以 `.openai.azure.com/` 结尾。当缺少 `/openai` 路径时，Zoo Code 会自动补全所需的 `/openai` 路径，因此资源端点和上面的完整基础 URL 都可以使用。

部署名称是用户自定义的，可以与底层模型名称不同。例如，如果您以名称 `brotherhood` 部署 `gpt-4o`，则应输入 `brotherhood`，而不是 `gpt-4o`。

**使用 Azure** 选项选择 Azure 基于部署的 API 格式。对于 `azure.com` URL，Zoo Code 会自动检测 Azure；当 Azure OpenAI 位于反向代理或其他主机名后面时，请手动启用此选项。

请求使用以下路径：

```text
https://<resource>.openai.azure.com/openai/deployments/<deployment-name>/chat/completions?api-version=<version>
```

---

## 故障排除

*   **“无效的 API 密钥”：** 仔细检查您输入的 API 密钥是否正确。
*   **“找不到模型”：** 确保您使用的是您选择的提供商的有效模型 ID。
*   **Azure OpenAI 的“404 找不到资源”：** 确保基础 URL 指向您的 Azure 资源，并且 Azure 部署名称与 Azure AI Studio 中配置的名称（而非底层模型名称）一致。Zoo Code 会自动为资源端点添加 `/openai`。
*   **连接错误：** 验证基础 URL 是否正确，以及您的提供商的 API 是否可访问。
*   **工具调用错误：** Zoo Code 需要原生工具调用。如果您的模型不支持，则需要切换到支持的模型。请查阅您的提供商的文档以了解工具调用的兼容性。
*   **意外结果：** 如果您获得意外结果，请尝试不同的模型。

通过使用兼容 OpenAI 的提供商，您可以利用 Zoo Code 的灵活性，与更广泛的 AI 模型一起使用。请务必始终查阅您的提供商的文档，以获取最准确和最新的信息。
