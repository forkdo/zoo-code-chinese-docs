---
description: 了解 Zoo Code 中提示词的技术结构。学习消息的构建方式、系统提示词的工作原理，并优化你的交互体验。
keywords:
  - prompt structure
  - system prompt
  - message flow
  - technical documentation
  - LLM communication
---

# 提示词结构

本页面解释了 Zoo Code 中提示词的技术结构——消息是如何构建并发送给大型语言模型（LLM）的。

---

## 核心消息类型

Zoo Code 在与 LLM 通信时使用三种主要消息类型：

- **系统提示词（System Prompt）**：定义 Zoo 能力、角色和操作规则的初始指令
- **用户消息（User Messages）**：你（用户）发送给 Zoo 的内容
- **助手消息（Assistant Messages）**：LLM 根据你的请求生成的回复

在 API 层面，还存在第四种消息角色：

- **工具消息（Tool Messages）**：工具执行后返回的结果，作为输入发送回 LLM

了解这些消息类型有助于你更有效地使用 Zoo，对故障排查或高级定制也很有价值。

---

## 系统提示词

系统提示词是 Zoo 行为的基础。它包含：

- **角色定义（Role Definition）**：基于所选模式（Code、Ask、Debug 等）的核心角色指令
- **工具描述（Tool Descriptions）**：可用工具的详细信息，包括参数和示例
- **工具使用指南（Tool Use Guidelines）**：工具使用规则（顺序执行、等待结果）
- **能力说明（Capabilities）**：描述 Zoo 在当前环境中能做什么
- **可用模式（Available Modes）**：所有可用模式及其描述的列表
- **操作规则（Operational Rules）**：处理文件、项目结构和用户交互的关键指南
- **系统信息（System Information）**：你的环境详情（操作系统、shell、工作目录）
- **自定义指令（Custom Instructions）**：你的全局和模式特定自定义设置

每次你与 Zoo 交互时，系统提示词都会动态生成，以适应你当前的模式、可用工具和自定义设置。

---

## 用户消息

用户消息包含你直接发送给 Zoo 的输入，以及额外的上下文信息：

- **你的查询（Your Query）**：你在聊天界面中输入的文本
- **图像（Images）**：你在消息中包含的任何图像（适用于支持的模型）
- **环境详情（Environment Details）**：自动附加的关于工作区状态的信息：
  - 打开的文件/标签页
  - 光标位置
  - 带有输出的活动终端
  - 最近修改的文件
  - 当前时间
  - Token/成本信息
  - 当前模式
  - 文件列表（初始连接时）

这种自动的上下文丰富功能帮助 Zoo 理解你的工作区，而无需你明确描述它。

---

## 助手消息

助手消息是 LLM 的回复，可能包括：

- **文本回复（Text Responses）**：对你查询的直接回答
- **思考过程（Thinking）**：内部推理过程（启用时可见）
- **工具调用（Tool Calls）**：请求使用特定工具，如读取文件或执行命令

请注意，虽然助手消息包含工具调用，但这些工具的结果会通过单独的工具消息发送回 LLM，而不是作为助手消息本身的一部分。

---

## 消息流

这些组件是如何协同工作的：

1. **初始设置（Initial Setup）**：Zoo 根据你选择的模式和配置生成系统提示词
2. **用户输入（User Input）**：你发送一条消息，该消息会附加环境详情
3. **LLM 处理（LLM Processing）**：LLM 接收所有之前的消息以及你的新输入
4. **助手回复（Assistant Response）**：LLM 生成回复，可能使用工具
5. **工具执行（Tool Execution）**：如果 LLM 请求工具，Zoo 会执行它并提供结果
6. **对话历史（Conversation History）**：所有消息都保存在结构化的历史记录中以提供上下文

---

## 技术实现

在内部，Zoo 的提示词构建由多个组件处理：

- **系统提示词生成（System Prompt Generation）**：`src/core/prompts/system.ts` 中的 `SYSTEM_PROMPT` 函数组装完整的系统提示词
- **部分生成器（Section Generators）**：专用函数创建系统提示词的每个部分
- **消息转换（Message Transformation）**：特定于提供者的转换器将 Zoo 的内部消息格式转换为每个 LLM API 所需的格式

---

## 支持提示词

除了主要的聊天流程外，Zoo 还为特定代码操作使用专门的模板：

- **代码操作提示词（Code Action Prompts）**：用于“解释”、“修复”、“改进”或“添加到上下文”等命令
- **基于模板（Template-Based）**：从 `src/shared/support-prompt.ts` 中的模板生成
- **独立上下文（Independent Context）**：通常在无主要聊天历史的情况下运行
- **任务特定格式（Task-Specific Format）**：针对正在执行的特定代码任务进行优化

这些支持提示词在正常对话流程之外工作，为特定编码任务提供专注的协助。

---

## 优化你的交互

了解此结构可以帮助你：

- **编写更好的提示词（Write Better Prompts）**：了解 Zoo 已有的上下文有助于避免冗余信息
- **排查问题（Troubleshoot Issues）**：理解消息流有助于识别问题可能出现的位置
- **创建自定义模式（Create Custom Modes）**：了解系统提示词结构后，你可以创建更有效的自定义模式

该技术基础支撑着 Zoo 的所有功能，使其能够理解你的请求并有效利用可用工具完成任务。