---
sidebar_label: LM Studio
description: 了解如何设置和使用 LM Studio 与 Zoo Code 配合运行本地语言模型，并支持 OpenAI 兼容 API。
keywords:
  - LM Studio
  - local models
  - Zoo Code
  - AI integration
  - GGUF models
  - CodeLlama
  - Mistral
  - DeepSeek
  - local inference
---

# 在 Zoo Code 中使用 LM Studio

Zoo Code 支持使用 LM Studio 在本地运行模型。LM Studio 提供了一个用户友好的界面，用于下载、配置和运行本地语言模型。它还包含一个内置的本地推理服务器，可模拟 OpenAI API，从而轻松与 Zoo Code 集成。

**网站：**[https://lmstudio.ai/](https://lmstudio.ai/)

---

## 设置 LM Studio

1.  **下载并安装 LM Studio：** 从 [LM Studio 网站](https://lmstudio.ai/) 下载 LM Studio。
2.  **下载模型：** 使用 LM Studio 界面搜索并下载 GGUF 格式的模型。您可以在 LM Studio 搜索界面或 [Hugging Face](https://huggingface.co/models?library=gguf) 上浏览所有可用模型。
3.  **启动本地服务器：**
    *   打开 LM Studio。
    *   点击 **“Local Server”** 选项卡（图标看起来像 `<->`）。
    *   选择您下载的模型。
    *   点击 **“Start Server”**。

---

## 在 Zoo Code 中配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API Provider”下拉菜单中选择“LM Studio”。
3.  **输入模型 ID：** 输入您在 LM Studio 中加载的模型的*文件名*（例如 `codellama-7b.Q4_0.gguf`）。您可以在 LM Studio 的“Local Server”选项卡中找到此信息。
4.  **（可选）Base URL：** 默认情况下，Zoo Code 将连接到 `http://localhost:1234` 上的 LM Studio。如果您已将 LM Studio 配置为使用其他地址或端口，请在此处输入完整的 URL。

---

## 提示和注意事项

*   **资源要求：** 在本地运行大型语言模型可能会消耗大量资源。请确保您的计算机满足所选模型的最低要求。
*   **模型选择：** LM Studio 提供了广泛的模型。请尝试找到最适合您需求的模型。
*   **本地服务器：** 必须运行 LM Studio 本地服务器，Zoo Code 才能连接到它。
*   **LM Studio 文档：** 有关更多信息，请参阅 [LM Studio 文档](https://lmstudio.ai/docs)。
*   **故障排除：** 如果您看到“请检查 LM Studio 开发者日志以调试出错原因”错误，则可能需要调整 LM Studio 中的上下文长度设置。
*   **Token 跟踪：** Zoo Code 会跟踪通过 LM Studio 运行的模型的 token 使用情况，帮助您监控消耗量。
*   **推理支持：** 对于支持的模型，Zoo Code 可以解析 LM Studio 响应中的“think”标签或类似的推理指示符，从而更深入地了解模型的推理过程。