---
description: 了解如何在 Zoo Code 中使用上下文提及 (@) 来引用文件、文件夹、问题、终端输出和 Git 提交，以获得更准确的 AI 辅助。
keywords:
  - "Zoo Code context mentions"
  - "@ mentions"
  - "file references"
  - "folder mentions"
  - "problems panel"
  - "terminal mentions"
  - "Git integration"
---

# 上下文提及

上下文提及是一种强大的方式，可以为 Zoo Code 提供关于项目的特定信息，使其能够更准确、高效地执行任务。您可以使用提及来引用文件、文件夹、问题以及 Git 提交。上下文提及以 `@` 符号开头。

<img src="/img/context-mentions/context-mentions.png" alt="Context Mentions Overview - showing the @ symbol dropdown menu in the chat interface" width="600" />

*上下文提及概览，展示了聊天界面中的 @ 符号下拉菜单。*

---

## 提及类型

<img src="/img/context-mentions/context-mentions-1.png" alt="File mention example showing a file being referenced with @ and its contents appearing in the conversation" width="600" />

*文件提及将实际代码内容添加到对话中，以便直接引用和分析。*

| 提及类型 | 格式 | 描述 | 使用示例 |
|--------------|--------|-------------|--------------|
| **文件** | `@/path/to/file.ts` | 在请求上下文中包含文件内容 | "解释 @/src/utils.ts 中的函数" |
| **图像** | `@/path/to/image.png` | 将图像作为内联视觉内容包含（支持视觉处理的文件提及） | "这个 UI 有什么问题？@/screenshots/bug.png" |
| **文件夹** | `@/path/to/folder/` | 包含文件夹内所有文件的内容（非递归） | "分析 @/src/components/ 中的代码" |
| **问题** | `@problems` | 包含 VS Code 问题面板的诊断信息 | "@problems 修复我代码中的所有错误" |
| **终端** | `@terminal` | 包含最近的终端命令和输出 | "修复 @terminal 中显示的错误" |
| **Git 提交** | `@a1b2c3d` | 通过哈希引用特定提交 | "提交 @a1b2c3d 有什么变化？" |
| **Git 更改** | `@git-changes` | 显示未提交的更改 | "为 @git-changes 建议一条消息" |
| **URL** | `@https://example.com` | 导入网站内容 | "总结 @https://docusaurus.io/" |
| **斜杠命令** | `/<command-name>` | 执行斜杠命令（使用 `/` 而非 `@`） | "/test 运行所有测试" |

### 文件提及

<img src="/img/context-mentions/context-mentions-1.png" alt="File mention example showing a file being referenced with @ and its contents appearing in the conversation" width="600" />

*文件提及将源代码与行号结合，以便精确引用。*
| 功能 | 详情 |
|------------|---------|
| **格式** | `@/path/to/file.ts`（始终从工作区根目录以 `/` 开头） |
| **提供** | 包含行号的完整文件内容 |
| **支持** | 文本文件、PDF 和 DOCX 文件（支持文本提取） |
| **适用场景** | 初始请求、反馈响应和后续消息 |
| **限制** | 非常大的文件可能会被截断；不支持二进制文件 |

### 图像提及

图像提及是具备特殊视觉处理的文件提及。当您提及图像文件且模型支持视觉功能时，图像会作为内联视觉内容发送，而不是文本。

| 功能 | 详情 |
|------------|---------|
| **类型** | 文件提及的子类型（并非独立的提及类型） |
| **格式** | `@/path/to/image.png`（与文件提及相同的路径格式） |
| **提供** | 图像作为内联视觉内容发送给模型 |
| **支持** | PNG、JPG、JPEG、GIF、BMP、SVG、WEBP、ICO、AVIF |
| **最佳用途** | UI 审查、截图调试、图表分析 |
| **要求** | 需要支持视觉功能的模型（非视觉模型无法解释图像） |

### 文件夹提及

<img src="/img/context-mentions/context-mentions-2.png" alt="Folder mention example showing directory contents being referenced in the chat" width="600" />

*文件夹提及包含指定目录内所有文件的内容。*
| 功能 | 详情 |
|------------|---------|
| **格式** | `@/path/to/folder/`（需要尾随斜杠以区别于文件提及） |
| **提供** | 目录内所有文件的完整内容 |
| **包含** | 文件夹内非二进制文本文件的内容（非递归） |
| **最佳用途** | 提供目录中多个文件的上下文 |
| **提示** | 提及大型目录时，请注意上下文窗口限制 |

### Problems Mention

<img src="/img/context-mentions/context-mentions-3.png" alt="Problems mention example showing VS Code problems panel being referenced with @problems" width="600" />

*问题提及直接从 VS Code 的问题面板导入诊断信息。*
| 功能 | 详情 |
|------------|---------|
| **格式** | `@problems` |
| **提供** | VS Code 问题面板中的所有错误和警告 |
| **包含** | 文件路径、行号和诊断消息 |
| **分组** | 问题按文件组织，以提高清晰度 |
| **最佳用途** | 无需手动复制即可修复错误 |

有关 Zoo Code 如何与 VSCode 的诊断系统集成的详细信息，请参阅[诊断集成](/features/diagnostics-integration)。

### 终端提及
<img src="/img/context-mentions/context-mentions-4.png" alt="Terminal mention example showing terminal output being included in Zoo's context" width="600" />

*终端提及捕获最近的命令输出，用于调试和分析。*

| 功能 | 详情 |
|------------|---------|
| **格式** | `@terminal` |
| **捕获** | 最后一个命令及其完整输出 |
| **保留** | 终端状态（不会清除终端） |
| **限制** | 仅限于可见的终端缓冲区内容 |
| **最佳用途** | 调试构建错误或分析命令输出 |

### Git 提及

<img src="/img/context-mentions/context-mentions-5.png" alt="Git commit mention example showing commit details being analyzed by Zoo" width="600" />

*Git 提及提供提交详情和差异，以便进行上下文感知的版本分析。*
| 类型 | 格式 | 提供 | 限制 |
|------|--------|----------|------------|
| **提交** | `@a1b2c3d` | 提交消息、作者、日期和完整差异 | 仅在 Git 仓库中有效 |
| **工作更改** | `@git-changes` | `git status` 输出和未提交更改的差异 | 仅在 Git 仓库中有效 |

### URL 提及
<img src="/img/context-mentions/context-mentions-6.png" alt="URL mention example showing website content being converted to Markdown in the chat" width="600" />

*URL 提及导入外部网页内容并将其转换为可读的 Markdown 格式。*

| 功能 | 详情 |
|------------|---------|
| **格式** | `@https://example.com` |
| **处理** | 使用无头浏览器获取内容 |
| **清理** | 移除脚本、样式和导航元素 |
| **输出** | 将内容转换为 Markdown 以提高可读性 |
| **限制** | 复杂页面可能无法完美转换 |

### 斜杠命令提及

斜杠命令由提及系统处理，但使用 `/` 前缀而非 `@`。它们执行预定义的命令以执行特定操作。

| 功能 | 详情 |
|------------|---------|
| **格式** | `/<command-name>`（使用 `/` 而非 `@`） |
| **提供** | 执行指定命令并包含相关上下文 |
| **内容类型** | 作为内容块类型 "command" 处理 |
| **示例** | `/test`、`/init`、`/deploy` 以及其他自定义命令 |
| **最佳用途** | 快速访问预定义的工作流和操作 |

有关可用斜杠命令以及如何创建自定义命令的详细信息，请参阅[斜杠命令](/features/slash-commands)。

---

## 如何使用提及

1. 在聊天输入中键入 `@` 以触发建议下拉菜单
2. 继续键入以过滤建议，或使用方向键导航
3. 使用 Enter 键或鼠标点击选择
4. 在请求中组合多个提及："修复 @/src/component.ts 中的 @problems"

下拉菜单会自动建议：
- 最近打开的文件
- 可见的文件夹
- 最近的 Git 提交
- 特殊关键字（`problems`、`terminal`、`git-changes`）
- **所有当前打开的文件**（无论忽略设置或目录过滤器如何）

默认情况下，下拉菜单会遵循 `.rooignore`，隐藏被忽略的文件。启用 `showRooIgnoredFiles` 设置以在下拉菜单中包含被忽略的文件（它们将显示 🔒 指示器）。常见目录（如 `node_modules`、`.git`、`dist` 和 `out`）也会被过滤，以减少干扰。

---

## 重要行为

### 忽略文件交互

| 行为 | 说明 |
|----------|-------------|
| **下拉筛选** | `@` 下拉菜单默认隐藏与 `.rooignore` 匹配的文件。启用 `showRooIgnoredFiles` 可查看这些文件（标记为 🔒）。 |
| **绕过 `.rooignore`** | 文件和文件夹的 `@mentions` 在获取上下文内容时会绕过 `.rooignore` 检查。如果直接提及被忽略的文件，其内容将被包含。 |
| **绕过 `.gitignore`** | 类似地，文件和文件夹的 `@mentions` 在获取内容时不遵循 `.gitignore` 规则。 |
| **遵循 Git 命令** | 与 Git 相关的提及（`@git-changes`、`@commit-hash`）会遵循 `.gitignore`，因为它们依赖 Git 命令。 |

---

## 相关功能

- [诊断集成](/features/diagnostics-integration) - 了解自动错误检测和智能严重性筛选
- [代码操作](/features/code-actions) - 探索编辑器中的快速修复和 AI 辅助功能
- [Shell 集成](/features/shell-integration) - 了解终端提及如何与 Shell 集成协同工作
