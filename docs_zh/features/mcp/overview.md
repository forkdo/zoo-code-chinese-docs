---
sidebar_label: MCP 概述
description: 了解 Zoo Code 中的模型上下文协议（MCP）。发现如何通过自定义工具、资源和服务器集成来扩展 AI 能力。
keywords:
  - MCP
  - Model Context Protocol
  - Zoo Code 扩展
  - 自定义工具
  - MCP 服务器
  - AI 集成
---

# Model Context Protocol (MCP)

模型上下文协议（MCP）是一个标准，通过连接到外部工具和服务来扩展 Zoo Code 的能力。MCP 服务器提供额外的工具和资源，帮助 Zoo 完成超出其内置功能的任务，例如访问数据库、自定义 API 和专业功能。

---

## MCP 文档

本文档分为以下几个部分：

* [**在 Zoo Code 中使用 MCP**](/features/mcp/using-mcp-in-roo) - 配置、启用和管理 Zoo Code 中 MCP 服务器的综合指南。包括服务器设置、工具审批和故障排除。

* [**什么是 MCP？**](/features/mcp/what-is-mcp) - 对模型上下文协议的清晰解释，包括其客户端-服务器架构，以及它如何使 AI 系统与外部工具交互。

* [**STDIO、可流式 HTTP 和 SSE 传输**](/features/mcp/server-transports) - 本地（STDIO）和远程（可流式 HTTP 和传统 SSE）传输机制的详细比较，以及每种方法的部署考虑。

* [**MCP 与 API**](/features/mcp/mcp-vs-api) - MCP 与 REST API 之间基本区别的分析，解释它们如何在 AI 系统的不同抽象层上运行。

* [**推荐的 MCP 服务器**](/features/mcp/recommended-mcp-servers) - 为 Zoo Code 精选的经过测试和推荐的 MCP 服务器列表，包括 Context7 的设置指南。
