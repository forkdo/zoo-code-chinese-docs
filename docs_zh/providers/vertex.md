---
description: 通过 GCP Vertex AI 在 Zoo Code 中访问 Google Gemini 和 Anthropic Claude 模型。配置身份验证并开始使用企业级 AI。
keywords:
  - Vertex AI
  - GCP
  - Google Cloud
  - Zoo Code
  - Gemini
  - Claude
  - AI 模型
  - 机器学习
  - 云 AI
sidebar_label: GCP Vertex AI
---

# 在 Zoo Code 中使用 GCP Vertex AI

Zoo Code 支持通过 Google Cloud Platform 的 Vertex AI 访问模型，这是一个托管机器学习平台，提供对各种基础模型的访问，包括 Anthropic 的 Claude 系列模型。

**网站：** [https://cloud.google.com/vertex-ai](https://cloud.google.com/vertex-ai)

---

## 先决条件

*   **Google Cloud 账户：** 您需要一个有效的 Google Cloud Platform (GCP) 账户。
*   **项目：** 您需要一个启用了 Vertex AI API 的 GCP 项目。
*   **模型访问权限：** 您必须申请并获得对您想要在 Vertex AI 上使用的特定 Claude 模型的访问权限。有关说明，请参阅 [Google Cloud 文档](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude#before_you_begin)。
*   **应用程序默认凭据 (ADC)：** Zoo Code 使用应用程序默认凭据对 Vertex AI 进行身份验证。设置此凭据的最简单方法是：
    1.  安装 Google Cloud CLI：[https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)
    2.  使用以下命令进行身份验证：`gcloud auth application-default login`
*   **服务账户密钥（备选方案）：** 或者，您可以使用 Google Cloud 服务账户密钥文件进行身份验证。您需要在 GCP 项目中生成此密钥。请参阅 [Google Cloud 关于创建服务账户密钥的文档](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)。

---

## 可用模型

Zoo Code 支持通过 Google Cloud Vertex AI 提供的所有模型，包括 Anthropic Claude、Google Gemini 和 MAAS（模型即服务）产品。

有关完整且最新的模型列表和 ID，请参阅 [Vertex AI 的模型文档](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/models)。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“GCP Vertex AI”。
3.  **配置身份验证：**
    
    Zoo Code 使用以下身份验证优先级：
    1. **Google Cloud 凭据 (JSON)：** 如果提供，则优先使用
    2. **Google Cloud 密钥文件路径：** 如果提供且没有 JSON 凭据，则使用此选项
    3. **应用程序默认凭据 (ADC)：** 当上述两项均未提供时，作为备用方案使用
    
    **选项 1 - 使用 ADC（最简单）：**
    *   安装 Google Cloud CLI 并运行 `gcloud auth application-default login`
    *   无需在 Zoo Code 中进行进一步配置
    
    **选项 2 - 使用服务账户密钥：**
    *   将 JSON 内容直接粘贴到 **Google Cloud 凭据** 字段中
    *   或者在 **Google Cloud 密钥文件路径** 字段中提供文件路径

4.  **输入项目 ID：** 输入您的 Google Cloud 项目 ID。
5.  **选择区域：** 选择您的 Vertex AI 资源所在的区域（例如 `us-east5`）。
6.  **选择模型：** 从“模型”下拉菜单中选择您所需的模型。

---

## 高级功能

### Gemini 特定功能

当通过 Vertex AI 使用 Gemini 模型时，可以使用额外的 grounding 功能：

#### URL 上下文

启用 URL 上下文以允许 Gemini 模型直接访问和分析网页内容。此功能允许 Zoo：

- 实时读取和理解网页
- 分析来自 URL 的文档
- 查看在线代码仓库
- 访问网站上的最新信息

**启用 URL 上下文：**
1. 在 Vertex AI 配置中选择一个 Gemini 模型
2. 启用出现的“URL 上下文”选项
3. 保存您的设置

#### Google 搜索 Grounding

启用 Google 搜索 grounding 以通过实时搜索结果增强 Gemini 的响应。这提供：

- 来自网络搜索的最新信息
- 事实核查能力
- 对当前事件的了解
- 技术查询的准确性增强

**启用搜索 Grounding：**
1. 在 Vertex AI 配置中选择一个 Gemini 模型
2. 启用出现的“Google 搜索 Grounding”选项
3. 保存您的设置

:::note
只有当选择 Gemini 模型时，才会出现 URL 上下文和 Google 搜索 Grounding 选项。这些功能可能会产生额外费用。
:::

---

## 提示和注意事项

*   **权限：** 确保您的 Google Cloud 账户具有访问 Vertex AI 和您要使用的特定模型所需的权限。
*   **定价：** 有关详细信息，请参阅 [Vertex AI 定价](https://cloud.google.com/vertex-ai/pricing) 页面。