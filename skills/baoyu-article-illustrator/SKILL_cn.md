---
name: baoyu-article-illustrator
description: 分析文章结构，识别需要视觉辅助的位置，通过类型×风格二维方式生成插图。当用户要求"为文章配图"、"添加图片"、"generate images for article"或"illustrate article"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-article-illustrator
---

# 文章配图工具

分析文章，识别插图位置，以类型×风格的一致性生成图片。

## 两个维度

| 维度 | 控制内容 | 示例 |
|------|----------|------|
| **类型** | 信息结构 | infographic（信息图）、scene（场景）、flowchart（流程图）、comparison（对比图）、framework（框架图）、timeline（时间线） |
| **风格** | 视觉美学 | notion、warm、minimal、blueprint、watercolor、elegant |

自由组合：`--type infographic --style blueprint`

或使用预设：`--preset tech-explainer` → 一个标志包含类型+风格。参见[风格预设](references/style-presets.md)。

## 类型

| 类型 | 最适用于 |
|------|----------|
| `infographic` | 数据、指标、技术类内容 |
| `scene` | 叙事、情感类内容 |
| `flowchart` | 流程、工作流 |
| `comparison` | 并排对比、选项比较 |
| `framework` | 模型、架构 |
| `timeline` | 历史、演进 |

## 风格

参见 [references/styles.md](references/styles.md) 了解核心风格、完整画廊和类型×风格兼容性。

## 工作流程

```
- [ ] 步骤 1：预检（EXTEND.md、参考资料、配置）
- [ ] 步骤 2：分析内容
- [ ] 步骤 3：确认设置（AskUserQuestion）
- [ ] 步骤 4：生成大纲
- [ ] 步骤 5：生成图片
- [ ] 步骤 6：收尾
```

### 步骤 1：预检

**1.5 加载偏好设置（EXTEND.md）⛔ 阻塞操作**

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
| 找到 | 读取、解析、显示摘要 |
| 未找到 | ⛔ 运行[首次设置](references/config/first-time-setup.md) |

完整流程：[references/workflow.md](references/workflow.md#step-1-pre-check)

### 步骤 2：分析

| 分析项 | 输出 |
|--------|------|
| 内容类型 | 技术 / 教程 / 方法论 / 叙事 |
| 目的 | 信息传达 / 可视化 / 想象 |
| 核心论点 | 2-5 个主要观点 |
| 位置 | 插图能增加价值的位置 |

**关键**：隐喻 → 可视化其背后的概念，而非字面意思。

完整流程：[references/workflow.md](references/workflow.md#step-2-setup--analyze)

### 步骤 3：确认设置 ⚠️

**一次 AskUserQuestion 调用，最多 4 个问题。Q1-Q2 必选。除非已选择预设，否则 Q3 也必选。**

| 问题 | 选项 |
|------|------|
| **Q1：预设或类型** | [推荐预设]、[备选预设]，或手动选择：infographic、scene、flowchart、comparison、framework、timeline、mixed |
| **Q2：密度** | minimal（1-2 张）、balanced（3-5 张）、per-section（每节至少 1 张，推荐）、rich（6 张以上） |
| **Q3：风格** | [推荐]、minimal-flat、sci-fi、hand-drawn、editorial、scene、poster、其他 — **若已选预设则跳过** |
| Q4：语言 | 当文章语言 ≠ EXTEND.md 设置时 |

完整流程：[references/workflow.md](references/workflow.md#step-3-confirm-settings-)

### 步骤 4：生成大纲

保存 `outline.md`，包含 frontmatter（type、density、style、image_count）和条目：

```yaml
## Illustration 1
**Position**: [章节/段落]
**Purpose**: [原因]
**Visual Content**: [内容]
**Filename**: 01-infographic-concept-name.png
```

完整模板：[references/workflow.md](references/workflow.md#step-4-generate-outline)

### 步骤 5：生成图片

⛔ **阻塞操作：在任何图片生成之前，必须先保存提示词文件。**

**执行策略**：当多张插图已保存提示词文件且任务仅为简单生成时，优先使用 `baoyu-image-gen` 批量模式（`build-batch.ts` → `--batchfile`），而非启动子代理。仅在每张图片仍需单独的提示词迭代或创意探索时才使用子代理。

1. 为每张插图创建提示词文件，参见 [references/prompt-construction.md](references/prompt-construction.md)
2. 保存至 `prompts/NN-{type}-{slug}.md`，包含 YAML frontmatter
3. 提示词**必须**使用带有结构化段落（ZONES / LABELS / COLORS / STYLE / ASPECT）的类型专用模板
4. LABELS **必须**包含文章中的具体数据：实际数字、术语、指标、引用
5. **不要**在未先保存提示词文件的情况下将临时内联提示词传递给 `--prompt`
6. 选择生成技能，处理参考素材（`direct`/`style`/`palette`）
7. 如果 EXTEND.md 中启用了水印则应用水印
8. 从已保存的提示词文件生成；失败时重试一次

完整流程：[references/workflow.md](references/workflow.md#step-5-generate-images)

### 步骤 6：收尾

在段落后插入 `![描述]({relative-path}/NN-{type}-{slug}.png)`。路径根据输出目录设置相对于文章文件计算。

```
文章配图完成！
文章：[路径] | 类型：[type] | 密度：[level] | 风格：[style]
图片：X/N 张已生成
```

## 输出目录

输出目录由 EXTEND.md 中的 `default_output_dir` 决定（在首次设置时配置）：

| `default_output_dir` | 输出路径 | Markdown 插入路径 |
|----------------------|----------|-------------------|
| `imgs-subdir`（默认） | `{article-dir}/imgs/` | `imgs/NN-{type}-{slug}.png` |
| `same-dir` | `{article-dir}/` | `NN-{type}-{slug}.png` |
| `illustrations-subdir` | `{article-dir}/illustrations/` | `illustrations/NN-{type}-{slug}.png` |
| `independent` | `illustrations/{topic-slug}/` | `illustrations/{topic-slug}/NN-{type}-{slug}.png`（相对于 cwd） |

所有辅助文件（大纲、提示词）保存在输出目录内：

```
{output-dir}/
├── outline.md
├── prompts/
│   └── NN-{type}-{slug}.md
└── NN-{type}-{slug}.png
```

当输入为**粘贴内容**（无文件路径）时，始终使用 `illustrations/{topic-slug}/`，并将 `source-{slug}.{ext}` 一同保存。

**Slug**：2-4 个单词，kebab-case。**冲突**：追加 `-YYYYMMDD-HHMMSS`。

## 修改

| 操作 | 步骤 |
|------|------|
| 编辑 | 更新提示词 → 重新生成 → 更新引用 |
| 添加 | 定位 → 提示词 → 生成 → 更新大纲 → 插入 |
| 删除 | 删除文件 → 移除引用 → 更新大纲 |

## 参考文档

| 文件 | 内容 |
|------|------|
| [references/workflow.md](references/workflow.md) | 详细流程 |
| [references/usage.md](references/usage.md) | 命令语法 |
| [references/styles.md](references/styles.md) | 风格画廊 |
| [references/style-presets.md](references/style-presets.md) | 预设快捷方式（类型+风格） |
| [references/prompt-construction.md](references/prompt-construction.md) | 提示词模板 |
| [references/config/first-time-setup.md](references/config/first-time-setup.md) | 首次设置 |
