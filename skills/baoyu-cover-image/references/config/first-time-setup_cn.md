---
name: first-time-setup
description: baoyu-cover-image 偏好设置的首次设置流程
---

# 首次设置

## 概述

当未找到 EXTEND.md 时，引导用户完成偏好设置。

**⛔ 阻塞操作**：此设置必须在任何其他工作流步骤之前完成。不要：
- 询问参考图
- 询问内容/文章
- 询问维度（类型、色板、渲染）
- 继续内容分析

只询问此设置流程中的问题，保存 EXTEND.md，然后继续。

## 设置流程

```
未找到 EXTEND.md
        │
        ▼
┌─────────────────────┐
│ AskUserQuestion     │
│ （所有问题）         │
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

**语言**：使用用户的输入语言或已保存的语言偏好。

使用 AskUserQuestion 在一次调用中提出所有问题：

### 问题 1：水印

```yaml
header: "水印"
question: "生成封面图时的水印文字？"
options:
  - label: "不使用水印（推荐）"
    description: "干净的封面，可以稍后在 EXTEND.md 中启用"
```

### 问题 2：偏好类型

```yaml
header: "类型"
question: "默认封面类型偏好？"
options:
  - label: "自动选择（推荐）"
    description: "每次根据内容分析选择"
  - label: "hero"
    description: "大面积视觉冲击 - 产品发布、公告"
  - label: "conceptual"
    description: "概念可视化 - 技术、架构"
```

### 问题 3：偏好色板

```yaml
header: "色板"
question: "默认色板偏好？"
options:
  - label: "自动选择（推荐）"
    description: "每次根据内容分析选择"
  - label: "elegant"
    description: "精致优雅 - 柔和珊瑚色、静谧青色、灰玫瑰色"
  - label: "warm"
    description: "友好温暖 - 橙色、金黄色、赤陶色"
  - label: "cool"
    description: "技术专业 - 工程蓝、海军蓝、青色"
```

### 问题 4：偏好渲染

```yaml
header: "渲染"
question: "默认渲染风格偏好？"
options:
  - label: "自动选择（推荐）"
    description: "每次根据内容分析选择"
  - label: "hand-drawn"
    description: "草图风格的有机插画，带有个人特色"
  - label: "flat-vector"
    description: "简洁的现代矢量，几何形状"
  - label: "digital"
    description: "精致的精确数字插画"
```

### 问题 5：默认宽高比

```yaml
header: "比例"
question: "封面图的默认宽高比？"
options:
  - label: "16:9（推荐）"
    description: "标准宽屏 - YouTube、演示文稿、通用"
  - label: "2.35:1"
    description: "电影宽幅 - 文章头图、博客封面"
  - label: "1:1"
    description: "正方形 - Instagram、微信、社交卡片"
  - label: "3:4"
    description: "竖版 - 小红书、Pinterest、移动内容"
```

注意：生成时还有更多比例可用（4:3, 3:2）。此处设置默认推荐值。

### 问题 6：默认输出目录

```yaml
header: "输出"
question: "封面图的默认输出目录？"
options:
  - label: "独立目录（推荐）"
    description: "cover-image/{topic-slug}/ - 与文章分开"
  - label: "同目录"
    description: "{article-dir}/ - 与文章文件在一起"
  - label: "imgs 子目录"
    description: "{article-dir}/imgs/ - 文章旁的图片文件夹"
```

### 问题 7：快速模式

```yaml
header: "快速模式"
question: "默认启用快速模式？"
options:
  - label: "否（推荐）"
    description: "每次确认维度选择"
  - label: "是"
    description: "跳过确认，使用自动选择"
```

### 问题 8：保存位置

```yaml
header: "保存"
question: "偏好设置保存在哪里？"
options:
  - label: "项目（推荐）"
    description: ".baoyu-skills/（仅限此项目）"
  - label: "用户"
    description: "~/.baoyu-skills/（所有项目）"
```

## 保存位置

| 选择 | 路径 | 范围 |
|--------|------|-------|
| 项目 | `.baoyu-skills/baoyu-cover-image/EXTEND.md` | 当前项目 |
| 用户 | `~/.baoyu-skills/baoyu-cover-image/EXTEND.md` | 所有项目 |

## 设置完成后

1. 如需要则创建目录
2. 写入含 frontmatter 的 EXTEND.md
3. 确认："偏好设置已保存到 [path]"
4. 继续步骤 1

## EXTEND.md 模板

```yaml
---
version: 3
watermark:
  enabled: [true/false]
  content: "[user input or empty]"
  position: bottom-right
  opacity: 0.7
preferred_type: [selected type or null]
preferred_palette: [selected palette or null]
preferred_rendering: [selected rendering or null]
preferred_text: title-only
preferred_mood: balanced
default_aspect: [16:9/2.35:1/1:1/3:4]
default_output_dir: [independent/same-dir/imgs-subdir]
quick_mode: [true/false]
language: null
custom_palettes: []
---
```

## 后续修改偏好设置

用户可以直接编辑 EXTEND.md 或重新运行设置：
- 删除 EXTEND.md 以触发设置
- 编辑 YAML frontmatter 进行快速更改
- 完整 schema：`preferences-schema_cn.md`

**EXTEND.md 支持**：水印 | 偏好类型 | 偏好色板 | 偏好渲染 | 偏好文字 | 偏好氛围 | 默认宽高比 | 默认输出目录 | 快速模式 | 自定义色板定义 | 语言偏好
