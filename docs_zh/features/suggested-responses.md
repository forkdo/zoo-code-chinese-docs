---
description: 了解 Zoo Code 中的建议回复如何通过预设选项帮助您快速回答后续问题，加快您的工作流程。
keywords:
  - 建议回复
  - 后续问题
  - 快速回答
  - 工作流程优化
  - ask_followup_question 工具
sidebar_label: 建议回复
---

import Codicon from '@site/src/components/Codicon';

# 建议回复

当 Zoo 需要更多信息来完成任务时，它会使用 [`ask_followup_question` 工具](/advanced-usage/available-tools/ask-followup-question)。为了使回复更容易和更快，Zoo 通常会在问题旁边提供建议的答案。

---

## 概述

建议回复以可点击按钮的形式直接出现在聊天界面中 Zoo 问题的下方。它们提供与问题相关的预设答案，帮助您快速提供输入。

<img src="/img/suggested-responses/suggested-responses.png" alt="Zoo 提出问题并提供建议回复按钮的示例" width="500" />

---

## 工作原理

1.  **问题出现**: Zoo 使用 `ask_followup_question` 工具提出问题。
2.  **显示建议**: 如果 Zoo 提供了建议，它们会以问题下方的按钮形式出现。
3.  **交互**: 您可以通过两种方式与这些建议进行交互。

---

## Interacting with Suggestions

您有三种使用建议回复的选项：

1.  **直接选择**:
    *   **操作**: 简单点击包含您想要提供的答案的按钮。
    *   **结果**: 选中的答案会立即作为您的回复发送回 Zoo。如果某个建议完全符合您的意图，这是最快的回复方式。

2.  **键盘快捷键**:
    *   **操作**: 使用您配置的键盘快捷键执行 `roo.acceptInput` 命令。
    *   **结果**: 主要（第一个）建议按钮会自动被选中。
    *   **注意**: 有关设置详情，请参见 [键盘快捷键](/features/keyboard-shortcuts)。

3.  **发送前编辑**:
    *   **操作**:
        *   按住 `Shift` 并点击建议按钮。
        *   *或者*，将鼠标悬停在建议按钮上并点击出现的铅笔图标 (<Codicon name="edit" />)。
    *   **结果**: 建议的文本会被复制到聊天输入框中。然后您可以在按 Enter 发送自定义回复之前根据需要修改文本。当建议接近但需要小幅调整时，这很有用。

<img src="/img/suggested-responses/suggested-responses-1.png" alt="显示从建议回复复制的文本的聊天输入框，准备进行编辑" width="600" />

---

## 优势

*   **速度**: 无需键入完整答案即可快速回复。
*   **清晰度**: 建议通常会澄清 Zoo 需要的信息类型。
*   **灵活性**: 在需要时编辑建议以提供精确的自定义答案。

当 Zoo 需要澄清时，此功能简化了交互，使您能够以最少的努力有效地指导任务。
