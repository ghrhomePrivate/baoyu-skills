---
name: baoyu-image-gen
description: 使用 OpenAI、Google、OpenRouter、DashScope、Jimeng、Seedream 与 Replicate API 进行 AI 图像生成。支持文生图、参考图、宽高比与从已保存 prompt 文件批量生成。默认顺序执行；若用户已有多个 prompt 或需要稳定多图吞吐，使用 batch 并行生成。在用户要求生成、创作或绘制图像时使用。
version: 1.56.3
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-image-gen
    requires:
      anyBins:
        - bun
        - npx
---

# 图像生成（AI SDK）

基于官方 API 的图像生成。支持 OpenAI、Google、OpenRouter、DashScope（阿里通义万象）、Jimeng（即梦）、Seedream（豆包）与 Replicate provider。

## 脚本目录

**Agent 执行**：
1. `{baseDir}` = 本 SKILL.md 所在目录
2. 脚本路径 = `{baseDir}/scripts/main.ts`
3. 解析 `${BUN_X}`：若已安装 `bun` → `bun`；若可用 `npx` → `npx -y bun`；否则建议安装 bun

## 步骤 0：加载偏好 ⛔ 阻塞

**关键**：本步骤必须在任何图像生成之前完成。不得跳过或延后。

按优先级检查 EXTEND.md（项目 → 用户）：

```bash
# macOS, Linux, WSL, Git Bash
test -f .baoyu-skills/baoyu-image-gen/EXTEND.md && echo "project"
test -f "${XDG_CONFIG_HOME:-$HOME/.config}/baoyu-skills/baoyu-image-gen/EXTEND.md" && echo "xdg"
test -f "$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md" && echo "user"
```

```powershell
# PowerShell (Windows)
if (Test-Path .baoyu-skills/baoyu-image-gen/EXTEND.md) { "project" }
$xdg = if ($env:XDG_CONFIG_HOME) { $env:XDG_CONFIG_HOME } else { "$HOME/.config" }
if (Test-Path "$xdg/baoyu-skills/baoyu-image-gen/EXTEND.md") { "xdg" }
if (Test-Path "$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md") { "user" }
```

| 结果 | 操作 |
|--------|--------|
| 找到 | 加载、解析并应用设置。若 `default_model.[provider]` 为 null → 仅询问 model（流程 2） |
| 未找到 | ⛔ 运行首次设置（[references/config/first-time-setup_cn.md](references/config/first-time-setup_cn.md)）→ 保存 EXTEND.md → 再继续 |

**关键**：若未找到，须用 AskUserQuestion 完成完整设置（provider + model + quality + 保存位置）后再生成任何图像。在创建 EXTEND.md 之前，生成 **被阻塞**。

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-image-gen/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 用户主目录 |

**EXTEND.md 支持**：默认 provider | 默认 quality | 默认宽高比 | 默认图像尺寸 | 默认 models | Batch worker 上限 | 各 provider 的 batch 限制

Schema：`references/config/preferences-schema_cn.md`

## 用法

```bash
# 基础
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image cat.png

# 指定宽高比
${BUN_X} {baseDir}/scripts/main.ts --prompt "A landscape" --image out.png --ar 16:9

# 高质量
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --quality 2k

# 从 prompt 文件
${BUN_X} {baseDir}/scripts/main.ts --promptfiles system.md content.md --image out.png

# 参考图（Google、OpenAI、OpenRouter、Replicate，或 Seedream 4.0/4.5/5.0）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --ref source.png

# 参考图（显式指定 provider/model）
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --provider google --model gemini-3-pro-image-preview --ref source.png

# OpenRouter（推荐默认 model）
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider openrouter

# OpenRouter + 参考图
${BUN_X} {baseDir}/scripts/main.ts --prompt "Make blue" --image out.png --provider openrouter --model google/gemini-3.1-flash-image-preview --ref source.png

# 指定 provider
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider openai

# DashScope（阿里通义万象）
${BUN_X} {baseDir}/scripts/main.ts --prompt "一只可爱的猫" --image out.png --provider dashscope

# DashScope Qwen-Image 2.0 Pro（自定义尺寸与文字渲染推荐）
${BUN_X} {baseDir}/scripts/main.ts --prompt "为咖啡品牌设计一张 21:9 横幅海报，包含清晰中文标题" --image out.png --provider dashscope --model qwen-image-2.0-pro --size 2048x872

# DashScope 旧版 Qwen 固定尺寸 model
${BUN_X} {baseDir}/scripts/main.ts --prompt "一张电影感海报" --image out.png --provider dashscope --model qwen-image-max --size 1664x928

# Replicate（google/nano-banana-pro）
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate

# Replicate 指定 model
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate --model google/nano-banana

# Batch：已保存的 prompt 文件
${BUN_X} {baseDir}/scripts/main.ts --batchfile batch.json

# Batch：显式 worker 数量
${BUN_X} {baseDir}/scripts/main.ts --batchfile batch.json --jobs 4 --json
```

### Batch 文件格式

```json
{
  "jobs": 4,
  "tasks": [
    {
      "id": "hero",
      "promptFiles": ["prompts/hero.md"],
      "image": "out/hero.png",
      "provider": "replicate",
      "model": "google/nano-banana-pro",
      "ar": "16:9",
      "quality": "2k"
    },
    {
      "id": "diagram",
      "promptFiles": ["prompts/diagram.md"],
      "image": "out/diagram.png",
      "ref": ["references/original.png"]
    }
  ]
}
```

`promptFiles`、`image`、`ref` 中的路径相对于 batch 文件所在目录解析。`jobs` 可选（可被 CLI `--jobs` 覆盖）。也接受顶层数组格式（无 `jobs` 包裹）。

## 选项

| 选项 | 说明 |
|--------|-------------|
| `--prompt <text>`, `-p` | Prompt 文本 |
| `--promptfiles <files...>` | 从文件读取 prompt（拼接） |
| `--image <path>` | 输出图像路径（单图模式必填） |
| `--batchfile <path>` | 多图生成的 JSON batch 文件 |
| `--jobs <count>` | Batch worker 数量（默认：自动，上限来自配置，内置默认 10） |
| `--provider google\|openai\|openrouter\|dashscope\|jimeng\|seedream\|replicate` | 强制 provider（默认：自动检测） |
| `--model <id>`, `-m` | Model ID（Google：`gemini-3-pro-image-preview`；OpenAI：`gpt-image-1.5`；OpenRouter：`google/gemini-3.1-flash-image-preview`；DashScope：`qwen-image-2.0-pro`） |
| `--ar <ratio>` | 宽高比（如 `16:9`、`1:1`、`4:3`） |
| `--size <WxH>` | 尺寸（如 `1024x1024`） |
| `--quality normal\|2k` | Quality 预设（默认：`2k`） |
| `--imageSize 1K\|2K\|4K` | Google/OpenRouter 图像尺寸（默认：由 quality 推导） |
| `--ref <files...>` | 参考图。支持 Google multimodal、OpenAI GPT Image edits、OpenRouter multimodal models、Replicate、Seedream 5.0/4.5/4.0。不支持 Jimeng、Seedream 3.0，或已移除的 SeedEdit 3.0 |
| `--n <count>` | 生成张数 |
| `--json` | JSON 输出 |

## 环境变量

| 变量 | 说明 |
|----------|-------------|
| `OPENAI_API_KEY` | OpenAI API key |
| `OPENROUTER_API_KEY` | OpenRouter API key |
| `GOOGLE_API_KEY` | Google API key |
| `DASHSCOPE_API_KEY` | DashScope API key（阿里云） |
| `REPLICATE_API_TOKEN` | Replicate API token |
| `JIMENG_ACCESS_KEY_ID` | Jimeng（即梦）Volcengine access key |
| `JIMENG_SECRET_ACCESS_KEY` | Jimeng（即梦）Volcengine secret key |
| `ARK_API_KEY` | Seedream（豆包）Volcengine ARK API key |
| `OPENAI_IMAGE_MODEL` | OpenAI model 覆盖 |
| `OPENROUTER_IMAGE_MODEL` | OpenRouter model 覆盖（默认：`google/gemini-3.1-flash-image-preview`） |
| `GOOGLE_IMAGE_MODEL` | Google model 覆盖 |
| `DASHSCOPE_IMAGE_MODEL` | DashScope model 覆盖（默认：`qwen-image-2.0-pro`） |
| `REPLICATE_IMAGE_MODEL` | Replicate model 覆盖（默认：google/nano-banana-pro） |
| `JIMENG_IMAGE_MODEL` | Jimeng model 覆盖（默认：jimeng_t2i_v40） |
| `SEEDREAM_IMAGE_MODEL` | Seedream model 覆盖（默认：doubao-seedream-5-0-260128） |
| `OPENAI_BASE_URL` | 自定义 OpenAI endpoint |
| `OPENROUTER_BASE_URL` | 自定义 OpenRouter endpoint（默认：`https://openrouter.ai/api/v1`） |
| `OPENROUTER_HTTP_REFERER` | OpenRouter 归因可选 app/site URL |
| `OPENROUTER_TITLE` | OpenRouter 归因可选应用名 |
| `GOOGLE_BASE_URL` | 自定义 Google endpoint |
| `DASHSCOPE_BASE_URL` | 自定义 DashScope endpoint |
| `REPLICATE_BASE_URL` | 自定义 Replicate endpoint |
| `JIMENG_BASE_URL` | 自定义 Jimeng endpoint（默认：`https://visual.volcengineapi.com`） |
| `JIMENG_REGION` | Jimeng 区域（默认：`cn-north-1`） |
| `SEEDREAM_BASE_URL` | 自定义 Seedream endpoint（默认：`https://ark.cn-beijing.volces.com/api/v3`） |
| `BAOYU_IMAGE_GEN_MAX_WORKERS` | 覆盖 batch worker 上限 |
| `BAOYU_IMAGE_GEN_<PROVIDER>_CONCURRENCY` | 覆盖 provider 并发，如 `BAOYU_IMAGE_GEN_REPLICATE_CONCURRENCY` |
| `BAOYU_IMAGE_GEN_<PROVIDER>_START_INTERVAL_MS` | 覆盖 provider 启动间隔，如 `BAOYU_IMAGE_GEN_REPLICATE_START_INTERVAL_MS` |

**加载优先级**：CLI 参数 > EXTEND.md > 环境变量 > `<cwd>/.baoyu-skills/.env` > `~/.baoyu-skills/.env`

## Model 解析

Model 优先级（高 → 低），适用于所有 provider：

1. CLI：`--model <id>`
2. EXTEND.md：`default_model.[provider]`
3. 环境变量：`<PROVIDER>_IMAGE_MODEL`（如 `GOOGLE_IMAGE_MODEL`）
4. 内置默认

**EXTEND.md 覆盖环境变量**。若同时存在 EXTEND.md `default_model.google: "gemini-3-pro-image-preview"` 与 `GOOGLE_IMAGE_MODEL=gemini-3.1-flash-image-preview`，以 EXTEND.md 为准。

**Agent 必须在每次生成前展示 model 信息**：
- 显示：`Using [provider] / [model]`
- 显示切换提示：`Switch model: --model <id> | EXTEND.md default_model.[provider] | env <PROVIDER>_IMAGE_MODEL`

### DashScope Models

用户需要官方 Qwen-Image 行为时，使用 `--model qwen-image-2.0-pro` 或设置 `default_model.dashscope` / `DASHSCOPE_IMAGE_MODEL`。

官方 DashScope model 族：

- `qwen-image-2.0-pro`、`qwen-image-2.0-pro-2026-03-03`、`qwen-image-2.0`、`qwen-image-2.0-2026-03-03`
  - 自由格式 `size`，`宽*高`
  - 总像素须在 `512*512` 与 `2048*2048` 之间
  - 默认尺寸约 `1024*1024`
  - 适合 `21:9` 等自定义比例与中英混排-heavy 版式
- `qwen-image-max`、`qwen-image-max-2025-12-30`、`qwen-image-plus`、`qwen-image-plus-2026-01-09`、`qwen-image`
  - 仅固定尺寸：`1664*928`、`1472*1104`、`1328*1328`、`1104*1472`、`928*1664`
  - 默认 `1664*928`
  - 当前 `qwen-image` 与 `qwen-image-plus` 能力相同
- 旧版如 `z-image-turbo`、`z-image-ultra`、`wanx-v1`
  - 仅在用户明确要求 legacy 或兼容时使用

将 CLI 参数映射到 DashScope 行为时：

- `--size` 优先于 `--ar`
- 对 `qwen-image-2.0*`，优先显式 `--size`；否则从 `--ar` 推断并使用下方官方推荐分辨率
- 对 `qwen-image-max/plus/image`，仅使用五种官方固定尺寸；若请求比例无法覆盖，改用 `qwen-image-2.0-pro`
- `--quality` 是 baoyu-image-gen 的兼容预设，非 DashScope API 原生字段。将 `normal` / `2k` 映射到下方 `qwen-image-2.0*` 表为实现推断，非官方 API 保证

常见宽高比下 `qwen-image-2.0*` 推荐尺寸：

| Ratio | `normal` | `2k` |
|-------|----------|------|
| `1:1` | `1024*1024` | `1536*1536` |
| `2:3` | `768*1152` | `1024*1536` |
| `3:2` | `1152*768` | `1536*1024` |
| `3:4` | `960*1280` | `1080*1440` |
| `4:3` | `1280*960` | `1440*1080` |
| `9:16` | `720*1280` | `1080*1920` |
| `16:9` | `1280*720` | `1920*1080` |
| `21:9` | `1344*576` | `2048*872` |

DashScope 官方 API 还提供 `negative_prompt`、`prompt_extend`、`watermark`，但 `baoyu-image-gen` 目前未提供对应 CLI 标志。

官方文档：

- [Qwen-Image API](https://help.aliyun.com/zh/model-studio/qwen-image-api)
- [文生图指南](https://help.aliyun.com/zh/model-studio/text-to-image)
- [Qwen-Image Edit API](https://help.aliyun.com/zh/model-studio/qwen-image-edit-api)

### OpenRouter Models

使用完整 OpenRouter model ID，例如：

- `google/gemini-3.1-flash-image-preview`（推荐，支持图像输出与参考图工作流）
- `google/gemini-2.5-flash-image-preview`
- `black-forest-labs/flux.2-pro`
- 其他 OpenRouter 支持出图的 model ID

说明：

- OpenRouter 图像生成使用 `/chat/completions`，非 OpenAI `/images` endpoint
- 若使用 `--ref`，须选择支持图像输入与图像输出的 multimodal model
- `--imageSize` 映射到 OpenRouter `imageGenerationOptions.size`；`--size <WxH>` 会转换为最接近的 OpenRouter 尺寸并在可能时推断宽高比

### Replicate Models

支持的 model 格式：

- `owner/name`（官方 model 推荐），如 `google/nano-banana-pro`
- `owner/name:version`（按版本的社区 model），如 `stability-ai/sdxl:<version>`

示例：

```bash
# 使用 Replicate 默认 model
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate

# 显式覆盖 model
${BUN_X} {baseDir}/scripts/main.ts --prompt "A cat" --image out.png --provider replicate --model google/nano-banana
```

## Provider 选择

1. 提供 `--ref` 且未指定 `--provider` → 优先自动选 Google，其次 OpenAI、OpenRouter、Replicate（Jimeng 与 Seedream 不支持参考图）
2. 指定 `--provider` → 使用该 provider（若带 `--ref`，须为 `google`、`openai`、`openrouter` 或 `replicate`）
3. 仅一个 API key 可用 → 使用该 provider
4. 多个可用 → 默认 Google

## Quality 预设

| Preset | Google imageSize | OpenAI Size | OpenRouter size | Replicate resolution | 适用场景 |
|--------|------------------|-------------|-----------------|----------------------|----------|
| `normal` | 1K | 1024px | 1K | 1K | 快速预览 |
| `2k`（默认） | 2K | 2048px | 2K | 2K | 封面、插画、信息图 |

**Google/OpenRouter imageSize**：可用 `--imageSize 1K|2K|4K` 覆盖

## 宽高比

支持：`1:1`、`16:9`、`9:16`、`4:3`、`3:4`、`2.35:1`

- Google multimodal：使用 `imageConfig.aspectRatio`
- OpenAI：映射到最接近的支持尺寸
- OpenRouter：发送 `imageGenerationOptions.aspect_ratio`；若仅给 `--size <WxH>`，会自动推断宽高比
- Replicate：向 model 传 `aspect_ratio`；提供 `--ref` 且无 `--ar` 时默认 `match_input_image`

## 生成模式

**默认**：顺序生成。

**Batch 并行**：当 `--batchfile` 中有 2 个或以上待处理任务时，脚本自动启用并行。

| 模式 | 适用场景 |
|------|-------------|
| Sequential（默认） | 常规使用、单图、小批量 |
| Parallel batch | Batch 模式且 2+ 任务 |

执行选择：

| 情况 | 推荐做法 | 原因 |
|-----------|--------------------|-----|
| 单图或 1–2 张简单图 | Sequential | 协调成本低、易调试 |
| 多图且已有保存的 prompt 文件 | Batch（`--batchfile`） | 复用已定稿 prompt、统一限流/重试、吞吐可预期 |
| 每张仍需单独推理、写 prompt 或风格探索 | Subagents | 仍处探索阶段，生成前可能需要独立分析 |
| 输出来自 `baoyu-article-illustrator` 的 `outline.md` + `prompts/` | Batch（`build-batch.ts` → `--batchfile`） | 该工作流已产出 prompt 文件，直接 batch 为预期路径 |

经验法则：

- prompt 文件已保存且任务是「全部生成」时，优先 batch 而非 subagents
- 仅当生成与逐图思考、改写或发散创意强绑定时再用 subagents

并行行为：

- 默认 worker 数为自动，受配置上限约束，内置默认 10
- 仅 batch 模式应用各 provider 限流，内置默认值在避免明显 RPM 突刺的同时偏吞吐
- 可用 `--jobs <count>` 覆盖 worker 数
- 每张图自动最多重试 3 次
- 最终输出含成功数、失败数及每张失败原因

## 错误处理

- 缺少 API key → 报错并附设置说明
- 生成失败 → 每张图自动最多重试 3 次
- 无效宽高比 → 警告后使用默认
- 不支持的 provider/model 使用参考图 → 报错并附修复提示

## 扩展支持

通过 EXTEND.md 自定义配置。路径与支持项见上文 **步骤 0** 与 **偏好** 相关说明。
