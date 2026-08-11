---
description: 在 Zoo Code 中使用 edit_file 搜索替换工具，替换文件中唯一标识的一处文本。
keywords:
  - edit_file
  - 搜索替换
  - 文件编辑
  - 文本替换
  - Zoo Code 工具
  - 代码修改
---

# edit_file

`edit_file` 工具对文件执行定向的搜索替换操作。默认情况下，它替换**恰好一个**唯一标识的匹配项，如果找到多处匹配则会报错。当 `old_string` 为空时，它还支持一种特殊的文件创建模式。

---

## 参数

该工具接受以下参数：

- `file_path`（必填）：待修改文件相对于当前工作目录的路径。
- `old_string`（必填）：要搜索并替换的确切文本。传入空字符串（`""`）可创建新文件或向现有文件追加内容。
- `new_string`（必填）：替换后的文本。
- `expected_replacements`（可选）：期望的替换次数（默认为 1）。如果实际次数与之不符，操作将失败。仅在确实需要替换多处匹配时再使用此参数。

---

## 功能说明

该工具在文件中搜索精确字符串，并将**恰好一处**匹配替换为新文本。搜索字符串必须能唯一标识目标位置。如果找到多处匹配，除非显式设置了 `expected_replacements` 与之匹配，否则工具会返回错误。当 `old_string` 为空时，工具会创建新文件，或将 `new_string` 追加到现有文件中。

---

## 使用场景

- 对文件中某个特定的、可唯一标识的位置做定向修改
- 在已知位置更新某个特定的字符串字面量或配置值
- 修正某处特定的拼写错误或过时术语
- 替换某处唯一标识的已弃用 API 或导入路径
- 创建新文件或向现有文件追加内容（`old_string=""`）
- 需要无需模糊逻辑的精确匹配替换

---

## 主要特性

- 默认替换**恰好一处**唯一标识的匹配
- 如果找到多处匹配则报错（除非显式设置了 `expected_replacements`）
- `old_string=""` 模式：创建新文件或向现有文件追加内容
- 精确字符串匹配（不支持正则或模糊匹配）
- 可选的 `expected_replacements` 用于有意的多处替换
- 应用前展示变更预览
- 如果实际替换次数与 `expected_replacements` 不符，则安全失败
- 保留文件格式和结构

---

## 局限性

- 需要精确字符串匹配（区分大小写、区分空白字符）
- 如果搜索字符串匹配到多处位置则报错（除非设置了 `expected_replacements`）
- 不能使用正则表达式或模式
- 不适合上下文相关的替换
- 对于复杂编辑，不如 [`apply_diff`](/advanced-usage/available-tools/apply-diff) 精确

---

## 工作原理

当调用 `edit_file` 工具时，它会遵循以下流程：

1. **参数校验**：校验必填的 `file_path`、`old_string` 和 `new_string` 参数。
2. **文件创建模式**：如果 `old_string` 为空（`""`），则以 `new_string` 作为内容创建文件（若文件已存在则追加），然后停止。
3. **文件加载**：读取目标文件内容。
4. **唯一性检查**：统计 `old_string` 的出现次数。如果次数与 `expected_replacements`（默认：1）不符，则返回错误。
5. **替换**：将匹配的一处（或多处）替换为 `new_string`。
6. **用户审查**：展示变更预览供用户批准。
7. **应用**：若获批准，将更改应用到文件。
8. **反馈**：报告已完成的替换次数。

---

## 与其他工具的关系

- `edit_file`：默认替换**恰好一处**唯一标识的匹配；支持 `old_string=""` 的文件创建（本工具）
- [`edit`](/advanced-usage/available-tools/edit)：仅替换**第一处**匹配（除非 `replace_all: true`）
- [`search_replace`](/advanced-usage/available-tools/search-replace)：同样替换**恰好一处**唯一标识的匹配
- [`apply_diff`](/advanced-usage/available-tools/apply-diff)：用于带模糊匹配的精确、上下文感知编辑

这些是不同的搜索替换实现，能力各有差异。
