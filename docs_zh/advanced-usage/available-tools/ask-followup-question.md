---
description: 在 Zoo Code 中通过 ask_followup_question 工具实现交互式沟通，用于收集澄清信息和用户偏好。
keywords:
  - ask_followup_question
  - user interaction
  - interactive communication
  - Zoo Code tools
  - clarification
  - user feedback
---

# ask_followup_question

`ask_followup_question` 工具通过提出具体问题来收集有效完成任务所需的额外信息，从而实现交互式沟通。

---

## 参数

该工具接受以下参数：

- `question` (必需): 向用户提出的具体问题
- `follow_up` (可选): 2-4 个建议答案的列表，用于引导用户回答，每个建议答案都包含在 `<suggest>` 标签中

---

## 功能说明

该工具在 Zoo 和用户之间创建了一个对话界面，允许在遇到模糊情况或决策点时收集澄清信息、额外细节或用户偏好。每个问题都可以包含建议回答，以简化交互过程。

---

## 使用时机

- 当原始请求中缺少关键信息时
- 当 Zoo 需要在多个有效实现方法之间做出选择时
- 当需要技术细节或偏好才能继续时
- 当 Zoo 遇到需要解决的模糊情况时
- 当额外上下文可以显著提高解决方案质量时

---

## 主要特性

- 提供了一种结构化的方式来收集特定信息，而不会中断工作流程
- 包含建议答案，以减少用户输入并引导回答
- 在交互过程中保持对话历史和上下文
- 支持包含图像和代码片段的回答
- 作为"始终可用"工具集的一部分，在所有模式下都可用
- 支持用户对实现决策的直接指导
- 使用 `<answer>` 标签格式化回答，以区别于常规对话
- 成功使用时重置连续错误计数器

---

## 局限性

- 每次工具使用仅限于提出一个具体问题
- 建议在用户界面中显示为可选择选项
- 无法强制结构化回答 - 用户仍然可以自由回答
- 过度使用可能会减慢任务完成速度并造成碎片化体验
- 建议答案必须是完整的，不能包含需要用户编辑的占位符
- 对用户回答没有内置验证机制
- 没有强制特定答案格式的机制

---

## 工作原理

当调用 `ask_followup_question` 工具时，它会遵循以下流程：

1. **参数验证**: 验证必需的 `question` 参数并检查可选建议
   - 确保提供了问题文本
   - 使用 `fast-xml-parser` 库从 `follow_up` 参数解析任何建议答案
   - 即使只有一个建议，也将建议标准化为数组格式

2. **JSON 转换**: 将 XML 结构转换为用于 UI 显示的标准 JSON 格式
   ```typescript
   {
     question: "User's question here",
     suggest: [
       { answer: "Suggestion 1" },
       { answer: "Suggestion 2" }
     ]
   }
   ```

3. **UI 集成**:
   - 通过 `ask("followup", ...)` 方法将 JSON 结构传递给 UI 层
   - 在界面中向用户显示可选择的建议按钮
   - 创建选择或输入回答的交互体验

4. **回答收集和处理**:
   - 捕获用户文本输入和回答中包含的任何图像
   - 在返回给助手时用 `<answer>` 标签包装用户回答
   - 保留用户回答中包含的任何图像
   - 通过将回答添加到历史记录中来保持对话上下文
   - 成功使用工具时重置连续错误计数器

5. **错误处理**:
   - 使用计数器跟踪连续错误
   - 成功使用工具时重置计数器
   - 提供特定的错误消息：
     - 缺少参数时："Missing required parameter 'question'"
     - XML 解析错误时："Failed to parse operations: [error message]"
     - 格式无效时："Invalid operations xml format"
   - 包含防护措施，防止缺少必需参数时执行工具
   - 发生错误时增加连续错误计数

---

## 工作流程序列

问答循环遵循以下序列：

1. **信息缺口识别**: Zoo 识别继续所需的缺失信息
2. **具体问题创建**: Zoo 制定一个清晰、有针对性的问题
3. **建议开发**: Zoo 创建相关的建议答案（可选但推荐）
4. **工具调用**: 助手使用问题调用工具，并可选择包含建议
5. **UI 展示**: 问题和建议作为交互元素显示给用户
6. **用户回答**: 用户选择一个建议或提供自定义答案
7. **消息处理**: 系统处理部分和完整消息
   - 对于流式响应，随着分块的到达进行处理
   - 对于完整消息，一次性处理整个响应
   - 无论消息分块如何，都保持状态一致性
8. **回答处理**: 系统用 `<answer>` 标签包装回答并保留图像
9. **上下文集成**: 回答被添加到对话历史中
10. **任务继续**: Zoo 使用新信息继续执行任务

---

## 使用示例

- 在开发 Web 应用程序时，Zoo 可能会询问首选的样式框架（Bootstrap、Tailwind、自定义 CSS）
- 在创建 API 时，Zoo 可能会询问认证方法（JWT、OAuth、API 密钥）
- 在重构代码时，Zoo 可能会询问优先考虑性能还是可读性
- 在设置数据库时，Zoo 可能会询问特定的模式设计偏好
- 在创建自定义功能时，Zoo 可能会询问特定的行为期望
- 在排查错误时，Zoo 可能会询问特定的环境细节

---

## 回答格式

当用户回答问题时，回答会使用 `<answer>` 标签格式化，以确保清晰和一致性。这种格式有助于清晰地将用户的答案与其他对话元素分开。

示例回答格式：
```
<answer>
Use Tailwind CSS for utility-first styling with maximum flexibility
</answer>
```

回答格式的主要特点：
- 被 `<answer>` 标签包围
- 保留原始回答的格式
- 如果回答中包含图像，则保留图像
- 即使是长回答，也返回完整文本

---

## 使用示例

询问实现偏好：
```
<ask_followup_question>
<question>Which styling approach would you prefer for this web application?</question>
<follow_up>
<suggest>Use Bootstrap for rapid development with consistent components</suggest>
<suggest>Use Tailwind CSS for utility-first styling with maximum flexibility</suggest>
<suggest>Use vanilla CSS with custom styling for complete control and minimal dependencies</suggest>
</follow_up>
</ask_followup_question>
```

请求技术澄清：
```
<ask_followup_question>
<question>What database should this application use for storing user data?</question>
<follow_up>
<suggest>MongoDB for flexible schema and document-based storage</suggest>
<suggest>PostgreSQL for relational data with strong consistency guarantees</suggest>
<suggest>Firebase for real-time updates and simplified backend management</suggest>
<suggest>SQLite for lightweight local storage without external dependencies</suggest>
</follow_up>
</ask_followup_question>
```

解决需求中的模糊性：
```
<ask_followup_question>
<question>How should the application handle user authentication?</question>
<follow_up>
<suggest>Implement email/password authentication with account verification</suggest>
<suggest>Use social login providers (Google, GitHub, etc.) for quick signup</suggest>
<suggest>Implement both email/password and social login options</suggest>
</follow_up>
</ask_followup_question>
```