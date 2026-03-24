---
name: baoyu-slide-deck
description: 从内容生成专业幻灯片图像。创建包含样式说明的大纲，然后生成单独的幻灯片图像。当用户要求"创建幻灯片"、"制作演示文稿"、"生成幻灯片"、"slide deck"或"PPT"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-slide-deck
    requires:
      anyBins:
        - bun
        - npx
---

# 幻灯片生成器

将内容转化为专业幻灯片图像。

## 用法

```bash
/baoyu-slide-deck path/to/content.md
/baoyu-slide-deck path/to/content.md --style sketch-notes
/baoyu-slide-deck path/to/content.md --audience executives
/baoyu-slide-deck path/to/content.md --lang zh
/baoyu-slide-deck path/to/content.md --slides 10
/baoyu-slide-deck path/to/content.md --outline-only
/baoyu-slide-deck  # 然后粘贴内容
```

## 脚本目录

**Agent 执行说明**：
1. 确定此 SKILL.md 文件的目录路径为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 解析 `${BUN_X}` 运行时：如果已安装 `bun` → `bun`；如果有 `npx` → `npx -y bun`；否则建议安装 bun

| 脚本 | 用途 |
|--------|---------|
| `scripts/merge-to-pptx.ts` | 将幻灯片合并为 PowerPoint |
| `scripts/merge-to-pdf.ts` | 将幻灯片合并为 PDF |

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--style <name>` | 视觉风格：预设名称、`custom` 或自定义风格名称 |
| `--audience <type>` | 目标受众：beginners、intermediate、experts、executives、general |
| `--lang <code>` | 输出语言（en、zh、ja 等） |
| `--slides <number>` | 目标幻灯片数量（推荐 8-25，最多 30） |
| `--outline-only` | 仅生成大纲，跳过图像生成 |
| `--prompts-only` | 生成大纲 + 提示词，跳过图像 |
| `--images-only` | 从已有提示词目录生成图像 |
| `--regenerate <N>` | 重新生成指定幻灯片：`--regenerate 3` 或 `--regenerate 2,5,8` |

**根据内容长度确定幻灯片数量**：
| 内容 | 幻灯片数 |
|---------|--------|
| < 1000 词 | 5-10 |
| 1000-3000 词 | 10-18 |
| 3000-5000 词 | 15-25 |
| > 5000 词 | 20-30（建议拆分） |

## 风格系统

### 预设

| 预设 | 维度 | 适用场景 |
|--------|------------|----------|
| `blueprint`（默认） | grid + cool + technical + balanced | 架构、系统设计 |
| `chalkboard` | organic + warm + handwritten + balanced | 教育、教程 |
| `corporate` | clean + professional + geometric + balanced | 投资演示、提案 |
| `minimal` | clean + neutral + geometric + minimal | 高管简报 |
| `sketch-notes` | organic + warm + handwritten + balanced | 教育、教程 |
| `watercolor` | organic + warm + humanist + minimal | 生活方式、健康 |
| `dark-atmospheric` | clean + dark + editorial + balanced | 娱乐、游戏 |
| `notion` | clean + neutral + geometric + dense | 产品演示、SaaS |
| `bold-editorial` | clean + vibrant + editorial + balanced | 产品发布、主题演讲 |
| `editorial-infographic` | clean + cool + editorial + dense | 技术解说、研究 |
| `fantasy-animation` | organic + vibrant + handwritten + minimal | 教育叙事 |
| `intuition-machine` | clean + cool + technical + dense | 技术文档、学术 |
| `pixel-art` | pixel + vibrant + technical + balanced | 游戏、开发者演讲 |
| `scientific` | clean + cool + technical + dense | 生物、化学、医学 |
| `vector-illustration` | clean + vibrant + humanist + balanced | 创意、儿童内容 |
| `vintage` | paper + warm + editorial + balanced | 历史、传统文化 |

### 风格维度

| 维度 | 选项 | 描述 |
|-----------|---------|-------------|
| **纹理** | clean、grid、organic、pixel、paper | 视觉纹理和背景处理 |
| **色调** | professional、warm、cool、vibrant、dark、neutral | 色温和配色风格 |
| **字体** | geometric、humanist、handwritten、editorial、technical | 标题和正文文字样式 |
| **密度** | minimal、balanced、dense | 每张幻灯片的信息密度 |

完整规格：`references/dimensions/*.md`

### 自动风格选择

| 内容信号 | 预设 |
|-----------------|--------|
| tutorial、learn、education、guide、beginner | `sketch-notes` |
| classroom、teaching、school、chalkboard | `chalkboard` |
| architecture、system、data、analysis、technical | `blueprint` |
| creative、children、kids、cute | `vector-illustration` |
| briefing、academic、research、bilingual | `intuition-machine` |
| executive、minimal、clean、simple | `minimal` |
| saas、product、dashboard、metrics | `notion` |
| investor、quarterly、business、corporate | `corporate` |
| launch、marketing、keynote、magazine | `bold-editorial` |
| entertainment、music、gaming、atmospheric | `dark-atmospheric` |
| explainer、journalism、science communication | `editorial-infographic` |
| story、fantasy、animation、magical | `fantasy-animation` |
| gaming、retro、pixel、developer | `pixel-art` |
| biology、chemistry、medical、scientific | `scientific` |
| history、heritage、vintage、expedition | `vintage` |
| lifestyle、wellness、travel、artistic | `watercolor` |
| 默认 | `blueprint` |

## 设计理念

幻灯片设计用于**阅读和分享**，而非现场演示：
- 每张幻灯片无需口头解说即可自解释
- 滚动浏览时逻辑连贯
- 每张幻灯片内包含所有必要的上下文
- 针对社交媒体分享优化

参见 `references/design-guidelines.md` 了解：
- 受众特定原则
- 视觉层次
- 内容密度指南
- 色彩和字体选择
- 字体推荐

参见 `references/layouts.md` 了解布局选项。

## 文件管理

### 输出目录

```
slide-deck/{topic-slug}/
├── source-{slug}.{ext}
├── outline.md
├── prompts/
│   └── 01-slide-cover.md, 02-slide-{slug}.md, ...
├── 01-slide-cover.png, 02-slide-{slug}.png, ...
├── {topic-slug}.pptx
└── {topic-slug}.pdf
```

**Slug**：提取主题（2-4 个词，kebab-case）。示例："Introduction to Machine Learning" → `intro-machine-learning`

**冲突处理**：参见步骤 1.3 了解已有内容检测和用户选项。

## 语言处理

**检测优先级**：
1. `--lang` 标志（显式指定）
2. EXTEND.md `language` 设置
3. 用户对话语言（输入语言）
4. 源内容语言

**规则**：所有回复使用用户的首选语言：
- 问题和确认
- 进度报告
- 错误消息
- 完成摘要

技术术语（风格名称、文件路径、代码）保持英文。

## 工作流程

复制此清单并在完成各项时打勾：

```
幻灯片进度：
- [ ] 步骤 1：设置与分析
  - [ ] 1.1 加载偏好设置
  - [ ] 1.2 分析内容
  - [ ] 1.3 检查已有内容 ⚠️ 必需
- [ ] 步骤 2：确认 ⚠️ 必需（第 1 轮，可选第 2 轮）
- [ ] 步骤 3：生成大纲
- [ ] 步骤 4：审查大纲（有条件）
- [ ] 步骤 5：生成提示词
- [ ] 步骤 6：审查提示词（有条件）
- [ ] 步骤 7：生成图像
- [ ] 步骤 8：合并为 PPTX/PDF
- [ ] 步骤 9：输出摘要
```

### 流程

```
输入 → 偏好设置 → 分析 → [检查已有内容?] → 确认（1-2 轮） → 大纲 → [审查大纲?] → 提示词 → [审查提示词?] → 图像 → 合并 → 完成
```

### 步骤 1：设置与分析

**1.1 加载偏好设置（EXTEND.md）**

检查 EXTEND.md 是否存在（按优先顺序）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-slide-deck/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-slide-deck/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-slide-deck/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-slide-deck/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────┬───────────────────┐
│                       路径                       │     位置          │
├──────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-slide-deck/EXTEND.md         │ 项目目录          │
├──────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-slide-deck/EXTEND.md   │ 用户主目录        │
└──────────────────────────────────────────────────┴───────────────────┘

**找到 EXTEND.md 时** → 读取、解析，**向用户输出摘要**：

```
📋 已从 [完整路径] 加载偏好设置
├─ 风格：[预设/自定义名称]
├─ 受众：[受众或"自动检测"]
├─ 语言：[语言或"自动检测"]
└─ 审查：[启用/禁用]
```

**未找到 EXTEND.md 时** → 使用 AskUserQuestion 进行首次设置或使用默认值继续。

**EXTEND.md 支持**：首选风格 | 自定义维度 | 默认受众 | 语言偏好 | 审查偏好

Schema：`references/config/preferences-schema.md`

**1.2 分析内容**

1. 保存源内容（如果粘贴，保存为 `source.md`）
   - **备份规则**：如果 `source.md` 存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 按照 `references/analysis-framework.md` 进行内容分析
3. 分析内容信号以获取风格推荐
4. 检测源语言
5. 确定推荐幻灯片数量
6. 从内容生成主题 slug

**1.3 检查已有内容** ⚠️ 必需

**必须在进入步骤 2 之前执行。**

使用 Bash 检查输出目录是否存在：

```bash
test -d "slide-deck/{topic-slug}" && echo "exists"
```

**如果目录存在**，使用 AskUserQuestion：

```
header: "已有内容"
question: "发现已有内容，如何处理？"
options:
  - label: "重新生成大纲"
    description: "保留图像，仅重新生成大纲"
  - label: "重新生成图像"
    description: "保留大纲，仅重新生成图像"
  - label: "备份并重新生成"
    description: "备份到 {slug}-backup-{timestamp}，然后全部重新生成"
  - label: "退出"
    description: "取消，保持已有内容不变"
```

**保存到 `analysis.md`**，包含：
- 主题、受众、内容信号
- 推荐风格（基于自动风格选择）
- 推荐幻灯片数量
- 语言检测

### 步骤 2：确认 ⚠️ 必需

**两轮确认**：第 1 轮始终执行，第 2 轮仅在选择"自定义维度"时执行。

**语言**：使用用户的输入语言或已保存的语言偏好。

**显示摘要**：
- 已识别的内容类型 + 主题
- 语言：[来自 EXTEND.md 或检测到的]
- **推荐风格**：[预设]（基于内容信号）
- **推荐幻灯片数**：[N]（基于内容长度）

#### 第 1 轮（始终执行）

**使用 AskUserQuestion** 提出全部 5 个问题：

**问题 1：风格**
```
header: "风格"
question: "这个幻灯片使用哪种视觉风格？"
options:
  - label: "{recommended_preset}（推荐）"
    description: "基于内容分析的最佳匹配"
  - label: "{alternative_preset}"
    description: "[备选风格描述]"
  - label: "自定义维度"
    description: "分别选择纹理、色调、字体、密度"
```

**问题 2：受众**
```
header: "受众"
question: "主要读者是谁？"
options:
  - label: "普通读者（推荐）"
    description: "广泛吸引力，内容通俗易懂"
  - label: "初学者/学习者"
    description: "侧重教育，清晰解释"
  - label: "专家/专业人士"
    description: "技术深度，领域知识"
  - label: "高管"
    description: "高层洞察，细节最少"
```

**问题 3：幻灯片数量**
```
header: "幻灯片"
question: "需要多少张幻灯片？"
options:
  - label: "{N} 张幻灯片（推荐）"
    description: "基于内容长度"
  - label: "更少（{N-3} 张幻灯片）"
    description: "更加精简，细节更少"
  - label: "更多（{N+3} 张幻灯片）"
    description: "更详细的拆分"
```

**问题 4：审查大纲**
```
header: "大纲"
question: "在生成提示词之前审查大纲？"
options:
  - label: "是，审查大纲（推荐）"
    description: "审查幻灯片标题和结构"
  - label: "否，跳过大纲审查"
    description: "直接进入提示词生成"
```

**问题 5：审查提示词**
```
header: "提示词"
question: "在生成图像之前审查提示词？"
options:
  - label: "是，审查提示词（推荐）"
    description: "审查图像生成提示词"
  - label: "否，跳过提示词审查"
    description: "直接进入图像生成"
```

#### 第 2 轮（仅在选择"自定义维度"时执行）

**使用 AskUserQuestion** 提出全部 4 个维度问题：

**问题 1：纹理**
```
header: "纹理"
question: "选择哪种视觉纹理？"
options:
  - label: "clean"
    description: "纯色实底，无纹理"
  - label: "grid"
    description: "微妙网格叠加，技术感"
  - label: "organic"
    description: "柔和纹理，手绘感"
  - label: "pixel"
    description: "像素块，8-bit 美学"
```
（注："paper" 可通过其他选项获得）

**问题 2：色调**
```
header: "色调"
question: "选择哪种色彩氛围？"
options:
  - label: "professional"
    description: "冷中性色，藏青/金色"
  - label: "warm"
    description: "大地色系，亲和"
  - label: "cool"
    description: "蓝色、灰色，分析感"
  - label: "vibrant"
    description: "高饱和度，大胆"
```
（注："dark"、"neutral" 可通过其他选项获得）

**问题 3：字体**
```
header: "字体"
question: "选择哪种字体风格？"
options:
  - label: "geometric"
    description: "现代无衬线，干净"
  - label: "humanist"
    description: "友好，易读"
  - label: "handwritten"
    description: "马克笔/毛笔，有机感"
  - label: "editorial"
    description: "杂志风格，戏剧感"
```
（注："technical" 可通过其他选项获得）

**问题 4：密度**
```
header: "密度"
question: "信息密度？"
options:
  - label: "balanced（推荐）"
    description: "每张 2-3 个要点"
  - label: "minimal"
    description: "单一焦点，最大留白"
  - label: "dense"
    description: "多个数据点，紧凑"
```

**第 2 轮完成后**：将自定义维度存储为风格配置。

**确认完成后**：
1. 用确认的偏好更新 `analysis.md`
2. 从问题 4 存储 `skip_outline_review` 标志
3. 从问题 5 存储 `skip_prompt_review` 标志
4. → 步骤 3

### 步骤 3：生成大纲

使用步骤 2 中确认的风格创建大纲。

**风格解析**：
- 如果选择预设 → 读取 `references/styles/{preset}.md`
- 如果自定义维度 → 从 `references/dimensions/` 读取维度文件并组合

**生成**：
1. 按照 `references/outline-template.md` 的结构
2. 从风格或维度构建 STYLE_INSTRUCTIONS
3. 应用确认的受众、语言、幻灯片数量
4. 保存为 `outline.md`

**生成完成后**：
- 如果使用 `--outline-only`，在此停止
- 如果 `skip_outline_review` 为 true → 跳过步骤 4，进入步骤 5
- 如果 `skip_outline_review` 为 false → 继续步骤 4

### 步骤 4：审查大纲（有条件）

**跳过此步骤**如果用户在步骤 2 中选择了"否，跳过大纲审查"。

**目的**：在提示词生成前审查大纲结构。

**语言**：使用用户的输入语言或已保存的语言偏好。

**显示**：
- 总幻灯片数：N
- 风格：[预设名称或"自定义：纹理+色调+字体+密度"]
- 逐页摘要表：

```
| # | 标题 | 类型 | 布局 |
|---|-------|------|--------|
| 1 | [标题] | 封面 | title-hero |
| 2 | [标题] | 内容 | [布局] |
| 3 | [标题] | 内容 | [布局] |
| ... | ... | ... | ... |
```

**使用 AskUserQuestion**：
```
header: "确认"
question: "准备好生成提示词了吗？"
options:
  - label: "是，继续（推荐）"
    description: "生成图像提示词"
  - label: "先编辑大纲"
    description: "我会在继续前修改 outline.md"
  - label: "重新生成大纲"
    description: "用不同方式创建新大纲"
```

**收到回复后**：
1. 如果"先编辑大纲" → 通知用户编辑 `outline.md`，准备好后再次询问
2. 如果"重新生成大纲" → 返回步骤 3
3. 如果"是，继续" → 进入步骤 5

### 步骤 5：生成提示词

1. 读取 `references/base-prompt.md`
2. 对大纲中的每张幻灯片：
   - 从大纲中提取 STYLE_INSTRUCTIONS（不再重新读取风格文件）
   - 添加幻灯片特定内容
   - 如果指定了 `Layout:`，从 `references/layouts.md` 包含布局指导
3. 保存到 `prompts/` 目录
   - **备份规则**：如果提示词文件存在，重命名为 `prompts/NN-slide-{slug}-backup-YYYYMMDD-HHMMSS.md`

**生成完成后**：
- 如果使用 `--prompts-only`，在此停止并输出提示词摘要
- 如果 `skip_prompt_review` 为 true → 跳过步骤 6，进入步骤 7
- 如果 `skip_prompt_review` 为 false → 继续步骤 6

### 步骤 6：审查提示词（有条件）

**跳过此步骤**如果用户在步骤 2 中选择了"否，跳过提示词审查"。

**目的**：在图像生成前审查提示词。

**语言**：使用用户的输入语言或已保存的语言偏好。

**显示**：
- 总提示词数：N
- 风格：[预设名称或自定义维度]
- 提示词列表：

```
| # | 文件名 | 幻灯片标题 |
|---|----------|-------------|
| 1 | 01-slide-cover.md | [标题] |
| 2 | 02-slide-xxx.md | [标题] |
| ... | ... | ... |
```

- 提示词目录路径：`prompts/`

**使用 AskUserQuestion**：
```
header: "确认"
question: "准备好生成幻灯片图像了吗？"
options:
  - label: "是，继续（推荐）"
    description: "生成所有幻灯片图像"
  - label: "先编辑提示词"
    description: "我会在继续前修改提示词"
  - label: "重新生成提示词"
    description: "用不同方式创建新提示词"
```

**收到回复后**：
1. 如果"先编辑提示词" → 通知用户编辑提示词，准备好后再次询问
2. 如果"重新生成提示词" → 返回步骤 5
3. 如果"是，继续" → 进入步骤 7

### 步骤 7：生成图像

**使用 `--images-only` 时**：从此步骤开始，使用已有提示词。

**使用 `--regenerate N` 时**：仅重新生成指定的幻灯片。

**标准流程**：
1. 选择可用的图像生成技能
2. 生成会话 ID：`slides-{topic-slug}-{timestamp}`
3. 对每张幻灯片：
   - **备份规则**：如果图像文件存在，重命名为 `NN-slide-{slug}-backup-YYYYMMDD-HHMMSS.png`
   - 使用相同会话 ID 按顺序生成图像
4. 报告进度："已生成 X/N"（使用用户语言）
5. 失败时自动重试一次，然后再报告错误

### 步骤 8：合并为 PPTX 和 PDF

```bash
${BUN_X} {baseDir}/scripts/merge-to-pptx.ts <slide-deck-dir>
${BUN_X} {baseDir}/scripts/merge-to-pdf.ts <slide-deck-dir>
```

### 步骤 9：输出摘要

**语言**：使用用户的输入语言或已保存的语言偏好。

```
幻灯片生成完成！

主题：[主题]
风格：[预设名称或自定义维度]
位置：[目录路径]
幻灯片：共 N 张

- 01-slide-cover.png - 封面
- 02-slide-intro.png - 内容
- ...
- {NN}-slide-back-cover.png - 封底

大纲：outline.md
PPTX：{topic-slug}.pptx
PDF：{topic-slug}.pdf
```

## 部分工作流程

| 选项 | 工作流程 |
|--------|----------|
| `--outline-only` | 仅步骤 1-3（大纲生成后停止） |
| `--prompts-only` | 步骤 1-5（生成提示词，跳过图像） |
| `--images-only` | 跳到步骤 7（需要已有 prompts/） |
| `--regenerate N` | 仅重新生成指定幻灯片 |

### 使用 `--prompts-only`

生成大纲和提示词，不生成图像：

```bash
/baoyu-slide-deck content.md --prompts-only
```

输出：`outline.md` + `prompts/*.md` 可供审查/编辑。

### 使用 `--images-only`

从已有提示词生成图像（从步骤 7 开始）：

```bash
/baoyu-slide-deck slide-deck/topic-slug/ --images-only
```

前提条件：
- 包含幻灯片提示词文件的 `prompts/` 目录
- 包含风格信息的 `outline.md`

### 使用 `--regenerate`

重新生成指定幻灯片：

```bash
# 单张幻灯片
/baoyu-slide-deck slide-deck/topic-slug/ --regenerate 3

# 多张幻灯片
/baoyu-slide-deck slide-deck/topic-slug/ --regenerate 2,5,8
```

流程：
1. 读取指定幻灯片的已有提示词
2. 仅为这些幻灯片重新生成图像
3. 重新生成 PPTX/PDF

## 幻灯片修改

### 快速参考

| 操作 | 命令 | 手动步骤 |
|--------|---------|--------------|
| **编辑** | `--regenerate N` | **先更新提示词文件** → 重新生成图像 → 重新生成 PDF |
| **添加** | 手动 | 创建提示词 → 生成图像 → 重编后续序号 → 更新大纲 → 重新生成 PDF |
| **删除** | 手动 | 删除文件 → 重编后续序号 → 更新大纲 → 重新生成 PDF |

### 编辑单张幻灯片

1. **先更新提示词文件** `prompts/NN-slide-{slug}.md`
2. 运行：`/baoyu-slide-deck <dir> --regenerate N`
3. 或手动重新生成图像 + PDF

**重要**：更新幻灯片时，必须**先**更新提示词文件（`prompts/NN-slide-{slug}.md`），然后再重新生成。这确保更改有据可查且可复现。

### 添加新幻灯片

1. 在指定位置创建提示词：`prompts/NN-slide-{new-slug}.md`
2. 使用相同会话 ID 生成图像
3. **重编序号**：后续文件 NN+1（slug 不变）
4. 更新 `outline.md`
5. 重新生成 PPTX/PDF

### 删除幻灯片

1. 删除 `NN-slide-{slug}.png` 和 `prompts/NN-slide-{slug}.md`
2. **重编序号**：后续文件 NN-1（slug 不变）
3. 更新 `outline.md`
4. 重新生成 PPTX/PDF

### 文件命名

格式：`NN-slide-[slug].png`
- `NN`：两位数序号（01、02、...）
- `slug`：从内容派生的 kebab-case（2-5 个词，唯一）

**重编序号规则**：仅 NN 变化，slug 保持不变。

参见 `references/modification-guide.md` 了解完整详情。

## 参考文件

| 文件 | 内容 |
|------|---------|
| `references/analysis-framework.md` | 演示文稿内容分析 |
| `references/outline-template.md` | 大纲结构和格式 |
| `references/modification-guide.md` | 编辑、添加、删除幻灯片工作流程 |
| `references/content-rules.md` | 内容和风格指南 |
| `references/design-guidelines.md` | 受众、字体、颜色、视觉元素 |
| `references/layouts.md` | 布局选项和选择技巧 |
| `references/base-prompt.md` | 图像生成基础提示词 |
| `references/dimensions/*.md` | 维度规格（纹理、色调、字体、密度） |
| `references/dimensions/presets.md` | 预设 → 维度映射 |
| `references/styles/<style>.md` | 完整风格规格（旧版） |
| `references/config/preferences-schema.md` | EXTEND.md 结构 |

## 注意事项

- 图像生成：每张幻灯片 10-30 秒
- 失败时自动重试一次
- 对敏感公众人物使用风格化替代方案
- 通过会话 ID 保持风格一致性
- **步骤 2 确认为必需** - 不可跳过（风格、受众、幻灯片数、大纲审查、提示词审查）
- **步骤 4 有条件** - 仅在用户在步骤 2 中请求大纲审查时
- **步骤 6 有条件** - 仅在用户在步骤 2 中请求提示词审查时

## 扩展支持

通过 EXTEND.md 自定义配置。参见**步骤 1.1**了解路径和支持的选项。
