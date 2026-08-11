---
description: 掌握 Zoo Code 中的键盘导航功能，通过可自定义的快捷键、命令执行和提示历史导航，实现高效编码工作流。
keywords:
  - keyboard shortcuts
  - keyboard navigation
  - roo-cline.acceptInput
  - prompt history
  - accessibility
  - vim compatibility
sidebar_label: 键盘导航
---

# 键盘导航

Zoo Code 界面支持键盘导航和快捷键，可简化您的工作流程，减少对鼠标交互的依赖。

---

## 可用键盘命令

Zoo Code 提供键盘命令以增强您的工作流程。本页面重点介绍 `roo-cline.acceptInput` 命令，但以下是所有键盘命令的快速参考：

| 命令 | 描述 | 默认快捷键 |
|---------|-------------|-----------------|
| `roo-cline.acceptInput` | 提交文本或接受主要建议 | 无（可配置） |
| `roo-cline.focusInput` | 聚焦 Zoo 输入框 | 无（可配置） |
| 添加到上下文 | 将选定代码添加到 Zoo 的上下文中 | macOS: Cmd+K Cmd+A；Windows/Linux: Ctrl+K Ctrl+A |
| 向上/向下箭头 | 在提示历史中导航 | 内置 |

### 键盘命令的主要优势

* **键盘驱动界面**：无需鼠标交互即可提交文本或选择主要建议按钮
* **增强可访问性**：对行动不便或鼠标使用不适的用户至关重要
* **Vim/Neovim 兼容性**：支持从以键盘为中心的环境无缝过渡的开发人员
* **工作流效率**：减少开发任务中键盘和鼠标之间的上下文切换

---

## roo-cline.acceptInput 命令

`roo-cline.acceptInput` 命令允许您使用键盘快捷键提交文本或接受建议，而无需点击按钮或在输入区域按 Enter 键。

### 功能说明

`roo-cline.acceptInput` 命令是一个通用输入提交命令。触发时，它会：

- 在文本输入区域提交当前文本或图像输入（相当于按 Enter 键）
- 当操作按钮可见时点击主要（第一个）按钮（如确认/取消按钮或任何其他操作按钮）

### 详细设置指南

#### 方法 1：使用 VS Code 界面

1. 打开命令面板（`Ctrl+Shift+P` 或 Mac 上的 `Cmd+Shift+P`）
2. 输入“Preferences: Open Keyboard Shortcuts”
3. 在搜索框中输入“roo-cline.acceptInput”
4. 在结果中找到“Zoo: Accept Input/Suggestion”
5. 点击命令左侧的 + 图标
6. 按下所需的按键组合（例如 `Ctrl+Enter` 或 `Alt+Enter`）
7. 按 Enter 键确认

#### 方法 2：直接编辑 keybindings.json

1. 打开命令面板（`Ctrl+Shift+P` 或 Mac 上的 `Cmd+Shift+P`）
2. 输入“Preferences: Open Keyboard Shortcuts (JSON)”
3. 将以下条目添加到 JSON 数组中：

```json
{
  "key": "ctrl+enter",  // 或您首选的按键组合
  "command": "roo-cline.acceptInput",
  "when": "view == roo-cline.SidebarProvider || activeWebviewPanelId == roo-cline.TabPanelProvider"
}
```

范围限定示例：
- 仅限侧边栏 Zoo 视图：
```json
{
  "key": "ctrl+enter",
  "command": "roo-cline.acceptInput",
  "when": "view == roo-cline.SidebarProvider"
}
```
- 仅限编辑器 Zoo 标签页：
```json
{
  "key": "ctrl+enter",
  "command": "roo-cline.acceptInput",
  "when": "activeWebviewPanelId == roo-cline.TabPanelProvider"
}
```

#### 推荐的按键组合

选择不与现有 VS Code 快捷键冲突的按键组合：

- `Alt+Enter` - 打字时易于按下
- `Ctrl+Space` - 对使用自动补全的用户来说很熟悉
- `Ctrl+Enter` - 对命令执行来说很直观
- `Alt+A` - “接受”的助记符

## 添加到上下文快捷键

- 默认：macOS: Cmd+K Cmd+A；Windows/Linux: Ctrl+K Ctrl+A
- 要求：when 条件 `editorTextFocus && editorHasSelection`
- 焦点不会自动更改。要立即继续输入，请使用“Zoo: Focus Input”（`roo-cline.focusInput`）或点击 Zoo 面板。

:::note 重做快捷键已恢复
标准重做快捷键（macOS: Cmd+Y；Windows/Linux: Ctrl+Y）保持不变，可在 VS Code 中用于其通常功能。
:::

### 实际使用案例

#### 快速开发工作流

- **文本提交**：无需将手从键盘移开即可向 Zoo 发送消息
- **操作确认**：接受保存文件、运行命令或应用差异等操作
- **多步骤流程**：快速通过需要确认或输入的步骤
- **连续任务**：以最小中断将多个任务链接在一起

#### 以键盘为中心的开发

- **Vim/Neovim 工作流**：如果您来自 Vim/Neovim 背景，请保持以键盘为中心的工作流
- **IDE 集成**：与其他 VS Code 键盘快捷键一起使用，实现无缝体验
- **代码审查**：使用 Zoo 审查代码时快速接受建议
- **文档编写**：生成文档时提交文本并接受格式建议

#### 可访问性使用案例

- **手部活动能力限制**：对手部使用鼠标有困难的用户至关重要
- **重复性劳损预防**：减少鼠标使用以预防或管理重复性劳损
- **屏幕阅读器集成**：与视觉障碍用户的屏幕阅读器配合良好
- **语音控制兼容性**：使用语音控制软件时可通过语音命令触发

### 可访问性优势

`roo-cline.acceptInput` 命令在设计时考虑了可访问性：

- **减少鼠标依赖**：无需触及鼠标即可完成整个工作流
- **减少身体劳损**：帮助因鼠标使用而感到不适或疼痛的用户
- **替代输入方法**：支持依赖键盘导航的行动不便用户
- **工作流优化**：对来自 Vim/Neovim 等以键盘为中心的环境的用户特别有价值

### 以键盘为中心的工作流

以下是一些完整的工作流示例，展示如何有效地将键盘快捷键与 Zoo 结合使用：

#### 开发工作流示例

1. 打开 VS Code 并导航到您的项目
2. 通过侧边栏打开 Zoo
3. 输入您的请求：“为用户注册创建一个 REST API 端点”
4. 当 Zoo 询问框架偏好时，使用 `roo-cline.acceptInput` 快捷键选择第一个建议
5. 继续使用快捷键接受代码生成建议
6. 当 Zoo 提供保存文件时，再次使用快捷键确认
7. 使用 VS Code 的内置快捷键导航创建的

#### 代码审查工作流

1. 选择要审查的代码并使用 VS Code 的“复制”命令
2. 要求 Zoo 审查：“审查此代码是否存在安全问题”
3. 当 Zoo 询问代码上下文的澄清问题时，使用快捷键接受建议
4. 当 Zoo 提供改进建议时，再次使用快捷键接受实施建议

### 故障排除

| 问题 | 解决方案 |
|-------|----------|
| 快捷键不起作用 | 确保 Zoo 已聚焦（首先点击 Zoo 面板） |
| 选择了错误的建议 | 该命令始终选择第一个（主要）按钮；如果需要其他选项，请使用鼠标 |
| 与现有快捷键冲突 | 在 VS Code 键盘设置中尝试不同的按键组合 |
| 使用时无视觉反馈 | 这是正常的 - 该命令静默激活功能，无视觉确认 |
| 快捷键工作不一致 | 确保 `when` 子句配置正确（使用 `view == roo-cline.SidebarProvider` 或 `activeWebviewPanelId == roo-cline.TabPanelProvider`） |

### 技术实现

`roo-cline.acceptInput` 命令的实现如下：

- 命令已注册为 `roo-cline.acceptInput`，在命令面板中显示标题为“Zoo：接受输入/建议”
- 触发时，它会向活动的 Zoo WebView 发送“acceptInput”消息
- WebView 根据当前 UI 状态确定适当的操作：
  - 如果操作按钮可见且已启用，则点击主要操作按钮
  - 如果文本区域已启用且包含文本/图像，则发送消息
- 无默认按键绑定 - 用户可分配其首选快捷键

### 限制

- 仅在 Zoo 界面处于活动状态时有效
- 如果当前没有可用的输入或建议，则无效
- 当显示多个选项时，优先选择主要（第一个）按钮

---

## Command Line Style Prompt History Navigation

使用方向键以类似终端的体验浏览您的提示历史记录。此功能让您可以轻松重用和改进之前的提示，无论是来自当前对话还是过去的任务。

### 主要功能
- **上/下方向键**：循环浏览之前的提示。
- **上下文感知**：在对话和任务历史记录之间切换。
- **保留输入**：记住您正在输入的内容。

### 为何重要

**之前**：重用提示需要向上滚动、复制和粘贴。
- 繁琐且缓慢
- 容易迷失位置
- 中断您的工作流程

**使用提示历史记录导航后**：无需离开键盘即可快速访问过去的提示。

### 工作原理

导航设计为直观且能适应您的当前上下文。

#### 在活动对话中
- **向上箭头**：显示您发送的上一个提示。继续按可进一步回溯对话。
- **向下箭头**：在对话历史记录中向前移动，最终返回到您正在输入的文本。

#### 开始新聊天
- **向上箭头**：显示当前工作区中任务历史记录的最新提示。
- **向下箭头**：在任务历史记录中向前移动。

#### 边缘情况
- 如果在导航时开始输入，历史记录将被取消，您的新文本将被保留。
- 导航仅在光标位于输入框的第一行或最后一行时有效，以避免干扰多行编辑。

### 配置

此功能默认启用。无需配置任何设置。

### 优势

- **更快的工作流程**：无需使用鼠标即可重用提示。
- **更好的上下文**：轻松访问并基于之前的交互构建。
- **更少的中断**：专注于手头的任务。

### 常见问题

**“为什么按上箭头时没有任何反应？”**
- 您可能正在输入多行提示。光标必须位于第一行。
- 当前上下文可能没有可用的历史记录。

**“对话历史记录和任务历史记录有什么区别？”**
- **对话历史记录**包括您当前活动聊天会话中的提示。
- **任务历史记录**包括当前工作区中所有先前任务的初始提示。
