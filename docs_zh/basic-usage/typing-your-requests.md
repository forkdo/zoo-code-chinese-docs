---
description: 了解如何使用自然语言与 Zoo Code 进行有效沟通。输入请求的最佳实践、示例以及需要避免的常见陷阱。
keywords:
  - Zoo Code 请求
  - 自然语言 AI
  - 输入命令
  - AI 通信
  - 请求示例
  - 最佳实践
---

# 输入您的请求

Zoo Code 旨在理解自然语言。您无需使用任何特殊命令或语法即可与它通信。只需用简单的英文输入您的请求，就像在与人类开发者交谈一样。

<img src="/img/typing-your-requests/naturally.gif" alt="在 Zoo Code 中输入请求的示例" width="600" />

---

## 有效的请求策略

明确说明您希望 Zoo Code 做什么。避免使用模糊或含糊不清的语言。

| 策略 | 实现方式 |
|----------|----------------|
| **具体明确** | "修复 `calculateTotal` 中返回错误结果的 bug" 而不是 "修复代码" |
| **提供上下文** | 使用 @ [上下文提及](/basic-usage/context-mentions) 进行文件和代码引用 |
| **分解任务** | 将复杂任务以较小的可管理步骤提交 |
| **包含示例** | 需要特定格式或样式时提供示例代码 |

---

## 请求示例

```
create a new file named `utils.py` and add a function called `add` that takes two numbers as arguments and returns their sum
```
```
in the file @src/components/Button.tsx, change the color of the button to blue
```
```
find all instances of the variable `oldValue` in @/src/App.js and replace them with `newValue`
```
```
run the command `npm install` in the terminal
```
```
explain the function `calculateTotal` in @/src/utils.ts
```
```
@problems address all detected problems
```

---

## 需要避免的常见陷阱

| 不要 | 要 |
|-------|---------|
| 模糊的请求 | 明确说明需要做什么 |
| 假设上下文 | 明确引用文件和函数 |
| 过度的技术术语 | 使用清晰、直接的语言 |
| 多个不相关的任务 | 一次提交一个集中的请求 |
| 未经确认就继续 | 检查代码以确保已完成 |