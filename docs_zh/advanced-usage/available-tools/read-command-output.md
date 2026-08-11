---
description: 在 Zoo Code 中使用 read_command_output 工具，获取 execute_command 中被截断的完整命令输出。
keywords:
  - read_command_output
  - 命令输出
  - 截断输出
  - CLI 输出
  - 终端输出
  - Zoo Code 工具
  - 产物检索
---

# read_command_output

`read_command_output` 工具用于检索通过 [`execute_command`](/advanced-usage/available-tools/execute-command) 执行的命令的完整输出——当输出过大而被截断时。它可访问已存储的命令输出产物，并具备高级过滤和分页能力。

---

## 参数

该工具接受以下参数：

- `artifact_id`（必填）：来自截断输出消息的产物文件名（例如 `cmd-1706119234567.txt`）。
- `search`（可选）：用于过滤行的模式（支持正则或字面字符串）。不区分大小写。类似于 `grep`。**如不需要请完全省略**（不要传入 null 或空字符串）。
- `offset`（可选）：用于分页读取的字节偏移量。默认：0。
- `limit`（可选）：返回的最大字节数。默认：40KB（40960 字节）。

---

## 功能说明

当 [`execute_command`](/advanced-usage/available-tools/execute-command) 产生非常大的输出时，输出会被截断并保存到产物文件中。该工具从这些产物中检索完整输出，支持搜索特定模式（类似 grep）以及对大型结果进行分页。

---

## 使用场景

- 当 [`execute_command`](/advanced-usage/available-tools/execute-command) 的输出中包含消息：`[OUTPUT TRUNCATED - Full output saved to artifact: cmd-XXXX.txt]`
- 需要在大型命令输出中搜索特定错误或模式时
- 分析冗长的构建日志、测试结果或编译输出时
- 对一次性无法查看的大型命令输出进行分页时
- 需要过滤命令输出以查找相关行，而无需通读全部内容时

---

## 主要特性

- **读取模式**：使用 `offset` 和 `limit` 进行分页，访问完整输出
- **搜索模式**：过滤匹配正则或字面模式的行（不区分大小写）
- 高效处理非常大型的命令输出
- 类似 `grep`，用于过滤输出
- 字节级分页，精确控制
- 可访问完整、未截断的命令输出

---

## 局限性

- 仅适用于由 [`execute_command`](/advanced-usage/available-tools/execute-command) 创建的产品
- 产物可能在一段时间后会被清理
- 搜索模式仅不区分大小写
- 以带上限的字节形式返回内容（对于非常大的输出不会一次性返回整个文件）
- 需要截断消息中的精确产物 ID

---

## 工作原理

当调用 `read_command_output` 工具时，它会遵循以下流程：

1. **产物查找**：根据 ID 定位已存储的命令输出产物。
2. **模式选择**：
   - 如果提供了 `search` 参数：以**搜索模式**运行（过滤行）
   - 否则：以**读取模式**运行（返回带 offset/limit 的原始内容）
3. **搜索模式**（若提供了 `search`）：
   - 对每一行应用正则或字面模式匹配
   - 仅返回匹配模式的行
   - 不区分大小写匹配
4. **读取模式**（若未提供 `search`）：
   - 从 `offset` 字节位置开始读取
   - 返回最多 `limit` 字节
   - 支持对大型文件分页
5. **结果返回**：返回过滤后或分页后的内容。

---

## 使用示例

读取被截断的输出：
```
当 execute_command 显示：
"[OUTPUT TRUNCATED - Full output saved to artifact: cmd-1706119234567.txt]"

使用：
<read_command_output>
  <artifact_id>cmd-1706119234567.txt</artifact_id>
</read_command_output>
```

搜索错误：
```
<read_command_output>
  <artifact_id>cmd-1706119234567.txt</artifact_id>
  <search>error|failed|Error</search>
</read_command_output>
```

对输出分页（读取下一块）：
```
<read_command_output>
  <artifact_id>cmd-1706119234567.txt</artifact_id>
  <offset>40960</offset>
  <limit>40960</limit>
</read_command_output>
```

---

## 与其他工具的关系

- [`execute_command`](/advanced-usage/available-tools/execute-command)：创建本工具所读取的产物
- [`search_files`](/advanced-usage/available-tools/search-files)：用于使用正则搜索项目文件
- `read_command_output`：用于搜索命令输出产物
