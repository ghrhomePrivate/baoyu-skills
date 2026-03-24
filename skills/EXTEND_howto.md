## EXTEND.md 写在哪里？

EXTEND.md 有**两个可选位置**，取决于你想让配置作用于单个项目还是所有项目：

| 存储位置 | 路径 | 作用范围 |
|---------|------|---------|
| **项目级**（推荐） | `项目根目录/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 仅当前项目 |
| **用户级** | `~/.baoyu-skills/baoyu-image-gen/EXTEND.md` | 所有项目通用 |

## 查找优先级

skill 启动时按以下顺序查找，找到即停：

```
1. .baoyu-skills/baoyu-image-gen/EXTEND.md        ← 项目目录（最高优先）
2. ${XDG_CONFIG_HOME:-~/.config}/baoyu-skills/...  ← XDG 配置目录
3. ~/.baoyu-skills/baoyu-image-gen/EXTEND.md       ← 用户 Home 目录
```

## 每个 skill 有自己的 EXTEND.md

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

## EXTEND.md 长什么样？

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

## 不需要手动创建

正常使用时**不需要手动写 EXTEND.md**。第一次使用任何需要它的 skill 时，Claude Code 会自动弹出交互式问卷引导你完成配置（选提供商→选模型→选质量→选存储位置），然后自动创建文件。

API Key 不写在 EXTEND.md 里，而是通过**环境变量**提供（如 `GOOGLE_API_KEY`、`OPENAI_API_KEY`、`DASHSCOPE_API_KEY` 等），通常放在 `.env` 文件或 shell 配置中。


## `.env` 文件查找机制

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