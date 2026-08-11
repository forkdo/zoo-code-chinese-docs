---
sidebar_label: Ollama
description: 在 Zoo Code 中设置 Ollama，以在本地运行开源语言模型，实现隐私保护、离线访问和低成本 AI 编程。
keywords:
  - Ollama
  - local models
  - Zoo Code
  - open source AI
  - CodeLlama
  - Qwen
  - offline AI
  - privacy
  - context window configuration
---
import KangarooIcon from '@site/src/components/KangarooIcon';

# 在 Zoo Code 中使用 Ollama

Zoo Code 支持使用 Ollama 在本地运行模型。这提供了隐私保护、离线访问和潜在的成本优势，但需要更多设置和性能强大的计算机。

**官网：** [https://ollama.com/](https://ollama.com/)

---

## 设置 Ollama

1.  **下载并安装 Ollama：** 从 [Ollama 官网](https://ollama.com/) 下载适用于您操作系统的 Ollama 安装程序。按照安装说明进行操作。确保 Ollama 正在运行

    ```bash
    ollama serve
    ```

2.  **下载模型：** 浏览 [Ollama 的模型库](https://ollama.com/library) 查看所有可用模型。要下载模型，请运行：

    ```bash
    ollama pull <model_name>
    ```

    例如：

    ```bash
    ollama pull qwen2.5-coder:32b
    ```

3. **配置模型：** 在 Ollama 中配置模型的上下文窗口并保存副本。

   :::info 默认上下文行为
   **Zoo Code 默认自动遵循 Modelfile 中的 `num_ctx` 设置。** 当您使用 Ollama 模型时，Zoo Code 会读取模型配置的上下文窗口并自动使用。您无需在 Zoo Code 设置中配置上下文大小——它会尊重您在 Ollama 模型中定义的内容。
   :::

   **选项 A：交互式配置**
   
   加载模型（我们将以 `qwen2.5-coder:32b` 为例）：
   
    ```bash
    ollama run qwen2.5-coder:32b
    ```

   更改上下文大小参数：

    ```bash
    /set parameter num_ctx 32768
    ```

    使用新名称保存模型：

    ```bash
    /save your_model_name
    ```

   **选项 B：使用 Modelfile（推荐）**

   创建一个包含所需配置的 `Modelfile`：

   ```dockerfile
   # 减少上下文的 Modelfile 示例
   FROM qwen2.5-coder:32b
   
   # 将上下文窗口设置为 32K tokens（从默认值减少）
   PARAMETER num_ctx 32768
   
   # 可选：调整温度以获得更一致的输出
   PARAMETER temperature 0.7
   
   # 可选：设置重复惩罚
   PARAMETER repeat_penalty 1.1
   ```

   然后创建您的自定义模型：

   ```bash
   ollama create qwen-32k -f Modelfile
   ```

   :::tip 覆盖上下文窗口
   如果您需要覆盖模型的默认上下文窗口：
   - **永久覆盖：** 使用上述任一方法保存具有所需 `num_ctx` 的新模型版本
   - **Zoo Code 行为：** Zoo 自动使用 Ollama 模型中配置的 `num_ctx`
   - **内存考虑：** 减少 `num_ctx` 有助于防止在硬件资源有限时出现内存不足错误
   :::

4.  **配置 Zoo Code：**
    *   打开 Zoo Code 侧边栏（<KangarooIcon /> 图标）。
    *   点击设置齿轮图标（<Codicon name="gear" />）。
    *   选择 "ollama" 作为 API 提供商。
    *   输入上一步中的模型标签或保存的名称（例如 `your_model_name`）。
    *   （可选）如果您在不同的机器上运行 Ollama，请配置基础 URL。默认为 `http://localhost:11434`。
    *   （可选）如果您的 Ollama 服务器需要身份验证，请输入 API 密钥。
    *   （高级）Zoo 默认对 "ollama" 提供商使用 Ollama 的原生 API。也存在兼容 OpenAI 的 `/v1` 处理程序，但典型设置中不需要。

---

## 提示和注意事项

*   **资源要求：** 在本地运行大型语言模型可能会消耗大量资源。确保您的计算机满足所选模型的最低要求。
*   **模型选择：** 尝试不同的模型，找到最适合您需求的模型。
*   **离线使用：** 下载模型后，您可以使用该模型离线使用 Zoo Code。
*   **Token 跟踪：** Zoo Code 跟踪通过 Ollama 运行的模型的 token 使用情况，帮助您监控消耗。
*   **Ollama 文档：** 有关安装、配置和使用 Ollama 的更多信息，请参阅 [Ollama 文档](https://ollama.com/docs)。

---

## 故障排除

### 首次请求时内存不足 (OOM)

**症状**
- Zoo 的首次请求因内存不足错误而失败
- 模型首次加载时 GPU/CPU 内存使用量激增
- 在 Ollama 中手动启动模型后可以正常工作

**原因**
如果没有模型实例在运行，Ollama 会按需启动一个。在冷启动期间，它可能会分配比预期更大的上下文窗口。更大的上下文窗口会增加内存使用量，并可能超过可用的 VRAM 或 RAM。这是 Ollama 的启动行为，而不是 Zoo Code 的错误。

**解决方法**
1. **预加载模型**
   ```bash
   ollama run &lt;model-name&gt;
   ```
   保持其运行，然后从 Zoo 发出请求。

2. **固定上下文窗口 (`num_ctx`)**
   - 选项 A — 交互式会话，然后保存：
     ```bash
     # 在 `ollama run &lt;base-model&gt;` 内部
     /set parameter num_ctx 32768
     /save &lt;your_model_name&gt;
     ```
   - 选项 B — Modelfile（推荐，可重现）：
     ```dockerfile
     FROM &lt;base-model&gt;
     PARAMETER num_ctx 32768
     # 根据您的可用内存调整：
     # 16384 适用于 ~8GB VRAM
     # 32768 适用于 ~16GB VRAM
     # 65536 适用于 ~24GB+ VRAM
     ```
     然后创建模型：
     ```bash
     ollama create &lt;your_model_name&gt; -f Modelfile
     ```

3. **确保模型的上下文窗口已固定**
   使用适当的 `num_ctx` 保存您的 Ollama 模型（通过 `/set` + `/save`，或首选 Modelfile）。**Zoo Code 会自动检测并使用模型配置的 `num_ctx`** —— 对于 Ollama 提供商，Zoo Code 中没有手动上下文大小设置。

4. **使用较小的变体**
   如果 GPU 内存有限，请使用较小的量化版本（例如 q4 而不是 q5）或较小的参数大小（例如 7B/13B 而不是 32B）。

5. **OOM 后重启**
   ```bash
   ollama ps
   ollama stop &lt;model-name&gt;
   ```

**快速检查清单**
- 在 Zoo 请求之前模型正在运行
- `num_ctx` 已固定（Modelfile 或 `/set` + `/save`）
- 模型以适当的 `num_ctx` 保存（Zoo 会自动使用此设置）
- 模型适合可用的 VRAM/RAM
- 没有残留的 Ollama 进程