# 更新日志

[English](./CHANGELOG.md) | 中文

## 1.79.0 - 2026-03-22

### 新功能
- `baoyu-post-to-wechat`：改进凭据加载，支持多来源解析、优先级排序，以及跳过不完整来源的诊断信息

## 1.78.0 - 2026-03-22

### 新功能
- `baoyu-url-to-markdown`：为 X/Twitter 和 archive.ph 站点添加 URL 特定解析层
- `baoyu-url-to-markdown`：改进 slug 生成，支持停用词移除和子目录输出结构

### 修复
- `baoyu-url-to-markdown`：在旧版转换器中保留包含媒体的锚元素
- `baoyu-url-to-markdown`：更智能的标题去重，避免冗余标题

## 1.77.0 - 2026-03-22

### 新功能
- `baoyu-youtube-transcript`：为章节数据添加结束时间（by @jzOcb）

### 修复
- `sync-clawhub`：跳过失败的技能而非中止

## 1.76.1 - 2026-03-21

### 文档
- `baoyu-youtube-transcript`：修复 zsh glob 问题 — 运行脚本时始终对 YouTube URL 使用单引号

## 1.76.0 - 2026-03-21

### 新功能
- `baoyu-youtube-transcript`：在 Markdown 输出中添加标题、描述摘要和封面图片

### 修复
- `baoyu-markdown-to-html`：在测试运行器中使用 process.execPath 和 tsx 导入

## 1.75.0 - 2026-03-21

### 新功能
- `baoyu-youtube-transcript`：新技能 — 下载 YouTube 视频字幕/转录文本和封面图片，支持多语言、章节和说话人识别

## 1.74.1 - 2026-03-21

### 修复
- `baoyu-image-gen`：对齐 OpenRouter 图像生成与当前 API，增强图像支持，收窄 Gemini 宽高比范围（by @cwandev）
- `baoyu-image-gen`：扩大 OpenRouter 模型检测和宽高比验证范围

## 1.74.0 - 2026-03-20

### 新功能
- `baoyu-markdown-to-html`：CLI 现在支持所有渲染选项 — color、font-family、font-size、code-theme、mac-code-block、line-number、count、legend

### 修复
- `baoyu-markdown-to-html`：修复 CSS 自定义属性正则表达式以处理带引号的值；grace/simple 主题现在叠加默认 CSS

## 1.73.3 - 2026-03-20

### 修复
- `baoyu-post-to-wechat`：修复占位符替换问题，避免较短的占位符匹配到较长的编号变体

## 1.73.2 - 2026-03-20

### 修复
- `baoyu-post-to-wechat`：修复正文图片上传，正确使用 media/uploadimg API 并验证格式和大小（by @AICreator-Wind）

### 重构
- `baoyu-post-to-wechat`：提取图片处理模块，用于本地格式转换（WebP/BMP/GIF → JPEG/PNG），替代素材 API 回退

## 1.73.1 - 2026-03-18

### 重构
- `baoyu-danger-x-to-markdown`：将测试从 bun:test 迁移到 node:test

## 1.73.0 - 2026-03-18

### 新功能
- `baoyu-danger-x-to-markdown`：为 X 文章添加视频媒体支持，包括海报图片和视频链接渲染

## 1.72.0 - 2026-03-18

### 新功能
- `baoyu-danger-x-to-markdown`：添加 MARKDOWN 实体支持，用于渲染 X 文章中嵌入的 markdown/代码块

## 1.71.0 - 2026-03-17

### 新功能
- `baoyu-image-gen`：为 Seedream 5.0/4.5/4.0 模型添加参考图片支持，包含模型特定的尺寸验证

## 1.70.0 - 2026-03-17

### 新功能
- `baoyu-format-markdown`：通过基于公式的推荐和直白备选方案优化标题生成
- `baoyu-format-markdown`：在 frontmatter 中自动生成双摘要（`summary` + `description`）

## 1.69.1 - 2026-03-16

### 修复
- `baoyu-chrome-cdp`：收紧 Chrome 自动连接逻辑，减少误报

## 1.69.0 - 2026-03-16

### 新功能
- `baoyu-chrome-cdp`：支持连接到已有的 Chrome 会话（by @bviews）

### 修复
- `baoyu-chrome-cdp`：支持 Chrome 146 原生远程调试审批模式（by @bviews）
- `baoyu-chrome-cdp`：在 findExistingChromeDebugPort 中保持 HTTP 验证（by @bviews）
- `baoyu-danger-gemini-web`：复用 openPageSession 并修复孤立标签页泄漏（by @bviews）
- `baoyu-danger-gemini-web`：优先使用显式配置文件配置而非自动发现（by @bviews）
- `baoyu-danger-gemini-web`：在自动发现跳过中遵守 BAOYU_CHROME_PROFILE_DIR（by @bviews）
- `baoyu-post-to-wechat`：提高浏览器发布可靠性（by @cfh-7598）

### 文档
- `baoyu-cover-image`：澄清人物参考图片工作流和交互式确认

## 1.68.0 - 2026-03-14

### 新功能
- `baoyu-article-illustrator`：添加可配置输出目录（`default_output_dir`），支持 4 个选项 — `imgs-subdir`、`same-dir`、`illustrations-subdir`、`independent`
- `baoyu-cover-image`：添加参考图片中的角色保留功能 — 使用 `usage: direct` 将人物参考传递给模型以生成风格化肖像

## 1.67.0 - 2026-03-13

### 新功能
- `baoyu-image-gen`：为 DashScope 提供商添加 qwen-image-2.0-pro 模型支持，支持自由尺寸和文字渲染（by @JianJang2017）

## 1.66.1 - 2026-03-13

### 测试
- 将测试文件从集中的 `tests/` 目录迁移到与源代码同位
- 将测试从 `.mjs` 转换为 TypeScript（`.test.ts`）并使用 `tsx` 运行器
- 在 CI 工作流中添加 npm workspaces 配置和 npm 缓存

## 1.66.0 - 2026-03-13

### 新功能
- `baoyu-image-gen`：添加即梦（Jimeng）和豆包 Seedream 图像生成提供商（by @lindaifeng）

### 修复
- `baoyu-image-gen`：收紧即梦提供商行为

### 重构
- `baoyu-image-gen`：导出函数以提高可测试性，添加模块入口守卫

### 文档
- `baoyu-image-gen`：在 SKILL.md 和 README 中添加即梦和 Seedream 提供商文档

### 测试
- 添加测试基础设施、CI 工作流和 image-gen 单元测试

## 1.65.1 - 2026-03-13

### 重构
- `baoyu-translate`：用 markdown-it 替换 remark/unified 进行分块解析，添加 main.ts CLI 入口

## 1.65.0 - 2026-03-13

### 新功能
- `baoyu-post-to-wechat`：添加占位图片上传支持，支持 Markdown 嵌入图片去重

### 修复
- `baoyu-post-to-wechat`：修复 frontmatter 解析以允许前导空白和可选的尾部换行

### 重构
- `baoyu-post-to-wechat`：用 `renderMarkdownWithPlaceholders` 替换 `renderMarkdownToHtml` 以获得结构化输出

## 1.64.0 - 2026-03-13

### 新功能
- `baoyu-image-gen`：添加 OpenRouter 提供商，支持图像生成、参考图片和可配置模型

## 1.63.0 - 2026-03-13

### 新功能
- `baoyu-url-to-markdown`：当本地浏览器捕获失败时添加托管 `defuddle.md` API 回退
- `baoyu-url-to-markdown`：将 YouTube 转录/字幕文本提取到 Markdown 输出中
- `baoyu-url-to-markdown`：将 Shadow DOM 内容实体化以改善 Web 组件页面转换
- `baoyu-url-to-markdown`：在可用时将语言提示包含在 Markdown front matter 中

### 重构
- `baoyu-url-to-markdown`：将单体转换器拆分为 defuddle、legacy 和 shared 模块

### 文档
- 修复 README 中 Claude Code 插件市场仓库的大小写

## 1.62.0 - 2026-03-12

### 新功能
- `baoyu-infographic`：支持灵活的宽高比，除命名预设外还支持自定义 W:H 值（如 3:4、4:3、2.35:1）

### 修复
- 在插件上设置严格模式以防止重复的斜杠命令

### 文档
- `baoyu-post-to-wechat`：替换类似凭据的占位符

## 1.61.0 - 2026-03-11

### 新功能
- `baoyu-post-to-wechat`：添加多账号支持，包括 `--account` CLI 参数、EXTEND.md accounts 配置块、隔离的 Chrome 配置文件和凭据解析链

### 修复
- 从技能发布文件中排除 `out/dist/build` 目录和 `bun.lockb`
- 使用正确的 MIME 类型进行技能发布，修复 ClawHub 拒绝问题

## 1.60.0 - 2026-03-11

### 新功能
- `baoyu-url-to-markdown`：支持复用已有的 Chrome CDP 实例并修复端口检测顺序

### 修复
- `baoyu-post-to-x`：添加缺失的 `fs` 导入到 x-article

### 重构
- 统一所有 CDP 技能使用共享的 `baoyu-chrome-cdp` 包及 vendor 副本
- 简化 CLAUDE.md，将详细文档移至 `docs/` 目录
- 从同步后的 vendor 直接发布技能，移除单独的构建物准备步骤

## 1.59.1 - 2026-03-11

### 修复
- `baoyu-translate`：改进短文本注释密度规则，在 02-prompt.md 中添加显式风格预设传递
- `baoyu-post-to-x`：移除 `--disable-blink-features=AutomationControlled` Chrome 标志

### 重构
- `baoyu-post-to-weibo`：在 md-to-html.ts 中添加入口守卫以兼容模块导入
- 用本地 sync-clawhub.mjs 脚本替换 clawhub CLI

### 文档
- 更新 CLAUDE.md 以反映 v1.59.0 代码库状态（by @jackL1020）

## 1.59.0 - 2026-03-09

### 新功能
- `baoyu-image-gen`：添加批量并行图像生成和提供商级别的节流（by @SeamoonAO）

### 修复
- `baoyu-image-gen`：在多个密钥可用时恢复 Google 作为默认提供商

### 文档
- 改进技能文档清晰度（by @SeamoonAO）

## 1.58.0 - 2026-03-08

### 新功能
- 为 EXTEND.md 添加 XDG 配置路径支持（by @liby）

### 修复
- `baoyu-post-to-wechat`：暴露 agent-browser 启动错误
- `baoyu-post-to-wechat`：加固 agent-browser 命令和 eval 处理（by @luojiyin1987）
- `baoyu-image-gen`：Google curl 请求使用 execFileSync（by @luojiyin1987）
- `baoyu-format-markdown`：autocorrect 命令使用 spawnSync（by @luojiyin1987）

### 文档
- 修复 CLAUDE 依赖说明（by @luojiyin1987）
- 在 README 工具技能中添加 markdown-to-html（by @luojiyin1987）

## 1.57.0 - 2026-03-08

### 新功能
- 添加 ClawHub/OpenClaw 发布支持，包含同步脚本和 README 文档

### 重构
- 为所有技能 frontmatter 添加 openclaw 元数据以兼容 ClawHub 注册表
- 在所有技能中将 `SKILL_DIR` 重命名为 `baseDir` 以保持一致性
- `baoyu-danger-gemini-web`、`baoyu-danger-x-to-markdown`：使用动态脚本路径显示
- `baoyu-comic`、`baoyu-xhs-images`：使用技能接口而非直接脚本调用进行图像生成

## 1.56.1 - 2026-03-08

### 修复
- `baoyu-post-to-weibo`：简化文章图片插入，使用基于退格键的占位符删除以兼容 ProseMirror

## 1.56.0 - 2026-03-08

### 新功能
- `baoyu-article-illustrator`：预设优先的选择流程，按内容类型分类的风格预设
- `baoyu-xhs-images`：从 6 步精简到 4 步工作流，支持智能确认（快速/自定义/详细路径）

### 修复
- `baoyu-post-to-wechat`：通过文件选择器拦截和回退提高图片上传可靠性

## 1.55.0 - 2026-03-08

### 新功能
- `baoyu-article-illustrator`：添加丝网印刷风格和 `--preset` 标志，支持快速类型+风格选择
- `baoyu-cover-image`：添加丝网印刷渲染和双色调调色板，包含 5 个新风格预设
- `baoyu-xhs-images`：添加丝网印刷风格和 `--preset` 标志，包含 23 个内置预设

### 文档
- 在两个 README 中添加致谢部分，感谢开源灵感来源

## 1.54.1 - 2026-03-07

### 修复
- `baoyu-post-to-x`：保持已编写的帖子在 Chrome 中打开，以便用户审核和手动发布

### 文档
- `baoyu-post-to-x`：记录默认帖子类型选择和手动发布流程
- `README`：在英文和中文 README 中添加 Star History 图表

## 1.54.0 - 2026-03-06

### 新功能
- `baoyu-format-markdown`：改进标题和摘要生成，支持风格区分候选项、禁用模式和钩子优先原则
- `baoyu-markdown-to-html`：添加 `--cite` 选项，将普通外部链接转换为编号底部引用
- `baoyu-post-to-wechat`：Markdown 输入默认启用底部引用，添加 `--no-cite` 标志禁用
- `baoyu-translate`：通过 EXTEND.md 中的 `glossary_files` 支持外部术语表（Markdown 表格或 YAML）
- `baoyu-translate`：添加 frontmatter 转换规则，用 `source` 前缀重命名源元数据字段

## 1.53.0 - 2026-03-06

### 新功能
- `baoyu-url-to-markdown`：将渲染的 HTML 快照保存为 `-captured.html` 文件，与 Markdown 输出并列
- `baoyu-url-to-markdown`：Defuddle 优先的 Markdown 转换，自动回退到旧版 Readability/选择器提取器

## 1.52.0 - 2026-03-06

### 新功能
- `baoyu-post-to-weibo`：通过 `--video` 标志添加视频上传支持（总计最多 18 个文件）
- `baoyu-post-to-weibo`：从剪贴板粘贴切换到 `DOM.setFileInputFiles` 以实现更可靠的上传

### 修复
- `baoyu-post-to-weibo`：添加 Chrome 健康检查，自动重启无响应的实例
- `baoyu-post-to-weibo`：添加导航检查以确保在发布前处于微博主页

## 1.51.2 - 2026-03-06

### 修复
- `release-skills`：用通用模式替换显式语言文件名模式（如 `CHANGELOG.de.md`），避免 Gen Agent Trust Hub URL 扫描器误报
- `baoyu-infographic`：添加凭据/密钥剥离说明，以解决 Snyk W007 不安全凭据处理审计问题

## 1.51.1 - 2026-03-06

### 重构
- 统一 Chrome CDP 配置文件路径 — 所有技能现在共享 `baoyu-skills/chrome-profile` 而非每个技能单独的目录
- 修复 `baoyu-post-to-weibo` 错误复用 `x-browser-profile` 路径

### 修复
- 从所有安装说明中移除 `curl | bash` 远程代码执行模式
- 在 `md-to-html` 脚本中强制仅使用 HTTPS 下载远程图片
- 添加重定向限制（最多 5 次）以防止无限重定向循环
- 在 CLAUDE.md 中添加安全指南部分

## 1.51.0 - 2026-03-06

### 新功能
- `baoyu-post-to-weibo`：新技能，用于发布微博内容 — 支持带图片的文本帖子和头条文章，通过 Chrome CDP 实现
- `baoyu-format-markdown`：添加标题/摘要多候选项选择 — 生成 3 个候选项供用户选择，支持 EXTEND.md 的 `auto_select`

## 1.50.0 - 2026-03-06

### 新功能
- `baoyu-translate`：将翻译风格预设从 4 个扩展到 9 个 — 添加学术、商务、幽默、对话和优雅风格
- `baoyu-translate`：添加 `--style` CLI 标志用于每次调用时覆盖风格
- `baoyu-translate`：将风格指令集成到子代理提示模板中

## 1.49.0 - 2026-03-06

### 新功能
- `baoyu-format-markdown`：添加读者视角内容分析阶段 — 在应用格式化前分析亮点、结构和格式问题
- `baoyu-format-markdown`：将工作流从 8 步重构为 7 步，包含明确的格式化原则（做/不做）和完成报告
- `baoyu-translate`：将步骤 2 的工作流机制提取到单独的参考文件中，使 SKILL.md 更简洁
- `baoyu-translate`：扩展触发关键词（改成中文、快翻、本地化等）以更好地激活技能
- `baoyu-translate`：为快速模式添加长内容主动警告
- `baoyu-translate`：分块时将 frontmatter 保存到 `chunks/frontmatter.md`

## 1.48.2 - 2026-03-06

### 新功能
- `baoyu-translate`：在精细工作流的批评和修订阶段添加比喻语言和情感忠实度审查步骤
- `baoyu-translate`：增强快速模式，强制执行意义优先的比喻语言翻译原则

## 1.48.1 - 2026-03-05

### 新功能
- `baoyu-translate`：在分析步骤中添加比喻语言和隐喻映射 — 翻译前先解读隐喻、惯用语和隐含含义，而非逐字翻译
- `baoyu-translate`：在 SKILL.md、精细工作流和子代理提示模板中添加"意义优先"、"比喻语言"和"情感忠实度"翻译原则

## 1.48.0 - 2026-03-05

### 新功能
- `baoyu-translate`：为 chunk.ts 添加 `--output-dir` 选项 — 分块现在写入翻译输出目录而非源文件目录
- `baoyu-translate`：改进精细工作流 — 将审查拆分为严格审查+修订（5→6 步），为中日韩目标语言添加欧化语言诊断

## 1.47.0 - 2026-03-05

### 新功能
- 添加 `baoyu-translate` 技能 — 三模式翻译（快速/普通/精细），支持自定义术语表、受众感知翻译和长文档并行分块翻译
- 为所有技能的 EXTEND.md 偏好检查添加跨平台 PowerShell 支持

## 1.46.0 - 2026-03-05

### 新功能
- 为 url-to-markdown 添加 `--output-dir` 选项，支持自定义输出目录和自动生成文件名

## 1.45.1 - 2026-03-05

### 重构
- 将所有技能中硬编码的 `npx -y bun` 替换为 `${BUN_X}` 运行时变量 — 优先使用原生 `bun`，回退到 `npx -y bun`
- 在 CLAUDE.md 中添加运行时检测部分，在所有 SKILL.md 文件中添加脚本目录说明

## 1.45.0 - 2026-03-05

### 新功能
- `baoyu-post-to-x`：为 X Articles 添加发布后验证 — 自动检查所有图片插入后的剩余占位符和图片数量
- `baoyu-post-to-x`：将 CDP 超时增加到 60 秒，在长文章的图片插入之间添加 3 秒 DOM 稳定延迟

## 1.44.0 - 2026-03-05

### 新功能
- `baoyu-url-to-markdown`：添加 `--download-media` 标志，将图片和视频下载到本地目录，并重写 Markdown 链接为本地路径
- `baoyu-url-to-markdown`：从页面 meta（og:image）提取封面图片到 YAML front matter 的 `coverImage` 字段
- `baoyu-url-to-markdown`：处理微信等站点的 `data-src` 懒加载
- `baoyu-url-to-markdown`：添加 EXTEND.md 偏好设置和首次配置媒体下载行为

## 1.43.2 - 2026-03-05

### 重构
- `baoyu-url-to-markdown`：用 defuddle 库替换自定义 HTML 提取（linkedom + Readability + Turndown），实现更简洁的内容提取和 Markdown 转换

## 1.43.1 - 2026-03-02

### 新功能
- `baoyu-post-to-x`：自动检测 WSL 环境并将 Chrome 配置文件解析为 Windows 原生路径，确保登录持久性
- `baoyu-post-to-wechat`：自动检测 WSL 环境并将 Chrome 配置文件解析为 Windows 原生路径，确保登录持久性
- `baoyu-danger-gemini-web`：WSL 自动检测 Chrome 配置文件路径；添加 `GEMINI_WEB_DEBUG_PORT` 环境变量支持固定调试端口
- `baoyu-danger-x-to-markdown`：WSL 自动检测 Chrome 配置文件路径；添加 `X_DEBUG_PORT` 环境变量支持固定调试端口

## 1.43.0 - 2026-03-02

### 新功能
- `baoyu-post-to-wechat`：支持通过环境变量覆盖浏览器调试端口（`WECHAT_BROWSER_DEBUG_PORT`）和配置文件目录（`WECHAT_BROWSER_PROFILE_DIR`）
- `baoyu-post-to-x`：支持通过环境变量覆盖浏览器调试端口（`X_BROWSER_DEBUG_PORT`）和配置文件目录（`X_BROWSER_PROFILE_DIR`）

## 1.42.3 - 2026-03-02

### 修复
- `baoyu-image-gen`：使用标准尺寸预设进行 DashScope 宽高比映射，而非自由计算

## 1.42.2 - 2026-03-01

### 新功能
- `baoyu-markdown-to-html`：内联渲染管道（无子进程），修复 CJK 强调顺序，增强 modern 主题的 GFM alerts 和改进排版
- `baoyu-post-to-wechat`：使用模块化渲染器内置 Markdown 转换，添加颜色支持，简化发布工作流

## 1.42.1 - 2026-02-28

### 新功能
- `baoyu-markdown-to-html`：将 render.ts 模块化为 cli、constants、extend-config、html-builder、renderer、themes 和 types 模块；在本地打包代码高亮主题

## 1.42.0 - 2026-02-28

### 新功能
- `baoyu-markdown-to-html`：将 heritage 和 warm 合并为单一 modern 主题，为每个主题添加默认颜色（default→blue、grace→purple、simple→green、modern→orange）
- `baoyu-post-to-wechat`：在 EXTEND.md 中添加默认颜色偏好支持，在首次配置中添加 modern 主题选项

## 1.41.0 - 2026-02-28

### 新功能
- `baoyu-markdown-to-html`：重命名主题（red→heritage、orange→warm），添加 13 个命名颜色预设、serif-cjk 字体系列和每个主题的样式默认值

## 1.40.1 - 2026-02-28

### 新功能
- `baoyu-image-gen`：澄清模型解析优先级（EXTEND.md 覆盖环境变量），在生成过程中显示当前模型和切换提示

## 1.40.0 - 2026-02-28

### 新功能
- `baoyu-image-gen`：支持 OpenAI chat completions 端点进行图像生成（by @zhao-newname）
- `baoyu-markdown-to-html`：添加 CLI 自定义选项（--color、--font-family、--font-size、--code-theme、--mac-code-block、--line-number、--cite、--count、--legend）和 EXTEND.md 配置支持

## 1.39.0 - 2026-02-28

### 新功能
- `baoyu-markdown-to-html`：添加 red 主题（传统书法风格，红金配色和衬线排版）和 orange 主题（温暖现代风格，圆角和宽松行高）

## 1.38.0 - 2026-02-28

### 新功能
- `baoyu-danger-x-to-markdown`：将文章中嵌入的推文渲染为带作者信息和文本摘要的引用块
- `baoyu-danger-x-to-markdown`：当 `--download-media` 针对已转换的 URL 时复用已有的 Markdown
- `baoyu-danger-x-to-markdown`：将 Twitter 图片下载升级到 4096x4096 高分辨率

### 修复
- `baoyu-danger-x-to-markdown`：改进实体解析，使用逻辑键查找以实现可靠的媒体和链接映射
- `baoyu-danger-x-to-markdown`：支持所有块类型（标题、列表、引用块）的尾部媒体

## 1.37.1 - 2026-02-27

### 修复
- `baoyu-danger-gemini-web`：同步模型头部与上游并更新模型列表（by @xkcoding）

## 1.37.0 - 2026-02-27

### 新功能
- `baoyu-danger-x-to-markdown`：为 X 文章内容添加内联链接渲染，将 LINK/MEDIA 实体映射为 Markdown 链接
- `baoyu-danger-x-to-markdown`：在输出目录路径中使用基于内容的 slug，实现有意义的文件夹名称
- `baoyu-danger-x-to-markdown`：为没有直接媒体引用的块添加原子媒体队列

## 1.36.0 - 2026-02-27

### 新功能
- `baoyu-image-gen`：添加 `gemini-3.1-flash-image-preview` 模型支持，用于 Google 多模态图像生成
- `baoyu-image-gen`：改进首次配置，支持阻塞式偏好设置流程和引导式配置

### 修复
- `baoyu-image-gen`：当检测到 HTTP 代理时使用 curl 回退调用 Google API（by @liye71023326）

## 1.35.0 - 2026-02-24

### 新功能
- `baoyu-image-gen`：添加 Replicate 提供商支持，支持可配置模型（by @justnode）
- `baoyu-infographic`：添加 `dense-modules` 布局和 3 个新风格（`morandi-journal`、`pop-laboratory`、`retro-pop-grid`）用于高密度信息图。添加关键词快捷方式自动选择。提示词致谢：[AJ](https://waytoagi.feishu.cn/wiki/YG0zwalijihRREkgmPzcWRInnUg)

### 文档
- `baoyu-image-gen`：添加 Replicate 模型配置文档

## 1.34.2 - 2026-02-25

### 文档
- `baoyu-markdown-to-html`：澄清主题解析顺序，包括本地和跨技能 EXTEND.md 回退，在提示用户前优先使用
- `baoyu-post-to-wechat`：对齐 Markdown 转换主题处理，使用确定性回退（`CLI --theme` -> EXTEND.md `default_theme` -> `default`），要求显式 `--theme` 参数

## 1.34.1 - 2026-02-20

### 修复
- `baoyu-post-to-wechat`：修复上传进度检查在第二次迭代时崩溃（by @LyInfi）

## 1.34.0 - 2026-02-17

### 新功能
- `baoyu-xhs-images`：添加参考图片链以实现多图系列的视觉一致性（by @jeffrey94）

### 重构
- `baoyu-article-illustrator`：将提示文件创建强制为图像生成前的阻塞步骤，添加结构化提示质量要求（ZONES / LABELS / COLORS / STYLE / ASPECT）和验证清单

## 1.33.1 - 2026-02-14

### 重构
- `baoyu-post-to-x`：用 marked 生态系统替换手写的 Markdown 解析器，用于 X Articles HTML 转换

### 文档
- `baoyu-post-to-x`：从所有脚本中移除 `--submit` 标志；澄清脚本仅将内容填入浏览器供手动审核和发布

## 1.33.0 - 2026-02-13

### 新功能
- `baoyu-post-to-x`：添加发布前环境检查脚本（`check-paste-permissions.ts`）；添加 Chrome 调试端口冲突的故障排除部分；用最长 15 秒的图片上传验证轮询替换固定等待
- `baoyu-post-to-wechat`：添加发布前环境检查脚本（`check-permissions.ts`），覆盖 Chrome、配置文件隔离、Bun、辅助功能、剪贴板、粘贴按键、API 凭据

## 1.32.0 - 2026-02-12

### 新功能
- `baoyu-danger-x-to-markdown`：添加 `--download-media` 标志，将图片/视频下载到本地并重写 Markdown 链接为相对路径；添加媒体本地化模块；添加 EXTEND.md 偏好设置的首次配置；在 frontmatter 输出中添加 `coverImage`

### 重构
- `baoyu-danger-x-to-markdown`：frontmatter 键使用 camelCase（`tweetCount`、`coverImage`、`requestedUrl` 等）
- `baoyu-format-markdown`：将 `featureImage` 重命名为 `coverImage` 作为主要 frontmatter 键（`featureImage` 作为兼容别名）
- `baoyu-post-to-wechat`：在封面图片 frontmatter 查找中优先使用 `coverImage` 而非 `featureImage`

## 1.31.2 - 2026-02-10

### 修复
- `baoyu-post-to-wechat`：修复 Windows 上 PowerShell 剪贴板复制失败，原因是 `param()`/`-Path` 与 `-Command` 不兼容
- `baoyu-post-to-x`：修复 Windows 上 PowerShell 剪贴板复制（同一问题）；修复 `getScriptDir()` 在 Windows 上返回无效路径（`/C:/...` 前缀）

## 1.31.1 - 2026-02-10

### 新功能
- `baoyu-post-to-wechat`：适配微信新 UI — 将"图文"重命名为"贴图"；添加 ProseMirror 编辑器支持和旧编辑器回退；添加备用文件输入选择器；添加上传进度监控；改进保存按钮检测和 toast 验证

### 修复
- `baoyu-post-to-wechat`：在标点边界处截断超过 120 字符的摘要；修复封面图片相对路径解析
- `baoyu-post-to-x`：通过 `open -na` 修复 macOS 上的 Chrome 启动；修复封面图片相对路径解析

## 1.31.0 - 2026-02-07

### 新功能
- `baoyu-post-to-wechat`：添加评论控制设置（`need_open_comment`、`only_fans_can_comment`）；添加封面图片回退链（CLI → frontmatter → `imgs/cover.png` → 第一张内联图片）；添加作者解析优先级；添加 EXTEND.md 偏好设置的首次配置流程

## 1.30.3 - 2026-02-06

### 重构
- `baoyu-article-illustrator`：将 SKILL.md 从 197 行优化到 150 行（减少 24%）；采用渐进式披露模式，提供简洁概述和详细参考

## 1.30.2 - 2026-02-06

### 重构
- `baoyu-cover-image`：将 SKILL.md 从 532 行优化到 233 行（减少 56%）；将参考图片处理提取到 `references/workflow/reference-images.md`；将图库压缩为仅包含值的表格和链接

## 1.30.1 - 2026-02-06

### 新功能
- `baoyu-image-gen`：添加 OpenAI GPT Image edits 对参考图片的支持（`--ref`）；提供参考图片时自动选择 Google 或 OpenAI

### 修复
- `baoyu-image-gen`：将参考图片相关警告改为显式错误并提供修复提示；添加参考图片验证
- `baoyu-cover-image`：增强参考图片分析，使用深度提取模板；要求 MUST INCORPORATE 部分包含具体视觉元素

## 1.30.0 - 2026-02-06

### 新功能
- `baoyu-cover-image`：添加字体维度，包含 4 种排版风格（clean、handwritten、serif、display）；包括自动选择规则、兼容性矩阵和 `warm-flat` 风格预设

## 1.29.0 - 2026-02-06

### 新功能
- `baoyu-image-gen`：添加 EXTEND.md 配置支持，包括 schema 文档和脚本中的运行时偏好加载（by @kingdomad）

### 修复
- `baoyu-post-to-wechat`：修复微信文章发布中的标题重复和有序列表编号问题（by @NantesCheval）
- `baoyu-url-to-markdown`：用多策略内容提取和 Turndown 转换替换纯正则转换；改进 Substack 风格页面的噪声过滤

## 1.28.4 - 2026-02-03

### 新功能
- `baoyu-markdown-to-html`：从 YAML frontmatter 在生成的 HTML 中添加 author 和 description meta 标签；剥离 frontmatter 值中的引号（支持中英文引号）

### 修复
- `baoyu-post-to-wechat`：移除图片粘贴后的多余空行；修复摘要字段时序，在内容粘贴后填写（防止被覆盖）

## 1.28.3 - 2026-02-03

### 修复
- `baoyu-post-to-wechat`：修复占位符匹配问题，`WECHATIMGPH_1` 错误匹配 `WECHATIMGPH_10`

## 1.28.2 - 2026-02-03

### 修复
- `baoyu-post-to-x`：复用已有的 Chrome 实例；修复占位符匹配问题，`XIMGPH_1` 错误匹配 `XIMGPH_10`；改进按占位符索引排序图片；使用 `execCommand` 实现更可靠的占位符删除

## 1.28.1 - 2026-02-02

### 重构
- `baoyu-article-illustrator`：通过将详细流程提取到 `workflow.md` 简化主 SKILL.md；添加核心风格层级（vector、minimal-flat、sci-fi、hand-drawn、editorial、scene）供快速选择；添加 `vector-illustration` 作为推荐默认风格；添加插图用途（信息/可视化/想象）以提供更好的类型/风格建议；在提示构建中添加默认构图要求、角色渲染指南和文字样式规则

## 1.28.0 - 2026-02-01

### 新功能
- `baoyu-cover-image`：添加参考图片支持（`--ref` 参数），支持 direct/style/palette 用途类型；添加按主题分类的图标词汇视觉元素库
- `baoyu-article-illustrator`：添加参考图片支持，支持 direct/style/palette 用途类型
- `baoyu-post-to-wechat`：添加 `newspic` 文章类型用于图文帖子

### 重构
- `baoyu-cover-image`、`baoyu-article-illustrator`、`baoyu-comic`、`baoyu-xhs-images`：将首次配置强制为任何其他工作流步骤前的阻塞操作
- `baoyu-cover-image`：移除标题字符限制，使用原始来源标题

## 1.26.1 - 2026-01-29

### 新功能
- `baoyu-article-illustrator`、`baoyu-comic`、`baoyu-cover-image`、`baoyu-infographic`、`baoyu-slide-deck`、`baoyu-xhs-images`：添加已有文件备份规则 — 在覆盖前自动以时间戳后缀重命名源文件、提示文件和图片文件

### 修复
- `baoyu-xhs-images`：移除 `notebook` 风格（剩余 10 个风格）

## 1.26.0 - 2026-01-29

### 新功能
- `baoyu-xhs-images`：添加 `notebook` 风格（手绘信息图，水彩渲染和莫兰迪配色）和 `study-notes` 风格（仿真手写照片美学）
- `baoyu-xhs-images`：添加 `mindmap`（中心辐射）和 `quadrant`（四象限网格）布局

## 1.25.4 - 2026-01-29

### 修复
- `baoyu-markdown-to-html`：生成带有 `data-local-path` 属性的正确 `<img>` 标签，而非文本占位符
- `baoyu-post-to-wechat`：修复 API 发布以从 `data-local-path` 属性读取图片路径；修复发布 HTML 文件时从对应 `.md` frontmatter 提取标题/封面
- `baoyu-post-to-wechat`：修复 CLI 参数解析以优雅处理未知参数；添加 `--summary` 参数支持
- `baoyu-post-to-wechat`：修复浏览器发布以在粘贴前将 `<img>` 标签转换回文本占位符

## 1.25.3 - 2026-01-28

### 新功能
- `baoyu-format-markdown`：为 Markdown 文件添加内容类型检测和用户确认；添加 CJK 标点处理，将成对标点移到强调标记之外

## 1.25.2 - 2026-01-28

### 文档
- `baoyu-post-to-wechat`：在 README 中添加微信 API 凭据配置指南

## 1.25.1 - 2026-01-28

### 新功能
- `baoyu-markdown-to-html`：为 CJK 内容添加预检步骤，建议在转换前使用 `baoyu-format-markdown` 格式化

## 1.25.0 - 2026-01-28

### 新功能
- `baoyu-format-markdown`：添加 Markdown 格式化技能，支持 frontmatter、排版和 CJK 间距
- `baoyu-markdown-to-html`：添加 Markdown 到 HTML 转换器，支持微信兼容主题、代码高亮、数学公式、PlantUML 和 alerts
- `baoyu-post-to-wechat`：添加基于 API 的发布方法和外部主题支持

## 1.24.4 - 2026-01-28

### 修复
- `baoyu-post-to-x`：修复封面图片模态框的应用按钮点击；添加重试逻辑和等待模态框关闭

## 1.24.3 - 2026-01-28

### 文档
- 强调在修改工作流中重新生成图片前需要更新提示文件（article-illustrator、slide-deck、xhs-images、cover-image、comic）

## 1.24.2 - 2026-01-28

### 重构
- `baoyu-image-gen`：默认使用顺序生成；按需提供并行生成

## 1.24.1 - 2026-01-28

### 新功能
- `baoyu-image-gen`：添加阿里云通义万相（DashScope）文生图模型支持（by @JianJang2017）

### 文档
- 在 README 中添加阿里云文生图模型配置

## 1.24.0 - 2026-01-27

### 新功能
- `baoyu-post-to-wechat`：复用已有的 Chrome 浏览器而无需关闭所有窗口（by @AliceLJY）

### 修复
- `baoyu-post-to-wechat`：改进标题提取以支持 h1/h2 标题；添加摘要自动填写和粘贴/输入后的内容验证；支持灵活的 HTML meta 标签属性排序

### 文档
- `release-skills`：为更新日志工作流添加第三方贡献者署名规则
- 补充历史更新日志条目中缺失的第三方贡献者署名

## 1.23.1 - 2026-01-27

### 修复
- `baoyu-compress-image`：压缩后将原始文件重命名为 `_original` 备份而非删除

## 1.23.0 - 2026-01-26

### 重构
- `baoyu-cover-image`：用 5 维系统（类型 × 调色板 × 渲染 × 文字 × 情绪）替换 20 个固定风格。9 种调色板 × 6 种渲染风格 = 54 种组合。添加向后兼容的风格预设、v2→v3 schema 迁移和新的参考结构（`palettes/`、`renderings/`、`workflow/`）

## 1.22.0 - 2026-01-25

### 新功能
- `baoyu-article-illustrator`：添加 `imgs-subdir` 输出目录选项；改进风格选择，始终询问并显示 EXTEND.md 中的 preferred_style
- `baoyu-cover-image`：添加 `default_output_dir` 偏好设置，支持 `same-dir`、`imgs-subdir` 和 `independent` 选项，包含步骤 1.5 的输出目录选择
- `baoyu-post-to-wechat`：在发布前添加主题选择（default/grace/simple），使用 AskUserQuestion 询问；添加 HTML 预览步骤；将图片占位符简化为 `WECHATIMGPH_N` 格式；将复制/粘贴重构为跨平台辅助函数

### 重构
- `baoyu-post-to-x`：将图片占位符从 `[[IMAGE_PLACEHOLDER_N]]` 简化为 `XIMGPH_N` 格式

## 1.21.4 - 2026-01-25

### 修复
- `baoyu-post-to-wechat`：添加 Windows 兼容性 — 使用 `fileURLToPath` 实现正确路径解析，用 CDP 键盘事件替换系统相关的复制/粘贴工具（osascript/xdotool）实现跨平台支持（by @JadeLiang003）
- `baoyu-post-to-wechat`：修复 Windows 兼容性 PR 的回归问题 — 修正损坏的 `-fixed` 文件名引用，恢复 frontmatter 引号剥离，恢复 `--title` CLI 参数，修复摘要提取以跳过标题/引用/列表，修复单横杠标志的参数解析，移除调试日志
- `baoyu-article-illustrator`、`baoyu-cover-image`、`baoyu-xhs-images`：从水印配置中移除透明度选项

## 1.21.3 - 2026-01-24

### 重构
- `baoyu-article-illustrator`：通过将内容提取到参考文件来简化 SKILL.md — 添加 `references/usage.md` 用于命令语法，`references/prompt-construction.md` 用于提示模板。将工作流从 5 步重组为 6 步，新增预检阶段。添加 `default_output_dir` 偏好选项

## 1.21.2 - 2026-01-24

### 新功能
- `baoyu-image-gen`：添加并行生成文档，推荐 4 个并发子代理用于批量操作

### 文档
- `release-skills`：添加技能/模块分组工作流和发布前的用户确认步骤

## 1.21.1 - 2026-01-24

### 文档
- `baoyu-comic`：添加角色表生成后的压缩步骤，以减少作为参考图片使用时的 token 用量

## 1.21.0 - 2026-01-24

### 新功能
- `baoyu-cover-image`：扩展宽高比选项 — 添加 4:3、3:2、3:4 比例；将默认值从 2.35:1 改为 16:9 以提高通用性。除非通过 `--aspect` 标志明确指定，否则始终确认宽高比
- `baoyu-image-gen`：重构 Google 提供商以统一 API 支持 Gemini 多模态和 Imagen 模型。添加 `--imageSize` 参数支持（1K/2K/4K）用于 Gemini 模型

## 1.20.0 - 2026-01-24

### 新功能
- `baoyu-cover-image`：从类型 × 风格两维系统升级为 **4 维系统** — 添加 `--text` 维度（none、title-only、title-subtitle、text-rich）用于文字密度控制，`--mood` 维度（subtle、balanced、bold）用于情感强度。新增 `--quick` 标志跳过确认并使用自动选择

### 文档
- `baoyu-cover-image`：添加维度参考文件 — `references/dimensions/text.md`（文字密度级别）和 `references/dimensions/mood.md`（情绪强度级别）
- `baoyu-cover-image`：更新 base-prompt、first-time-setup 和 preferences-schema 以支持新的 4 维系统（v2 schema）
- `README.md`、`README.zh.md`：更新 baoyu-cover-image 文档以反映新的 4 维系统，包含 `--text`、`--mood` 和 `--quick` 选项

## 1.19.0 - 2026-01-24

### 新功能
- `baoyu-comic`：添加部分工作流选项 — `--storyboard-only`、`--prompts-only`、`--images-only` 和 `--regenerate N` 用于灵活的工作流控制
- `baoyu-image-gen`：为 Google 提供商添加 `--imageSize` 参数（1K/2K/4K），将默认质量改为 2k
- `baoyu-image-gen`：添加 `GEMINI_API_KEY` 作为 `GOOGLE_API_KEY` 的别名

### 重构
- `baoyu-comic`：将详细工作流提取到 `references/workflow.md`，减少 SKILL.md 约 400 行同时保留功能
- `baoyu-comic`：将内容信号分析提取到 `references/auto-selection.md`，部分工作流文档提取到 `references/partial-workflows.md`
- `baoyu-image-gen`：模块化代码 — 将类型提取到 `types.ts`，提供商实现提取到 `providers/google.ts` 和 `providers/openai.ts`

### 文档
- `baoyu-comic`：改进 ohmsha 预设文档，包含明确的默认哆啦 A 梦角色定义和视觉描述

## 1.18.3 - 2026-01-23

### 文档
- `baoyu-comic`：改进角色参考处理，包含明确的策略 A/B 选择 — 策略 A 使用 `--ref` 参数用于支持的技能，策略 B 在提示中嵌入角色描述用于不支持的技能。包含两种方法的具体代码示例

### 修复
- `baoyu-image-gen`：从多模态模型列表中移除不支持的 Gemini 模型（`gemini-2.0-flash-exp-image-generation`、`gemini-2.5-flash-preview-native-audio-dialog`）

## 1.18.2 - 2026-01-23

### 重构
- 按照官方最佳实践精简 7 个技能（`baoyu-compress-image`、`baoyu-danger-gemini-web`、`baoyu-danger-x-to-markdown`、`baoyu-image-gen`、`baoyu-post-to-wechat`、`baoyu-post-to-x`、`baoyu-url-to-markdown`）的 SKILL.md 文档 — 总共减少约 300 行同时保留所有功能

### 文档
- `CLAUDE.md`：添加官方技能编写最佳实践链接、技能加载规则、描述编写指南和渐进式披露模式

## 1.18.1 - 2026-01-23

### 文档
- `baoyu-slide-deck`：在进度清单中添加详细子步骤（1.1-1.3），将步骤 1.3 标记为必需并提供显式 Bash 检查命令用于检测已有目录

## 1.18.0 - 2026-01-23

### 新功能
- `baoyu-slide-deck`：引入基于维度的风格系统 — 用模块化 4 维架构替换整体式风格定义：**纹理**（clean、grid、organic、pixel、paper）、**情绪**（professional、warm、cool、vibrant、dark、neutral）、**排版**（geometric、humanist、handwritten、editorial、technical）和**密度**（minimal、balanced、dense）。16 个预设映射到特定维度组合，提供"自定义维度"选项以实现完全灵活性
- `baoyu-slide-deck`：添加两轮确认工作流 — 第一轮询问风格/受众/幻灯片数量/审核偏好，第二轮（可选）在用户选择"自定义维度"时收集自定义维度选择
- `baoyu-slide-deck`：添加条件性大纲和提示审核 — 用户可跳过审核以加快生成速度或启用审核以获得更多控制

### 文档
- `baoyu-slide-deck`：添加维度参考文件 — `references/dimensions/texture.md`、`references/dimensions/mood.md`、`references/dimensions/typography.md`、`references/dimensions/density.md` 和 `references/dimensions/presets.md`（预设→维度映射）
- `baoyu-slide-deck`：添加设计指南 — `references/design-guidelines.md`，包含受众原则、视觉层级、内容密度、颜色选择、排版和字体建议
- `baoyu-slide-deck`：添加布局参考 — `references/layouts.md`，包含布局选项和选择提示
- `baoyu-slide-deck`：添加偏好 schema — `references/config/preferences-schema.md` 用于 EXTEND.md 配置

## 1.17.1 - 2026-01-23

### 重构
- `baoyu-infographic`：简化 SKILL.md 文档 — 移除冗余内容，精简工作流描述，提高可读性
- `baoyu-xhs-images`：改进步骤 0（加载偏好设置）文档 — 添加更清晰的首次配置流程，包含可视化表格和明确的路径检查说明

### 改进
- `baoyu-infographic`：增强 `craft-handmade` 风格，严格执行手绘要求 — 要求所有图像保持卡通/插图美学，不使用写实或照片元素

## 1.17.0 - 2026-01-23

### 新功能
- `baoyu-cover-image`：通过 EXTEND.md 添加用户偏好支持 — 配置水印（内容、位置、透明度）、首选类型/风格、默认宽高比和自定义风格。新增步骤 0 在项目（`.baoyu-skills/`）或用户（`~/.baoyu-skills/`）级别检查偏好设置，包含首次配置流程

### 重构
- `baoyu-cover-image`：重构为类型 × 风格两维系统 — 添加 6 种类型（`hero`、`conceptual`、`typography`、`metaphor`、`scene`、`minimal`）控制视觉构图，20 种风格控制美学。新增 `--type` 和 `--aspect` 选项、类型 × 风格兼容性矩阵，以及带进度清单的结构化工作流

### 文档
- `baoyu-cover-image`：添加三个参考文档 — `references/config/preferences-schema.md`（EXTEND.md YAML schema）、`references/config/first-time-setup.md`（配置流程）、`references/config/watermark-guide.md`（水印配置）
- `README.md`、`README.zh.md`：更新 baoyu-cover-image 文档以反映新的类型 × 风格系统，包含 `--type` 和 `--aspect` 选项

## 1.16.0 - 2026-01-23

### 新功能
- `baoyu-article-illustrator`：通过 EXTEND.md 添加用户偏好支持 — 配置水印（内容、位置、透明度）、首选类型/风格和自定义风格。新增步骤 1.1 在项目（`.baoyu-skills/`）或用户（`~/.baoyu-skills/`）级别检查偏好设置，包含首次配置流程

### 重构
- `baoyu-article-illustrator`：重构为类型 × 风格两维系统 — 用模块化的类型（infographic、scene、flowchart、comparison、framework、timeline）× 风格（notion、elegant、warm、minimal、blueprint、watercolor、editorial、scientific）架构替换 20+ 个单维风格。添加 `--type` 和 `--density` 选项、类型 × 风格兼容性矩阵和结构化提示构建模板

### 文档
- `baoyu-article-illustrator`：添加三个参考文档 — `references/styles.md`（风格画廊和兼容性矩阵）、`references/config/preferences-schema.md`（EXTEND.md YAML schema）、`references/config/first-time-setup.md`（配置流程）
- `README.md`、`README.zh.md`：更新 baoyu-article-illustrator 文档以反映新的类型 × 风格系统，包含 `--type` 和 `--style` 选项

## 1.15.3 - 2026-01-23

### 重构
- `baoyu-comic`：将风格系统重构为 3 维架构 — 用模块化的 `art-styles/`（5 种风格：ligne-claire、manga、realistic、ink-brush、chalk）、`tones/`（7 种氛围：neutral、warm、dramatic、romantic、energetic、vintage、action）和 `presets/`（3 个快捷方式：ohmsha、wuxia、shoujo）替换 10 个整体式风格文件。新的 art × tone × layout 系统支持灵活组合，同时预设为特定类型保留特殊规则

### 文档
- `release-skills`：添加步骤 5（检查 README 更新）— 确保发布期间 README 文档与代码更改保持同步
- `README.md`、`README.zh.md`：更新 baoyu-comic 文档以反映新的 `--art` 和 `--tone` 选项替换 `--style`

## 1.15.2 - 2026-01-23

### 文档
- `release-skills`：全面重写 SKILL.md — 添加多语言更新日志支持、.releaserc.yml 配置、dry-run 模式、语言检测规则，以及 7 种语言的章节标题翻译

## 1.15.1 - 2026-01-22

### 重构
- `baoyu-xhs-images`：将参考文档重构为模块化架构 — 将分散的文件重组到 `config/`（设置）、`elements/`（视觉构建块）、`presets/`（风格定义）和 `workflows/`（流程指南）目录中，提高可维护性

## 1.15.0 - 2026-01-22

### 新功能
- `baoyu-xhs-images`：通过 EXTEND.md 添加用户偏好支持 — 配置水印（内容、位置、透明度）、首选风格、首选布局和自定义风格。新增步骤 0 在项目（`.baoyu-skills/`）或用户（`~/.baoyu-skills/`）级别检查偏好设置，包含首次配置流程

### 文档
- `baoyu-xhs-images`：添加三个参考文档 — `preferences-schema.md`（YAML schema）、`watermark-guide.md`（位置和透明度指南）、`first-time-setup.md`（配置流程）

## 1.14.0 - 2026-01-22

### 修复
- `baoyu-post-to-x`：改进视频就绪检测以实现更可靠的视频发布（by @fkysly）

### 文档
- `baoyu-slide-deck`：全面增强 SKILL.md — 添加幻灯片数量指南（推荐 8-25，最多 30）、受众指南表格和特定受众原则、风格选择原则和内容类型建议、布局选择提示和常见错误、视觉层级原则、内容密度指南（麦肯锡式高密度原则）、颜色选择指南、排版原则和字体建议（英文和中文字体及多语言搭配）和视觉元素参考（背景、排版处理、几何装饰）

## 1.13.0 - 2026-01-21

### 新功能
- `baoyu-url-to-markdown`：新工具技能，通过 Chrome CDP 获取任意 URL 并转换为干净的 Markdown。支持两种捕获模式 — auto（页面加载时立即捕获）和 wait（需要登录的页面由用户控制捕获）

### 改进
- `baoyu-xhs-images`：更新风格推荐 — 将 `tech` 引用替换为 `notion` 和 `chalkboard`，用于技术和教育内容

## 1.12.0 - 2026-01-21

### 新功能
- `baoyu-post-to-x`：添加引用推文支持（by @threehotpot-bot）

### 重构
- `baoyu-post-to-x`：将共享工具提取到 `x-utils.ts` — 将 Chrome 检测、CDP 连接、剪贴板操作和辅助函数从 `x-article.ts`、`x-browser.ts`、`x-quote.ts` 和 `x-video.ts` 合并到单一可复用模块

## 1.11.0 - 2026-01-21

### 新功能
- `baoyu-image-gen`：新的基于 AI SDK 的图像生成技能，使用官方 OpenAI 和 Google API。支持文生图、参考图片（Google 多模态）、宽高比和质量预设（`normal`、`2k`）。根据可用的 API 密钥自动检测提供商
- `baoyu-slide-deck`：添加包含 24 种布局类型的布局画廊 — 10 种幻灯片专用布局（`title-hero`、`quote-callout`、`key-stat`、`split-screen`、`icon-grid`、`two-columns`、`three-columns`、`image-caption`、`agenda`、`bullet-list`）和 14 种信息图衍生布局（`linear-progression`、`binary-comparison`、`comparison-matrix`、`hierarchical-layers`、`hub-spoke`、`bento-grid`、`funnel`、`dashboard`、`venn-diagram`、`circular-flow`、`winding-roadmap`、`tree-branching`、`iceberg`、`bridge`）

### 文档
- `README.md`、`README.zh.md`：添加 baoyu-image-gen 文档，包含使用示例、选项表格和环境变量；添加 API 密钥配置的环境配置部分

## 1.10.0 - 2026-01-21

### 新功能
- `baoyu-post-to-x`：添加视频发布支持 — 新增 `x-video.ts` 脚本用于发布带视频文件（MP4、MOV、WebM）的文本。支持预览模式并处理视频处理超时（by @fkysly）

## 1.9.0 - 2026-01-20

### 新功能
- `baoyu-xhs-images`：添加 `chalkboard` 风格 — 黑色粉笔板背景配彩色粉笔画，用于教育和教程内容
- `baoyu-comic`：添加 `chalkboard` 风格 — 教育粉笔画风格，黑色粉笔板上的教程、讲解和知识漫画

### 改进
- `baoyu-article-illustrator`、`baoyu-cover-image`、`baoyu-infographic`：更新 `chalkboard` 风格，增强视觉指南

### 破坏性变更
- `baoyu-xhs-images`：移除 `tech` 风格（技术内容请使用 `minimal` 或 `notion`）

### 文档
- `README.md`、`README.zh.md`：为 xhs-images 添加风格和布局预览画廊（9 种风格、6 种布局）

## 1.8.0 - 2026-01-20

### 新功能
- `baoyu-infographic`：新技能，用于生成专业信息图，包含 20 种布局类型（bridge、circular-flow、comparison-table、do-dont、equation、feature-list、fishbone、funnel、grid-cards、iceberg、journey-path、layers-stack、mind-map、nested-circles、priority-quadrants、pyramid、scale-balance、timeline-horizontal、tree-hierarchy、venn）和 17 种视觉风格。分析内容、推荐布局 × 风格组合并生成出版级信息图

### 修复
- `baoyu-danger-gemini-web`：改进 Cookie 验证，通过验证实际 Gemini 会话就绪状态而非仅检查 Cookie 存在

## 1.7.0 - 2026-01-19

### 新功能
- `baoyu-comic`：添加 `shoujo` 风格 — 经典少女漫画风格，大而闪亮的眼睛、花朵、光点和柔和的粉色/薰衣草色调。适合爱情、成长、友情和情感剧

## 1.6.0 - 2026-01-19

### 新功能
- `baoyu-cover-image`：添加 `flat-doodle` 风格 — 粗黑轮廓、明亮柔和色彩、简单的扁平形状和可爱圆润比例。适合生产力、SaaS 和工作流内容
- `baoyu-article-illustrator`：添加 `flat-doodle` 风格 — 相同的视觉美学用于文章插图

## 1.5.0 - 2026-01-19

### 新功能
- `baoyu-article-illustrator`：将风格库扩展到 20 种 — 将风格提取到 `references/styles/` 目录并添加 11 种新风格（`blueprint`、`chalkboard`、`editorial`、`fantasy-animation`、`flat`、`intuition-machine`、`pixel-art`、`retro`、`scientific`、`sketch-notes`、`vector-illustration`、`vintage`、`watercolor`）

### 破坏性变更
- `baoyu-article-illustrator`：移除 `tech`、`bold` 和 `isometric` 风格
- `baoyu-cover-image`：移除 `bold` 风格（粗体编辑内容请使用 `bold-editorial`）

### 文档
- `README.md`、`README.zh.md`：为 article-illustrator 添加风格预览画廊（20 种风格）

## 1.4.2 - 2026-01-19

### 文档
- `baoyu-danger-gemini-web`：添加支持的浏览器列表（Chrome、Chromium、Edge）和代理配置指南

## 1.4.1 - 2026-01-18

### 修复
- `baoyu-post-to-x`：支持 X Articles 的多语言 UI 选择器（by @ianchenx）

## 1.4.0 - 2026-01-18

### 新功能
- `baoyu-cover-image`：将风格库从 8 种扩展到 19 种，新增 12 种 — `blueprint`、`bold-editorial`、`chalkboard`、`dark-atmospheric`、`editorial-infographic`、`fantasy-animation`、`intuition-machine`、`notion`、`pixel-art`、`sketch-notes`、`vector-illustration`、`vintage`、`watercolor`
- `baoyu-slide-deck`：添加 `chalkboard` 风格 — 黑色粉笔板背景配彩色粉笔画，用于教育和教程

### 破坏性变更
- `baoyu-cover-image`：移除 `tech` 风格（技术内容请使用 `blueprint` 或 `editorial-infographic`）

### 文档
- `README.md`、`README.zh.md`：更新 cover-image 和 slide-deck 的风格预览截图

## 1.3.0 - 2026-01-18

### 新功能
- `baoyu-comic`：添加 `wuxia` 风格 — 港漫武侠风格，水墨笔触、动态战斗姿势和气功效果。适合武侠/仙侠和中国历史小说
- `baoyu-comic`：在 README 中为所有 8 种风格和 6 种布局添加风格和布局预览截图

### 重构
- `baoyu-comic`：移除 `tech` 风格（技术内容由 `ohmsha` 替代）

## 1.2.0 - 2026-01-18

### 新功能
- 会话独立的输出目录：每次生成会话创建新目录（`<skill-suffix>/<topic-slug>/`），即使针对同一源文件。冲突时追加时间戳
- 多源文件支持：源文件现保存为 `source-{slug}.{ext}`，支持多个输入（文本、图片、对话中的文件）

### 文档
- `CLAUDE.md`：更新输出路径约定，包含新的会话独立目录结构和多源文件命名
- 多个技能：更新文件管理部分以反映新的目录和源文件约定
  - `baoyu-slide-deck`、`baoyu-article-illustrator`、`baoyu-cover-image`、`baoyu-xhs-images`、`baoyu-comic`

## 1.1.0 - 2026-01-18

### 新功能
- `baoyu-compress-image`：新的工具技能，用于跨平台图片压缩。默认转换为 WebP，支持 PNG 到 PNG。使用系统工具（sips、cwebp、ImageMagick）并以 Sharp 作为备选

### 重构
- 插件市场结构：将插件重组为三个类别 — `content-skills`、`ai-generation-skills` 和 `utility-skills` — 以实现更好的组织

### 文档
- `CLAUDE.md`、`README.md`、`README.zh.md`：更新技能架构文档以反映新的三类别结构

## 1.0.1 - 2026-01-18

### 重构
- 改进代码结构以提高可读性和可维护性
- `baoyu-slide-deck`：统一风格参考文件格式

### 其他
- 截图：从 PNG 转换为 WebP 格式以减小文件大小；为新风格添加截图

## 1.0.0 - 2026-01-18

### 新功能
- `baoyu-danger-x-to-markdown`：新技能，将 X/Twitter 帖子和线程转换为 Markdown 格式

### 破坏性变更
- `baoyu-gemini-web` 重命名为 `baoyu-danger-gemini-web`，以标示使用逆向工程 API 的潜在风险

## 0.11.0 - 2026-01-18

### 新功能
- `baoyu-danger-gemini-web`：添加免责声明同意检查流程 — 首次使用前需要用户接受，并在每个平台持久化存储同意状态

## 0.10.0 - 2026-01-18

### 新功能
- `baoyu-slide-deck`：将风格库从 10 种扩展到 15 种，新增 8 种 — `dark-atmospheric`、`editorial-infographic`、`fantasy-animation`、`intuition-machine`、`pixel-art`、`scientific`、`vintage`、`watercolor`

### 破坏性变更
- `baoyu-slide-deck`：移除 3 种风格（`playful`、`storytelling`、`warm`）；将默认风格从 `notion` 改为 `blueprint`

## 0.9.0 - 2026-01-17

### 新功能
- 扩展支持：所有技能现在支持通过 `EXTEND.md` 文件自定义。检查 `.baoyu-skills/<skill-name>/EXTEND.md`（项目级）或 `~/.baoyu-skills/<skill-name>/EXTEND.md`（用户级）以获取自定义风格和配置

### 其他
- `.gitignore`：添加 `.baoyu-skills/` 目录用于用户扩展文件

## 0.8.2 - 2026-01-17

### 重构
- `baoyu-danger-gemini-web`：重组脚本架构 — 将模块化文件移入 `gemini-webapi/` 子目录，更新 SKILL.md 使用 `${SKILL_DIR}` 路径引用

## 0.8.1 - 2026-01-17

### 重构
- `baoyu-danger-gemini-web`：重构脚本架构 — 将 10 个独立文件合并为结构化的 `gemini-webapi/` 模块（gemini_webapi Python 库的 TypeScript 移植版）

## 0.8.0 - 2026-01-17

### 新功能
- `baoyu-xhs-images`：添加内容分析框架（`analysis-framework.md`、`outline-template.md`），用于结构化内容分解和大纲生成

### 文档
- `CLAUDE.md`：添加输出路径约定（目录结构、备份规则）和图像命名约定（格式、slug 规则）以标准化图像生成输出
- 多个技能：更新文件管理约定，使用统一目录结构（`[source-name-no-ext]/<skill-suffix>/`）
  - `baoyu-article-illustrator`、`baoyu-comic`、`baoyu-cover-image`、`baoyu-slide-deck`、`baoyu-xhs-images`

## 0.7.0 - 2026-01-17

### 新功能
- `baoyu-comic`：添加 `--aspect`（3:4、4:3、16:9）和 `--lang` 选项；引入多变体分镜工作流（时间序、主题序、角色序）供用户选择

### 增强
- `baoyu-comic`：添加 `analysis-framework.md` 和 `storyboard-template.md` 用于结构化内容分析和变体生成
- `baoyu-slide-deck`：添加 `analysis-framework.md`、`content-rules.md`、`modification-guide.md` 和 `outline-template.md` 参考以提高大纲质量
- `baoyu-article-illustrator`、`baoyu-cover-image`、`baoyu-xhs-images`：增强 SKILL.md 文档，包含更清晰的工作流

### 文档
- 多个技能：重构 SKILL.md 文件 — 将详细内容移至 `references/` 目录以提高可维护性
- `baoyu-slide-deck`：简化 SKILL.md，合并风格描述

## 0.6.1 - 2026-01-17

- `baoyu-slide-deck`：添加 `scripts/merge-to-pdf.ts` 将生成的幻灯片导出为单个 PDF；文档更新包含 pptx/pdf 输出
- `baoyu-comic`：添加 `scripts/merge-to-pdf.ts` 将封面/页面合并为 PDF；文档澄清角色参考处理（图片 vs 文本）
- 文档约定：在 `CLAUDE.md` 中添加"脚本目录"模板；对齐 `baoyu-danger-gemini-web` / `baoyu-slide-deck` / `baoyu-comic` 文档使用 `${SKILL_DIR}` 以便代理可从任何安装位置运行脚本

## 0.6.0 - 2026-01-17

- `baoyu-slide-deck`：添加 `scripts/merge-to-pptx.ts` 将幻灯片图片合并为 PPTX 并将 `prompts/` 内容附加为演讲者注释
- `baoyu-slide-deck`：重塑/扩展风格库（添加 `blueprint` / `bold-editorial` / `sketch-notes` / `vector-illustration`，调整/替换部分旧风格）
- `baoyu-comic`：添加 `realistic` 风格参考
- 文档：刷新 `README.md` / `README.zh.md`

## 0.5.3 - 2026-01-17

- `baoyu-post-to-x`（X Articles）：使图片占位符替换更可靠（选择重试+验证；通过退格键删除并在粘贴前验证删除），减少错误插入/失败

## 0.5.2 - 2026-01-16

- `baoyu-danger-gemini-web`：添加 `--sessionId`（本地持久化会话，加上 `--list-sessions`）用于多轮对话和一致的多图生成
- `baoyu-danger-gemini-web`：添加 `--reference/--ref` 用于参考图片（视觉输入），增强超时处理和 Cookie 刷新恢复
- 文档：`baoyu-xhs-images` / `baoyu-slide-deck` / `baoyu-comic` 记录会话用法（每组复用同一 `sessionId`）以改善视觉一致性

## 0.5.1 - 2026-01-16

- `baoyu-comic`：添加创作模板/参考（角色模板、Ohmsha 指南、大纲模板）以加速"角色 → 分镜 → 生成"流程

## 0.5.0 - 2026-01-16

- 添加 `baoyu-comic`：知识漫画生成器，支持 `style × layout` 和完整的风格/布局参考集以获得更稳定的输出
- `baoyu-xhs-images`：将风格/布局详情移入 `references/styles/*` 和 `references/layouts/*`，将基础提示迁移到 `references/base-prompt.md` 以便维护/复用
- `baoyu-slide-deck` / `baoyu-cover-image`：类似地将基础提示和风格参考拆分到 `references/`，降低 SKILL.md 复杂度，使风格扩展更容易
- 文档：更新 `README.md` / `README.zh.md` 技能列表和示例

## 0.4.2 - 2026-01-15

- `baoyu-danger-gemini-web`：更新描述，澄清其作为其他技能（如 `cover-image`、`xhs-images`、`article-illustrator`）图像生成后端的定位

## 0.4.1 - 2026-01-15

- `baoyu-post-to-x` / `baoyu-post-to-wechat`：添加 `scripts/paste-from-clipboard.ts` 发送"真实粘贴"按键（Cmd/Ctrl+V），避免网站忽略 CDP 合成事件
- `baoyu-post-to-x`：添加 X Articles/普通帖子文档，图片上传优先使用真实粘贴（带 CDP 回退）
- `baoyu-post-to-wechat`：文档添加脚本位置指导和 `${SKILL_DIR}` 路径用法，确保代理可靠执行
- 文档：添加 `screenshots/update-plugins.png` 用于插件市场更新流程

## 0.4.0 - 2026-01-15

- 为技能目录添加 `baoyu-` 前缀并相应更新插件市场路径/文档，以减少命名冲突

## 0.3.1 - 2026-01-15

- `xhs-images`：升级文档为风格 × 布局系统（添加 `--layout`、自动布局选择和 `notion` 风格），包含更完整的使用示例
- `article-illustrator` / `cover-image`：文档不再硬编码 `gemini-web`；改为指导代理选择可用的图像生成技能
- `slide-deck`：文档添加 `notion` 风格并更新自动风格映射
- 工具/文档：将 `.DS_Store` 添加到 `.gitignore`；刷新 `README.md` / `README.zh.md`

## 0.3.0 - 2026-01-14

- 添加 `post-to-wechat`：Chrome CDP 自动化用于微信公众号发布（图文+完整文章），包括 Markdown → 微信 HTML 转换和多种主题
- 添加 `CLAUDE.md`：仓库结构、运行约定和"添加新技能"指南
- 文档：更新 `README.md` / `README.zh.md` 安装/更新/使用说明

## 0.2.0 - 2026-01-13

- 添加新技能：`post-to-x`（真实 Chrome/CDP 自动化用于帖子和 X Articles）、`article-illustrator`、`cover-image` 和 `slide-deck`
- `xhs-images`：添加多风格支持（`--style`），包含自动风格选择，更新基础提示（如语言跟随输入、手绘信息图约束）
- 文档：添加 `README.zh.md` 并改进 `README.md` 和 `.gitignore`

## 0.1.1 - 2026-01-13

- 插件市场重构：引入 `metadata`（包括 `version`），将插件入口重命名为 `content-skills` 并明确列出可安装的技能；移除旧版 `.claude-plugin/plugin.json`
- 添加 `xhs-images`：小红书信息图系列生成器（大纲+每张图片的提示）
- `gemini-web`：添加 `--promptfiles` 从多个文件构建提示（系统/内容分离）
- 文档：添加 `README.md`

## 0.1.0 - 2026-01-13

- 初始发布：`.claude-plugin/marketplace.json` 加上 `gemini-web`（文本/图像生成，浏览器登录+Cookie 缓存）
