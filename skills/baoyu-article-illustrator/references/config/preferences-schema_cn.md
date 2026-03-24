---
name: preferences-schema
description: baoyu-article-illustrator 用户偏好设置的 EXTEND.md YAML schema
---

# 偏好设置 Schema

## 完整 Schema

```yaml
---
version: 1

watermark:
  enabled: false
  content: ""
  position: bottom-right  # bottom-right|bottom-left|bottom-center|top-right

preferred_style:
  name: null              # 内置或自定义风格名称
  description: ""         # 覆盖/备注

language: null            # zh|en|ja|ko|auto

default_output_dir: null  # same-dir|illustrations-subdir|independent

custom_styles:
  - name: my-style
    description: "风格描述"
    color_palette:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    visual_elements: "简洁线条、几何形状"
    typography: "现代无衬线字体"
    best_for: "商务、教育"
---
```

## 字段参考

| 字段 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `version` | int | 1 | Schema 版本 |
| `watermark.enabled` | bool | false | 启用水印 |
| `watermark.content` | string | "" | 水印文字（@用户名或自定义） |
| `watermark.position` | enum | bottom-right | 图片上的位置 |
| `preferred_style.name` | string | null | 风格名称或 null |
| `preferred_style.description` | string | "" | 自定义备注/覆盖 |
| `language` | string | null | 输出语言（null = 自动检测） |
| `default_output_dir` | enum | null | 输出目录偏好（null = 每次询问） |
| `custom_styles` | array | [] | 用户自定义风格 |

## 位置选项

| 值 | 描述 |
|----|------|
| `bottom-right` | 右下角（默认，最常用） |
| `bottom-left` | 左下角 |
| `bottom-center` | 底部居中 |
| `top-right` | 右上角 |

## 输出目录选项

| 值 | 描述 |
|----|------|
| `same-dir` | 与文章同目录 |
| `illustrations-subdir` | `{article-dir}/illustrations/` 子目录 |
| `independent` | 工作目录下的 `illustrations/{topic-slug}/` |

## 自定义风格字段

| 字段 | 必填 | 描述 |
|------|------|------|
| `name` | 是 | 唯一风格标识符（kebab-case） |
| `description` | 是 | 风格传达的感觉 |
| `color_palette.primary` | 否 | 主色调（数组） |
| `color_palette.background` | 否 | 背景色 |
| `color_palette.accents` | 否 | 强调色（数组） |
| `visual_elements` | 否 | 装饰元素 |
| `typography` | 否 | 字体/字形风格 |
| `best_for` | 否 | 推荐的内容类型 |

## 示例：最简偏好设置

```yaml
---
version: 1
watermark:
  enabled: true
  content: "@myusername"
preferred_style:
  name: notion
---
```

## 示例：完整偏好设置

```yaml
---
version: 1
watermark:
  enabled: true
  content: "@myaccount"
  position: bottom-right

preferred_style:
  name: notion
  description: "技术文章的简洁插图"

language: zh

custom_styles:
  - name: corporate
    description: "专业 B2B 风格"
    color_palette:
      primary: ["#1E3A5F", "#4A90D9"]
      background: "#F5F7FA"
      accents: ["#00B4D8", "#48CAE4"]
    visual_elements: "简洁线条、微妙渐变、几何形状"
    typography: "现代无衬线字体、专业感"
    best_for: "商务、SaaS、企业"
---
```
