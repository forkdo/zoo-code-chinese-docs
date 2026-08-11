---
description: 了解模型上下文协议 (MCP) 以及它如何让 AI 系统与外部工具进行交互。学习 MCP 架构及其优势。
keywords:
  - what is MCP
  - Model Context Protocol
  - MCP architecture
  - AI tools integration
  - MCP client-server
  - JSON-RPC protocol
sidebar_label: 什么是 MCP？
---

# 什么是 MCP？

MCP（模型上下文协议）是 LLM 系统与外部工具和服务进行交互的标准化通信协议。它充当 AI 助手与各种数据源或应用程序之间的通用适配器。

---

## 工作原理

MCP 采用客户端-服务器架构：

1. AI 助手（客户端）连接到 MCP 服务器
2. 每个服务器提供特定功能（文件访问、数据库查询、API 集成）
3. AI 通过标准化接口使用这些功能
4. 通信通过 JSON-RPC 2.0 消息进行

可以将 MCP 理解为类似 USB-C 接口，任何兼容的 LLM 都可以连接到任何 MCP 服务器以访问其功能。这种标准化消除了为每个工具和服务构建自定义集成的需求。

例如，使用 MCP 的 AI 可以执行“搜索我们的公司数据库并生成报告”等任务，而无需为每个数据库系统开发专门的代码。

---

## 常见问题

- **MCP 是云服务吗？** MCP 服务器可以根据使用场景和安全要求，在本地计算机上运行或作为云服务远程运行。

- **MCP 是否取代了其他集成方法？** 不是。MCP 是对现有工具（如 API 插件和检索增强生成）的补充。它提供了工具交互的标准化协议，但不会取代专门的集成方法。

- **如何处理安全性？** 用户可以控制连接到哪些 MCP 服务器以及这些服务器拥有的权限。与任何访问数据或服务的工具一样，请使用可信来源并配置适当的访问控制。

---

## Zoo Code 中的 MCP

Zoo Code 实现了模型上下文协议，以：

- 连接到本地和远程 MCP 服务器
- 为访问工具提供一致的接口
- 无需修改核心即可扩展功能
- 按需启用专门的功能

MCP 为 AI 系统与外部工具和服务的交互提供了一种标准化方式，使复杂的集成更加易于访问和一致。

---

## 进一步了解 MCP

准备好深入了解了吗？请查看以下指南：

- [MCP 概览](/features/mcp/overview) - 快速了解 MCP 文档结构
- [在 Zoo Code 中使用 MCP](/features/mcp/using-mcp-in-roo) - 开始在 Zoo 中使用 MCP，包括创建简单的服务器
- [MCP 与 API](/features/mcp/mcp-vs-api) - 与传统 API 相比的技术优势
- [STDIO & Streamable HTTP & SSE 传输](/features/mcp/server-transports) - 本地与托管部署模型
