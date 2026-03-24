---
name: baoyu-danger-x-to-markdown
description: 将 X（Twitter）推文与文章转为带 YAML front matter 的 markdown。使用需用户同意的逆向 API。在用户提到「X 转 markdown」「推文转 markdown」「保存推文」，或提供 x.com/twitter.com 链接需转换时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-danger-x-to-markdown
    requires:
      anyBins:
        - bun
        - npx
---

# X 转 Markdown

将 X 内容转为 markdown：
- 推文/串 → 带 YAML front matter 的 Markdown
- X Articles → 全文抽取

## 脚本目录

脚本位于 `scripts/` 子目录。

**路径解析**：
1. `{baseDir}` = 本 SKILL.md 所在目录
2. 脚本路径 = `{baseDir}/scripts/main.ts`
3. 解析 `${BUN_X}` 运行时：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun

## 同意要求

**在任何转换之前**，须检查并取得同意。

### 同意流程

**步骤 1**：检查同意文件

```bash
# macOS
cat ~/Library/Application\ Support/baoyu-skills/x-to-markdown/consent.json

# Linux
cat ~/.local/share/baoyu-skills/x-to-markdown/consent.json
```

**步骤 2**：若 `accepted: true` 且 `disclaimerVersion: "1.0"` → 打印警告并继续：
```
Warning: Using reverse-engineered X API. Accepted on: <acceptedAt>
```

**步骤 3**：若缺失或版本不匹配 → 展示免责声明：
```
DISCLAIMER

This tool uses a reverse-engineered X API, NOT official.

Risks:
- May break if X changes API
- No guarantees or support
- Possible account restrictions
- Use at your own risk

Accept terms and continue?
```

使用 `AskUserQuestion`，选项：「Yes, I accept」|「No, I decline」

**步骤 4**：接受 → 创建同意文件：
```json
{
  "version": 1,
  "accepted": true,
  "acceptedAt": "<ISO timestamp>",
  "disclaimerVersion": "1.0"
}
```

**步骤 5**：拒绝 → 输出「User declined. Exiting.」并停止。

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────────┬───────────────────┐
│                            路径                            │       位置        │
├────────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md         │ 项目目录          │
├────────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-danger-x-to-markdown/EXTEND.md   │ 用户主目录        │
└────────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ **必须** 运行首次设置（见下文）— **不要** 静默创建默认值                  │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认是否下载媒体 | 默认输出目录

### 首次设置（阻塞）

**关键**：未找到 EXTEND.md 时，**必须** 使用 `AskUserQuestion` 询问用户偏好后再创建 EXTEND.md。**禁止** 在未询问的情况下用默认值创建。此为 **阻塞** 操作 — 在完成设置前 **不要** 进行任何转换。

在一次调用中用 `AskUserQuestion` 提出 **全部** 问题：

**问题 1** — header: "Media", question: "How to handle images and videos in tweets?"
- "Ask each time (Recommended)" — 保存 markdown 后，再询问是否下载媒体
- "Always download" — 始终下载媒体到本地 `imgs/` 与 `videos/` 目录
- "Never download" — 在 markdown 中保留原始远程 URL

**问题 2** — header: "Output", question: "Default output directory?"
- "x-to-markdown (Recommended)" — 保存到 ./x-to-markdown/{username}/{tweet-id}.md
- （用户可选 "Other" 以输入自定义路径）

**问题 3** — header: "Save", question: "Where to save preferences?"
- "User (Recommended)" — ~/.baoyu-skills/（所有项目）
- "Project" — .baoyu-skills/（仅本项目）

用户回答后，在选定位置创建 EXTEND.md，确认「Preferences saved to [path]」，再继续。

完整说明：[references/config/first-time-setup_cn.md](references/config/first-time-setup_cn.md)

### 支持的键

| 键 | 默认值 | 取值 | 说明 |
|-----|---------|--------|-------------|
| `download_media` | `ask` | `ask` / `1` / `0` | `ask` = 每次询问，`1` = 始终下载，`0` = 从不 |
| `default_output_dir` | 空 | 路径或空 | 默认输出目录（空 = `./x-to-markdown/`） |

**取值优先级**：
1. CLI 参数（`--download-media`、`-o`）
2. EXTEND.md
3. 技能默认值

## 用法

```bash
${BUN_X} {baseDir}/scripts/main.ts <url>
${BUN_X} {baseDir}/scripts/main.ts <url> -o output.md
${BUN_X} {baseDir}/scripts/main.ts <url> --download-media
${BUN_X} {baseDir}/scripts/main.ts <url> --json
```

## 选项

| 选项 | 说明 |
|--------|-------------|
| `<url>` | 推文或文章 URL |
| `-o <path>` | 输出路径 |
| `--json` | JSON 输出 |
| `--download-media` | 将图片/视频资源下载到本地 `imgs/` 与 `videos/`，并将 markdown 中的链接改写为本地相对路径 |
| `--login` | 仅刷新 cookie |

## 支持的 URL

- `https://x.com/<user>/status/<id>`
- `https://twitter.com/<user>/status/<id>`
- `https://x.com/i/article/<id>`

## 输出

```markdown
---
url: "https://x.com/user/status/123"
author: "Name (@user)"
tweetCount: 3
coverImage: "https://pbs.twimg.com/media/example.jpg"
---

Content...
```

**文件结构**：`x-to-markdown/{username}/{tweet-id}/{content-slug}.md`

启用 `--download-media` 时：
- 图片保存在 markdown 同目录的 `imgs/`
- 视频保存在 `videos/`
- Markdown 中的媒体链接会改写为本地相对路径

## 媒体下载工作流

依据 EXTEND.md 中的 `download_media`：

| 设置 | 行为 |
|---------|----------|
| `1`（始终） | 使用 `--download-media` 运行脚本 |
| `0`（从不） | 不使用 `--download-media` 运行脚本 |
| `ask`（默认） | 按下方「每次询问」流程 |

### 每次询问流程

1. **不带** `--download-media` 运行脚本 → 保存 markdown
2. 检查已保存 markdown 中是否有远程媒体 URL（图片/视频链接中的 `https://`）
3. **若无远程媒体** → 结束，无需再问
4. **若有远程媒体** → 使用 `AskUserQuestion`：
   - header: "Media", question: "Download N images/videos to local files?"
   - "Yes" — 下载到本地目录
   - "No" — 保留远程 URL
5. 若用户确认 → **再次** 使用 `--download-media` 运行脚本（覆盖 markdown，链接本地化）

## 认证

1. **环境变量**（优先）：`X_AUTH_TOKEN`、`X_CT0`
2. **Chrome 登录**（回退）：自动打开 Chrome，在本地缓存 cookie

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好设置** 一节。
