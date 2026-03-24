# 大纲模板

包含风格说明的幻灯片大纲标准结构。

## 大纲格式

```markdown
# Slide Deck Outline

**Topic**: [主题描述]
**Style**: [预设名称或 "custom"]
**Dimensions**: [texture] + [mood] + [typography] + [density]
**Audience**: [目标受众]
**Language**: [输出语言]
**Slide Count**: N slides
**Generated**: YYYY-MM-DD HH:mm

---

<STYLE_INSTRUCTIONS>
Design Aesthetic: [2-3 句描述，组合维度特征]

Background:
  Texture: [来自纹理维度]
  Base Color: [来自色调维度配色]

Typography:
  Headlines: [来自字体维度 - 描述视觉外观]
  Body: [来自字体维度 - 描述视觉外观]

Color Palette:
  Primary Text: [名称] ([十六进制]) - [用途]
  Background: [名称] ([十六进制]) - [用途]
  Accent 1: [名称] ([十六进制]) - [用途]
  Accent 2: [名称] ([十六进制]) - [用途]

Visual Elements:
  - [来自纹理 + 色调组合的元素 1]
  - [带渲染指导的元素 2]
  - ...

Density Guidelines:
  - Content per slide: [来自密度维度]
  - Whitespace: [来自密度维度]

Style Rules:
  Do: [来自维度组合的指导方针]
  Don't: [来自维度组合的反面模式]
</STYLE_INSTRUCTIONS>

---

[幻灯片条目如下...]
```

## 从维度构建 STYLE_INSTRUCTIONS

使用自定义维度或预设时，通过组合以下内容构建 STYLE_INSTRUCTIONS：

### 1. 设计美学

将所有四个维度的特征组合成 2-3 句话：

| 纹理 | 贡献 |
|---------|--------------|
| clean | "干净、数字精度、锐利边缘" |
| grid | "技术网格叠加、工程精度" |
| organic | "手绘感、柔和纹理" |
| pixel | "像素块美学、8-bit 魅力" |
| paper | "做旧纸张纹理、复古特色" |

| 色调 | 贡献 |
|------|--------------|
| professional | "专业藏青金色配色" |
| warm | "温暖大地色调，营造亲和氛围" |
| cool | "冷色调分析蓝灰系" |
| vibrant | "大胆、高饱和度色彩，充满活力" |
| dark | "深邃电影感背景配发光点缀" |
| neutral | "极简灰度精致感" |

### 2. 背景

来自 `references/dimensions/texture.md`：
- 纹理描述
- 来自色调配色的基础颜色

### 3. 字体排印

来自 `references/dimensions/typography.md`：
- 标题视觉描述（非字体名称）
- 正文视觉描述（非字体名称）

**重要**：描述外观以用于图像生成："粗体几何无衬线，O 形状为完美圆形"，而非 "Inter 字体"。

### 4. 色彩方案

来自 `references/dimensions/mood.md`：
- 复制所选色调的配色规格
- 包含十六进制代码和用途说明

### 5. 视觉元素

组合纹理和色调特征：

| 组合 | 视觉元素 |
|-------------|-----------------|
| clean + professional | 干净图表、轮廓图标、结构化网格 |
| grid + cool | 技术示意图、尺寸线、蓝图 |
| organic + warm | 手绘图标、笔触、涂鸦 |
| pixel + vibrant | 像素艺术图标、复古游戏元素 |
| paper + warm | 复古印章、做旧元素、棕褐色叠加 |

### 6. 密度指南

来自 `references/dimensions/density.md`：
- 每张幻灯片的内容限制
- 留白要求
- 元素数量指南

### 7. 风格规则

组合维度特定规则：

**各纹理的"做"规则**：
- clean：保持锐利边缘，使用网格对齐
- grid：展示精确测量，使用技术图表
- organic：允许不完美，微妙叠加层次
- pixel：保持锯齿边缘，使用粗块元素
- paper：添加微妙做旧效果，使用暖色调

**各纹理的"不做"规则**：
- clean：不使用手绘元素
- grid：不使用有机曲线
- organic：不使用完美几何
- pixel：不平滑边缘
- paper：不使用明亮的数字色彩

## 封面幻灯片模板

```markdown
## Slide 1 of N

**Type**: Cover
**Filename**: 01-slide-cover.png

// NARRATIVE GOAL
[此幻灯片在故事弧中的作用]

// KEY CONTENT
Headline: [主标题]
Sub-headline: [支持性标语]

// VISUAL
[详细的视觉描述 - 具体元素、构图、氛围]

// LAYOUT
Layout: [可选：布局库中的布局名称，如 title-hero]
[构图、层次、空间排列]
```

## 内容幻灯片模板

```markdown
## Slide X of N

**Type**: Content
**Filename**: {NN}-slide-{slug}.png

// NARRATIVE GOAL
[此幻灯片在故事弧中的作用]

// KEY CONTENT
Headline: [主要信息 - 叙事性，非标签]
Sub-headline: [支持上下文]
Body:
- [要点 1，含具体细节]
- [要点 2，含具体细节]
- [要点 3，含具体细节]

// VISUAL
[详细的视觉描述]

// LAYOUT
Layout: [可选：布局库中的布局名称]
[构图、层次、空间排列]
```

## 封底幻灯片模板

```markdown
## Slide N of N

**Type**: Back Cover
**Filename**: {NN}-slide-back-cover.png

// NARRATIVE GOAL
[有意义的结语 - 不仅仅是"谢谢"]

// KEY CONTENT
Headline: [令人难忘的结束语或行动号召]
Body: [可选的总结要点或下一步]

// VISUAL
[强化核心信息的视觉效果]

// LAYOUT
Layout: [可选：布局库中的布局名称]
[简洁、有冲击力的构图]
```

## STYLE_INSTRUCTIONS 块

`<STYLE_INSTRUCTIONS>` 块是此大纲中风格信息的唯一权威来源。

| 部分 | 内容 | 来源 |
|---------|---------|--------|
| Design Aesthetic | 整体视觉方向 | 所有维度组合 |
| Background | 基础颜色和纹理细节 | 纹理 + 色调维度 |
| Typography | 字体描述（视觉性，非名称） | 字体维度 |
| Color Palette | 命名颜色含十六进制代码和用途 | 色调维度 |
| Visual Elements | 图形元素含渲染说明 | 纹理 + 色调维度 |
| Density Guidelines | 内容限制和留白 | 密度维度 |
| Style Rules | 做/不做指导方针 | 维度组合 |

**重要**：
- 字体描述必须描述视觉外观（如"圆润无衬线"、"粗体几何"），因为图像生成器无法使用字体名称
- 提示词应从此大纲提取 STYLE_INSTRUCTIONS，而非重新读取风格文件

## 预设 → 维度参考

使用预设时，在 `references/dimensions/presets.md` 中查找维度：

| 预设 | 维度 |
|--------|------------|
| blueprint | grid + cool + technical + balanced |
| sketch-notes | organic + warm + handwritten + balanced |
| corporate | clean + professional + geometric + balanced |
| minimal | clean + neutral + geometric + minimal |
| ... | 完整映射参见 presets.md |

## 分节线

在以下内容之间使用 `---`（水平线）：
- 头部元数据和 STYLE_INSTRUCTIONS
- STYLE_INSTRUCTIONS 和第一张幻灯片
- 每个幻灯片条目

## 幻灯片编号

- 封面始终是 Slide 1
- 内容幻灯片使用连续编号
- 封底始终是最后一张幻灯片（N）
- 文件名前缀匹配幻灯片位置：`01-`、`02-` 等

## 文件名 Slug

从幻灯片内容生成有意义的 slug：

| 幻灯片类型 | Slug 模式 | 示例 |
|------------|--------------|---------|
| 封面 | `cover` | `01-slide-cover.png` |
| 内容 | `{topic-slug}` | `02-slide-problem-statement.png` |
| 封底 | `back-cover` | `10-slide-back-cover.png` |

Slug 规则：
- Kebab-case（小写、连字符）
- 从标题或主要主题派生
- 最多 30 个字符
- 在幻灯片组内唯一
