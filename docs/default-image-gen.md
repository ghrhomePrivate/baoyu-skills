## 帮我开发一个新的技能 default_image_gen, 使用默认ide，比如cursor的能力生成配图， 支持和baoyu-cover-image一样的提示流程

新技能 `baoyu-default-image-gen` 已创建完成。以下是整体设计：

## 文件结构

```
skills/baoyu-default-image-gen/
├── SKILL.md                              # 主文件（~200 行）
└── references/
    ├── auto-selection.md                  # 内容信号 → 维度自动选择规则
    ├── prompt-construction.md             # 维度 → 图像描述文本的构建指南
    └── workflow/
        └── confirm-options.md             # AskUserQuestion 交互确认流程
```

## 核心设计

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