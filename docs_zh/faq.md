---
description: 查找有关 Zoo Code 的常见问题解答，包括设置、使用、故障排除和高级功能。获取有关 API 密钥、模式等的帮助。
keywords:
  - Zoo Code 常见问题
  - 常见问题
  - 故障排除
  - API 设置
  - 自定义模式
  - MCP
  - 本地模型
---

import KangarooIcon from '@site/src/components/KangarooIcon';

# 常见问题解答

此页面解答了有关 Zoo Code 的一些常见问题。

---

## 通用

### 什么是 Zoo Code？

Zoo Code 是一套 AI 驱动的编码产品，旨在充分利用最先进的大型语言模型。

### Zoo Code 是如何工作的？

Zoo Code 使用大型语言模型 (LLM) 来理解您的请求并将其转化为操作。它可以：

- 读取和写入项目中的文件
- 执行 shell 命令
- 执行网页浏览（如果启用）
- 通过模型上下文协议 (MCP) 使用外部工具

您可以通过聊天界面（在扩展中）或通过不同渠道（在云端）与 Zoo Code 进行交互。

### Zoo Code 能做什么？

Zoo Code 可以帮助完成各种编码任务，包括：

*   根据自然语言描述生成代码。
*   重构现有代码。
*   修复错误。
*   编写文档。
*   解释代码。
*   回答有关代码库的问题。
*   自动化重复性任务。
*   创建新文件和项目。

### Zoo Code 可以免费使用吗？

Zoo Code 扩展是免费的，并且是[开源](https://github.com/RooCodeInc/Roo-Code/)的。
Zoo Code 云平台也是免费的，但某些功能是付费的。

在这两种情况下，Zoo Code 都依赖外部 LLM 推理提供商（如 [Anthropic](providers/anthropic)、[OpenAI](providers/openai)、[OpenRouter](providers/openrouter)、[Requesty](providers/requesty) 等）来实现其 AI 功能。
这些提供商通常根据处理的令牌数量对 API 使用收费。您需要创建一个帐户并从您选择的提供商处获取 API 密钥。了解更多[关于提供商以及如何设置它们](/providers/)的详细信息。

### 使用 Zoo Code 有什么风险？

Zoo Code 是一个强大的工具，负责任地使用它非常重要。以下是一些需要注意的事项：

*   **Zoo Code 可能会出错。** 在批准 Zoo Code 提出的更改之前，请仔细审查。
*   **Zoo Code 可以执行命令。** 谨慎允许 Zoo Code 运行命令，尤其是在使用自动批准时。
*   **Zoo Code 可以访问互联网。** 如果您使用的是支持网页浏览的提供商，请注意 Zoo Code 可能会访问敏感信息。

---

## 设置和安装

### 如何安装 Zoo Code？

请参阅[安装指南](/getting-started/installing)以获取详细说明。

### 支持哪些 API 提供商？

请参阅[完整列表](/providers/)。

### 如何获取 API 密钥？

每个 API 提供商都有自己的获取 API 密钥的流程。请参阅[设置您的第一个 AI 提供商](/getting-started/connecting-api-provider)以获取每个提供商的相关文档链接。

### 我可以将 Zoo Code 与本地模型一起使用吗？
是的，Zoo Code 支持使用 [Ollama](/providers/ollama) 和 [LM Studio](/providers/lmstudio) 在本地运行模型。请参阅[使用本地模型](/advanced-usage/local-models)以获取说明。

---

## 扩展使用

### 如何开始新任务？
打开 Zoo Code 面板 (<KangarooIcon />) 并在聊天框中输入您的任务。清楚地具体说明您希望 Zoo Code 做什么。请参阅[输入您的请求](/basic-usage/typing-your-requests)以获取最佳实践。

### Zoo Code 中的模式是什么？
[模式](/basic-usage/using-modes) 是 Zoo Code 可以采用的不同的角色，每个角色都有特定的关注点和一组功能。内置模式包括：

*   **Code（代码）：** 用于通用编码任务。
*   **Architect（架构师）：** 用于规划和技术领导。
*   **Ask（询问）：** 用于回答问题和提供信息。
*   **Debug（调试）：** 用于系统性故障诊断。
您还可以创建[自定义模式](/features/custom-modes)。

### 如何在模式之间切换？

使用聊天输入区域中的下拉菜单选择不同的模式，或使用 `/` 命令切换到特定模式。

### 什么是工具以及如何使用它们？
[工具](/basic-usage/how-tools-work) 是 Zoo Code 与您的系统交互的方式。Zoo Code 会自动选择并使用适当的工具来完成您的任务。您无需直接调用工具。系统会提示您批准或拒绝每次工具使用。

### 什么是上下文提及？
[上下文提及](/basic-usage/context-mentions) 是一种向 Zoo Code 提供有关项目的特定信息的方式，例如文件、文件夹或问题。使用 "@" 符号后跟您要提及的项目（例如，`@/src/file.ts`、`@problems`）。

### Zoo Code 可以访问互联网吗？

是的，如果您使用的是支持网页浏览的模型的提供商。请注意允许这样做的安全影响。

### Zoo Code 可以在我的终端中运行命令吗？

是的，Zoo Code 可以在您的 VS Code 终端中执行命令。除非您已为命令启用自动批准，否则系统会在执行每个命令之前提示您批准。对自动批准命令要格外谨慎。如果您在终端命令方面遇到问题，请参阅[Shell 集成指南](/features/shell-integration)进行故障排除。

### 如何向 Zoo Code 提供反馈？

您可以通过批准或拒绝 Zoo Code 提出的操作来提供反馈。您可以通过使用反馈字段提供额外的反馈。

### 我可以自定义 Zoo Code 的行为吗？

是的，您可以通过多种方式自定义 Zoo Code：

*   **自定义指令：** 提供适用于所有模式或特定模式的通用指令。
*   **自定义模式：** 创建具有定制提示和某些工具权限的自定义模式。
*   **`.roorules` 文件：** 在您的项目中创建 `.roorules` 文件以提供其他指南。
*   **设置：** 调整各种设置，例如自动批准、差异编辑等。

### Zoo Code 有任何自动批准设置吗？
是的，Zoo Code 有一些设置，启用后将自动批准操作。了解更多[此处](/features/auto-approving-actions)。

---

## 高级功能

### 我可以离线使用 Zoo 吗？
是的，如果您使用[本地模型](/advanced-usage/local-models)。

### 什么是 MCP（模型上下文协议）？
[MCP](/features/mcp/overview) 是一个协议，允许 Zoo Code 与外部服务器通信，通过自定义工具和资源扩展其功能。

### 我可以创建自己的 MCP 服务器吗？

是的，您可以创建自己的 MCP 服务器以向 Zoo Code 添加自定义功能。请参阅 [MCP 文档](https://github.com/modelcontextprotocol) 以获取详细信息。

### 什么是代码库索引？

[代码库索引](/features/codebase-indexing) 使用 AI 嵌入为您的项目创建语义搜索索引。这使得 Zoo Code 能够通过基于含义而非仅关键字查找相关代码，从而更好地理解和导航大型代码库。

### 代码库索引的成本是多少？

代码库索引需要 OpenAI API 密钥来生成嵌入，以及 Qdrant 向量数据库进行存储。成本取决于您的项目大小和使用的嵌入模型。初始索引是最昂贵的部分；后续更新是增量的，并且便宜得多。

---

## 故障排除

### Zoo Code 没有响应。我该怎么办？

*   确保您的 API 密钥正确且未过期。
*   检查您的互联网连接。
*   检查您选择的 API 提供商的状态。
*   尝试重新启动 VS Code。
*   如果问题仍然存在，请在 [GitHub](https://github.com/RooCodeInc/Roo-Code/issues) 或 [Discord](https://discord.gg/roocode) 上报告问题。

### 我看到一条错误消息。它是什么意思？

错误消息应提供有关问题的某些信息。如果您不确定如何解决，请在 [Discord](https://discord.gg/roocode) 中寻求帮助。

### Zoo Code 进行了我不想要的更改。如何撤销它们？

Zoo Code 使用 VS Code 内置的文件编辑功能。您可以使用标准的“撤销”命令 (Ctrl/Cmd + Z) 来还原更改。此外，如果启用了实验性检查点，Zoo 可以还原对文件所做的更改。

### Zoo Code 无法写入 markdown 文件。出了什么问题？

如果 Zoo Code 无法写入 `.md` 文件，并出现“无法打开差异编辑器”或“write_to_file 工具失败”等错误，这通常是由干扰文件编辑的 VS Code 扩展或设置引起的：

**常见原因：**
- 具有“保存时格式化”功能的扩展
- 默认在预览模式下打开 markdown 文件的 VS Code 设置
- Markdown Preview 扩展或类似的 markdown 处理扩展

**解决方案：**
- 禁用任何在保存时自动格式化文件的扩展
- 从 VS Code 的 `settings.json` 中删除以下设置：
  ```json
  "markdown.preview.openMarkdownLinks": "inPreview",
  "workbench.editorAssociations": {
    "*.md": "vscode.markdown.preview.editor"
  }
  ```
- 暂时禁用与 markdown 相关的扩展，以测试它们是否导致问题
- 进行这些更改后，重启 VS Code

### 如何报告错误或建议功能？

请在 Zoo Code 的 [Issues 页面](https://github.com/RooCodeInc/Roo-Code/issues) 和 [功能请求页面](https://github.com/RooCodeInc/Roo-Code/discussions/categories/feature-requests?discussions_q=is%3Aopen+category%3A%22Feature+Requests%22+sort%3Atop) 上报告错误或建议功能。
