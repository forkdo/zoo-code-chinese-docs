---
sidebar_label: LiteLLM
description: 在 Zoo Code 中通过 LiteLLM 的统一 OpenAI 兼容 API 访问超过 100 个 LLM。简化多模型管理并降低成本。
keywords:
  - litellm
  - roo code
  - api provider
  - unified api
  - openai compatible
  - multi model
  - llm proxy
  - local deployment
  - cost management
---

# 在 Zoo Code 中使用 LiteLLM

LiteLLM 是一个多功能工具，通过提供 OpenAI 兼容的 API 为超过 100 个大型语言模型（LLMs）提供统一接口。这允许您运行一个本地服务器，该服务器可以将请求代理到各种模型提供商或服务本地模型，所有这些都可以通过一致的 API 端点访问。

**网站：** [https://litellm.ai/](https://litellm.ai/) (主项目) & [https://docs.litellm.ai/](https://docs.litellm.ai/) (文档)

---

## 主要优势

*   **统一 API：** 通过单个 OpenAI 兼容的 API 访问各种 LLM（来自 OpenAI、Anthropic、Cohere、HuggingFace 等）。
*   **本地部署：** 在本地运行您自己的 LiteLLM 服务器，让您更好地控制模型访问并可能减少延迟。
*   **简化配置：** 在一个地方（您的 LiteLLM 服务器）管理凭据和模型配置，然后让 Zoo Code 连接到它。
*   **成本管理：** LiteLLM 提供了跨不同模型和提供商跟踪成本的功能。

---

## 设置您的 LiteLLM 服务器

要在 Zoo Code 中使用 LiteLLM，您首先需要设置并运行 LiteLLM 服务器。

### 安装

1. 安装带有代理支持的 LiteLLM：
   ```bash
   pip install 'litellm[proxy]'
   ```

### 配置

2. 创建配置文件 (`config.yaml`) 来定义您的模型和提供商：
   ```yaml
   model_list:
     # 配置 Anthropic 模型
     - model_name: claude-sonnet
       litellm_params:
         model: anthropic/claude-sonnet-model-id
         api_key: os.environ/ANTHROPIC_API_KEY
     
     # 配置 OpenAI 模型
     - model_name: gpt-model
       litellm_params:
         model: openai/gpt-model-id
         api_key: os.environ/OPENAI_API_KEY
     
     # 配置 Azure OpenAI
     - model_name: azure-model
       litellm_params:
         model: azure/my-deployment-name
         api_base: https://your-resource.openai.azure.com/
         api_version: "2023-05-15"
         api_key: os.environ/AZURE_API_KEY
   ```

### 启动服务器

3. 启动 LiteLLM 代理服务器：
   ```bash
   # 使用配置文件（推荐）
   litellm --config config.yaml
   
   # 或快速启动单个模型
   export ANTHROPIC_API_KEY=your-anthropic-key
   litellm --model anthropic/claude-model-id
   ```

4. 代理默认在 `http://0.0.0.0:4000` 运行（可通过 `http://localhost:4000` 访问）。
   *   您还可以为您的 LiteLLM 服务器本身配置 API 密钥以增加安全性。

请参阅 [LiteLLM 文档](https://docs.litellm.ai/docs/) 获取有关高级服务器配置和功能的详细说明。

---

## 在 Zoo Code 中的配置

一旦您的 LiteLLM 服务器运行，您有两种选项在 Zoo Code 中配置它：

### 选项 1：使用 LiteLLM Provider（推荐）

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择 Provider：** 从"API Provider"下拉菜单中选择"LiteLLM"。
3.  **输入 Base URL：**
    *   输入您的 LiteLLM 服务器的 URL。
    *   如果留空，默认为 `http://localhost:4000`。
4.  **输入 API 密钥（可选）：**
    *   如果您为您的 LiteLLM 服务器配置了 API 密钥，请在此输入。
    *   如果您的 LiteLLM 服务器不需要 API 密钥，Zoo Code 将使用默认的虚拟密钥 (`"dummy-key"`)，这应该可以正常工作。
5.  **选择模型：**
    *   Zoo Code 将尝试通过查询 `${baseUrl}/v1/model/info` 端点从您的 LiteLLM 服务器获取可用模型列表。
    *   下拉菜单中显示的模型来自此端点。
    *   如果您在 LiteLLM 服务器上添加了新模型，请使用刷新按钮更新模型列表。
    *   如果未选择模型，Zoo Code 将使用默认模型。确保您已在 LiteLLM 服务器上配置了至少一个模型。

### 选项 2：使用 OpenAI 兼容 Provider

或者，您可以使用"OpenAI 兼容"provider 配置 LiteLLM：

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择 Provider：** 从"API Provider"下拉菜单中选择"OpenAI 兼容"。
3.  **输入 Base URL：** 输入您的 LiteLLM 代理 URL（例如 `http://localhost:4000`）。
4.  **输入 API 密钥：** 使用任何字符串作为 API 密钥（例如 `"sk-1234"`），因为 LiteLLM 处理实际的提供商身份验证。
5.  **选择模型：** 选择您在 `config.yaml` 文件中配置的模型名称。

<img src="/img/litellm/litellm.png" alt="Zoo Code LiteLLM Provider 设置" width="600" />

---

## Zoo Code 如何获取和解释模型信息

当您配置 LiteLLM provider 时，Zoo Code 与您的 LiteLLM 服务器交互以获取有关可用模型的详细信息：

*   **模型发现：** Zoo Code 向您的 LiteLLM 服务器上的 `${baseUrl}/v1/model/info` 发出 GET 请求。如果在 Zoo Code 的设置中提供了 API 密钥，它将包含在 `Authorization: Bearer ${apiKey}` 标头中。
*   **模型属性：** 对于您的 LiteLLM 服务器报告的每个模型，Zoo Code 提取并解释以下内容：
    *   `model_name`：模型的标识符。
    *   `maxTokens`：最大输出标记数。如果 LiteLLM 未指定，则默认为 `8192`。
    *   `contextWindow`：最大上下文标记数。如果 LiteLLM 未指定，则默认为 `200000`。
    *   `supportsImages`：从 LiteLLM 提供的 `model_info.supports_vision` 确定。
    *   `supportsPromptCache`：从 LiteLLM 提供的 `model_info.supports_prompt_caching` 确定。
    *   `inputPrice` / `outputPrice`：从 LiteLLM 的 `model_info.input_cost_per_token` 和 `model_info.output_cost_per_token` 计算。
    *   `supportsComputerUse`：如果底层模型标识符与 Zoo Code 中预定义为适合"计算机使用"的 Anthropic 模型之一匹配，则此标志设置为 `true`（请参阅技术详细信息中的 `COMPUTER_USE_MODELS`）。

如果您的 LiteLLM 服务器的 `/model/info` 端点未为给定模型显式提供这些属性中的某些，则 Zoo Code 为它们使用默认值。默认值为：
*   `maxTokens`：8192
*   `contextWindow`：200,000
*   `supportsImages`：`true`
*   `supportsComputerUse`：`true`（对于默认模型 ID）
*   `supportsPromptCache`：`true`
*   `inputPrice`：3.0（每 1k 标记的 µUSD）
*   `outputPrice`：15.0（每 1k 标记的 µUSD）

---

## 提示和注意事项

*   **LiteLLM 服务器是关键：** 模型的主要配置、下游提供商（如 OpenAI、Anthropic）的 API 密钥以及其他高级功能都在您的 LiteLLM 服务器上进行管理。Zoo Code 充当该服务器的客户端。
*   **配置选项：** 您可以使用专用的 "LiteLLM" 提供商（推荐）进行自动模型发现，或使用 "OpenAI Compatible" 提供商进行简单的手动配置。
*   **模型可用性：** Zoo Code "Model" 下拉菜单中可用的模型完全取决于您的 LiteLLM 服务器通过其 `/v1/model/info` 端点暴露的内容。
*   **网络可访问性：** 确保您的 LiteLLM 服务器正在运行，并且可以从运行 VS Code 和 Zoo Code 的机器访问（例如，如果不是在 `localhost` 上，请检查防火墙规则）。
*   **故障排除：** 如果模型未出现或请求失败：
    *   验证您的 LiteLLM 服务器是否正在运行并正确配置。
    *   检查 LiteLLM 服务器日志中的错误。
    *   确保 Zoo Code 设置中的基础 URL 与您的 LiteLLM 服务器地址匹配。
    *   确认您的 LiteLLM 服务器所需的任何 API 密钥都已正确输入到 Zoo Code 中。
*   **计算机使用模型：** Zoo Code 中的 `supportsComputerUse` 标志主要适用于某些已知在工具使用和函数调用任务中表现良好的 Anthropic 模型。如果您通过 LiteLLM 路由其他模型，除非底层模型 ID 与 Zoo Code 识别的特定 Anthropic 模型匹配，否则此标志可能不会自动设置。

通过利用 LiteLLM，您可以显著扩展可供 Zoo Code 访问的模型范围，同时集中管理它们。
