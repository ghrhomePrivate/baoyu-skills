---
name: baoyu-format-markdown
description: 为纯文本或带 frontmatter 的 markdown 排版：标题、摘要、各级标题、加粗、列表与代码块。在用户要求「格式化 markdown」「美化文章」「加排版」或改进文章版式时使用。输出为 {filename}-formatted.md。
version: 1.57.0
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-format-markdown
    requires:
      anyBins:
        - bun
        - npx
---

# Markdown 排版器

将纯文本或 markdown 转为结构清晰、便于阅读的 markdown。目标是帮助读者快速抓住要点、亮点与结构 — **不改变** 任何原文含义。

**核心原则**：只调整格式并修正明显笔误。**绝不** 增删或改写正文。

## 脚本目录

脚本在 `scripts/` 子目录。`{baseDir}` = 本 SKILL.md 所在目录。解析 `${BUN_X}`：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun。将 `{baseDir}` 与 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 主入口与 CLI 选项（使用 remark-cjk-friendly 处理 CJK 强调） |
| `scripts/quotes.ts` | 将 ASCII 引号替换为全角引号 |
| `scripts/autocorrect.ts` | 通过 autocorrect 处理中英混排间距 |

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-format-markdown/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-format-markdown/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-format-markdown/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-format-markdown/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-format-markdown/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-format-markdown/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────────────┬───────────────────┐
│                           路径                           │       位置        │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-format-markdown/EXTEND.md            │ 项目目录          │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-format-markdown/EXTEND.md      │ 用户主目录        │
└──────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 使用默认值                                                                │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：

| 设置 | 取值 | 默认 | 说明 |
|---------|--------|---------|-------------|
| `auto_select` | `true`/`false` | `false` | 跳过标题与摘要选择，自动选最优 |
| `auto_select_title` | `true`/`false` | `false` | 仅跳过标题选择 |
| `auto_select_summary` | `true`/`false` | `false` | 仅跳过摘要选择 |
| 其他 | — | — | 默认排版选项、字体/标点偏好等 |

## 用法

工作流分两阶段：**分析**（理解内容）再 **排版**（应用格式）。Claude 负责内容分析与排版（步骤 1–5），再运行脚本做标点/间距修正（步骤 6）。

## 工作流

### 步骤 1：读取并判断内容类型

读取用户指定文件，判断类型：

| 特征 | 分类 |
|-----------|----------------|
| 含 `---` YAML frontmatter | Markdown |
| 含 `#`、`##`、`###` 标题 | Markdown |
| 含 `**粗体**`、`*斜体*`、列表、代码块、引用 | Markdown |
| 以上皆无 | 纯文本 |

**若判定为 Markdown，用 `AskUserQuestion` 询问：**

```
Detected existing markdown formatting. What would you like to do?

1. Optimize formatting (Recommended)
   - Analyze content, improve headings, bold, lists for readability
   - Run typography script (spacing, emphasis fixes)
   - Output: {filename}-formatted.md

2. Keep original formatting
   - Preserve existing markdown structure
   - Run typography script only
   - Output: {filename}-formatted.md

3. Typography fixes only
   - Run typography script on original file in-place
   - No copy created, modifies original file directly
```

**按用户选择：**
- **Optimize**：进入步骤 2（完整流程）
- **Keep original**：跳到步骤 5，复制文件后执行步骤 6
- **Typography only**：跳到步骤 6，直接对原文件运行

### 步骤 2：分析内容（读者视角）

通读全文。站在读者角度：怎样能更快理解并记住关键信息？

产出需覆盖以下维度：

**2.1 亮点与核心洞见**
- 作者的核心论点或结论
- 令人意外的事实、数据或反直觉观点
- 可引用的金句或表达出色的句子（golden quotes）

**2.2 结构评估**
- 逻辑线是否清晰？是什么？
- 是否有自然分段却缺少标题？
- 是否有大段文字需要视觉分隔？

**2.3 对读者重要的信息**
- 可执行的建议或 takeaway
- 关键概念的定义与解释
- 埋在段落里的列表或枚举
- 更适合做成表格的对比

**2.4 格式问题**
- 标题层级缺失或不一致
- 一段内混杂多个主题
- 本可并列的条目却写成散文
- 命令、路径、术语未标为 code
- 明显笔误或格式错误

**将分析写入文件**：`{original-filename}-analysis.md`

该文件作为步骤 3 的蓝图。建议格式：

```markdown
# Content Analysis: {filename}

## Highlights & Key Insights
- [list findings]

## Structure Assessment
- Current flow: [describe]
- Suggested sections: [list heading candidates with brief rationale]

## Reader-Important Information
- [list actionable items, key concepts, buried lists, potential tables]

## Formatting Issues
- [list specific issues with location references]

## Typos Found
- [list any obvious typos with corrections, or "None found"]
```

### 步骤 3：检查/创建 frontmatter、标题与摘要

检查是否存在 YAML frontmatter（`---` 块）。若无则创建。

| 字段 | 处理 |
|-------|------------|
| `title` | 见下文 **标题生成** |
| `slug` | 从文件路径推断或由标题生成 |
| `summary` | 一句话摘要（见 **摘要生成**） |
| `description` | 更长描述性摘要（见 **摘要生成**） |
| `coverImage` | 若同目录存在 `imgs/cover.png`，使用相对路径 |

**标题生成：**

无论是否已有标题，除非设置了 `auto_select_title`，否则始终走标题优化流程。

**准备** — 通读全文并提取：
- 核心论点（一句话：「这篇文章在讲什么？」）
- 最有冲击力的观点或结论
- 读者痛点或好奇点
- 最难忘的比喻或金句

**生成标题** 时使用 `references/title-formulas_cn.md` 中的公式：

1. 根据文章的内容、语气与结构，选择 **2–3 个最匹配的 hook 公式**（参考文档中的「何时选用各公式」）
2. 再生成 **1–2 个直白标题**（描述型或论断型，不用公式 — 清楚准确）
3. 若用户指定方向（如「要悬念感」），优先该方向
4. 合计 **4–5 个候选**

用 `AskUserQuestion` 展示候选：

```
Pick a title:

1. [Hook title A] — (recommended) [formula name]
2. [Hook title B] — [formula name]
3. [Hook title C] — [formula name]
4. [Straightforward title D] — straightforward
5. [Straightforward title E] — straightforward

Enter number, or type a custom title:
```

最强的 hook 放首位并标 (recommended)。标题原则与禁止模式见 `references/title-formulas_cn.md`。

若首行为 H1，提取到 frontmatter 并从正文移除。若 frontmatter 已有 `title`，仍作为上下文，但照常生成新候选。

**摘要生成：**

直接生成两个版本（无需用户再选），均写入 frontmatter：

| 字段 | 长度 | 用途 |
|-------|--------|---------|
| `summary` | 1 句，约 50–80 字 | 简洁 hook — feed、社交分享、SEO meta |
| `description` | 2–3 句，约 100–200 字 | 更丰富上下文 — 文章预览、newsletter 摘要 |

**原则：**
- 传达对读者的 **核心价值**，而非仅陈述话题
- 用具体细节（数字、结果、具体方法）代替空泛描述
- `summary` 要短而完整；`description` 可补充支撑信息
- 若 frontmatter 已有 `summary` 或 `description`，保留已有项，只补缺失项

**禁止模式：**
- 「本文介绍……」「本文探讨……」
- 只有话题没有价值主张
- 用不同说法重复标题

**EXTEND.md 跳过行为：** 若 EXTEND.md 中 `auto_select: true` 或 `auto_select_title: true`，跳过标题选择 — 直接生成最优候选，不询问。

标题进入 frontmatter 后，正文 **不要** 再保留 H1（避免重复）。

### 步骤 4：排版正文

依据步骤 2 的分析应用格式。目标是可扫读、重点一眼可见。

**排版工具箱：**

| 元素 | 何时使用 | 格式 |
|---------|-------------|--------|
| 标题 | 自然话题边界、分段 | `##`、`###` 层级 |
| 加粗 | 关键结论、重要术语、核心 takeaway | `**bold**` |
| 无序列表 | 并列项、功能列表、示例 | `- item` |
| 有序列表 | 步骤、排序、流程 | `1. item` |
| 表格 | 对比、结构化数据、选项矩阵 | Markdown 表格 |
| Code | 命令、路径、术语、变量名 | `` `inline` `` 或围栏代码块 |
| 引用 | 金句、重要警告、引文 | `> quote` |
| 分隔线 | 大话题切换 | `---` |

**排版原则 — 不要做：**
- **不要** 增加句子、解释或评论
- **不要** 删除或缩短任何正文
- **不要** 改述或重写作者原文
- **不要** 加带偏见的标题（如「惊人发现」— 用中性描述性标题）
- **不要** 过度排版：不是每句都要加粗，不是每段都要标题

**排版原则 — 要做：**
- 保留作者语气、口吻与每个字
- **对读者会划重点的句子加粗** — 结论与核心 takeaway
- 仅在结构已很明显时，才把并列项从散文抽成列表
- 在话题真正切换处加标题 — 优先具体生动，避免泛泛（例如「3 天搞定 vs 传统方案」优于「方案对比」）
- 把埋在段落里的对比或结构化信息做成表格
- 金句、难忘表述或重要警告用 blockquote
- 修正明显笔误（依据步骤 2）

### 步骤 5：保存排版后文件

保存为 `{original-filename}-formatted.md`

**若目标文件已存在则先备份：**

```bash
if [ -f "{filename}-formatted.md" ]; then
  mv "{filename}-formatted.md" "{filename}-formatted.backup-$(date +%Y%m%d-%H%M%S).md"
fi
```

### 步骤 6：执行排版脚本

对输出文件运行脚本：

```bash
${BUN_X} {baseDir}/scripts/main.ts {output-file-path} [options]
```

**脚本选项：**

| 选项 | 简写 | 说明 | 默认 |
|--------|-------|-------------|---------|
| `--quotes` | `-q` | 将 ASCII 引号替换为全角引号 `"..."` | false |
| `--no-quotes` | | 不替换引号 | |
| `--spacing` | `-s` | 通过 autocorrect 处理中英间距 | true |
| `--no-spacing` | | 不处理中英间距 | |
| `--emphasis` | `-e` | 修正 CJK 强调标点问题 | true |
| `--no-emphasis` | | 不修正 CJK 强调问题 | |

**示例：**

```bash
# 默认：启用 spacing + emphasis，不替换引号
${BUN_X} {baseDir}/scripts/main.ts article.md

# 启用全部功能含引号替换
${BUN_X} {baseDir}/scripts/main.ts article.md --quotes

# 只修正强调，跳过间距
${BUN_X} {baseDir}/scripts/main.ts article.md --no-spacing
```

**脚本按选项执行：**
1. 修正 CJK 强调/加粗标点（默认开启）
2. 通过 autocorrect 处理中英混排间距（默认开启）
3. ASCII 引号 → 全角（默认关闭）
4. 格式化 frontmatter YAML（始终开启）

### 步骤 7：完成报告

汇总本次改动的报告：

```
**Formatting Complete**

**Files:**
- Analysis: {filename}-analysis.md
- Formatted: {filename}-formatted.md

**Content Analysis Summary:**
- Highlights found: X key insights
- Golden quotes: X memorable sentences
- Formatting issues fixed: X items

**Changes Applied:**
- Frontmatter: [added/updated] (title, slug, summary)
- Headings added: X (##: N, ###: N)
- Bold markers added: X
- Lists created: X (from prose → list conversion)
- Tables created: X
- Code markers added: X
- Blockquotes added: X
- Typos fixed: X [list each: "original" → "corrected"]

**Typography Script:**
- CJK spacing: [applied/skipped]
- Emphasis fixes: [applied/skipped]
- Quote replacement: [applied/skipped]
```

按实际改动调整报告 — 无改动的类别可省略。

## 说明

- 保留原文写作风格与语气
- 代码块指定正确语言（如 `python`、`javascript`）
- 保持中英混排间距规范
- 分析文件是工作文档 — 用于对齐「识别到的问题」与「实际排版」

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好设置** 一节。
