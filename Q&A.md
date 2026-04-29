# Baoyu-Skills Q&A 对话记录

---

## Q1: 把项目下所有的.md文件翻译成一版中文

**问：** 把项目下所有的.md文件翻译成一版中文，在原文件同步录下根上新建一个{原文件名}_cn.md的新文件

**答：**

共找到 235 个 `.md` 文件。其中 `CHANGELOG.zh.md` 和 `README.zh.md` 已经是中文版本，将跳过。剩余 233 个文件需要翻译。

按目录分组并行处理，启动 7 个并行翻译任务：

| 目录 | 文件数 |
|------|--------|
| 顶层（CHANGELOG、CLAUDE、README） | 3 |
| `docs/` | 6 |
| `skills/baoyu-article-illustrator/` | 31 |
| `skills/baoyu-comic/` | 33 |
| `skills/baoyu-compress-image/` | 1 |
| `skills/baoyu-cover-image/` | 33 |
| `skills/baoyu-infographic/` | 45 |
| `skills/baoyu-slide-deck/` | 30 |
| 其他 skills 目录（11 个子目录） | 51 |

**翻译完成：共 233 个 `_cn.md` 中文翻译文件**

命名规则：每个翻译文件都在原文件同目录下，文件名为 `{原文件名}_cn.md`。例如：
- `SKILL.md` → `SKILL_cn.md`
- `docs/chrome-profile.md` → `docs/chrome-profile_cn.md`
- `references/styles/blueprint.md` → `references/styles/blueprint_cn.md`

已跳过的文件：`CHANGELOG.zh.md` 和 `README.zh.md`（已有中文版本）。

---

## Q2: 项目中 .claude-plugin 和 packages 起的作用是什么

**问：** 项目中 .claude-plugin 和 packages起的作用是什么

**答：**

### `.claude-plugin/` 的作用

这个目录是 **Claude Code 插件市场的清单目录**，里面只有一个文件 `marketplace.json`。它定义了整个插件的元数据和 skill 注册信息：

- **插件名称**：`baoyu-skills`
- **版本号**：`1.79.0`
- **三大分类**，每个分类下注册了若干 skill 路径：

| 分组 | 说明 | 包含的 skills |
|------|------|---------------|
| `content-skills` | 内容生成与发布 | xhs-images、post-to-x/wechat/weibo、article-illustrator、cover-image、slide-deck、comic、infographic |
| `ai-generation-skills` | AI 生成后端 | danger-gemini-web、baoyu-image-gen |
| `utility-skills` | 内容处理工具 | danger-x-to-markdown、compress-image、url-to-markdown、format-markdown、markdown-to-html、translate、youtube-transcript |

Claude Code 通过读取这个文件来发现和加载各个 skill。

### `packages/` 的作用

这是通过根目录 `package.json` 中 `"workspaces": ["packages/*"]` 声明的 **npm workspaces 共享库**，供多个 skill 的脚本复用。目前包含 **两个内部包**：

#### 1. `baoyu-md` — Markdown 渲染引擎

核心功能是将 Markdown 转换为 HTML，包括：
- 基于 `marked` 的 Markdown 解析
- `highlight.js` 代码高亮（内置 79 个代码主题 CSS）
- 多种 Marked 扩展：KaTeX 数学公式、目录生成、PlantUML 图表、脚注、Ruby 注音、信息图等
- 4 套文章主题（default、grace、modern、simple）
- CSS 内联（`juice`）、阅读时间计算、中文友好换行等

被 `baoyu-markdown-to-html`、`baoyu-post-to-wechat`、`baoyu-post-to-weibo` 等多个 skill 引用。

#### 2. `baoyu-chrome-cdp` — Chrome 浏览器自动化工具

核心功能是通过 Chrome DevTools Protocol (CDP) 控制浏览器，包括：
- 自动查找 Chrome 可执行文件
- 管理共享 Chrome Profile 目录（可通过 `BAOYU_CHROME_PROFILE_DIR` 环境变量覆盖）
- WebSocket CDP 连接、启动/关闭 Chrome、页面会话管理

被 `baoyu-danger-gemini-web`、`baoyu-post-to-x`、`baoyu-url-to-markdown`、`baoyu-danger-x-to-markdown` 等需要浏览器操作的 skill 引用。

**简单来说**：`.claude-plugin/` 是插件的"目录索引"，告诉 Claude Code 有哪些 skill 可用；`packages/` 是两个底层共享库，分别提供 Markdown 渲染和浏览器自动化能力，供上层各 skill 脚本调用。

---

## Q3: 这个 workspaces 配置是给用户用的吗？共享机制是什么？

**问：** 这是通过根目录 package.json 中 "workspaces": ["packages/*"] 声明的 npm workspaces 共享库，供多个 skill 的脚本复用。目前包含 两个内部包： 这个放到自己的项目中也是这样配置吗？ 比如项目中安装skill，需要这样配置项目@package.json 吗？ 另外， 这里的共享是npm的运行机制还是在skill里有定义

**答：**

### 1. 用户项目中不需要这样配置

根目录的 `package.json` 中的 `workspaces` 配置是**这个插件仓库自身开发维护用的**，不是给安装 skill 的用户用的。用户安装 skill 后不需要也不会接触到这个配置。

### 2. 共享机制：既不是纯 npm workspaces，也不是 skill 定义，而是自定义同步脚本

这是这个项目最巧妙的设计。整个流程分两层：

**开发时（源码层）**：`packages/` 是共享库的唯一源头

```
packages/
├── baoyu-md/          ← Markdown 渲染库（源码）
└── baoyu-chrome-cdp/  ← Chrome CDP 库（源码）
```

**分发时（vendor 层）**：通过 `scripts/sync-shared-skill-packages.mjs` 这个自定义脚本，把 `packages/` 下的源码**复制**到每个需要它的 skill 的 `scripts/vendor/` 目录中。

具体流程如下：

```
sync-shared-skill-packages.mjs 执行时：

1. 扫描 packages/ 下的所有 workspace 包（发现 baoyu-md、baoyu-chrome-cdp）
2. 扫描 skills/*/scripts/package.json，找到依赖了这些包的 skill
3. 把源码整个复制到 skill 的 scripts/vendor/ 目录
4. 把 skill 的 package.json 中的依赖路径改写为 file:./vendor/xxx
5. 运行 bun install 刷新依赖
```

所以你会看到每个 skill 里都有这样的结构：

```
skills/baoyu-markdown-to-html/scripts/
├── package.json          ← "baoyu-md": "file:./vendor/baoyu-md"
├── main.ts
└── vendor/
    └── baoyu-md/         ← 从 packages/baoyu-md 复制过来的完整副本
        ├── package.json
        └── src/
            └── index.ts
```

### 3. 为什么要这样设计？

**目的是让每个 skill 自包含（self-contained）**。

| 层面 | 机制 | 用途 |
|------|------|------|
| `packages/` + root `workspaces` | npm workspaces | 开发时方便统一修改、跑测试 |
| `sync-shared-skill-packages.mjs` | 自定义脚本 | 把共享包复制到各 skill 的 vendor 里 |
| `file:./vendor/xxx` | npm 本地路径依赖 | 让 skill 脱离 workspace 也能独立运行 |

这样设计的好处是：
- **开发者**只需维护 `packages/` 下的一份源码，修改后跑一次 sync 就全部更新
- **skill 分发后**不依赖根目录的 workspace 配置，每个 skill 目录自带所有依赖，可以独立执行
- 用户安装插件后，每个 skill 直接用自己 `vendor/` 里的代码，不需要任何额外配置

**简单说：`workspaces` 是给开发者用的便利工具，`vendor` 复制是给运行时用的分发策略，两者通过 sync 脚本桥接。**

---

## Q4: 帮我整理 skills 中所有的能力和功能

**问：** 帮我整理skills中所有的能力和功能

**答：**

### 一、内容生成与发布（Content Skills）— 9 个

#### 1. `baoyu-xhs-images` — 小红书图片生成
- **功能**：为小红书生成系列信息图卡片（1–10 张）
- **核心能力**：
  - 11 种风格（cute/fresh/warm/bold/minimal/retro/pop/notion/chalkboard/study-notes/screen-print）
  - 8 种布局（sparse/balanced/dense/list/comparison/flow/mindmap/quadrant）
  - 三种大纲策略：故事驱动 / 信息密集 / 视觉优先
  - 预设组合（knowledge-card、poster、ohmsha 等场景）
- **输出**：`xhs-images/{topic}/` 下的分析文件 + 多张 PNG
- **亮点**：封面先出，后续图通过 `--ref` 保持视觉一致性

#### 2. `baoyu-post-to-x` — 发布到 X（Twitter）
- **功能**：通过 Chrome CDP 向 X 发帖
- **支持类型**：
  - 图文帖（最多 4 张图）
  - 视频帖
  - 引用帖
  - 长文 Article（Markdown → 富文本）
- **特点**：真实浏览器操作，绕过反自动化；用户手动点发布确认

#### 3. `baoyu-post-to-wechat` — 发布到微信公众号
- **功能**：微信公众号文章 & 图文消息发布
- **两种通道**：API（AppID/Secret）或 Chrome 浏览器
- **核心能力**：
  - Markdown/HTML/纯文本 → 公众号文章
  - 图文贴图发布（多图）
  - 外链自动转文末引用
  - 多账号支持（`--account <alias>`）
- **配置**：主题/颜色/作者/评论开关等通过 EXTEND.md

#### 4. `baoyu-post-to-weibo` — 发布到微博
- **功能**：微博发帖与头条文章
- **支持**：图片（最多 18 个媒体）、视频、Markdown 长文章
- **依赖**：Chrome CDP + 登录

#### 5. `baoyu-article-illustrator` — 文章配图
- **功能**：分析文章结构，智能标注配图位置，批量生成插图
- **维度**：Type（infographic/scene/flowchart/comparison/framework/timeline）× Style（20+ 风格预设）
- **工作流**：分析 → 确认 → 生成大纲 → 写 prompt → 批量生图
- **亮点**：支持 batch 模式并行生成多张图

#### 6. `baoyu-cover-image` — 文章封面生成
- **功能**：五维定制封面图
- **五个维度**：
  - Type（封面类型）
  - Palette（10 种色板）
  - Rendering（7 种渲染风格：粉笔/数字/扁平矢量/手绘/油画/像素/丝网印刷）
  - Text（标题文字处理）
  - Mood（情绪氛围）
  - 额外：Font（字体）
- **支持比例**：2.35:1、16:9、1:1 等

#### 7. `baoyu-slide-deck` — 幻灯片生成
- **功能**：从内容生成幻灯片图片系列，可合并为 PPTX/PDF
- **风格预设**：blueprint、chalkboard、notion、corporate、minimal、vintage 等 15+ 种
- **选项**：`--slides`（8–30 张）、`--audience`、`--lang`
- **工作流**：分析 → 两轮确认 → 生成大纲 → 逐张生图 → 合并输出
- **输出脚本**：`merge-to-pptx.ts`、`merge-to-pdf.ts`

#### 8. `baoyu-comic` — 知识漫画生成
- **功能**：将知识内容转化为漫画
- **三个维度**：
  - Art（ligne-claire/manga/realistic/ink-brush/chalk）
  - Tone（action/dramatic/energetic/neutral/romantic/vintage/warm）
  - Layout（cinematic/dense/mixed/splash/standard/webtoon）
- **预设**：ohmsha（技术解说）、wuxia（武侠风）、shoujo（少女漫画风）
- **关键**：必须先生成角色参考图 `characters.png`，后续每页通过 `--ref` 保持角色一致

#### 9. `baoyu-infographic` — 信息图生成
- **功能**：单张信息图，忠实还原数据
- **规模**：21 种布局 × 20 种风格
- **布局示例**：bento-grid、hub-spoke、iceberg、periodic-table、funnel、venn-diagram、subway-map 等
- **风格示例**：craft-handmade、cyberpunk-neon、kawaii、pixel-art、origami、technical-schematic 等

### 二、AI 生成后端（AI Generation Skills）— 2 个

#### 10. `baoyu-danger-gemini-web` — Gemini Web 逆向接口
- **功能**：通过逆向 Gemini Web 进行文本/图像生成
- **能力**：多轮对话、参考图输入、会话管理
- **模型**：gemini-3-pro / flash / flash-thinking / 3.1-pro-preview 等
- **注意**：首次使用需同意风险声明（consent.json）；需浏览器 Cookie 认证

#### 11. `baoyu-image-gen` — 官方 API 图像生成
- **功能**：统一调用多家 API 进行文生图
- **支持提供商**：
  - OpenAI（gpt-image-1 / dall-e-3）
  - Google（Imagen 4 等）
  - OpenRouter
  - DashScope（通义万相）
  - Jimeng（即梦）
  - Seedream
  - Replicate
- **核心能力**：
  - `--ref` 参考图生成
  - `--batchfile` 批量 JSON 并行生成
  - 多种比例/尺寸/质量选项
  - 失败自动重试（每图最多 3 次）
- **地位**：作为其他内容生成 skill 的底层图像引擎

### 三、工具类（Utility Skills）— 7 个

#### 12. `baoyu-danger-x-to-markdown` — X 推文转 Markdown
- **功能**：将 X（Twitter）推文/线程/文章转为 Markdown
- **方式**：逆向 API
- **选项**：媒体下载、JSON 输出
- **认证**：`X_AUTH_TOKEN`/`X_CT0` 或 Chrome 登录

#### 13. `baoyu-compress-image` — 图片压缩
- **功能**：图片压缩，默认输出 WebP
- **工具链**：sips → cwebp → ImageMagick → Sharp（自动选择可用工具）
- **选项**：格式（webp/png/jpeg）、质量、递归处理目录

#### 14. `baoyu-url-to-markdown` — 网页转 Markdown
- **功能**：Chrome CDP 抓取网页 → 转为干净的 Markdown
- **能力**：
  - Defuddle 提取正文（失败时回退到 legacy 或托管 API）
  - `--wait` 支持需登录的页面
  - 媒体文件本地化下载
  - YouTube 字幕抓取
- **输出**：Markdown 文件 + 原始 HTML 备份

#### 15. `baoyu-format-markdown` — Markdown 格式化
- **功能**：排版整理，不改写语义
- **能力**：
  - 中英文排版（CJK 友好）
  - 引号规范化
  - 自动纠错
  - 标题公式推荐（多候选让用户选）
- **三种模式**：优化格式 / 保留结构仅排版 / 仅 typography

#### 16. `baoyu-markdown-to-html` — Markdown 转 HTML
- **功能**：Markdown → 带内联 CSS 的 HTML（适配公众号等平台）
- **支持**：
  - 4 种主题（default/grace/simple/modern）
  - 数学公式（KaTeX）
  - PlantUML 图表
  - 脚注、Alert 框、Mermaid 图
  - 外链转文末引用（`--cite`）
- **选项**：`--theme`、`--color`、`--font-family`、`--font-size`、`--title`

#### 17. `baoyu-translate` — 翻译
- **功能**：三种翻译模式
  - **Quick**：快速直译
  - **Normal**：标准翻译
  - **Refined**：精翻（含反思与润色流程）
- **能力**：术语表、受众/风格定制、长文自动分块、子代理并行翻译
- **输出**：翻译文件 + normal/refined 模式的中间过程文件

#### 18. `baoyu-youtube-transcript` — YouTube 字幕下载
- **功能**：无需 API Key 或浏览器，下载 YouTube 字幕与封面
- **能力**：
  - 多语言字幕
  - 自动翻译
  - 章节信息提取
  - 说话人识别（需 AI 后处理）
  - 输出格式：text / srt
- **输出**：`youtube-transcript/{channel}/{title}/` 下的 meta、字幕、封面等

### 整体架构关系

```
用户请求（自然语言）
    │
    ├── 内容创作层 ──→ 小红书/幻灯片/漫画/信息图/封面/配图
    │       │
    │       └── 调用 AI 引擎 → baoyu-image-gen（7 家 API）
    │                        → Gemini Web（逆向接口）
    │
    ├── 发布层 ─────→ X / 微信公众号 / 微博
    │       │
    │       └── 依赖 → markdown-to-html（渲染）
    │                → Chrome CDP（浏览器自动化）
    │
    └── 工具层 ─────→ 翻译 / 格式化 / 压缩 / 网页抓取 / 字幕下载
```

内容生成 skill 是「上层编排者」，它们组织内容分析、大纲生成、prompt 编写等流程，最终调用 `baoyu-image-gen` 或 `baoyu-danger-gemini-web` 作为底层图像引擎完成实际生图。

---

## Q5: 帮我利用 skills 输出写一个使用方案

**问：** 根据以上信息，帮我利用skills输出写一个根据当前BAOYU-SKILLS应用的使用方案，要求简明易懂，并生成配图和封面，用于wechat发布

**答：**

检测到当前环境 `bun` 未安装且 `EXTEND.md`（图像生成 API 配置）不存在，无法直接调用 skill 脚本生成图片。因此：
1. 撰写文章内容
2. 用 Cursor 内置的图像生成工具创建封面和配图
3. 说明如何通过 skill 完成 WeChat 发布流程

文章已写入 `article/baoyu-skills-guide.md`，包含：
1. **封面** — AI 内容工作流视觉（cover.png）
2. **一句话理解** — 快速定位产品价值
3. **能做什么** — 三大类 18 个技能表格（配 skills-overview.png）
4. **三步上手** — 安装→配置→自然语言驱动
5. **实战案例** — 一篇文章六步全平台发布（配 workflow.png）
6. **架构一览** — 三层结构（配 architecture.png）
7. **实用技巧** — 风格一致性、分步执行、多账号、批量处理
8. **环境要求** — Bun + Chrome + API Key

输出文件位于 `article/` 目录下：

| 文件 | 说明 |
|------|------|
| `baoyu-skills-guide.md` | 文章正文（Markdown，含图片引用） |
| `cover.png` | 封面图（16:9，蓝紫渐变 + AI 工作流视觉） |
| `skills-overview.png` | 配图 1：18 个技能卡片总览 |
| `workflow.png` | 配图 2：创作→多平台发布流程图 |
| `architecture.png` | 配图 3：三层架构全景图 |

微信发布后续步骤：

**1. 安装 Bun**
```bash
brew install oven-sh/bun/bun
```

**2. 在 Claude Code 中执行发布流程**
```
你：把 article/baoyu-skills-guide.md 发布到微信公众号

Claude Code 会自动：
→ 检测 EXTEND.md，首次引导配置 API Key
→ baoyu-markdown-to-html 转换为带内联 CSS 的 HTML
→ baoyu-post-to-wechat 通过 Chrome 打开公众号后台
→ 填入内容，等你手动确认发布
```

---

## Q6: bun 是用于什么的

**问：** bun是用于什么的

**答：**

**Bun** 是一个高性能的 JavaScript/TypeScript 运行时，类似于 Node.js，但速度更快。

### 在这个项目中的作用

Baoyu-Skills 的所有 skill 脚本都是用 **TypeScript** 编写的（`.ts` 文件），需要一个运行时来执行。Bun 在这里承担两个角色：

**1. TypeScript 运行时** — 直接运行 `.ts` 文件，无需编译步骤

```bash
bun skills/baoyu-image-gen/scripts/main.ts --prompt "A cat" --image cat.png
```

Node.js 原生不支持直接运行 TypeScript，需要额外的编译工具。Bun 省去了这一步。

**2. 包管理器** — 替代 npm/yarn 安装依赖

每个 skill 的 `scripts/` 目录下有 `package.json`，Bun 用 `bun install` 安装依赖，并生成 `bun.lock` 锁文件。

### 为什么选 Bun 而不是 Node.js

- **原生支持 TypeScript**：无需 `tsc` 编译或 `tsx` 等转译工具
- **启动更快**：skill 脚本通常是短命令，启动速度很关键
- **兼容 npm 生态**：可以直接使用现有的 npm 包

### 没装 Bun 怎么办

项目做了降级处理。`CLAUDE.md` 中定义的运行时检测逻辑：

```bash
if command -v bun &>/dev/null; then BUN_X="bun"        # 优先用 bun
elif command -v npx &>/dev/null; then BUN_X="npx -y bun" # 回退：通过 npx 临时下载 bun 执行
else echo "Error: install bun"                            # 都没有则报错
fi
```

所以即使没有全局安装 Bun，只要有 `npx`（Node.js 自带），也能通过 `npx -y bun` 临时调用。

### 安装方式

```bash
# macOS
brew install oven-sh/bun/bun

# 或通用方式
npm install -g bun
```

**简单说：Bun = 更快的 Node.js + 原生 TypeScript 支持**，是这个项目所有 skill 脚本的执行引擎。

---

## Q7: EXTEND.md 配置写在哪里

**问：** EXTEND.md 配置写在哪里

**答：**

### EXTEND.md 写在哪里？

EXTEND.md 有**两个可选位置**，取决于你想让配置作用于单个项目还是所有项目：

| 存储位置 | 路径 | 作用范围 |
|---------|------|---------|
| **项目级**（推荐） | `项目根目录/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 仅当前项目 |
| **用户级** | `~/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 所有项目通用 |

### 查找优先级

skill 启动时按以下顺序查找，找到即停：

```
1. .baoyu-skills/baoyu-image-gen/EXTEND.md        ← 项目目录（最高优先）
2. ${XDG_CONFIG_HOME:-~/.config}/baoyu-skills/...  ← XDG 配置目录
3. ~/.baoyu-skills/baoyu-image-gen/EXTEND.md       ← 用户 Home 目录
```

### 每个 skill 有自己的 EXTEND.md

不同 skill 的配置是**独立的**，路径中的目录名对应 skill 名称：

```
.baoyu-skills/
├── baoyu-image-gen/EXTEND.md        ← 图像生成配置
├── baoyu-cover-image/EXTEND.md      ← 封面生成配置
├── baoyu-post-to-wechat/EXTEND.md   ← 微信发布配置
├── baoyu-translate/EXTEND.md        ← 翻译配置
├── baoyu-url-to-markdown/EXTEND.md  ← 网页抓取配置
└── ...
```

### EXTEND.md 长什么样？

以 `baoyu-image-gen` 为例，它是一个带 YAML front matter 的 Markdown 文件：

**最简配置**：

```yaml
---
version: 1
default_provider: google
default_quality: 2k
---
```

**完整配置**：

```yaml
---
version: 1
default_provider: google
default_quality: 2k
default_aspect_ratio: "16:9"
default_image_size: 2K
default_model:
  google: "gemini-3-pro-image-preview"
  openai: "gpt-image-1.5"
  openrouter: "google/gemini-3.1-flash-image-preview"
  dashscope: "qwen-image-2.0-pro"
  replicate: "google/nano-banana-pro"
batch:
  max_workers: 10
  provider_limits:
    replicate:
      concurrency: 5
      start_interval_ms: 700
---
```

### 不需要手动创建

正常使用时**不需要手动写 EXTEND.md**。第一次使用任何需要它的 skill 时，Claude Code 会自动弹出交互式问卷引导你完成配置（选提供商→选模型→选质量→选存储位置），然后自动创建文件。

API Key 不写在 EXTEND.md 里，而是通过**环境变量**提供（如 `GOOGLE_API_KEY`、`OPENAI_API_KEY`、`DASHSCOPE_API_KEY` 等），通常放在 `.env` 文件或 shell 配置中。

---

## Q8: .env 文件查找机制是什么，我放到项目根目录里吗

**问：** .env 文件查找机制是什么，我放到项目根目录里吗

**答：**

### 不是放在项目根目录，而是放在 `.baoyu-skills/` 子目录下

`.env` 文件的查找路径是：

| 优先级 | 路径 | 说明 |
|--------|------|------|
| 1（最高） | `<项目根目录>/.baoyu-skills/.env` | 项目级，仅当前项目生效 |
| 2 | `~/.baoyu-skills/.env` | 用户级，所有项目通用 |

注意：是 `.baoyu-skills/.env`，**不是**项目根目录的 `.env`。

### 完整的配置加载优先级

脚本启动时的加载顺序（高优先级覆盖低优先级）：

```
CLI 参数 > EXTEND.md 配置 > 系统环境变量 > 项目 .env > 用户 .env
```

具体来说：
1. **CLI 参数**（`--provider google`）— 最高优先
2. **EXTEND.md** 中的 YAML 配置
3. **`process.env`**（shell 中已 export 的环境变量）
4. **`<cwd>/.baoyu-skills/.env`**（项目级 .env）
5. **`~/.baoyu-skills/.env`**（用户级 .env）— 最低优先

代码逻辑是：先加载用户级 `.env`，再加载项目级 `.env`，但只在 `process.env` 中该 key **不存在**时才写入，所以已有的环境变量不会被覆盖。

### 怎么写 `.env` 文件

标准的 `KEY=VALUE` 格式，支持注释和引号：

```bash
# ~/.baoyu-skills/.env 或 <项目>/.baoyu-skills/.env

# Google / Gemini
GOOGLE_API_KEY=your-google-api-key

# OpenAI
OPENAI_API_KEY=sk-your-openai-key

# DashScope（阿里云通义万相）
DASHSCOPE_API_KEY=sk-your-dashscope-key

# OpenRouter
OPENROUTER_API_KEY=sk-or-your-key

# Replicate
REPLICATE_API_TOKEN=r8_your-token

# 即梦
JIMENG_ACCESS_KEY_ID=your-key-id
JIMENG_SECRET_ACCESS_KEY=your-secret-key

# Seedream（豆包）
ARK_API_KEY=your-ark-key
```

### 推荐做法

**只用一家提供商**：放在 `~/.baoyu-skills/.env`，所有项目通用

```bash
mkdir -p ~/.baoyu-skills
echo 'GOOGLE_API_KEY=your-key-here' > ~/.baoyu-skills/.env
```

**不同项目用不同提供商**：放在项目的 `.baoyu-skills/.env`

```bash
mkdir -p .baoyu-skills
echo 'DASHSCOPE_API_KEY=your-key-here' > .baoyu-skills/.env
```

**别忘了 `.gitignore`**：如果放在项目目录下，确保不被提交

```bash
echo '.baoyu-skills/.env' >> .gitignore
```

---

## Q9: 如果不配置，会自动使用 cursor 或者 claude 默认的 agent 吗

**问：** 如果不配置，会自动使用cursor或者claude默认的agent吗

**答：**

**不会。不配置就无法使用，会直接报错。**

### 为什么不能自动使用 Cursor/Claude 的能力？

`baoyu-image-gen` 是一个**独立的 TypeScript 脚本**，通过 `bun` 在终端中运行。它直接调用第三方 API（OpenAI、Google 等）来生成图片，和 Cursor/Claude 的运行环境完全隔离。

代码逻辑很清楚（`skills/baoyu-image-gen/scripts/main.ts` 第 635-638 行）：

```javascript
throw new Error(
  "No API key found. Set GOOGLE_API_KEY, GEMINI_API_KEY, OPENAI_API_KEY, OPENROUTER_API_KEY, DASHSCOPE_API_KEY, REPLICATE_API_TOKEN, JIMENG keys, or ARK_API_KEY.\n" +
    "Create ~/.baoyu-skills/.env or <cwd>/.baoyu-skills/.env with your keys."
);
```

找不到任何 API Key → 直接抛出错误，不会回退到任何默认引擎。

### 两套图像生成能力的区别

| | Cursor 内置图像生成 | baoyu-image-gen skill |
|---|---|---|
| **运行方式** | Cursor IDE 内部调用 | 终端执行 TypeScript 脚本 |
| **需要 API Key** | 不需要（Cursor 自带） | 需要自己配置 |
| **能力** | 基础文生图 | 7 家提供商、参考图、批量、自定义尺寸等 |
| **与其他 skill 联动** | 不能 | 被封面/配图/漫画/幻灯片等 skill 调用 |

### 所以实际使用时有两种选择

**选择 A：不配置 API Key，用 Cursor 内置能力**

就像之前帮你生成文章配图那样，直接用 Cursor 的 `GenerateImage` 工具。简单但功能有限，且无法与 skill 工作流联动。

**选择 B：配置 API Key，走完整 skill 流程**

```bash
# 1. 创建 .env
mkdir -p ~/.baoyu-skills
echo 'GOOGLE_API_KEY=你的key' > ~/.baoyu-skills/.env

# 2. 之后所有 skill 的图像生成自动走 Google API
```

配置完成后，`baoyu-cover-image`、`baoyu-article-illustrator`、`baoyu-comic`、`baoyu-slide-deck` 等上层 skill 才能正常调用 `baoyu-image-gen` 生图。

**总结：这两套系统是完全独立的，互不感知，不会自动切换。**

---

## Q10: 项目中的 docs 和 scripts 目录起什么作用

**问：** 项目中的docs 和 scripts目录起什么作用，也是开发时应用的吗

**答：**

对，这两个目录都是**给插件开发者/维护者用的**，普通用户使用 skill 时不需要接触。

### `docs/` — 开发者文档

给维护这个仓库的人看的参考文档：

| 文件 | 内容 |
|------|------|
| `chrome-profile.md` | Chrome 共享 Profile 的各平台路径说明 |
| `comic-style-maintenance.md` | 漫画风格的维护指南（如何新增/修改画风） |
| `creating-skills.md` | **如何开发新 skill** 的完整指南 |
| `image-generation.md` | 图像生成的通用规范和注意事项 |
| `publishing.md` | ClawHub/OpenClaw 发布流程说明 |
| `testing.md` | 测试规范和运行方式 |

这些文档不参与 skill 运行，是纯粹的**开发者参考手册**。

### `scripts/` — 仓库维护脚本

给插件开发者执行仓库级维护操作用的工具脚本：

| 脚本 | 作用 |
|------|------|
| `sync-shared-skill-packages.mjs` | 把 `packages/` 下的共享库同步到各 skill 的 `vendor/` 目录 |
| `publish-skill.mjs` | 发布 skill 到 ClawHub/OpenClaw 平台 |
| `sync-clawhub.mjs` / `.sh` | 同步仓库内容到 ClawHub |
| `install-git-hooks.mjs` | 安装 Git hooks（自动化检查） |
| `lib/shared-skill-packages.mjs` | sync 脚本的核心逻辑库 |
| `lib/release-files.mjs` | 发布流程的文件检测逻辑 |

### 和 `skills/*/scripts/` 的区别

注意区分两种 `scripts/`：

```
项目根目录/
├── scripts/                        ← 仓库维护脚本（开发者用）
│   ├── sync-shared-skill-packages.mjs
│   ├── publish-skill.mjs
│   └── ...
│
└── skills/
    └── baoyu-image-gen/
        └── scripts/                ← skill 运行脚本（用户用）
            ├── main.ts             ← 实际执行图像生成的入口
            ├── package.json
            └── vendor/
```

| | 根目录 `scripts/` | `skills/*/scripts/` |
|---|---|---|
| **使用者** | 插件开发者 | 普通用户（通过 Claude Code 间接调用） |
| **何时运行** | 开发/发布/维护时 | 使用 skill 时 |
| **举例** | 同步 vendor、发布到平台 | 生成图片、发布文章、翻译等 |

**一句话总结：`docs/` 是给开发者看的文档，根目录 `scripts/` 是给开发者跑的维护工具，用户日常使用 skill 不需要关心这两个目录。**

---

## Q11: 帮我开发一个新的技能 default_image_gen

**问：** 帮我开发一个新的技能 default_image_gen, 使用默认ide，比如cursor的能力生成配图， 支持和baoyu-cover-image一样的提示流程

**答：**

新技能 `baoyu-default-image-gen` 已创建完成。它使用 IDE 内置的图像生成能力（如 Cursor 的 GenerateImage），不需要外部 API Key，同时保留 baoyu-cover-image 那样的五维确认流程。

### 文件结构

```
skills/baoyu-default-image-gen/
├── SKILL.md                              # 主文件（~200 行）
└── references/
    ├── auto-selection.md                  # 内容信号 → 维度自动选择规则
    ├── prompt-construction.md             # 维度 → 图像描述文本的构建指南
    └── workflow/
        └── confirm-options.md             # AskUserQuestion 交互确认流程
```

### 核心设计

**与 `baoyu-image-gen` 的关键区别**：

| | `baoyu-image-gen` | `baoyu-default-image-gen` |
|---|---|---|
| 图像生成方式 | 调用 TypeScript 脚本 → 外部 API | 直接调用 IDE 内置工具（如 Cursor GenerateImage） |
| 需要 API Key | 是 | 否 |
| 需要 Bun | 是 | 否 |
| 需要 EXTEND.md | 是 | 否 |
| 参考图 `--ref` | 支持 | 不支持 |
| 批量生成 | 支持 | 不支持 |

**三种模式**：

- **cover** — 与 `baoyu-cover-image` 相同的五维确认流程（type/palette/rendering/text/mood + font），通过 `AskUserQuestion` 交互确认
- **illustration** — 根据文章内容自动生成配图
- **image** — 通用文生图

**与其他 skill 的联动**：当 `baoyu-cover-image`、`baoyu-article-illustrator` 等上层 skill 在 Step 4 检查可用的图像生成后端时，如果 `baoyu-image-gen` 未配置，就可以选择这个技能作为替代。

**已注册**到 `marketplace.json` 的 `ai-generation-skills` 分类下。

---

## Q12: 利用 baoyu-default-image-gen 生成小悟空初遇布尔玛的配图

**问：** 利用刚生成的baoyu-default-image-gen skill生成一张小悟空初遇布尔玛的配图

**答：**

按照 `baoyu-default-image-gen` 的工作流来执行。

**Step 1: 确定模式** — 这是一个具体场景的插画请求，使用 `image` 模式。

**Step 2: 内容分析**
- 主题：龙珠经典场景 — 少年悟空与布尔玛初遇
- 信号：动漫、冒险、青春、经典、怀旧
- 自动维度推荐：palette → `vivid`（动漫色彩鲜明）、rendering → `flat-vector`（接近动画风格）、mood → `bold`（充满活力）

**Step 3: 跳过确认** — image 模式直接生成。

**Step 4: 构建 prompt**

根据 `references/prompt-construction.md` 的模板，保存到 `article/dragonball/prompts/goku-meets-bulma.md`：

```yaml
---
mode: image
palette: vivid
rendering: flat-vector
mood: bold
aspect: 16:9
---
```

**Step 5: 调用 IDE 内置工具生成图像**

使用 Cursor 的 GenerateImage 工具，传入根据 prompt-construction 指南构建的详细图像描述。

**Step 6: 完成报告**

```
Image Generated!

Mode: image
Scene: 小悟空初遇布尔玛（龙珠经典开篇）
Palette: vivid | Rendering: flat-vector (anime style) | Mood: bold
Aspect: 16:9
Location: article/dragonball/

Files:
✓ prompts/goku-meets-bulma.md
✓ goku-meets-bulma.png
```

画面还原了龙珠经典的初遇场景：
- **小悟空** — 标志性的黑色刺猬头、橙色道服、蓝色腰带、背上的如意棒、标志性的猴尾巴，一脸天真的笑容
- **布尔玛** — 紫蓝色短发、粉色装扮、一脸惊讶又好奇的表情
- **背景** — 翠绿的山间小路、阳光穿透树林、远处的青山、以及布尔玛标志性的黄色胶囊汽车
