---
description: 了解模型上下文协议 (MCP) 与 REST API 之间的关键差异。理解为何 MCP 是专为 AI 设计的原生协议，以及它如何实现有状态的上下文交互。
keywords:
  - MCP vs API
  - Model Context Protocol
  - REST API comparison
  - AI protocols
  - stateful connections
  - tool discovery
sidebar_label: MCP vs API
---

# MCP 与 REST API：根本性区别

将 REST API 与模型上下文协议 (MCP) 进行比较是一种范畴错误。它们处于不同的抽象层级，在 AI 系统中承担着根本不同的职责。

---

## 架构差异

| 特性 | MCP | REST API |
|---------|-----|-----------|
| 状态管理 | **有状态** - 在交互过程中保持上下文 | **无状态** - 每个请求相互独立 |
| 连接类型 | 持久化双向连接 | 单向请求/响应 |
| 通信风格 | 基于 JSON-RPC 的持续会话 | 基于 HTTP 的离散请求 |
| 上下文处理 | 上下文是协议的固有特性 | 必须手动管理上下文 |
| 工具发现 | 运行时发现可用工具 | 需要预先了解的设计时集成 |
| 集成方式 | 具备动态能力的运行时集成 | 需要代码更改的设计时集成 |

---

## 不同层级，不同目的

REST API 和 MCP 在技术栈中服务于不同的层级：

1. **REST**：暴露资源操作的底层网络通信模式
2. **MCP**：协调工具使用并维护上下文的高级 AI 协议

MCP 通常在内部使用 REST API，但为 AI 抽象了这些细节。可以将 MCP 视为中间件，它将离散的 Web 服务转化为 AI 可以操作的 cohesive 环境。

---

## 上下文保持：AI 工作流的关键

MCP 的有状态设计解决了 REST 在 AI 应用中的一个关键限制：

- **REST 方式**：每次调用都是孤立的，需要在步骤之间手动传递上下文
- **MCP 方式**：一个对话上下文可以跨多个工具使用而持续存在

例如，AI 在调试代码库时可以打开文件、运行测试并识别错误，而不会在步骤之间丢失上下文。MCP 会话会保持对先前操作和结果的 awareness。

---

## 动态工具发现

MCP 使 AI 能够在运行时发现并使用工具：

```json
// AI 发现可用工具
{
  "tools": [
    {
      "name": "readFile",
      "description": "Reads content from a file",
      "parameters": {
        "path": { "type": "string", "description": "File path" }
      }
    },
    {
      "name": "createTicket",
      "description": "Creates a ticket in issue tracker",
      "parameters": {
        "title": { "type": "string" },
        "description": { "type": "string" }
      }
    }
  ]
}
```

这种"即插即用"能力允许添加新工具而无需重新部署或修改 AI 本身。

---

## 实际示例：多工具工作流

考虑一个需要多个服务的任务："检查最近的提交，为 bug 修复创建一个 JIRA 工单，并发布到 Slack。"

**基于 REST 的方式**：
- 需要为 Git、JIRA 和 Slack API 分别进行集成
- 需要自定义代码来管理调用之间的上下文
- 如果任何服务更改其 API，就会中断

**基于 MCP 的方式**：
- 所有工具使用一个统一的协议
- 在整个工作流中保持上下文
- 新工具可以无需代码更改就能替换进来

---

## 为什么 Zoo Code 使用 MCP

Zoo Code 利用 MCP 提供：

1. **可扩展性**：无需等待官方集成即可添加无限的自定义工具
2. **上下文感知**：工具可以访问对话历史和项目上下文
3. **简化集成**：一个标准协议而非众多 API 模式
4. **运行时灵活性**：动态发现并使用新功能

MCP 在 Zoo Code 和外部服务之间创建了一个通用连接器，而 REST API 通常在幕后为这些服务提供支持。

---

## 结论：互补而非竞争的技术

MCP 并不取代 REST API——它建立在 REST API 之上。REST 擅长提供离散服务，而 MCP 擅长为 AI 代理协调这些服务。

关键区别在于 MCP 是 AI 原生的：它将模型视为一等用户，为 AI 代理在复杂环境中有效运行提供了所需的上下文感知、有状态交互层。
