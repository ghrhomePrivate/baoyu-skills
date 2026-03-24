# 步骤 2：确认选项

## 目的

验证全部 6 个维度 + 宽高比。

## 跳过条件

| 条件 | 跳过的问题 | 仍然询问 |
|-----------|-------------------|-------------|
| `--quick` 标志 | 类型、色板、渲染、文字、氛围、字体 | **宽高比**（除非指定 `--aspect`） |
| 全部 6 个维度 + `--aspect` 均已指定 | 全部 | 无 |
| EXTEND.md 中 `quick_mode: true` | 类型、色板、渲染、文字、氛围、字体 | **宽高比**（除非指定 `--aspect`） |
| 其他情况 | 无 | 全部 7 个问题 |

**重要**：宽高比始终会被询问，除非通过 `--aspect` CLI 标志显式指定。EXTEND.md 中的用户预设作为推荐选项显示，而非自动选择。

## 快速模式输出

跳过 6 个维度时：

```
快速模式：自动选择的维度
• 类型：[type]（[原因]）
• 色板：[palette]（[原因]）
• 渲染：[rendering]（[原因]）
• 文字：[text]（[原因]）
• 氛围：[mood]（[原因]）
• 字体：[font]（[原因]）

[然后询问问题 7：宽高比]
```

## 确认流程

**语言**：自动确定（用户输入语言 > 保存的偏好 > 源内容语言）。无需询问。

在**单次 AskUserQuestion 调用**中呈现所有选项（最多 4 个问题）。

如果某个维度已通过 CLI 标志或 `--style` 预设指定，则跳过该问题。

### 问题 1：类型（如果 `--type` 已指定则跳过）

```yaml
header: "类型"
question: "选择封面类型？"
multiSelect: false
options:
  - label: "[自动推荐的类型]（推荐）"
    description: "[基于内容信号的理由]"
  - label: "hero"
    description: "大面积视觉冲击，标题覆盖 - 产品发布、公告"
  - label: "conceptual"
    description: "概念可视化 - 技术、架构"
  - label: "typography"
    description: "文字为主的布局 - 观点、引言"
```

### 问题 2：色板（如果 `--palette` 或 `--style` 已指定则跳过）

```yaml
header: "色板"
question: "选择色板？"
multiSelect: false
options:
  - label: "[自动推荐的色板]（推荐）"
    description: "[基于内容信号的理由]"
  - label: "warm"
    description: "友好温暖 - 橙色、金黄色、赤陶色"
  - label: "elegant"
    description: "精致优雅 - 柔和珊瑚色、静谧青色、灰玫瑰色"
  - label: "cool"
    description: "技术专业 - 工程蓝、海军蓝、青色"
```

### 问题 3：渲染（如果 `--rendering` 或 `--style` 已指定则跳过）

显示兼容的渲染（兼容性矩阵中 ✓✓ 优先）：

```yaml
header: "渲染"
question: "选择渲染风格？"
multiSelect: false
options:
  - label: "[最佳兼容的渲染]（推荐）"
    description: "[基于色板 + 类型 + 内容的理由]"
  - label: "flat-vector"
    description: "干净轮廓、平面填充、几何图标"
  - label: "hand-drawn"
    description: "草图感、有机、不完美笔触"
  - label: "digital"
    description: "精致、精确、微妙渐变"
```

### 问题 4：字体（如果 `--font` 已指定则跳过）

```yaml
header: "字体"
question: "选择字体风格？"
multiSelect: false
options:
  - label: "[自动推荐的字体]（推荐）"
    description: "[基于内容信号的理由]"
  - label: "clean"
    description: "现代几何无衬线 - 技术、专业"
  - label: "handwritten"
    description: "温暖手写体 - 个人、友好"
  - label: "serif"
    description: "经典优雅 - 编辑、奢华"
  - label: "display"
    description: "粗体装饰 - 公告、娱乐"
```

### 问题 5：其他设置（如果所有剩余维度均已指定则跳过）

将剩余设置合并为一个问题。包含：输出目录（如果无偏好 + 文件路径输入）、文字、氛围、比例。显示自动选择的值作为推荐选项。用户可以接受全部或通过"其他"输入调整。

**需要询问输出目录时**（无 `default_output_dir` 偏好 + 文件路径输入）：

```yaml
header: "设置"
question: "输出 / 文字 / 氛围 / 比例？"
multiSelect: false
options:
  - label: "imgs/ / [自动文字] / [自动氛围] / [预设比例]（推荐）"
    description: "{article-dir}/imgs/，[文字理由]，[氛围理由]，[比例来源]"
  - label: "same-dir / [自动文字] / [自动氛围] / [预设比例]"
    description: "{article-dir}/，与文章同目录"
  - label: "independent / [自动文字] / [自动氛围] / [预设比例]"
    description: "cover-image/{topic-slug}/，独立于文章"
```

**输出目录已设置时**（偏好已存在或粘贴内容）：

```yaml
header: "设置"
question: "文字 / 氛围 / 比例？"
multiSelect: false
options:
  - label: "[自动文字] / [自动氛围] / [预设比例]（推荐）"
    description: "自动选择：[文字理由]，[氛围理由]，[比例来源]"
  - label: "[自动文字] / bold / [预设比例]"
    description: "高对比、鲜艳 — 匹配 [内容信号]"
  - label: "[自动文字] / subtle / [预设比例]"
    description: "低对比、柔和 — 平静、专业"
```

*注意*："其他"（自动添加）允许输入自定义组合。解析与问题格式匹配的 `/` 分隔值。

## 响应后

使用确认的维度继续步骤 3。
