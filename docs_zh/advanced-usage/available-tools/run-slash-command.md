---
description: 执行预定义的斜杠命令，为常见任务提供模板化指令，支持 Zoo Code 中的内置、全局和项目特定命令。
keywords:
  - run_slash_command
  - slash commands
  - command templates
  - Zoo Code tools
  - workflow automation
  - instruction templates
  - custom commands
  - experimental feature
---

# run_slash_command

:::warning 实验性功能
`run_slash_command` 工具是一项实验性功能，必须在设置中明确启用。导航至设置 > 实验性设置，并启用“运行斜杠命令”以使用此工具。
:::

`run_slash_command` 工具执行预定义的斜杠命令以检索特定指令或内容模板。这些命令充当常见任务的可重用指令集，提供 Zoo 可解释和执行的详细指导。命令可在三个级别定义，并具有清晰的优先级层次结构：项目 > 全局 > 内置。

---

## 参数

该工具接受以下参数：

- `command`（必需）：要执行的斜杠命令名称（不带前导斜杠）
- `args`（可选）：传递给命令的其他参数或上下文

---

## 功能说明

此工具检索并执行定义为命令目录中 markdown 文件的指令模板。它通过共享命令模板实现标准化工作流、可重用任务指令和团队范围内的一致性。该工具验证实验性标志状态，通过优先级层次结构解析命令，并返回 Zoo 可解释的格式化指令。

---

## 使用场景

- 执行需要一致步骤的标准化工作流时
- 检索项目特定或团队范围的指令模板时
- 使用分析和文档初始化代码库时
- 将复杂的多步骤流程作为单个命令访问时
- 维护团队开发实践的一致性时

---

## 关键特性

- **三级命令系统**：内置、全局 (~/.roo/commands/) 和项目特定 (.roo/commands/) 命令
- **优先级层次结构**：项目命令覆盖全局命令，全局命令覆盖内置命令
- **基于 Markdown 的模板**：简单的 `.md` 文件，可选 YAML frontmatter 用于元数据
- **动态参数**：传递上下文特定参数以自定义命令执行
- **自动发现**：命令自动从其各自的目录中找到
- **安全执行**：命令仅为需要用户批准的文本指令，而非可执行代码
- **元数据支持**：可选 frontmatter 用于描述和参数提示
- **错误恢复**：优雅处理，提供有用的错误消息和命令建议
- **无需注册**：只需将 `.md` 文件放置在命令目录中

---

## 要求

此工具需要明确启用：

1. 打开 VS Code 设置
2. 导航至实验性设置
3. 启用“运行斜杠命令”
4. 如有必要，重启 VS Code

---

## 限制

- **实验性状态**：默认禁用该功能，需要选择启用
- **仅文本指令**：命令提供指令，而非直接代码执行
- **需要批准**：所有命令执行都需要用户批准
- **基于目录**：命令必须位于特定目录位置
- **区分大小写**：命令名称匹配区分大小写
- **单个命令**：每次工具调用只能执行一个命令

---

## 工作原理

当调用 `run_slash_command` 工具时，它遵循以下流程：

1. **实验性标志验证**：
   - 检查是否启用了 `runSlashCommand` 实验
   - 如果功能被禁用，返回描述性错误
   - 提供启用功能的说明

2. **参数处理**：
   - 验证必需的 `command` 参数
   - 捕获用于命令自定义的可选 `args`
   - 为缺失参数增加错误计数

3. **命令解析**：
   - 首先搜索项目目录 (`.roo/commands/`)
   - 回退到全局目录 (`~/.roo/commands/`)
   - 最后检查内置命令
   - 如果命令不存在，返回 undefined

4. **命令加载**：
   - 读取命令的 markdown 文件
   - 使用 `gray-matter` 解析可选的 YAML frontmatter
   - 提取描述和参数提示（如果存在）
   - 返回不带 frontmatter 的命令内容

5. **响应格式化**：
   - 包括命令名称和源位置
   - 添加描述和参数提示（如果可用）
   - 显示提供的参数以提供上下文
   - 返回完整命令内容以供解释

6. **错误处理**：
   - 如果请求的命令未找到，列出可用命令
   - 提供带有替代方案的有用错误消息
   - 跟踪连续错误以识别错误模式

---

## 命令结构

### 文件格式

命令是放置在指定目录中的 markdown 文件：

```markdown
---
description: 此命令功能的简要描述
argument-hint: 此命令接受的参数
---

# 命令内容

任务的详细说明写在这里。
这可以包括：
- 分步程序
- 代码模板
- 配置示例
- 最佳实践
```

### 命名约定

- 文件名成为命令名称
- 使用 `.md` 扩展名
- 示例：`deploy.md` 创建 `/deploy` 命令
- 区分大小写匹配

### 目录位置

1. **内置命令**：硬编码在源代码中
2. **全局命令**：`~/.roo/commands/`
3. **项目命令**：`<project-root>/.roo/commands/`

---

## 内置命令

### /init 命令

当前唯一的内置命令分析您的代码库并创建文档：

- 分析项目结构和架构
- 创建 AGENTS.md 文档文件
- 识别编码模式和约定
- 记录非显而易见的实现细节
- 提供对 AI 友好的项目上下文

---

## 创建自定义命令

### 分步指南

1. **创建命令目录**：
   ```bash
   # 对于项目特定命令
   mkdir -p .roo/commands
   
   # 对于全局命令
   mkdir -p ~/.roo/commands
   ```

2. **创建命令文件**：
   ```bash
   # 创建一个部署命令
   touch .roo/commands/deploy.md
   ```

3. **添加命令内容**：
   ```markdown
   ---
   description: 将应用程序部署到生产环境
   argument-hint: 环境名称（staging、production）
   ---
   
   ## 部署流程
   
   1. 运行测试套件以确保所有测试通过
   2. 使用优化构建生产包
   3. 为目标更新环境变量
   4. 部署到指定环境
   5. 运行部署后健康检查
   6. 更新部署文档
   ```

4. **使用命令**：
   命令立即可用，无需注册。

---

## 命令优先级系统

当存在多个同名命令时：

1. **项目级别**（最高优先级）
   - 位于 `.roo/commands/`
   - 允许项目特定覆盖
   - 提交到版本控制以进行团队共享

2. **全局级别**（中等优先级）
   - 位于 `~/.roo/commands/`
   - 跨所有项目共享
   - 用户特定自定义

3. **内置级别**（最低优先级）
   - 硬编码在扩展中
   - 提供默认功能
   - 始终作为回退可用

---

## 使用示例

- 初始化新项目时，Zoo 执行 `/init` 以分析代码库结构并创建综合文档。
- 部署应用程序时，Zoo 检索特定于项目基础设施的标准化部署指令。
- 实现功能时，Zoo 通过自定义命令访问团队约定的模式和最佳实践。
- 设置开发环境时，Zoo 一致地遵循项目特定的设置指令。
- 执行代码审查时，Zoo 使用定义为命令的标准化审查清单。

---

## 使用示例

执行内置初始化命令：
```xml
<run_slash_command>
<command>init</command>
</run_slash_command>
```

使用带有参数的自定义部署命令：
```xml
<run_slash_command>
<command>deploy</command>
<args>production environment with zero-downtime strategy</args>
</run_slash_command>
```

执行带有特定焦点的测试命令：
```xml
<run_slash_command>
<command>test</command>
<args>focus on integration tests for authentication module</args>
</run_slash_command>
```

运行项目特定的构建命令：
```xml
<run_slash_command>
<command>build</command>
<args>optimized for production with source maps</args>
</run_slash_command>
```

访问团队编码标准：
```xml
<run_slash_command>
<command>standards</command>
<args>TypeScript and React best practices</args>
</run_slash_command>
```

---

## 最佳实践

### 命令设计

1. **清晰的命名**：使用描述性、面向操作的名称
2. **全面的说明**：包含所有必要步骤
3. **参数灵活性**：设计命令使其能够在有参数或无参数的情况下工作
4. **元数据使用**：始终包含描述和参数提示
5. **版本控制**：提交项目命令以确保团队一致性

### 组织策略

1. **分类**：使用前缀对相关命令进行分组（例如 `test-unit`、`test-integration`）
2. **文档**：在命令目录中维护 README 文件
3. **模板**：为常见模式创建模板命令
4. **覆盖**：使用项目级命令自定义全局命令
5. **维护**：定期审查和更新命令内容

### 团队协作

1. **标准化**：在全局目录中定义团队范围的命令
2. **项目特定**：使用项目级自定义覆盖
3. **文档**：记录可用命令及其用法
4. **审查流程**：在代码审查中包含命令更改
5. **培训**：在团队成员之间共享命令知识

---

## 故障排除

### 常见问题

**功能未启用**：
- 错误："Run slash command is an experimental feature that must be enabled in settings"
- 解决方案：在实验性设置中启用“运行斜杠命令”

**命令未找到**：
- 错误："Command 'X' not found. Available commands: Y, Z"
- 解决方案：检查命令名称拼写和可用命令列表

**缺少参数**：
- 错误：在连续错误计数器中跟踪
- 解决方案：提供必需的 `command` 参数

### 调试命令

1. **验证文件位置**：确保 `.md` 文件位于正确的目录中
2. **检查文件名**：命令名称必须与不带扩展名的文件名匹配
3. **验证 Frontmatter**：确保 YAML frontmatter 格式正确
4. **测试解析**：尝试在不同级别使用相同的命令名称以测试优先级
5. **审查内容**：确保命令内容格式正确的 Markdown