---
name: baoyu-post-to-x
description: 向 X（Twitter）发布内容与长文。支持带图/视频的普通帖与 X Articles（长文 Markdown）。使用真实 Chrome + CDP 绕过反自动化。适用于用户说「post to X」「tweet」「publish to Twitter」「share on X」等场景。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-x
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到 X（Twitter）

通过真实 Chrome 浏览器向 X 发布文字、图片、视频与长文（绕过反机器人检测）。

## 脚本目录

**重要**：本 skill 下所有脚本位于 `scripts/` 子目录。

**Agent 执行说明**：
1. 将本 SKILL_cn.md 所在目录记为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 将本文档中所有 `{baseDir}` 替换为实际路径
4. 解析 `${BUN_X}`：若已安装 `bun` → `bun`；若有 `npx` → `npx -y bun`；否则建议安装 bun

**脚本一览**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/x-browser.ts` | 普通发帖（文字 + 图片） |
| `scripts/x-video.ts` | 视频帖（文字 + 视频） |
| `scripts/x-quote.ts` | 引用推文并评论 |
| `scripts/x-article.ts` | 长文发布（Markdown） |
| `scripts/md-to-html.ts` | Markdown → HTML 转换 |
| `scripts/copy-to-clipboard.ts` | 复制内容到剪贴板 |
| `scripts/paste-from-clipboard.ts` | 发送真实粘贴按键 |
| `scripts/check-paste-permissions.ts` | 校验环境与权限 |

## 偏好（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-x/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-x/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-x/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-x/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────┬───────────────────┐
│                       路径                        │       位置        │
├──────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-x/EXTEND.md          │ 项目目录          │
├──────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-x/EXTEND.md    │ 用户主目录        │
└──────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│  Result   │                                  Action                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                       │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 使用默认值                                                                 │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认 Chrome profile

## 前置条件

- Google Chrome 或 Chromium
- `bun` 运行时
- 首次运行：手动登录 X（会话会保留）

## 预检（可选）

首次使用前建议运行环境检查，用户也可跳过。

```bash
${BUN_X} {baseDir}/scripts/check-paste-permissions.ts
```

检查项：Chrome、profile 隔离、Bun、Accessibility、剪贴板、粘贴按键、Chrome 冲突。

**若任一项失败**，按条目给出修复说明：

| 检查项 | 处理 |
|-------|-----|
| Chrome | 安装 Chrome 或设置环境变量 `X_BROWSER_CHROME_PATH` |
| Profile 目录 | 共享 profile 位于 `baoyu-skills/chrome-profile`（见 CLAUDE.md 中 Chrome Profile 一节） |
| Bun 运行时 | `brew install oven-sh/bun/bun`（macOS）或 `npm install -g bun` |
| Accessibility（macOS） | 系统设置 → 隐私与安全性 → 辅助功能 → 为终端应用开启 |
| 剪贴板复制 | 确保可用 Swift/AppKit（macOS Xcode CLI：`xcode-select --install`） |
| 粘贴按键（macOS） | 同上 Accessibility |
| 粘贴按键（Linux） | 安装 `xdotool`（X11）或 `ydotool`（Wayland） |

## 参考文档

- **普通发帖**：见 `references/regular-posts_cn.md`（手动流程、排错与技术细节）
- **X Articles**：见 `references/articles_cn.md`（长文发布指南）

---

## 发帖类型选择

除非用户明确指定类型：
- **纯文本** 且不超过 1 万字符 → **普通帖**（Premium 最高 1 万字符，非 Premium：280）
- **Markdown 文件**（.md）→ **X Article**

## 普通发帖

```bash
${BUN_X} {baseDir}/scripts/x-browser.ts "Hello!" --image ./photo.png
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<text>` | 帖子正文（位置参数） |
| `--image <path>` | 图片文件（可重复，最多 4 张） |
| `--profile <dir>` | 自定义 Chrome profile |

**说明**：脚本会打开浏览器并填入内容，需用户自行检查并手动发布。

---

## 视频帖

文字 + 视频文件。

```bash
${BUN_X} {baseDir}/scripts/x-video.ts "Check this out!" --video ./clip.mp4
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<text>` | 帖子正文（位置参数） |
| `--video <path>` | 视频文件（MP4、MOV、WebM） |
| `--profile <dir>` | 自定义 Chrome profile |

**说明**：脚本会打开浏览器并填入内容，需用户自行检查并手动发布。

**限制**：普通用户最长约 140 秒，Premium 最长 60 分钟。处理约 30–60 秒。

---

## 引用推文

引用已有推文并添加评论。

```bash
${BUN_X} {baseDir}/scripts/x-quote.ts https://x.com/user/status/123 "Great insight!"
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<tweet-url>` | 要引用的推文 URL（位置参数） |
| `<comment>` | 评论文字（位置参数，可选） |
| `--profile <dir>` | 自定义 Chrome profile |

**说明**：脚本会打开浏览器并填入内容，需用户自行检查并手动发布。

---

## X Articles

长文 Markdown（需 X Premium）。

```bash
${BUN_X} {baseDir}/scripts/x-article.ts article.md
${BUN_X} {baseDir}/scripts/x-article.ts article.md --cover ./cover.jpg
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<markdown>` | Markdown 文件（位置参数） |
| `--cover <path>` | 封面图 |
| `--title <text>` | 覆盖标题 |

**Frontmatter**：YAML 头中支持 `title`、`cover_image`。

**说明**：脚本会打开浏览器并填入长文，需用户自行检查并手动发布。

**合成后检查**：全部图片插入后，脚本会自动校验：
- 编辑区是否仍含 `XIMGPH_` 占位符
- 预期与实际图片数量

若检查失败（输出中有警告），请在用户发布前明确告知具体问题。

---

## 故障排除

### Chrome debug 端口未就绪

若脚本报错 `Chrome debug port not ready` 或 `Unable to connect`，先结束已有 Chrome CDP 进程再重试：

```bash
pkill -f "Chrome.*remote-debugging-port" 2>/dev/null; pkill -f "Chromium.*remote-debugging-port" 2>/dev/null; sleep 2
```

**重要**：应自动执行——遇到该错误时，结束 Chrome CDP 实例后重试命令，无需再询问用户。

## 说明

- 首次运行：需手动登录（会话持久化）
- 所有脚本仅向浏览器填入内容，须用户自行检查并手动发布
- 跨平台：macOS、Linux、Windows

## 扩展支持

可通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好** 一节。
