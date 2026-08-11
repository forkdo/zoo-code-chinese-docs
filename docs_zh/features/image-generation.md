---
description: 使用 OpenRouter API 在 Zoo Code 中根据文本提示生成新图像或编辑现有图像。转换、增强 AI 处理的图像并将其保存到工作区，支持预览功能。
keywords:
  - image generation
  - image editing
  - text to image
  - image transformation
  - OpenRouter
  - AI images
  - experimental feature
  - image creation
  - prompt to image
  - watercolor
  - upscaling
  - style transfer
---

# 图像生成

根据文本提示生成新图像或编辑工作区中的现有图像。将结果保存到项目中，并在聊天中预览。此实验性功能需要 OpenRouter API 密钥。

:::warning 实验性功能
图像生成是一项实验性功能，需要在设置中启用并配置 OpenRouter API 密钥。
:::

---

## 主要功能

- 使用自然语言根据文本提示创建新图像
- 编辑和转换工作区中的现有图像
- 保存到您选择的工作区路径；如果缺少扩展名，会自动添加适当的扩展名（.png 或 .jpg）
- 在对话中显示生成/编辑图像的预览
- 目前通过 OpenRouter 使用 Gemini 2.5 Flash Image Preview 模型
- 在实验性设置下提供简单的开关切换

---

## 使用场景

### 图像生成
**之前：** 您需要将提示复制到外部网站，下载结果，然后将其移动到工作区。

**使用此功能：** 要求 Zoo 生成图像，确认后选择保存位置，图像已存在于项目中，即可继续编辑。

### 图像编辑
**之前：** 导出图像，上传到外部编辑器，进行更改，下载，再导入回项目。

**使用此功能：** 直接要求 Zoo 转换现有图像——它会读取文件，应用您的编辑，并将结果保存在项目中。

---

## 工作原理

启用后，Zoo 会将您的提示（以及可选的现有图像）通过 OpenRouter 发送到支持图像生成的模型。OpenRouter 返回的生成或编辑图像将保存到您当前工作区中指定的路径。Zoo 会在聊天中显示预览，并且文件会出现在文件资源管理器中。

---

## 要求

- OpenRouter 账户和 API 密钥
- 互联网访问权限
- 一个打开的、可写的工作区文件夹

---

## 配置

### 1. 启用图像生成（实验性）
- **用途：** 开启此功能，使 Zoo 能够根据请求创建图像
- **默认值：** 关闭
- **位置：** 设置 > 实验性

### 2. OpenRouter API 密钥
- **用途：** 授权图像生成请求
- **默认值：** 空（必需）
- **获取密钥：** [https://openrouter.ai/keys](https://openrouter.ai/keys)

### 3. 图像生成模型
- **用途：** 选择用于生成的模型
- **默认值：** Gemini 2.5 Flash Image Preview
- **可用模型：** 目前仅限于 Gemini 2.5 Flash Image Preview 及其免费版本

---

## 使用图像生成

1. 在聊天中，要求 Zoo 生成图像并描述您想要的内容（主题、风格、光线、构图）。
2. 在提示时确认操作。Zoo 可能会要求您选择保存路径（例如：`images/sunset.png`）。
3. Zoo 生成图像并保存。如果您不包含扩展名，会根据输出格式自动添加适当的扩展名（.png 或 .jpg）。
4. 在聊天中查看图像预览，并在工作区中找到该文件。

---

## Editing Existing Images

Zoo 还可以转换和编辑工作区中的现有图像：

1. 要求 Zoo 编辑图像，描述您想要的转换
2. 指定输入图像路径和保存结果的位置
3. Zoo 将对现有图像应用您请求的编辑

**支持的输入格式**：PNG、JPG、JPEG、GIF、WEBP

**示例请求**：
- “将 `photos/portrait.jpg` 转换为水彩画并保存为 `art/watercolor-portrait.png`”
- “将 `images/logo.png` 放大并增强到更高分辨率”
- “对 `screenshots/app.png` 应用复古滤镜”

**注意**：输入图像路径和输出路径都必须可访问（不被 `.rooignore` 阻止）

---

## 获得更好结果的技巧

### 具体明确
在提示中包含以下元素：
- **风格：** 艺术媒介、艺术流派或特定艺术家风格
- **情绪：** 情感基调、氛围
- **调色板：** 特定颜色或配色方案
- **相机/光线：** 角度、透视、光照条件
- **宽高比：** 尺寸或方向

---

## 限制

- 实验性功能；可用性和模型列表有限
- 目前仅限于 Gemini 2.5 Flash Image Preview 模型
- 每次请求生成一张图像
- 支持的输出格式：PNG 或 JPG
- 编辑支持的输入格式：仅 PNG、JPG、JPEG、GIF、WEBP
- 图像路径必须可访问（不被 `.rooignore` 限制阻止）
- 使用可能受您的 OpenRouter 计划限制和费用约束

---

## 状态

此功能为实验性功能，在未来的版本中可能会更改或移除。请通过 [GitHub Issues](https://github.com/RooCodeInc/Zoo-Code/issues) 提供反馈。
