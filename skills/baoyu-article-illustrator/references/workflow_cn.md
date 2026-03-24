# 详细工作流程

## 步骤 1：预检

### 1.0 检测并保存参考图片 ⚠️ 如果提供了图片则为必需

检查用户是否提供了参考图片。根据输入类型处理：

| 输入类型 | 操作 |
|----------|------|
| 提供了图片文件路径 | 复制到 `references/` 子目录 → 可使用 `--ref` |
| 对话中有图片（无路径） | **用 AskUserQuestion 询问用户文件路径** |
| 用户无法提供路径 | 口头提取风格/配色 → 追加到提示词（frontmatter 中不添加 references） |

**关键**：仅当文件确实已保存到 `references/` 目录时，才在提示词 frontmatter 中添加 `references`。

**如果用户提供了文件路径**：
1. 复制到 `references/NN-ref-{slug}.png`
2. 创建描述文件：`references/NN-ref-{slug}.md`
3. 确认文件存在后再继续

**如果用户无法提供路径**（口头提取）：
1. 视觉分析图片，提取：颜色、风格、构图
2. 创建 `references/extracted-style.md` 保存提取的信息
3. 不要在提示词 frontmatter 中添加 `references`
4. 改为将提取的风格/颜色直接追加到提示词文本

**描述文件格式**（仅当文件已保存时）：
```yaml
---
ref_id: NN
filename: NN-ref-{slug}.png
---
[用户的描述或自动生成的描述]
```

**验证**（仅针对已保存的文件）：
```
已保存的参考图片：
- 01-ref-{slug}.png ✓（可使用 --ref）
- 02-ref-{slug}.png ✓（可使用 --ref）
```

**或针对提取的风格**：
```
已提取参考风格（无文件）：
- 颜色：#E8756D 珊瑚色、#7ECFC0 薄荷色...
- 风格：极简扁平矢量、简洁线条...
→ 将追加到提示词文本（非 --ref）
```

---

### 1.1 确定输入类型

| 输入 | 输出目录 | 下一步 |
|------|----------|--------|
| 文件路径 | EXTEND.md 的 `default_output_dir`（默认：`imgs-subdir`）。若未配置，在 1.2 中确认。 | → 1.2 |
| 粘贴内容 | `illustrations/{topic-slug}/` | → 1.4 |

**粘贴内容备份规则**：如果目标目录中已存在 `source.md`，在保存前重命名为 `source-backup-YYYYMMDD-HHMMSS.md`。

### 1.2-1.4 配置（仅文件路径输入）

检查偏好设置和现有状态，然后在一次 AskUserQuestion 调用中询问所有需要的问题（最多 4 个问题）。

**需要包含的问题**（如果偏好设置已存在或不适用则跳过）：

| 问题 | 何时询问 | 选项 |
|------|----------|------|
| 输出目录 | EXTEND.md 中无 `default_output_dir` | `{article-dir}/imgs/`（推荐）、`{article-dir}/`、`{article-dir}/illustrations/`、`illustrations/{topic-slug}/` |
| 现有图片 | 目标目录有 `.png/.jpg/.webp` 文件 | `supplement`（补充）、`overwrite`（覆盖）、`regenerate`（重新生成） |
| 文章更新 | 始终（文件路径输入） | `update`（更新）、`copy`（复制） |

**偏好设置值**（如已配置则跳过询问）：

| `default_output_dir` | 路径 |
|----------------------|------|
| `same-dir` | `{article-dir}/` |
| `imgs-subdir` | `{article-dir}/imgs/` |
| `illustrations-subdir` | `{article-dir}/illustrations/` |
| `independent` | `illustrations/{topic-slug}/` |

### 1.5 加载偏好设置（EXTEND.md）⛔ 阻塞操作

**关键**：如果未找到 EXTEND.md，必须在任何其他问题或步骤之前完成首次设置。不要继续处理参考图片，不要询问内容，不要询问类型/风格 — 仅先完成偏好设置。

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-article-illustrator/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-article-illustrator/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-article-illustrator/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|------|------|
| 找到 | 读取、解析、显示摘要 → 继续 |
| 未找到 | ⛔ **阻塞**：仅运行首次设置（[config/first-time-setup.md](config/first-time-setup.md)）→ 完成并保存 EXTEND.md → 然后继续 |

**支持**：水印 | 偏好类型/风格 | 自定义风格 | 语言 | 输出目录

---

## 步骤 2：设置与分析

### 2.1 分析内容

| 分析项 | 描述 |
|--------|------|
| 内容类型 | 技术 / 教程 / 方法论 / 叙事 |
| 插图目的 | 信息传达 / 可视化 / 想象 |
| 核心论点 | 2-5 个需要可视化的主要观点 |
| 视觉机会 | 插图能增加价值的位置 |
| 推荐类型 | 基于内容信号和目的 |
| 推荐密度 | 基于长度和复杂度 |

### 2.2 提取核心论点

- 主要论题
- 读者需要理解的关键概念
- 对比/对照
- 提出的框架/模型

**关键**：如果文章使用隐喻（如"电锯切西瓜"），不要按字面意思配图。要可视化其**背后的概念**。

### 2.3 识别位置

**应配图的**：
- 核心论点（必需）
- 抽象概念
- 数据对比
- 流程、工作流

**不应配图的**：
- 字面隐喻
- 装饰性场景
- 通用插图

### 2.4 分析参考图片（如果在步骤 1.0 中提供了）

对每张参考图片：

| 分析项 | 描述 |
|--------|------|
| 视觉特征 | 风格、颜色、构图 |
| 内容/主题 | 参考素材描绘的内容 |
| 适用位置 | 哪些章节与此参考匹配 |
| 风格匹配 | 哪些插图类型/风格吻合 |
| 使用建议 | `direct` / `style` / `palette` |

| 用途 | 何时使用 |
|------|----------|
| `direct` | 参考素材与期望输出高度吻合 |
| `style` | 仅提取视觉风格特征 |
| `palette` | 仅提取配色方案 |

---

## 步骤 3：确认设置 ⚠️

**不要跳过。** 使用一次 AskUserQuestion 调用，最多 4 个问题。**Q1、Q2、Q3 全部必选。**

### Q1：预设或类型 ⚠️ 必选

基于步骤 2 的内容分析，优先推荐一个预设（同时设定类型和风格）。查找 [style-presets.md](style-presets.md) 中的"内容类型 → 预设推荐"表。

- [推荐预设] — [简述：类型+风格+原因]（推荐）
- [备选预设] — [简述]
- 或手动选择类型：infographic / scene / flowchart / comparison / framework / timeline / mixed

**如果用户选择了预设 → 跳过 Q3**（类型和风格均已确定）。
**如果用户选择了类型 → Q3 为必选。**

### Q2：密度 ⚠️ 必选 - 不要跳过
- minimal（1-2 张）- 仅核心概念
- balanced（3-5 张）- 主要章节
- per-section - 每节至少 1 张（推荐）
- rich（6 张以上）- 全面覆盖

### Q3：风格 ⚠️ 必选（如果在 Q1 中选择了预设则跳过）

如果 EXTEND.md 有 `preferred_style`：
- [自定义风格名称 + 简要描述]（推荐）
- [最佳兼容核心风格 1]
- [最佳兼容核心风格 2]
- 其他（查看完整风格画廊）

如果没有 `preferred_style`（优先展示核心风格）：
- [最佳兼容核心风格]（推荐）
- [其他兼容核心风格 1]
- [其他兼容核心风格 2]
- 其他（查看完整风格画廊）

**核心风格**（简化选择）：

| 核心风格 | 对应风格 | 最适用于 |
|----------|----------|----------|
| `minimal-flat` | notion | 通用、知识分享、SaaS |
| `sci-fi` | blueprint | AI、前沿科技、系统设计 |
| `hand-drawn` | sketch/warm | 轻松、反思、休闲 |
| `editorial` | editorial | 流程、数据、新闻 |
| `scene` | warm/watercolor | 叙事、情感、生活方式 |
| `poster` | screen-print | 观点、评论、文化、电影感 |

风格选择基于类型×风格兼容性矩阵（styles.md）。
完整规格：`styles/<style>.md`

### Q4：图片文字语言 ⚠️ 当文章语言 ≠ EXTEND.md `language` 时必选

从内容中检测文章语言。如果与 EXTEND.md `language` 设置不同，必须询问：
- 文章语言（与文章内容一致）（推荐）
- EXTEND.md 语言（用户的通用偏好）

**仅在以下情况跳过**：文章语言与 EXTEND.md `language` 一致，或 EXTEND.md 未设置 `language`。

### 显示参考素材用途（如果在步骤 1.0 中检测到参考素材）

向用户展示大纲预览时，显示参考素材分配：

```
参考图片：
| 编号 | 文件名 | 推荐用途 |
|------|--------|----------|
| 01 | 01-ref-diagram.png | direct → 插图 1、3 |
| 02 | 02-ref-chart.png | palette → 插图 2 |
```

---

## 步骤 4：生成大纲

保存为 `{output-dir}/outline.md`（以下所有路径相对于步骤 1.1/1.2 确定的输出目录）：

```yaml
---
type: infographic
density: balanced
style: blueprint
image_count: 4
references:                    # 仅在提供了参考素材时
  - ref_id: 01
    filename: 01-ref-diagram.png
    description: "展示系统架构的技术图表"
  - ref_id: 02
    filename: 02-ref-chart.png
    description: "品牌配色方案图表"
---

## Illustration 1

**Position**: [章节] / [段落]
**Purpose**: [为什么这有帮助]
**Visual Content**: [展示什么]
**Type Application**: [类型如何应用]
**References**: [01]                    # 可选：列出使用的 ref_id
**Reference Usage**: direct             # direct | style | palette
**Filename**: 01-infographic-concept-name.png

## Illustration 2
...
```

**要求**：
- 每个位置都有内容需求支撑
- 类型一致应用
- 描述中体现风格
- 数量与密度匹配
- 参考素材基于步骤 2.4 的分析进行分配

---

## 步骤 5：生成图片

### 5.1 创建提示词 ⛔ 阻塞操作

**每张插图在生成开始前都必须有保存的提示词文件。不要跳过此步骤。**

对大纲中的每张插图：

1. **创建提示词文件**：`{output-dir}/prompts/NN-{type}-{slug}.md`
2. **包含 YAML frontmatter**：
   ```yaml
   ---
   illustration_id: 01
   type: infographic
   style: custom-flat-vector
   ---
   ```
3. **遵循类型专用模板**，来自 [prompt-construction.md](prompt-construction.md)
4. **提示词质量要求**（全部必需）：
   - `Layout`：描述整体构图（grid / radial / hierarchical / left-right / top-down）
   - `ZONES`：描述每个视觉区域的具体内容，而非模糊描述
   - `LABELS`：使用**文章中的实际数字、术语、指标、引用** — 不是通用占位符
   - `COLORS`：指定带语义含义的十六进制色值（例如：`珊瑚色 (#E07A5F) 用于强调`）
   - `STYLE`：描述线条处理、纹理、情绪、人物渲染
   - `ASPECT`：指定比例（例如：`16:9`）
5. **应用默认值**：构图要求、人物渲染、文字准则、水印
6. **备份规则**：如果提示词文件已存在，重命名为 `prompts/NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.md`

**验证** ⛔：在进入 5.2 之前，确认所有提示词文件存在：
```
提示词文件：
- prompts/01-infographic-overview.md ✓
- prompts/02-infographic-distillation.md ✓
...
```

**不要**在未先保存提示词文件的情况下将临时内联文本传递给 `--prompt`。生成命令应使用 `--promptfiles prompts/NN-{type}-{slug}.md` 或读取已保存文件的内容用于 `--prompt`。

**执行选择**：
- 如果多张插图已有保存的提示词文件且任务仅为简单生成，优先使用 `baoyu-image-gen` 批量模式（`build-batch.ts` -> `main.ts --batchfile`）
- 仅在每张插图仍需单独的提示词重写、风格探索或其他逐图推理时才使用子代理

**关键 - Frontmatter 中的 References**：
- 仅当文件确实存在于 `references/` 目录时才添加 `references` 字段
- 如果风格/配色是口头提取的（无文件），将信息追加到提示词正文
- 写入 frontmatter 前验证：`test -f references/NN-ref-{slug}.png`

### 5.2 选择生成技能

检查可用技能。如有多个，询问用户。

### 5.3 处理参考素材 ⚠️ 如果在步骤 1.0 中保存了参考素材则为必需

**如果用户提供了参考图片，不要跳过。** 对每张带参考素材的插图：

1. **先验证文件存在**：
   ```bash
   test -f references/NN-ref-{slug}.png && echo "exists" || echo "MISSING"
   ```
   - 如果文件缺失但在 frontmatter 中 → 错误，修复 frontmatter 或移除 references 字段
   - 如果文件存在 → 继续处理

2. 读取提示词 frontmatter 获取参考信息
3. 按用途类型处理：

| 用途 | 操作 | 示例 |
|------|------|------|
| `direct` | 将参考路径添加到 `--ref` 参数 | `--ref references/01-ref-brand.png` |
| `style` | 分析参考素材，将风格特征追加到提示词 | "风格：简洁线条、渐变背景..." |
| `palette` | 从参考素材提取颜色，追加到提示词 | "颜色：#E8756D 珊瑚色、#7ECFC0 薄荷色..." |

4. 检查图片生成技能的能力：

| 技能是否支持 `--ref` | 操作 |
|----------------------|------|
| 是（例如使用 Google 的 baoyu-image-gen） | 通过 `--ref` 传递参考图片 |
| 否 | 转换为文字描述，追加到提示词 |

**验证**：生成前确认参考素材处理情况：
```
参考素材处理：
- 插图 1：使用 01-ref-brand.png（direct）✓
- 插图 2：从 02-ref-style.png 提取配色 ✓
```

### 5.4 应用水印（如启用）

添加：`Include a subtle watermark "[content]" at [position].`

### 5.5 生成

1. 对每张插图：
   - **备份规则**：如果图片文件已存在，重命名为 `NN-{type}-{slug}-backup-YYYYMMDD-HHMMSS.md`
   - 如果参考素材为 `direct` 用途：包含 `--ref` 参数
   - 生成图片
2. 每次生成后显示："已生成 X/N"
3. 失败时：重试一次，然后记录日志并继续

---

## 步骤 6：收尾

### 6.1 更新文章

在对应段落后插入，使用相对于文章文件的路径：

| `default_output_dir` | 插入路径 |
|----------------------|----------|
| `imgs-subdir` | `![描述](imgs/NN-{type}-{slug}.png)` |
| `same-dir` | `![描述](NN-{type}-{slug}.png)` |
| `illustrations-subdir` | `![描述](illustrations/NN-{type}-{slug}.png)` |
| `independent` | `![描述](illustrations/{topic-slug}/NN-{type}-{slug}.png)`（相对于 cwd） |

Alt 文字：使用文章语言的简洁描述。

### 6.2 输出摘要

```
文章配图完成！

文章：[路径]
类型：[type] | 密度：[level] | 风格：[style]
位置：[目录]
图片：X/N 张已生成

位置：
- 01-xxx.png → 在"[章节]"之后
- 02-yyy.png → 在"[章节]"之后

[如有失败]
失败：
- NN-zzz.png：[原因]
```
