---
description: 了解 Zoo Code 推荐的 MCP 服务器，包括 Context7。学习如何通过分步说明安装和配置 MCP 服务器。
keywords:
  - MCP servers
  - Context7
  - Zoo Code MCP
  - MCP installation
  - MCP configuration
  - recommended servers
sidebar_label: 推荐的 MCP 服务器
---

# 推荐的 MCP 服务器

虽然 Zoo Code 可以连接任何遵循 Model Context Protocol (MCP) 规范的服务器，但社区已经构建了一些开箱即用的高质量服务器。本页面精选了我们**积极推荐**的服务器，并提供了分步设置说明，以便您在几分钟内就能投入工作。

> 我们将保持此列表的更新。如果您维护了一个希望我们考虑的服务器，请提交拉取请求。

---

## Context7

`Context7` 是我们的首选通用 MCP 服务器。它提供了一系列高度需求的工具，只需一条命令即可安装，并在所有支持 MCP 的主流编辑器中都有出色的支持。

### 为什么我们推荐 Context7

* **一键安装** – 所有内容都已打包，无需本地构建步骤。
* **跨平台** – 可在 macOS、Windows、Linux 或 Docker 内部运行。
* **积极维护** – Upstash 团队频繁更新。
* **丰富的工具集** – 数据库访问、网络搜索、文本工具等。
* **开源** – 基于 MIT 许可证发布。

---

## 在 Zoo Code 中安装 Context7

有两种常见的方法来注册服务器：

1. **全局配置** – 在每个工作区都可用。
2. **项目级配置** – 与代码一起提交到版本控制。

下面将介绍这两种方法。

### 1. 全局配置

1. 点击 <Codicon name="server" /> 图标，打开 Zoo Code **MCP 设置**面板。
2. 点击**编辑全局 MCP**。
3. 将下面的 JSON 粘贴到 `mcpServers` 对象中并保存。

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

**Windows (cmd.exe) 变体**

```json
{
  "mcpServers": {
    "context7": {
      "type": "stdio",
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

在 **Windows (cmd)** 上，您可能还需要通过 `cmd.exe` 调用 `npx`：

<img src="/img/recommended-mcp-servers/context7-global-setup-fixed.png" alt="Adding Context7 to the global MCP settings" width="600" />

### 2. 项目级配置

如果您希望将配置提交到仓库，请在项目根目录创建一个名为 `.roo/mcp.json` 的文件，并添加相同的代码片段：

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

**Windows (cmd.exe) 变体**

```json
{
  "mcpServers": {
    "context7": {
      "type": "stdio",
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@upstash/context7-mcp@latest"]
    }
  }
}
```

<img src="/img/recommended-mcp-servers/context7-project-setup-fixed.png" alt="Adding Context7 to a project-level MCP file" width="600" />

> 当全局和项目文件都定义了同名服务器时，**项目配置优先**。

---

## 验证安装

1. 确保在 MCP 设置面板中已开启**启用 MCP 服务器**。
2. 您现在应该能看到 **Context7** 已列出。如果尚未运行，请点击 <Codicon name="activate" /> 开关来启动它。
3. 当首次调用 Context7 工具时，Zoo Code 会提示您。批准请求以继续。

<img src="/img/recommended-mcp-servers/context7-running-fixed.png" alt="Context7 running in Zoo Code" width="400" />

---

## 后续步骤

* 在服务器面板中浏览 Context7 提供的工具列表。
* 为您最常用的工具配置**始终允许**，以简化工作流程。
* 想要暴露自己的 API？请查看 [MCP 服务器创建指南](/features/mcp/using-mcp-in-roo#enabling-or-disabling-mcp-server-creation)。

正在寻找其他服务器？请继续关注此页面 – 我们很快会添加更多推荐！
