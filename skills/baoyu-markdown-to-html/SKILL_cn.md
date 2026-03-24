---
name: baoyu-markdown-to-html
description: 将 Markdown 转为带样式的 HTML，主题兼容微信公众号。支持代码高亮、数学公式、PlantUML、脚注、提示块、信息图，以及可选地将外链转为文末引用。在用户要求「markdown 转 html」「将 md 转为 html」「md 转 html」「微信外链转底部引用」，或需要从 markdown 输出带样式 HTML 时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-markdown-to-html
    requires:
      anyBins:
        - bun
        - npx
---

# Markdown 转 HTML

将 Markdown 转为内联 CSS 的美观 HTML，针对微信公众号及其他平台优化。

## 脚本目录

**Agent 执行**：将本 SKILL.md 所在目录记为 `{baseDir}`。解析 `${BUN_X}`：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun。将 `{baseDir}` 与 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 主入口 |

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-markdown-to-html/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-markdown-to-html/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-markdown-to-html/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-markdown-to-html/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────────────────┬───────────────────┐
│                             路径                             │       位置        │
├──────────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-markdown-to-html/EXTEND.md               │ 项目目录          │
├──────────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-markdown-to-html/EXTEND.md         │ 用户主目录        │
└──────────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 使用默认值                                                                │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认主题 | 自定义 CSS 变量 | 代码块样式

## 工作流

### 步骤 0：预检（中文内容）

**条件**：仅当输入文件含中文文本时执行。

**检测**：
1. 读取输入 markdown
2. 判断是否含 CJK 字符（中/日/韩）
3. 若无 CJK → 跳到步骤 1

**格式建议**：

若检测到 CJK **且** 可用 `baoyu-format-markdown` 技能：

使用 `AskUserQuestion` 询问是否先排版。排版可修复：
- 标点夹在加粗标记内导致 `**` 解析失败
- 中英间距问题

**若用户同意**：调用 `baoyu-format-markdown` 排版，再以排版后文件为输入。

**若用户拒绝**：继续使用原文件。

### 步骤 1：确定主题

**主题解析顺序**（先匹配先用）：
1. 用户显式指定主题（CLI `--theme` 或对话）
2. EXTEND.md `default_theme`（本技能 EXTEND.md，在步骤 0 已查）
3. `baoyu-post-to-wechat` EXTEND.md `default_theme`（跨技能回退）
4. 若皆无 → 用 AskUserQuestion 确认

**跨技能 EXTEND.md 检查**（仅当本技能 EXTEND.md 无 `default_theme`）：

```bash
# 从 baoyu-post-to-wechat EXTEND.md 读取 default_theme
test -f "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && grep -o 'default_theme:.*' "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md"
```

```powershell
# PowerShell (Windows)
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { Select-String -Pattern 'default_theme:.*' -Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" | ForEach-Object { $_.Matches.Value } }
```

**若主题来自 EXTEND.md**：直接使用，**不要** 再问用户。

**若无默认**：用 AskUserQuestion 确认：

| 主题 | 说明 |
|-------|-------------|
| `default`（推荐） | 经典 — 传统版式，标题居中底边线，H2 白字彩色底 |
| `grace` | 雅致 — 文字阴影、圆角卡片、精致引用 |
| `simple` | 极简 — 现代极简、不对称圆角、留白干净 |
| `modern` | 现代 — 大圆角、胶囊标题、宽松行高（可配 `--color red` 做传统红金风） |

### 步骤 1.5：引用模式

**默认**：关闭。默认不询问。

**仅在用户明确要求**「微信外链转底部引用」「底部引用」「文末引用」，或传入 `--cite` 时启用。

**启用后行为**：
- 普通外链以上标序号呈现，并在文末汇总为 `引用链接` 区块。
- `https://mp.weixin.qq.com/...` 链接保持正文内直接跳转，不移到文末。
- 链接文本与 URL 相同的裸链保持行内。

### 步骤 2：转换

```bash
${BUN_X} {baseDir}/scripts/main.ts <markdown_file> --theme <theme> [--cite]
```

### 步骤 3：汇报结果

展示 JSON 结果中的输出路径。若已创建备份，一并说明。

## 用法

```bash
${BUN_X} {baseDir}/scripts/main.ts <markdown_file> [options]
```

**选项：**

| 选项 | 说明 | 默认 |
|--------|-------------|---------|
| `--theme <name>` | 主题名（default, grace, simple, modern） | default |
| `--color <name\|hex>` | 主色：预设名或十六进制 | 主题默认 |
| `--font-family <name>` | 字体：sans, serif, serif-cjk, mono，或 CSS 值 | 主题默认 |
| `--font-size <N>` | 字号：14px, 15px, 16px, 17px, 18px | 16px |
| `--title <title>` | 覆盖 frontmatter 中的标题 | |
| `--cite` | 将普通外链转为文末引用，追加 `引用链接` 区块 | false（关） |
| `--keep-title` | 保留正文首个标题 | false（会移除） |
| `--help` | 显示帮助 | |

**颜色预设：**

| Name | Hex | Label |
|------|-----|-------|
| blue | #0F4C81 | Classic Blue |
| green | #009874 | Emerald Green |
| vermilion | #FA5151 | Vibrant Vermilion |
| yellow | #FECE00 | Lemon Yellow |
| purple | #92617E | Lavender Purple |
| sky | #55C9EA | Sky Blue |
| rose | #B76E79 | Rose Gold |
| olive | #556B2F | Olive Green |
| black | #333333 | Graphite Black |
| gray | #A9A9A9 | Smoke Gray |
| pink | #FFB7C5 | Sakura Pink |
| red | #A93226 | China Red |
| orange | #D97757 | Warm Orange (modern default) |

**示例：**

```bash
# 基础转换（默认主题，移除首个标题）
${BUN_X} {baseDir}/scripts/main.ts article.md

# 指定主题
${BUN_X} {baseDir}/scripts/main.ts article.md --theme grace

# 主题 + 自定义颜色
${BUN_X} {baseDir}/scripts/main.ts article.md --theme modern --color red

# 普通外链转文末引用
${BUN_X} {baseDir}/scripts/main.ts article.md --cite

# 保留正文首个标题
${BUN_X} {baseDir}/scripts/main.ts article.md --keep-title

# 覆盖标题
${BUN_X} {baseDir}/scripts/main.ts article.md --title "My Article"
```

## 输出

**文件位置**：与输入 markdown 同目录。
- 输入：`/path/to/article.md`
- 输出：`/path/to/article.html`

**冲突处理**：若 HTML 已存在，会先备份：
- 备份：`/path/to/article.html.bak-YYYYMMDDHHMMSS`

**stdout JSON 输出：**

```json
{
  "title": "Article Title",
  "author": "Author Name",
  "summary": "Article summary...",
  "htmlPath": "/path/to/article.html",
  "backupPath": "/path/to/article.html.bak-20260128180000",
  "contentImages": [
    {
      "placeholder": "MDTOHTMLIMGPH_1",
      "localPath": "/path/to/img.png",
      "originalPath": "imgs/image.png"
    }
  ]
}
```

## 主题

| 主题 | 说明 |
|-------|-------------|
| `default` | 经典 — 传统版式，标题居中底边线，H2 白字彩色底 |
| `grace` | 雅致 — 文字阴影、圆角卡片、精致引用（by @brzhang） |
| `simple` | 极简 — 现代极简、不对称圆角、留白干净（by @okooo5km） |
| `modern` | 现代 — 大圆角、胶囊标题、宽松行高（可配 `--color red` 做传统红金风） |

## 支持的 Markdown 能力

| 能力 | 语法 |
|---------|--------|
| 标题 | `# H1` 至 `###### H6` |
| 粗体/斜体 | `**bold**`、`*italic*` |
| 代码块 | ` ```lang ` 与语法高亮 |
| 行内代码 | `` `code` `` |
| 表格 | GitHub 风格 markdown 表格 |
| 图片 | `![alt](src)` |
| 链接 | `[text](url)`；加 `--cite` 可将普通外链移到文末引用 |
| 引用 | `> quote` |
| 列表 | `-` 无序，`1.` 有序 |
| Alerts | `> [!NOTE]`、`> [!WARNING]` 等 |
| 脚注 | `[^1]` 引用 |
| Ruby | `{base|annotation}` |
| Mermaid | ` ```mermaid ` 图 |
| PlantUML | ` ```plantuml ` 图 |

## Frontmatter

支持 YAML frontmatter 元数据：

```yaml
---
title: Article Title
author: Author Name
description: Article summary
---
```

若无标题，从首个 H1/H2 或文件名提取。

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好设置** 一节。
