---
name: baoyu-translate
description: 在多种语言间翻译文章与文档，提供三种模式：quick（直接译）、normal（先分析再译）、refined（分析、翻译、审校、润色）。支持通过 EXTEND.md 自定义术语表与用词一致。适用于用户说「translate」「翻译」「精翻」「改成中文/英文」「本地化」或提供 URL/文件并表达翻译意图；也适用于「refined translation」「精细翻译」「快速翻译」「快翻」等表述。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-translate
    requires:
      anyBins:
        - bun
        - npx
---

# 翻译器（Translator）

三模式翻译 skill：**quick** 直接翻译；**normal** 在分析基础上翻译；**refined** 含审校与润色，面向可发布质量。

## 脚本目录

脚本位于 `scripts/` 子目录。`{baseDir}` = 本 SKILL_cn.md 所在目录。解析 `${BUN_X}`：若已安装 `bun` → `bun`；若有 `npx` → `npx -y bun`；否则建议安装 bun。将 `{baseDir}` 与 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | CLI 入口。默认行为将 Markdown 分块；亦支持显式 `chunk` 子命令 |
| `scripts/chunk.ts` | Markdown 分块实现，供 `main.ts` 使用，也可直接调用 |

## 偏好（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-translate/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-translate/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-translate/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-translate/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-translate/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-translate/EXTEND.md") { "user" }
```

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-translate/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-translate/EXTEND.md` | 用户主目录 |

| 结果 | 操作 |
|--------|--------|
| 找到 | 读取、解析并应用设置。本会话首次使用时简短提示：「正在使用 [path] 中的偏好设置。可编辑 EXTEND.md 自定义术语表、读者等。」 |
| 未找到 | **必须**执行首次设置（见下文）— **不得**静默使用默认项 |

**EXTEND.md 支持**：默认目标语言 | 默认模式 | 目标读者 | 自定义术语（行内或文件路径）| 翻译风格 | 分块参数

Schema：[references/config/extend-schema_cn.md](references/config/extend-schema_cn.md)

### 首次设置（阻塞）

**关键**：未找到 EXTEND.md 时，**必须**先完成首次设置，再开始任何翻译。此为**阻塞**操作。

完整说明：[references/config/first-time-setup_cn.md](references/config/first-time-setup_cn.md)

使用 `AskUserQuestion`，在**一次**调用中包含全部问题（目标语言、模式、读者、风格、保存位置）。用户回答后，在选定位置创建 EXTEND.md，确认「偏好已保存至 [path]」，再继续。

## 默认值

可配置项集中如下。EXTEND.md 覆盖下列默认；CLI 标志覆盖 EXTEND.md。

| 设置 | 默认值 | EXTEND.md 键 | CLI 标志 | 说明 |
|---------|---------|---------------|----------|-------------|
| 目标语言 | `zh-CN` | `target_language` | `--to` | 翻译目标语言 |
| 模式 | `normal` | `default_mode` | `--mode` | 翻译模式 |
| 读者 | `general` | `audience` | `--audience` | 目标读者画像 |
| 风格 | `storytelling` | `style` | `--style` | 翻译风格偏好 |
| 分块阈值 | `4000` | `chunk_threshold` | — | 超过该词数触发分块翻译 |
| 每块最大词数 | `5000` | `chunk_max_words` | — | 单块上限 |

## 模式

| 模式 | 标志 | 步骤 | 适用场景 |
|------|------|-------|-------------|
| Quick | `--mode quick` | 翻译 | 短文、非正式、要快 |
| Normal | `--mode normal`（默认） | 分析 → 翻译 | 文章、博客、一般内容 |
| Refined | `--mode refined` | 分析 → 翻译 → 审校 → 润色 | 可发布质量、重要文档 |

**默认模式**：Normal（可在 EXTEND.md 的 `default_mode` 中覆盖）。

**风格预设** — 控制译文语气（与读者预设独立）：

| 值 | 说明 | 效果 |
|-------|-------------|--------|
| `storytelling` | 叙事感强（默认） | 吸引读者、过渡顺、措辞生动 |
| `formal` | 专业、结构清晰 | 中性、组织清楚、少口语 |
| `technical` | 精确、文档体 | 简练、术语多、少修饰 |
| `literal` | 贴近原文结构 | 少重组、保留源句型 |
| `academic` | 学术严谨 | 正式语体、可用复杂从句、注意引用语境 |
| `business` | 简洁、结果导向 | 偏行动、高管友好、条目化思维 |
| `humorous` | 保留并改编幽默 | 机智活泼，在目标语中再现喜剧效果 |
| `conversational` | 口语、像说话 | 友好、像对朋友解释 |
| `elegant` | 文学、考究 | 修辞讲究、节奏与用词精细 |

也接受自定义风格描述，例如 `--style "poetic and lyrical"`。

**自动识别**：
- 「快翻」「quick」「直接翻译」→ quick
- 「精翻」「refined」「publication quality」「proofread」→ refined
- 其他 → 默认 normal

**升级提示**：normal 完成后展示：
> 译文已保存。若要进一步审校与润色，请回复「继续润色」或 `refine`。

若用户继续，对已有输出执行审校 → 修订 → 润色（与 refined 模式中 refined-workflow.md 的步骤 4–6 相同）。

## 用法

```
/translate [--mode quick|normal|refined] [--from <lang>] [--to <lang>] [--audience <audience>] [--style <style>] [--glossary <file>] <source>
```

- `<source>`：文件路径、URL 或内联文本
- `--from`：源语言（省略则自动检测）
- `--to`：目标语言（来自 EXTEND.md 或默认 `zh-CN`）
- `--audience`：读者画像（来自 EXTEND.md 或默认 `general`）
- `--style`：风格（来自 EXTEND.md 或默认 `storytelling`）
- `--glossary`：额外术语文件，与 EXTEND.md 术语合并

**读者预设**：

| 值 | 说明 | 效果 |
|-------|-------------|--------|
| `general` | 大众读者（默认） | 通俗，术语多作译者注 |
| `technical` | 开发者/工程师 | 常见技术词少注 |
| `academic` | 研究者/学者 | 正式语体、术语精确 |
| `business` | 商务人士 | 商务语气，技术概念会解释 |

也接受自定义读者描述，例如 `--audience "AI感兴趣的普通读者"`。

## 工作流

### 步骤 1：加载偏好

1.1 检查 EXTEND.md（见上文 **偏好**）

1.2 若存在内置术语表则加载（语言对）：
- EN→ZH：[references/glossary-en-zh.md](references/glossary-en-zh.md)

1.3 合并术语：EXTEND.md `glossary`（行内）+ EXTEND.md `glossary_files`（外部文件，路径相对 EXTEND.md）+ 内置表 + `--glossary` 文件（CLI 优先级最高）

### 步骤 2：物化源文并创建输出目录

物化源（文件照用；内联文本/URL → 保存为 `translate/{slug}.md`），再创建输出目录：`{source-dir}/{source-basename}-{target-lang}/`。若未指定 `--from` 则检测源语言。

细节：[references/workflow-mechanics.md](references/workflow-mechanics.md)

**输出目录内容**（中间与最终文件均在此）：

| 文件 | 模式 | 说明 |
|------|------|-------------|
| `translation.md` | 全部 | 终稿（固定此文件名） |
| `01-analysis.md` | Normal、Refined | 内容分析（领域、语气、术语等） |
| `02-prompt.md` | Normal、Refined | 组装后的翻译提示 |
| `03-draft.md` | Refined | 审校前初稿 |
| `04-critique.md` | Refined | 批判性审读（仅诊断） |
| `05-revision.md` | Refined | 按审读修订后的译文 |
| `chunks/` | 分块时 | 源块 + 译块 |

### 步骤 3：评估篇幅

Quick 模式不分块 — 无论多长都一次译完。翻译前估算词数。若超过分块阈值（默认 4000 词），主动提示：「本文约 {N} 词。Quick 模式不做分块一次译完 — 长文建议 `--mode normal` 以利于术语一致。」用户若不切换则继续。

Normal 与 Refined：

| 内容 | 操作 |
|---------|--------|
| < 阈值 | 整篇翻译 |
| ≥ 阈值 | 分块翻译（见步骤 3.1） |

**3.1 长文准备**（仅 normal/refined，且 ≥ 阈值）

分块翻译前：

1. **提取术语**：通读全文，收集专名、技术词、重复短语
2. **构建会话术语表**：与已加载术语合并，统一译法
3. **分块**：`${BUN_X} {baseDir}/scripts/main.ts <file> [--max-words <chunk_max_words>] [--output-dir <output-dir>]`
   - 按 Markdown 块（标题、段落、列表、代码块、表格等）解析
   - 在块边界切分以保持结构
   - 单块仍超阈值则回退到按行、再按词切分
4. **组装翻译提示**：
   - 主 Agent 读取 `01-analysis.md`（若存在），用 [references/subagent-prompt-template.md](references/subagent-prompt-template.md) 的 Part 1 组装共享上下文 — 内联解析后的风格预设（来自 `--style`、EXTEND.md `style` 或默认 `storytelling`）、内容背景、合并术语表、理解难点
   - 保存为输出目录中的 `02-prompt.md`（仅共享上下文，不含任务指令）
5. **通过 subagent 起草**（若可用 Agent 工具）：
   - **每块** 并行起一个 subagent（模板 Part 2）
   - 各 subagent 读 `02-prompt.md`，翻译本分块，写入 `chunks/chunk-NN-draft.md`
   - 术语一致由共享 `02-prompt.md`（术语 + 分析中的理解难点）保证
   - 无分块（低于阈值）：起一个 subagent 处理整篇源文件
   - 若无 Agent 工具：用 `02-prompt.md` 顺序内联翻译各块
6. **合并**：全部 subagent 完成后按序合并译块。若存在 `chunks/frontmatter.md` 则前置。保存为 `03-draft.md`（refined）或 `translation.md`（normal）
7. 中间文件（源块 + 译块）保留在 `chunks/`

**分块初稿合并后**，交回主 Agent 做批判审读、修订与润色（步骤 4）。

### 步骤 4：翻译与润色

**翻译原则**（各模式均适用）：

- **准确优先**：事实、数据、逻辑与原文一致
- **意译优先**：传达作者意图，而非逐词硬译。直译拗口或失效应大胆重组，用目标语自然说法表同一含义
- **比喻与修辞**：按意图理解隐喻、习语与修辞；源语意象在目标语中联想不同时，换用自然表达以传达相同观点与情绪
- **情绪忠实**：保留词语的感情色彩，不仅是字典义；带主观色彩的词（如 alarming、haunting）应在目标语中唤起相近感受
- **行文自然**：目标语语序与句式；源结构在目标语不自然时可拆分或重组
- **术语**：通用译法；首次出现可在括号注明原文
- **保留格式**：保留 Markdown（标题、粗斜体、图片、链接、代码块）
- **图片与语言**：翻译过程中图片引用保持原样；译完后检查文内图片，主文字语言是否与译文语言一致
- **Frontmatter**：若源文有 YAML 头，译文中保留并做如下处理：(1) 描述**源文**的元数据字段重命名 — `url`→`sourceUrl`，`title`→`sourceTitle`，`description`→`sourceDescription`，`author`→`sourceAuthor`，`date`→`sourceDate`，以及类似来源字段 — 统一加 `source` 前缀（camelCase）。(2) 文本字段的值译出，并作为新的顶层字段加入。(3) 其余字段（tags、categories、自定义）尽量保留，值按需翻译
- **尊重原文**：保持原意与意图；不增删、不妄加评论 — 但句式与意象可为表意而调整
- **译者注**：目标读者可能不懂的术语、概念或文化点 — 因行话、文化隔阂或领域知识 — 在术语后括号内用简练白话说明*指什么*，勿只贴英文。格式：`译文（English original，通俗解释）`。注疏深度按读者调整：大众多于技术读者。极短文（< 5 句）再减注 — 仅注目标读者多半不认识的非通用词；语境已明或广为人知的词不注。真正需要才注，忌过度注释。

#### Quick 模式

直接翻译 → 保存 `translation.md`。无分析文件，仍须遵守上述原则 — 尤其：修辞按义而非逐词、保留情绪色彩、句式为目标语自然表达。

#### Normal 模式

1. **分析** → `01-analysis.md`（领域、语气、读者、术语、理解难点、比喻与隐喻映射）
2. **组装提示** → `02-prompt.md`（内联风格预设、背景、术语表、理解难点的翻译说明）
3. **翻译**（依 `02-prompt.md`）→ `translation.md`

完成后提示用户：「译文已保存。若要进一步审校与润色，请回复 **继续润色** 或 **refine**。」

若用户继续，执行批判审读 → 修订 → 润色（同 refined 步骤 4–6），保存 `03-draft.md`（将当前 `translation.md` 更名）、`04-critique.md`、`05-revision.md`，并更新 `translation.md`。

#### Refined 模式

面向可发布质量的完整流程。逐步说明见 [references/refined-workflow.md](references/refined-workflow.md)。

步骤 3.1 中的 subagent 仅负责初稿。后续批判审读、修订、润色由主 Agent 完成，可自行再委派 subagent。

步骤与产出文件（均在输出目录）：
1. **分析** → `01-analysis.md`
2. **组装提示** → `02-prompt.md`
3. **初稿** → `03-draft.md`（含译者注；若分块则来自 subagent）
4. **批判审读** → `04-critique.md`（仅诊断：准确度、欧化、策略执行、表达问题）
5. **修订** → `05-revision.md`（落实全部审读意见）
6. **润色** → `translation.md`（终稿）

每步读取上一步产出并递进。

### 步骤 5：输出

终稿始终在输出目录的 `translation.md`。

写入终稿后做一次轻量「图片语言」检查：

1. 从译文中收集图片引用
2. 标出可能字多的图：封面、截图、图解、图表、框架图、信息图等
3. 若某图主文字语言可能与译文语言不一致，主动提醒用户
4. 提醒仅为列表；除非用户要求，不要自动本地化图片

提醒格式（沿用文中已有图片语法 — 标准 Markdown 或 wikilink）：
```text
可能需要图片本地化：
- ![example cover](attachments/example-cover.png): 封面可能仍为源语言文字，而正文已为目标语言
- ![example diagram](attachments/example-diagram.png): 可能为字多的框架图，请确认标签是否需翻译
```

展示摘要：
```
**翻译完成**（{mode} 模式）

源：{source-path}
语言：{from} → {to}
输出目录：{output-dir}/
终稿：{output-dir}/translation.md
已应用术语条目数：{count}
```

若发现图片语言可能不匹配，在摘要后附简短说明，并列出候选图。

## 扩展支持

可通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好**。
