---
name: baoyu-xhs-images
description: 生成小红书信息图系列，含 11 种视觉风格与 8 种版式。将内容拆成 1–10 张卡通风格图片，针对小红书互动优化。适用于用户提到「小红书图片」「XHS images」「RedNote infographics」「小红书种草」，或需要面向中文平台的社交媒体信息图时。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-xhs-images
---

# 小红书信息图系列生成器

将复杂内容拆解为多种风格可选、适合小红书传播的吸睛信息图系列。

## 用法

```bash
# 根据内容自动选择风格与版式
/baoyu-xhs-images posts/ai-future/article.md

# 指定风格
/baoyu-xhs-images posts/ai-future/article.md --style notion

# 指定版式
/baoyu-xhs-images posts/ai-future/article.md --layout dense

# 组合风格与版式
/baoyu-xhs-images posts/ai-future/article.md --style notion --layout list

# 使用预设（风格 + 版式简写）
/baoyu-xhs-images posts/ai-future/article.md --preset knowledge-card

# 预设 + 覆盖版式
/baoyu-xhs-images posts/ai-future/article.md --preset poster --layout quadrant

# 直接输入正文
/baoyu-xhs-images
[paste content]

# 带选项的直接输入
/baoyu-xhs-images --style bold --layout comparison
[paste content]
```

## 选项

| 选项 | 说明 |
|------|------|
| `--style <name>` | 视觉风格（见风格一览） |
| `--layout <name>` | 信息版式（见版式一览） |
| `--preset <name>` | 风格 + 版式简写（见 [Style Presets](references/style-presets.md)） |

## 两个维度

| 维度 | 控制内容 | 可选值 |
|------|----------|--------|
| **Style** | 视觉审美：色彩、线条、装饰 | cute, fresh, warm, bold, minimal, retro, pop, notion, chalkboard, study-notes, screen-print |
| **Layout** | 信息结构：密度、排布 | sparse, balanced, dense, list, comparison, flow, mindmap, quadrant |

风格 × 版式可自由组合。例如：`--style notion --layout dense` 可得到偏知性、高信息密度的知识卡片感。

或使用预设：`--preset knowledge-card` → 一个 flag 同时指定风格 + 版式。详见 [Style Presets](references/style-presets.md)。

## 风格一览

| 风格 | 说明 |
|------|------|
| `cute`（默认） | 甜美、可爱、少女感——经典小红书审美 |
| `fresh` | 清爽、自然 |
| `warm` | 温馨、友好、易亲近 |
| `bold` | 强冲击、抓眼球 |
| `minimal` | 极简、精致 |
| `retro` | 复古、怀旧、潮流感 |
| `pop` | 鲜艳、有活力、吸睛 |
| `notion` | 极简手绘线稿、知性 |
| `chalkboard` | 黑板彩色粉笔、教育感 |
| `study-notes` | 拟真手写照片风，蓝笔 + 红批注 + 黄荧光 |
| `screen-print` | 粗海报风、网点纹理、有限色、符号化叙事 |

详细风格定义：`references/presets/<style>.md`

## 预设一览

按内容场景快速上手。使用 `--preset <name>`，或在步骤 2 中推荐。

**知识向**：

| 预设 | 风格 | 版式 | 适用 |
|------|------|------|------|
| `knowledge-card` | notion | dense | 干货知识卡、概念科普 |
| `checklist` | notion | list | 清单、排行榜、必备清单 |
| `concept-map` | notion | mindmap | 概念图、知识脉络 |
| `swot` | notion | quadrant | SWOT分析、四象限分类 |
| `tutorial` | chalkboard | flow | 教程步骤、操作流程 |
| `classroom` | chalkboard | balanced | 课堂笔记、知识讲解 |
| `study-guide` | study-notes | dense | 学习笔记、考试重点 |

**生活与分享**：

| 预设 | 风格 | 版式 | 适用 |
|------|------|------|------|
| `cute-share` | cute | balanced | 少女风分享、日常种草 |
| `girly` | cute | sparse | 甜美封面、氛围感 |
| `cozy-story` | warm | balanced | 生活故事、情感分享 |
| `product-review` | fresh | comparison | 产品对比、测评 |
| `nature-flow` | fresh | flow | 健康流程、自然主题 |

**观点与冲击**：

| 预设 | 风格 | 版式 | 适用 |
|------|------|------|------|
| `warning` | bold | list | 避坑指南、重要提醒 |
| `versus` | bold | comparison | 正反对比、强烈对照 |
| `clean-quote` | minimal | sparse | 金句、极简封面 |
| `pro-summary` | minimal | balanced | 专业总结、商务内容 |

**潮流与趣味**：

| 预设 | 风格 | 版式 | 适用 |
|------|------|------|------|
| `retro-ranking` | retro | list | 复古排行、经典盘点 |
| `throwback` | retro | balanced | 怀旧分享、老物件 |
| `pop-facts` | pop | list | 趣味冷知识、好玩的事 |
| `hype` | pop | sparse | 炸裂封面、惊叹分享 |

**海报与评论**：

| 预设 | 风格 | 版式 | 适用 |
|------|------|------|------|
| `poster` | screen-print | sparse | 海报风封面、影评书评 |
| `editorial` | screen-print | balanced | 观点文章、文化评论 |
| `cinematic` | screen-print | comparison | 电影对比、戏剧张力 |

完整预设定义：[references/style-presets.md](references/style-presets.md)

## 版式一览

| 版式 | 说明 |
|------|------|
| `sparse`（默认） | 信息极少、冲击最大（1–2 个要点） |
| `balanced` | 常规内容排布（3–4 个要点） |
| `dense` | 高密度、知识卡片风（5–8 个要点） |
| `list` | 列举与排行（4–7 条） |
| `comparison` | 左右对照 |
| `flow` | 流程与时间线（3–6 步） |
| `mindmap` | 中心放射思维导图（4–8 个分支） |
| `quadrant` | 四象限 / 环形分区 |

详细版式定义：`references/elements/canvas.md`

## 自动匹配

| 内容信号 | 风格 | 版式 | 推荐预设 |
|----------|------|------|----------|
| Beauty, fashion, cute, girl, pink | `cute` | sparse/balanced | `cute-share`, `girly` |
| Health, nature, clean, fresh, organic | `fresh` | balanced/flow | `product-review`, `nature-flow` |
| Life, story, emotion, feeling, warm | `warm` | balanced | `cozy-story` |
| Warning, important, must, critical | `bold` | list/comparison | `warning`, `versus` |
| Professional, business, elegant, simple | `minimal` | sparse/balanced | `clean-quote`, `pro-summary` |
| Classic, vintage, old, traditional | `retro` | balanced | `throwback`, `retro-ranking` |
| Fun, exciting, wow, amazing | `pop` | sparse/list | `hype`, `pop-facts` |
| Knowledge, concept, productivity, SaaS | `notion` | dense/list | `knowledge-card`, `checklist` |
| Education, tutorial, learning, teaching, classroom | `chalkboard` | balanced/dense | `tutorial`, `classroom` |
| Notes, handwritten, study guide, knowledge, realistic, photo | `study-notes` | dense/list/mindmap | `study-guide` |
| Movie, album, concert, poster, opinion, editorial, dramatic, cinematic | `screen-print` | sparse/comparison | `poster`, `editorial`, `cinematic` |

## 大纲策略

针对不同内容目标的三套差异化大纲策略：

### Strategy A：故事驱动型

| 方面 | 说明 |
|------|------|
| **Concept** | 以个人经历为主线，情绪共鸣优先 |
| **Features** | 从痛点切入，呈现前后变化，真实感强 |
| **Best for** | 测评、个人分享、蜕变故事 |
| **Structure** | 钩子 → 问题 → 发现 → 体验 → 结论 |

### Strategy B：信息密集型

| 方面 | 说明 |
|------|------|
| **Concept** | 价值优先，高效传递信息 |
| **Features** | 结构清晰、论点明确、专业可信 |
| **Best for** | 教程、对比、产品测评、清单 |
| **Structure** | 核心结论 → 信息卡 → 利弊 → 建议 |

### Strategy C：视觉优先型

| 方面 | 说明 |
|------|------|
| **Concept** | 视觉冲击为核心，文字极简 |
| **Features** | 大图、氛围感、一眼抓住 |
| **Best for** | 高颜值单品、生活方式、情绪向内容 |
| **Structure** | 主视觉 → 细节图 → 生活场景 → CTA |

## 目录结构

每次会话按内容 slug 建立独立目录：

```
xhs-images/{topic-slug}/
├── source-{slug}.{ext}             # 源文件（正文、图片等）
├── analysis.md                     # 深度分析 + 已问问题
├── outline-strategy-a.md           # 策略 A：故事驱动
├── outline-strategy-b.md           # 策略 B：信息密集
├── outline-strategy-c.md           # 策略 C：视觉优先
├── outline.md                      # 最终选定/合并的大纲
├── prompts/
│   ├── 01-cover-[slug].md
│   ├── 02-content-[slug].md
│   └── ...
├── 01-cover-[slug].png
├── 02-content-[slug].png
└── NN-ending-[slug].png
```

**Slug 生成**：
1. 从内容提取主题（2–4 个词，kebab-case）
2. 示例：「AI工具推荐」→ `ai-tools-recommend`

**冲突处理**：
若 `xhs-images/{topic-slug}/` 已存在：
- 追加时间戳：`{topic-slug}-YYYYMMDD-HHMMSS`
- 示例：已有 `ai-tools` → `ai-tools-20260118-143052`

**源文件**：
所有源文件复制为 `source-{slug}.{ext}`：
- `source-article.md`、`source-photo.jpg` 等
- 支持多源：正文、图片、对话中的文件

## 工作流

### 进度清单

复制并跟踪进度：

```
XHS Infographic Progress:
- [ ] Step 0: Check preferences (EXTEND.md) ⛔ BLOCKING
  - [ ] Found → load preferences → continue
  - [ ] Not found → run first-time setup → MUST complete before Step 1
- [ ] Step 1: Analyze content → analysis.md
- [ ] Step 2: Smart Confirm ⚠️ REQUIRED
  - [ ] Path A: Quick confirm → generate recommended outline
  - [ ] Path B: Customize → adjust then generate outline
  - [ ] Path C: Detailed → 3 outlines → second confirm → generate outline
- [ ] Step 3: Generate images (sequential)
- [ ] Step 4: Completion report
```

### 流程

```
Input → [Step 0: Preferences] ─┬─ Found → Continue
                               │
                               └─ Not found → First-Time Setup ⛔ BLOCKING
                                              │
                                              └─ Complete setup → Save EXTEND.md → Continue
                                                                                      │
        ┌───────────────────────────────────────────────────────────────────────────┘
        ↓
Analyze → [Smart Confirm] ─┬─ Quick: confirm recommended → outline.md → Generate → Complete
                           │
                           ├─ Customize: adjust options → outline.md → Generate → Complete
                           │
                           └─ Detailed: 3 outlines → [Confirm 2] → outline.md → Generate → Complete
```

### Step 0：加载偏好（EXTEND.md）⛔ 阻塞

**目的**：加载用户偏好或执行首次设置。

**关键**：若未找到 EXTEND.md，必须先完成首次设置，再进行任何其他问题或步骤。不要进入内容分析，不要问风格，不要问版式——**只**先完成偏好设置。

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-xhs-images/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-xhs-images/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-xhs-images/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-xhs-images/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────┬───────────────────┐
│                        路径                        │       位置        │
├────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-xhs-images/EXTEND.md           │ 项目目录          │
├────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-xhs-images/EXTEND.md     │ 用户主目录        │
└────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────┐
│   结果    │                                              操作                                                │
├───────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Found     │ 读取、解析、展示摘要 → 进入 Step 1                                                                 │
├───────────┼─────────────────────────────────────────────────────────────────────────────────────────────────────┤
│ Not found │ ⛔ 阻塞：仅运行首次设置（见下文）→ 完成并保存 EXTEND.md → 再 Step 1                              │
└───────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────┘

**首次设置**（未找到 EXTEND.md 时）：

**语言**：使用用户输入语言或已保存的语言偏好。

在**一次** `AskUserQuestion` 中问完**所有**问题。问题细节见 `references/config/first-time-setup.md`。

**EXTEND.md 支持**：水印 | 首选风格/版式 | 自定义风格定义 | 语言偏好

Schema：`references/config/preferences-schema.md`

### Step 1：分析内容 → `analysis.md`

读取源内容，必要时保存，并做深度分析。

**操作**：
1. **保存源内容**（若尚非文件）：
   - 用户提供文件路径：直接使用
   - 用户粘贴内容：保存到目标目录下的 `source.md`
   - **备份规则**：若已存在 `source.md`，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 读取源内容
3. **深度分析**（遵循 `references/workflows/analysis-framework.md`）：
   - 内容类型（种草/干货/测评/教程/避坑…）
   - Hook 分析（爆款标题潜力）
   - 目标受众
   - 互动潜力（收藏/分享/评论）
   - 视觉机会映射
   - 划动流设计
4. 检测源语言
5. 推荐张数（2–10）
6. 根据内容信号**自动推荐**最佳策略 + 风格 + 版式
7. **保存到 `analysis.md`**

### Step 2：智能确认 ⚠️

**目的**：展示自动推荐方案，由用户确认或调整。**不要跳过。**

**自动推荐逻辑**：
1. 用「自动匹配」表匹配内容信号 → 最佳策略 + 风格 + 版式
2. 根据内容密度推断最佳张数
3. 从预设加载该风格的默认元素

**展示**（分析摘要 + 推荐方案）：

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 内容分析
  主题：[topic] | 类型：[content_type]
  要点：[key points summary]
  受众：[target audience]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎨 推荐方案（自动匹配）
  策略：[A/B/C] [strategy name]（[reason]）
  风格：[style] · 布局：[layout] · 预设：[preset]
  图片：[N]张（封面+[N-2]内容+结尾）
  元素：[background] / [decorations] / [emphasis]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**使用 AskUserQuestion**，单一问题：

| 选项 | 说明 |
|------|------|
| 1. ✅ 确认，直接生成（推荐） | 信任自动推荐，立即继续 |
| 2. 🎛️ 自定义调整 | 一步内调整策略/风格/版式/张数 |
| 3. 📋 详细模式 | 生成 3 份大纲再选（两次确认） |

#### 路径 A：快速确认（选项 1）

用推荐策略 + 风格生成单份大纲 → 保存 `outline.md` → Step 3。

#### 路径 B：自定义（选项 2）

**使用 AskUserQuestion**，可调选项（留空 = 保留推荐）：

1. **策略风格**：当前：[strategy + style]。可选：A Story-Driven(warm) | B Information-Dense(notion) | C Visual-First(screen-print)。或直接指定风格：cute/fresh/warm/bold/minimal/retro/pop/notion/chalkboard/study-notes/screen-print。或使用预设：knowledge-card / checklist / tutorial / poster / cinematic / 等。
2. **版式**：当前：[layout]。可选：sparse | balanced | dense | list | comparison | flow | mindmap | quadrant
3. **张数**：当前：[N]。范围：2–10
4. **补充说明**（可选）：卖点侧重、受众调整、色彩偏好等

**用户回答后**：按用户选择生成单份大纲 → 保存 `outline.md` → Step 3。

#### 路径 C：详细模式（选项 3）

两次确认的完整流程，控制度最高：

**Step 2a：内容理解**

**使用 AskUserQuestion** 询问：
1. 核心卖点（multiSelect: true）
2. 目标受众
3. 风格偏好：真实分享 / 专业测评 / 氛围美感 / 自动
4. 补充说明（可选）

**回答后**：更新 `analysis.md`。

**Step 2b：生成 3 份大纲变体**

| 策略 | 文件名 | 大纲特点 | 推荐风格 |
|------|--------|----------|----------|
| A | `outline-strategy-a.md` | 故事向：情绪、前后对比 | warm, cute, fresh |
| B | `outline-strategy-b.md` | 信息向：结构化、事实 | notion, minimal, chalkboard |
| C | `outline-strategy-c.md` | 视觉向：氛围、少字 | bold, pop, retro, screen-print |

**大纲格式**（YAML front matter + 正文）：
```yaml
---
strategy: a  # a, b, or c
name: Story-Driven
style: warm  # recommended style for this strategy
style_reason: "Warm tones enhance emotional storytelling and personal connection"
elements:  # from style preset, can be customized
  background: solid-pastel
  decorations: [clouds, stars-sparkles]
  emphasis: star-burst
  typography: highlight
layout: balanced  # primary layout
image_count: 5
---

## P1 Cover
**Type**: cover
**Hook**: "入冬后脸不干了🥹终于找到对的面霜"
**Visual**: Product hero shot with cozy winter atmosphere
**Layout**: sparse

## P2 Problem
**Type**: pain-point
**Message**: Previous struggles with dry skin
**Visual**: Before state, relatable scenario
**Layout**: balanced

...
```

**差异化要求**：
- 每个策略的大纲结构**与**推荐风格必须不同
- 页数灵活：A 通常 4–6，B 通常 3–5，C 通常 3–4
- 含 `style_reason`，说明该风格为何适合该策略

参考：`references/workflows/outline-template.md`

**Step 2c：大纲与风格选择**

**使用 AskUserQuestion**，三个问题：

**Q1：大纲策略**：A / B / C / Combine（注明各策略取哪些页）

**Q2：视觉风格**：采用推荐 | 选预设 | 选风格 | 自定义描述

**Q3：视觉元素**：使用默认（推荐） | 调整背景 | 调整装饰 | 自定义

**回答后**：将选定/合并后的大纲保存到 `outline.md`，并确认风格与元素 → Step 3。

### Step 3：生成图片

在已确认大纲 + 风格 + 版式的前提下：

**视觉一致性 — 参考图链**：
为保证系列内角色/风格一致：
1. **先生成第 1 张（封面）** — 不带 `--ref`
2. **第 1 张作为后续所有图的 `--ref`**（2、3、…、N）
   - 锚定角色造型、上色与插画风格
   - 命令模式：每次后续生成附加 `--ref <path-to-image-01.png>`

对含固定角色、吉祥物或插画元素的风格尤为关键。第 1 张即全系列视觉锚点。

**每张图（封面 + 中间内容 + 结尾）**：
1. 将 prompt 保存到 `prompts/NN-{type}-[slug].md`（用户偏好语言）
   - **备份规则**：若 prompt 文件已存在，重命名为 `prompts/NN-{type}-[slug]-backup-YYYYMMDD-HHMMSS.md`
2. 出图：
   - **第 1 张**：不带 `--ref`（建立视觉锚点）
   - **第 2 张及以后**：带 `--ref <image-01-path>` 以保持一致
   - **备份规则**：若图片文件已存在，重命名为 `NN-{type}-[slug]-backup-YYYYMMDD-HHMMSS.png`
3. 每生成一张后汇报进度

**水印**（偏好中启用时）：
在每张图的生成 prompt 中加入：
```
Include a subtle watermark "[content]" positioned at [position].
The watermark should be legible but not distracting from the main content.
```
参考：`references/config/watermark-guide.md`

**出图 Skill 选择**：
- 检查可用的图片生成 skill
- 若有多个，询问用户偏好

**会话管理**：
若出图 skill 支持 `--sessionId`：
1. 生成唯一 session ID：`xhs-{topic-slug}-{timestamp}`
2. 所有图片使用同一 session ID
3. 与参考图链配合，最大化视觉一致性

### Step 4：完成报告

```
Xiaohongshu Infographic Series Complete!

Topic: [topic]
Mode: [Quick / Custom / Detailed]
Strategy: [A/B/C/Combined]
Style: [style name]
Layout: [layout name or "varies"]
Location: [directory path]
Images: N total

✓ analysis.md
✓ outline.md
✓ outline-strategy-a/b/c.md (detailed mode only)

Files:
- 01-cover-[slug].png ✓ Cover (sparse)
- 02-content-[slug].png ✓ Content (balanced)
- 03-content-[slug].png ✓ Content (dense)
- 04-ending-[slug].png ✓ Ending (sparse)
```

## 改图

| 操作 | 步骤 |
|------|------|
| **Edit** | **先更新 prompt 文件** → 用相同 session ID 重新生成 |
| **Add** | 指定插入位置 → 写 prompt → 生成 → 后续文件序号 +1 → 更新大纲 |
| **Delete** | 删文件 → 后续序号 −1 → 更新大纲 |

**重要**：更新图片时，**务必先**更新 prompt 文件（`prompts/NN-{type}-[slug].md`）再重新生成，以便变更可记录、可复现。

## 内容拆分原则

1. **封面（第 1 张）**：钩子 + 视觉冲击 → `sparse` 版式
2. **中间内容**：每张一个核心价值 → `balanced`/`dense`/`list`/`comparison`/`flow`
3. **结尾（最后一张）**：CTA / 总结 → `sparse` 或 `balanced`

**风格 × 版式矩阵**（✓✓ = 强烈推荐，✓ = 适用）：

| | sparse | balanced | dense | list | comparison | flow | mindmap | quadrant |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| cute | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ |
| fresh | ✓✓ | ✓✓ | ✓ | ✓ | ✓ | ✓✓ | ✓ | ✓ |
| warm | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ |
| bold | ✓✓ | ✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ |
| minimal | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| retro | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓ | ✓ | ✓ | ✓ |
| pop | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓ |
| notion | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓✓ |
| chalkboard | ✓✓ | ✓✓ | ✓✓ | ✓✓ | ✓ | ✓✓ | ✓✓ | ✓ |
| study-notes | ✗ | ✓ | ✓✓ | ✓✓ | ✓ | ✓ | ✓✓ | ✓ |
| screen-print | ✓✓ | ✓✓ | ✗ | ✓ | ✓✓ | ✓ | ✗ | ✓✓ |

## 参考

`references/` 下的详细模板：

**Elements**（视觉积木）：
- `elements/canvas.md` - 画幅比例、安全区、网格
- `elements/image-effects.md` - 抠图、描边、滤镜
- `elements/typography.md` - 花字、标签、字向
- `elements/decorations.md` - 强调符号、背景、涂鸦、框线

**Presets**（风格预设）：
- `presets/<name>.md` - 元素组合定义（cute、notion、warm…）
- `style-presets.md` - 预设快捷（风格 + 版式组合）

**Workflows**（流程指南）：
- `workflows/analysis-framework.md` - 内容分析框架
- `workflows/outline-template.md` - 大纲模板与版式指引
- `workflows/prompt-assembly.md` - Prompt 组装指南

**Config**（设置）：
- `config/preferences-schema.md` - EXTEND.md schema
- `config/first-time-setup.md` - 首次设置流程
- `config/watermark-guide.md` - 水印配置

## 说明

- 失败时自动重试一次 | 敏感人物可用卡通替代
- 使用已确认的语言偏好 | 保持风格一致
- **必须做智能确认**（Step 2）— 不可跳过；详细模式含两次子确认

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见 **Step 0**。
