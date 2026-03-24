---
name: baoyu-post-to-wechat
description: 通过 API 或 Chrome CDP 向微信公众号发布内容。支持文章（HTML、markdown 或纯文本）与多图贴图（贴图，原图文）。Markdown 文章工作流默认将普通外链转为文末引用，便于微信排版。在用户提到「发布公众号」「post to wechat」「微信公众号」或「贴图/图文/文章」时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-post-to-wechat
    requires:
      anyBins:
        - bun
        - npx
---

# 发布到微信公众号

## 语言

**与用户语言一致**：用户使用中文则用中文回复；用户使用英文则用英文回复。

## 脚本目录

**Agent 执行**：将本 SKILL.md 所在目录记为 `{baseDir}`，脚本为 `{baseDir}/scripts/<name>.ts`。解析 `${BUN_X}`：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun。

| 脚本 | 用途 |
|--------|---------|
| `scripts/wechat-browser.ts` | 图文贴图 |
| `scripts/wechat-article.ts` | 浏览器发文章 |
| `scripts/wechat-api.ts` | API 发文章 |
| `scripts/md-to-wechat.ts` | Markdown → 微信可用 HTML（含图片占位） |
| `scripts/check-permissions.ts` | 检查环境与权限 |

## 偏好设置（EXTEND.md）

按优先级检查 EXTEND.md：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-post-to-wechat/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-post-to-wechat/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md") { "user" }
```

┌────────────────────────────────────────────────────────┬───────────────────┐
│                          路径                          │       位置        │
├────────────────────────────────────────────────────────┼───────────────────┤
│ .baoyu-skills/baoyu-post-to-wechat/EXTEND.md           │ 项目目录          │
├────────────────────────────────────────────────────────┼───────────────────┤
│ $HOME/.baoyu-skills/baoyu-post-to-wechat/EXTEND.md     │ 用户主目录        │
└────────────────────────────────────────────────────────┴───────────────────┘

┌───────────┬───────────────────────────────────────────────────────────────────────────┐
│   结果    │                                  操作                                   │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 找到      │ 读取、解析并应用设置                                                      │
├───────────┼───────────────────────────────────────────────────────────────────────────┤
│ 未找到    │ 运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 保存 → 继续 │
└───────────┴───────────────────────────────────────────────────────────────────────────┘

**EXTEND.md 支持**：默认主题 | 默认颜色 | 默认发布方式（api/browser）| 默认作者 | 默认是否开评论 | 默认是否仅粉丝可评论 | Chrome profile 路径

首次设置：[references/config/first-time-setup.md](references/config/first-time-setup.md)

**最低支持键名**（大小写不敏感，可用 `1/0` 或 `true/false`）：

| 键 | 默认 | 映射 |
|-----|---------|---------|
| `default_author` | 空 | CLI/frontmatter 未提供 `author` 时的回退 |
| `need_open_comment` | `1` | `draft/add` 请求中 `articles[].need_open_comment` |
| `only_fans_can_comment` | `0` | `draft/add` 请求中 `articles[].only_fans_can_comment` |

**EXTEND.md 示例（推荐）**：

```md
default_theme: default
default_color: blue
default_publish_method: api
default_author: 宝玉
need_open_comment: 1
only_fans_can_comment: 0
chrome_profile_path: /path/to/chrome/profile
```

**主题选项**：default, grace, simple, modern

**颜色预设**：blue, green, vermilion, yellow, purple, sky, rose, olive, black, gray, pink, red, orange（或十六进制）

**取值优先级**：
1. CLI 参数
2. Frontmatter
3. EXTEND.md（账号级 → 全局级）
4. 技能默认值

## 多账号支持

EXTEND.md 可管理多个公众号。存在 `accounts:` 块时，每个账号可有独立凭据、Chrome profile 与默认设置。

**兼容规则**：

| 条件 | 模式 | 行为 |
|-----------|------|----------|
| 无 `accounts` 块 | 单账号 | 与原先一致 |
| `accounts` 仅 1 条 | 单账号 | 自动选用，不提示 |
| `accounts` 2 条及以上 | 多账号 | 发布前提示选择 |
| 某账号 `default: true` | 多账号 | 预选默认，用户可切换 |

**多账号 EXTEND.md 示例**：

```md
default_theme: default
default_color: blue

accounts:
  - name: 宝玉的技术分享
    alias: baoyu
    default: true
    default_publish_method: api
    default_author: 宝玉
    need_open_comment: 1
    only_fans_can_comment: 0
    app_id: your_wechat_app_id
    app_secret: your_wechat_app_secret
  - name: AI工具集
    alias: ai-tools
    default_publish_method: browser
    default_author: AI工具集
    need_open_comment: 1
    only_fans_can_comment: 0
```

**可按账号设置的键**（也可全局作为回退）：
`default_publish_method`、`default_author`、`need_open_comment`、`only_fans_can_comment`、`app_id`、`app_secret`、`chrome_profile_path`

**仅全局的键**（跨账号共享）：
`default_theme`、`default_color`

### 账号选择（步骤 0.5）

插在文章发布工作流的步骤 0 与步骤 1 之间：

```
if no accounts block:
    → single-account mode (current behavior)
elif accounts.length == 1:
    → auto-select the only account
elif --account <alias> CLI arg:
    → select matching account
elif one account has default: true:
    → pre-select, show: "Using account: <name> (--account to switch)"
else:
    → prompt user:
      "Multiple WeChat accounts configured:
       1) <name1> (<alias1>)
       2) <name2> (<alias2>)
       Select account [1-N]:"
```

### 凭据解析（API 方式）

对已选账号，alias 为 `{alias}` 时：

1. EXTEND.md 账号块内联 `app_id` / `app_secret`
2. 环境变量 `WECHAT_{ALIAS}_APP_ID` / `WECHAT_{ALIAS}_APP_SECRET`（alias 大写，连字符变下划线）
3. `.baoyu-skills/.env` 中带前缀的 `WECHAT_{ALIAS}_APP_ID`
4. `~/.baoyu-skills/.env` 中带前缀的键
5. 回退到无前缀 `WECHAT_APP_ID` / `WECHAT_APP_SECRET`

**.env 多账号示例**：

```bash
# Account: baoyu
WECHAT_BAOYU_APP_ID=your_wechat_app_id
WECHAT_BAOYU_APP_SECRET=your_wechat_app_secret

# Account: ai-tools
WECHAT_AI_TOOLS_APP_ID=your_ai_tools_wechat_app_id
WECHAT_AI_TOOLS_APP_SECRET=your_ai_tools_wechat_app_secret
```

### Chrome Profile（浏览器方式）

每个账号使用独立 Chrome profile，登录会话互不影响：

| 来源 | 路径 |
|--------|------|
| EXTEND.md 账号 `chrome_profile_path` | 原样使用 |
| 由 alias 自动生成 | `{shared_profile_parent}/wechat-{alias}/` |
| 单账号回退 | 共享默认 profile（原行为） |

### CLI `--account` 参数

所有发布脚本均支持 `--account <alias>`：

```bash
${BUN_X} {baseDir}/scripts/wechat-api.ts <file> --theme default --account ai-tools
${BUN_X} {baseDir}/scripts/wechat-article.ts --markdown <file> --theme default --account baoyu
${BUN_X} {baseDir}/scripts/wechat-browser.ts --markdown <file> --images ./photos/ --account baoyu
```

## 起飞前检查（可选）

首次使用前建议运行环境检查。用户可跳过。

```bash
${BUN_X} {baseDir}/scripts/check-permissions.ts
```

检查项：Chrome、profile 隔离、Bun、Accessibility、剪贴板、粘贴快捷键、API 凭据、Chrome 冲突。

**若有失败项**，按项给出修复说明：

| 检查 | 修复 |
|-------|-----|
| Chrome | 安装 Chrome 或设置 `WECHAT_BROWSER_CHROME_PATH` |
| Profile 目录 | 共享 profile 位于 `baoyu-skills/chrome-profile`（见 CLAUDE.md Chrome Profile） |
| Bun | `brew install oven-sh/bun/bun`（macOS）或 `npm install -g bun` |
| Accessibility（macOS） | 系统设置 → 隐私与安全性 → 辅助功能 → 允许终端应用 |
| 剪贴板复制 | 确保 Swift/AppKit 可用（macOS Xcode CLI：`xcode-select --install`） |
| 粘贴快捷键（macOS） | 同上 Accessibility |
| 粘贴快捷键（Linux） | 安装 `xdotool`（X11）或 `ydotool`（Wayland） |
| API 凭据 | 按步骤 2 引导设置，或手动写入 `.baoyu-skills/.env` |

## 图文贴图

短内容 + 多图（最多 9 张）：

```bash
${BUN_X} {baseDir}/scripts/wechat-browser.ts --markdown article.md --images ./images/
${BUN_X} {baseDir}/scripts/wechat-browser.ts --title "标题" --content "内容" --image img.png --submit
```

详情见 [references/image-text-posting.md](references/image-text-posting.md)。

## 文章发布工作流

复制下列清单并在完成时勾选：

```
Publishing Progress:
- [ ] Step 0: Load preferences (EXTEND.md)
- [ ] Step 0.5: Resolve account (multi-account only)
- [ ] Step 1: Determine input type
- [ ] Step 2: Select method and configure credentials
- [ ] Step 3: Resolve theme/color and validate metadata
- [ ] Step 4: Publish to WeChat
- [ ] Step 5: Report completion
```

### 步骤 0：加载偏好

检查并加载 EXTEND.md（见上文 **偏好设置**）。

**关键**：若未找到，须先完成首次设置，再进行其他步骤或提问。

解析并保存供后续使用：
- `default_theme`（默认 `default`）
- `default_color`（未设置则省略 — 用主题默认）
- `default_author`
- `need_open_comment`（默认 `1`）
- `only_fans_can_comment`（默认 `0`）

### 步骤 1：判断输入类型

| 输入类型 | 检测 | 操作 |
|------------|-----------|--------|
| HTML 文件 | 路径以 `.html` 结尾且文件存在 | 跳到步骤 3 |
| Markdown 文件 | 路径以 `.md` 结尾且文件存在 | 进入步骤 2 |
| 纯文本 | 非文件路径，或文件不存在 | 存为 markdown 后进入步骤 2 |

**纯文本处理**：

1. 从内容生成 slug（前 2–4 个有意义词，kebab-case）
2. 创建目录并保存文件：

```bash
mkdir -p "$(pwd)/post-to-wechat/$(date +%Y-%m-%d)"
# 保存到: post-to-wechat/yyyy-MM-dd/[slug].md
```

3. 按 markdown 继续处理

**Slug 示例**：
- "Understanding AI Models" → `understanding-ai-models`
- "人工智能的未来" → `ai-future`（slug 译为英文）

### 步骤 2：选择发布方式并配置凭据

**询问发布方式**（除非 EXTEND.md 或 CLI 已指定）：

| 方式 | 速度 | 要求 |
|--------|-------|--------------|
| `api`（推荐） | 快 | API 凭据 |
| `browser` | 慢 | Chrome、登录会话 |

**若选 API — 检查凭据**：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/.env && grep -q "WECHAT_APP_ID" .baoyu-skills/.env && echo "project"
test -f "$HOME/.baoyu-skills/.env" && grep -q "WECHAT_APP_ID" "$HOME/.baoyu-skills/.env" && echo "user"
```

```powershell
# PowerShell (Windows)
if ((Test-Path .baoyu-skills/.env) -and (Select-String -Quiet -Pattern "WECHAT_APP_ID" .baoyu-skills/.env)) { "project" }
if ((Test-Path "$HOME/.baoyu-skills/.env") -and (Select-String -Quiet -Pattern "WECHAT_APP_ID" "$HOME/.baoyu-skills/.env")) { "user" }
```

**若缺少凭据 — 引导设置**：

```
WeChat API credentials not found.

To obtain credentials:
1. Visit https://mp.weixin.qq.com
2. Go to: 开发 → 基本配置
3. Copy AppID and AppSecret

Where to save?
A) Project-level: .baoyu-skills/.env (this project only)
B) User-level: ~/.baoyu-skills/.env (all projects)
```

选定位置后提示输入并写入 `.env`：

```
WECHAT_APP_ID=<user_input>
WECHAT_APP_SECRET=<user_input>
```

### 步骤 3：解析主题/颜色并校验元数据

1. **主题**（先匹配先用，已解析则 **不要** 再问用户）：
   - CLI `--theme`
   - EXTEND.md `default_theme`（步骤 0 已加载）
   - 回退：`default`

2. **颜色**（先匹配先用）：
   - CLI `--color`
   - EXTEND.md `default_color`（步骤 0）
   - 未设置则省略（主题默认生效）

3. **校验元数据**（markdown 用 frontmatter，HTML 用 meta 标签）：

| 字段 | 若缺失 |
|-------|------------|
| Title | 提示：「输入标题，或回车从正文自动生成」 |
| Summary | 提示：「输入摘要，或回车自动生成（SEO 推荐）」 |
| Author | 回退链：CLI `--author` → frontmatter `author` → EXTEND.md `default_author` |

**自动生成逻辑**：
- **Title**：首个 H1/H2，或首句
- **Summary**：首段，截断至 120 字符

4. **封面图检查**（API `article_type=news` 必填）：
   1. 若提供 CLI `--cover` 则用之
   2. 否则 frontmatter（`coverImage`、`featureImage`、`cover`、`image`）
   3. 否则文章目录默认 `imgs/cover.png`
   4. 否则首张正文内联图
   5. 仍无则停止并要求提供封面

### 步骤 4：发布至微信

**关键**：发布脚本内部会处理 markdown 转换。**不要** 事先把 markdown 转成 HTML — 直接传原始 `.md`。这样 API 路径会把图片渲染为 `<img>`（便于 API 上传），浏览器路径使用占位符（粘贴替换工作流）。

**Markdown 引用默认**：
- markdown 输入时，普通外链默认转为文末引用。
- 仅当用户明确要求保留行内普通外链时使用 `--no-cite`。
- 已有 HTML 输入保持原样，不额外做引用转换。

**API 方式**（接受 `.md` 或 `.html`）：

```bash
${BUN_X} {baseDir}/scripts/wechat-api.ts <file> --theme <theme> [--color <color>] [--title <title>] [--summary <summary>] [--author <author>] [--cover <cover_path>] [--no-cite]
```

**关键**：始终包含 `--theme`。即使用 `default` 也不要省略。仅当用户或 EXTEND.md 显式设置颜色时才加 `--color`。

**`draft/add` 载荷规则**：
- Endpoint：`POST https://api.weixin.qq.com/cgi-bin/draft/add?access_token=ACCESS_TOKEN`
- `article_type`：`news`（默认）或 `newspic`
- `news` 须含 `thumb_media_id`（封面必填）
- 始终解析并发送：
  - `need_open_comment`（默认 `1`）
  - `only_fans_can_comment`（默认 `0`）
- `author`：`--author` → frontmatter `author` → EXTEND.md `default_author`

若脚本参数未暴露上述两评论字段，仍须保证最终 API 请求体含解析后的值。

**浏览器方式**（`--markdown` 或 `--html`）：

```bash
${BUN_X} {baseDir}/scripts/wechat-article.ts --markdown <markdown_file> --theme <theme> [--color <color>] [--no-cite]
${BUN_X} {baseDir}/scripts/wechat-article.ts --html <html_file>
```

### 步骤 5：完成报告

**API 方式**须含草稿管理链接：

```
WeChat Publishing Complete!

Input: [type] - [path]
Method: API
Theme: [theme name] [color if set]

Article:
• Title: [title]
• Summary: [summary]
• Images: [N] inline images
• Comments: [open/closed], [fans-only/all users]

Result:
✓ Draft saved to WeChat Official Account
• media_id: [media_id]

Next Steps:
→ Manage drafts: https://mp.weixin.qq.com (登录后进入「内容管理」→「草稿箱」)

Files created:
[• post-to-wechat/yyyy-MM-dd/slug.md (if plain text)]
[• slug.html (converted)]
```

**浏览器方式**：

```
WeChat Publishing Complete!

Input: [type] - [path]
Method: Browser
Theme: [theme name] [color if set]

Article:
• Title: [title]
• Summary: [summary]
• Images: [N] inline images

Result:
✓ Draft saved to WeChat Official Account

Files created:
[• post-to-wechat/yyyy-MM-dd/slug.md (if plain text)]
[• slug.html (converted)]
```

## 详细参考

| 主题 | 参考 |
|-------|-----------|
| 图文参数、自动压缩 | [references/image-text-posting.md](references/image-text-posting.md) |
| 文章主题、图片处理 | [references/article-posting.md](references/article-posting.md) |

## 功能对比

| 能力 | 图文 | 文章（API） | 文章（浏览器） |
|---------|------------|---------------|-------------------|
| 纯文本输入 | ✗ | ✓ | ✓ |
| HTML 输入 | ✗ | ✓ | ✓ |
| Markdown 输入 | 标题/正文 | ✓ | ✓ |
| 多图 | ✓（最多 9） | ✓（行内） | ✓（行内） |
| 主题 | ✗ | ✓ | ✓ |
| 自动生成元数据 | ✗ | ✓ | ✓ |
| 默认封面回退（`imgs/cover.png`） | ✗ | ✓ | ✗ |
| 评论控制（`need_open_comment`、`only_fans_can_comment`） | ✗ | ✓ | ✗ |
| 需要 Chrome | ✓ | ✗ | ✓ |
| 需要 API 凭据 | ✗ | ✓ | ✗ |
| 速度 | 中 | 快 | 慢 |

## 前置条件

**API 方式**：
- 微信公众号 API 凭据
- 步骤 2 引导设置，或手动写入 `.baoyu-skills/.env`

**浏览器方式**：
- Google Chrome
- 首次运行：登录公众号（会话会保留）

**配置文件位置**（优先级）：
1. 环境变量
2. `<cwd>/.baoyu-skills/.env`
3. `~/.baoyu-skills/.env`

## 故障排除

| 问题 | 处理 |
|-------|---------|
| 缺少 API 凭据 | 按步骤 2 引导设置 |
| Access token 错误 | 检查凭据是否有效、未过期 |
| 浏览器未登录 | 首次运行会打开浏览器 — 扫码登录 |
| 找不到 Chrome | 设置 `WECHAT_BROWSER_CHROME_PATH` |
| 缺标题/摘要 | 用自动生成或手动填写 |
| 无封面 | 在 frontmatter 加封面，或将 `imgs/cover.png` 放在文章目录 |
| 评论默认值不对 | 检查 EXTEND.md 中 `need_open_comment`、`only_fans_can_comment` |
| 粘贴失败 | 检查系统剪贴板权限 |

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **偏好设置** 一节。
