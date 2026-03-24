---
name: baoyu-youtube-transcript
description: 按 URL 或视频 ID 下载 YouTube 视频字幕/转写文本与封面图。支持多语言、翻译、章节与说话人识别。缓存原始数据以便快速重新排版。适用于用户要求「获取 YouTube 字幕」「下载字幕」「get captions」「YouTube 字幕」「YouTube 封面」「视频封面」「video thumbnail」「video cover image」，或提供 YouTube 链接并希望提取转写/字幕文本或封面图。
version: 1.1.0
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-youtube-transcript
    requires:
      anyBins:
        - bun
        - npx
---

# YouTube 字幕（Transcript）

从 YouTube 视频下载字幕（转写/captions）。支持人工上传与自动生成的字幕。无需 API key 或浏览器——直接调用 YouTube InnerTube API。

首次运行会拉取视频元数据与封面图，并缓存原始数据以便快速换格式输出。

## 脚本目录

脚本位于 `scripts/` 子目录。`{baseDir}` = 本 SKILL.md 所在目录。解析 `${BUN_X}`：若已安装 `bun` → `bun`；若有 `npx` → `npx -y bun`；否则提示安装 bun。将 `{baseDir}` 与 `${BUN_X}` 替换为实际值。

| 脚本 | 用途 |
|--------|---------|
| `scripts/main.ts` | 字幕下载 CLI |

## 用法

```bash
# 默认：带时间戳的 Markdown（英文）
${BUN_X} {baseDir}/scripts/main.ts <youtube-url-or-id>

# 指定语言（优先级顺序）
${BUN_X} {baseDir}/scripts/main.ts <url> --languages zh,en,ja

# 不带时间戳
${BUN_X} {baseDir}/scripts/main.ts <url> --no-timestamps

# 按章节切分
${BUN_X} {baseDir}/scripts/main.ts <url> --chapters

# 说话人识别（需 AI 后处理）
${BUN_X} {baseDir}/scripts/main.ts <url> --speakers

# SRT 字幕文件
${BUN_X} {baseDir}/scripts/main.ts <url> --format srt

# 翻译转写
${BUN_X} {baseDir}/scripts/main.ts <url> --translate zh-Hans

# 列出可用字幕
${BUN_X} {baseDir}/scripts/main.ts <url> --list

# 强制重新拉取（忽略缓存）
${BUN_X} {baseDir}/scripts/main.ts <url> --refresh
```

## 选项

| 选项 | 说明 | 默认 |
|--------|-------------|---------|
| `<url-or-id>` | YouTube URL 或视频 ID（可多个） | 必填 |
| `--languages <codes>` | 语言代码，逗号分隔，按优先级 | `en` |
| `--format <fmt>` | 输出格式：`text`、`srt` | `text` |
| `--translate <code>` | 翻译到指定语言代码 | |
| `--list` | 仅列出可用字幕，不下载 | |
| `--timestamps` | 每段带 `[HH:MM:SS → HH:MM:SS]` 时间戳 | 开启 |
| `--no-timestamps` | 关闭时间戳 | |
| `--chapters` | 从视频简介解析章节并分段 | |
| `--speakers` | 输出含元数据的原始转写，供说话人识别 | |
| `--exclude-generated` | 跳过自动生成字幕 | |
| `--exclude-manually-created` | 跳过人工创建字幕 | |
| `--refresh` | 强制重新拉取，忽略缓存 | |
| `-o, --output <path>` | 保存到指定文件路径 | 自动生成 |
| `--output-dir <dir>` | 输出根目录 | `youtube-transcript` |

## 输入格式

以下任意形式均可作为视频输入：
- 完整 URL：`https://www.youtube.com/watch?v=dQw4w9WgXcQ`
- 短链：`https://youtu.be/dQw4w9WgXcQ`
- 嵌入 URL：`https://www.youtube.com/embed/dQw4w9WgXcQ`
- Shorts URL：`https://www.youtube.com/shorts/dQw4w9WgXcQ`
- 视频 ID：`dQw4w9WgXcQ`

## 输出格式

| 格式 | 扩展名 | 说明 |
|--------|-----------|-------------|
| `text` | `.md` | Markdown，含 frontmatter（含 `description`）、标题、摘要，可选目录/封面/时间戳/章节/说话人 |
| `srt` | `.srt` | SubRip，供播放器使用 |

## 输出目录

```
youtube-transcript/
├── .index.json                          # 视频 ID → 目录路径映射（缓存查找）
└── {channel-slug}/{title-full-slug}/
    ├── meta.json                        # 视频元数据（标题、频道、简介、时长、章节等）
    ├── transcript-raw.json              # YouTube API 原始字幕片段（缓存）
    ├── transcript-sentences.json        # 按句切分（按标点跨片段合并）
    ├── imgs/
    │   └── cover.jpg                    # 视频缩略图/封面
    ├── transcript.md                    # Markdown 转写（由句子生成）
    └── transcript.srt                   # SRT（由 raw 片段生成，若使用 --format srt）
```

- `{channel-slug}`：频道名 kebab-case
- `{title-full-slug}`：完整标题 kebab-case

`--list` 模式仅输出到 stdout，不写文件。

## 缓存

首次拉取会保存：
- `meta.json` — 元数据、章节、封面路径、语言信息
- `transcript-raw.json` — API 原始片段（`{ text, start, duration }[]`）
- `transcript-sentences.json` — 按句切分（`{ text, start: "HH:mm:ss", end: "HH:mm:ss" }[]`），按句末标点切分、时间按字符比例分配、CJK 感知合并
- `imgs/cover.jpg` — 缩略图

同一视频再次运行会使用缓存（无网络请求）。用 `--refresh` 强制重拉。若请求不同语言，会自动刷新缓存。

SRT（`--format srt`）由 `transcript-raw.json` 生成。文本/Markdown 使用 `transcript-sentences.json` 以获得更自然的断句。

## 工作流

用户提供 YouTube 链接并要字幕时：

1. 若用户未指定语言，可先 `--list` 查看可用选项
2. **URL 务必用单引号包裹** — zsh 会把 `?` 当 glob，未加引号的 YouTube 链接会报 "no matches found"：使用 `'https://www.youtube.com/watch?v=ID'`
3. 默认建议 `--chapters --speakers` 以获得最丰富输出（章节 + 说话人）
4. 脚本会自动保存缓存与输出文件，并打印路径
5. `--speakers` 模式：脚本保存原始文件后，按下方「说话人」流程用 AI 后处理打上说话人标签

用户只要封面或元数据时，任意选项运行一次也会缓存 `meta.json` 与 `imgs/cover.jpg`。

同一视频换格式（如先 text 再 SRT）会复用缓存，无需重新拉取。

## 章节与说话人流程

### 章节（`--chapters`）

脚本从视频简介解析章节时间戳（如 `0:00 Introduction`），按章节边界切分转写，将片段组成可读段落，并保存为带目录的 `.md`。一般无需再处理。

若简介无章节时间戳，则输出为分组段落，无章节标题。

### 说话人识别（`--speakers`）

说话人识别需要 AI 处理。脚本会输出原始 `.md`，其中包含：
- 含视频元数据的 YAML frontmatter（标题、频道、日期、封面、简介、语言）
- 视频简介（用于提取说话人姓名）
- 来自简介的章节列表（若有）
- SRT 格式的原始转写（已带起止时间，省 token）

脚本保存原始文件后，可 spawn 子 agent（为省成本可用较轻模型如 Sonnet）按以下步骤处理说话人：

1. 读取已保存的 `.md`
2. 读取 `{baseDir}/prompts/speaker-transcript.md` 中的 prompt 模板
3. 按模板处理原始转写：
   - 用元数据识别说话人（标题→嘉宾、频道→主持、简介→姓名）
   - 从对话流、问答模式与语境判断话轮
   - 按章节切分（有简介章节则用，否则按话题转折新建）
   - 格式为 `**Speaker Name:**` 标签、段落分组（2–4 句）、`[HH:MM:SS → HH:MM:SS]` 时间戳
4. 用处理后的转写覆盖 `.md`（保留 YAML frontmatter）

使用 `--speakers` 时隐含 `--chapters` — 处理后输出始终含章节结构。

## 异常情况

| 错误 | 含义 |
|-------|---------|
| Transcripts disabled | 视频完全无字幕 |
| No transcript found | 请求的语言不可用 |
| Video unavailable | 视频已删、私有或地区限制 |
| IP blocked | 请求过多，请稍后重试 |
| Age restricted | 需登录验证年龄 |
