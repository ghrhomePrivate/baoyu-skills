---
name: first-time-setup
description: baoyu-article-illustrator 偏好设置的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**⛔ 阻塞操作**：此设置必须在任何其他工作流步骤之前完成。不要：
- 询问参考图片
- 询问内容/文章
- 询问类型或风格偏好
- 继续进行内容分析

仅询问本设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ （所有问题）          │
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

**语言**：所有问题使用用户的输入语言或偏好语言。不要总是使用英语。

使用单次 AskUserQuestion 包含多个问题（AskUserQuestion 会自动添加"其他"选项）：

### 问题 1：水印

```
header: "水印"
question: "生成插图的水印文字？请输入您的水印内容（例如，姓名、@账号）"
options:
  - label: "不使用水印（推荐）"
    description: "不添加水印，可稍后在 EXTEND.md 中启用"
```

位置默认为右下角。

### 问题 2：偏好风格

```
header: "风格"
question: "默认插图风格偏好？或输入其他风格名称或您的自定义风格"
options:
  - label: "无（推荐）"
    description: "根据内容分析自动选择"
  - label: "notion"
    description: "极简手绘线条画"
  - label: "warm"
    description: "友好、亲切、个人化"
```

### 问题 3：输出目录

```
header: "输出目录"
question: "为文件配图时将生成的插图保存到哪里？"
options:
  - label: "imgs-subdir（推荐）"
    description: "{article-dir}/imgs/ — 图片保存在文章旁的子目录中"
  - label: "same-dir"
    description: "{article-dir}/ — 图片与文章文件放在一起"
  - label: "illustrations-subdir"
    description: "{article-dir}/illustrations/ — 单独的 illustrations 子目录"
  - label: "independent"
    description: "illustrations/{topic-slug}/ — 在当前工作目录下的独立目录"
```

### 问题 4：保存位置

```
header: "保存"
question: "偏好设置保存到哪里？"
options:
  - label: "项目级别"
    description: ".baoyu-skills/（仅当前项目）"
  - label: "用户级别"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|------|------|------|
| 项目级别 | `.baoyu-skills/baoyu-article-illustrator/EXTEND.md` | 当前项目 |
| 用户级别 | `~/.baoyu-skills/baoyu-article-illustrator/EXTEND.md` | 所有项目 |

## 设置完成后

1. 如需要则创建目录
2. 写入包含 frontmatter 的 EXTEND.md
3. 确认："偏好设置已保存到 [路径]"
4. 继续步骤 1

## EXTEND.md 模板

```yaml
---
version: 1
watermark:
  enabled: [true/false]
  content: "[用户输入或留空]"
  position: bottom-right
  opacity: 0.7
preferred_style:
  name: [选择的风格或 null]
  description: ""
default_output_dir: imgs-subdir  # same-dir | imgs-subdir | illustrations-subdir | independent
language: null
custom_styles: []
---
```

## 后续修改偏好设置

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 以触发重新设置
- 编辑 YAML frontmatter 进行快速修改
- 完整 schema 参见：`config/preferences-schema.md`
