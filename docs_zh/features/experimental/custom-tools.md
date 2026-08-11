---
description: 定义 TypeScript/JavaScript 工具来扩展 Zoo 内置工具之外的功能，支持 npm 依赖和每个工具的环境变量。
keywords:
  - experimental features
  - custom tools
  - TypeScript tools
  - JavaScript tools
  - tool extension
  - defineCustomTool
  - workflow automation
  - npm dependencies
  - environment variables
---
# 自定义工具

定义 TypeScript 或 JavaScript 工具，让 Zoo 可以像调用内置工具一样调用它们——将团队工作流标准化，而不是每次任务都重复提示相同的步骤。

:::warning 实验性功能
自定义工具是一项实验性功能。启用后，自定义工具会被**自动批准**——Zoo 在运行它们之前不会请求许可。只有在信任工具代码的情况下才启用此功能。
:::

---

## 功能说明

自定义工具让你可以将特定于项目的操作编写成 TypeScript/JavaScript 文件，Zoo 会像调用 [`read_file()`](/basic-usage/how-tools-work) 或 [`execute_command()`](/basic-usage/how-tools-work) 一样调用这些工具。将工具 schema 与你的仓库一起发布，这样团队成员就不需要反复解释相同的工作流步骤。工具会通过 Zod 进行验证，并自动从 TypeScript 转译。

---

## 如何创建工具

工具位于 `.roo/tools/`（项目特定）或 `~/.roo/tools/`（全局）目录中，以 `.ts` 或 `.js` 文件形式存在。后面目录中的工具可以覆盖前面目录中的工具。

#### 基本结构

```typescript
import { parametersSchema as z, defineCustomTool } from "@roo-code/types"

export default defineCustomTool({
  name: "tool_name",
  description: "工具的功能描述（显示给 AI）",
  parameters: z.object({
    param1: z.string().describe("参数描述"),
    param2: z.number().describe("另一个参数"),
  }),
  async execute(args, context) {
    // args 是类型安全且经过验证的
    // context 提供：mode, task
    return "结果字符串，显示给 AI"
  }
})
```

#### 你需要定义的内容

- **`name`**：Zoo 在其可用工具列表中看到的工具名称
- **`description`**：显示给 AI，以便它知道何时调用该工具
- **`parameters`**：Zod schema，转换为 JSON Schema 用于验证
- **`execute`**：异步函数，返回字符串结果给 Zoo

工具会动态加载并通过 esbuild 转译。文件更改时的自动重新加载不可靠——使用**刷新自定义工具**命令可立即获取更改。

---

## 启用功能

1. 打开 Zoo Code 设置（右上角齿轮图标）
2. 转到"实验性"选项卡
3. 切换"启用自定义工具"

<img src="/img/custom-tools/custom-tools.png" alt="在实验性设置中启用自定义工具开关" width="400" />

**重要提示：**启用后，自定义工具会被**自动批准**——Zoo 会直接运行它们而不会询问。如果不信任工具代码，请禁用此功能。

---

## 工具目录

- 工作区中的 **`.roo/tools/`**：与团队共享的项目特定工具
- 主文件夹中的 **`~/.roo/tools/`**：跨所有项目的个人工具

两个目录中的工具都会被加载。`.roo/tools/` 中同名的工具会覆盖 `~/.roo/tools/` 中的工具。

---

## 使用 npm 依赖

自定义工具可以使用 npm 包。在与工具相同的文件夹中安装依赖，导入会正常解析。

```bash
# 从你的工具目录
cd .roo/tools/
npm init -y
npm install axios lodash
```

然后在工具中导入：

```typescript
import { parametersSchema as z, defineCustomTool } from "@roo-code/types"
import axios from "axios"

export default defineCustomTool({
  name: "fetch_api",
  description: "从 API 端点获取数据",
  parameters: z.object({
    url: z.string().describe("API 端点 URL"),
  }),
  async execute({ url }) {
    const response = await axios.get(url)
    return JSON.stringify(response.data, null, 2)
  }
})
```

---

## 每个工具的环境变量

Zoo 会将工具目录中的 `.env` 和 `.env.*` 文件复制到工具的缓存文件夹中，以便你的工具在运行时加载它们。**Zoo 不会自动将这些变量注入到 `process.env` 中**——你的工具必须自己加载它们。

**设置方法：**

1. 在与工具相同的目录创建 `.env` 文件：
   ```
   .roo/tools/
   ├── my-tool.ts
   ├── .env          # 加载时复制到缓存目录
   └── package.json
   ```

2. 添加你的密钥：
   ```bash
   # .roo/tools/.env
   SLACK_WEBHOOK_URL=https://hooks.slack.com/services/XXX
   API_SECRET=your-secret-key
   ```

3. 在你的工具中使用 `dotenv` 和 `__dirname` 加载 `.env`：
   ```typescript
   import { parametersSchema as z, defineCustomTool } from "@roo-code/types"
   import dotenv from "dotenv"
   import path from "path"

   // 从工具的缓存目录加载 .env
   dotenv.config({ path: path.join(__dirname, ".env") })

   export default defineCustomTool({
     name: "notify_slack",
     description: "向 Slack 发送通知",
     parameters: z.object({
       message: z.string().describe("要发送的消息"),
     }),
     async execute({ message }) {
       const webhookUrl = process.env.SLACK_WEBHOOK_URL
       if (!webhookUrl) {
         return "错误：.env 中未设置 SLACK_WEBHOOK_URL"
       }
       
       const response = await fetch(webhookUrl, {
         method: "POST",
         headers: { "Content-Type": "application/json" },
         body: JSON.stringify({ text: message }),
       })
       
       return response.ok ? "消息已发送" : `失败：${response.status}`
     }
   })
   ```

**为什么使用 `__dirname`？** Zoo 会将你的 `.env` 文件与转译后的工具一起复制到缓存目录中。使用 `__dirname` 可以确保你的工具无论原始定义位置在哪里，都能在正确的位置找到 `.env` 文件。

**安全性：**确保你的 `.env` 文件被版本控制忽略，以保护密钥安全。

---

## 限制

- **无批准提示**：启用功能后工具会被自动批准——为了便利性而做出的安全权衡
- **仅限字符串结果**：工具必须返回字符串（Zoo 协议约束）
- **无交互式输入**：工具无法在执行过程中提示用户
- **缓存失效**：工具更新可能需要重新加载窗口

**与 MCP 对比：**[MCP](/features/mcp/overview) 用于外部服务（搜索、API）。自定义工具用于你直接控制的仓库内逻辑。MCP 更具扩展性；自定义工具对于特定于项目的操作更轻量级。
