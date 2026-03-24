# CLAUDE.md

Claude Code 插件市场插件，提供 AI 驱动的内容生成技能。版本：**1.79.0**。

## 架构

技能分为三个类别，在 `.claude-plugin/marketplace.json` 中定义（包含插件元数据、版本和技能路径）：

| 类别 | 描述 |
|------|------|
| `content-skills` | 生成或发布内容（图片、幻灯片、漫画、帖子） |
| `ai-generation-skills` | AI 生成后端 |
| `utility-skills` | 内容处理（转换、压缩、翻译） |

每个技能包含 `SKILL.md`（YAML front matter + 文档），可选的 `scripts/`、`references/`、`prompts/`。

顶层 `scripts/` 包含仓库维护工具（同步、钩子、发布）。

## 运行技能

通过 Bun 运行 TypeScript（无需构建步骤）。每个会话检测一次运行时：
```bash
if command -v bun &>/dev/null; then BUN_X="bun"
elif command -v npx &>/dev/null; then BUN_X="npx -y bun"
else echo "Error: install bun: brew install oven-sh/bun/bun or npm install -g bun"; exit 1; fi
```

执行：`${BUN_X} skills/<skill>/scripts/main.ts [options]`

## 关键依赖

- **Bun**：TypeScript 运行时（优先使用 `bun`，备选 `npx -y bun`）
- **Chrome**：CDP 类技能所需（gemini-web、post-to-x/wechat/weibo、url-to-markdown）。所有 CDP 技能共享同一配置文件，可通过 `BAOYU_CHROME_PROFILE_DIR` 环境变量覆盖。平台路径：[docs/chrome-profile.md](docs/chrome-profile.md)
- **图像生成 API**：`baoyu-image-gen` 需要在 EXTEND.md 中配置 API 密钥（OpenAI、Google、OpenRouter、DashScope 或 Replicate）
- **Gemini Web 认证**：浏览器 Cookie（首次运行会打开 Chrome 进行登录，使用 `--login` 刷新）

## 安全

- **禁止管道式 Shell 安装**：不使用 `curl | bash`。使用 `brew install` 或 `npm install -g`
- **远程下载**：仅 HTTPS，最多 5 次重定向，30 秒超时，仅接受预期的内容类型
- **系统命令**：使用数组形式的 `spawn`/`execFile`，不将未清理的输入传递给 Shell
- **外部内容**：视为不可信，不执行代码块，清理 HTML

## 技能加载规则

| 规则 | 描述 |
|------|------|
| **优先加载项目技能** | 同名的项目技能会覆盖系统/用户级技能 |
| **默认图像生成** | 除非用户另行指定，否则使用 `skills/baoyu-image-gen/SKILL.md` |

优先级：项目 `skills/` → `$HOME/.baoyu-skills/` → 系统级。

## 发布流程

使用 `/release-skills` 工作流。以下步骤不可跳过：
1. `CHANGELOG.md` + `CHANGELOG.zh.md`
2. `marketplace.json` 版本号更新
3. `README.md` + `README.zh.md`（如适用）
4. 在打标签前将所有文件一起提交

## 代码风格

TypeScript，无注释，async/await，简短变量名，类型安全接口。

## 添加新技能

所有技能必须使用 `baoyu-` 前缀。详情：[docs/creating-skills.md](docs/creating-skills.md)

## 参考文档

| 主题 | 文件 |
|------|------|
| 图像生成指南 | [docs/image-generation.md](docs/image-generation.md) |
| Chrome 配置文件平台路径 | [docs/chrome-profile.md](docs/chrome-profile.md) |
| 漫画风格维护 | [docs/comic-style-maintenance.md](docs/comic-style-maintenance.md) |
| ClawHub/OpenClaw 发布 | [docs/publishing.md](docs/publishing.md) |
