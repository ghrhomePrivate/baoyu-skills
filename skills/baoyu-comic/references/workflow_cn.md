# 完整工作流程

生成知识漫画的完整工作流程。

## 进度清单

复制并追踪进度：

```
漫画进度：
- [ ] 步骤 1：设置与分析
  - [ ] 1.1 加载偏好设置
  - [ ] 1.2 分析内容
  - [ ] 1.3 检查已有内容 ⚠️ 必须执行
- [ ] 步骤 2：确认 1 - 风格与选项 ⚠️ 必须执行
- [ ] 步骤 3：生成分镜 + 角色
- [ ] 步骤 4：审阅大纲（有条件）
- [ ] 步骤 5：生成提示词
- [ ] 步骤 6：审阅提示词（有条件）
- [ ] 步骤 7：生成图片
- [ ] 步骤 8：合并为 PDF
- [ ] 步骤 9：完成报告
```

## 流程图

```
输入 → 偏好设置 → 分析 → [检查已有内容?] → [确认 1: 风格 + 审阅] → 分镜 → [审阅大纲?] → 提示词 → [审阅提示词?] → 图片 → PDF → 完成
```

---

## 步骤 1：设置与分析

### 1.1 加载偏好设置（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-comic/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-comic/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-comic/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-comic/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-comic/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-comic/EXTEND.md") { "user" }
```

| 路径 | 位置 |
|------|------|
| `.baoyu-skills/baoyu-comic/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-comic/EXTEND.md` | 用户主目录 |

**找到 EXTEND.md 时** → 读取、解析，**向用户输出摘要**：

```
📋 已从 [完整路径] 加载偏好设置
├─ 水印：[启用/禁用] [内容（如已启用）]
├─ 画风：[风格名称或"自动选择"]
├─ 基调：[基调名称或"自动选择"]
├─ 布局：[布局或"自动选择"]
├─ 语言：[语言或"自动检测"]
└─ 角色预设：[数量] 个已定义
```

**必须输出此摘要**，让用户了解当前配置。不要跳过或静默加载。

**未找到 EXTEND.md 时** → 首次设置：

1. 通知用户："未找到偏好设置。让我们设置你的默认值。"
2. 使用 AskUserQuestion 收集偏好（参见 `config/first-time-setup.md`）
3. 在用户选择的位置创建 EXTEND.md
4. 确认："✓ 偏好设置已保存到 [路径]"

**EXTEND.md 支持**：水印 | 首选画风/基调/布局 | 自定义风格定义 | 角色预设 | 语言偏好

Schema：`config/preferences-schema.md`

**重要**：一旦 EXTEND.md 存在，水印、语言和风格默认值在确认 1 或 2 中不再询问。这些是会话持久设置。

### 1.2 分析内容 → `analysis.md`

读取源内容，必要时保存，并进行深度分析。

**操作**：
1. **保存源内容**（如果还不是文件）：
   - 如果用户提供了文件路径：直接使用
   - 如果用户粘贴了内容：保存到目标目录的 `source.md`
   - **备份规则**：如果 `source.md` 已存在，重命名为 `source-backup-YYYYMMDD-HHMMSS.md`
2. 读取源内容
3. **深度分析**，遵循 `analysis-framework.md`：
   - 目标受众识别
   - 对读者的价值主张
   - 核心主题和叙事潜力
   - 关键人物及其故事弧
4. 检测源语言
5. **确定语言**：
   - 如果 EXTEND.md 有 `language` → 使用它
   - 否则如果提供了 `--lang` 选项 → 使用它
   - 否则 → 使用检测到的源语言
6. 确定推荐页数：
   - 短故事：5-8 页
   - 中等复杂度：9-15 页
   - 完整传记：16-25 页
7. 分析内容信号以推荐画风/基调/布局
8. **保存到 `analysis.md`**

**analysis.md 格式**：YAML front matter（title、topic、time_span、source_language、user_language、aspect_ratio、recommended_page_count、recommended_art、recommended_tone）+ 目标受众、价值主张、核心主题、关键人物与故事弧、内容信号、推荐方案各部分。完整模板参见 `analysis-framework.md`。

### 1.3 检查已有内容 ⚠️ 必须执行

**在进入步骤 2 之前必须执行。**

使用 Bash 检查输出目录是否存在：

```bash
test -d "comic/{topic-slug}" && echo "exists"
```

**如果目录存在**，使用 AskUserQuestion：

```
header: "已有内容"
question: "发现已有内容。如何处理？"
options:
  - label: "重新生成分镜"
    description: "保留图片，仅重新生成分镜和角色"
  - label: "重新生成图片"
    description: "保留分镜，仅重新生成图片"
  - label: "备份并重新生成"
    description: "备份到 {slug}-backup-{timestamp}，然后全部重新生成"
  - label: "退出"
    description: "取消，保持已有内容不变"
```

保存结果并相应处理：
- **重新生成分镜**：跳到步骤 3，保留 `prompts/` 和图片
- **重新生成图片**：跳到步骤 7，使用已有提示词
- **备份并重新生成**：移动目录，从步骤 2 重新开始
- **退出**：立即结束工作流程

---

## 步骤 2：确认 1 - 风格与选项 ⚠️

**目的**：选择视觉风格 + 决定是否在生成前审阅大纲。**不要跳过。**

**注意**：水印和语言已在 EXTEND.md 中配置（步骤 1）。

**显示摘要**：
- 识别的内容类型 + 主题
- 提取的关键人物
- 检测的时间跨度
- 推荐页数
- 语言：[来自 EXTEND.md 或检测结果]
- **推荐风格**：[画风] + [基调]（基于内容信号）

**使用 AskUserQuestion** 询问：

### 问题 1：视觉风格

如果推荐了预设（参见 `auto-selection.md`），优先显示：

```
header: "风格"
question: "这部漫画使用哪种视觉风格？"
options:
  - label: "[预设名称] 预设（推荐）"       # 如果推荐了预设
    description: "[预设描述] - 包含特殊规则"
  - label: "[推荐画风] + [推荐基调]（推荐）"  # 如果没有推荐预设
    description: "根据分析结果最匹配你的内容"
  - label: "ligne-claire + neutral"
    description: "经典教育风格，Logicomix 风格"
  - label: "ohmsha 预设"
    description: "教育漫画风，视觉隐喻，道具，禁止对话头像"
  - label: "自定义"
    description: "指定你自己的画风 + 基调或预设"
```

**预设 vs 画风+基调**：预设包含超出画风+基调的特殊规则。`ohmsha` = manga + neutral + 视觉隐喻规则 + 角色设定 + 禁止对话头像。单纯的 `manga + neutral` 不包含这些规则。

### 问题 2：叙事重点（multiSelect: true）

```
header: "重点"
question: "漫画应该侧重什么？（可多选）"
options:
  - label: "传记/人生故事"
    description: "追踪一个人的关键人生事件"
  - label: "概念解释"
    description: "以视觉方式拆解复杂概念"
  - label: "历史事件"
    description: "戏剧化呈现重要历史时刻"
  - label: "教程/操作指南"
    description: "分步骤的教育指南"
```

### 问题 3：目标受众

```
header: "受众"
question: "主要读者是谁？"
options:
  - label: "普通读者"
    description: "广泛吸引力，通俗内容"
  - label: "学生/学习者"
    description: "教育导向，清晰讲解"
  - label: "行业专业人士"
    description: "技术深度，领域知识"
  - label: "儿童/青少年读者"
    description: "简化语言，吸引人的视觉"
```

### 问题 4：大纲审阅

```
header: "审阅"
question: "是否要在图片生成前审阅大纲？"
options:
  - label: "是，让我审阅（推荐）"
    description: "在生成图片前审阅分镜和角色"
  - label: "否，直接生成"
    description: "跳过大纲审阅，立即开始生成"
```

### 问题 5：提示词审阅

```
header: "提示词"
question: "在生成图片前审阅提示词？"
options:
  - label: "是，审阅提示词（推荐）"
    description: "在生成前审阅图片生成提示词"
  - label: "否，跳过提示词审阅"
    description: "直接进入图片生成"
```

**回答后**：
1. 用用户偏好更新 `analysis.md`
2. **根据问题 4 的回答存储 `skip_outline_review`** 标志
3. **根据问题 5 的回答存储 `skip_prompt_review`** 标志
4. → 步骤 3

---

## 步骤 3：生成分镜 + 角色

使用步骤 2 确认的风格创建分镜和角色定义。

**加载风格参考**：
- 画风：`art-styles/{art}.md`
- 基调：`tones/{tone}.md`
- 如果是预设（ohmsha/wuxia/shoujo）：同时加载 `presets/{preset}.md`

**生成**：

1. **分镜**（`storyboard.md`）：
   - YAML front matter 包含 art_style、tone、layout、aspect_ratio
   - 封面设计
   - 每一页：布局、分格细分、视觉提示
   - **使用用户首选语言编写**（来自步骤 1）
   - 参考：`storyboard-template.md`
   - **如果使用预设**：加载并应用 `presets/` 中的预设规则

2. **角色定义**（`characters/characters.md`）：
   - 匹配画风的视觉规格（使用用户首选语言）
   - 包含参考表提示词，用于后续图片生成
   - 参考：`character-template.md`
   - **如果使用 ohmsha 预设**：使用默认哆啦A梦角色（见下方）

**Ohmsha 默认角色**（除非用户指定 `--characters`，否则使用这些）：

| 角色 | 名称 | 视觉描述 |
|------|------|----------|
| 学生 | 大雄 (Nobita) | 日本男孩，10岁，圆形眼镜，黑发中分，黄色衬衫，深蓝短裤 |
| 导师 | 哆啦A梦 (Doraemon) | 圆形蓝色机器猫，大白眼，红鼻子，胡须，白肚皮带四次元口袋，金色铃铛，无耳朵 |
| 挑战者 | 胖虎 (Gian) | 壮实男孩，粗犷面容，小眼睛，橙色衬衫 |
| 辅助 | 静香 (Shizuka) | 可爱女孩，黑色短发，粉色裙子，温柔表情 |

这些是标准 ohmsha 风格角色。除非明确要求，否则不要为 ohmsha 创建自定义角色。

**生成后**：
- 如果 `skip_outline_review` 为 true → 跳过步骤 4，直接进入步骤 5
- 如果 `skip_outline_review` 为 false → 继续步骤 4

---

## 步骤 4：审阅大纲（有条件）

**如果用户在步骤 2 选择了"否，直接生成"则跳过此步骤。**

**目的**：用户在生成前审阅并确认分镜 + 角色。

**显示**：
- 页数和结构
- 画风 + 基调组合
- 逐页摘要（封面 → P1 → P2...）
- 角色列表及简要描述

**使用 AskUserQuestion**：

```
header: "确认"
question: "准备好用这个大纲生成图片了吗？"
options:
  - label: "是，继续（推荐）"
    description: "生成角色参考表和漫画页面"
  - label: "先编辑分镜"
    description: "我将在继续前修改 storyboard.md"
  - label: "先编辑角色"
    description: "我将在继续前修改 characters/characters.md"
  - label: "两者都编辑"
    description: "我将在继续前修改两个文件"
```

**回答后**：
1. 如果用户要编辑 → 等待用户完成编辑，然后再次询问
2. 如果用户确认 → 继续步骤 5

---

## 步骤 5：生成提示词

为所有页面创建图片生成提示词。

**风格参考加载**：
- 读取 `art-styles/{art}.md` 获取渲染指南
- 读取 `tones/{tone}.md` 获取情绪/色彩调整
- 如果是预设：读取 `presets/{preset}.md` 获取特殊规则

**对于每一页（封面 + 内页）**：
1. 按照画风 + 基调指南创建提示词
2. 包含角色视觉描述以保持一致性
3. 保存到 `prompts/NN-{cover|page}-[slug].md`
   - **备份规则**：如果提示词文件存在，重命名为 `prompts/NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.md`

**提示词文件格式**：
```markdown
# Page NN: [标题]

## Visual Style
Art: [画风] | Tone: [基调] | Layout: [布局类型]

## Character Reference
[来自 characters/characters.md 的角色描述]

## Panel Breakdown
[来自 storyboard.md - 分格描述、动作、对话]

## Generation Prompt
[用于图片生成技能的合并提示词]
```

**水印应用**（如果偏好设置中已启用）：
在每个提示词中添加：
```
Include a subtle watermark "[content]" positioned at [position]
with approximately [opacity*100]% visibility. The watermark should
be legible but not distracting from the comic panels and storytelling.
Ensure watermark does not overlap speech bubbles or key action.
```
参考：`config/watermark-guide.md`

**生成后**：
- 如果 `skip_prompt_review` 为 true → 跳过步骤 6，直接进入步骤 7
- 如果 `skip_prompt_review` 为 false → 继续步骤 6

---

## 步骤 6：审阅提示词（有条件）

**如果用户在步骤 2 选择了"否，跳过提示词审阅"则跳过此步骤。**

**目的**：用户在图片生成前审阅并确认提示词。

**显示提示词摘要表**：

| 页面 | 标题 | 关键元素 |
|------|------|----------|
| 封面 | [标题] | [主要视觉] |
| P1 | [标题] | [关键元素] |
| ... | ... | ... |

**使用 AskUserQuestion**：

```
header: "确认"
question: "准备好用这些提示词生成图片了吗？"
options:
  - label: "是，继续（推荐）"
    description: "生成所有漫画页面图片"
  - label: "先编辑提示词"
    description: "我将在继续前修改 prompts/*.md"
  - label: "重新生成提示词"
    description: "用不同方式重新生成所有提示词"
```

**回答后**：
1. 如果用户要编辑 → 等待用户完成编辑，然后再次询问
2. 如果用户要重新生成 → 回到步骤 5
3. 如果用户确认 → 继续步骤 7

---

## 步骤 7：生成图片

使用步骤 5/6 确认的提示词：

### 7.1 生成角色参考表（首先）

1. 使用 `characters/characters.md` 中的参考表提示词
2. **备份规则**：如果 `characters/characters.png` 存在，重命名为 `characters/characters-backup-YYYYMMDD-HHMMSS.png`
3. 生成 → `characters/characters.png`
4. 这确保了后续所有页面的视觉一致性

### 7.2 生成漫画页面

**关键：角色参考是必须的**，以确保所有页面的视觉一致性。

**在生成任何页面之前**：
1. 读取图片生成技能的 SKILL.md
2. 检查是否支持参考图输入（`--ref`、`--reference` 等）
3. 选择下方相应的策略

**角色参考策略**：

| 技能能力 | 策略 | 操作 |
|----------|------|------|
| 支持 `--ref` | **策略 A** | 每一页都传入 `characters/characters.png` |
| 不支持 `--ref` | **策略 B** | 在每个提示词前添加角色描述 |

**策略 A：使用 `--ref` 参数**（如 baoyu-image-gen）

- 读取所选图片生成技能的 `SKILL.md`
- 通过其文档化的接口调用该已安装技能，而非直接调用其脚本
- 对每一页，使用 `prompts/01-page-xxx.md` 作为提示词文件输入
- 输出保存到 `01-page-xxx.png`
- 使用宽高比 `3:4`
- 在每次页面生成时传入 `characters/characters.png` 作为 `--ref`

**策略 B：在提示词中嵌入角色描述**

当技能不支持参考图时，创建合并提示词文件：

```markdown
# prompts/01-page-xxx.md（嵌入角色参考）

## Character Reference（保持一致性）
[从 characters/characters.md 复制相关部分到此处]
- 大雄：日本男孩，圆形眼镜，黄色衬衫，深蓝短裤...
- 哆啦A梦：圆形蓝色机器猫，白肚皮，红鼻子，金色铃铛...

## Page Content
[原始页面提示词内容]
```

**对于每一页（封面 + 内页）**：
1. 从 `prompts/NN-{cover|page}-[slug].md` 读取提示词
2. **备份规则**：如果图片文件存在，重命名为 `NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.png`
3. 使用策略 A 或 B 生成图片（基于技能能力）
4. 保存到 `NN-{cover|page}-[slug].png`
5. 每次生成后报告进度："已生成 X/N：[页面标题]"

**会话管理**：
如果图片生成技能支持 `--sessionId`：
1. 生成唯一会话 ID：`comic-{topic-slug}-{timestamp}`
2. 所有页面使用同一会话 ID
3. 确保生成图片间的视觉一致性

---

## 步骤 8：合并为 PDF

所有图片生成后：

```bash
${BUN_X} {baseDir}/scripts/merge-to-pdf.ts <comic-dir>
```

创建 `{topic-slug}.pdf`，所有页面作为全页图片。

---

## 步骤 9：完成报告

```
漫画完成！
标题：[标题] | 画风：[画风] | 基调：[基调] | 页数：[数量] | 宽高比：[比例] | 语言：[语言]
水印：[启用/禁用]
位置：[路径]
✓ analysis.md
✓ characters.png
✓ 00-cover-[slug].png ... NN-page-[slug].png
✓ {topic-slug}.pdf
```

---

## 页面修改

| 操作 | 步骤 |
|------|------|
| **编辑** | 更新提示词 → 重新生成图片 → 重新生成 PDF |
| **添加** | 在指定位置创建提示词 → 生成图片 → 后续编号+1 → 更新分镜 → 重新生成 PDF |
| **删除** | 删除文件 → 后续编号-1 → 更新分镜 → 重新生成 PDF |

**文件命名**：`NN-{cover|page}-[slug].png`（如 `03-page-enigma-machine.png`）
- Slug：kebab-case，唯一，源自内容
- 重新编号：仅更新 NN 前缀，slug 不变
