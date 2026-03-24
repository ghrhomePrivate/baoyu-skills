---
name: first-time-setup
description: baoyu-url-to-markdown 偏好的首次设置流程
---

# 首次设置

## 概述

未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：必须先完成本设置，才能执行其他任何工作流步骤。**不要**：
- 开始转换 URL
- 询问 URL 或输出路径
- 进入任何转换流程

**仅**询问本设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
No EXTEND.md found
        |
        v
+---------------------+
| AskUserQuestion     |
| (all questions)     |
+---------------------+
        |
        v
+---------------------+
| Create EXTEND.md    |
+---------------------+
        |
        v
    Continue conversion
```

## 问题

**语言**：使用用户输入语言或已保存的语言偏好。

在**一次** `AskUserQuestion` 调用中包含**全部**问题：

### 问题 1：下载媒体

```yaml
header: "Media"
question: "How to handle images and videos in pages?"
options:
  - label: "Ask each time (Recommended)"
    description: "After saving markdown, ask whether to download media"
  - label: "Always download"
    description: "Always download media to local imgs/ and videos/ directories"
  - label: "Never download"
    description: "Keep original remote URLs in markdown"
```

### 问题 2：默认输出目录

```yaml
header: "Output"
question: "Default output directory?"
options:
  - label: "url-to-markdown (Recommended)"
    description: "Save to ./url-to-markdown/{domain}/{slug}.md"
```

说明：用户通常会选 "Other" 以输入自定义路径。

### 问题 3：保存位置

```yaml
header: "Save"
question: "Where to save preferences?"
options:
  - label: "User (Recommended)"
    description: "~/.baoyu-skills/ (all projects)"
  - label: "Project"
    description: ".baoyu-skills/ (this project only)"
```

## 保存位置

| 选项 | 路径 | 范围 |
|------|------|------|
| User | `~/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md` | 所有项目 |
| Project | `.baoyu-skills/baoyu-url-to-markdown/EXTEND.md` | 当前项目 |

## 设置完成后

1. 必要时创建目录
2. 写入 EXTEND.md
3. 确认：「Preferences saved to [path]」
4. 按已保存偏好继续转换

## EXTEND.md 模板

```md
download_media: [ask/1/0]
default_output_dir: [path or empty]
```

## 稍后修改偏好

用户可直接编辑 EXTEND.md，或删除它以再次触发设置。
