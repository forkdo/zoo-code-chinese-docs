---
description: 在 Zoo Code 中使用 generate_image 工具，通过 AI 模型生成或编辑图像。
keywords:
  - generate_image
  - AI 图像
  - 图像生成
  - 图像编辑
  - OpenRouter
  - Zoo Code 工具
  - 实验性
---

# generate_image

`generate_image` 工具根据文本提示创建新图像，或使用 AI 模型修改现有图像。它支持两个提供方：**OpenRouter** 和 **Roo 提供方**。这一实验性特性让你能够在开发工作流中进行视觉内容生成与变换。

---

## 参数

该工具接受以下参数：

- `prompt`（必填）：对要生成的内容或要如何编辑图像的文字描述。
- `path`（必填）：生成/编辑后的图像应保存的文件路径（相对于工作区）。若未提供扩展名，工具会自动添加合适的扩展名。
- `image`（可选）：要编辑或变换的输入图像的文件路径（相对于工作区）。支持的格式：PNG、JPG、JPEG、GIF、WEBP。

---

## 功能说明

该工具使用 AI 模型，根据文字描述生成图像，或对现有图像应用变换。未提供输入图像时，它从零开始创建新图像。提供输入图像时，它将提示作为编辑指令来变换图像。

---

## 使用场景

- 为文档、线框图或原型创建视觉素材
- 生成占位图像或插图
- 变换现有图像（风格迁移、增强、修改）
- 根据描述创建示意图或可视化解释
- 以视觉方式制作 UI 元素原型

---

## 主要特性

- **文生图**：根据描述性提示创建图像
- **图生图变换**：编辑或变换现有图像
- 支持多种输入格式（PNG、JPG、JPEG、GIF、WEBP）
- 自动处理文件扩展名
- 由 **OpenRouter** 或 **Roo 提供方** 驱动，可访问多种 AI 模型
- 实验性特性，持续改进中

---

## 局限性

- 需要配置 OpenRouter 或 Roo 提供方的 API
- 图像质量取决于 AI 模型和提示质量
- 生成时间因复杂度和模型而异
- 实验性特性：未来版本中行为可能变化
- API 使用可能会根据 OpenRouter 定价产生费用
- 某些图像变换可能无法产生预期结果

---

## 工作原理

当调用 `generate_image` 工具时，它会遵循以下流程：

1. **参数校验**：校验必填的 `prompt` 和 `path` 参数。
2. **模式选择**：
   - 如果提供了 `image` 参数：以**编辑模式**运行（变换现有图像）
   - 否则：以**生成模式**运行（根据提示创建新图像）
3. **API 请求**：向已配置的提供方（OpenRouter 或 Roo）发送请求，附带提示和可选的输入图像。
4. **图像处理**：从 API 接收生成/编辑后的图像。
5. **文件保存**：将图像以合适的扩展名保存到指定的 `path`。
6. **反馈**：报告成功信息及生成图像的位置。

---

## 使用示例

生成新图像：
```
<generate_image>
  <prompt>一幅群山之上壮丽的日落，带有鲜艳的橙色和紫色</prompt>
  <path>images/sunset.png</path>
</generate_image>
```

编辑现有图像：
```
<generate_image>
  <prompt>将这张图像转换成水彩画风格</prompt>
  <path>images/watercolor-output.png</path>
  <image>images/original-photo.jpg</image>
</generate_image>
```

放大并增强：
```
<generate_image>
  <prompt>将这张图像放大到更高分辨率，增强细节，在保持原始内容和构图的同时提升清晰度与锐度</prompt>
  <path>images/enhanced-photo.png</path>
  <image>images/low-res-photo.jpg</image>
</generate_image>
```

---

## 与相关特性的关系

`generate_image` 工具是 [图像生成](/features/image-generation) 特性的编程接口。有关配置、模型选择、API 设置以及高级用法的完整文档，请参阅[图像生成特性文档](/features/image-generation)。

---

## 配置

图像生成需要配置 OpenRouter API。有关详细设置说明，请参阅[图像生成](/features/image-generation)特性页面，包括：

- OpenRouter API 密钥配置
- 模型选择和性能
- 提示词最佳实践
- 故障排查与局限性
