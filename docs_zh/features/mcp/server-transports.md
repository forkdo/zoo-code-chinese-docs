---
description: Zoo Code 中 MCP 传输机制的全面指南。比较 STDIO、流式 HTTP 和 SSE 传输在本地和远程服务器部署中的差异。
keywords:
  - MCP transports
  - STDIO transport
  - Streamable HTTP
  - SSE transport
  - MCP deployment
  - local vs remote MCP
sidebar_label: STDIO、流式 HTTP 和 SSE 传输
---

# MCP 服务器传输：STDIO、流式 HTTP 和 SSE

模型上下文协议 (MCP) 支持三种主要的传输机制，用于 Zoo Code 与 MCP 服务器之间的通信：标准输入/输出 (STDIO)、流式 HTTP（现代标准）和服务器发送事件 (SSE)（用于传统用途）。每种传输机制都有其独特的特点、优势和使用场景。

---

## STDIO Transport

STDIO 传输在本地机器上运行，并通过标准输入/输出流进行通信。

### STDIO 传输的工作原理

1. 客户端 (Zoo Code) 将 MCP 服务器作为子进程启动
2. 通信通过进程流进行：客户端写入服务器的 STDIN，服务器通过 STDOUT 响应
3. 每条消息由换行符分隔
4. 消息格式为 JSON-RPC 2.0

```
客户端                    服务器
  |                         |
  |---- JSON 消息 ---------->| (通过 STDIN)
  |                         | (处理请求)
  |<---- JSON 消息 ----------| (通过 STDOUT)
  |                         |
```

### STDIO 传输的特点

* **本地性**：与 Zoo Code 运行在同一台机器上
* **性能**：延迟和开销极低（不涉及网络堆栈）
* **简单性**：无需网络配置的直接进程通信
* **关系**：客户端与服务器之间的一对一关系
* **安全性**：由于没有网络暴露，因此本质上更安全

### 何时使用 STDIO

STDIO 传输适用于以下场景：

* 本地集成和运行在同一台机器上的工具
* 安全敏感的操作
* 低延迟要求
* 单客户端场景（每个服务器一个 Zoo Code 实例）
* 命令行工具或 IDE 扩展

### STDIO 实现示例

```typescript
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server({name: 'local-server', version: '1.0.0'});
// 注册工具...

// 使用 STDIO 传输
const transport = new StdioServerTransport(server);
transport.listen();
```
---

## Streamable HTTP Transport

流式 HTTP 传输是远程 MCP 服务器通信的现代标准，取代了较旧的 HTTP+SSE 传输。它通过 HTTP/HTTPS 运行，并允许更灵活的服务器实现。

### 流式 HTTP 传输的工作原理

1. 服务器提供一个支持 POST 和 GET 方法的单一 HTTP 端点（MCP 端点）。
2. 客户端 (Zoo Code) 使用 HTTP POST 向此 MCP 端点发送请求。
3. 服务器处理请求并发送回响应。
4. 可选地，服务器可以通过同一连接使用服务器发送事件 (SSE) 向客户端流式传输多条消息或通知。这既支持基本的请求-响应交互，也支持更高级的流式传输和服务器发起的通信。

```
客户端                             服务器
  |                                  |
  |---- HTTP POST /mcp_endpoint ---->| (客户端请求)
  |                                  | (处理请求)
  |<--- HTTP 响应 / SSE 流 ----------| (服务器响应 / 流)
  |                                  |
```

### 流式 HTTP 传输的特点

* **现代标准**：新远程 MCP 服务器实现的首选方法。
* **远程访问**：可以托管在与 Zoo Code 不同的机器上。
* **可扩展性**：可以并发处理多个客户端连接。
* **协议**：通过标准 HTTP/HTTPS 运行。
* **灵活性**：支持简单的请求-响应和高级流式传输。
* **单一端点**：所有 MCP 通信使用单一 URL 路径。
* **身份验证**：可以使用标准 HTTP 身份验证机制。
* **向后兼容性**：服务器可以保持与较旧的 HTTP+SSE 客户端的兼容性。

### 何时使用流式 HTTP

流式 HTTP 传输适用于以下场景：

* 所有新的远程 MCP 服务器开发。
* 需要强大、可扩展和灵活通信的服务器。
* 可能涉及流式数据或服务器发送通知的集成。
* 公共服务或集中式工具。
* 替换传统的 SSE 传输实现。

### 流式 HTTP 实现示例

`settings.json` 中的配置：
```json
"mcp.servers": {
  "StreamableHTTPMCPName": {
    "type": "streamable-http",
    "url": "http://localhost:8080/mcp"
  }
}
```

有关服务器端实现，请参阅 MCP SDK 文档中的 `StreamableHTTPClientTransport`。

### 与 HTTP+SSE 的向后兼容性

客户端和服务器可以保持与已弃用的 HTTP+SSE 传输（来自协议版本 2024-11-05）的向后兼容性。

希望支持较旧客户端的服务器应：
* 继续托管旧传输的 SSE (`/events`) 和 POST (`/message`) 端点，以及为流式 HTTP 传输定义的新“MCP 端点”。

---

## SSE Transport (Legacy)

服务器发送事件 (SSE) 传输是一种通过 HTTP/HTTPS 进行远程服务器通信的传统方法。对于新实现，**推荐使用流式 HTTP 传输。** SSE 仍可用于与较旧的 MCP 服务器保持兼容性。

### SSE 传输的工作原理

1. 客户端 (Zoo Code) 通过 HTTP GET 请求连接到服务器的 SSE 端点
2. 这建立了一个持久连接，服务器可以通过该连接向客户端推送事件
3. 对于客户端到服务器的通信，客户端向单独的端点发出 HTTP POST 请求
4. 通信通过两个通道进行：
   * 事件流 (GET)：服务器到客户端的更新
   * 消息端点 (POST)：客户端到服务器的请求

```
客户端                             服务器
  |                                  |
  |---- HTTP GET /events ----------->| (建立 SSE 连接)
  |<---- SSE 事件流 -----------------| (持久连接)
  |                                  |
  |---- HTTP POST /message --------->| (客户端请求)
  |<---- 包含响应的 SSE 事件 -------| (服务器响应)
  |                                  |
```

### SSE 传输的特点

* **远程访问**：可以托管在与 Zoo Code 不同的机器上
* **可扩展性**：可以并发处理多个客户端连接
* **协议**：通过标准 HTTP 运行（无需特殊协议）
* **持久性**：为服务器到客户端的消息维护持久连接
* **身份验证**：可以使用标准 HTTP 身份验证机制

### 何时使用 SSE

SSE 传输更适用于以下场景：

* 跨网络的远程访问
* 多客户端场景
* 公共服务
* 许多用户需要访问的集中式工具
* 与 Web 服务的集成

### SSE 实现示例

```typescript
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { SSEServerTransport } from '@modelcontextprotocol/sdk/server/sse.js';
import express from 'express';

const app = express();
const server = new Server({name: 'remote-server', version: '1.0.0'});
// 注册工具...

// 使用 SSE 传输
const transport = new SSEServerTransport(server);
app.use('/mcp', transport.requestHandler());
app.listen(3000, () => {
  console.log('MCP 服务器监听端口 3000');
});
```
---

## 本地 vs. 托管：部署方面

STDIO 和 SSE 传输之间的选择直接影响您部署和管理 MCP 服务器的方式。

### STDIO：本地部署模型

STDIO 服务器在本地与 Zoo Code 运行在同一台机器上，这有几个重要的含义：

* **安装**：服务器可执行文件必须安装在每个用户的计算机上
* **分发**：您需要为不同操作系统提供安装包
* **更新**：每个实例必须单独更新
* **资源**：使用本地计算机的 CPU、内存和磁盘
* **访问控制**：依赖本地计算机的文件系统权限
* **集成**：易于与本地系统资源（文件、进程）集成
* **执行**：随 Zoo Code 启动和停止（子进程生命周期）
* **依赖项**：任何依赖项都必须安装在用户的计算机上

#### 实际示例

使用 STDIO 的本地文件搜索工具会：
* 在用户的计算机上运行
* 直接访问本地文件系统
* 在需要时由 Zoo Code 启动
* 不需要网络配置
* 需要与 Zoo Code 一起安装或通过包管理器安装

### 可流式 HTTP / SSE（旧版）：托管部署模型

可流式 HTTP（推荐）和旧版 SSE 服务器可以部署到远程服务器并通过网络访问：

* **安装**：在服务器上安装一次，供多个用户访问
* **分发**：单个部署可服务多个客户端
* **更新**：集中更新会立即影响所有用户
* **资源**：使用服务器资源，而非本地计算机资源
* **访问控制**：通过身份验证和授权系统管理
* **集成**：与用户特定资源的集成更复杂
* **执行**：作为独立服务运行（通常持续运行）
* **依赖项**：在服务器上管理，而非用户计算机上

#### 实际示例

使用 SSE 的数据库查询工具会：
* 在中央服务器上运行
* 使用服务器端凭据连接数据库
* 持续为多个用户提供服务
* 需要适当的网络安全配置
* 使用容器或云技术部署

### 混合方法

某些场景适合采用混合方法：

1. **带网络访问的 STDIO**：作为远程服务代理的本地 STDIO 服务器
2. **带本地命令的 SSE**：可通过回调触发客户端操作的远程 SSE 服务器
3. **网关模式**：用于本地操作的 STDIO 服务器，连接到 SSE 服务器以执行专用功能

---

## 在传输方式之间选择

| 考虑因素 | STDIO | 可流式 HTTP | SSE（旧版） |
|---------|-------|-------------|-------------|
| **位置** | 仅限本地计算机 | 本地或远程 | 本地或远程 |
| **客户端** | 单个客户端 | 多个客户端 | 多个客户端 |
| **性能** | 延迟更低 | 延迟更高（网络开销） | 延迟更高（网络开销） |
| **设置复杂度** | 更简单 | 更复杂（需要 HTTP 服务器） | 更复杂（需要 HTTP 服务器，可能需要两个端点） |
| **安全性** | 固有安全性 | 需要明确的安全措施 | 需要明确的安全措施 |
| **网络访问** | 不需要 | 必需 | 必需 |
| **可扩展性** | 仅限于本地计算机 | 可在网络中分发 | 可在网络中分发 |
| **部署** | 按用户安装 | 集中安装 | 集中安装 |
| **更新** | 分布式更新 | 集中更新 | 集中更新 |
| **资源使用** | 使用客户端资源 | 使用服务器资源 | 使用服务器资源 |
| **依赖项** | 客户端依赖项 | 服务器端依赖项 | 服务器端依赖项 |
| **推荐** | 适用于本地、安全、单客户端工具的理想选择 | **所有新远程服务器的现代标准** | 旧版，适用于现有旧服务器 |

---

## 在 Zoo Code 中配置传输方式

有关在 Zoo Code 中配置 STDIO、可流式 HTTP 和 SSE（旧版）传输方式的详细信息，包括配置示例，请参阅《在 Zoo Code 中使用 MCP》指南中的[理解传输类型](/features/mcp/using-mcp-in-roo#understanding-transport-types)部分。
