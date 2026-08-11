---
sidebar_label: 导入/导出/重置设置
description: 通过导出、导入或重置为默认设置来管理您的 Zoo Code 设置。
keywords:
  - settings management
  - import settings
  - export settings
  - reset settings
  - configuration backup
  - auto import
---

# 导入、导出和重置设置

Zoo Code 允许您通过导出、导入和重置选项有效地管理配置设置。这些功能对于备份您的设置、与他人共享配置或在需要时恢复默认设置非常有用。

您可以在 Zoo Code 设置页面的底部找到这些选项，可通过 Zoo Code 聊天视图中的齿轮图标 (<i class="codicon codicon-gear"></i>) 访问。

<img src="/img/settings-management/settings-management.png" alt="Zoo Code 设置中的导出、导入和重置按钮" width="400" />
*图片：导出、导入和重置按钮。*

---

## 导出设置

点击 **导出** 按钮可将当前 Zoo Code 设置保存为 JSON 文件。

*   **导出内容：** 该文件包含您配置的 API 提供商配置文件和全局设置（UI 首选项、模式配置、上下文设置等）。
*   **安全警告：** 导出的 JSON 文件包含您配置的**所有** API 提供商配置文件和全局设置。**关键的是，这包括以明文形式存储的 API 密钥**。请将该文件视为高度敏感文件。请勿公开分享或与不可信的人员共享，因为它会授予对您 API 账户的访问权限。
*   **操作流程：**
    1.  点击 **导出**。
    2.  将出现一个文件保存对话框，建议将文件名设为 `roo-code-settings.json`（通常位于 `~/Documents` 文件夹中）。
    3.  选择位置并保存文件。

这将创建您配置的备份或一个可以共享的文件。

---

## Import Settings

点击 **导入** 按钮允许您从之前导出的 JSON 文件加载设置。

*   **操作流程：**
    1.  点击 **导入**。
    2.  将出现一个文件打开对话框。选择您要导入的 `roo-code-settings.json` 文件（或类似名称的文件）。
    3.  Zoo Code 将读取文件，根据预期模式验证其内容，并应用设置。
*   **合并：** 导入设置会**合并**配置。它会根据文件内容添加新的 API 配置文件并更新现有的配置文件和全局设置。它**不会**删除您当前设置中存在但导入文件中缺失的配置。
*   **验证：** 导入会验证文件，但仍可能成功并带有警告。

    - 如果**某些** API 配置文件引用了不再存在的提供商（或以其他方式无效），Zoo Code 会导入其余部分并报告警告。
    - 仅当**所有**配置文件都无效时，导入才会失败。

---

## Automatic Configuration Import

每次启动 VS Code 时，自动从文件导入您的 Zoo Code 设置。这是在多个设备间同步配置或为整个团队标准化设置的强大方式。

### 主要功能
- **轻松同步**：在不同的工作区和设备间保持设置一致。
- **团队标准化**：共享单个配置文件，确保整个团队使用相同的设置。
- **灵活路径**：支持绝对路径，或相对于主目录的路径（例如 `~/Documents/roo-settings.json`）。
- **静默且安全**：如果找不到文件或包含错误，Zoo Code 将正常启动，不会阻止您的工作流程。

### 使用场景

**之前**：每次换到新机器或想要共享设置时，都需要手动导出和导入设置。
- 手动打开设置面板。
- 将当前设置导出到文件。
- 将文件发送给队友或新机器。
- 手动导入文件。

**使用此功能后**：配置一次路径，Zoo Code 会在每次启动时处理其余操作。

### 工作原理

当 VS Code 启动时，Zoo Code 会检查特定设置：`roo-cline.autoImportSettingsPath`。如果此设置包含指向有效 Zoo Code 配置文件（`.json`）的路径，Zoo Code 将自动加载它。

- 导入成功后，您将看到通知：`Successfully imported settings from [your-file-name.json]`。
- 如果文件无效或找不到，您将收到一个非侵入性警告，扩展将使用您上次已知的设置启动。`autoImportSettings` 函数旨在永不阻止扩展激活。

### 配置

要使用此功能，请将以下内容添加到 VS Code 的 `settings.json` 文件中：

1.  **打开您的 `settings.json` 文件**：
    *   使用命令面板 (`Ctrl/Cmd + Shift + P`) 并搜索“首选项：打开用户设置 (JSON)”。

2.  **添加设置**：
    *   添加 `roo-cline.autoImportSettingsPath` 键，并指定配置文件路径。

**示例**：

*   **绝对路径（推荐）**
    ```json
    {
      "roo-cline.autoImportSettingsPath": "/Users/your-username/Documents/dev-configs/roo-code.json"
    }
    ```

*   **主目录路径**（使用 `~`）
    ```json
    {
      "roo-cline.autoImportSettingsPath": "~/roo-code-settings.json"
    }
    ```

*   **要禁用**，只需将路径留空或完全删除该行：
    ```json
    {
      "roo-cline.autoImportSettingsPath": ""
    }
    ```

### 常见问题

**“如果我的文件有错误会怎样？”**
- Zoo Code 将显示带有错误详情的警告通知。扩展将继续使用您之前保存的设置正常加载。

**“Zoo Code 在哪里查找相对路径？”**
- 为了安全和一致性，非绝对或基于主目录的路径将相对于您的主目录解析。

**“我可以用它来管理团队的设置吗？”**
- 是的。将配置文件放在共享位置（如同步的云文件夹或共享网络驱动器），并让每个团队成员指向该文件。
---

## 重置设置

点击 **重置** 按钮将完全清除所有 Zoo Code 配置数据，并将扩展恢复到默认状态。这是一个用于故障排除或重新开始的有破坏性的操作。

*   **警告：** 此操作**不可逆**。它将永久删除所有 API 配置（包括存储在密钥存储中的密钥）、自定义模式、全局设置和任务历史记录。

*   **操作流程：**
    1.  点击红色的 **重置** 按钮。
    2.  将出现一个确认对话框，警告该操作无法撤消。
    3.  点击“是”以确认。

*   **重置内容：**
    *   **API 提供商配置文件：** 所有配置将从设置和密钥存储中删除。
    *   **全局设置：** 所有首选项（UI、模式、批准、浏览器等）将重置为默认值。
    *   **自定义模式：** 所有用户定义的模式将被删除。
    *   **密钥存储：** 所有由 Zoo Code 管理的 API 密钥和其他密钥将被清除。
    *   **任务历史记录：** 当前任务堆栈将被清除。

*   **结果：** Zoo Code 将返回到其初始状态，如同刚安装一样，具有默认设置且没有用户配置。

仅当您确定要删除所有 Zoo Code 数据或在故障排除过程中被指示时才使用此选项。如果您以后可能想要恢复设置，请考虑先导出您的设置。

---

## Command Palette Commands

Zoo Code 提供了若干可通过 VS Code 命令面板（`Ctrl/Cmd + Shift + P`）访问的实用命令。这些命令提供了管理设置和存储的替代方式。

### Set Custom Storage Path

**命令：** `roo-cline.setCustomStoragePath`

打开对话框以设置 Zoo Code 数据的自定义存储目录。默认情况下，Zoo Code 将任务历史、设置及其他数据存储在标准的 VS Code 扩展存储位置。此命令允许您选择替代位置。

**使用场景：**
- **团队协作**：将 Zoo Code 数据存储在共享网络文件夹中，以便团队成员访问相同的任务历史和设置
- **磁盘管理**：将数据保存在特定驱动器上（例如，比主 SSD 更大的辅助驱动器）
- **云同步**：将数据存储在云同步文件夹（Dropbox、OneDrive 等）中，以便跨多台机器同步
- **备份策略**：将数据放置在您定期备份例程所覆盖的位置

**使用方法：**
1. 打开命令面板（`Ctrl/Cmd + Shift + P`）
2. 输入 "Set Custom Storage Path" 或搜索 `roo-cline.setCustomStoragePath`
3. 选择该命令
4. 在文件选择对话框中选择一个目录
5. 重启 VS Code 以使更改生效

**注意：** 此设置也可在 VS Code 设置中配置为 `roo-cline.customStoragePath`。详见下文的 [VS Code 设置参考](#vs-code-settings-reference) 部分。

### Import Settings from File

**命令：** `roo-cline.importSettings`

通过命令面板从 JSON 文件导入 Zoo Code 设置。这是使用设置 UI 中“导入”按钮的替代方式。

**使用方法：**
1. 打开命令面板（`Ctrl/Cmd + Shift + P`）
2. 输入 "Import Settings" 或搜索 `roo-cline.importSettings`
3. 选择该命令
4. 在文件选择对话框中选择您的设置 JSON 文件
5. 设置将被导入并与您当前的配置合并

此命令提供的功能与上文 [导入设置](#import-settings) 部分中描述的“导入”按钮相同。

---

## VS Code Settings Reference

Zoo Code 提供了可通过 VS Code `settings.json` 文件配置的 VS Code 设置。这些设置可对命令执行、任务管理、API 行为、存储、索引和调试进行细粒度控制。

要配置这些设置，请打开您的 VS Code 设置（`Ctrl/Cmd + ,`）并搜索 "roo-cline"，或直接编辑您的 `settings.json` 文件（`Ctrl/Cmd + Shift + P` → "Preferences: Open User Settings (JSON)"）。

### Command & Execution

#### `roo-cline.allowedCommands`
- **类型**：字符串数组
- **默认值**：`["git log", "git diff", "git show"]`
- **描述**：无需批准即可自动执行的命令。当 Zoo Code 请求执行与此列表中的条目相匹配的命令时，它将自动执行而无需提示批准。这对于安全的只读命令非常有用。

#### `roo-cline.deniedCommands`
- **类型**：字符串数组
- **默认值**：`[]`
- **描述**：始终被阻止执行的命令。Zoo Code 将拒绝执行与此列表中的条目相匹配的任何命令，从而提供一种防止潜在危险操作的安全机制。

#### `roo-cline.commandExecutionTimeout`
- **类型**：数字（秒）
- **默认值**：`0`
- **范围**：0-600
- **描述**：命令执行的超时时间（秒）。当设置为大于 0 的值时，运行时间超过该时长的命令将被终止。值为 0 表示无超时（命令可无限期运行）。另请参阅 `commandTimeoutAllowlist` 以豁免特定命令。

#### `roo-cline.commandTimeoutAllowlist`
- **类型**：字符串数组
- **默认值**：`[]`
- **描述**：豁免执行超时的命令。与此列表中的条目相匹配的命令不受 `commandExecutionTimeout` 限制，允许它们在无时间限制的情况下运行。对于已知的长时间运行操作（如构建过程或部署脚本）非常有用。

### Task Management

#### `roo-cline.newTaskRequireTodos`
- **类型**：布尔值
- **默认值**：`false`
- **描述**：启用时，通过 boomerang/子任务创建新任务时要求提供待办列表。这通过强制新任务包含待完成步骤的检查清单，来确保复杂工作的结构化规划。

#### `roo-cline.preventCompletionWithOpenTodos`
- **类型**：布尔值
- **默认值**：`false`
- **描述**：防止在存在未完成任务时完成。启用后，如果待办列表仍有待处理项，Zoo Code 将不允许您将任务标记为完成，从而确保所有计划的工作都已完成。

### API & Network

#### `roo-cline.apiRequestTimeout`
- **类型**：数字（秒）
- **默认值**：`600`
- **范围**：0-3600
- **描述**：API 请求的超时时间（秒）。决定 Zoo Code 在超时前等待 AI 提供商 API 响应的最长时间。值为 0 表示无超时。

### Storage & Import

#### `roo-cline.customStoragePath`
- **类型**：字符串
- **默认值**：`""`（空）
- **描述**：Zoo Code 存储目录的自定义文件路径。默认情况下，Zoo Code 将其数据存储在标准扩展存储位置。使用此设置可指定用于存储任务历史、设置及其他数据的替代目录。

#### `roo-cline.autoImportSettingsPath`
- **类型**：字符串
- **默认值**：`""`（空）
- **描述**：启动时自动导入设置的文件路径。配置后，Zoo Code 将在每次 VS Code 启动时自动从指定的 JSON 文件导入设置。详见上文的 [自动配置导入](#automatic-configuration-import) 部分以获取详细使用说明。

### Code Index

#### `roo-cline.maximumIndexedFilesForFileSearch`
- **类型**：数字
- **默认值**：`10000`
- **范围**：5000-500000
- **描述**：用于文件搜索的最大索引文件数。控制 Zoo Code 将为语义搜索功能建立索引的文件数量上限。较高的值会增加搜索覆盖率，但可能影响性能。

#### `roo-cline.codeIndex.embeddingBatchSize`
- **类型**：数字
- **默认值**：`60`
- **范围**：1-200
- **描述**：代码索引期间嵌入操作的批处理大小。决定在生成语义搜索嵌入时一起处理的代码块数量。较低的值可减少内存使用但增加处理时间；较高的值速度更快但占用更多内存。

### Editor Integration

#### `roo-cline.enableCodeActions`
- **类型**：布尔值
- **默认值**：`true`
- **描述**：控制 Zoo Code 操作是否出现在编辑器上下文菜单和灯泡菜单中。启用后，您可以在编辑器中右键单击或使用灯泡菜单，通过上下文提示将代码选择快速发送到 Zoo Code。

#### `roo-cline.vsCodeLmModelSelector`
- **类型**：对象
- **默认值**：`{}`
- **描述**：VS Code 语言模型 API 提供商选择的配置。允许您指定 vendor 和 family 属性，以控制在选择 VS Code LM API 提供商时使用的语言模型。详见 [VS Code LM API 文档](/providers/vscode-lm)。

### Rules & Instructions

#### `roo-cline.useAgentRules`
- **类型**：布尔值
- **默认值**：`true`
- **描述**：启用加载 AGENTS.md 文件以获取代理特定的指令。启用后，Zoo Code 将在您的项目目录中查找并加载 `AGENTS.md` 文件，为 AI 提供上下文特定的指导。如果您想阻止自动加载这些指令文件，请禁用此选项。

### Debug

#### `roo-cline.debug`
- **类型**：布尔值
- **默认值**：`false`
- **描述**：启用调试模式以输出额外的日志。启用后，Zoo Code 将向控制台输出详细的调试信息，有助于排查问题或了解内部行为。

#### `roo-cline.debugProxy.enabled`
- **类型**：布尔值
- **默认值**：`false`
- **描述**：启用调试代理以拦截 API 请求。启用后，所有 API 请求都将通过调试代理服务器路由，以便您检查和调试 API 通信。

#### `roo-cline.debugProxy.serverUrl`
- **类型**：字符串
- **默认值**：`"http://127.0.0.1:8888"`
- **描述**：调试代理服务器的 URL。指定 `debugProxy.enabled` 为 true 时使用的代理服务器地址。常见的调试代理工具（如 mitmproxy 或 Charles Proxy）通常在此默认地址上运行。

#### `roo-cline.debugProxy.tlsInsecure`
- **类型**：布尔值
- **默认值**：`false`
- **描述**：允许通过调试代理的不安全 TLS 连接。启用后，将忽略证书验证错误，这在使用带有调试代理的自签名证书时是必需的。仅在开发环境中启用此选项。

---

## UI 设置

#### 系统提示上下文切换

控制系统提示中显示哪些上下文信息：

- **包含当前时间** (设置 → 常规)
  - 启用时，将当前时间戳添加到系统提示中
  - 禁用时，从提示中省略时间信息
  - 默认：启用

- **包含当前成本** (设置 → 常规)
  - 启用时，将当前任务成本添加到系统提示中
  - 禁用时，从提示中省略成本信息
  - 默认：启用

**示例影响：**

两者都启用时，系统提示包括：
```
# Current Time
Current time in ISO 8601 UTC format: 2025-10-28T23:06:08.458Z
User time zone: America/Edmonton, UTC-6:00

# Current Cost
$0.14
```

两者都禁用时，这些部分将被省略，在您不需要此上下文时减少 token 使用量。

#### 默认折叠思考消息

- 位置：设置 → 用户界面
- 默认值：已启用（思考消息默认折叠）
- 行为：
  - 已启用（默认）：思考块保持折叠状态，直到您展开它们。
  - 已禁用：思考块默认展开。
- 备注：
  - 全局适用于所有对话。
  - 文本已本地化；标签可能因语言而异。
