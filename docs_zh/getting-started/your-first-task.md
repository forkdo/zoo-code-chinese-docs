---
sidebar_label: 你的第一个任务
description: 了解如何使用 Zoo Code AI 助手开始你的第一个任务。面向初学者的分步指南，帮助你理解审批工作流和迭代流程。
keywords:
  - Zoo Code 教程
  - 第一个任务
  - 入门指南
  - AI 编程助手教程
  - 审批工作流
---
import KangarooIcon from '@site/src/components/KangarooIcon';

# 你的第一个任务

现在你已经[配置好了 AI 提供商和模型](/getting-started/connecting-api-provider)，可以开始使用 Zoo Code 了！本指南将引导你完成第一次交互。

---

## 步骤 1：打开 Zoo Code 面板

点击 VS Code 活动栏（窗口侧边的垂直栏）中的 Zoo Code 图标 (<KangarooIcon />) 打开聊天界面。如果看不到该图标，请确认扩展已安装并启用。

<figure>
  <img src="/img/your-first-task/your-first-task.png" alt="VS Code 活动栏中的 Zoo Code 图标" width="600" />
  <figcaption>活动栏中的 Zoo Code 图标可打开聊天界面。你可以拖动它来调整 Zoo 的位置。</figcaption>
</figure>

## 步骤 2：输入你的任务

在面板底部的聊天框中，用清晰简洁的语言描述你希望 Zoo Code 完成的任务。有效的任务示例：

* “创建一个名为 `hello.txt` 的文件，内容为 'Hello, world!'。”
* “编写一个将两个数字相加的 Python 函数。”
* “创建一个简单的网站 HTML 文件，标题为 'Zoo test'”

无需特殊命令或语法——只需使用自然语言即可。

<figure>
  <img src="/img/your-first-task/your-first-task-6.png" alt="在 Zoo Code 聊天界面中输入任务" width="400" />
  <figcaption>用自然语言输入你的任务——无需特殊语法。</figcaption>
</figure>

## 步骤 3：发送任务

按下回车键或点击输入框右侧的发送图标 (<Codicon name="send" />)。

## 步骤 4：查看并批准操作

Zoo Code 会分析你的请求并提出具体操作。这些操作可能包括：

* **读取文件：** 显示需要访问的文件内容
* **写入文件：** 显示包含建议更改的差异（绿色为新增行，红色为删除行）
* **执行命令：** 显示要在终端中运行的精确命令
* **使用浏览器：** 概述浏览器操作（点击、输入等）
* **提出问题：** 在需要时请求澄清以便继续

<figure>
  <img src="/img/your-first-task/your-first-task-7.png" alt="查看建议的文件创建操作" width="800" />
  <figcaption>Zoo Code 会明确显示它想要执行的操作，并等待你的批准。</figcaption>
</figure>

**每个操作都需要你的明确批准**（除非已启用自动批准）：

* **批准：** 点击“批准”按钮执行建议的操作
* **拒绝：** 点击“拒绝”按钮，并在需要时提供反馈

## 步骤 5：迭代

Zoo Code 采用迭代式工作。每次操作后，它会等待你的反馈，然后再提出下一步。继续这个“查看-批准”循环，直到任务完成。

<figure>
  <img src="/img/your-first-task/your-first-task-8.png" alt="已完成任务的最终结果，展示迭代过程" width="500" />
  <figcaption>任务完成后，Zoo Code 会显示最终结果，并等待你的下一条指令。</figcaption>
</figure>

---

## 总结

你已经使用 Zoo Code 完成了第一个任务！通过这个过程，你学到了：

* 如何用自然语言与 Zoo Code 交互
* 让你保持控制权的基于审批的工作流
* Zoo Code 用于逐步解决问题的迭代方法

这种迭代式、基于审批的工作流是 Zoo Code 的核心——让 AI 处理编码中的繁琐部分，而你则保持完全的监督权。现在你已经掌握了基础知识，可以着手处理更复杂的任务，探索不同的[模式](/basic-usage/using-modes)来实现专门的工作流，或者尝试[自动批准功能](/features/auto-approving-actions)来加速重复性任务。
