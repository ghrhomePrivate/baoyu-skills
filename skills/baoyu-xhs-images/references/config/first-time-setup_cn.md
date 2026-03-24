---
name: first-time-setup
description: baoyu-xhs-images 偏好的首次设置流程
---

# 首次设置

## 概述

未找到 EXTEND.md 时，引导用户完成偏好设置。

**⛔ 阻塞操作**：必须先完成本设置，才能执行其他任何工作流步骤。**不要**：
- 询问正文/文章
- 询问风格或版式
- 询问目标受众
- 进入内容分析

**仅**询问本设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
No EXTEND.md found
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ (all questions)     │
└─────────────────────┘
        │
        ▼
┌─────────────────────┐
│ Create EXTEND.md    │
└─────────────────────┘
        │
        ▼
    Continue to Step 1
```

## 问题

**语言**：使用用户输入语言或已保存的语言偏好。

使用单次 `AskUserQuestion`，包含多个问题（`AskUserQuestion` 会自动附加 "Other" 选项）：

### 问题 1：水印

```
header: "Watermark"
question: "Watermark text for generated images? Type your watermark content (e.g., name, @handle)"
options:
  - label: "No watermark (Recommended)"
    description: "No watermark, can enable later in EXTEND.md"
```

位置默认为右下角。

### 问题 2：首选风格

```
header: "Style"
question: "Default visual style preference? Or type another style name or your custom style"
options:
  - label: "None (Recommended)"
    description: "Auto-select based on content analysis"
  - label: "cute"
    description: "Sweet, adorable - classic XHS aesthetic"
  - label: "notion"
    description: "Minimalist hand-drawn, intellectual"
```

### 问题 3：保存位置

```
header: "Save"
question: "Where to save preferences?"
options:
  - label: "Project"
    description: ".baoyu-skills/ (this project only)"
  - label: "User"
    description: "~/.baoyu-skills/ (all projects)"
```

## 保存位置

| 选项 | 路径 | 范围 |
|------|------|------|
| Project | `.baoyu-skills/baoyu-xhs-images/EXTEND.md` | 当前项目 |
| User | `~/.baoyu-skills/baoyu-xhs-images/EXTEND.md` | 所有项目 |

## 设置完成后

1. 必要时创建目录
2. 写入带 front matter 的 EXTEND.md
3. 确认：「Preferences saved to [path]」
4. 进入步骤 1

## EXTEND.md 模板

```yaml
---
version: 1
watermark:
  enabled: [true/false]
  content: "[user input or empty]"
  position: bottom-right
  opacity: 0.7
preferred_style:
  name: [selected style or null]
  description: ""
preferred_layout: null
language: null
custom_styles: []
---
```

## 稍后修改偏好

用户可直接编辑 EXTEND.md，或再次运行设置：
- 删除 EXTEND.md 以触发设置
- 编辑 YAML front matter 做快速修改
- 完整 schema：`config/preferences-schema.md`
