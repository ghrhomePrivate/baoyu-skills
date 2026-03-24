以下是项目中 18 个 skill 的完整能力整理，按三大分类组织：

---

## 一、内容生成与发布（Content Skills）— 9 个

### 1. `baoyu-xhs-images` — 小红书图片生成
- **功能**：为小红书生成系列信息图卡片（1–10 张）
- **核心能力**：
  - 11 种风格（cute/fresh/warm/bold/minimal/retro/pop/notion/chalkboard/study-notes/screen-print）
  - 8 种布局（sparse/balanced/dense/list/comparison/flow/mindmap/quadrant）
  - 三种大纲策略：故事驱动 / 信息密集 / 视觉优先
  - 预设组合（knowledge-card、poster、ohmsha 等场景）
- **输出**：`xhs-images/{topic}/` 下的分析文件 + 多张 PNG
- **亮点**：封面先出，后续图通过 `--ref` 保持视觉一致性

### 2. `baoyu-post-to-x` — 发布到 X（Twitter）
- **功能**：通过 Chrome CDP 向 X 发帖
- **支持类型**：
  - 图文帖（最多 4 张图）
  - 视频帖
  - 引用帖
  - 长文 Article（Markdown → 富文本）
- **特点**：真实浏览器操作，绕过反自动化；用户手动点发布确认

### 3. `baoyu-post-to-wechat` — 发布到微信公众号
- **功能**：微信公众号文章 & 图文消息发布
- **两种通道**：API（AppID/Secret）或 Chrome 浏览器
- **核心能力**：
  - Markdown/HTML/纯文本 → 公众号文章
  - 图文贴图发布（多图）
  - 外链自动转文末引用
  - 多账号支持（`--account <alias>`）
- **配置**：主题/颜色/作者/评论开关等通过 EXTEND.md

### 4. `baoyu-post-to-weibo` — 发布到微博
- **功能**：微博发帖与头条文章
- **支持**：图片（最多 18 个媒体）、视频、Markdown 长文章
- **依赖**：Chrome CDP + 登录

### 5. `baoyu-article-illustrator` — 文章配图
- **功能**：分析文章结构，智能标注配图位置，批量生成插图
- **维度**：Type（infographic/scene/flowchart/comparison/framework/timeline）× Style（20+ 风格预设）
- **工作流**：分析 → 确认 → 生成大纲 → 写 prompt → 批量生图
- **亮点**：支持 batch 模式并行生成多张图

### 6. `baoyu-cover-image` — 文章封面生成
- **功能**：五维定制封面图
- **五个维度**：
  - Type（封面类型）
  - Palette（10 种色板）
  - Rendering（7 种渲染风格：粉笔/数字/扁平矢量/手绘/油画/像素/丝网印刷）
  - Text（标题文字处理）
  - Mood（情绪氛围）
  - 额外：Font（字体）
- **支持比例**：2.35:1、16:9、1:1 等

### 7. `baoyu-slide-deck` — 幻灯片生成
- **功能**：从内容生成幻灯片图片系列，可合并为 PPTX/PDF
- **风格预设**：blueprint、chalkboard、notion、corporate、minimal、vintage 等 15+ 种
- **选项**：`--slides`（8–30 张）、`--audience`、`--lang`
- **工作流**：分析 → 两轮确认 → 生成大纲 → 逐张生图 → 合并输出
- **输出脚本**：`merge-to-pptx.ts`、`merge-to-pdf.ts`

### 8. `baoyu-comic` — 知识漫画生成
- **功能**：将知识内容转化为漫画
- **三个维度**：
  - Art（ligne-claire/manga/realistic/ink-brush/chalk）
  - Tone（action/dramatic/energetic/neutral/romantic/vintage/warm）
  - Layout（cinematic/dense/mixed/splash/standard/webtoon）
- **预设**：ohmsha（技术解说）、wuxia（武侠风）、shoujo（少女漫画风）
- **关键**：必须先生成角色参考图 `characters.png`，后续每页通过 `--ref` 保持角色一致

### 9. `baoyu-infographic` — 信息图生成
- **功能**：单张信息图，忠实还原数据
- **规模**：21 种布局 × 20 种风格
- **布局示例**：bento-grid、hub-spoke、iceberg、periodic-table、funnel、venn-diagram、subway-map 等
- **风格示例**：craft-handmade、cyberpunk-neon、kawaii、pixel-art、origami、technical-schematic 等

---

## 二、AI 生成后端（AI Generation Skills）— 2 个

### 10. `baoyu-danger-gemini-web` — Gemini Web 逆向接口
- **功能**：通过逆向 Gemini Web 进行文本/图像生成
- **能力**：多轮对话、参考图输入、会话管理
- **模型**：gemini-3-pro / flash / flash-thinking / 3.1-pro-preview 等
- **注意**：首次使用需同意风险声明（consent.json）；需浏览器 Cookie 认证

### 11. `baoyu-image-gen` — 官方 API 图像生成
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

---

## 三、工具类（Utility Skills）— 7 个

### 12. `baoyu-danger-x-to-markdown` — X 推文转 Markdown
- **功能**：将 X（Twitter）推文/线程/文章转为 Markdown
- **方式**：逆向 API
- **选项**：媒体下载、JSON 输出
- **认证**：`X_AUTH_TOKEN`/`X_CT0` 或 Chrome 登录

### 13. `baoyu-compress-image` — 图片压缩
- **功能**：图片压缩，默认输出 WebP
- **工具链**：sips → cwebp → ImageMagick → Sharp（自动选择可用工具）
- **选项**：格式（webp/png/jpeg）、质量、递归处理目录

### 14. `baoyu-url-to-markdown` — 网页转 Markdown
- **功能**：Chrome CDP 抓取网页 → 转为干净的 Markdown
- **能力**：
  - Defuddle 提取正文（失败时回退到 legacy 或托管 API）
  - `--wait` 支持需登录的页面
  - 媒体文件本地化下载
  - YouTube 字幕抓取
- **输出**：Markdown 文件 + 原始 HTML 备份

### 15. `baoyu-format-markdown` — Markdown 格式化
- **功能**：排版整理，不改写语义
- **能力**：
  - 中英文排版（CJK 友好）
  - 引号规范化
  - 自动纠错
  - 标题公式推荐（多候选让用户选）
- **三种模式**：优化格式 / 保留结构仅排版 / 仅 typography

### 16. `baoyu-markdown-to-html` — Markdown 转 HTML
- **功能**：Markdown → 带内联 CSS 的 HTML（适配公众号等平台）
- **支持**：
  - 4 种主题（default/grace/simple/modern）
  - 数学公式（KaTeX）
  - PlantUML 图表
  - 脚注、Alert 框、Mermaid 图
  - 外链转文末引用（`--cite`）
- **选项**：`--theme`、`--color`、`--font-family`、`--font-size`、`--title`

### 17. `baoyu-translate` — 翻译
- **功能**：三种翻译模式
  - **Quick**：快速直译
  - **Normal**：标准翻译
  - **Refined**：精翻（含反思与润色流程）
- **能力**：术语表、受众/风格定制、长文自动分块、子代理并行翻译
- **输出**：翻译文件 + normal/refined 模式的中间过程文件

### 18. `baoyu-youtube-transcript` — YouTube 字幕下载
- **功能**：无需 API Key 或浏览器，下载 YouTube 字幕与封面
- **能力**：
  - 多语言字幕
  - 自动翻译
  - 章节信息提取
  - 说话人识别（需 AI 后处理）
  - 输出格式：text / srt
- **输出**：`youtube-transcript/{channel}/{title}/` 下的 meta、字幕、封面等

---

## 整体架构关系

```
用户请求
   │
   ├─ 内容创作类 ──→ xhs-images / slide-deck / comic / infographic / cover-image / article-illustrator
   │                    │
   │                    └─ 调用 → baoyu-image-gen（官方 API）或 danger-gemini-web（逆向）生图
   │
   ├─ 发布类 ─────→ post-to-x / post-to-wechat / post-to-weibo
   │                    │
   │                    └─ 调用 → markdown-to-html 渲染 + baoyu-chrome-cdp 浏览器操作
   │
   └─ 工具类 ─────→ translate / format-markdown / compress-image / url-to-markdown / x-to-markdown / youtube-transcript
```

内容生成 skill 是「上层编排者」，它们组织内容分析、大纲生成、prompt 编写等流程，最终调用 `baoyu-image-gen` 或 `baoyu-danger-gemini-web` 作为底层图像引擎完成实际生图。