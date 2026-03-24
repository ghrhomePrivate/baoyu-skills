---
name: baoyu-post-to-weibo
description: 向微博发布内容。支持带文字、图片、视频的普通微博，以及通过 Chrome CDP 用 Markdown 发布头条文章。适用于用户说「发微博」「发布微博」「post to Weibo」「share on Weibo」「写微博」「微博头条文章」等场景。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-weibo
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到微博

通过真实 Chrome 浏览器发布文字、图片、视频与长文到微博（绕过反自动化检测）。

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
| `scripts/weibo-post.ts` | 普通微博（文字 + 图片） |
| `scripts/weibo-article.ts` | 头条文章发布（Markdown） |
| `scripts/copy-to-clipboard.ts` | 复制内容到剪贴板 |
| `scripts/paste-from-clipboard.ts` | 发送真实粘贴按键 |

## 偏好（EXTEND.md）

按优先级检查 EXTEND.md 是否存在：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-weibo/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-weibo/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-weibo/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-weibo/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md") { "user" }
```

┌──────────────────────────────────────────────────┬───────────────────┐
│                       路径                        │       位置        │
├──────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-weibo/EXTEND.md      │ 项目目录          │
├──────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-weibo/EXTEND.md│ 用户主目录        │
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
- 首次运行：手动登录微博（会话会保留）

---

## 普通微博

文字 + 图片/视频（合计最多 18 个文件）。在微博首页发布。

```bash
${BUN_X} {baseDir}/scripts/weibo-post.ts "Hello Weibo!" --image ./photo.png
${BUN_X} {baseDir}/scripts/weibo-post.ts "Watch this" --video ./clip.mp4
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<text>` | 微博正文（位置参数） |
| `--image <path>` | 图片文件（可重复） |
| `--video <path>` | 视频文件（可重复） |
| `--profile <dir>` | 自定义 Chrome profile |

**说明**：脚本会打开浏览器并填入内容，需用户自行检查并手动发布。

---

## 头条文章

长文 Markdown，发布地址：`https://card.weibo.com/article/v3/editor`。

```bash
${BUN_X} {baseDir}/scripts/weibo-article.ts article.md
${BUN_X} {baseDir}/scripts/weibo-article.ts article.md --cover ./cover.jpg
```

**参数**：
| 参数 | 说明 |
|-----------|-------------|
| `<markdown>` | Markdown 文件（位置参数） |
| `--cover <path>` | 封面图 |
| `--title <text>` | 覆盖标题（最多 32 字，超长截断） |
| `--summary <text>` | 覆盖摘要/导语（最多 44 字，超长会重新生成） |
| `--profile <dir>` | 自定义 Chrome profile |

**Frontmatter**：YAML 头中支持 `title`、`summary`、`cover_image`。

**字数限制**：
- 标题：最多 32 字（超长会截断并警告）
- 摘要/导语：最多 44 字（超长会从正文自动再生成）

**文章流程**：
1. 打开 `https://card.weibo.com/article/v3/editor`
2. 点击「写文章」，等待编辑器可编辑
3. 填写标题（校验 32 字上限）
4. 填写摘要/导语（校验 44 字上限）
5. 通过粘贴将 HTML 插入 ProseMirror 编辑器
6. 逐个替换图片占位符（复制图片 → 选中占位 → 粘贴）

**合成后检查**：全部图片插入后，脚本会自动校验：
- 编辑区是否仍含 `WBIMGPH_` 占位符
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
