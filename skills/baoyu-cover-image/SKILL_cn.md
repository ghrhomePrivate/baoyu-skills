---
name: baoyu-cover-image
description: 生成具有 5 个维度（类型、色板、渲染、文字、氛围）的文章封面图，组合 10 种色板和 7 种渲染风格。支持电影宽幅（2.35:1）、宽屏（16:9）和正方形（1:1）比例。当用户要求"生成封面图"、"创建文章封面"或"制作封面"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-cover-image
---

# 封面图生成器

为文章生成精美封面图，支持 5 维度自定义。

## 用法

```bash
# 根据内容自动选择维度
/baoyu-cover-image path/to/article.md

# 快速模式：跳过确认
/baoyu-cover-image article.md --quick

# 指定维度
/baoyu-cover-image article.md --type conceptual --palette warm --rendering flat-vector

# 风格预设（色板 + 渲染的快捷方式）
/baoyu-cover-image article.md --style blueprint

# 使用参考图
/baoyu-cover-image article.md --ref style-ref.png

# 直接输入内容
/baoyu-cover-image --palette mono --aspect 1:1 --quick
[粘贴内容]
```

## 选项

| 选项 | 描述 |
|--------|-------------|
| `--type <name>` | hero, conceptual, typography, metaphor, scene, minimal |
| `--palette <name>` | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone |
| `--rendering <name>` | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print |
| `--style <name>` | 预设快捷方式（见 [风格预设](references/style-presets_cn.md)） |
| `--text <level>` | none, title-only, title-subtitle, text-rich |
| `--mood <level>` | subtle, balanced, bold |
| `--font <name>` | clean, handwritten, serif, display |
| `--aspect <ratio>` | 16:9（默认）, 2.35:1, 4:3, 3:2, 1:1, 3:4 |
| `--lang <code>` | 标题语言（en, zh, ja 等） |
| `--no-title` | `--text none` 的别名 |
| `--quick` | 跳过确认，使用自动选择 |
| `--ref <files...>` | 用于风格/构图参考的参考图 |

## 五个维度

| 维度 | 可选值 | 默认值 |
|-----------|--------|---------|
| **类型** | hero, conceptual, typography, metaphor, scene, minimal | auto |
| **色板** | warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone | auto |
| **渲染** | flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print | auto |
| **文字** | none, title-only, title-subtitle, text-rich | title-only |
| **氛围** | subtle, balanced, bold | balanced |
| **字体** | clean, handwritten, serif, display | clean |

自动选择规则：[references/auto-selection_cn.md](references/auto-selection_cn.md)

## 展示画廊

**类型**：hero, conceptual, typography, metaphor, scene, minimal
→ 详情：[references/types_cn.md](references/types_cn.md)

**色板**：warm, elegant, cool, dark, earth, vivid, pastel, mono, retro, duotone
→ 详情：[references/palettes/](references/palettes/)

**渲染**：flat-vector, hand-drawn, painterly, digital, pixel, chalk, screen-print
→ 详情：[references/renderings/](references/renderings/)

**文字级别**：none（纯视觉）| title-only（默认）| title-subtitle | text-rich（带标签）
→ 详情：[references/dimensions/text_cn.md](references/dimensions/text_cn.md)

**氛围级别**：subtle（低对比度）| balanced（默认）| bold（高对比度）
→ 详情：[references/dimensions/mood_cn.md](references/dimensions/mood_cn.md)

**字体**：clean（无衬线）| handwritten | serif | display（粗体装饰）
→ 详情：[references/dimensions/font_cn.md](references/dimensions/font_cn.md)

## 文件结构

输出目录由 `default_output_dir` 偏好设置决定：
- `same-dir`：`{article-dir}/`
- `imgs-subdir`：`{article-dir}/imgs/`
- `independent`（默认）：`cover-image/{topic-slug}/`

```
<output-dir>/
├── source-{slug}.{ext}    # 源文件
├── refs/                  # 参考图（如有）
│   ├── ref-01-{slug}.{ext}
│   └── ref-01-{slug}.md   # 描述文件
├── prompts/cover.md       # 生成提示词
└── cover.png              # 输出图片
```

**Slug**：2-4 个单词，kebab-case。冲突时追加 `-YYYYMMDD-HHMMSS`

## 工作流

### 进度清单

```
封面图进度：
- [ ] 步骤 0：检查偏好设置 (EXTEND.md) ⛔ 阻塞
- [ ] 步骤 1：分析内容 + 保存参考图 + 确定输出目录
- [ ] 步骤 2：确认选项（6 个维度）⚠️ 除非 --quick
- [ ] 步骤 3：创建提示词
- [ ] 步骤 4：生成图片
- [ ] 步骤 5：完成报告
```

### 流程

```
输入 → [步骤 0：偏好设置] ─┬─ 找到 → 继续
                           └─ 未找到 → 首次设置 ⛔ 阻塞 → 保存 EXTEND.md → 继续
        ↓
分析 + 保存参考图 → [输出目录] → [确认：6 个维度] → 提示词 → 生成 → 完成
                                      ↓
                             （如果 --quick 或全部已指定则跳过）
```

### 步骤 0：加载偏好设置 ⛔ 阻塞

检查 EXTEND.md 是否存在（优先级：项目 → 用户）：
```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-cover-image/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-cover-image/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-cover-image/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-cover-image/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-cover-image/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-cover-image/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|--------|--------|
| 找到 | 加载，显示摘要 → 继续 |
| 未找到 | ⛔ 运行首次设置（[references/config/first-time-setup_cn.md](references/config/first-time-setup_cn.md)）→ 保存 → 继续 |

**关键**：如果未找到，必须在任何其他步骤或问题之前完成设置。

### 步骤 1：分析内容

1. **保存参考图**（如有）→ [references/workflow/reference-images_cn.md](references/workflow/reference-images_cn.md)
2. **保存源内容**（如果是粘贴的内容，保存为 `source.md`）
3. **分析内容**：主题、语气、关键词、视觉隐喻
4. **深度分析参考图** ⚠️：提取具体、可复现的元素（见 reference-images_cn.md）
5. **检测语言**：比较源内容、用户输入、EXTEND.md 偏好设置
6. **确定输出目录**：按照文件结构规则

**⚠️ 参考图中的人物 — 必须遵循全部 3 条规则：**

如果参考图包含**需要出现在封面中的人物**：

1. **`usage: direct`** — 必须在参考图描述文件中设置。当人物需要出现时，绝对不要使用 `style` 或 `palette`
2. **逐人描述** — 必须在 `refs/ref-NN-{slug}.md` 中描述每个人的显著特征（发型、眼镜、肤色、服装）。模糊的描述如"一个男人"将会失败
3. **`--ref` 标志** — 必须在步骤 4 中通过 `--ref` 传递参考图，以便模型能看到实际面部

详见 [reference-images_cn.md § 人物分析](references/workflow/reference-images_cn.md) 了解描述格式。

### 步骤 2：确认选项 ⚠️

**必须使用 `AskUserQuestion` 工具**以交互式选择方式呈现选项 — 而非纯文本表格。在单次 `AskUserQuestion` 调用中最多呈现 4 个问题（类型、色板、渲染、字体 + 设置）。每个问题首先显示推荐选项及理由，然后是其他备选项。

完整确认流程和问题格式：[references/workflow/confirm-options_cn.md](references/workflow/confirm-options_cn.md)

| 条件 | 跳过 | 仍然询问 |
|-----------|---------|-------------|
| `--quick` 或 `quick_mode: true` | 6 个维度 | 宽高比（除非指定 `--aspect`） |
| 全部 6 个 + `--aspect` 均已指定 | 全部 | 无 |

### 步骤 3：创建提示词

保存到 `prompts/cover.md`。模板：[references/workflow/prompt-template_cn.md](references/workflow/prompt-template_cn.md)

**关键 - 前置元数据中的参考图**：
- 保存到 `refs/` 的文件 → 添加到前置元数据 `references` 列表
- 以文字形式提取的风格（无文件）→ 省略 `references`，在正文中描述
- 写入之前 → 验证：`test -f refs/ref-NN-{slug}.{ext}`

**正文中的参考图元素**必须详细描述，使用"MUST"/"REQUIRED"前缀，并说明整合方式。

### 步骤 4：生成图片

1. **备份现有** `cover.png`（如果是重新生成）
2. **检查图片生成技能**；如果有多个，询问偏好
3. **处理参考图**（来自提示词前置元数据）：
   - `direct` 用途 → 通过 `--ref` 传递（使用支持参考图的后端）
   - `style`/`palette` → 提取特征，附加到提示词
4. **生成**：调用技能，传入提示词文件、输出路径、宽高比
5. 失败时：自动重试一次

### 步骤 5：完成报告

```
封面已生成！

主题：[topic]
类型：[type] | 色板：[palette] | 渲染：[rendering]
文字：[text] | 氛围：[mood] | 字体：[font] | 比例：[ratio]
标题：[title 或 "纯视觉"]
语言：[lang] | 水印：[enabled/disabled]
参考图：[N 张图片 或 "提取风格" 或 "无"]
位置：[directory path]

文件：
✓ source-{slug}.{ext}
✓ prompts/cover.md
✓ cover.png
```

## 图片修改

| 操作 | 步骤 |
|--------|-------|
| **重新生成** | 备份 → 先更新提示词文件 → 重新生成 |
| **更改维度** | 备份 → 确认新值 → 更新提示词 → 重新生成 |

## 构图原则

- **留白**：40-60% 的呼吸空间
- **视觉锚点**：主要元素居中或偏左
- **角色**：简化的轮廓；不使用写实人物
- **标题**：使用用户/源内容的原始标题；不要自行创造

## 扩展支持

通过 EXTEND.md 自定义配置。路径见**步骤 0**。

支持：水印 | 偏好维度 | 默认比例/输出 | 快速模式 | 自定义色板 | 语言

Schema：[references/config/preferences-schema_cn.md](references/config/preferences-schema_cn.md)

## 参考文档

**维度**：[text_cn.md](references/dimensions/text_cn.md) | [mood_cn.md](references/dimensions/mood_cn.md) | [font_cn.md](references/dimensions/font_cn.md)
**色板**：[references/palettes/](references/palettes/)
**渲染**：[references/renderings/](references/renderings/)
**类型**：[references/types_cn.md](references/types_cn.md)
**自动选择**：[references/auto-selection_cn.md](references/auto-selection_cn.md)
**风格预设**：[references/style-presets_cn.md](references/style-presets_cn.md)
**兼容性**：[references/compatibility_cn.md](references/compatibility_cn.md)
**视觉元素**：[references/visual-elements_cn.md](references/visual-elements_cn.md)
**工作流**：[confirm-options_cn.md](references/workflow/confirm-options_cn.md) | [prompt-template_cn.md](references/workflow/prompt-template_cn.md) | [reference-images_cn.md](references/workflow/reference-images_cn.md)
**配置**：[preferences-schema_cn.md](references/config/preferences-schema_cn.md) | [first-time-setup_cn.md](references/config/first-time-setup_cn.md) | [watermark-guide_cn.md](references/config/watermark-guide_cn.md)
