---
description: 了解 use_mcp_tool 如何集成外部 MCP 服务器，为 Zoo Code 提供专用工具、领域特定功能和外部服务。
keywords:
  - use_mcp_tool
  - MCP tools
  - Model Context Protocol
  - external tools
  - Zoo Code integration
  - MCP servers
  - domain tools
  - tool extension
  - VS Code AI
---

# use_mcp_tool

`use_mcp_tool` 工具支持与连接的模型上下文协议（MCP）服务器提供的外部工具进行交互。它通过标准化协议为 Zoo 提供领域特定功能，扩展其能力。

---

## 参数

该工具接受以下参数：

- `server_name`（必需）：提供工具的 MCP 服务器名称
- `tool_name`（必需）：要执行的工具名称
- `arguments`（必需/可选）：包含工具输入参数的 JSON 对象，遵循工具的输入架构。对于不需要输入的工具，此参数可能为可选。

---

## 功能说明

此工具允许 Zoo 访问外部 MCP 服务器提供的专用功能。每个 MCP 服务器可提供多个具有独特能力的工具，从而扩展 Zoo 的内置功能。系统会根据架构验证参数、管理服务连接，并处理各种内容类型的响应（文本、图像、资源）。

---

## 使用场景

- 当需要核心工具中不可用的专用功能时
- 当需要执行领域特定操作时
- 当需要与外部系统或服务集成时
- 当处理需要特定处理或分析的数据时
- 当通过标准化接口访问专有工具时

---

## 主要特性

- 通过 `@modelcontextprotocol/sdk` 库使用标准化的 MCP 协议
- 支持多种传输机制（StdioClientTransport、StreamableHTTPClientTransport 和 SSEClientTransport）
- 在客户端和服务器端使用 Zod 架构验证参数
- 处理多种响应内容类型：文本、图像和资源引用
- 管理服务器生命周期，当服务器代码更改时自动重启
- 提供“始终允许”机制，绕过可信工具的审批流程
- 与配套的 `access_mcp_resource` 工具配合使用，实现资源检索
- 对失败操作进行适当的错误跟踪和处理
- 支持可配置的超时设置（1-3600 秒，默认：60 秒）
- 允许文件监视器自动检测并重新加载服务器更改

---

## 局限性

- 依赖外部 MCP 服务器的可用性和连接状态
- 仅限于已连接服务器提供的工具
- 不同 MCP 服务器的工具能力各不相同
- 网络问题可能影响可靠性和性能
- 执行前需要用户批准（除非在“始终允许”列表中）
- 无法同时执行多个 MCP 工具操作

---

## 服务器配置

MCP 服务器可在全局或项目级别进行配置：

- **全局配置**：通过 VS Code 中的 Zoo Code 扩展设置进行管理。这些设置适用于所有项目，除非被覆盖。
- **项目级配置**：在项目根目录下的 `.roo/mcp.json` 文件中定义。
 - 这允许项目特定的服务器设置。
 - 如果项目级服务器与全局服务器同名，则项目级服务器优先。
 - 由于 `.roo/mcp.json` 可以提交到版本控制，因此便于与团队共享配置。

---

## 工作原理

当调用 `use_mcp_tool` 工具时，系统遵循以下流程：

1. **初始化和验证**：
   - 系统验证 MCP 中心是否可用
   - 确认指定的服务器存在并已连接
   - 验证请求的工具在服务器上存在
   - 根据工具的架构定义验证参数
   - 从服务器配置中提取超时设置（默认：60 秒）

2. **执行和通信**：
   - 系统选择合适的传输机制：
     - `StdioClientTransport`：通过标准 I/O 与本地进程通信
     - `SSEClientTransport`：通过服务器发送事件与 HTTP 服务器通信
     - `StreamableHTTPClientTransport`：通过流式 HTTP 事件与 HTTP 服务器通信
   - 发送包含已验证的服务器名称、工具名称和参数的请求
   - 使用 `@modelcontextprotocol/sdk` 库进行标准化交互
   - 使用超时处理跟踪请求执行，防止操作挂起

3. **响应处理**：
   - 响应可包含多种内容类型：
     - 文本内容：纯文本响应
     - 图像内容：带有 MIME 类型信息的二进制图像数据
     - 资源引用：访问服务器资源的 URI（与 `access_mcp_resource` 配合使用）
   - 系统检查 `isError` 标志以确定是否需要错误处理
   - 结果被格式化以便在 Zoo 界面中显示

4. **资源和错误处理**：
   - 系统使用 WeakRef 模式防止内存泄漏
   - 连续错误计数器跟踪和管理错误
   - 文件监视器监控服务器代码更改并触发自动重启
   - 安全模型要求批准工具执行，除非在“始终允许”列表中

---

## 安全和权限

MCP 架构提供以下安全功能：

- 用户必须在执行前批准工具使用（默认情况下）
- 特定工具可在“始终允许”列表中标记为自动批准
- 使用 Zod 架构验证服务器配置以确保完整性
- 可配置的超时设置防止操作挂起（1-3600 秒）
- 可通过 UI 启用或禁用服务器连接

---

## 使用示例场景

- 使用服务器端处理工具分析专用数据格式
- 通过外部服务器托管的 AI 模型生成图像或其他媒体
- 执行复杂的领域特定计算，而无需本地实现
- 通过受控接口访问专有 API 或服务
- 从专用数据库或数据源检索数据

---

## 使用示例

请求天气预报数据（文本响应）：
```
<use_mcp_tool>
<server_name>weather-server</server_name>
<tool_name>get_forecast</tool_name>
<arguments>
{
  "city": "San Francisco",
  "days": 5,
  "format": "text"
}
</arguments>
</use_mcp_tool>
```

使用专用工具分析源代码（返回 JSON）：
```
<use_mcp_tool>
<server_name>code-analysis</server_name>
<tool_name>complexity_metrics</tool_name>
<arguments>
{
  "language": "typescript",
  "file_path": "src/app.ts",
  "include_functions": true,
  "metrics": ["cyclomatic", "cognitive"]
}
</arguments>
</use_mcp_tool>
```

生成具有特定参数的图像：
```
<use_mcp_tool>
<server_name>image-generation</server_name>
<tool_name>create_image</tool_name>
<arguments>
{
  "prompt": "A futuristic city with flying cars",
  "style": "photorealistic",
  "dimensions": {
    "width": 1024,
    "height": 768
  },
  "format": "webp"
}
</arguments>
</use_mcp_tool>
```

通过返回资源引用的工具访问资源：
```
<use_mcp_tool>
<server_name>database-connector</server_name>
<tool_name>query_and_store</tool_name>
<arguments>
{
  "database": "users",
  "type": "select",
  "fields": ["name", "email", "last_login"],
  "where": {
    "status": "active"
  },
  "store_as": "active_users"
}
</arguments>
</use_mcp_tool>
```

无需参数的工具：
```
<use_mcp_tool>
<server_name>system-monitor</server_name>
<tool_name>get_current_status</tool_name>
<arguments>
{}
</arguments>
</use_mcp_tool>
```