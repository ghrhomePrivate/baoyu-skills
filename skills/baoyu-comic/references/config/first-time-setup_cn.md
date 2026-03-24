---
name: first-time-setup
description: baoyu-comic 偏好设置的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**⛔ 阻塞操作**：此设置必须在任何其他工作流步骤之前完成。不要：
- 询问内容/源材料
- 询问画风或基调
- 询问布局偏好
- 进行内容分析

仅提出此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│（所有问题）          │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ 创建 EXTEND.md      │
└─────────────────────┘
        │
        ▼
    继续步骤 1
```

## 问题

**语言**：使用用户的输入语言或首选语言提出所有问题。不要总是使用英语。

使用单个 AskUserQuestion 提出多个问题（AskUserQuestion 自动添加"其他"选项）：

### 问题 1：水印

```
header: "水印"
question: "生成漫画页面的水印文字？输入你的水印内容（如姓名、@用户名）"
options:
  - label: "无水印（推荐）"
    description: "不添加水印，稍后可在 EXTEND.md 中启用"
```

位置默认为右下角。

### 问题 2：首选画风

```
header: "画风"
question: "默认画风偏好？或输入其他风格名称"
options:
  - label: "自动选择（推荐）"
    description: "根据内容分析自动选择"
  - label: "ligne-claire"
    description: "均匀线条，平涂色彩，欧洲漫画（丁丁风格）"
  - label: "manga"
    description: "日本漫画风格，富有表现力的眼睛和情感"
  - label: "realistic"
    description: "数字绘画，精致且专业"
```

### 问题 3：首选基调

```
header: "基调"
question: "默认基调/氛围偏好？"
options:
  - label: "自动选择（推荐）"
    description: "根据内容信号自动选择"
  - label: "neutral"
    description: "平衡、理性、教育性"
  - label: "warm"
    description: "怀旧、私人、温暖"
  - label: "dramatic"
    description: "高对比、紧张、有力"
```

### 问题 4：语言

```
header: "语言"
question: "漫画文字的输出语言？"
options:
  - label: "自动检测（推荐）"
    description: "匹配源内容语言"
  - label: "zh"
    description: "中文（中文）"
  - label: "en"
    description: "English"
```

### 问题 5：保存位置

```
header: "保存"
question: "偏好设置保存到哪里？"
options:
  - label: "项目"
    description: ".baoyu-skills/（仅此项目）"
  - label: "用户"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-comic/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-comic/EXTEND.md` | 所有项目 |

## 设置完成后

1. 如需要则创建目录
2. 写入包含 frontmatter 的 EXTEND.md
3. 确认："偏好已保存到 [路径]"
4. 继续步骤 1

## EXTEND.md 模板

```yaml
---
version: 2
watermark:
  enabled: [true/false]
  content: "[用户输入或留空]"
  position: bottom-right
  opacity: 0.5
preferred_art: [选定的画风或 null]
preferred_tone: [选定的基调或 null]
preferred_layout: null
preferred_aspect: null
language: [选定语言或 null]
character_presets: []
---
```

## 后续修改偏好

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 以触发设置
- 编辑 YAML frontmatter 进行快速更改
- 完整 schema：`config/preferences-schema.md`
