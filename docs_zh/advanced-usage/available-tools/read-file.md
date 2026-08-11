---
description: 探索 read_file 工具检查文件内容的能力，支持行范围、PDF/DOCX 提取、图像读取以及实验性的多文件并发读取。
keywords:
  - read_file
  - Zoo Code tools
  - file reading
  - concurrent reads
  - line numbers
  - PDF extraction
  - DOCX support
  - image support
  - OCR workflows
  - code analysis
  - VS Code AI
---

# read_file

`read_file` 工具用于检查项目中的文件内容。它让 Zoo 能够理解代码、配置文件、文档，以及图像，从而提供更好的帮助。

:::info 多文件支持
`read_file` 工具通过 `args` 格式接受多个文件。并发数和每个请求的限制在 UI 中配置；后端工具不会硬性限制文件数量。某些模型可能使用简化的单文件变体。

**注意：** 读取文件时（即使是单个文件），LLM 会看到一条鼓励多文件读取的消息：“一次性读取多个文件对 LLM 更高效。如果其他文件与您当前任务相关，请同时读取它们。”
:::

---

## 参数

该工具接受两种格式的参数：

### 标准格式（单文件）

- `path`（必需）：要读取的文件路径，相对于当前工作目录
- `mode`（可选）：读取模式 — `"slice"`（默认）或 `"indentation"`
- `offset`（可选）：基于 1 的起始行号，从该处开始读取（仅 slice 模式，默认：`1`）
- `limit`（可选）：返回的最大行数（仅 slice 模式，默认：`2000`）
- `indentation`（可选）：缩进模式选项 — 仅当 `mode="indentation"` 时使用：
  - `anchor_line`（必需）：基于 1 的锚定行号，用于定位提取位置。工具会提取包含该行的完整语义代码块（函数、类、方法）。
  - `max_levels`（可选）：在锚定行上方包含的最大缩进层级数。
  - `include_siblings`（可选）：是否包含同一缩进层级的兄弟代码块。
  - `include_header`（可选）：是否在输出顶部包含文件头内容（import、模块级注释）。
  - `max_lines`（可选）：缩进模式下返回行数的硬性上限。

:::note 模式概要
- **Slice 模式**（默认）：从 `offset` 开始按顺序读取，最多 `limit` 行。适用于初次探索文件或读取特定行范围。
- **Indentation 模式**：基于缩进层级，提取 `anchor_line` 周围完整、语法有效的代码块。当您已有目标行号（例如来自搜索结果或错误消息）并需要整个函数/类而不被截断在函数中段时，优先使用此模式。
- `start_line` 和 `end_line` **并非**参数。在 slice 模式中进行范围读取，请使用 `offset` 和 `limit`。
:::

### 增强格式（多文件）

该工具还接受包含多个文件条目的 `args` 参数。并发数由 UI 配置；无论该设置如何，后端都接受多个文件。某些模型可能使用简单的单文件工具。

- `args`（必需）：多个文件规范的容器
  - `file`（必需）：单个文件规范
    - `path`（必需）：要读取的文件路径
    - `line_range`（可选）：行范围规范（例如 "1-50" 或 "100-150"）。每个文件可以指定多个 `line_range` 元素。

---

## 功能说明

该工具读取指定文件的内容，并附带行号以便于参考。它可以读取整个文件或特定部分，从 PDF 和 Word 文档中提取文本，并以各种格式显示图像。

---

## 使用场景

- 当 Zoo 需要理解现有代码结构时
- 当 Zoo 需要分析配置文件时
- 当 Zoo 需要从文本文件中提取信息时
- 当 Zoo 需要在建议更改之前查看代码时
- 当讨论中需要引用特定行号时

---

## 主要特性

- 显示带行号的文件内容，便于参考
- 可通过指定行范围读取文件的特定部分
- 从 PDF、DOCX、XLSX 和 IPYNB 文件中提取可读文本
- **图像支持**：以多种格式显示图像（PNG、JPG、JPEG、GIF、WebP、SVG、BMP、ICO、TIFF/TIF、AVIF）
- **智能读取**：根据 token 预算自动截断以适应剩余预算，而不是失败
- **大文件预览**：为超大文件返回 100KB 预览，以便快速检查
- **优雅错误恢复**：从流错误中恢复，并引导您使用 line_range 进行有针对性的读取
- 当未指定行范围时，自动截断大文本文件，显示文件开头
- 仅高效流式传输请求的行范围，以获得更好的性能
- 通过行号标记，便于讨论代码的特定部分
- **多文件支持**：通过批量批准同时读取多个文件

---

## 多文件功能

支持多文件读取。并发数和每个请求的限制在“设置”中配置；后端工具不会硬性限制文件数量，行为可能受模型/工具选择约束：

### 配置
- **位置**：设置 > 上下文 > “并发文件读取限制”
- **描述**：“'read_file' 工具可以同时处理的最大文件数。较高的值可能会加快读取多个小文件的速度，但会增加内存使用量。”
- **范围**：1-100（滑块控件）
- **默认值**：5

### 批量处理
- 每个请求最多支持 100 个文件（默认 5 个），由 UI 配置。后端不会硬性限制；实际行为可能受模型/工具约束。
- 并行处理以提高性能
- 批量批准界面供用户同意

### 增强用户体验
- 多个文件的单个批准对话框
- 单个文件覆盖选项
- 清晰显示将访问哪些文件
- 优雅处理混合成功/失败场景

### 提高效率
- 减少多个批准对话框的干扰
- 通过并行文件读取加快处理速度
- 智能批处理相关文件
- 可配置的并发限制以匹配系统能力

---

## 局限性

- **大文件**：对于超大文件，该工具可能返回预览，并引导您使用 `line_range` 进行有针对性的读取。
- **二进制文件**：除了 PDF、DOCX、XLSX、IPYNB 和支持的图像格式外，内容可能无法被人阅读。
- **UI/模型约束**：并发限制和每个请求的文件数在 UI 中配置；后端工具不会硬性限制。
- **图像文件**：图像以 base64 数据 URL 形式提供。高分辨率图像可能很大。
  - 默认单个图像最大尺寸：5MB
  - 默认图像总尺寸上限：20MB
- **不支持的二进制格式**：返回 `<binary_file format="ext">Binary file - content not displayed</binary_file>` 占位符。
- **Token 预算**：内容可能会被截断以适应剩余的 token 预算；通知会指示如何继续。

---

## 工作原理

当调用 `read_file` 工具时，它遵循以下流程：

1. **参数验证**：验证必需的 `path` 参数和可选参数
2. **路径解析**：将相对路径解析为绝对路径
3. **读取策略选择**：
   - 该工具使用严格的优先级层次结构（详细说明如下）
   - 它在范围读取、自动截断或完整文件读取之间进行选择
4. **内容处理**：
   - 为内容添加行号（例如 "1 | const x = 13"），其中 `1 |` 是行号。
   - 对于被截断的文件，添加截断通知和方法定义
   - 对于特殊格式（PDF、DOCX、XLSX、IPYNB），提取可读文本
   - 对于图像格式，XML 包含一个带有尺寸的 `<notice>`；实际图像作为 base64 数据 URL 附加到工具结果中（不返回尺寸；MIME 类型由数据 URL 隐含）

---

## 读取策略优先级

该工具使用清晰的决策层次结构来确定如何读取文件：

1. **第一优先级：显式行范围**
   - 单文件格式：在 slice 模式中使用 `offset` 和 `limit` 进行范围读取，或在 indentation 模式中使用 `anchor_line`。
   - 多文件 `args` 格式：每个文件指定一个或多个 `line_range` 条目。
   - 范围读取仅流式传输请求的行，并绕过 `maxReadFileLine`，优先于其他选项。

2. **第二优先级：Token 预算管理**
   - 该工具尊重剩余的 token 预算，以防止上下文溢出
   - 如果文件将超出剩余预算，它会自动截断以适应
   - 对于超大文件（超出实际限制），返回 100KB 预览以便快速检查
   - 提供指导，使用 `line_range` 有针对性地读取特定部分
   - 从流错误中优雅恢复，并建议替代方法

3. **第三优先级：大文本文件自动截断**
   - 仅在以下所有条件均满足时生效：
     - 未指定 `offset`/`limit` 范围（slice 模式）且未提供 `anchor_line`（indentation 模式）。
     - 文件被识别为文本类文件（非 PDF/DOCX/XLSX/IPYNB 等二进制格式）。
     - 文件总行数超过 `maxReadFileLine` 设置值（可配置；UI 默认值可能为 500；后端未设置时使用 `-1` 表示无行数限制）。
   - 自动截断时的行为：
     - 工具仅读取前 `maxReadFileLine` 行。
     - 附加提示信息，例如：`仅显示共 Y 行中的前 X 行。如需读取更多行，请使用 line_range 参数。`
   - **特殊情况 —— 仅定义模式**：当 `maxReadFileLine` 设为 `0` 时，工具仅返回代码定义而不包含文件内容（并附带提示信息）。

4. **默认行为：读取整个文件**
    - 如果既未指定显式范围，也不满足自动截断条件（例如文件行数在限制内，或属于支持的二进制类型），则工具会读取完整内容。
    - 对于 PDF 和 DOCX 等支持格式，尝试提取全部文本内容。
    - 对于图像格式，返回可在聊天界面中显示的 base64 编码数据 URL。

---

## 使用场景示例

- 当被要求解释或改进代码时，Zoo 首先读取相关文件以理解当前实现。
- 当排查配置问题时，Zoo 读取配置文件以识别潜在问题。
- 当处理文档时，Zoo 先读取现有文档以了解当前内容，再建议改进方案。

---

## 使用示例

以下是多个演示 `read_file` 工具使用方式及典型输出的场景。

### 读取整个文件

要读取文件的完整内容：

**输入：**
```xml
<read_file>
<path>src/app.js</path>
</read_file>
```

**模拟输出（针对小型文件如 `example_small.txt`）：**
```
1 | 这是第一行。
2 | 这是第二行。
3 | 这是第三行。
```
*（实际输出取决于文件具体内容）*

### 读取指定行范围

仅读取特定行范围（例如第 46-68 行），请在 slice 模式中使用 `offset` 和 `limit`：

**输入：**
```xml
<read_file>
<path>src/app.js</path>
<offset>46</offset>
<limit>23</limit>
</read_file>
```

**模拟输出（针对 `example_five_lines.txt` 的第 2-3 行）：**
```
2 | 第二行的内容。
3 | 第三行的内容。
```
*（输出仅显示请求的行及其原始行号）*

### 读取大文本文件（自动截断）

当读取大文本文件且未指定行范围时，若其超过内部行数限制（例如 500 行），工具会自动截断内容。

**输入：**
```xml
<read_file>
<path>logs/large_app.log</path>
</read_file>
```

**模拟输出（针对 1500 行日志文件，限制为 500 行）：**
```
1 | 日志条目 1...
2 | 日志条目 2...
...
500 | 日志条目 500...

仅显示共 1500 行中的前 500 行。使用 line_range 读取特定部分。
// 可选：代码文件可能在此处显示源码定义摘要
```
*（输出显示从开头到 `maxReadFileLine` 限制的行，并附带截断提示。如需完整访问，请使用行范围参数。）*

### 仅读取定义

当用户设置中将 `maxReadFileLine` 设为 `0` 时，工具仅返回源码定义而不包含文件内容：

**输入：**
```xml
<!-- 假设用户设置中 maxReadFileLine 设为 0 -->
<read_file>
<path>src/services/auth.service.ts</path>
</read_file>
```

**模拟输出：**
```xml
<file>
  <path>src/services/auth.service.ts</path>
  <notice>仅显示共 150 行中的前 0 行。如需读取更多行，请使用 line_range 参数</notice>
</file>
```
*（此模式可在不读取内容的情况下快速了解文件结构。）*

### 尝试读取不存在的文件

如果指定的文件不存在：

**输入：**
```xml
<read_file>
<path>non_existent_file.txt</path>
</read_file>
```

**模拟输出（错误）：**
```
错误：路径 'non_existent_file.txt' 下未找到文件。
```

### 尝试读取被屏蔽的文件

如果文件被 `.rooignore` 文件中的规则排除：

**输入：**
```xml
<read_file>
<path>.env</path>
</read_file>
```

**模拟输出（错误）：**
```xml
<file>
  <path>.env</path>
  <error>被 .rooignore 规则拒绝访问</error>
</file>
```

---

### 智能读取与 Token 预算管理

读取大文件时，工具会自动管理 token 预算以防止上下文溢出。

**场景：** 读取超大文件且未指定行范围。

**输入：**
```xml
<read_file>
<path>logs/massive-debug.log</path>
</read_file>
```

**模拟输出（针对超出 token 预算的文件）：**
```
预览：显示 …MB 文件中的前 …MB。使用 line_range 读取特定部分。
```

替代截断提示：
```
因上下文限制，文件已截断为 M 字符中的前 N 字符。使用 line_range 读取特定部分。
```

此行为确保：
- 小文件完全读取，零开销
- 大文件自动截断以适配剩余 token 预算
- 超大文件提供快速预览
- 引导您使用 `line_range` 进行定向读取
- 流式错误被优雅处理

**使用 offset/limit 进行定向读取的示例：**
```xml
<read_file>
<path>logs/massive-debug.log</path>
<offset>1000</offset>
<limit>101</limit>
</read_file>
```

## 图像读取示例

`read_file` 工具现已支持直接在聊天界面中读取和显示图像，从而实现强大的可视化分析工作流。

### 读取单张图像

**输入：**
```xml
<read_file>
<path>assets/logo.png</path>
</read_file>
```

**输出：**
```xml
<file>
  <path>assets/logo.png</path>
  <notice>图像文件 (123 KB)</notice>
</file>
```

图像以内联方式显示在聊天中（工具结果附带 base64 数据 URL）。不返回尺寸信息；MIME 类型由数据 URL 隐含。

### OCR 工作流示例

从文件夹中读取多张图像以进行文本提取：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>screenshots/page1.png</path>
  </file>
  <file>
    <path>screenshots/page2.png</path>
  </file>
  <file>
    <path>screenshots/page3.png</path>
  </file>
</args>
</read_file>
```

**使用方式：**
```
请从这些截图图像中提取所有文本，并将其编译为单个 Markdown 文档。
```

### 设计评审工作流

分析多个设计稿：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>designs/homepage-v1.jpg</path>
  </file>
  <file>
    <path>designs/homepage-v2.jpg</path>
  </file>
  <file>
    <path>designs/mobile-view.png</path>
  </file>
</args>
</read_file>
```

**使用方式：**
```
比较这些设计稿并提供以下方面的反馈：
1. 视觉一致性
2. 移动端适配性
3. 无障碍访问问题
4. UI/UX 改进建议
```

### 支持的图像格式

工具支持以下图像格式：
- PNG
- JPG/JPEG
- GIF
- WebP
- SVG
- BMP
- ICO
- TIFF/TIF
- AVIF

### 图像分析使用场景

1. **文档截图**：从 UI 截图中提取文本并创建文档
2. **错误调试**：分析错误截图以理解问题
3. **设计评审**：比较设计稿并提供视觉反馈
4. **图表分析**：理解架构图和流程图
5. **代码截图**：当文本不可用时从图像中提取代码
6. **UI 测试**：验证视觉元素和布局

---

## 多文件示例

您可以使用增强的 XML 格式同时读取多个文件。

### 同时读取多个完整文件

要一次性读取多个文件的完整内容：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>src/app.ts</path>
  </file>
  <file>
    <path>src/utils.ts</path>
  </file>
  <file>
    <path>src/config.json</path>
  </file>
</args>
</read_file>
```

**模拟输出：**
```xml
<files>
  <file>
    <path>src/app.ts</path>
    <content>
      1 | import React from 'react'
      2 | import { Utils } from './utils'
      // ... 文件其余内容
    </content>
  </file>
  <file>
    <path>src/utils.ts</path>
    <content>
      1 | export class Utils {
      2 |   static formatDate(date: Date): string {
      3 |     // ... 工具函数
    </content>
  </file>
  <file>
    <path>src/config.json</path>
    <content>
      1 | {
      2 |   "apiUrl": "https://api.example.com",
      3 |   "timeout": 5000
      4 | }
    </content>
  </file>
</files>
```

### 读取多个文件的指定行范围

要读取多个文件的特定部分：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>src/app.ts</path>
    <line_range>1-20</line_range>
    <line_range>45-60</line_range>
  </file>
  <file>
    <path>src/utils.ts</path>
    <line_range>10-25</line_range>
  </file>
</args>
</read_file>
```

**模拟输出：**
```xml
<files>
  <file>
    <path>src/app.ts</path>
    <content>
      1 | import React from 'react'
      2 | import { Utils } from './utils'
      ...
      20 | const App = () => {
      
      45 |   const handleSubmit = () => {
      46 |     // 处理表单提交
      ...
      60 |   }
    </content>
  </file>
  <file>
    <path>src/utils.ts</path>
    <content>
      10 |   static formatDate(date: Date): string {
      11 |     return date.toISOString().split('T')[0]
      ...
      25 |   }
    </content>
  </file>
</files>
```

### 处理混合结果（部分文件被拒绝/阻止）

当某些文件被批准而其他文件被拒绝或阻止时：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>src/app.ts</path>
  </file>
  <file>
    <path>.env</path>
  </file>
  <file>
    <path>src/secret-config.ts</path>
  </file>
</args>
</read_file>
```

**模拟输出：**
```xml
<files>
  <file>
    <path>src/app.ts</path>
    <content>
      1 | import React from 'react'
      2 | // ... 文件内容读取成功
    </content>
  </file>
  <file>
    <path>.env</path>
    <error>被 .rooignore 规则拒绝访问</error>
  </file>
  <file>
    <path>src/secret-config.ts</path>
    <error>用户拒绝了文件访问权限</error>
  </file>
</files>
```

### 批量审批界面

当请求多个文件时，您将看到一个批量审批界面，允许您：

- **全部批准**：授予对所有请求文件的访问权限
- **全部拒绝**：拒绝对所有请求文件的访问权限
- **单独控制**：覆盖特定文件的决定
- **文件预览**：点击文件标题在编辑器中打开它们

该界面清晰地显示每个文件路径，让您在授予权限之前轻松了解 Zoo 想要访问的内容。

### 混合内容类型

您可以在单个请求中读取不同类型的文件：

**输入：**
```xml
<read_file>
<args>
  <file>
    <path>README.md</path>
  </file>
  <file>
    <path>architecture-diagram.png</path>
  </file>
  <file>
    <path>config.json</path>
  </file>
  <file>
    <path>requirements.pdf</path>
  </file>
</args>
</read_file>
```

这允许 Zoo 在一个上下文中分析文档、可视化图表、配置和规范。

---

## 故障排除

- 范围读取返回错误
  - 原因：`offset` 或 `limit` 值无效（例如非正整数）。
  - 修复：在 slice 模式中使用 `offset`（基于 1 的起始行）和 `limit`（返回的最大行数）作为正整数；或在 indentation 模式中使用 `anchor_line`；或使用包含 `line_range` 条目的多文件 `args` 格式。
  - 预防：跨多个文件进行有针对性的读取时，优先使用包含 `line_range` 的多文件 `args` 格式。

- 大文件返回预览
  - 原因：文件超出令牌预算或大文件令牌化阈值；返回了预览。
  - 修复：使用 `line_range` 仅请求您需要的部分；减少请求范围。
  - 预防：在设置中调整 `maxReadFileLine`，或在大文件上优先使用有针对性的范围。

- 图片未显示
  - 原因：模型可能不支持图片，或图片限制超出（每张图片 5MB；每次请求总计 20MB）。
  - 修复：切换到支持视觉的模型；减小图片大小；请求更少/更小的图片。
  - 预防：保持图片在限制范围内并使用支持的格式（PNG、JPG/JPEG、GIF、WebP、SVG、BMP、ICO、TIFF/TIF、AVIF）。