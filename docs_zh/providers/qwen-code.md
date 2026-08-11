---
sidebar_label: Qwen Code CLI
description: 通过 OAuth 认证访问 Qwen3 Coder 模型。支持 1M 上下文窗口和自动令牌刷新。
keywords:
  - qwen code
  - qwen cli
  - qwen3 coder
  - zoo code
  - api provider
  - oauth
  - alibaba
  - dashscope
---

# Qwen Code CLI Provider

通过 OAuth 认证访问阿里巴巴的 Qwen3 Coder 模型，支持自动令牌刷新。具备高达 1M 令牌的上下文窗口，专为大型代码库优化。

:::info Setup Required
1. **安装 Qwen Client**：从官方网站下载
2. **认证**：运行客户端并登录以创建 OAuth 凭据
3. **在 Zoo Code 中配置**：选择 "Qwen Code CLI API" 作为您的 Agent
   - 默认路径 `~/.qwen/oauth_creds.json` 将自动生效
   - 如需，也可指定自定义凭据路径
:::

**网站：** [https://chat.qwen.ai](https://chat.qwen.ai)

---

## 可用模型

Qwen3 Coder 模型支持高达 1M 的上下文窗口和 65K 最大输出令牌。

有关完整且最新的模型列表，请在 Zoo Code 中配置该 Agent 时查看 Qwen Code Agent 的模型目录。

---

## 配置

### OAuth 凭据路径
- **默认路径**：`~/.qwen/oauth_creds.json`
- **支持自定义路径**：包括绝对路径和以 `~/` 开头的路径
- 您通过 Qwen 客户端认证后将自动创建

---

## 主要特性

- **OAuth 2.0**：通过自动令牌刷新实现安全认证
- **1M 上下文**：可在单次对话中处理整个代码库
- **自动刷新**：令牌在后台透明刷新，预留 30 秒缓冲时间
- **免费层**：促销期间提供每日 2,000 次请求、每分钟 60 次请求，无令牌限制
- **推理支持**：完全支持思维块（thinking blocks）

---

## 常见问题

**“无法找到凭据文件”**
- 请确保您已通过 Qwen 客户端完成认证
- 检查文件是否存在于 `~/.qwen/oauth_creds.json`

**“令牌刷新失败”**
- 检查网络连接
- 重新通过 Qwen 客户端进行认证

**“401 Unauthorized”**
- Agent 应自动刷新令牌（请检查日志）
- 若问题持续，请删除凭据并重新认证
---