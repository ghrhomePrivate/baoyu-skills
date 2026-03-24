---
name: baoyu-comic
description: 知识漫画创作工具，支持多种画风和基调。创建原创教育漫画，包含详细的分镜布局和连续图像生成。当用户要求创建"知识漫画"、"教育漫画"、"传记漫画"、"教程漫画"或"Logicomix 风格漫画"时使用。
version: 1.56.1
metadata:
  openclaw:
    homepage: https://github.com/JimLiu/baoyu-skills#baoyu-comic
    requires:
      anyBins:
        - bun
        - npx
---

# 知识漫画创作工具

创建原创知识漫画，支持灵活的画风 × 基调组合。

## 用法

```bash
/baoyu-comic posts/turing-story/source.md
/baoyu-comic article.md --art manga --tone warm
/baoyu-comic  # 然后粘贴内容
```

## 选项

### 视觉维度

| 选项 | 值 | 说明 |
|--------|--------|-------------|
| `--art` | ligne-claire（默认）、manga、realistic、ink-brush、chalk | 画风 / 渲染技法 |
| `--tone` | neutral（默认）、warm、dramatic、romantic、energetic、vintage、action | 情绪 / 氛围 |
| `--layout` | standard（默认）、cinematic、dense、splash、mixed、webtoon | 分格排列 |
| `--aspect` | 3:4（默认，竖版）、4:3（横版）、16:9（宽屏） | 页面宽高比 |
| `--lang` | auto（默认）、zh、en、ja 等 | 输出语言 |

### 部分工作流选项

| 选项 | 说明 |
|--------|-------------|
| `--storyboard-only` | 仅生成分镜，跳过提示词和图像 |
| `--prompts-only` | 生成分镜 + 提示词，跳过图像 |
| `--images-only` | 从已有提示词目录生成图像 |
| `--regenerate N` | 仅重新生成指定页面（如 `3` 或 `2,5,8`） |

详情：[references/partial-workflows.md](references/partial-workflows.md)

### 画风（Art Styles）

| 画风 | 中文 | 说明 |
|-------|------|-------------|
| `ligne-claire` | 清线 | 均匀线条，平涂色彩，欧洲漫画传统（丁丁历险记、Logicomix） |
| `manga` | 日漫 | 大眼睛，漫画惯例，情感表达丰富 |
| `realistic` | 写实 | 数字绘画，写实比例，精致风格 |
| `ink-brush` | 水墨 | 中国毛笔笔触，水墨渲染效果 |
| `chalk` | 粉笔 | 黑板美学，手绘温暖感 |

### 基调（Tones）

| 基调 | 中文 | 说明 |
|------|------|-------------|
| `neutral` | 中性 | 平衡、理性、教育性 |
| `warm` | 温馨 | 怀旧、私人、温暖 |
| `dramatic` | 戏剧 | 高对比、紧张、有力 |
| `romantic` | 浪漫 | 柔和、唯美、装饰性元素 |
| `energetic` | 活力 | 明亮、动感、激动人心 |
| `vintage` | 复古 | 历史感、陈旧、年代真实感 |
| `action` | 动作 | 速度线、冲击效果、格斗 |

### 预设快捷方式

带有特殊规则的预设（不仅限于画风+基调）：

| 预设 | 等价配置 | 特殊规则 |
|--------|-----------|---------------|
| `--style ohmsha` | `--art manga --tone neutral` | 视觉隐喻，禁止说教式对话，道具揭秘 |
| `--style wuxia` | `--art ink-brush --tone action` | 气功效果，格斗视觉，氛围元素 |
| `--style shoujo` | `--art manga --tone romantic` | 装饰元素，眼部细节，浪漫节拍 |

### 兼容性矩阵

| 画风 | ✓✓ 最佳 | ✓ 可用 | ✗ 避免 |
|-----------|---------|---------|---------|
| ligne-claire | neutral、warm | dramatic、vintage、energetic | romantic、action |
| manga | neutral、romantic、energetic、action | warm、dramatic | vintage |
| realistic | neutral、warm、dramatic、vintage | action | romantic、energetic |
| ink-brush | neutral、dramatic、action、vintage | warm | romantic、energetic |
| chalk | neutral、warm、energetic | vintage | dramatic、action、romantic |

详情：[references/auto-selection.md](references/auto-selection.md)

## 自动选择

内容信号决定默认的画风 + 基调 + 布局（或预设）：

| 内容信号 | 推荐方案 |
|-----------------|-------------|
| 教程、指南、编程、教育 | **ohmsha** 预设 |
| 1950年前、古典、古代 | realistic + vintage |
| 个人故事、导师 | ligne-claire + warm |
| 武术、武侠 | **wuxia** 预设 |
| 恋爱、校园生活 | **shoujo** 预设 |
| 传记、均衡 | ligne-claire + neutral |

**当推荐预设时**：加载 `references/presets/{preset}.md` 并应用所有特殊规则。

详情：[references/auto-selection.md](references/auto-selection.md)

## 脚本目录

**重要**：所有脚本位于此技能的 `scripts/` 子目录中。

**Agent 执行指令**：
1. 确定此 SKILL.md 文件所在目录路径为 `{baseDir}`
2. 脚本路径 = `{baseDir}/scripts/<script-name>.ts`
3. 将本文档中所有 `{baseDir}` 替换为实际路径
4. 解析 `${BUN_X}` 运行时：若 `bun` 已安装 → `bun`；若 `npx` 可用 → `npx -y bun`；否则建议安装 bun

**脚本参考**：
| 脚本 | 用途 |
|--------|---------|
| `scripts/merge-to-pdf.ts` | 合并漫画页面为 PDF |

## 文件结构

输出目录：`comic/{topic-slug}/`
- Slug：2-4 个单词的 kebab-case，源自主题（如 `alan-turing-bio`）
- 冲突时：追加时间戳（如 `turing-story-20260118-143052`）

**内容**：
| 文件 | 说明 |
|------|-------------|
| `source-{slug}.{ext}` | 源文件 |
| `analysis.md` | 内容分析 |
| `storyboard.md` | 含分格分解的分镜 |
| `characters/characters.md` | 角色定义 |
| `characters/characters.png` | 角色参考图 |
| `prompts/NN-{cover\|page}-[slug].md` | 生成提示词 |
| `NN-{cover\|page}-[slug].png` | 生成的图像 |
| `{topic-slug}.pdf` | 最终合并的 PDF |

## 语言处理

**检测优先级**：
1. `--lang` 标志（显式指定）
2. EXTEND.md 中的 `language` 设置
3. 用户对话语言
4. 源内容语言

**规则**：使用用户的输入语言或保存的语言偏好进行所有交互：
- 分镜大纲和场景描述
- 图像生成提示词
- 用户选择选项和确认
- 进度更新、问题、错误、摘要

技术术语保留英文。

## 工作流

### 进度清单

```
漫画进度：
- [ ] 步骤 1：设置与分析
  - [ ] 1.1 偏好设置（EXTEND.md）⛔ 阻塞
    - [ ] 找到 → 加载偏好 → 继续
    - [ ] 未找到 → 运行首次设置 → 必须在其他步骤之前完成
  - [ ] 1.2 分析，1.3 检查已有内容
- [ ] 步骤 2：确认 - 风格与选项 ⚠️ 必需
- [ ] 步骤 3：生成分镜 + 角色
- [ ] 步骤 4：审阅大纲（条件性）
- [ ] 步骤 5：生成提示词
- [ ] 步骤 6：审阅提示词（条件性）
- [ ] 步骤 7：生成图像 ⚠️ 需要角色参考
  - [ ] 7.1 首先生成角色参考图 → characters/characters.png
  - [ ] 7.2 使用 --ref characters/characters.png 生成页面
- [ ] 步骤 8：合并为 PDF
- [ ] 步骤 9：完成报告
```

### 流程

```
输入 → [偏好设置] ─┬─ 找到 → 继续
                   │
                   └─ 未找到 → 首次设置 ⛔ 阻塞
                                │
                                └─ 完成设置 → 保存 EXTEND.md → 继续
                                                                │
        ┌───────────────────────────────────────────────────────┘
        ↓
分析 → [检查已有？] → [确认：风格 + 审阅] → 分镜 → [审阅？] → 提示词 → [审阅？] → 图像 → PDF → 完成
```

### 步骤摘要

| 步骤 | 操作 | 关键输出 |
|------|--------|------------|
| 1.1 | 加载 EXTEND.md 偏好 ⛔ 未找到时阻塞 | 配置已加载 |
| 1.2 | 分析内容 | `analysis.md` |
| 1.3 | 检查已有目录 | 处理冲突 |
| 2 | 确认风格、焦点、受众、审阅选项 | 用户偏好 |
| 3 | 生成分镜 + 角色 | `storyboard.md`、`characters/` |
| 4 | 审阅大纲（如请求） | 用户批准 |
| 5 | 生成提示词 | `prompts/*.md` |
| 6 | 审阅提示词（如请求） | 用户批准 |
| **7.1** | **首先生成角色参考图** | `characters/characters.png` |
| **7.2** | 使用角色参考生成页面 | `*.png` 文件 |
| 8 | 合并为 PDF | `{slug}.pdf` |
| 9 | 完成报告 | 摘要 |

### 步骤 7：图像生成 ⚠️ 关键

**角色参考对视觉一致性至关重要。**

**7.1 首先生成角色参考图**：
- **备份规则**：如果 `characters/characters.png` 已存在，重命名为 `characters/characters-backup-YYYYMMDD-HHMMSS.png`
- 调用已安装的图像生成技能（如 `baoyu-image-gen`）
- 阅读该技能的 `SKILL.md` 并遵循其文档化接口，而非直接调用其脚本
- 使用 `characters/characters.md` 作为提示词文件输入
- 输出保存到 `characters/characters.png`
- 使用宽高比 `4:3`

**压缩角色参考图**（推荐）：
压缩以减少作为参考图像时的 token 消耗：
- 使用可用的图像压缩技能（如有）
- 或系统工具：`pngquant`、`optipng`、`sips`（macOS）
- **保持 PNG 格式**，优先无损压缩

**7.2 使用角色参考生成每一页**：

| 技能能力 | 策略 |
|------------------|----------|
| 支持 `--ref` | 每一页都传入 `characters/characters.png` |
| 不支持 `--ref` | 在每个提示词文件前添加角色描述 |

**页面生成备份规则**：
- 如果提示词文件已存在：重命名为 `prompts/NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.md`
- 如果图像文件已存在：重命名为 `NN-{cover|page}-[slug]-backup-YYYYMMDD-HHMMSS.png`
- 为每一页调用已安装的图像生成技能
- 使用 `prompts/01-page-xxx.md` 作为提示词文件输入
- 输出保存到 `01-page-xxx.png`
- 使用宽高比 `3:4`
- 如果所选技能支持参考图像，传入 `characters/characters.png` 作为 `--ref`

**完整工作流详情**：[references/workflow.md](references/workflow.md)

### EXTEND.md 路径 ⛔ 阻塞

**关键**：如果未找到 EXTEND.md，必须在其他任何问题或步骤之前完成首次设置。不要进行内容分析，不要询问画风，不要询问基调——只完成偏好设置。

| 路径 | 位置 |
|------|----------|
| `.baoyu-skills/baoyu-comic/EXTEND.md` | 项目目录 |
| `$HOME/.baoyu-skills/baoyu-comic/EXTEND.md` | 用户主目录 |

| 结果 | 操作 |
|--------|--------|
| 找到 | 读取、解析、显示摘要 → 继续 |
| 未找到 | ⛔ **阻塞**：仅运行首次设置（[references/config/first-time-setup.md](references/config/first-time-setup.md)）→ 完成并保存 EXTEND.md → 然后继续 |

**EXTEND.md 支持**：水印 | 首选画风/基调/布局 | 自定义风格定义 | 角色预设 | 语言偏好

Schema：[references/config/preferences-schema.md](references/config/preferences-schema.md)

## 参考资料

**核心模板**：
- [analysis-framework.md](references/analysis-framework.md) - 深度内容分析
- [character-template.md](references/character-template.md) - 角色定义格式
- [storyboard-template.md](references/storyboard-template.md) - 分镜结构
- [ohmsha-guide.md](references/ohmsha-guide.md) - Ohmsha 漫画规范

**风格定义**：
- `references/art-styles/` - 画风（ligne-claire、manga、realistic、ink-brush、chalk）
- `references/tones/` - 基调（neutral、warm、dramatic、romantic、energetic、vintage、action）
- `references/presets/` - 带特殊规则的预设（ohmsha、wuxia、shoujo）
- `references/layouts/` - 布局（standard、cinematic、dense、splash、mixed、webtoon）

**工作流**：
- [workflow.md](references/workflow.md) - 完整工作流详情
- [auto-selection.md](references/auto-selection.md) - 内容信号分析
- [partial-workflows.md](references/partial-workflows.md) - 部分工作流选项

**配置**：
- [config/preferences-schema.md](references/config/preferences-schema.md) - EXTEND.md schema
- [config/first-time-setup.md](references/config/first-time-setup.md) - 首次设置
- [config/watermark-guide.md](references/config/watermark-guide.md) - 水印配置

## 页面修改

| 操作 | 步骤 |
|--------|-------|
| **编辑** | **先更新提示词文件** → `--regenerate N` → 重新生成 PDF |
| **添加** | 在指定位置创建提示词 → 使用角色参考生成 → 重新编号后续页面 → 更新分镜 → 重新生成 PDF |
| **删除** | 删除文件 → 重新编号后续页面 → 更新分镜 → 重新生成 PDF |

**重要**：更新页面时，务必**先更新提示词文件**（`prompts/NN-{cover|page}-[slug].md`），再重新生成。这确保更改有据可查且可复现。

## 注意事项

- 图像生成：每页 10-30 秒
- 生成失败时自动重试一次
- 对敏感公众人物使用风格化替代
- 通过 session ID 保持风格一致性
- **步骤 2 确认为必需** - 不可跳过
- **步骤 4/6 为条件性** - 仅当用户在步骤 2 中请求时
- **步骤 7.1 角色参考图必须在页面之前生成** - 确保一致性
- **步骤 7.2 每一页都必须引用角色** - 使用 `--ref` 或嵌入描述
- 水印/语言在 EXTEND.md 中一次性配置
