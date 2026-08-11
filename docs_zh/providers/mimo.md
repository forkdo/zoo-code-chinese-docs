---
sidebar_label: MiMo
description: 在 Zoo Code 中配置小米 MiMo 模型。访问 MiMo V2.5 Pro 和 V2.5，通过 100 万 token 上下文窗口实现深度推理与工具调用。
keywords:
  - mimo
  - 小米
  - mimo v2.5 pro
  - mimo v2.5
  - zoo code
  - api 提供方
  - 推理 ai
  - 编程 ai
  - 工具调用
---

# 在 Zoo Code 中使用 MiMo

Zoo Code 支持通过小米 MiMo API 访问模型，包括 `mimo-v2.5-pro` 和 `mimo-v2.5`。MiMo 模型专为深度推理、工具调用和结构化输出而设计，支持最高 100 万 token 的上下文。

**网站：** [https://platform.xiaomimimo.com/](https://platform.xiaomimimo.com/)

---

## 获取 API 密钥

1.  **注册/登录：** 前往 [MiMo 平台](https://platform.xiaomimimo.com/) 创建账户或登录。
2.  **进入 API 密钥页面：** 在平台仪表板中找到你的 API 密钥。
3.  **创建密钥：** 点击"Create new API key"。为你的密钥取一个描述性名称（例如"Zoo Code"）。
4.  **复制密钥：** **重要：** 立即复制 API 密钥。你将无法再次看到它。请妥善保管。

---

## 可用模型

Zoo Code 支持以下 MiMo 模型：

| 模型 | 上下文窗口 | 最大输出 | 图像 | 描述 |
|-------|---------------|------------|--------|-------------|
| `mimo-v2.5-pro`（默认） | 100 万 token | 131K token | 否 | 具备深度思考与工具调用的旗舰推理模型 |
| `mimo-v2.5` | 100 万 token | 131K token | 是 | 全模态模型（文本、图像、音频、视频输入），具备深度思考 |

有关完整、最新的模型列表与能力，请参阅 [MiMo 的 API 文档](https://platform.xiaomimimo.com/docs/en-US/quick-start/model-hyperparameters)。

---

## 在 Zoo Code 中的配置

1.  **打开 Zoo Code 设置：** 点击 Zoo Code 面板中的齿轮图标（<Codicon name="gear" />）。
2.  **选择提供方：** 从"API Provider"下拉菜单中选择"Xiaomi MiMo"。
3.  **输入 API 密钥：** 将你的 MiMo API 密钥粘贴到"MiMo API Key"字段。
4.  **选择模型：** 从"Model"下拉菜单中选择你想要的模型。
5.  **（可选）Base URL：** 如果你需要使用自定义的 MiMo API base URL，请在"Base URL"字段中输入。默认值为 `https://token-plan-sgp.xiaomimimo.com/v1`。其他区域端点也可用（例如中国用 `https://token-plan-cn.xiaomimimo.com/v1`，欧洲用 `https://token-plan-ams.xiaomimimo.com/v1`）。

---

## 提示与说明

*   **定价：** MiMo 提供有竞争力的定价。有关当前费率，请参阅 [MiMo 定价页面](https://platform.xiaomimimo.com/)。注意，长上下文用量（超过 256K token）按标准费率的 2 倍计费。
*   **思考模式：** MiMo 模型支持深度思考模式，默认开启。模型会逐步推理复杂问题，提升编程和推理任务的准确性。
*   **工具调用：** MiMo 模型支持用于智能体工作流的工具调用。在多轮带工具调用的对话中，推理内容会被保留，以获得一致的结果。
*   **缓存定价：** 缓存读取可享受折扣价。缓存写入目前限时免费。
