---
name: preferences-schema
description: baoyu-cover-image 用户偏好的 EXTEND.md YAML schema
---

# 偏好设置 Schema

## 完整 Schema

```yaml
---
version: 3

watermark:
  enabled: false
  content: ""
  position: bottom-right  # bottom-right|bottom-left|bottom-center|top-right

preferred_type: null      # hero|conceptual|typography|metaphor|scene|minimal 或 null 为自动选择

preferred_palette: null   # warm|elegant|cool|dark|earth|vivid|pastel|mono|retro 或 null 为自动选择

preferred_rendering: null # flat-vector|hand-drawn|painterly|digital|pixel|chalk 或 null 为自动选择

preferred_text: title-only  # none|title-only|title-subtitle|text-rich

preferred_mood: balanced    # subtle|balanced|bold

default_aspect: "2.35:1"  # 2.35:1|16:9|1:1

quick_mode: false         # 为 true 时跳过确认

language: null            # zh|en|ja|ko|auto（null = 自动检测）

custom_palettes:
  - name: my-palette
    description: "色板描述"
    colors:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8"]
    decorative_hints: "简洁线条、几何形状"
    best_for: "商业、技术内容"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 描述 |
|-------|------|---------|-------------|
| `version` | int | 3 | Schema 版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文字（@用户名或自定义） |
| `watermark.position` | enum | bottom-right | 图片上的位置 |
| `preferred_type` | string | null | 类型名称或 null 为自动 |
| `preferred_palette` | string | null | 色板名称或 null 为自动 |
| `preferred_rendering` | string | null | 渲染名称或 null 为自动 |
| `preferred_text` | string | title-only | 文字密度级别 |
| `preferred_mood` | string | balanced | 氛围强度级别 |
| `default_aspect` | string | "2.35:1" | 默认宽高比 |
| `quick_mode` | bool | false | 跳过确认步骤 |
| `language` | string | null | 输出语言（null = 自动检测） |
| `custom_palettes` | array | [] | 用户自定义色板 |

## 类型选项

| 值 | 描述 |
|-------|-------------|
| `hero` | 大面积视觉冲击，标题覆盖 |
| `conceptual` | 概念可视化，抽象核心理念 |
| `typography` | 文字为主的布局，突出标题 |
| `metaphor` | 视觉隐喻，用具体表达抽象 |
| `scene` | 氛围场景，叙事感 |
| `minimal` | 极简构图，大面积留白 |

## 色板选项

| 值 | 描述 |
|-------|-------------|
| `warm` | 友好、亲切——橙色、金黄色、赤陶色 |
| `elegant` | 精致、优雅——柔和珊瑚色、静谧青色、灰玫瑰色 |
| `cool` | 技术、专业——工程蓝、海军蓝、青色 |
| `dark` | 电影感、高端——电光紫、青色、品红 |
| `earth` | 自然、有机——森林绿、鼠尾草绿、大地棕 |
| `vivid` | 活力、醒目——亮红色、霓虹绿、电光蓝 |
| `pastel` | 温柔、梦幻——柔粉色、薄荷绿、薰衣草紫 |
| `mono` | 简洁、专注——黑色、近黑色、白色 |
| `retro` | 怀旧、复古——柔和橙色、灰粉色、栗色 |

## 渲染选项

| 值 | 描述 |
|-------|-------------|
| `flat-vector` | 简洁轮廓、均匀填充、几何图标 |
| `hand-drawn` | 草图风格、有机、不完美笔触、纸张纹理 |
| `painterly` | 柔和笔触、颜色晕染、水彩感 |
| `digital` | 精致、精确边缘、微妙渐变、UI 组件 |
| `pixel` | 像素网格、抖动、粗块 8-bit 形状 |
| `chalk` | 粉笔笔触、粉尘效果、黑板纹理 |

## 文字选项

| 值 | 描述 |
|-------|-------------|
| `none` | 纯视觉，无文字元素 |
| `title-only` | 单个标题 |
| `title-subtitle` | 标题 + 副标题 |
| `text-rich` | 标题 + 副标题 + 关键词标签（2-4 个） |

## 氛围选项

| 值 | 描述 |
|-------|-------------|
| `subtle` | 低对比度、柔和颜色、平静美学 |
| `balanced` | 中等对比度、正常饱和度、通用 |
| `bold` | 高对比度、鲜艳颜色、动感能量 |

## 位置选项

| 值 | 描述 |
|-------|-------------|
| `bottom-right` | 右下角（默认，最常用） |
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中 |
| `top-right` | 右上角 |

## 宽高比选项

| 值 | 描述 | 最适用于 |
|-------|-------------|----------|
| `2.35:1` | 电影宽幅 | 文章头图、博客封面 |
| `16:9` | 标准宽屏 | 演示文稿、视频缩略图 |
| `1:1` | 正方形 | 社交媒体、头像图片 |

## 自定义色板字段

| 字段 | 必填 | 描述 |
|-------|----------|-------------|
| `name` | 是 | 唯一色板标识符（kebab-case） |
| `description` | 是 | 色板传达的感觉 |
| `colors.primary` | 否 | 主要颜色（十六进制数组） |
| `colors.background` | 否 | 背景色（十六进制） |
| `colors.accents` | 否 | 强调色（十六进制数组） |
| `decorative_hints` | 否 | 装饰元素和图案 |
| `best_for` | 否 | 推荐的内容类型 |

## 示例：最小偏好设置

```yaml
---
version: 3
watermark:
  enabled: true
  content: "@myhandle"
preferred_type: null
preferred_palette: elegant
preferred_rendering: hand-drawn
preferred_text: title-only
preferred_mood: balanced
quick_mode: false
---
```

## 示例：完整偏好设置

```yaml
---
version: 3
watermark:
  enabled: true
  content: "myblog.com"
  position: bottom-right

preferred_type: conceptual

preferred_palette: cool

preferred_rendering: digital

preferred_text: title-subtitle

preferred_mood: subtle

default_aspect: "16:9"

quick_mode: true

language: en

custom_palettes:
  - name: corporate-tech
    description: "专业 B2B 技术色板"
    colors:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    decorative_hints: "简洁线条、微妙渐变、电路图案"
    best_for: "SaaS、企业、技术"
---
```

## 从 v2 迁移

加载 v2 schema 时自动升级：

| v2 字段 | v3 字段 | 迁移方式 |
|----------|----------|-----------|
| `version: 2` | `version: 3` | 更新 |
| `preferred_style` | `preferred_palette` + `preferred_rendering` | 使用预设映射表 |
| `custom_styles` | `custom_palettes` | 重命名、重构字段 |

**风格 → 色板 + 渲染映射**：

| v2 `preferred_style` | v3 `preferred_palette` | v3 `preferred_rendering` |
|----------------------|----------------------|-------------------------|
| `elegant` | `elegant` | `hand-drawn` |
| `blueprint` | `cool` | `digital` |
| `chalkboard` | `dark` | `chalk` |
| `dark-atmospheric` | `dark` | `digital` |
| `editorial-infographic` | `cool` | `digital` |
| `fantasy-animation` | `pastel` | `painterly` |
| `flat-doodle` | `pastel` | `flat-vector` |
| `intuition-machine` | `retro` | `digital` |
| `minimal` | `mono` | `flat-vector` |
| `nature` | `earth` | `hand-drawn` |
| `notion` | `mono` | `digital` |
| `pixel-art` | `vivid` | `pixel` |
| `playful` | `pastel` | `hand-drawn` |
| `retro` | `retro` | `digital` |
| `sketch-notes` | `warm` | `hand-drawn` |
| `vector-illustration` | `retro` | `flat-vector` |
| `vintage` | `retro` | `hand-drawn` |
| `warm` | `warm` | `hand-drawn` |
| null (auto) | null | null |

**自定义风格迁移**：

| v2 字段 | v3 字段 |
|----------|----------|
| `custom_styles[].name` | `custom_palettes[].name` |
| `custom_styles[].description` | `custom_palettes[].description` |
| `custom_styles[].color_palette` | `custom_palettes[].colors` |
| `custom_styles[].visual_elements` | `custom_palettes[].decorative_hints` |
| `custom_styles[].typography` | （已移除——由渲染决定） |
| `custom_styles[].best_for` | `custom_palettes[].best_for` |

## 从 v1 迁移

加载 v1 schema 时自动升级到 v3：

| v1 字段 | v3 字段 | 默认值 |
|----------|----------|---------------|
| （缺失） | `version` | 3 |
| （缺失） | `preferred_palette` | null |
| （缺失） | `preferred_rendering` | null |
| （缺失） | `preferred_text` | title-only |
| （缺失） | `preferred_mood` | balanced |
| （缺失） | `quick_mode` | false |

v1 `--no-title` 标志映射为 `preferred_text: none`。
