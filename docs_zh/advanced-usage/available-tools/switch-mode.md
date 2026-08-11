---
description: 了解 switch_mode 如何在 Zoo Code 中实现 Code、Architect、Ask 和 Debug 模式之间的无缝转换，以处理专门的任务。
keywords:
  - switch_mode
  - Zoo Code tools
  - mode switching
  - operational modes
  - Code mode
  - Architect mode
  - Debug mode
  - Ask mode
  - task transitions
  - VS Code AI
tags:
  - tools
---

# switch_mode

`switch_mode` tool 使 Zoo 能够在不同的操作模式之间切换，每种模式都具有处理特定类型任务的专门功能。这允许在当前任务需要不同专业技能时，在 Code、Architect、Ask 或 Debug 等模式之间进行无缝转换。

---

## 参数

该 tool 接受以下参数：

- `mode_slug`（必需）：要切换到的模式的 slug（例如，"code"、"ask"、"architect"）
- `reason`（可选）：切换模式的原因，为用户提供上下文

---

## 功能说明

当当前任务由另一种模式的功能处理会更好时，此 tool 会请求模式更改。它在保持上下文的同时，将 Zoo 的重点和可用工具集转移到匹配新任务阶段要求的新模式。

---

## 使用时机

- 从信息收集过渡到代码实现时
- 从编码转向架构或设计时
- 当前任务需要仅在不同模式下可用的功能时
- 复杂项目特定阶段需要专门专业知识时

---

## 主要特性

- 在模式转换期间保持上下文连续性
- 为模式切换建议提供清晰的理由
- 要求用户批准所有模式更改
- 强制执行特定于每个模式的工具组限制
- 根据所选模式无缝调整工具可用性
- 与标准模式和自定义模式一起工作
- 在 UI 中显示模式切换和理由
- 使用 XML 样式格式化进行参数规范
- 处理特定于某些模式的文件类型限制

---

## 限制

- 无法切换到系统中不存在的模式
- 需要用户明确批准每次模式转换
- 在切换完成之前无法使用特定于模式的工具
- 在模式切换后应用 500ms 延迟以允许更改生效
- 某些模式有文件类型限制（例如，Architect 模式只能编辑 markdown 文件）
- 模式保存以供恢复仅适用于 `new_task` 功能，不适用于一般模式切换

---

## 工作原理

当调用 `switch_mode` tool 时，它遵循以下过程：

1. **请求验证**：
   - 验证请求的模式是否存在于系统中
   - 检查是否提供了 `mode_slug` 参数且有效
   - 验证用户是否已处于请求的模式中
   - 确保提供的 `reason` 参数（如果提供）格式正确

2. **模式转换准备**：
   - 使用提供的原因打包模式更改请求
   - 向用户显示更改请求以获得批准

3. **模式激活（用户批准后）**：
   - 更新 UI 以反映新模式
   - 根据模式的工具组配置调整可用工具
   - 应用特定于模式的提示和行为
   - 应用 500ms 延迟以允许更改在执行下一个 tool 之前生效
   - 强制执行特定于模式的任何文件限制

4. **继续执行**：
   - 使用新模式的功能继续执行任务
   - 保留来自先前交互的相关上下文

---

## Tool Group 关联

`switch_mode` tool 属于 "modes" tool group，但也包含在 "always available" tools 列表中。这意味着：

- 无论模式配置的工具组如何，都可以在任何模式中使用
- 它与 `ask_followup_question` 和 `attempt_completion` 等其他核心 tools 一起可用
- 当任务要求发生变化时，允许在工作流程中的任何点进行模式转换

---

## 模式结构

系统中的每个模式都有特定结构：

- `slug`：模式的唯一标识符（例如，"code"、"ask"）
- `name`：模式的显示名称（例如，"Code"、"Ask"）
- `roleDefinition`：模式的专门角色和功能
- `customInstructions`：可选的模式特定指令，用于指导行为
- `groups`：模式可用的工具组，带有可选限制

---

## 模式功能

核心模式提供以下专门功能：

- **Code Mode**：专注于编码任务，可完全访问代码编辑工具
- **Architect Mode**：专门用于系统设计和架构规划，仅限编辑 markdown 文件
- **Ask Mode**：优化用于回答问题和提供信息
- **Debug Mode**：配备系统性问题诊断和解决功能

---

## 自定义模式

除了核心模式外，系统还支持自定义项目特定模式：

- 可以定义具有特定工具组启用的自定义模式
- 它们可以指定自定义角色定义和指令
- 系统首先检查自定义模式，然后回退到核心模式
- 自定义模式定义优先于具有相同 slug 的核心模式

---

## 文件限制

不同模式可能有特定的文件类型限制：

- **Architect Mode**：只能编辑匹配 `.md` 扩展名的文件
- 尝试编辑受限制的文件类型会导致 `FileRestrictionError`
- 这些限制有助于在模式之间强制执行适当的关注点分离

---

## 使用示例

- 讨论新功能时，Zoo 从 Ask 模式切换到 Architect 模式以帮助设计系统结构。
- 在 Architect 模式中完成架构规划后，Zoo 切换到 Code 模式以实现设计的功能。
- 在开发过程中遇到错误时，Zoo 从 Code 模式切换到 Debug 模式进行系统性故障排除。

---

## 使用示例

切换到 Code 模式进行实现：
```
<switch_mode>
<mode_slug>code</mode_slug>
<reason>Need to implement the login functionality based on the architecture we've discussed</reason>
</switch_mode>
```

切换到 Architect 模式进行设计：
```
<switch_mode>
<mode_slug>architect</mode_slug>
<reason>Need to design the system architecture before implementation</reason>
</switch_mode>
```

切换到 Debug 模式进行故障排除：
```
<switch_mode>
<mode_slug>debug</mode_slug>
<reason>Need to systematically diagnose the authentication error</reason>
</switch_mode>
```

切换到 Ask 模式获取信息：
```
<switch_mode>
<mode_slug>ask</mode_slug>
<reason>Need to answer questions about the implemented feature</reason>
</switch_mode>
```