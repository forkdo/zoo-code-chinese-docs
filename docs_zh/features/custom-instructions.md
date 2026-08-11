---
description: 了解如何使用自定义指令，根据你的偏好、编码风格和项目需求来定制 Zoo Code 的行为。
keywords:
  - custom instructions
  - personalization
  - AI customization
  - coding preferences
  - project rules
---

# 自定义指令

自定义指令允许你个性化定制 Zoo 的行为，提供具体的指导，塑造其响应方式、编码风格和决策流程。

:::info 指令文件位置
你可以通过全局规则（应用于所有项目）、工作区规则（特定于项目）或通过 Prompts 标签页界面来提供自定义指令。

**全局规则目录：** 自动应用于所有项目。
*   **Linux/macOS：** `~/.roo/rules/` 和 `~/.roo/rules-{modeSlug}/`
*   **Windows：** `%USERPROFILE%\.roo\rules\` 和 `%USERPROFILE%\.roo\rules-{modeSlug}\`

**工作区规则：** 仅应用于当前项目，当与全局规则冲突时，工作区规则优先。
*   **首选方法：目录（`.roo/rules/`）**
    ```
    .
    ├── .roo/
    │   └── rules/          # 工作区范围的规则
    │       ├── 01-general.md
    │       └── 02-coding-style.txt
    └── ... (其他项目文件)
    ```
*   **备用方法：单个文件（`.roorules`）**
    ```
    .
    ├── .roorules           # 工作区范围的规则（单个文件）
    └── ... (其他项目文件)
    ```

**特定模式的指令：** 仅应用于特定模式（例如 `code`）。
*   **首选方法：目录（`.roo/rules-{modeSlug}/`）**
    ```
    .
    ├── .roo/
    │   └── rules-code/     # “code”模式的规则
    │       ├── 01-js-style.md
    │       └── 02-ts-style.md
    └── ... (其他项目文件)
    ```
*   **备用方法：单个文件（`.roorules-{modeSlug}`）**
    ```
    .
    ├── .roorules-code      # “code”模式的规则（单个文件）
    └── ... (其他项目文件)
    ```

规则加载顺序：先加载全局规则，然后加载工作区规则。如果存在冲突，工作区规则优先。详情请参阅[全局规则目录](#global-rules-directory)。
::}

---

## 什么是自定义指令？

自定义指令定义了超出 Zoo 基本角色定义的特定行为、偏好和约束。示例包括编码风格、文档标准、测试要求和工作流指南。

---

## 设置自定义指令

### 全局自定义指令

这些指令适用于所有工作区，无论你在哪个项目中工作，都能保持你的偏好设置。

**设置方法：**

<img src="/img/custom-instructions/custom-instructions.png" alt="Zoo Code Prompts 标签页显示全局自定义指令界面" width="600" />
1.  **打开 Prompts 标签页：** 点击 Zoo Code 顶部菜单栏中的 <Codicon name="notebook" /> 图标
2.  **找到对应部分：** 找到“所有模式的自定义指令”部分
3.  **输入指令：** 在文本区域中输入你的指令
4.  **保存更改：** 点击“完成”以保存你的更改

### 全局规则目录

全局规则目录功能提供了可重用的规则和自定义指令，可自动应用于你的所有项目。该系统支持全局配置和特定于项目的覆盖。

#### 主要优势

**无全局规则时**：你必须在每个项目中维护单独规则文件：
- 将相同规则复制到每个新项目
- 手动跨多个项目更新规则
- 项目之间缺乏一致性

**有全局规则时**：创建一次规则即可随处使用：
- 全局设置你首选的编码标准
- 在需要时针对特定项目自定义规则
- 在所有工作中保持一致性
- 轻松一次性更新所有项目的规则

#### 目录结构

全局规则目录位置是固定的，无法自定义：

**Linux/macOS：**
```
~/.roo/                           # 你的全局 Zoo 配置
├── rules/                        # 应用于所有项目的通用规则
│   ├── coding-standards.md
│   ├── formatting-rules.md
│   └── security-guidelines.md
├── rules-code/                   # 特定于 Code 模式的规则
│   ├── typescript-rules.md
│   └── testing-requirements.md
├── rules-docs-extractor/         # 文档提取规则
│   └── documentation-style.md
└── rules-{mode}/                 # 其他特定模式的规则
    └── mode-specific-rules.md
```

**Windows：**
```
%USERPROFILE%\.roo\               # 你的全局 Zoo 配置
├── rules\                        # 应用于所有项目的通用规则
│   ├── coding-standards.md
│   ├── formatting-rules.md
│   └── security-guidelines.md
├── rules-code\                   # 特定于 Code 模式的规则
│   ├── typescript-rules.md
│   └── testing-requirements.md
└── rules-{mode}\                 # 其他特定模式的规则
    └── mode-specific-rules.md
```

#### Setting Up Global Rules

1. **创建全局规则目录：**
   ```bash
   # Linux/macOS
   mkdir -p ~/.roo/rules
   
   # Windows
   mkdir %USERPROFILE%\.roo\rules
   ```

2. **添加通用规则** (`~/.roo/rules/coding-standards.md`)：
   ```markdown
   # 全局编码标准
   
   1. 新项目始终使用 TypeScript
   2. 为所有新函数编写单元测试
   3. 使用描述性变量名
   4. 为公共 API 添加 JSDoc 注释
   ```

3. **添加特定模式规则** (`~/.roo/rules-code/typescript-rules.md`)：
   ```markdown
   # TypeScript Code 模式规则
   
   1. 在 tsconfig.json 中使用严格模式
   2. 对于对象形状，优先使用接口而非类型别名
   3. 始终为函数指定返回类型
   ```

#### 可用规则目录

| 目录 | 用途 |
|------|------|
| `rules/` | 应用于所有模式的通用规则 |
| `rules-code/` | 特定于 Code 模式的规则 |
| `rules-docs-extractor/` | 文档提取规则 |
| `rules-architect/` | 系统架构任务规则 |
| `rules-debug/` | 调试工作流规则 |
| `rules-{mode}/` | 任何自定义模式的规则 |

#### 规则加载顺序

规则按此顺序加载：

1. **全局规则**（来自 `~/.roo/`）
2. **项目规则**（来自 `project/.roo/`）- 当与全局规则冲突时优先于全局规则
3. [仅通用] **遗留文件**（工作区根目录 `.roorules`、`.clinerules`）- 仅当未加载任何通用规则目录内容时使用

在每个级别内，特定模式的规则在通用规则之前加载。

### 工作区级指令

这些指令仅适用于当前工作区，允许你为特定项目定制 Zoo Code 的行为。

#### 通过文件/目录设置工作区范围的指令

工作区范围的指令应用于当前项目中的所有模式，可通过文件定义：

*   **首选方法：基于目录（`.roo/rules/`）**
    *   在工作区根目录创建一个名为 `.roo/rules/` 的目录。
    *   将指令文件（例如 `.md`、`.txt`）放入其中。Zoo Code 会递归读取文件（包括子目录），并根据文件名按**字母顺序**将其内容附加到系统提示中。
    *   当此目录存在且包含文件时，其内容将与任何全局规则目录一起加载。
    *   注意：如果 `.roo/rules/` 目录存在但为空，Zoo Code 将转而使用 `.roorules` 文件。
*   **备用方法：基于文件（`.roorules`）**
    *   如果 `.roo/rules/` 不存在或为空，Zoo Code 会在工作区根目录查找单个 `.roorules` 文件。
    *   如果找到，则加载其内容。

#### 特定模式的指令

特定模式的指令可以通过两种独立的方式设置，可同时使用：

1.  **使用 Prompts 标签页：**

<img src="/img/custom-instructions/custom-instructions-2.png" alt="Zoo Code Prompts tab showing mode-specific custom instructions interface" width="600" />
* **打开标签页：** 点击 Zoo Code 顶部菜单栏中的 <Codicon name="notebook" /> 图标
* **选择模式：** 在“Modes”标题下，点击您要自定义的模式对应的按钮
* **输入指令：** 在“Mode-specific Custom Instructions (optional)”下方的文本区域中输入您的指令
* **保存更改：** 点击“Done”保存您的更改

:::info 全局模式规则
如果模式本身是全局性的（而非工作区特定），您为其设置的任何自定义指令也将全局应用于所有工作区中的该模式。
:::

2. **使用规则文件/目录：** 通过文件提供特定于模式的指令：
    *   **首选方法：基于目录（`.roo/rules-{modeSlug}/`）**
        *   在工作区根目录创建一个名为 `.roo/rules-{modeSlug}/` 的目录（例如 `.roo/rules-docs-writer/`）。
        *   将指令文件放入其中（递归加载，包括子目录）。文件按文件名**字母顺序**读取并附加到系统提示中。
        *   如果目录存在且包含文件，此方法优先于特定模式的回退文件方法。
    *   **回退方法：基于文件（`.roorules-{modeSlug}`）**
        *   如果 `.roo/rules-{modeSlug}/` 不存在或为空，Zoo Code 会在工作区根目录查找单个 `.roorules-{modeSlug}` 文件（例如 `.roorules-code`）。
        *   如果找到，其内容将加载到该模式。

来自“Prompts”标签页、全局规则、工作区规则和特定于模式的规则的指令都会被组合。请参见下面的确切顺序。

---

## 指令如何组合

指令以以下确切格式放置在系统提示中：

```
====
USER'S CUSTOM INSTRUCTIONS

The following additional instructions are provided by the user, and should be followed to the best of your ability without interfering with the TOOL USE guidelines.

Language Preference:
[Language preference if set]

Global Instructions:
[Global Instructions from Prompts Tab]

Mode-specific Instructions:
[Mode-specific Instructions from Prompts Tab for the current mode]

Rules:

# Rules from rules-{modeSlug} directories:
[Contents of ALL files from ~/.roo/rules-{modeSlug}/ AND .roo/rules-{modeSlug}/ if they exist]

# Rules from .roorules-{modeSlug}:
[Contents of .roorules-{modeSlug} file if no mode-specific directories have files]

# Rules from .rooignore:
[.rooignore-related instructions if applicable]

# Agent Rules Standard (AGENTS.md):
[Contents of AGENTS.md or AGENT.md from workspace root if present and enabled]

# Rules from rules directories:
[Contents of ALL files from ~/.roo/rules/ AND .roo/rules/ if they exist]

# Rules from .roorules:
[Contents of .roorules file if no general rules directories have files]

====
```

*注意：系统会从所有适用的目录（全局 `~/.roo/` 和工作区 `.roo/`）加载规则，而不仅仅是第一个包含文件的目录。特定于模式的规则出现在通用规则之前。基于目录的规则仅在确定使用哪种方法时优先于基于文件的回退，但所有适用的目录都会被读取。*

---

## Global Rules Directory

* **文件位置：** 首选方法使用 `.roo/` 内的目录（`.roo/rules/` 和 `.roo/rules-{modeSlug}/`）。回退方法使用直接位于工作区根目录的单个文件（`.roorules` 和 `.roorules-{modeSlug}`）。
* **递归读取：** 规则目录会递归读取，包括子目录中的所有文件
* **文件过滤：** 系统会自动排除缓存和临时文件（`.DS_Store`、`*.bak`、`*.cache`、`*.log`、`*.tmp`、`Thumbs.db` 等）
* **空文件：** 空或缺失的规则文件会被静默跳过
* **源文件头：** 基于目录的规则包含每个文件的头 `# Rules from {absolute path}:`，而基于文件的规则包含 `# Rules from {filename}:` 头
* **聚合：** 全局和工作区规则目录会针对特定于模式的规则和通用规则进行聚合（不是二选一）
* **排序：** 文件仅按基本名称排序，不区分大小写
* **头路径：** 头路径是绝对路径，并遵循符号链接
* **规则交互：** 特定于模式的规则是对全局规则的补充，而不是替换
* **符号链接：** 文件和目录的符号链接都完全支持，最大解析深度为 5，以防止无限循环

---

## AGENTS.md 支持

Zoo Code 还支持从工作区根目录的 `AGENTS.md`（或 `AGENT.md` 作为回退）文件加载规则：

* **目的：** 为 AI 行为提供特定于代理的规则和指南
* **位置：** 必须位于工作区根目录
* **加载：** 默认自动加载。要禁用 AGENTS.md 加载，请在 VSCode 设置中将 `"roo-cline.useAgentRules": false`
* **设置：** `roo-cline.useAgentRules`（默认值：true）
* **优先级：** 如果两者都存在，`AGENTS.md` 优先于 `AGENT.md`
* **优先级：** 在特定于模式的规则和 `.rooignore` 之后加载，在来自 `~/.roo/rules` 和 `.roo/rules` 的通用规则之前加载
* **头：** 添加到系统提示中，头为 `# Agent Rules Standard (AGENTS.md):` 或 `(AGENT.md):`
* **空文件：** 空或仅包含空白字符的 `AGENTS.md` 会被忽略
* **符号链接：** 文件或目录的符号链接在读取前会被解析

此功能允许团队维护标准化的 AI 代理行为规则，这些规则可以与项目代码一起进行版本控制。

---

## 自定义指令示例

* “始终使用空格进行缩进，宽度为 4 个空格”
* “变量名使用 camelCase”
* “为所有新函数编写单元测试”
* “在提供代码之前解释您的推理”
* “专注于代码的可读性和可维护性”
* “优先使用社区中最常见的库”
* “向网站添加新功能时，确保它们是响应式且可访问的”

:::tip 专业提示：团队标准化
对于团队环境，请考虑以下方法：

**项目标准：** 使用版本控制下的工作区 `.roo/rules/` 目录来标准化特定项目的 Zoo 行为。这确保了团队成员之间一致的代码风格和开发工作流程。

**组织标准：** 使用全局规则（`~/.roo/rules/`）来建立适用于所有项目的组织范围的编码标准。团队成员可以设置相同的全局规则，以确保所有工作的一致性。

**混合方法：** 将组织标准的全局规则与特定于项目的特定于工作区的规则相结合，以满足特定于项目的需求。当规则冲突时，工作区规则优先。

基于目录的方法比单个 `.roorules` 文件提供了更好的组织，并支持全局和项目级别的定制。
:::

---

## 与自定义模式结合

对于高级定制，请与[自定义模式](/features/custom-modes)结合使用，以创建具有特定工具访问权限、文件限制和定制指令的专门环境。
