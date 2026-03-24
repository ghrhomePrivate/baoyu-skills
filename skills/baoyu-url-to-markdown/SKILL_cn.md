---
name: baoyu-url-to-markdown
description: 通过 Chrome CDP 抓取任意 URL 并转为 markdown。会一并保存渲染后的 HTML 快照，采用升级后的 Defuddle 流水线（更好地处理 web component 与 YouTube 字幕提取），必要时自动回退到 Defuddle 之前的 HTML 转 Markdown 流水线。若本地浏览器抓取完全失败，可回退到托管的 defuddle.md API。支持两种模式：页面加载后自动抓取，或等待用户信号（适用于需登录的页面）。适用于用户想把网页保存为 markdown 的场景。
version: 1.59.0
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-url-to-markdown
    requires:
      anyBins:
        - bun
        - npx
---

# URL 转 Markdown

通过 Chrome CDP 抓取任意 URL，保存渲染后的 HTML 快照，并转换为干净的 markdown。

## 脚本目录

**重要**：本 skill 的所有脚本位于 `scripts/` 子目录。

**Agent 执行说明**：
1. 将本 SKILL.md 所在目录记为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 解析 `${BUN_X}` 运行时：若已安装 `bun` → `bun`；若有 `npx` → `npx -y bun`；否则建议安装 bun
4. 将本文中的 `{baseDir}`、`${BUN_X}` 替换为实际值

**脚本索引**：
| 脚本 | 用途 |
|------|------|
| `scripts/main.ts` | URL 抓取的 CLI 入口 |
| `scripts/html-to-markdown.ts` | Markdown 转换入口与转换器选择 |
| `scripts/parsers/index.ts` | 统一 parser 入口：在通用转换器之前按 URL 分发站点规则 |
| `scripts/parsers/types.ts` | 各规则文件共用的统一 parser 接口 |
| `scripts/parsers/rules/*.ts` | 每个 URL 规则一个文件，例如 X 动态与 X 文章 |
| `scripts/defuddle-converter.ts` | 基于 Defuddle 的转换 |
| `scripts/legacy-converter.ts` | Defuddle 之前的旧版抽取与 markdown 转换 |
| `scripts/markdown-conversion-shared.ts` | 共用的元数据解析与 markdown 文档辅助 |

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-url-to-markdown/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-url-to-markdown/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-url-to-markdown/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-url-to-markdown/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────┬───────────────────┐
│                          路径                          │       位置        │
├────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-url-to-markdown/EXTEND.md          │ 项目目录          │
├────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-url-to-markdown/EXTEND.md    │ 用户主目录        │
└────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  结果     │                                  操作                                     │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ **必须**执行首次设置（见下文）— **不要**静默创建默认配置                  │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认是否下载媒体 | 默认输出目录 | 默认抓取模式 | 超时设置

### 首次设置（阻塞）

**关键**：未找到 EXTEND.md 时，**必须**使用 `AskUserQuestion` 询问用户偏好后再创建 EXTEND.md。**切勿**在未询问的情况下用默认值创建。此为**阻塞**操作 — 在完成设置前**不要**进行任何转换。

在**一次** `AskUserQuestion` 调用中包含**全部**问题：

**问题 1** — header: "Media", question: "How to handle images and videos in pages?"
- "Ask each time (Recommended)" — 保存 markdown 后询问是否下载媒体
- "Always download" — 始终下载媒体到本地 `imgs/` 与 `videos/` 目录
- "Never download" — markdown 中保留原始远程 URL

**问题 2** — header: "Output", question: "Default output directory?"
- "url-to-markdown (Recommended)" — 保存到 ./url-to-markdown/{domain}/{slug}.md
- （用户可选 "Other" 以输入自定义路径）

**问题 3** — header: "Save", question: "Where to save preferences?"
- "User (Recommended)" — ~/.baoyu-skills/（所有项目）
- "Project" — .baoyu-skills/（仅本项目）

用户回答后，在选定位置创建 EXTEND.md，确认「Preferences saved to [path]」，然后继续。

完整说明：[references/config/first-time-setup.md](references/config/first-time-setup.md)

### 支持的键

| 键 | 默认值 | 取值 | 说明 |
|-----|---------|--------|------|
| `download_media` | `ask` | `ask` / `1` / `0` | `ask` = 每次询问，`1` = 始终下载，`0` = 从不 |
| `default_output_dir` | empty | path or empty | 默认输出目录（空 = `./url-to-markdown/`） |

**EXTEND.md → CLI 映射**：
| EXTEND.md 键 | CLI 参数 | 说明 |
|---------------|----------|------|
| `download_media: 1` | `--download-media` | |
| `default_output_dir: ./posts/` | `--output-dir ./posts/` | 目录路径。**不要**传给 `-o`（`-o` 需要文件路径） |

**取值优先级**：
1. CLI 参数（`--download-media`、`-o`、`--output-dir`）
2. EXTEND.md
3. Skill 默认值

## 功能

- Chrome CDP 完整执行 JavaScript 渲染
- 针对需自定义 HTML 规则的站点的 URL 专属 parser 层
- 两种抓取模式：自动或等待用户
- 将渲染后的 HTML 保存为同名的 `-captured.html` 文件
- 带元数据的干净 markdown 输出
- 优先 Defuddle 的 markdown 转换，必要时自动回退到 git 历史中的 pre-Defuddle 抽取器
- X/Twitter 页面可对推文与文章使用 HTML 专项解析，改善 `x.com` / `twitter.com` 上的标题/正文/媒体抽取
- `archive.ph` 及相关镜像可从 `input[name=q]` 还原原始 URL，并在回退到正文前优先 `#CONTENT`
- 转换前物化 shadow DOM，使 web component 页面在序列化时更完整
- 当 YouTube 提供字幕轨时，可在 markdown 中包含字幕/文案文本
- 本地浏览器抓取彻底失败时，可回退到 `defuddle.md/<url>` 并仍保存 markdown
- 通过 wait 模式处理需登录的页面
- 将图片与视频下载到本地目录

## 用法

```bash
# 自动模式（默认）— 页面加载后抓取
${BUN_X} {baseDir}/scripts/main.ts <url>

# 等待模式 — 抓取前等待用户信号
${BUN_X} {baseDir}/scripts/main.ts <url> --wait

# 保存到指定文件
${BUN_X} {baseDir}/scripts/main.ts <url> -o output.md

# 保存到自定义输出目录（自动生成文件名）
${BUN_X} {baseDir}/scripts/main.ts <url> --output-dir ./posts/

# 将图片与视频下载到本地目录
${BUN_X} {baseDir}/scripts/main.ts <url> --download-media
```

## 选项

| 选项 | 说明 |
|------|------|
| `<url>` | 要抓取的 URL |
| `-o <path>` | 输出**文件**路径 — 必须是文件路径，不能是目录（默认：自动生成） |
| `--output-dir <dir>` | 输出根目录 — 自动生成 `{dir}/{domain}/{slug}.md`（默认：`./url-to-markdown/`） |
| `--wait` | 抓取前等待用户信号 |
| `--timeout <ms>` | 页面加载超时（默认：30000） |
| `--download-media` | 将图片/视频资源下载到本地 `imgs/` 与 `videos/`，并将 markdown 中的链接改写为本地相对路径 |

## 抓取模式

| 模式 | 行为 | 适用场景 |
|------|------|----------|
| Auto（默认） | 网络空闲时抓取 | 公开页面、静态内容 |
| Wait（`--wait`） | 用户就绪后再抓取 | 需登录、懒加载、付费墙 |

**等待模式流程**：
1. 使用 `--wait` 运行 → 脚本输出 "Press Enter when ready"
2. 请用户确认页面已就绪
3. 向 stdin 发送换行以触发抓取

## 输出格式

每次运行并排保存两个文件：

- Markdown：YAML front matter 含 `url`、`title`、`description`、`author`、`published`、可选 `coverImage` 与 `captured_at`，随后为转换后的 markdown 正文
- HTML 快照：`*-captured.html`，为从 Chrome 抓取的渲染后页面 HTML

当 Defuddle 或页面元数据提供语言线索时，markdown front matter 还会包含 `language`。

HTML 快照在任何 markdown 媒体本地化**之前**保存，因此能忠实反映用于转换的页面 DOM。
若使用托管 `defuddle.md` API 回退，仍会保存 markdown，但该次运行不会有本地 `-captured.html` 快照。

## 输出目录

默认：`url-to-markdown/<domain>/<slug>.md`  
使用 `--output-dir ./posts/`：`./posts/<domain>/<slug>.md`

HTML 快照使用相同 basename：

- `url-to-markdown/<domain>/<slug>-captured.html`
- `./posts/<domain>/<slug>-captured.html`

- `<slug>`：来自页面标题或 URL 路径（kebab-case，2–6 个词）
- 冲突处理：追加时间戳 `<slug>-YYYYMMDD-HHMMSS.md`

启用 `--download-media` 时：
- 图片保存在 markdown 同级的 `imgs/`
- 视频保存在 markdown 同级的 `videos/`
- Markdown 中的媒体链接改写为本地相对路径

## 转换回退

转换顺序：

1. 若站点规则匹配，先尝试 URL 专属 parser 层
2. 无专项 parser 匹配时，尝试 Defuddle
3. 对 YouTube 等富页面，优先保留 Defuddle 的 extractor 专属输出（含可用字幕），不要用 legacy 流水线整体替换
4. 若 Defuddle 抛错、无法加载、返回明显不完整的 markdown，或质量低于 legacy 流水线，则自动回退到 pre-Defuddle 抽取器
5. 若本地浏览器抓取流程在产出 markdown 前完全失败，尝试托管 API `https://defuddle.md/<url>` 并直接保存其 markdown 输出
6. Legacy 回退路径使用从 git 历史恢复的、基于 Readability/选择器/Next.js 数据的旧版 HTML 转 Markdown 实现

CLI 输出会显示：

- `Converter: parser:...` — URL 专属 parser 成功时
- `Converter: defuddle` — Defuddle 成功时
- `Converter: legacy:...` 以及 `Fallback used: ...` — 需要回退时
- `Converter: defuddle-api` — 本地浏览器抓取失败而改用托管 API 时

## 媒体下载工作流

依据 EXTEND.md 中的 `download_media`：

| 设置 | 行为 |
|------|------|
| `1`（始终） | 使用 `--download-media` 运行脚本 |
| `0`（从不） | 不使用 `--download-media` 运行脚本 |
| `ask`（默认） | 按下方「每次询问」流程 |

### 每次询问流程

1. **不**带 `--download-media` 运行脚本 → 保存 markdown
2. 检查已保存 markdown 中是否有远程媒体 URL（图片/视频链接中的 `https://`）
3. **若无远程媒体** → 结束，无需再询问
4. **若有远程媒体** → 使用 `AskUserQuestion`：
   - header: "Media", question: "Download N images/videos to local files?"
   - "Yes" — 下载到本地目录
   - "No" — 保留远程 URL
5. 若用户确认 → **再次**使用 `--download-media` 运行脚本（覆盖 markdown，链接改为本地）

## 环境变量

| 变量 | 说明 |
|------|------|
| `URL_CHROME_PATH` | 自定义 Chrome 可执行文件路径 |
| `URL_DATA_DIR` | 自定义数据目录 |
| `URL_CHROME_PROFILE_DIR` | 自定义 Chrome profile 目录 |

**排错**：找不到 Chrome → 设置 `URL_CHROME_PATH`。超时 → 增大 `--timeout`。复杂页面 → 尝试 `--wait`。若 markdown 质量差，查看保存的 `-captured.html`，并确认日志是否出现 legacy 回退。

### YouTube 说明

- 升级后的 Defuddle 路径使用异步 extractor，因此 YouTube 页面可直接在 markdown 正文中包含字幕文本。
- 是否有字幕取决于 YouTube 是否暴露字幕轨。关闭字幕、限制播放或区域屏蔽的视频仍可能只有简介类输出。
- 若页面需要时间加载简介、章节或播放器元数据，优先使用 `--wait`，在观看页完全 hydrated 后再抓取。

### 托管 API 回退

- 托管回退端点为 `https://defuddle.md/<url>`。Shell 示例：`curl https://defuddle.md/stephango.com`
- 仅在本地 Chrome/CDP 抓取路径彻底失败时使用。本地路径保真度更高，因可保存抓取 HTML 并处理已登录页面。
- 托管 API 返回的已是带 YAML front matter 的 Markdown，可按原样保存，再按需执行常规媒体本地化步骤。

## 扩展支持

通过 EXTEND.md 自定义配置。路径与可选项见**偏好设置**一节。
