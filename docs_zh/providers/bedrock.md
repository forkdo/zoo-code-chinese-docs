---
sidebar_label: AWS Bedrock
description: 通过 AWS 使用 Amazon Bedrock 与 Zoo Code 访问 Claude、Llama 及其他基础模型。配置凭据和 VPC 终端节点。
keywords:
  - aws bedrock
  - amazon bedrock
  - roo code
  - api provider
  - claude bedrock
  - llama bedrock
  - aws ai
  - foundation models
  - vpc endpoint
---

# 在 Zoo Code 中使用 AWS Bedrock

Zoo Code 支持通过 Amazon Bedrock 访问模型。Amazon Bedrock 是一项完全托管的服务，通过单一 API 提供来自领先 AI 公司的一系列高性能基础模型 (FMs)。

**网站：** [https://aws.amazon.com/bedrock/](https://aws.amazon.com/bedrock/)

---

## 先决条件

*   **AWS 账户：** 您需要一个有效的 AWS 账户。
*   **Bedrock 访问权限：** 您必须申请并获得 Amazon Bedrock 的访问权限。有关申请访问权限的详细信息，请参阅 [AWS Bedrock 文档](https://docs.aws.amazon.com/bedrock/latest/userguide/getting-started.html)。
*   **模型访问权限：** 在 Bedrock 中，您需要申请对您要使用的特定模型（例如 Anthropic Claude）的访问权限。
*   **安装 AWS CLI：** 使用 AWS CLI 为您的账户配置身份验证
    ```bash
     aws configure
    ```

---

## 获取凭据

您有两个主要选项来配置 AWS 凭据：

1.  **AWS 访问密钥（推荐用于开发）：**
    *   创建一个具有必要权限的 IAM 用户（至少需要 `bedrock:InvokeModel` 权限）。
    *   为该用户生成一个访问密钥 ID 和秘密访问密钥。
    *   *（可选）* 如果您的 IAM 配置要求，请创建一个会话令牌。
2.  **AWS 配置文件：**
    *   使用 AWS CLI 或通过手动编辑 AWS 凭据文件来配置 AWS 配置文件。有关详细信息，请参阅 [AWS CLI 文档](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)。

---

## 可用模型

Zoo Code 支持通过 Amazon Bedrock 提供的所有基础模型。

有关包含 ID 和功能的最完整、最新的模型列表，请参阅 [AWS Bedrock 支持的模型文档](https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html)。

**重要提示：** 在配置 Zoo Code 时，请使用*模型 ID*，而不是模型名称。

---

## Configuration in Zoo Code

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标 (<Codicon name="gear" />)。
2.  **选择提供商：** 从“API 提供商”下拉菜单中选择“Bedrock”。
3.  **选择身份验证方法：**
    *   **AWS 凭据：**
        *   输入您的“AWS 访问密钥”和“AWS 秘密密钥”。
        *   （可选）如果您使用的是临时凭据，请输入您的“AWS 会话令牌”。
    *   **AWS 配置文件：**
        *   输入您的“AWS 配置文件”名称（例如“default”）。
4.  **选择区域：** 选择您的 Bedrock 服务所在的 AWS 区域（例如“us-east-1”）。
5.  **（可选）跨区域推理：** 如果您想访问与配置的 AWS 区域不同的区域中的模型，请勾选“使用跨区域推理”。
6.  **（可选）VPC 终端节点：** 适用于企业环境：
    *   勾选“使用 VPC 终端节点”以通过您的 VPC 终端节点路由所有 Bedrock API 调用
    *   在出现的文本字段中输入您的 VPC 终端节点 URL
    *   这可确保所有 LLM 事务都保留在您的企业网络内
7.  **选择模型：** 从“模型”下拉菜单中选择您所需的模型。

---
---

## Claude 模型的推理预算

Zoo Code 支持在 Bedrock 上为 Anthropic 的 Claude 模型使用推理预算（扩展思考）。这允许模型在响应之前进行更多“思考”，这对于复杂任务可能很有用。

要启用推理预算：

1.  **选择一个包含推理功能的受支持 Claude 模型。**
2.  **在模型设置中启用推理模式。**
3.  **调整思考预算**以控制模型应“思考”的程度。

此功能仅适用于受支持的 Claude 模型。

## 提示和注意事项

*   **权限：** 确保您的 IAM 用户或角色具有调用 Bedrock 模型所需的权限。需要 `bedrock:InvokeModel` 权限。
*   **定价：** 有关模型成本的详细信息，请参阅 [Amazon Bedrock 定价](https://aws.amazon.com/bedrock/pricing/) 页面。
*   **跨区域推理：** 使用跨区域推理可能会导致更高的延迟。
*   **VPC 终端节点：** 使用 VPC 终端节点时，请确保您的终端节点已正确配置以处理 Bedrock API 调用。对于有严格安全要求、要求将所有 API 流量保留在其私有网络内的组织，此功能特别有用。
