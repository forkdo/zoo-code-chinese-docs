---
sidebar_label: 连接 AI 提供商
description: 了解如何将 Zoo Code 连接到 AI 提供商，例如 Anthropic Claude、OpenAI 和 OpenRouter。API 密钥设置和配置的逐步指南。
keywords:
  - Zoo Code API 密钥
  - Claude API
  - OpenAI API
  - OpenRouter
  - Anthropic API
  - AI 提供商设置
  - API 配置
---
import KangarooIcon from '@site/src/components/KangarooIcon';

# 连接您的第一个 LLM 提供商

Zoo Code 需要一个推理提供商来访问使其工作的 LLM 模型。

一个很好的入门模型是 **Claude Sonnet 4.5**，它在合理的价格点上提供了强大的功能。要开始使用，请选择一个提供商：

- **Zoo Code Router（推荐）：** 访问多个经过 Zoo Code 测试的提供商的最简单方法。无需 API。要开始使用，只需选择 Zoo Code Router 作为提供商并按照说明操作。[了解更多信息](/roo-code-router/overview)。

- **OpenRouter：** 通过单个 API 密钥提供来自不同实验室的多个 AI 模型的访问。非常适合灵活性和快速入门。要获取 API 密钥，[请按照这些说明操作 <LucideIcon name="ArrowRight" />](/providers/openrouter#getting-an-api-key)

- **Anthropic：** 直接访问 Claude 模型系列。需要 API 访问批准，并且可能[根据您的层级有速率限制](https://docs.anthropic.com/en/api/rate-limits#requirements-to-advance-tier)。要获取 API 密钥，[请按照这些说明操作 <LucideIcon name="ArrowRight" />](/providers/anthropic#getting-an-api-key)

Zoo Code 与提供 Claude 的[其他提供商](/providers)兼容，并且可以与您可以尝试的各种不同模型配合使用。

:::info 模型选择建议
我们推荐 **Claude Sonnet 4.5**，因为它在大多数任务中开箱即用。我们在内部大量使用它。

您可以选择其他模型，但这会增加复杂性。不同的模型在遵循工具指令、解析格式以及在多步操作中保持上下文方面有所不同，因此最好稍后再尝试它们。如果您确实要试验其他模型，请选择专门为结构化推理和工具使用而设计的模型。
:::

---

## 在 VS Code 中配置提供商

1. 通过单击 VS Code 活动栏中的 Zoo Code 图标 (<KangarooIcon />) 打开 Zoo Code 面板
2. 在欢迎屏幕上，选择您的 LLM 提供商。
3. 如果您选择 Zoo Code Router，只需连接您的帐户并从下拉列表中选择 `anthropic/claude-sonnet-4-5`。您就设置好了。
4. 如果您选择了其他提供商，请将您从中复制的 API 密钥粘贴到正确的字段中并继续
5. 选择您的模型（它应该被称为 `claude-sonnet-4-5` 或 `anthropic/claude-sonnet-4-5`）并完成该过程。

现在您可以开始编码了！
