---
name: first-time-setup
description: baoyu-danger-x-to-markdown 偏好的首次设置流程
---

# 首次设置

## 概述

未找到 EXTEND.md 时，引导用户完成偏好设置。

**阻塞操作**：本设置必须在 **任何** 其他工作流步骤之前完成。**不要**：
- 开始转换推文或文章
- 询问 URL 或输出路径
- 进行任何转换

**仅** 询问本设置流程中的问题，保存 EXTEND.md，然后再继续。

## 设置流程

```
未找到 EXTEND.md
        |
        v
+---------------------+
| AskUserQuestion     |
| (全部问题)          |
+---------------------+
        |
        v
+---------------------+
| 创建 EXTEND.md      |
+---------------------+
        |
        v
    继续转换
```

## 问题

**语言**：使用用户输入语言或已保存的语言偏好。

在一次调用中用 AskUserQuestion 提出 **全部** 问题：

### 问题 1：下载媒体

```yaml
header: "Media"
question: "How to handle images and videos in tweets?"
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
  - label: "x-to-markdown (Recommended)"
    description: "Save to ./x-to-markdown/{username}/{tweet-id}.md"
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

| 选择 | 路径 | 范围 |
|--------|------|-------|
| User | `~/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md` | 所有项目 |
| Project | `.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md` | 当前项目 |

## 设置完成后

1. 必要时创建目录
2. 写入 EXTEND.md
3. 确认：「Preferences saved to [path]」
4. 使用已保存偏好继续转换

## EXTEND.md 模板

```md
download_media: [ask/1/0]
default_output_dir: [path or empty]
```

## 后续修改偏好

用户可直接编辑 EXTEND.md，或删除它以再次触发设置。
