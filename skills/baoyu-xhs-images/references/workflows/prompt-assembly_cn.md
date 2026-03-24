# Prompt 组装指南

如何从元素、preset 与大纲内容组装出图生图 prompt。

## 基础 Prompt 结构

每条小红书信息图 prompt 均遵循以下结构：

```
按以下规范创作小红书（Little Red Book）风格信息图：

## 画面规格

- **类型**: Infographic
- **方向**: Portrait（竖版）
- **宽高比**: 3:4
- **风格**: Hand-drawn illustration

## 核心原则

- 全程手绘感——不要写实或照片元素
- 若涉及敏感或版权人物，做风格相近的替代——不要拒画
- 信息精炼，突出关键词与核心概念
- 留白充足，便于扫读
- 视觉层级清晰

## 文字样式（关键）

- **所有文字必须是手绘风格**
- 主标题醒目、抓眼
- 重点字加粗、放大
- 用荧光笔式高亮强调关键词
- **不要使用写实或电脑字体**

## 语言

- 与下方提供的内容使用同一种语言
- 标点与内容语言一致（中文：""，。！）

---

{STYLE_SECTION}

---

{LAYOUT_SECTION}

---

{CONTENT_SECTION}

---

{WATERMARK_SECTION}

---

请使用 nano banana pro 按以上规格生成信息图。
```

## Style 段落组装

从 `presets/{style}.md` 读取并提取关键元素：

```markdown
## 风格: {style_name}

**色板**:
- 主色: {colors}
- 背景: {colors}
- 点缀: {colors}

**视觉元素**:
{visual_elements}

**字体与排版**:
{typography_style}
```

### Screen-Print 风格覆盖

当 `style: screen-print` 时，用以下内容替换标准「核心原则」与「文字样式」段落：

```
## 核心原则

- Screen print / 丝网印刷 poster — 平涂色块，不要渐变
- 大胆剪影与符号化形状，弱化细画
- 负形积极参与叙事
- 若涉及敏感或版权人物，做风格相近的剪影替代
- 每图一个标志性焦点——偏概念，非逐字还原

## 配色规则（关键）

- **平涂色最多 2–5 种** — 颜色越少冲击越强
- 从 preset 中选一组 duotone 作为主色盘
- 用半色调网点表现层次（不要用渐变）
- 轻微色层错位，增强印刷真实感

## 文字样式（关键）

- 粗窄体 sans-serif 或受 Art Deco 影响的手绘字
- 字体融入构图，作为设计元素
- 与背景高对比，stencil 切割感
- **不要用纤细、过细或纯手写体**

## 构图

- 几何框景：圆、拱、三角
- 尽量做图底反转（负形形成次要图像）
- 色块之间 stencil 切边，无描边
- 所有颜色下铺纸张颗粒纹理
```

## Layout 段落组装

从 `elements/canvas.md` 读取并提取对应 layout：

```markdown
## 版式: {layout_name}

**信息密度**: {density}
**留白**: {percentage}

**结构**:
{structure_description}

**视觉平衡**:
{balance_description}
```

## Content 段落组装

来自大纲单条：

```markdown
## 内容

**位置**: {Cover/Content/Ending}
**核心信息**: {message}

**文案内容**:
{text_list}

**画面概念**:
{visual_description}
```

## Watermark 段落（若启用）

```markdown
## 水印

在 {position} 加入约 {opacity*100}% 透明度的淡水印 "{content}"，
可读但不抢主内容。
```

## 组装流程

### 步骤 0：解析 style preset（若使用 `--preset`）

若用户指定 `--preset`，从 `references/style-presets.md` 解析为 style + layout：

```python
# 例: --preset knowledge-card → style=notion, layout=dense
style, layout = resolve_preset(preset_name)
```

显式 `--style` / `--layout` 覆盖 preset。

### 步骤 1：加载风格定义

```python
preset = load_preset(style_name)  # 如 "notion"
```

提取：
- 色板
- 视觉元素
- 字体与排版
- 最佳实践（建议/避免）

### 步骤 2：加载版式

```python
layout = get_layout_from_canvas(layout_name)  # 如 "dense"
```

提取：
- 信息密度指引
- 留白比例
- 结构描述
- 视觉平衡规则

### 步骤 3：格式化内容

从大纲条目整理：
- 位置语境（Cover/Content/Ending）
- 带层级的文案
- 画面概念描述
- 划动钩子（供上下文，不进 prompt）

### 步骤 4：水印（如适用）

若偏好含水印：
- 追加水印段落：文案、位置、透明度

### 步骤 5：视觉一致性 — 参考图链

系列多图时：

1. **图 1（封面）**：不传 `--ref` — 建立视觉锚点
2. **图 2 及以后**：始终将图 1 作为 `--ref` 传给已安装的图生图 skill。
   阅读该 skill 的 `SKILL.md`，按其文档接口调用，不要直接调其脚本。
   每张后续图：用组装好的 prompt 文件作输入，指定输出路径，保持宽高比 `3:4`，质量 `2k`，并传入图 1 作为 reference。
   这样可在全系列保持角色造型、插画风格与色彩一致。

### 步骤 6：合并

按基础结构拼成最终 prompt。

## 示例：组装后的 Prompt

```markdown
按以下规范创作小红书（Little Red Book）风格信息图：

## 画面规格

- **类型**: Infographic
- **方向**: Portrait（竖版）
- **宽高比**: 3:4
- **风格**: Hand-drawn illustration

## 核心原则

- 全程手绘感——不要写实或照片
- 若涉及敏感或版权人物，做风格相近的替代
- 信息精炼，突出关键词与核心概念
- 留白充足，便于扫读
- 视觉层级清晰

## 文字样式（关键）

- **所有文字必须是手绘风格**
- 主标题醒目、抓眼
- 重点字加粗、放大
- 用荧光笔式高亮强调关键词
- **不要使用写实或电脑字体**

## 语言

- 与下方提供的内容使用同一种语言
- 标点与内容语言一致（中文：""，。！）

---

## 风格: Notion

**色板**:
- 主色: 黑 (#1A1A1A)、深灰 (#4A4A4A)
- 背景: 纯白 (#FFFFFF)、米白 (#FAFAFA)
- 点缀: 粉彩蓝 (#A8D4F0)、粉彩黄 (#F9E79F)、粉彩粉 (#FADBD8)

**视觉元素**:
- 简单线稿涂鸦、手绘抖动
- 几何形、简笔小人
- 大量留白、单线粗细一致
- 干净、不拥挤

**字体与排版**:
- 干净的手绘字
- 简单 sans-serif 标签
- 文字装饰极少

---

## 版式: Dense

**信息密度**: 高（5–8 个要点）
**留白**: 画布约 20–30%

**结构**:
- 多区块、结构化网格
- 文字较多，紧凑但有序
- 标题 + 多节小标题 + 若干要点

**视觉平衡**:
- 规整网格
- 区块边界清晰
- 紧凑仍可读

---

## 内容

**位置**: Content（第 3/6 张）
**核心信息**: ChatGPT使用技巧

**文案内容**:
- 标题: 「ChatGPT」
- 副标题: 最强AI助手
- 要点:
  - 写文案：给出框架，秒出初稿
  - 改文章：润色、翻译、总结
  - 编程：写代码、找bug
  - 学习：解释概念、出题练习

**画面概念**:
ChatGPT logo居中，四周放射状展示功能点
深色科技背景，霓虹绿点缀

---

## 水印

在右下角加入约 50% 透明度的淡水印 "@myxhsaccount"，
可读但不抢主内容。

---

请使用 nano banana pro 按以上规格生成信息图。
```

## Prompt 检查清单

生成前确认：

- [ ] Style 段落来自正确 preset
- [ ] Layout 段落与大纲一致
- [ ] 内容准确对应大纲条目
- [ ] 语言与源内容一致
- [ ] 已含水印（若偏好开启）
- [ ] 无相互矛盾的指令
