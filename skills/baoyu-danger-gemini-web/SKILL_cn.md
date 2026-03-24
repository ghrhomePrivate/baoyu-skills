---
name: baoyu-danger-gemini-web
description: 通过逆向的 Gemini Web API 生成图像与文本。支持文本生成、根据 prompt 生成图像、参考图作为 vision 输入，以及多轮对话。在其他技能需要图像生成后端，或用户要求「用 Gemini 生成图」「Gemini 文本生成」，或需要具备 vision 能力的 AI 生成时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-danger-gemini-web
    requires:
      anyBins:
        - bun
        - npx
---

# Gemini Web 客户端

通过 Gemini Web API 进行文本/图像生成。支持参考图与多轮对话。

## 脚本目录

**重要**：本技能所有脚本位于 `scripts/` 子目录。

**Agent 执行说明**：
1. 将本 SKILL.md 所在目录记为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 解析 `${BUN_X}` 运行时：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun
4. 将本文档中所有 `{baseDir}` 与 `${BUN_X}` 替换为实际值

**脚本说明**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 文本/图像生成的 CLI 入口 |
| `scripts/gemini-webapi/*` | `gemini_webapi` 的 TypeScript 移植（GeminiClient、类型、工具函数） |

## 同意确认（必填）

首次使用前，须确认用户对使用逆向 API 的同意。

**同意文件位置**：
- macOS: `~/Library/Application Support/baoyu-skills/gemini-web/consent.json`
- Linux: `~/.local/share/baoyu-skills/gemini-web/consent.json`
- Windows: `%APPDATA%\baoyu-skills\gemini-web\consent.json`

**流程**：
1. 检查同意文件是否存在且含 `accepted: true` 与 `disclaimerVersion: "1.0"`
2. 若有效同意存在 → 打印含 `acceptedAt` 日期的警告后继续
3. 若无同意 → 展示免责声明，通过 `AskUserQuestion` 询问用户：
   - 「Yes, I accept」→ 以 ISO 时间戳创建同意文件后继续
   - 「No, I decline」→ 输出拒绝信息并停止
4. 同意文件格式：`{"version":1,"accepted":true,"acceptedAt":"<ISO>","disclaimerVersion":"1.0"}`

---

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-danger-gemini-web/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-danger-gemini-web/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────────────┬───────────────────┐
│                           路径                           │       位置        │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-danger-gemini-web/EXTEND.md          │ 项目目录          │
├──────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-danger-gemini-web/EXTEND.md    │ 用户主目录        │
└──────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 使用默认值                                                                │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认 model | 代理设置 | 自定义 data 目录

## 用法

```bash
# 文本生成
${BUN_X} {baseDir}/scripts/main.ts "Your prompt"
${BUN_X} {baseDir}/scripts/main.ts --prompt "Your prompt" --model gemini-3-flash

# 图像生成
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cute cat" --image cat.png
${BUN_X} {baseDir}/scripts/main.ts --promptfiles system.md content.md --image out.png

# Vision 输入（参考图）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Describe this" --reference image.png
${BUN_X} {baseDir}/scripts/main.ts --prompt "Create variation" --reference a.png --image out.png

# 多轮对话
${BUN_X} {baseDir}/scripts/main.ts "Remember: 42" --sessionId session-abc
${BUN_X} {baseDir}/scripts/main.ts "What number?" --sessionId session-abc

# JSON 输出
${BUN_X} {baseDir}/scripts/main.ts "Hello" --json
```

## 选项

| 选项 | 说明 |
|--------|-------------|
| `--prompt`, `-p` | Prompt 文本 |
| `--promptfiles` | 从文件读取 prompt（拼接） |
| `--model`, `-m` | Model：gemini-3-pro（默认）、gemini-3-flash、gemini-3-flash-thinking、gemini-3.1-pro-preview |
| `--image [path]` | 生成图像（默认：generated.png） |
| `--reference`, `--ref` | Vision 输入用的参考图 |
| `--sessionId` | 多轮对话的 session ID |
| `--list-sessions` | 列出已保存的 session |
| `--json` | 以 JSON 输出 |
| `--login` | 刷新 cookie 后退出 |
| `--cookie-path` | 自定义 cookie 文件路径 |
| `--profile-dir` | Chrome profile 目录 |

## 模型

| Model | 说明 |
|-------|-------------|
| `gemini-3-pro` | 默认，最新 3.0 Pro |
| `gemini-3-flash` | 快速、轻量 3.0 Flash |
| `gemini-3-flash-thinking` | 带 thinking 的 3.0 Flash |
| `gemini-3.1-pro-preview` | 3.1 Pro preview（空 header，自动路由） |

## 认证

首次运行会打开浏览器进行 Google 登录。Cookie 会自动缓存。

未显式设置 profile 目录时，刷新 cookie 可能会复用已在运行的本地 Chrome/Chromium 调试会话（绑定到标准 user-data 目录）。
设置 `--profile-dir` 或 `GEMINI_WEB_CHROME_PROFILE_DIR` 可强制使用独立 profile，并跳过已有会话复用。
这是尽力而为的 CDP session 复用路径，不是 Chrome 官方文档里基于 Chrome DevTools MCP 的 `--autoConnect` 流程。

支持的浏览器（自动检测）：Chrome、Chrome Canary/Beta、Chromium、Edge。

强制刷新：`--login`。覆盖浏览器：`GEMINI_WEB_CHROME_PATH` 环境变量。

## 环境变量

| 变量 | 说明 |
|----------|-------------|
| `GEMINI_WEB_DATA_DIR` | Data 目录 |
| `GEMINI_WEB_COOKIE_PATH` | Cookie 文件路径 |
| `GEMINI_WEB_CHROME_PROFILE_DIR` | Chrome profile 目录 |
| `GEMINI_WEB_CHROME_PATH` | Chrome 可执行文件路径 |
| `HTTP_PROXY`, `HTTPS_PROXY` | 访问 Google 的代理（可与命令写在同一行） |

## Sessions

Session 文件保存在 data 目录下的 `sessions/<id>.json`。

包含：`id`、`metadata`（Gemini chat 状态）、`messages` 数组、时间戳。

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好设置** 一节。
