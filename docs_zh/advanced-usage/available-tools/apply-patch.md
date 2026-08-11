---
description: 在 Zoo Code 中使用 apply_patch 工具，通过一次操作将统一差异补丁应用到多个文件。
keywords:
  - apply_patch
  - 补丁
  - 统一差异
  - 多文件编辑
  - 文件操作
  - Zoo Code 工具
  - diff 补丁
---

# apply_patch

`apply_patch` 工具通过一次操作将统一差异（unified diff）补丁应用到多个文件。它支持自定义补丁头，用于新增、删除和更新文件，非常适合复杂的多文件重构操作。

---

## 参数

该工具接受以下参数：

- `patch`（必填）：带有自定义头的统一差异补丁字符串。支持 `*** Add File:`、`*** Delete File:` 和 `*** Update File:` 头。

---

## 功能说明

该工具处理包含多个文件操作的统一差异补丁。它会解析补丁内容，识别文件操作（新增、删除、更新），并以原子方式应用这些更改。[`apply_diff`](/advanced-usage/available-tools/apply-diff) 处理的是单文件搜索替换操作，而 `apply_patch` 使用的是传统的统一差异格式。

---

## 使用场景

- 应用由版本控制系统或 diff 工具生成的补丁
- 执行带精确行级变更的复杂多文件重构
- 将代码变更从一个分支或仓库迁移到另一个分支或仓库
- 在一次操作中批量新增、更新或删除多个文件
- 使用来自外部来源或自动化工具的补丁

---

## 主要特性

- 单次补丁操作支持多个文件
- 支持文件的新增、删除和修改
- 使用统一差异格式实现精确的行级控制
- 自定义头（`*** Add File:`、`*** Delete File:`、`*** Update File:`）使意图清晰
- 以原子方式操作，应用更改前先进行校验
- 兼容标准 diff/patch 工具的输出

---

## 局限性

- 需要正确的统一差异格式语法
- 行号和上下文必须与现有文件内容匹配
- 无法应用存在冲突或上下文不匹配的补丁
- 对于模糊匹配而言，不如搜索替换工具灵活
- 补丁要求精确的行级准确性

---

## 工作原理

当调用 `apply_patch` 工具时，它会遵循以下流程：

1. **补丁解析**：解析补丁字符串，识别自定义头（`*** Add File:`、`*** Delete File:`、`*** Update File:`）和统一差异块。
2. **操作识别**：按文件路径和操作类型（新增、删除、更新）对变更进行分组。
3. **校验**：校验目标文件是否存在（针对更新/删除），或可创建（针对新增）。
4. **RooIgnore 检查**：确保目标文件不受 `.rooignore` 规则限制。
5. **用户审查**：展示补丁操作供用户审查和批准。
6. **应用**：按顺序将已批准的更改应用到每个文件。
7. **反馈**：报告每个文件操作的成功或失败情况。

---

## 补丁格式

补丁格式使用自定义头，后接统一差异块：

```diff
*** Add File: src/utils/newHelper.ts
--- /dev/null
+++ b/src/utils/newHelper.ts
@@ -0,0 +1,5 @@
+export function helperFunction(value: string): string {
+  return value.toUpperCase();
+}
 
*** Update File: src/main.ts
--- a/src/main.ts
+++ b/src/main.ts
@@ -10,7 +10,7 @@
 import { config } from './config';
-const timeout = 5000;
+const timeout = 10000;
 
 function main() {

*** Delete File: src/deprecated/oldUtil.ts
```

---

## 与其他工具的关系

- [`apply_diff`](/advanced-usage/available-tools/apply-diff)：用于带模糊匹配的单文件搜索替换
- `apply_patch`：用于采用统一差异格式的多文件操作
- [`write_to_file`](/advanced-usage/available-tools/write-to-file)：用于创建完整的新文件
