---
name: first-time-setup
description: baoyu-image-gen 首次设置与默认 model 选择流程
---

# 首次设置

## 概述

在以下情况触发：
1. 未找到 EXTEND.md → 完整设置（provider + model + 偏好）
2. 已找到 EXTEND.md 但 `default_model.[provider]` 为 null → 仅选择 model

## 设置流程

```
未找到 EXTEND.md          已找到 EXTEND.md，model 为 null
        │                            │
        ▼                            ▼
┌─────────────────────┐    ┌──────────────────────┐
│ AskUserQuestion     │    │ AskUserQuestion      │
│ (完整设置)          │    │ (仅 model)           │
└─────────────────────┘    └──────────────────────┘
        │                            │
        ▼                            ▼
┌─────────────────────┐    ┌──────────────────────┐
│ 创建 EXTEND.md      │    │ 更新 EXTEND.md       │
└─────────────────────┘    └──────────────────────┘
        │                            │
        ▼                            ▼
    继续                         继续
```

## 流程 1：无 EXTEND.md（完整设置）

**语言**：使用用户输入语言或已保存的语言偏好。

在一次调用中用 AskUserQuestion 提出 **全部** 问题：

### 问题 1：默认 Provider

```yaml
header: "Provider"
question: "Default image generation provider?"
options:
  - label: "Google (Recommended)"
    description: "Gemini multimodal - high quality, reference images, flexible sizes"
  - label: "OpenAI"
    description: "GPT Image - consistent quality, reliable output"
  - label: "OpenRouter"
    description: "Router for Gemini/FLUX/OpenAI-compatible image models"
  - label: "DashScope"
    description: "Alibaba Cloud - Qwen-Image, strong Chinese/English text rendering"
  - label: "Replicate"
    description: "Community models - nano-banana-pro, flexible model selection"
```

### 问题 2：默认 Google Model

仅当用户选择 Google 或未显式指定 provider（自动检测）时展示。

```yaml
header: "Google Model"
question: "Default Google image generation model?"
options:
  - label: "gemini-3-pro-image-preview (Recommended)"
    description: "Highest quality, best for production use"
  - label: "gemini-3.1-flash-image-preview"
    description: "Fast generation, good quality, lower cost"
  - label: "gemini-3-flash-preview"
    description: "Fast generation, balanced quality and speed"
```

### 问题 2b：默认 OpenRouter Model

仅当用户选择 OpenRouter 时展示。

```yaml
header: "OpenRouter Model"
question: "Default OpenRouter image generation model?"
options:
  - label: "google/gemini-3.1-flash-image-preview (Recommended)"
    description: "Best general-purpose OpenRouter image model with reference-image workflows"
  - label: "google/gemini-2.5-flash-image-preview"
    description: "Fast Gemini preview model on OpenRouter"
  - label: "black-forest-labs/flux.2-pro"
    description: "Strong text-to-image quality through OpenRouter"
```

### 问题 3：默认 Quality

```yaml
header: "Quality"
question: "Default image quality?"
options:
  - label: "2k (Recommended)"
    description: "2048px - covers, illustrations, infographics"
  - label: "normal"
    description: "1024px - quick previews, drafts"
```

### 问题 4：保存位置

```yaml
header: "Save"
question: "Where to save preferences?"
options:
  - label: "Project (Recommended)"
    description: ".baoyu-skills/ (this project only)"
  - label: "User"
    description: "~/.baoyu-skills/ (all projects)"
```

### 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| Project | `.baoyu-skills/baoyu-image-gen/EXTEND.md` | 当前项目 |
| User | `$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 所有项目 |

### EXTEND.md 模板

```yaml
---
version: 1
default_provider: [selected provider or null]
default_quality: [selected quality]
default_aspect_ratio: null
default_image_size: null
default_model:
  google: [selected google model or null]
  openai: null
  openrouter: [selected openrouter model or null]
  dashscope: null
  replicate: null
---
```

## 流程 2：已有 EXTEND.md，Model 为 null

当 EXTEND.md 存在但 `default_model.[current_provider]` 为 null 时，**仅** 就当前 provider 询问 model。

### Google Model 选择

```yaml
header: "Google Model"
question: "Choose a default Google image generation model?"
options:
  - label: "gemini-3-pro-image-preview (Recommended)"
    description: "Highest quality, best for production use"
  - label: "gemini-3.1-flash-image-preview"
    description: "Fast generation, good quality, lower cost"
  - label: "gemini-3-flash-preview"
    description: "Fast generation, balanced quality and speed"
```

### OpenAI Model 选择

```yaml
header: "OpenAI Model"
question: "Choose a default OpenAI image generation model?"
options:
  - label: "gpt-image-1.5 (Recommended)"
    description: "Latest GPT Image model, high quality"
  - label: "gpt-image-1"
    description: "Previous generation GPT Image model"
```

### OpenRouter Model 选择

```yaml
header: "OpenRouter Model"
question: "Choose a default OpenRouter image generation model?"
options:
  - label: "google/gemini-3.1-flash-image-preview (Recommended)"
    description: "Recommended for image output and reference-image edits"
  - label: "google/gemini-2.5-flash-image-preview"
    description: "Fast preview-oriented image generation"
  - label: "black-forest-labs/flux.2-pro"
    description: "High-quality text-to-image through OpenRouter"
```

### DashScope Model 选择

```yaml
header: "DashScope Model"
question: "Choose a default DashScope image generation model?"
options:
  - label: "qwen-image-2.0-pro (Recommended)"
    description: "Best DashScope model for text rendering and custom sizes"
  - label: "qwen-image-2.0"
    description: "Faster 2.0 variant with flexible output size"
  - label: "qwen-image-max"
    description: "Legacy Qwen model with five fixed output sizes"
  - label: "qwen-image-plus"
    description: "Legacy Qwen model, same current capability as qwen-image"
  - label: "z-image-turbo"
    description: "Legacy DashScope model for compatibility"
  - label: "z-image-ultra"
    description: "Legacy DashScope model, higher quality but slower"
```

DashScope 设置说明：

- 用户需要自定义 `--size`、非常见比例如 `21:9` 或强中英文字渲染时，优先 `qwen-image-2.0-pro`。
- `qwen-image-max` / `qwen-image-plus` / `qwen-image` 仅支持五种固定尺寸：`1664*928`、`1472*1104`、`1328*1328`、`1104*1472`、`928*1664`。
- 在 `baoyu-image-gen` 中，`quality` 为兼容预设，不是 DashScope 原生参数。

### Replicate Model 选择

```yaml
header: "Replicate Model"
question: "Choose a default Replicate image generation model?"
options:
  - label: "google/nano-banana-pro (Recommended)"
    description: "Google's fast image model on Replicate"
  - label: "google/nano-banana"
    description: "Google's base image model on Replicate"
```

### 更新 EXTEND.md

用户选定 model 后：

1. 读取现有 EXTEND.md
2. 若已有 `default_model:` 段 → 更新对应 provider 的键
3. 若缺少 `default_model:` 段 → 添加完整段：

```yaml
default_model:
  google: [value or null]
  openai: [value or null]
  openrouter: [value or null]
  dashscope: [value or null]
  replicate: [value or null]
```

仅设置所选 provider 的 model；其余保持原值或 null。

## 设置完成后

1. 必要时创建目录
2. 写入/更新带 frontmatter 的 EXTEND.md
3. 确认：「Preferences saved to [path]」
4. 继续图像生成
