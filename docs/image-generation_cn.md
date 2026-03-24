# 图像生成指南

需要图像生成的技能必须委托给可用的图像生成技能。

## 技能选择

**默认**：`skills/baoyu-image-gen/SKILL.md`（除非用户另行指定）。

1. 阅读技能的 SKILL.md 了解参数和功能
2. 如果用户要求使用其他技能，检查 `skills/` 中的替代方案
3. 仅在存在多个可用选项时询问用户

## 生成流程模板

```markdown
### Step N: Generate Images

**Skill Selection**:
1. Check available skills (`baoyu-image-gen` default, or `baoyu-danger-gemini-web`)
2. Read selected skill's SKILL.md for parameters
3. If multiple skills available, ask user to choose

**Generation Flow**:
1. Call skill with prompt, output path, and skill-specific parameters
2. Generate sequentially by default (batch parallel only when user has multiple prompts)
3. Output progress: "Generated X/N"
4. On failure, auto-retry once before reporting error
```

**批量并行**（仅 `baoyu-image-gen`）：通过 EXTEND.md 中的 `batch.max_workers` 配置并发工作线程数和按提供商节流。

## 输出路径约定

**输出目录**：`<skill-suffix>/<topic-slug>/`
- `<skill-suffix>`：例如 `xhs-images`、`cover-image`、`slide-deck`、`comic`
- `<topic-slug>`：2-4 个单词，kebab-case 格式，来源于内容主题
- 冲突：追加时间戳 `<topic-slug>-YYYYMMDD-HHMMSS`

**源文件**：复制到输出目录，命名为 `source-{slug}.{ext}`

## 图像命名约定

**格式**：`NN-{type}-[slug].png`
- `NN`：两位数序号（01、02...）
- `{type}`：cover、content、page、slide、illustration 等
- `[slug]`：2-5 个单词的 kebab-case 描述，在目录内唯一

示例：
```
01-cover-ai-future.png
02-content-key-benefits.png
03-slide-architecture-overview.png
```
